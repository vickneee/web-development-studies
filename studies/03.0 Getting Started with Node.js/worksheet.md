# Getting Started with Node.js

## Installation
1. Visit and download **Node.js** on the [Node.js official website](https://nodejs.org/).

## Checking Installation
1. Open your command line interface (CLI).
2. Type `node -v` and press Enter.
3. If Node.js is installed correctly, you should see the version number printed.

## Using `npm init` - Initializing a New Project

The `npm init` command is used to initialize a new Node.js project and create a **package.json** file, which stores metadata about the project and its dependencies.

1. Open your command line interface (CLI).
2. Navigate to the directory where you want to create your project.
3. Type `npm init` and press Enter.
4. Follow the prompts to enter information about your project, such as its name, version, description, entry point, test command, repository, keywords, author, and license.
5. After completing the prompts, npm will generate a **package.json** file in your project directory.

```json
{
  "name": "helloworld",
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

## Running a Node.js Script
1. Create a new file with a **.js** extension.
2. Write your **Node.js** script in the file.
3. Open your CLI and navigate to the directory containing the script.
4. Type `node yourScriptName.js` and press Enter.

## Installing Packages
1. To install packages, use the Node Package Manager (**npm**).
2. Open your CLI.
3. Navigate to your project directory.
4. Type `npm install packagenName` and press Enter.

## Creating a Node.js Server

1. Create a new file **index.js**. 
2. Write your server code.
3. Install any necessary packages using **npm**.

```javascript
const http = require('http');

const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

server.listen(port, () => {
  console.log('Server is running at port ' + port);
});
```

## Running a Node.js Server

1. Run the server by typing `node index.js` in your CLI.

## Additional Resources
- [Node.js Documentation](https://nodejs.org/en/docs/)
- [npm Documentation](https://docs.npmjs.com/)
