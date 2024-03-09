# JSON Web Token Example

```javascript
const express = require('express');
const bodyParser = require('body-parser');
const query = require('./db/customers');
const auth = require('./services/authenticate');

const app = express();
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));

const port = 3000;

process.env.SECRET_KEY = "5b1a3923cc1e1e19523fd5c3f20b409509d3ff9d42710a4da095a2ce285b009f0c3730cd9b8e1af3eb84d";

// Routes for REST API
app.get("/api/customers", auth.authenticate, query.getAllCustomers);
app.post("/api/customers", auth.authenticate, query.addCustomer);
app.delete("/api/customers/:id", auth.authenticate, query.deleteCustomer);
app.put("/api/customers/:id", auth.authenticate, query.updateCustomer);
// Route for authentication
app.post("/login", auth.login);

app.listen(port, () => {
  console.log(`Server is running on port ${port}.`);
});

module.exports = app;
```

```javascript
// services/Authenticate.js
const jwt = require('jsonwebtoken');
const user = require('../db/users');
const bcrypt = require('bcrypt');


// User login
const login = (req, res) => {
  const email = req.body.email;
  const password = req.body.password;

  const loginUser = user.getUserByEmail(email, (user) => {
    if (user.length > 0) {
      const hash = user[0].password;
      const token = jwt.sign({userId: email}, process.env.SECRET_KEY);

      if (bcrypt.compareSync(password, hash))
        res.send({token});
      else
        res.sendStatus(400).end();
    }
    else {
      res.sendStatus(400).end();
    }
  });
}

// User authentication
const authenticate = (req, res, next) => {
  const token = req.header('Authorization');
  if(!token) {
    res.sendStatus(400).end();
  }
  
  jwt.verify(token, process.env.SECRET_KEY, (err, decoded) => {
    if (err)
      res.sendStatus(400).end();
    else
      next();
  });
} 

module.exports = {
  authenticate: authenticate,
  login: login
}
```

