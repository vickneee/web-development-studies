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

