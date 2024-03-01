# Getting Started with Express.js

1. Create a new folder called helloexpress.
2. Navigate to the helloexpress directory.

## Using `npm init` - Initializing a New Project

The `npm init` command is used to initialize a new Node.js project and create a `package.json` file, which stores metadata about the project and its dependencies.

1. Open your command line interface (CLI).
2. Navigate to the directory where you want to create your project.
3. Type `npm init` and press Enter.
4. Follow the prompts to enter information about your project, such as its name, version, description, entry point, test command, repository, keywords, author, and license.
5. After completing the prompts, npm will generate a `package.json` file in your project directory.

```javascript
{
  "name": "helloexpress",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

## Add start script to the JSON file.

```json 
{
  "name": "helloexpress",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
},
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
}
}
```

Now, you can invoke our start script using the npm start command which then executes node index.js command for you.

## Creating an Express Application - Installing Express.js

1. To install packages, use the Node Package Manager (npm).
2. Open your CLI.
3. Navigate to your project directory.
4. Type `npm install packagenName` and press Enter.

````shell
npm install express
````
5. Now you have installed Express dependency.

```json
{
  "name": "helloexpress",
  "version": "1.0.0",
  "description": "Node express app",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
},
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1",
  }
}
```

## Creating an index.js File

1. Create a new file **index.js** in your project directory.
2. Import Express module at the top of your file

```javascript
const express = require('express'); // Import Express module at the top of your file

const app = express(); // Create an Express application by calling the express() function

const port = 3000;

app.get("/", (req, res) => {
    res.send("Hello Express!");
})

app.listen(port, () => {
    console.log(`Server is running on port ${port}.`);
});
```

## Creating Routes

1. Define routes to handle HTTP requests.
2. Use the HTTP methods (GET, POST, PUT, DELETE, etc.) to define route handlers.
3. Example:

In the express library, all HTTP methods have their own routing methods. The code below shows how to make routing for POST requests for the /hello endpoint.

```javascript
app.get("/", (req, res) => {
    res.send("Hello Express!");
})

app.post("/hello", (req, res) => {
  res.send("Hello World!");
})

app.get("/home", (req, res) => {
    res.send("Welcome to our page");
})

app.get("/about", (req, res) => {
    res.send("About us...");
})
```

Now, if you save the **index.js** file and navigate to **the/home endpoint** you should see the ‘Welcome to our page’- message.

## How to Use Route Parameters

1. The following example have one route parameter called name:

```javascript
const express = require('express');

const app = express();

const port = 3000;

app.get("/home/:name", (req, res) => {
    res.send("Welcome " + req.params.name);
})

app.listen(port, () => {
    console.log(`Server is running on port ${port}.`);
});
```

Now, if you navigate to http://localhost:3000/home/Victoria, you should see the following message.

- Welcome Victoria

2. You can also have multiple route parameters, for example first and last names:

```javascript
app.get("/home/user/:firstname/:lastname", (req, res) => {
    res.send(`Welcome ${req.params.firstname} ${req.params.lastname}`);
})
```

Now, if you start your app and navigate to http://localhost:3000/home/user/Victoria/Vavulina, you should see following message.

- Welcome Victoria Vavulina

## Sending JSON in response

1. You can also return data in JSON format by using the res.json() function.

```javascript
app.get("/home/user/", (req, res) => {
  res.json({username: 'Victoria'});
})
```

Now, if you navigate to the http://localhost:3000/home/user endpoint, you can see that response data is in JSON format.

```javascript
{
    "username": "Victoria"
}
```

## Response Status

Handling the response status

You can send an empty response using the res.end() method.

If you want to set the status of the response, you can use either the status() or sendStatus() methods. 

The difference between these two methods is that sendStatus() will set the status and send the response. The status() method only sets the status. See the examples below:

```javascript
// Send empty response with the status 404 using the status() and sendStatus() methods
res.status(404).end();
res.sendStatus(404);
```

An empty response with the status is used, for example, in the case that some resource is not found.

```javascript
app.get("/api/something", (req, res) => {
    if (found)
        res.json(result);
    else
        res.status(404).end
})
````
## Starting the Server:

```shell
npm start
```