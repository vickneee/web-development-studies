# NodeMovie PostgreSQL Database Example

This is a simple example of a Node.js application that uses a Postgres database. The application is a REST API that allows users to perform CRUD operations on a database of movies. The application uses the `pg` module to connect to the Postgres database and execute SQL queries.

## Setup

1. Install the `pg` module using npm:

   ```bash
   npm install pg
   ```
   
2. Create a Postgres database and a table to store movies:

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

3. Create a file called `.env` to store environment variables:

```dotenv
# .env
# You can generate a random secret key using a tool like https://randomkeygen.com/
SECRET_KEY="5b1a3923cc1e1e19523fd5c3f20b409509d3ff9d42710a4da095a2ce285b009f0c3730cd9b8e1af3eb84d";
```

4. Create a file called `index.js`:

```javascript
// index.js
import express from 'express';
import dotenv from 'dotenv';
import MovieRouter from './routes/MovieRoutes.js';
import db from './db/db.js';

import bodyParser from "body-parser";

const app = express();

dotenv.config(); // Load environment variables

// Middleware
app.use(express.json());
app.use(bodyParser.json());

// Routes
app.use('/', MovieRouter);

// PostgresSQL connection
db.connect()
    .then(() => console.log('Connected to Postgres'))
    .catch(err => console.error('Failed to connect to Postgres', err));

// Start the server
const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`Server running on port ${port}`));
```

5. Create a file called `db.js` to connect to the database:

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

6. Create a file called `users.js` to define the CRUD operations for users:

```javascript
// db/users.js
import db from './db.js';

// Get user by email
const getUserByEmail = (email, next) => {
   const query = {
      text: 'SELECT * FROM users WHERE email = $1',
      values: [email],
   }

   db.query(query, (err, result) => {
      if (err) {
         return console.error('Error executing query', err.stack)
      } else {
         console.log(result.rows); // Add this line
         next(result.rows);
      }
   })
}

// Store user in database
const storeUserInDatabase = (email, hash, next) => {
   const query = {
      text: 'INSERT INTO users (email, password) VALUES ($1, $2)',
      values: [email, hash],
   }

   db.query(query, (err, result) => {
      if (err) {
         if (err.code === '23505') { // Unique violation error
            next('User with this email already exists');
         } else {
            console.error('Error executing query', err.stack);
            next('Database error');
         }
      } else {
         console.log(result.rows); // Add this line
         next(null);
      }
   })
}

// Delete user from database
const deleteUserByEmail = (email, next) => {
   const query = {
      text: 'DELETE FROM users WHERE email = $1',
      values: [email],
   }

   db.query(query, (err, result) => {
      if (err) {
         console.error('Error executing query', err.stack);
         next('Database error');
      } else {
         console.log(result.rows);
         next(null);
      }
   })
}

export {getUserByEmail, storeUserInDatabase, deleteUserByEmail};
```

7. Create a file called `MovieRoutes.js` to define the routes for the REST API:

```javascript
// routes/MovieRoutes.js
import express from 'express';
import MovieController from '../controllers/MovieController.js';
import auth from '../services/authenticate.js';
import { deleteUserByEmail } from '../db/users.js';

const router = express.Router();

// Define routes
router.post("/movies", auth.authenticate, MovieController.addMovie);
router.get('/movies/:id', auth.authenticate, MovieController.getMovieById);
router.get('/movies', auth.authenticate, MovieController.getAllMovies);
router.put("/movies/:id", auth.authenticate, MovieController.updateMovieById);
router.delete("/movies/:id", auth.authenticate, MovieController.deleteMovieById);

// Route for user deletion
router.delete("/users/:email", auth.authenticate, (req, res) => {
   const email = req.params.email;
   deleteUserByEmail(email, (err) => {
      if (err) {
         res.status(500).send({error: err});
      } else {
         res.sendStatus(200).end();
      }
   });
});

// Route for login
router.post("/login", auth.login);

// Route for user creation
router.post("/users", auth.createUser);

export default router;

```

8. Create a file called `MovieController.js` to define the CRUD operations for movies:

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
