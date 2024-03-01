# Using Nodemon for Development in Node.js Projects

Nodemon is a utility that monitors changes in your Node.js application and automatically restarts the server. Follow these instructions to integrate Nodemon into your Node.js project for smoother development workflow.

## Installation
1. Ensure Node.js and npm are installed on your system. If not, download and install them from [Node.js official website](https://nodejs.org/).
2. Open your command line interface (CLI).
3. Navigate to your project directory.
4. Install Nodemon globally using npm:

```sh
npm install -g nodemon
 ```

## Using Nodemon

1. Once Nodemon is installed, you can start your Node.js application using the nodemon command instead of node.


```shell
nodemon yourScript.js
```

Replace **yourScript.js** with the **entry point** of your Node.js application (e.g., **index.js**). 

2. Nodemon will monitor the files in your project directory for changes. Whenever you save changes to a file, Nodemon will automatically restart the server, allowing you to see the changes without manually restarting the server.

## Custom Configuration

1. You can create a nodemon.json file in your project directory to customize Nodemon's behavior.
2. Example nodemon.json file:


```json
{
"watch": ["src"],
"ext": "js,json"
}
```

- **watch:** Specifies the directories to watch for changes.
- **ext:** Specifies the file extensions to monitor for changes.

## Using Nodemon with npm Scripts

1. You can integrate Nodemon into your package.json scripts for easier usage.
2. Example package.json script:

```json
"scripts": {
  "start": "nodemon index.js"
}
```

This allows you to start your application using **npm start** instead of **nodemon index.js**.

```shell 
npm start
```

Or 

1. You can also create npm dev script that will start our project using nodemon. Add a new dev script to scripts section.

```json
"scripts": {
  "dev": "nodemon index.js",
  "start": "node index.js",
  "test": "echo \"Error: no test specified\" && exit 1"
},
```

Now, you can run your web server using nodemon by typing the following command in your terminal:

```shell 
npm run dev
```

Every time you save changes, they should be seen in the browser immediately.