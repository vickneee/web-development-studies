# NodeMovies PostgreSQL Database with JWT & Bcrypt

This is a simple example of a Node.js application that uses a Postgres database. The application is a REST API that allows users to perform CRUD operations on a database of movies. The application uses the `pg` module to connect to the Postgres database and execute SQL queries.

## Setup

1. Create a new directory for the project and navigate to it:

   ```bash
   mkdir nodemovies-postgres
   cd nodemovies-postgres
   ```
2. Initialize the project using npm:

   ```bash
   npm init -y
   ```

3. Install the `pg` module using npm:

   ```bash
   npm install pg
   ```

4. Create a Postgres database and a table to store movies and users:

```sql
CREATE TABLE movies (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    director VARCHAR(255) NOT NULL,
    year INTEGER NOT NULL
);

INSERT INTO movies (title, director, year) VALUES ('The Shawshank Redemption', 'Frank Darabont', 1994);
INSERT INTO movies (title, director, year) VALUES ('The Godfather', 'Francis Ford Coppola', 1972);
INSERT INTO movies (title, director, year) VALUES ('The Dark Knight', 'Christopher Nolan', 2008);

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL
);

INSERT INTO users (email, password) VALUES ('example@email', 'password');
```

5. Create a file called `.env` to store environment variables:

```dotenv
# .env
# You can generate a random secret key using a tool like https://randomkeygen.com/
SECRET_KEY="5b1a3923cc1e1e19523fd5c3f20b409509d3ff9d42710a4da095a2ce285b009f0c3730cd9b8e1af3eb84d";
```

6. Create a file called `index.js` inside the project directory and add the following code to create a simple REST API using Express and the `pg` module:

```javascript
// index.js
import express from 'express';
import dotenv from 'dotenv';
import MovieRouter from './routes/MovieRoutes.js';
import UserRouter from './routes/UserRoutes.js';
import AuthRouter from "./routes/AuthRoutes.js";
import db from './db/db.js';

import bodyParser from "body-parser"; // This is a middleware that will help us parse the request body

const app = express(); // Create an instance of an Express app

dotenv.config(); // Load environment variables

// Middleware
app.use(express.json()); // This will parse incoming request bodies in JSON format
app.use(bodyParser.json()); // This will parse incoming request bodies in JSON format

// Route Middlewares
// This will prefix all routes in MovieRouter with '/movies' (e.g. '/movies', '/movies/:id')
app.use('/movies', MovieRouter);
// This will prefix all routes in UserRouter with '/users' (e.g. '/users/:email', '/users/id/:id')
app.use('/users', UserRouter);
// This will prefix all routes in AuthRouter with '/auth' (e.g. '/auth/register', '/auth/login')
app.use('/auth', AuthRouter);

// PostgresSQL connection
db.connect() // Connect to the database
        .then(() => console.log('Connected to Postgres')) // If the connection is successful, log a message
        .catch(err => console.error('Failed to connect to Postgres', err)); // If the connection fails, log an error message

// Start the server
const port = process.env.PORT || 3000; // Use the PORT environment variable, or 3000 if it's not set
app.listen(port, () => console.log(`Server running on port ${port}`)); // Start the server and log a message
```

7. Create a folder called `db/` and a file called `db.js` to connect to the database:

```javascript
// db/db.js
import pkg from 'pg';
const { Pool } = pkg;

const db = new Pool({
    user: "victoriavavulina",
    host: "localhost",
    port: 5432,
    database: "movies",
    password: "postgres"
})

export default db;
```

8. Create a folder called `services/` and a file called `Authenticate.js` to define the authentication service:

