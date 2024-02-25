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
        container: {  // Optional Example
            padding: {  // Optional Example
                md: "10rem",  // Optional Example
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
    plugins: {
        tailwindcss: {},
        autoprefixer: {},
    }
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

This snippet uses the @import rule to import the Tailwind CSS stylesheets directly.

5. **Check you have imported index.css** file to your **main.jsx** or **main.tsx** file  

```javascript
// main.jsx or main.tsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";
import "./index.css";  // Check you have imported "./index.css" file

ReactDOM.createRoot(document.getElementById("root")!).render(
    <React.StrictMode>
        <App />
    </React.StrictMode>
);
```

6. **Start the Tailwind CLI build process:** 

Run the CLI tool to scan your template files for classes and build your CSS. (main css file: example index.css)

```terminal
npx tailwindcss -i ./src/index.css -o ./src/output.css --watch
```

8. **Include the compiled CSS in your HTML:** Link the compiled CSS file (**output.css**) in your HTML file:

```html
<link rel="stylesheet" href="./src/output.css">
```

9. **Start using Tailwind classes:** Now you can start using Tailwind's utility classes directly in your HTML or in your custom CSS.

That's it! You're now set up to use Tailwind CSS in your project. Feel free to customize the configuration and explore Tailwind's extensive utility classes to style your project efficiently.

[Tailwind CSS Components](https://tailwindcss.com/docs/installation)

[Tailwind CSS WebStorm](https://www.jetbrains.com/help/webstorm/tailwind-css.html)
