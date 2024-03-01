# API Testing with Postman

These instructions provide a basic overview of how to test different API methods using Postman, along with examples of endpoints and request body payloads. Make sure to replace the placeholder values with actual API endpoint URLs, request bodies, and other parameters as required by your API.

### GET Request:

1. Open Postman.
2. Create a new request.
3. Set the request type to GET.
4. Enter the API endpoint URL
   (e.g., get information about all users `https://api.example.com/users`, or a single user `https://api.example.com/users/:userId`).
5. Click "Send" to execute the GET request.
6. View the response data in the response body.

**Example:**

Now, you can test get functionality using **Postman**. Set the method to **GET** and use the following endpoint: http://localhost:3000/api/movies and http://localhost:3000/api/movies/:id.

```javascript
const express = require('express');

const app = express();

const port = 3000;

// Local database
let movies = [
    {id: '1588323375416', title: 'Star Wars: Episode IX - The Rise of Skywalker', year: 2019, director: 'J.J. Abrams'},
    {id: '1588323390624', title: 'The Irishman', year: 2019, director: 'Martin Scorsese'},
    {id: '1588323412643', title: 'Harry Potter and the Sorcerers Stone', year: 2001, director: 'Chris Columbus'}
]

// Get all movies
app.get("/api/movies", (req, res) => {
    res.json(movies);
});

// Get movie by id
app.get("/api/movies/:id", (req, res) => {
    const movieId = req.params.id;

    // Filter movie by id
    const movie = movies.filter(movie => movie.id === movieId);
    if (movie.length > 0)
        res.json(movie);
    else
        res.status(404).end(); // Respond status 404 (Not Found)
});

// Server listen and running on port ...
app.listen(port, () => {
    console.log(`Server is running on port ${port}.`);
});
```

### POST Request:

1. Open Postman.
2. Create a new request.
3. Set the request type to POST.
4. Enter the API endpoint URL
   (e.g., add a single user `https://api.example.com/users` or order a single book `https://api.example.com/orders` ).
5. Add JSON body payload if required (e.g., `{ "name": "John", "email": "john@example.com" }`).
6. Click "Send" to execute the POST request.
7. View the response data in the response body.

Add the following code after the **const app = express();** statement:

We have to define that we are using **Express body-parser**.

```javascript
app.use(express.json()) // Using Express body-parser.
```

Next, we have to implement the **app.post()** route that is listening for **POST* requests on the **/api/movies** endpoint.

```javascript
// Add new movie
app.post("/api/movies", (req, res) => {
    // Extract movie from the request body and generate id. Ids have been generated using the Date.now()
    const newMovie = {'id': Date.now().toString(), ...req.body};

    // Add new movie at the end of the movies array
    movies = [...movies, newMovie];

res.json(newMovie);
});
```

**Note!** We don't have to send the id because that is automatically generated in the web service using **Date.now()**.

Now, you can test create functionality using **Postman**. Set the method to **POST** and use the following endpoint: http://localhost:3000/api/movies.

**Example:**

```javascript
const express = require('express');

const app = express();

app.use(express.json()); // Added Using Express body-parser.

const port = 3000;

// Local database
let movies = [
    {id: '1588323375416', title: 'Star Wars: Episode IX - The Rise of Skywalker', year: 2019, director: 'J.J. Abrams'},
    {id: '1588323390624', title: 'The Irishman', year: 2019, director: 'Martin Scorsese'},
    {id: '1588323412643', title: 'Harry Potter and the Sorcerers Stone', year: 2001, director: 'Chris Columbus'}
]

// Get all movies
app.get("/api/movies", (req, res) => {
    res.json(movies);
});

// Get movie by id
app.get("/api/movies/:id", (req, res) => {
    const movieId = req.params.id;

    // Filter movie by id
    const movie = movies.filter(movie => movie.id === movieId);
    if (movie.length > 0)
        res.json(movie);
    else
        res.status(404).end(); // Respond status 404 (Not Found)
});

// Add new movie
app.post("/api/movies", (req, res) => {
   // Extract movie from the request body and generate id. Ids have been generated using the Date.now()
   const newMovie = {'id': Date.now().toString(), ...req.body};

   // Add new movie at the end of the movies array
   movies = [...movies, newMovie];

   res.json(newMovie);
});

// Server listen and running on port ...
app.listen(port, () => {
    console.log(`Server is running on port ${port}.`);
});
```

### PUT Request:

1. Open Postman.
2. Create a new request.
3. Set the request type to PUT.
4. Enter the API endpoint URL
   (e.g., update every part of user information `https://api.example.com/users/123`, `https://api.example.com/users/:userId`).
5. Add JSON body payload with updated data (e.g., `{ "name": "Updated John", "email": "updatedjohn@example.com" }`).
6. Click "Send" to execute the PUT request.
7. View the response data in the response body.

PUT is used to update or replace an entire resource.

**Example:**