```javascript
// services/Authenticate.js
import jwt from 'jsonwebtoken';
import bcrypt from 'bcrypt';
import UserController from "../controllers/UserController.js";

// User creation
const createUser = async (req, res) => {
   // Extract user details from the request body
   const email = req.body.email;
   const password = req.body.password;

   // Check if email and password are provided
   if (!email || !password) {
      console.log('Email or password not provided');
      return res.sendStatus(400).end();
   }

   try { // Check if the user already exists
      const existingUser = await UserController.getUserByEmail(email);
      if (existingUser) {
         console.log('User already exists');
         return res.status(409).send({error: 'User already exists'});
      }

      // Hash the password
      const salt = bcrypt.genSaltSync(10);
      // Replace plain password with hashed password
      req.body.password = bcrypt.hashSync(password, salt);
      // Store the user in the database
      await UserController.storeUserInDatabase(req, res);
   } catch (err) {
      console.error('Error executing query', err.stack);
      res.status(500).send({error: 'Database error', details: err.stack});
   }
}

// User login
const login = async (req, res) => {
   // Extract email and password from the request body
   const email = req.body.email;
   const password = req.body.password;

   // Check if email and password are provided
   if (!email || !password) {
      console.log('Email or password not provided');
      return res.sendStatus(400).end();
   }

   try { // Check if the user exists
      const user = await UserController.getUserByEmail(email);
      if (user) {
         const hash = user.password;
         // Create JSON Web Token
         const token = jwt.sign({userId: email}, process.env.SECRET_KEY);

         // If password match, send the token
         if (bcrypt.compareSync(password, hash))
            res.send({token, message: 'You are logged in'});
         else {
            console.log('Password does not match');
            res.sendStatus(400).end();
         }
      } else { // If a user does not exist, send a 400 status code
         console.log('User not found');
         res.sendStatus(400).end();
      }
   } catch (err) {
      console.error('Error executing query', err.stack);
      res.status(500).send({error: 'Database error', details: err.stack});
   }
}

// User authentication
const authenticate = (req, res, next) => {
   const authHeader = req.header('Authorization');

   // Check if the Authorization header is provided
   if (!authHeader) {
      console.log('Authorization header not provided');
      return res.sendStatus(400).end();
   }

   const token = authHeader.split(' ')[1]; // Extract the token from the Authorization header

   console.log('Token:', token); // Log the token

   jwt.verify(token, process.env.SECRET_KEY, (err, decoded) => { // Use the correct secret key
      console.log('JWT verify error:', err); // Log the JWT verify error
      console.log('JWT verify decoded:', decoded); // Log the JWT verify decoded

      if (err) {  // If the token is not successfully verified
         return res.status(500).send({auth: false, message: 'Failed to authenticate token.'});
      }

      // If the token is successfully verified
      req.userId = decoded.userId; // Use the correct property from the decoded token
      next();
   });
}

export default {
   authenticate,
   login,
   createUser
}
```

9. Create a folder called routes and a file called `AuthRoutes.js` to define the routes for user authentication:

```javascript
// routes/AuthRoutes.js
import express from 'express';
import auth from '../services/Authenticate.js';

const router = express.Router(); // Create a new router

// Route Middlewares
// This will prefix all routes in AuthRouter with '/auth' (e.g. '/auth/register', '/auth/login')
// app.use('/auth', AuthRouter);

// Route for user creation
router.post("/register", auth.createUser);

// Route for login
router.post("/login", auth.login);

export default router;

```

10. Create a folder called routes and a file called `UserRoutes.js` to define the routes for the REST API:

```javascript
// routes/UserRoutes.js
import express from 'express';
import UserController from '../controllers/UserController.js';
import auth from '../services/Authenticate.js';

const router = express.Router(); // Create a new router

// Route Middlewares
// This will prefix all routes in UserRouter with '/users' (e.g. '/users/:email', '/users/id/:id')
// app.use('/users', UserRouter);

// Route for user deletion
router.delete("/:email", auth.authenticate, UserController.deleteUserByEmail);
router.delete("/id/:id", auth.authenticate, UserController.deleteUserById);

export default router;
```

11. Create a folder called routes and a file called `MovieRoutes.js` to define the routes for the REST API:

```javascript
// routes/MovieRoutes.js
import express from 'express';
import MovieController from '../controllers/MovieController.js';
import auth from '../services/Authenticate.js';

const router = express.Router(); // Create a new router

// Route Middlewares
// This will prefix all routes in MovieRouter with '/movies' (e.g. '/movies', '/movies/:id')
// app.use('/movies', MovieRouter);

// Define routes for Movie also authenticate the routes
router.post("/", auth.authenticate, MovieController.addMovie);
router.get('/', auth.authenticate, MovieController.getAllMovies);
router.get('/:id', auth.authenticate, MovieController.getMovieById);
router.put("/:id", auth.authenticate, MovieController.updateMovieById);
router.delete("/:id", auth.authenticate, MovieController.deleteMovieById);

export default router;
```

12. Create a folder called controllers and a file called `MovieController.js` to define the CRUD operations for movies:

