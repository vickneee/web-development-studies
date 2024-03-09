# JWT (JSON Web Tokens) Authorization Worksheet

## Introduction

```javascript
import jwt from "jsonwebtoken";
import { createError } from "./error.js";

// Extract token from request
const extractToken = (req) => {
  const bearerToken = req.headers.authorization;
  const cookieToken = req.cookies.access_token;
  return bearerToken?.split(' ')[1] || cookieToken;
};

// VERIFY Token
export const verifyToken = (req, res, next) => {
  const token = extractToken(req);
  if (!token) {
    return next(createError(401, "You are not authenticated!"));
  }

  // Processing TOKEN with JWT SECRET KEY
  jwt.verify(token, process.env.JWT, (err, user) => {
    if (err) {
      console.error(err);
      return next(createError(403, "Token is not valid!"));
    }
    req.user = user;
    next();
  });
};

// VERIFY User
export const verifyUser = (req, res, next) => {
  verifyToken(req, res, (err) => {
    if (err) {
      return next(err);
    }
    if (req.user.id === req.params.id) {
      next();
    } else {
      return next(createError(403, "You are not authorized!"));
    }
  });
};

// VERIFY Admin
export const verifyAdmin = (req, res, next) => {
  verifyToken(req, res, (err) => {
    if (err) {
      return next(err);
    }
    if (req.user.isAdmin) {
      next();
    } else {
      return next(createError(403, "You are not authorized!"));
    }
  });
};
```

This code now checks both the `Authorization` header and the `access_token` cookie for the token. It also logs any errors that occur during token verification. The `verifyUser` and `verifyAdmin` functions now pass any errors from `verifyToken` to the next middleware, instead of calling `verifyToken` with a callback.
