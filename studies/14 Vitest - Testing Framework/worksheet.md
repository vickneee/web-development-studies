# Vitest

**Vitest (https://vitest.dev/)** is a testing framework developed for Vite that we are using to create our React app. **Vitest** provides **Jest** like API that is a good replacement in most cases.

To start using **Vitest**, we have to install it to our project using the following command in your React project folder:

```bash
npm install -D vitest
```

Vitest also requires some configuration to work. The configuration is done using the Vite configuration file vite.config.js that you can find from the root of your React project folder. The vite.config.js file looks like the following:

```javascript
// vite.config.js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [react()],
})
```

To configure Vitest, you have to add test property to the configuration file:

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
- **jest-dom** library - provides custom matchers that you can use to extend vitest(See all matchers https://github.com/testing-library/jest-dom)
- **jsdom** - Provides Browser API

To install these libraries, type the following terminal command in the project directory:

```bash
npm install -D jsdom @testing-library/react @testing-library/jest-dom
```

After the installation, we have to add the following configuration to our vite.config.js file:

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

**globals** - Enables Jest globals API (https://jestjs.io/docs/api).
**environment** - Defines the environment that will be used for testing (Node.js isdefault). We will use browser-based environment **jsdom**.

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

Now, you can run tests by typing the following terminal command in the project directory:

```bash
npm run test
```

The command finds all test files (.test.js or .test.jsx extension) from your project and runs the test cases. By default, it will run in watch mode and re-run the tests each time when code is updated. You can stop watch mode by pressing 'q' in the terminal.

## Writing Test Cases with Vitest and React Testing Library

Let's first create a simple test case that renders our App component. Create a new file called **App.test.jsx** in the **src** folder. Import the **App component** that we want to render and create the first test case like shown in the next code snippet:

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

We will also import screen method from a **React testing library**. The screen method returns an object that provides queries (i.e. **getByText**, **getByLabelText** etc.) that are bound to the whole rendered document.body. These queries can be used to find elements from the HTML document. In the following code, we are using **getByText** query to find our header text:

```javascript
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders App component', () => {
render(<App />);
const header = screen.getByText('My Todolist');
});
```

Then, we check if the 'My Todolist'- **header** text exists in the DOM using the **toBeInTheDocument()** matcher from the **jest-dom library**. We have to import matchers and use extend method to extend **Vitest** matchers.

```javascript
import { render, screen } from '@testing-library/react';
import matchers from '@testing-library/jest-dom/matchers';
import App from './App';

excpect.extend(matchers);

test('renders App component', () => {
render(<App />);
const header = screen.getByText('My Todolist');
excpect(header).toBeInTheDocument();
});
```

> **Note!** You can also use not keyword if you want to check that something doesn't exist, for example, expect(header).not.toBeInTheDocument().

Now, we can run our first test case by typing the following terminal command in the project folder:

```bash
npm run test
```

We can see that header text is found and the first test case is passed.

---

## Writing Test Cases with Vitest and React Testing Library

**React testing library** provides **fireEvent** method that you can use for firing DOM events like button click or input change events. Let's create a test case that adds one todo and then checks that todo can be found from our table. First, add a new test case to the **App.test.jsx** file:

```javascript
test('add todo', () => {
    render(<App />);
});
```

Add **fireEvent** method to the import:

```javascript
import { render, screen, fireEvent } from '@testing-library/react';
```

Next, we will use **getByPlaceholderText** query to find our input element and **fireEvent.change** method to change their values:

```javascript
test('add todo',() => {
    render(<App/>);

    const desc = screen.getByPlaceholderText('Description');
    fireEvent.change(desc, { target: { value: 'Go to coffee' } });
    const date = screen.getByPlaceholderText('Date');
    fireEvent.change(date, { target: { value: '29.12.2023' } });
    const status = screen.getByPlaceholderText('Status');
    fireEvent.change(status, { target: { value: 'Open' } });
})
```

After the input elements are populated with values, we find the button by using the **getByText** query and invoke button's click event by using the **fireEvent.click** method:

```javascript
test('add todo',() => {
render(<App/>);

const desc = screen.getByPlaceholderText('Description');
fireEvent.change(desc, { target: { value: 'Go to coffee' } });
const date = screen.getByPlaceholderText('Date');
fireEvent.change(date, { target: { value: '29.12.2023' } });
const status = screen.getByPlaceholderText('Status');
fireEvent.change(status, { target: { value: 'Open' } });
const button = screen.getByText('Add');
fireEvent.click(button);
})
```

Then, we find the table by using **getByRole** query and check that 'Go to coffee' text can be found from the table using the **toHaveTextContent** matcher:

```javascript
test('add todo',() => {
    render(<App/>);

    const desc = screen.getByPlaceholderText('Description');
    fireEvent.change(desc, { target: { value: 'Go to coffee' } });
    const date = screen.getByPlaceholderText('Date');
    fireEvent.change(date, { target: { value: '29.12.2023' } });
    const status = screen.getByPlaceholderText('Status');
    fireEvent.change(status, { target: { value: 'Open' } });
    const button = screen.getByText('Add');
    fireEvent.click(button);

    const table = screen.getByRole('table');
    expect(table).toHaveTextContent('Go to coffee');
})
```

Now, if you run the test cases, you should see the two passed test cases.
