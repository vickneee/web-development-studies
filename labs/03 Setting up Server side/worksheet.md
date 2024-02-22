# Setting up Server side: Establishing a Node.js Environment for Backend Development

In this section, we'll walk through the step-by-step process of setting up a server-side environment using Node.js. We'll cover everything from initializing a new project to configuring a basic server with automatic restart functionality using nodemon. By following these instructions, you'll be ready to start building robust backend applications with ease.

1. **Create a Folder and Initialize npm:**

- Open your terminal.
- Navigate to the directory where you want to create your project.
- Create a new folder named api:
```bash
mkdir api
```

- Move into the newly created api folder:
```bash
cd api
```

- Initialize npm in this folder:
```bash
npm init -y
```

This will create a package.json file with default values.

2. Create index.js File:

- Create a new file named index.js inside the api folder.
- You can do this via your text editor or with the command line:
```bash
touch index.js
```

3. Edit index.js File:

- Open the index.js file in a text editor.
- Add some simple code to start your server. For example,

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello, world!\n');
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
```

4. Update package.json with Start Command:

- Open package.json in a text editor.
- Find the "scripts" section.
- Add a "start" script that runs node index.js:

```json
"scripts": {
  "start": "node index.js"
},
```

5. Install nodemon:

- Install nodemon as a development dependency:

```bash
npm install nodemon --save-dev
```
or

```bash
yarn add nodemon --dev
```

6. Update package.json with nodemon:

- Modify the "start" script to use nodemon:

```json
"scripts": {
  "start": "nodemon index.js"
},
```

7. Save Changes:

- Save your changes to package.json and index.js.

8. Start the Server:

- In your terminal, run:
- 
```bash
npm start
```

or

```bash
yarn start
```

This will start your server using nodemon.

Now, your server should be running with automatic restarts whenever changes are made to your index.js file. You can test it by accessing http://localhost:3000 in your web browser or using tools like cURL or Postman.
