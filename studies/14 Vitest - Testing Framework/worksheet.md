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

Now, you can run tests by typing the following terminal command in the project directory:

```bash
npm run test
```

The command finds all test files (**.test.js** or **.test.jsx** extension) from your project and runs the test cases. By default, it will run in watch mode and re-run the tests each time when code is updated. You can stop watch mode by pressing 'q' in the terminal.

---

## Writing Test Cases with Vitest and React Testing Library


