# Vitest

**Vitest (https://vitest.dev/)** is a testing framework developed for Vite that we are using to create our React app. **Vitest** provides **Jest** like API that is a good replacement in most cases.

To start using **Vitest**, we have to install it to our project using the following command in your React project folder:

```bash
npm install -D vitest jsdom
```

or

```bash
npm install --save-dev vitest jsdom
```

Let's create a file testSetup.js in the project root with the following content

```javascript
import { afterEach } from 'vitest'
import { cleanup } from '@testing-library/react'
import '@testing-library/jest-dom/vitest'

afterEach(() => {
  cleanup()
})copy
```

Now, after each test, the function cleanup is performed that resets the jsdom that is simulating the browser.

**Vitest** also requires some configuration to work. The configuration is done using the Vite configuration file **vite.config.js** that you can find from the root of your React project folder. The **vite.config.js** file looks like the following:

```javascript
// vite.config.js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [react()],
})
```

To configure **Vitest**, you have to add test property to the configuration file:

```javascript
// vite.config.js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [react()], 
    test: {
        // vitest configuration
    }
})
```

When testing React components, we need to install other testing libraries:
- **React Testing Library** - Helps you to test React components
- **jest-dom** library - provides custom matchers that you can use to extend vitest (See all matchers https://github.com/testing-library/jest-dom)
- **jsdom** - Provides Browser API

To install these libraries, type the following terminal command in the project directory:

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

or 

```bash
npm install -D jsdom @testing-library/react @testing-library/jest-dom
```

After that, we have to add the following configuration to our vite.config.js file:

```javascript
// vite.config.js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [react()], 
    test: {
        globals: true, 
        environment: 'jsdom',
    }
})
```
, where

**globals** - Enables **Jest** globals API (https://jestjs.io/docs/api).
**environment** - Defines the environment that will be used for testing (Node.js is default). We will use browser-based environment **jsdom**.

To run tests, we should also add npm script to our project's **package.json** file

```json
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "test": "vitest",
  "lint": "eslint src --ext js,jsx --report-unused-disable-directives --max-warnings 0",
  "preview": "vite preview"
},
```

or

We add a script to the package.json file to run the tests:

```javascript
{
  "scripts": {
    // ...
    "test": "vitest run"
  }
  // ...
}copy
```

Now, you can run tests by typing the following terminal command in the project directory:

```bash
npm run test
```

The command finds all test files (**.test.js** or **.test.jsx** extension) from your project and runs the test cases. By default, it will run in watch mode and re-run the tests each time when code is updated. You can stop watch mode by pressing 'q' in the terminal.

---

## Writing Test Cases with Vitest and React Testing Library

Let's first create a simple test case that renders our **App component**. Create a new file called **App.test.jsx** in the **src** folder. Import the **App component** that we want to render and create the first test case like shown in the next code snippet:

```javascript
import App from './App';

test('renders App component', () => {

});
```

**React testing library** provides a render method that can be used to render a **React component** for testing purposes. First, we import render method, and then we call it by passing **App component** that we want to render.

```javascript
import { render } from '@testing-library/react';
import App from './App';

test('renders App component', () => {
    render(<App />);
});
```

We will also import screen method from a **React testing library**. The screen method returns an object that provides queries (i.e. **getByText**, **getByLabelText** etc.) that are bound to the whole rendered **document.body**. These queries can be used to find elements from the HTML document. In the following code, we are using **getByText** query to find our header text:

```javascript
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders App component', () => {
    render(<App />);
    const header = screen.getByText('My Todolist');
});
```

```bash
npm run test
```
