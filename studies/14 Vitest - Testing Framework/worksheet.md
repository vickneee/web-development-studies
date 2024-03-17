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

To install other libraries, type the following terminal command in the project directory:

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

or 

```bash
npm install -D jsdom @testing-library/react @testing-library/jest-dom
```

To run tests, we should also add npm script to our project's **package.json** file

```json
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "test": "vitest run",
  "lint": "eslint src --ext js,jsx --report-unused-disable-directives --max-warnings 0",
  "preview": "vite preview"
},
```

Let's create a file **testSetup.js** in the project root with the following content

```javascript
import { afterEach } from 'vitest'
import { cleanup } from '@testing-library/react'
import '@testing-library/jest-dom/vitest'

afterEach(() => {
  cleanup()
})
```

Now, after each test, the function cleanup is performed that resets the **jsdom** that is simulating the browser.

After that, we have to add the following configuration to our **vite.config.js** file:

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
        setupFiles: './testSetup.js', 
    }
})
```

With **globals: true**, there is **no need** to import keywords such as **describe**, **test** and **expect** into the tests.

Now, you can run tests by typing the following terminal command in the project directory:

```bash
npm test
```

The command finds all test files (**.test.js** or **.test.jsx** extension) from your project and runs the test cases. By default, it will run in watch mode and re-run the tests each time when code is updated. You can stop watch mode by pressing 'q' in the terminal.

---

## Writing Test Cases with Vitest and React Testing Library