```javascript
// controllers/MovieController.js
import express from 'express';
import db from '../db/db.js';

const app = express(); // Create an instance of an Express app
app.use(express.json()); // This middleware will allow us to parse JSON requests

const MovieController = {

   // ADD a new movie
   addMovie: async (req, res) => {
      const { title, director, year } = req.body;
      try { // Try to execute the query
         await db.query('INSERT INTO movies (title, director, year) VALUES ($1, $2, $3)', [title, director, year]);
         res.status(201).json({ message: "Movie created successfully" });
      } catch (error) {  // If an error occurs, log it and send a 500 status code
         console.error("Error creating movie:", error);
         res.status(500).json({ message: "Internal server error" });
      }
   },

   // GET all movies
   getAllMovies: async (req, res) => {
      try { // Try to execute the query
         const result = await db.query('SELECT * FROM movies');
         res.json(result.rows);
      } catch (error) { // If an error occurs, log it and send a 500 status code
         console.error("Error fetching movies:", error);
         res.status(500).json({message: "Internal server error"});
      }
   },

   // GET movie by id
   getMovieById: async (req, res) => {
      const query = { // Create a query object with the SQL query and the values to be interpolated
         text: 'SELECT * FROM movies WHERE id = $1',
         values: [req.params.id],
      }

      try { // Try to execute the query
         const result = await db.query(query);
         if (result.rows.length > 0) {
            res.json(result.rows[0]);
         } else { // If no movie is found, send a 404 status code
            res.status(404).json({ message: "Movie not found" });
         }
      } catch (error) { // If an error occurs, log it and send a 500 status code
         console.error('Error executing query', error.stack);
         res.status(500).json({ message: "Internal server error" });
      }
   },

   // UPDATE movie by id
   updateMovieById: async (req, res) => {
      const { id } = req.params;
      const { title, director, year } = req.body;
      const query = { // Create a query object with the SQL query and the values to be interpolated
         text: 'UPDATE movies SET title = $1, director = $2, year = $3 WHERE id = $4',
         values: [title, director, year, id],
      }

      try { // Try to execute the query
         await db.query(query);
         res.json({ message: "Movie updated successfully" });
      } catch (error) { // If an error occurs, log it and send a 500 status code
         console.error("Error updating movie:", error);
         res.status(500).json({ message: "Internal server error" });
      }
   },

   // DELETE movie by id
   deleteMovieById: async (req, res) => {
      const { id } = req.params;
      const query = { // Create a query object with the SQL query and the values to be interpolated
         text: 'DELETE FROM movies WHERE id = $1',
         values: [id],
      }

      try { // Try to execute the query
         await db.query(query);
         res.json({ message: "Movie deleted successfully" });
      } catch (error) { // If an error occurs, log it and send a 500 status code
         console.error("Error deleting movie:", error);
         res.status(500).json({ message: "Internal server error" });
      }
   },

};
export default MovieController;
```

13. Create a folder called controllers and a file called `UserController.js` to define the CRUD operations for users:

```javascript
// controllers/UserController.js
import db from '../db/db.js';

const UserController = {

   // Store a user in the database
   storeUserInDatabase: async (req, res) => {
      const {email, password} = req.body;
      const query = { // The query object is used to pass the SQL query and its parameters to the database
         text: 'INSERT INTO users (email, password) VALUES ($1, $2)',
         values: [email, password],
      }

      try {
         const result = await db.query(query);
         if (result.rowCount === 0) {
            // If the user was not added to the database, send a 500 Internal Server Error status code
            res.status(500).send({error: 'User not added to database'});
         } else { // If the user was added to the database, send a 201 Created status code
            res.status(201).send({message: 'User added to database'});
         }
      } catch (err) {
         console.error('Error executing query', err.stack);
         if (err.code === '23505') { // 23505 is the error code for a unique violation in PostgreSQL
            res.status(409).send({error: 'Duplicate entry', details: err.stack});
         } else { // If the error is not a unique violation, send a 500 Internal Server Error status code
            res.status(500).send({error: 'Database error', details: err.stack});
         }
      }
   },

   // Get users from the database by email
   getUserByEmail: async (email) => {
      const query = { // The query object is used to pass the SQL query and its parameters to the database
         text: 'SELECT * FROM users WHERE email = $1',
         values: [email],
      }

      try { // The try block is used to catch any errors that occur when executing the query
         const result = await db.query(query);
         if (result.rowCount === 0) {
            return null;
         } else { // If the query was successful, return the first row of the result
            return result.rows[0];
         }
      } catch (err) { // The catch block is used to handle any errors that occur when executing the query
         console.error('Error executing query', err.stack);
         throw err;
      }
   },

   // Delete a user from the database
   deleteUserByEmail: (req, res) => {
      const email = req.params.email;
      const query = { // The query object is used to pass the SQL query and its parameters to the database
         text: 'DELETE FROM users WHERE email = $1',
         values: [email],
      }

      // The query method is used to execute the SQL query
      db.query(query, (err, result) => {
         if (err) { // The if statement is used to check if an error occurred when executing the query
            console.error('Error executing query', err.stack);
            res.status(500, result).send({error: 'Database error'});
         } else { // If the query was successful, send a 200 OK status code
            res.sendStatus(200).end();
         }
      });
   },

   // Delete a user from the database by id
   deleteUserById: (req, res) => {
      const id = req.params.id;
      const query = { // The query object is used to pass the SQL query and its parameters to the database
         text: 'DELETE FROM users WHERE id = $1',
         values: [id],
      }

      // The query method is used to execute the SQL query
      db.query(query, (err, result) => {
         if (err) { // The if statement is used to check if an error occurred when executing the query
            console.error('Error executing query', err.stack);
            res.status(500, result).send({error: 'Database error'});
         } else { // If the query was successful, send a 200 OK status code
            res.sendStatus(200).end();
         }
      });
   }
}

export default UserController;
```

