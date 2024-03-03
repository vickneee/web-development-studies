# Using MongoDB and Mongoose with Node.js

To create an Express.js project with MongoDB integration, you'll typically need several files and directories. Here's a basic project structure along with the files you'll commonly have:

**project-root/** You can name the project-root directory anything you like.

**controllers/:** This directory contains controller files responsible for handling the business logic of your application. Controllers interact with models to perform CRUD operations and handle requests and responses.

**routes/:** This directory contains route handlers for different parts of your API. Each route handler file will handle specific API routes and their corresponding logic.

**models/:** This directory contains Mongoose models that define the structure of your MongoDB documents. Each model file corresponds to a collection in your MongoDB database.

**.env:** This file holds environment variables that your application needs. It should include sensitive information like database connection strings or API keys.

**index.js:** This is your main application file where you initialize and configure your Express app, set up routes, and start the server.

**package.json:** This file holds metadata relevant to the project and manages project dependencies. You can create it by running npm init in your project directory.

## Here's a basic structure for your project:

```bash
Copy code
project-root/
│
├── controllers/
│   └── MovieController.js
│ 
├── routes/
│   └── MovieRoutes.js
│ 
├──  models/
│   └── Movie.js
│ 
├── .env
├── index.js
└── package.json

```

Now, let's create these files:

## package.json: 

You can create this file by running npm init in your project directory.

```shell
npm init
```

## .env:

Add port and your MongoDB URL. 

```makefile
PORT=3000
MONGO_URL=<your-mongodb-connection-url>

MONGO_URL_EXAMPLE=mongodb+srv://MovieDB:MovieDB@cluster0.4nkj1va.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0
````

## Custom database name:

You can consider it a custom database name because it's the name you specify for your MongoDB database. This name can be anything you choose, and it's typically reflective of the purpose or content of the database.

If you're using custom database name in MongoDB Atlas, the URI might look something like this (**Note!** **your_database_name_here**):

```
MONGO_URL=mongodb+srv://username:password@clustername.mongodb.net/your_database_name_here?retryWrites=true&w=majority
```

Example:

```
MONGO_URL=mongodb+srv://MovieDB:MovieDB@cluster0.4nkj1va.mongodb.net/moviedb?retryWrites=true&w=majority&appName=Cluster0
```

## index.js:

```javascript
import express from 'express';
import mongoose from 'mongoose';
import dotenv from 'dotenv';
import MovieRouter from './routes/MovieRoutes.js';

const app = express();
dotenv.config();

// Middleware
app.use(express.json());

// Routes
app.use('/', MovieRouter);

// MongoDB connection
mongoose.connect(process.env.MONGO_URL)
    .then(() => console.log('Connected to MongoDB'))
    .catch(err => console.error('Failed to connect to MongoDB', err));

// Start the server
const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`Server running on port ${port}`));
```

## controllers/movieController.js:

```javascript
import Movie from '../models/Movie.js';

const MovieController = {
  // Add new movie
  addMovie: async (req, res) => {
    const { title, director, year } = req.body;
    const movie = new Movie({ title, director, year });

    try {
      const newMovie = await movie.save();
      res.status(201).json({ newMovie });
    } catch (err) {
      res.status(500).json({ message: err.message });
    }
  },

  // Get a movie by ID
  getMovieById: async (req, res) => {
    try {
      const movie = await Movie.findById(req.params.id);
      if (!movie) {
        return res.status(404).json({ message: "Movie not found!!!" });
      }
      return res.status(200).json(movie);
    } catch (error) {
      console.error("Error fetching movie:", error);
      return res.status(500).json({ message: "Internal server error" });
    }
  },

  // Get all movies
  getAllMovies: async (req, res) => {
    try {
      const movies = await Movie.find();
      res.send(movies);
    } catch (error) {
      res.status(500).json({ error: error.message });
    }
  },

  // Update a movie by ID
  updateMovieById: async (req, res) => {
    const response = await Movie.findOneAndUpdate({ _id: req.params.id }, req.body, { new: true });
    if (response === null) {
      return res.status(404).json({ message: "Movie not found" });
    }
    return res.status(200).json({ message: "Movie updated" });
  },

  // Delete a movie by ID
  deleteMovieById: async (req, res) => {
    const movie = await Movie.findByIdAndDelete(req.params.id);
    if (movie === null) {
      return res.status(404).json({ message: "Movie not found" });
    }
    return res.status(200).json({ message: "Movie deleted" });
  },

  // Delete a movie by title
  deleteMovieByTitle: async (req, res) => {
    try {
      const movie = await Movie.findOneAndDelete({ title: req.body.title });
      if (movie) {
        return res.status(200).json({ message: "Movie deleted" });
      } else {
        return res.status(404).json({ message: "Movie not found" });
      }
    } catch (error) {
      console.error("Error deleting movie:", error);
      return res.status(500).json({ message: "Internal server error" });
    }
  }
};

export default MovieController;
```

## models/Movie.js:

```javascript
import mongoose from 'mongoose';

const movieSchema = new Schema({
   title: {
      type: String,
      required: true,
      maxLength: 150
   },
   director: {
      type: String,
      required: true,
      maxlength: 200
   },
   year: {
      type: Number,
      required: true
   }
});
const Movie = mongoose.model('Movie', movieSchema);

export default Movie;
```

## routes/movieRoutes.js:

```javascript
import express from 'express';
import MovieController from './MovieController.js';

const router = express.Router();

// Define routes
router.post("/movies", MovieController.addMovie);
router.get('/movies/:id', MovieController.getMovieById);
router.get('/movies', MovieController.getAllMovies);
router.put("/movies/:id", MovieController.updateMovieById);
router.delete("/movies/:id", MovieController.deleteMovieById);
router.delete("/movies", MovieController.deleteMovieByTitle);

export default router;
```

Ensure you have MongoDB installed and running locally or replace MONGO_URL in the .env file with your MongoDB Atlas connection URL.

## Separate Controllers Examples

## Add a New Movie:

Mongoose model has save() function that can be used to insert data to the MongoDB. If the save was invokes successfuly we will send status 201 and the new movie in the response. In the case of error, we will send status 500 and error message in the response.

```javascript
import express from 'express';
import Movie from '../models/Movie.js';

const router = express.Router();


// Add new movie
router.post("/movies", async (req, res) => {
   const movie = new Movie({
      title: req.body.title,
      director: req.body.director,
      year: req.body.year
   });

   try {
      const newMovie = await movie.save();
      res.status(201).json({ newMovie });
   } catch(err) {
      return res.status(500).json({ message: err.message });
   }
});


export default router;
```

## Get a Movie by ID:

```javascript
import express from 'express';
import Movie from '../models/Movie.js';

const router = express.Router();


// Get a movie
router.get('/movies/:id', async (req, res) => {
   try {
      const movie = await Movie.findById(req.params.id);
      if (!movie) {
         return res.status(404).json({message: "Movie not found!!!"});
      }
      return res.status(200).json(movie);
   } catch (error) {
      // Handle errors
      console.error("Error fetching movie:", error);
      return res.status(500).json({ message: "Internal server error" });
   }
});


export default router;
```

## Get all Movies:

```javascript
import express from 'express';
import Movie from '../models/Movie.js';

const router = express.Router();


// Get all movies
router.get('/movies', async (req, res) => {
    try {
        const movies = await Movie.find();
            res.send(movies);
    } catch (error) {
            res.status(500).json({ error: error.message });
    }
});


export default router;
```

## Update a Movie by ID:

```javascript
import express from 'express';
import Movie from '../models/Movie.js';

const router = express.Router();

// Update movie by id
router.put("/movies/:id", async (req, res) => {
    const response = await Movie.findOneAndUpdate({ _id: req.params.id }, req.body, { new: true });
    if (response === null) {
        return res.status(404).json({ message: "Movie not found" });
    }
    return res.status(200).json({ message: "Movie updated" });
});


export default router;
```

## Delete a Movie by ID:

```javascript
import express from 'express';
import Movie from '../models/Movie.js';

const router = express.Router();

// Delete movie by id
router.delete("/movies/:id", async (req, res) => {
   const movie = await Movie.findByIdAndDelete(req.params.id);
   if (movie === null) {
      return res.status(404).json({message: "Movie not found"});
   }
   return res.status(200).json({message: "Movie deleted"});
});


export default router;
```

## Delete a Movie by Title:

```javascript
import express from 'express';
import Movie from '../models/Movie.js';

const router = express.Router();

// Delete movie by title
router.delete("/api/movies", async (req, res) => {
   try {
      const movie = await Movie.findOneAndDelete({ title: req.body.title });
      if (movie) {
         return res.status(200).json({ message: "Movie deleted" });
      }
      else if (!movie) {
         return res.status(404).json({ message: "Movie not found" });
      }


   } catch (error) {
      console.error("Error deleting movie:", error);
      return res.status(500).json({ message: "Internal server error" });
   }
});

export default router;
```

## Example of Blog Collections:

The blog collections consist of three main entities: users, posts, and comments. Users store information about registered users, posts contain details of blog posts authored by users, and comments capture user-generated feedback on blog posts.

1. **users**
    - Stores information about users registered on the platform.
    - Example Fields:
        - `username`: (String) The unique username of the user.
        - `email`: (String) The email address of the user.
        - `password`: (String) The hashed password of the user.
        - `createdAt`: (Date) The date when the user account was created.

2. **posts**
    - Stores information about blog posts created by users.
    - Example Fields:
        - `title`: (String) The title of the blog post.
        - `content`: (String) The content of the blog post.
        - `author`: (ObjectId) The ID of the user who created the post.
        - `createdAt`: (Date) The date when the post was created.
        - `updatedAt`: (Date) The date when the post was last updated.

3. **comments**
    - Stores user made comments on blog posts.
    - Example Fields:
        - `content`: (String) The content of the comment.
        - `post_id`: (ObjectId) The ID of the post the comment belongs to.
        - `author`: (ObjectId) The ID of the user who made the comment.
        - `createdAt`: (Date) The date when the comment was created.
        - `updatedAt`: (Date) The date when the comment was last updated.
