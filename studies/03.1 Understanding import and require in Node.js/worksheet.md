# Understanding `import` and `require` in Node.js

When working with modules in Node.js, you have two options for importing modules: `import` and `require`. The choice between them depends on the version of Node.js you are using and the type of modules you are working with.

## CommonJS (`require`)
CommonJS is the module system used in Node.js before version 13. It uses `require` to import modules. You should use CommonJS syntax in Node.js versions before 13 or when working with packages that haven't been updated to use ES Modules.

Example usage:
```javascript
const express = require('express');
const dotenv = require('dotenv');
const mongoose = require('mongoose');
const cookieParser = require('cookie-parser');
const routes = require('./routes/routes.js');
```

## ES Modules (`import/export`)
ES Modules are the standardized module system in JavaScript. They use `import` and `export` to import and export modules. To use ES Modules in Node.js, you need to either name your file with a `.mjs` extension or set `"type": "module"` in your `package.json` file. ES Modules are supported in Node.js versions 13 and later.

Example usage:
```javascript
import express from 'express';
import dotenv from 'dotenv';
import mongoose from 'mongoose';
import cookieParser from 'cookie-parser';
import routes from './routes/routes.js';
```

If you're unsure which one to use, consider the following:
- If you're working with a project that uses a newer version of Node.js (13 or later) and supports ES Modules, you can use `import/export`.
- If you're working with an older version of Node.js or with packages that haven't migrated to ES Modules, you'll need to use `require`.

In summary, choose `import/export` for newer Node.js projects or those that support ES Modules, and use `require` for older Node.js versions or packages that require CommonJS syntax.
