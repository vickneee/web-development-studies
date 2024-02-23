# Error Handling in Express with Short Error Messages

Error handling is an essential part of building robust web applications. In an Express application, proper error handling ensures that errors are caught and handled gracefully, providing meaningful responses to clients.

## Overview

This guide demonstrates how to implement error handling in an Express application using short error messages. Short error messages are concise and provide essential information about the encountered error.

## 1. Create Short Error Messages Utility

```javascript
// utils/error.js

export const createShortError = (status, message) => {
    const err = new Error(message);
    err.status = status;
    return err;
};
```

## 2. Use Short Error Messages in Controller Functions

```javascript
// controllers/userController.js

import { createShortError } from "../utils/error.js";

// UPDATE a User
export const updateUser = async (req, res, next) => {
    try {
        const updatedUser = await User.findByIdAndUpdate(
            req.params.id,
            { $set: req.body },
            { new: true }
);

        // Check if no user was found with the provided ID
        if (!updatedUser) {
            return next(createShortError(404, "User not found"));
        }

        res.status(200).json(updatedUser);
    } catch (err) {
        // Handle other errors
        next(err);
    }
};
```

3. Add Error Handling Middleware

```javascript
// utils/errorMiddleware.js

const errorMiddleware = (err, req, res, next) => {
    const status = err.status || 500;
    const message = err.message || "Internal Server Error";
    res.status(status).json({ error: message });
};

export default errorMiddleware;
```

## 4. Integrate Error Middleware with Express App

```javascript
// server.js

import express from 'express';
import errorMiddleware from './utils/errorMiddleware.js'; // Import path

const app = express();

// Define routes and other middlewares...
app.use("/api/auth", authRoute);
app.use("/api/users", usersRoute);
app.use("/api/hotels", hotelsRoute);
app.use("/api/rooms", roomsRoute);

// Add error middleware
app.use(errorMiddleware);

// Start the server...
```

By following these steps, you can implement error handling with short error messages in your Express application. Short error messages provide concise information about errors, improving the overall user experience.

The error messages, including their content and format, are typically defined by developers based on the requirements and context of their applications.

## Here's a list of commonly used error messages in web applications:

1. **404 Not Found:** Indicates that the requested resource could not be found on the server.
2. **400 Bad Request:** Indicates that the server cannot process the request due to a client error (e.g., malformed request syntax).
3. **401 Unauthorized:** Indicates that the request requires authentication, but the client is not authenticated.
4. **403 Forbidden:** Indicates that the server understood the request, but refuses to authorize it.
5. **500 Internal Server Error:** Indicates that the server encountered an unexpected condition that prevented it from fulfilling the request.
6. **502 Bad Gateway:** Indicates that the server received an invalid response from an upstream server while acting as a gateway or proxy.
7. **503 Service Unavailable:** Indicates that the server is currently unable to handle the request due to temporary overload or maintenance.
8. **504 Gateway Timeout:** Indicates that the server did not receive a timely response from an upstream server while acting as a gateway or proxy.

Developers can also create custom error messages tailored to specific application requirements. These messages should provide relevant information to users or clients about the encountered error while avoiding revealing sensitive information that attackers could exploit.
