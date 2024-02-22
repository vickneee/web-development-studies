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

2. **Create a .env File:**

- Open your text editor.
- Create a new file in your project directory named .env.

3. **Define Environment Variables in the .env File:**

- Inside the .env file, define your specific environment variables. For example:

```makefile
PORT=8800
```

4. **Install and Configure dotenv:**

- If you haven't already, install the dotenv package as a dependency:

```bash
npm install dotenv
```

or

```bash
yarn add dotenv
```

5. **Create index.js File to Use Environment Variables:**

- Create a new file named index.js inside the api folder in a text editor.
- You can do this via your text editor or with the command line:

```bash
touch index.js
```

6. **Edit index.js File:**

- Open the index.js file in a text editor.
- Add some simple code to start your server. For example,

```javascript
const http = require('http');
require('dotenv').config(); // Import and configure dotenv to load environment variables

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello, world!\n');
});

const PORT = process.env.PORT || 3000; // Use PORT environment variable or default to 3000
server.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
```

7. **Save Changes:**

- Save your changes to both .env and index.js.

8. **Update package.json with Start Command:**

- Open package.json in a text editor.
- Find the "scripts" section.
- Add a "start" script that runs node index.js:

```json
"scripts": {
  "start": "node index.js"
},
```

9. **Install nodemon:**

- Install nodemon as a development dependency:

```bash
npm install nodemon --save-dev
```
or

```bash
yarn add nodemon --dev
```

10. **Update package.json with nodemon:**

- Modify the "start" script to use nodemon:

```json
"scripts": {
  "start": "nodemon index.js"
},
```

11. **Save Changes:**

- Save your changes to package.json.

12. **Start the Server:**

- In your terminal, run:

```bash
npm start
```

or

```bash
yarn start
```

This will start your server using nodemon.

Now, your server should be running with automatic restarts whenever changes are made to your index.js file. You can test it by accessing http://localhost:8800 or default http://localhost:3000 in your web browser or using tools like Postman.