14. Add npm scripts to the `package.json` file to start the server:

```json
{
  "scripts": {
    "start": "node index.js"
  }
}
```

- Start the server:

```bash
npm start
```

16. Or install nodemon and use it to start the server:

```bash
npm install nodemon --save-dev
```

- or install nodemon globally:

```bash
npm install -g nodemon
```

- and add the following script to the `package.json` file:

```json
{
  "scripts": {
    "start": "nodemon index.js"
  }
}
```
    
- Then start the server using nodemon:

```bash
npm run start
```


17. Test the application using a tool like Postman.

18. The application provides the following API endpoints:

---

### User API Routes

#### POST users/auth/register

Create a new user.

**Parameters:**
- `email`: The email of the user.
- `password`: The password of the user.

**Responses:**
- `200`: User created successfully.
- `500`: Server error.
- 
---

#### POST /users/auth/login

Login a user.

**Authentication:**
- This route requires authentication.

**Parameters:**
- `email`: The email of the user.
- `password`: The password of the user.

**Responses:**
- `200`: User logged in successfully.
- `401`: Unauthorized.
- `500`: Server error.

--- 

#### DELETE /users/:email

Delete a user by Email.

**Authentication:**
- This route requires authentication.

**Parameters:**
- `email`: The email of the user to delete.

**Responses:**
- `200`: User deleted successfully.
- `500`: Server error.

---

#### DELETE /users/id/:id

Delete a user by ID.

**Authentication:**
- This route requires authentication.

**Parameters:**
- `id`: The ID of the user to delete.

**Responses:**
- `200`: User deleted successfully.
- `500`: Server error.

---

### Movie API Routes

#### POST /movies

Create a new movie.

**Authentication:**
- This route requires authentication.

**Parameters:**
- `title`: The title of the movie.
- `director`: The director of the movie.
- `year`: The year the movie was released.

**Responses:**
- `200`: Movie created successfully.
- `500`: Server error.

---

#### GET /movies

Gets all movies.

**Authentication:**
- This route requires authentication.

**Responses:**
- `200`: Movies retrieved successfully.
- `500`: Server error.

---

#### GET /movies/:id

Get a movie by ID.

**Authentication:**
- This route requires authentication.

**Parameters:**
- `id`: The ID of the movie to get.

**Responses:**
- `200`: Movie retrieved successfully.
- `500`: Server error.

---

#### PUT /movies/:id

Updates a movie by ID.

**Authentication:**
- This route requires authentication.

**Parameters:**
- `id`: The ID of the movie to update.
- `title`: The new title of the movie.
- `director`: The new director of the movie.
- `year`: The new year the movie was released.

**Responses:**
- `200`: Movie updated successfully.
- `500`: Server error.

---

#### DELETE /movies/:id

Delete a movie by ID.

**Authentication:**
- This route requires authentication.

**Parameters:**
- `id`: The ID of the movie to delete.

**Responses:**
- `200`: Movie deleted successfully.
- `500`: Server error.

---
