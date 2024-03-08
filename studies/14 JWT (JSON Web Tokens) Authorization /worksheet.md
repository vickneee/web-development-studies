# JWT (JSON Web Tokens) Authorization Worksheet and Study Guide

JWT (JSON Web Tokens) are an open, industry standard method for representing claims securely between two parties. They are commonly used for authorization. Once a user is logged in, each subsequent request will include the JWT, allowing the user to access routes, services, and resources that are permitted with that token.

Here's a basic example of how you can use JWT in a Node.js application with Express and jsonwebtoken library:

1. **Install jsonwebtoken library**: You can install it using npm:

```bash
npm install jsonwebtoken
```

2. **Import jsonwebtoken in your file**:

```javascript
import jwt from 'jsonwebtoken';
```

3. **Generate a Token**: After the user logs in with the correct credentials, you can generate a token for that user:

```javascript
const user = { id: 3 }; // This would be fetched from your database in a real scenario
const token = jwt.sign(user, 'your-unique-secret-key', { expiresIn: '1h' });
```

In this code, `jwt.sign()` is used to generate the token. The first argument is the payload that you want to include in the token. The second argument is a secret key that you should keep safe, as it is used to sign the tokens. The third argument is an options object where you can set the token's expiration time.

4. **Send the Token to the Client**: You can send the generated token to the client in the response:

```javascript
res.json({ token });
```

5. **Verify the Token**: When the client sends a request with a token, you can verify the token using `jwt.verify()`:

```javascript
const authMiddleware = (req, res, next) => {
  const token = req.headers['authorization'];

  if (!token) {
    return res.status(403).json({ error: 'No token provided' });
  }

  jwt.verify(token, 'your-unique-secret-key', (err, user) => {
    if (err) {
      return res.status(500).json({ error: 'Failed to authenticate token' });
    }

    req.user = user;
    next();
  });
};
```

In this code, `jwt.verify()` is used to verify the token. The first argument is the token. The second argument is the same secret key that you used to sign the tokens. The third argument is a callback function that will be called once the token is verified. If the token is valid, the callback function will receive the decoded payload as its second argument, and you can assign it to `req.user` to make it available to your route handlers.

6. **Use the Middleware**: You can use the `authMiddleware` to protect your routes:

```javascript
router.get('/protected', authMiddleware, (req, res) => {
  res.json({ message: 'This is a protected route' });
});
```

In this code, `authMiddleware` is used as a middleware for the `/protected` route. This means that the `authMiddleware` function will be called before the route handler function. If the token is valid, `req.user` will be available in your route handler function.