Now, you can test put functionality using **Postman**. Set the method to **PUT** and use the following endpoint: http://localhost:3000/api/movies/:id.

```javascript
const express = require('express');

const app = express();

app.use(express.json()); // Using Express body-parser.

const port = 3000;

// Local database
let movies = [
    {id: '1588323375416', title: 'Star Wars: Episode IX - The Rise of Skywalker', year: 2019, director: 'J.J. Abrams'},
    {id: '1588323390624', title: 'The Irishman', year: 2019, director: 'Martin Scorsese'},
    {id: '1588323412643', title: 'Harry Potter and the Sorcerers Stone', year: 2001, director: 'Chris Columbus'}
]

// Get all movies
app.get("/api/movies", (req, res) => {
    res.json(movies);
});

// Get movie by id
app.get("/api/movies/:id", (req, res) => {
    const movieId = req.params.id;

// Filter movie by id
const movie = movies.filter(movie => movie.id === movieId);
    if (movie.length > 0)
        res.json(movie);
    else
        res.status(404).end(); // Respond status 404 (Not Found)
});

// Add new movie
app.post("/api/movies", (req, res) => {
   // Extract movie from the request body and generate id. Ids have been generated using the Date.now()
   const newMovie = {'id': Date.now().toString(), ...req.body};

   // Add new movie at the end of the movies array
   movies = [...movies, newMovie];

   res.json(newMovie);
});

// Update movie
app.put("/api/movies/:id", (req, res) => {
   const id = req.params.id;
   const updatedMovie = {'id': id, ...req.body};

   // Get the index of updated movie
   const index = movies.findIndex(movie => movie.id === id);
   // Replace updated movie in the array
   movies.splice(index, 1, updatedMovie);

   res.json(updatedMovie);
});

// Server listen and running on port ...
app.listen(port, () => {
    console.log(`Server is running on port ${port}.`);
});
```

### PATCH Request:

1. Open Postman.
2. Create a new request.
3. Set the request type to PATCH.
4. Enter the API endpoint URL
   (e.g., update part of user information `https://api.example.com/users/123`, `https://api.example.com/:userId`).
5. Add JSON body payload with partial data to update (e.g., `{ "email": "updatedemail@example.com" }`).
6. Click "Send" to execute the PATCH request.
7. View the response data in the response body.

PATCH is used to apply partial modifications to a resource.

### DELETE Request:

1. Open Postman.
2. Create a new request.
3. Set the request type to DELETE.
4. Enter the API endpoint URL
   (e.g., delete a user `https://api.example.com/users/123`, `https://api.example.com/:userId`).
5. Click "Send" to execute the DELETE request.
6. View the response data in the response body.

**Example:**

Now, you can test delete functionality using **Postman**. Set the method to **DELETE** and use the following endpoint: http://localhost:3000/api/movies/:id.

```javascript
const express = require('express');

const app = express();

app.use(express.json()); // Using Express body-parser.

const port = 3000;

// Local database
let movies = [
    {id: '1588323375416', title: 'Star Wars: Episode IX - The Rise of Skywalker', year: 2019, director: 'J.J. Abrams'},
    {id: '1588323390624', title: 'The Irishman', year: 2019, director: 'Martin Scorsese'},
    {id: '1588323412643', title: 'Harry Potter and the Sorcerers Stone', year: 2001, director: 'Chris Columbus'}
]

// Get all movies
app.get("/api/movies", (req, res) => {
    res.json(movies);
});

// Get movie by id
app.get("/api/movies/:id", (req, res) => {
    const movieId = req.params.id;

    // Filter movie by id
    const movie = movies.filter(movie => movie.id === movieId);
    if (movie.length > 0)
        res.json(movie);
    else
        res.status(404).end(); // Respond status 404 (Not Found)
});

// Add new movie
app.post("/api/movies", (req, res) => {
   // Extract movie from the request body and generate id. Ids have been generated using the Date.now()
   const newMovie = {'id': Date.now().toString(), ...req.body};

   // Add new movie at the end of the movies array
   movies = [...movies, newMovie];

   res.json(newMovie);
});

// Update movie
app.put("/api/movies/:id", (req, res) => {
   const id = req.params.id;
   const updatedMovie = {'id': id, ...req.body};

   // Get the index of updated movie
   const index = movies.findIndex(movie => movie.id === id);
   // Replace updated movie in the array
   movies.splice(index, 1, updatedMovie);

   res.json(updatedMovie);
});

// Delete movie
app.delete("/api/movies/:id", (req, res) => {
   const id = req.params.id;

   // Get the id of deleting movie
   movies = movies.filter(movie => movie.id !== id);
   res.status(204).end(); // Response with status 204 (No Content)
});

// Server listen and running on port ...
app.listen(port, () => {
    console.log(`Server is running on port ${port}.`);
});
```

We will use the filter() function to remove correct movie from the movies array and finally, we send empty response with status 204 (No Content).
