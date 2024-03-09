# Using PostgreSQL Database with Node.js

**Example:**

This code sets up an Express server with endpoints to perform CRUD operations (Create, Read, Update, Delete) on a Postgres database table named movies. It uses an express application to handle HTTP requests and a pg connection pool to interact with the postgres database.

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

The `db.js` file exports a connection pool to the Postgres database. The pool is created using the `pg` library and the connection details for the Postgres database.

```javascript
// db.js
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

The `MovieRoutes.js` file defines the routes for the movies API. It uses the `express.Router` class to create a router and defines the routes for adding, getting, updating, and deleting movies.

```javascript
// MovieRoutes.js
import express from 'express';
import MovieController from '../controllers/MovieController.js';

const router = express.Router();

// Define routes
router.post("/movies", MovieController.addMovie);
router.get('/movies/:id', MovieController.getMovieById);
router.get('/movies', MovieController.getAllMovies);
router.put("/movies/:id", MovieController.updateMovieById);
router.delete("/movies/:id", MovieController.deleteMovieById);

export default router;
```

The `MovieController.js` file defines the controller for the movies API. It contains methods to add, get, update, and delete movies from the database.

```javascript
// MovieController.js
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
            const result = await pool.query(query);
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

**Explanation:**

We suppose to have a table named movies in our Postgres database with the following schema:

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
```

## API Endpoints:

And we want to create an API to perform CRUD operations on this table.
API endpoints are defined in the `MovieRoutes.js` file and the corresponding controller methods are defined in the `MovieController.js` file.

- POST /movies - Add a new movie
- GET /movies/:id - Get a movie by id
- GET /movies - Get all movies
- PUT /movies/:id - Update a movie by id
- DELETE /movies/:id - Delete a movie by id





