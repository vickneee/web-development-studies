# NodeMovie PostgreSQL Database Example

This is a simple example of a Node.js application that uses a Postgres database. The application is a REST API that allows users to perform CRUD operations on a database of movies. The application uses the `pg` module to connect to the Postgres database and execute SQL queries.

## Setup

1. Create a new directory for the project and navigate to it:

   ```bash
   mkdir node-movie-postgres
   cd node-movie-postgres
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

6. Create a file called `index.js`:

```javascript
// index.js
import express from 'express';
import dotenv from 'dotenv';
import MovieRouter from './routes/MovieRoutes.js';
import UserRouter from './routes/UserRoutes.js';
import AuthRouter from "./routes/AuthRoutes.js";
import db from './db/db.js';

import bodyParser from "body-parser";

const app = express();

dotenv.config(); // Load environment variables

// Middleware
app.use(express.json());
app.use(bodyParser.json());

// Route Middlewares
app.use('/movies', MovieRouter);
app.use('/users', UserRouter);
app.use('/auth', AuthRouter);
app.use('/login', AuthRouter);

// PostgresSQL connection
db.connect()
        .then(() => console.log('Connected to Postgres'))
        .catch(err => console.error('Failed to connect to Postgres', err));

// Start the server
const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`Server running on port ${port}`));

```

7. Create a file called `db.js` to connect to the database:

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

8. Create a file called `Authenticate.js` to define the authentication service:

```javascript
// services/Authenticate.js
import jwt from 'jsonwebtoken';
import bcrypt from 'bcrypt';
import UserController from "../controllers/UserController.js";

// User creation
const createUser = (req, res) => {
   // Extract user details from the request body
   const email = req.body.email;
   const password = req.body.password;

   if (!email || !password) {
      console.log('Email or password not provided');
      return res.sendStatus(400).end();
   }

   // Hash the password
   const salt = bcrypt.genSaltSync(10);
   // Replace plain password with hashed password
   req.body.password = bcrypt.hashSync(password, salt);
   // Store the user in the database
   UserController.storeUserInDatabase(req, res);
}

// User login
const login = (req, res) => {
   // Extract email and password from the request body
   const email = req.body.email;
   const password = req.body.password;

   if (!email || !password) {
      console.log('Email or password not provided');
      return res.sendStatus(400).end();
   }

   UserController.getUserByEmail(email, (user) => {
      if (user.length > 0) {
         const hash = user[0].password;
         // Create JSON Web Token
         const token = jwt.sign({userId: email}, process.env.SECRET_KEY);

         // If password match, send the token
         if (bcrypt.compareSync(password, hash))
            res.send({token});
         else {
            console.log('Password does not match');
            res.sendStatus(400).end();
         }
      } else {
         console.log('User not found');
         res.sendStatus(400).end();
      }
   });
}

