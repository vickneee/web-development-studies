# Metropolia UAS React.js Learning Diary

## Date: 17.03.2024

### Main Points of the Lecture:
The main points of the lectures were to learn how to use React.js framework. The lectures covered the basics of React.js, such as components, props, state and networking (Fetch API). Additionally, the lectures covered the use of React Router, testing with Vitest, and deployment.

### Prior Knowledge:
During my school programming classes, I was recently actively working on a MERN full-stack project. I had prior knowledge of React.js, React Router, but I had never used Vitest before.

### New Learning:
I learned about React.js code testing for the first time, and I feel like I need to understand it better. I want to learn also an End-to-End testing (E2E) with Playwright (and also Cypress).

### Specific Interesting Points:
I found the React.js code testing with Vitest particularly interesting and feel I need to learn about that more. (Also E2E testing with Playwright and Cypress.)

### Questions Arising:
I faced the testing part of the course some issues. But I solved the issues with the help of Helsinki University's Full Stack Open course. Example: With globals: true, there is no need to import keywords such as describing, test and expect into the tests. It worked for me.

Then in the deployment part, I faced some issues with the GitHub Pages deployment. I solved the issue by editing the `vite.config.js` file. I added the `base` property to the `vite.config.js` file as Vite Guide suggested.

[Vite Guide GitHub Pages](https://vitejs.dev/guide/static-deploy.html)

```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  base: '/firebase-bookstore-app/',
  plugins: [react()],
})
```

Then I edited Edit static.yml file as Vite Guide suggested:

```yaml
# Simple workflow for deploying static content to GitHub Pages
name: Deploy static content to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ['main']

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets the GITHUB_TOKEN permissions to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: 'pages'
  cancel-in-progress: true

jobs:
  # Single deploy job since we're just deploying
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Set up Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
      - name: Install dependencies
        run: npm ci
      - name: Build
        run: npm run build
      - name: Setup Pages
        uses: actions/configure-pages@v4
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          # Upload dist folder
          path: './dist'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

My Bookstore app is now deployed to GitHub Pages: [Firebase Bookstore App](https://vickneee.github.io/firebase-bookstore-app/)

### Desired Further Understanding:
I'd like to learn and understand more about code testing with Vitest and End-to-End testing (E2E) with Playwright (and also Cypress). I started Full Stack open course of Helsinki University. I also want to learn more about the design of the React.js components with the Tailwind CSS framework. I see that it is a beneficial framework for the design of the components, and it is straightforward to use and very widely used.

### Meaning and Future Use:
I will definitely use React.js Framework and Vitest testing (also E2E tests) in my future as a developer.

## Conclusion:
These lectures were very interesting to learn and practice. There were many chances to practice, which I liked a lot, and I learned many new things. Overall, that course was very beneficial. I will continue to learn more about React.js and its testing on Full Stack Open course of Helsinki University.
