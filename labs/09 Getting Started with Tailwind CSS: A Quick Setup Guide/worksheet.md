# Getting Started with Tailwind CSS: A Quick Setup Guide

1. **Install Tailwind CSS:** First, install Tailwind CSS and its dependencies using npm:

```
npm install tailwindcss postcss autoprefixer
```

2. **Create a Tailwind CSS configuration file:** Generate a **tailwind.config.js** file by running:

```csharp
npx tailwindcss init -p
```

```javascript 
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
export default {
    content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
    theme: {
        extend: {},
        container: {
            padding: {
                md: "10rem",
            },
        },
    },
    plugins: [],
};
```

This file allows you to customize Tailwind's default configuration if needed.

3. **Set up PostCSS:** Create a **postcss.config.js** file in your project's root directory:

```javascript
export default {
    plugins: [
        require('tailwindcss'),
        require('autoprefixer'),
        // Add any other PostCSS plugins here
    ]
}
```

4. **Include Tailwind in your CSS:** Create your main CSS file (e.g., **index.css**) (You can delete App.css) and include Tailwind's base, components, and utilities:

```css
/* Import Tailwind CSS */
@import 'tailwindcss/base';
@import 'tailwindcss/components';
@import 'tailwindcss/utilities';

/* Your custom styles go here */
```

The first snippet uses the @import rule to import the Tailwind CSS stylesheets directly.

or 

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Your custom styles go here */
```
the second snippet uses the @tailwind directive.

The @tailwind directive is a special directive provided by Tailwind CSS that is processed by a tool like PostCSS, which replaces it with the appropriate @import statements during compilation. It's a more concise and expressive way of including Tailwind's styles, especially in environments where configuration is handled automatically (e.g., with tools like postcss-import).

In summary, both snippets accomplish the same task, but the second snippet with @tailwind directives may be preferred in environments where it's supported and provides a cleaner syntax.

5. **Check you have imported index.css** file to your **main.jsx** or **main.tsx** file  

```javascript
// main.jsx or main.tsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.tsx";
import "./index.css";  // Check you have imported "./index.css" file

ReactDOM.createRoot(document.getElementById("root")!).render(
    <React.StrictMode>
        <App />
    </React.StrictMode>
);
```

6. **Start the Tailwind CLI build process:** 

Run the CLI tool to scan your template files for classes and build your CSS.

```terminal
npx tailwindcss -i ./src/input.css -o ./src/output.css --watch
```

or 

7. **Build your CSS:** Add a build script in your **package.json** file to compile your CSS:

```json
"scripts": {
"build:css": "postcss styles.css -o output.css"
}
```

Then run:

```arduino
npm run build:css
```

This command will compile your **styles.css** file and output the compiled CSS to **output.css**.

8. **Include the compiled CSS in your HTML:** Link the compiled CSS file (**output.css**) in your HTML file:

```html
<link rel="stylesheet" href="./output.css">
```

9. **Start using Tailwind classes:** Now you can start using Tailwind's utility classes directly in your HTML or in your custom CSS.

That's it! You're now set up to use Tailwind CSS in your project. Feel free to customize the configuration and explore Tailwind's extensive utility classes to style your project efficiently.

[Tailwind CSS Components](https://tailwindcss.com/docs/installation)

[Tailwind CSS WebStorm](https://www.jetbrains.com/help/webstorm/tailwind-css.html)
