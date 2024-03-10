# NodeCustomer MySQL Database with Node.js

**Example:**

This code sets up an Express server with endpoints to perform CRUD operations (Create, Read, Update, Delete) on a MySQL database table named customers. It uses an express application to handle HTTP requests and a mysql connection pool to interact with the MySQL database.

```javascript
// index.js
import express from 'express';
import mysql from 'mysql';

const app = express();
const port = 3000;

// Create a MySQL connection pool
const pool = mysql.createPool({
    connectionLimit: 10,
    host: 'localhost',
    user: 'test',
    password: 'testVictoria',
    database: 'customer'
});

// Middleware
app.use(express.json());

// CREATE a new customer
app.post("/api/customers", (req, res) => {
    const newCustomer = req.body;
    pool.query('INSERT INTO customers SET ?', newCustomer, (error, results) => {
        if (error) {
            console.error(error);
            res.status(500).json({ error: 'Internal Server Error' });
            return;
        }
        newCustomer.id = results.insertId;
        res.json(newCustomer);
    });
});

// GET all customers
app.get("/api/customers", (req, res) => {
    pool.query('SELECT * FROM customers', (error, results) => {
        if (error) {
            console.error(error);
            res.status(500).json({ error: 'Internal Server Error' });
            return;
        }
        res.json(results);
    });
});

// GET a customer by id
app.get("/api/customers/:id", (req, res) => {
    const customerId = req.params.id;
    pool.query('SELECT * FROM customers WHERE id = ?', customerId, (error, results) => {
        if (error) {
            console.error(error);
            res.status(500).json({ error: 'Internal Server Error' });
            return;
        }
        if (results.length > 0)
            res.json(results[0]);
        else
            res.status(404).end();
    });
});

// UPDATE a customer
app.put("/api/customers/:id", (req, res) => {
    const id = req.params.id;
    const updatedCustomer = req.body;
    pool.query('UPDATE customers SET ? WHERE id = ?', [updatedCustomer, id], (error) => {
        if (error) {
            console.error(error);
            res.status(500).json({ error: 'Internal Server Error' });
            return;
        }
        updatedCustomer.id = id;
        res.json(updatedCustomer);
    });
});

// DELETE a customer
app.delete("/api/customers/:id", (req, res) => {
    const id = req.params.id;
    pool.query('DELETE FROM customers WHERE id = ?', id, (error) => {
        if (error) {
            console.error(error);
            res.status(500).json({ error: 'Internal Server Error' });
            return;
        }
        res.status(204).end();
    });
});

app.listen(port, () => {
    console.log(`Server is running on port ${port}.`);
});
```

**Example 2:**

```javascript
// index.js
const express = require('express');
const bodyParser = require('body-parser');
const query = require('./db/customers');

const app = express();
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));

const port = 3000;

// Routes for REST API
app.get("/api/customers", query.getAllCustomers);
app.post("/api/customers",  query.addCustomer);
app.delete("/api/customers/:id",  query.deleteCustomer);
app.put("/api/customers/:id", query.updateCustomer);
app.get("/api/customers/:id", query.getCustomerById);

app.listen(port, () => {
    console.log(`Server is running on port ${port}.`);
});

module.exports = app;
```

```javascript
// customers.js
const db = require('./dbconfig');

// Get all customers
const getAllCustomers = (req, res) => {
  db.query('SELECT * FROM customers', (err, result) => {
    if (err)
      console.error(err);
    else
      res.json(result.rows)
  })
}

// Get customer by id
const getCustomerById = (req, res) => {
  const query = {
    text: 'SELECT * FROM customers WHERE id = $1',
    values: [req.params.id],
  }

  db.query(query, (err, result) => {
    if (err) {
      return console.error('Error executing query', err.stack)
    }
    else {
      if (result.rows.length > 0)
        res.json(result.rows);
      else
        res.status(404).end();
    }
  })
}

// Add new customer
const addCustomer = (req, res) => {
  // Extract customer from the request body
  const newcustomer = req.body;

  const query = {
    text: 'INSERT INTO customers (firstname, lastname, email, phone) VALUES ($1, $2, $3, $4)',
    values: [newcustomer.firstname, newcustomer.lastname, newcustomer.email, newcustomer.phone],
  }

  db.query(query, (err, res) => {
    if (err) {
      return console.error('Error executing query', err.stack)
    }
  })

  res.json(newcustomer);
}

// Delete customer
const deleteCustomer = (req, res) => {
  const query = {
    text: 'DELETE FROM customers WHERE id = $1',
    values: [req.params.id],
  }

  db.query(query, (err, res) => {
    if (err) {
      return console.error('Error executing query', err.stack)
    }
  })

  res.status(204).end();
}

// Delete all customers
const deleteAllCustomers = () => {
  db.query('DELETE FROM customers', (err, res) => {
    if (err) {
      return console.error('Error executing query', err.stack)
    }
  })
}

const updateCustomer = (req, res) => {
  // Extract edited customer from the request body
  const editedcustomer = req.body;

  const query = {
    text: 'UPDATE customers SET firstname=$1, lastname=$2, email=$3, phone=$4 WHERE id = $5',
    values: [editedcustomer.firstname, editedcustomer.lastname, editedcustomer.email, editedcustomer.phone, req.params.id],
  }

  db.query(query, (err, res) => {
    if (err) {
      return console.error('Error executing query', err.stack)
    }
  })

  res.json(editedcustomer);
}

module.exports = {
  getAllCustomers: getAllCustomers,
  getCustomerById: getCustomerById,
  addCustomer: addCustomer,
  deleteCustomer: deleteCustomer,
  updateCustomer: updateCustomer,
  deleteAllCustomers: deleteAllCustomers,
}
```

```javascript
// dbconfig.js
const mysql = require('mysql');

// Create a connection pool
const db = mysql.createPool({
    connectionLimit: 10,
    host: 'localhost',
    user: 'test',
    password: 'testVictoria',
    database: 'customer',
});

module.exports = db;
```