// User authentication
const authenticate = (req, res, next) => {
   const authHeader = req.header('Authorization');

   if (!authHeader) {
      console.log('Authorization header not provided');
      return res.sendStatus(400).end();
   }

   const token = authHeader.split(' ')[1]; // Extract the token from the Authorization header

   console.log('Token:', token); // Log the token

   jwt.verify(token, process.env.SECRET_KEY, (err, decoded) => { // Use the correct secret key
      console.log('JWT verify error:', err); // Log the JWT verify error
      console.log('JWT verify decoded:', decoded); // Log the JWT verify decoded

      if (err) {
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

9. Create a file called `AuthRoutes.js` to define the routes for user authentication:

```javascript
// routes/AuthRoutes.js
import express from 'express';
import auth from '../services/authenticate.js';

const router = express.Router();

// Route Middlewares
// app.use('/auth', AuthRouter);
// app.use('/login', AuthRouter);

// Route for user creation
router.post("/", auth.createUser);

// Route for login
router.post("/", auth.login);

export default router;
```

10. Create a file called `UserRoutes.js` to define the routes for the REST API:

```javascript
// routes/UserRoutes.js
import express from 'express';
import UserController from '../controllers/UserController.js';
import auth from '../services/authenticate.js';

const router = express.Router();

// Route Middlewares
// app.use('/users', UserRouter);

// Route for user deletion
router.delete("/:email", auth.authenticate, UserController.deleteUserByEmail);
router.delete("/id/:id", auth.authenticate, UserController.deleteUserById);

export default router;

```

11. Create a file called `MovieRoutes.js` to define the routes for the REST API:

```javascript
// routes/MovieRoutes.js
import express from 'express';
import MovieController from '../controllers/MovieController.js';
import auth from '../services/authenticate.js';
import { deleteUserByEmail } from '../db/users.js';

const router = express.Router();

// Route Middlewares
// app.use('/movies', MovieRouter);

// Define routes
router.post("/", auth.authenticate, MovieController.addMovie);
router.get('/', auth.authenticate, MovieController.getAllMovies);
router.get('/:id', auth.authenticate, MovieController.getMovieById);
router.put("/:id", auth.authenticate, MovieController.updateMovieById);
router.delete("/:id", auth.authenticate, MovieController.deleteMovieById);

export default router;
```

12. Create a file called `MovieController.js` to define the CRUD operations for movies:

```javascript
// controllers/MovieController.js
import express from 'express';
import db from '../db/db.js';

const app = express();
app.use(express.json());

const MovieController = {

   // ADD a new movie
   addMovie: async (req, res) => {
      const { title, director, year } = req.body;
      try {
         await db.query('INSERT INTO movies (title, director, year) VALUES ($1, $2, $3)', [title, director, year]);
         res.status(201).json({ message: "Movie created successfully" });
      } catch (error) {
         console.error("Error creating movie:", error);
         res.status(500).json({ message: "Internal server error" });
      }
   },

   // GET all movies
   getAllMovies: async (req, res) => {
      try {
         const result = await db.query('SELECT * FROM movies');
         res.json(result.rows);
      } catch (error) {
         console.error("Error fetching movies:", error);
         res.status(500).json({message: "Internal server error"});
      }
   },

   // GET movie by id
   getMovieById: async (req, res) => {
      const query = {
         text: 'SELECT * FROM movies WHERE id = $1',
         values: [req.params.id],
      }

      try {
         const result = await db.query(query);
         if (result.rows.length > 0) {
            res.json(result.rows[0]);
         } else {
            res.status(404).json({ message: "Movie not found" });
         }
      } catch (error) {
         console.error('Error executing query', error.stack);
         res.status(500).json({ message: "Internal server error" });
      }
   },
   
   // UPDATE movie by id
   updateMovieById: async (req, res) => {
      const { id } = req.params;
      const { title, director, year } = req.body;
      const query = {
         text: 'UPDATE movies SET title = $1, director = $2, year = $3 WHERE id = $4',
         values: [title, director, year, id],
      }

      try {
         await db.query(query);
         res.json({ message: "Movie updated successfully" });
      } catch (error) {
         console.error("Error updating movie:", error);
         res.status(500).json({ message: "Internal server error" });
      }
   },

   // DELETE movie by id
   deleteMovieById: async (req, res) => {
      const { id } = req.params;
      const query = {
         text: 'DELETE FROM movies WHERE id = $1',
         values: [id],
      }

      try {
         await db.query(query);
         res.json({ message: "Movie deleted successfully" });
      } catch (error) {
         console.error("Error deleting movie:", error);
         res.status(500).json({ message: "Internal server error" });
      }
   },

};
export default MovieController;
```

13. Create a file called `UserController.js` to define the CRUD operations for users:

```javascript
// controllers/UserController.js
import db from '../db/db.js';

const UserController = {

   storeUserInDatabase: (req, res) => {
      const { email, password } = req.body;
      const query = {
         text: 'INSERT INTO users (email, password) VALUES ($1, $2)',
         values: [email, password],
      }

      db.query(query, (err, result) => {
         if (err) {
            console.error('Error executing query', err.stack);
            res.status(500, result).send({error: 'Database error'});
         } else {
            res.sendStatus(200).end();
         }
      });
   },

   getUserByEmail: (req, res) => {
      const email = req.params.email;
      const query = {
         text: 'SELECT * FROM users WHERE email = $1',
         values: [email],
      }

      db.query(query, (err, result) => {
         if (err) {
            console.error('Error executing query', err.stack);
            res.status(500).send({error: 'Database error'});
         } else {
            res.json(result);
         }
      });
   },

   deleteUserByEmail: (req, res) => {
      const email = req.params.email;
      const query = {
         text: 'DELETE FROM users WHERE email = $1',
         values: [email],
      }

      db.query(query, (err, result) => {
         if (err) {
            console.error('Error executing query', err.stack);
            res.status(500, result).send({error: 'Database error'});
         } else {
            res.sendStatus(200).end();
         }
      });
   },

   deleteUserById: (req, res) => {
      const id = req.params.id;
      const query = {
         text: 'DELETE FROM users WHERE id = $1',
         values: [id],
      }

      db.query(query, (err, result) => {
         if (err) {
            console.error('Error executing query', err.stack);
            res.status(500, result).send({error: 'Database error'});
         } else {
            res.sendStatus(200).end();
         }
      });
   }
}

export default UserController;
```

14. Run the application using the following command:

```bash
node index.js
```

15. Test the application using a tool like Postman.

16. The application provides the following API endpoints:

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
