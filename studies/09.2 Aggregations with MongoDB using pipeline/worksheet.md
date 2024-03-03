# Aggregations with MongoDB using Pipeline

## Performing Aggregations with MongoDB using Pipeline

To perform aggregations with MongoDB using the aggregation pipeline, follow these steps:

1. **Connect to MongoDB**: Establish a connection to your MongoDB database from your application using the appropriate MongoDB client.

2. **Define Aggregation Pipeline**: Define the aggregation pipeline, which consists of multiple stages such as `$match`, `$group`, `$project`, etc., each performing specific operations on the data.

3. **Execute Aggregation**: Use the `aggregate()` method provided by the MongoDB client to execute the aggregation pipeline on the desired collection.

4. **Handle Results**: Handle the results of the aggregation operation returned by MongoDB. You can then process or display the aggregated data as needed.

Here's an example of a basic aggregation pipeline in MongoDB:

```javascript
db.collection.aggregate([
    { $match: { field: { $gte: 100 } } },
    { $group: { _id: "$category", total: { $sum: "$value" } } },
    { $sort: { total: -1 } }
]);
```
This pipeline filters documents based on a condition, groups them by a certain field, calculates the total value for each group, and then sorts the results in descending order of total value.
    
## General example: (not tested)

```javascript
const express = require('express');
const { MongoClient } = require('mongodb');

const app = express();
const port = 3000;

// Connection URI
const uri = 'mongodb://localhost:27017';

// Database Name
const dbName = 'mydatabase';

// Create a new MongoClient
const client = new MongoClient(uri, { useNewUrlParser: true, useUnifiedTopology: true });

// Connect to the MongoDB server
client.connect(async err => {
if (err) {
console.error('Error occurred while connecting to MongoDB', err);
return;
}

console.log('Connected to MongoDB server');

const db = client.db(dbName);

// Define the aggregation pipeline
const pipeline = [
// Add your aggregation stages here
// Example: { $group: { _id: "$field", total: { $sum: 1 } } }
];

// Define a route to perform aggregation
app.get('/aggregate', async (req, res) => {
try {
// Execute the aggregation pipeline
const result = await db.collection('mycollection').aggregate(pipeline).toArray();

      // Send the aggregation result as JSON
      res.json(result);
    } catch (err) {
      console.error('Error occurred while aggregating data', err);
      res.status(500).send('Error occurred while aggregating data');
    }
});
});

// Start the server
app.listen(port, () => {
console.log(`Server is running on http://localhost:${port}`);
});
```

## Update a year: (not tested)

If you want to update documents based on the results of an aggregation, you can use the aggregation pipeline to compute the values you want to update with, and then use the results to perform updates. Here's how you can do it:

```javascript

const express = require('express');
const { MongoClient } = require('mongodb');

const app = express();
const port = 3000;

// Connection URI
const uri = 'mongodb://localhost:27017';

// Database Name
const dbName = 'mydatabase';

// Create a new MongoClient
const client = new MongoClient(uri, { useNewUrlParser: true, useUnifiedTopology: true });

// Connect to the MongoDB server
client.connect(async err => {
  if (err) {
    console.error('Error occurred while connecting to MongoDB', err);
    return;
  }

  console.log('Connected to MongoDB server');

  const db = client.db(dbName);

  // Define the aggregation pipeline
  const pipeline = [
    // Add your aggregation stages here
    // Example: { $group: { _id: "$field", total: { $sum: 1 } } }
    // Example 2: { $group: { _id: null, maxYear: { $max: "$year" } } }
  ];

  // Define a route to perform aggregation and update
  app.get('/updateYear', async (req, res) => {
    try {
      // Execute the aggregation pipeline
      const result = await db.collection('mycollection').aggregate(pipeline).toArray();

      // If there are results, update documents based on the aggregation result
      if (result.length > 0) {
        const maxYear = result[0].maxYear; // Assuming the aggregation pipeline returns the maximum year

        // Update documents where the year equals a certain value (example)
        const updateResult = await db.collection('mycollection').updateMany(
          { year: 2020 }, // Example condition
          { $set: { year: maxYear } } // Set the year to the maxYear value
        );

        res.json(updateResult);
      } else {
        res.status(404).send('No aggregation result found');
      }
    } catch (err) {
      console.error('Error occurred while updating year', err);
      res.status(500).send('Error occurred while updating year');
    }
  });
});

// Start the server
app.listen(port, () => {
  console.log(`Server is running on http://localhost:${port}`);
});
```

```javascript
const express = require('express');
const { MongoClient } = require('mongodb');

const app = express();
const port = 3000;

// Connection URI
const uri = 'mongodb://localhost:27017';

// Database Name
const dbName = 'mydatabase';

// Create a new MongoClient
const client = new MongoClient(uri, { useNewUrlParser: true, useUnifiedTopology: true });

// Connect to the MongoDB server
client.connect(async err => {
  if (err) {
    console.error('Error occurred while connecting to MongoDB', err);
    return;
  }

  console.log('Connected to MongoDB server');

  const db = client.db(dbName);

  // Define the aggregation pipeline
  const pipeline = [
    // Add your aggregation stages here
    // Example: { $group: { _id: "$field", total: { $sum: 1 } } }
  ];

  // Define a route to perform aggregation and update
  app.get('/updateYear', async (req, res) => {
    try {
      // Execute the aggregation pipeline
      const result = await db.collection('mycollection').aggregate(pipeline).toArray();

      // If there are results, update documents based on the aggregation result
      if (result.length > 0) {
        const newYear = 2022; // Set your new year value here

        // Update documents with the new year value
        const updateResult = await db.collection('mycollection').updateMany(
          {}, // Update all documents in the collection
          { $set: { year: newYear } } // Set the year to the newYear value
        );

        res.json(updateResult);
      } else {
        res.status(404).send('No aggregation result found');
      }
    } catch (err) {
      console.error('Error occurred while updating year', err);
      res.status(500).send('Error occurred while updating year');
    }
  });
});

// Start the server
app.listen(port, () => {
  console.log(`Server is running on http://localhost:${port}`);
});
```

```javascript
const express = require('express');
const { MongoClient } = require('mongodb');

const app = express();
const port = 3000;

// Connection URI
const uri = 'mongodb://localhost:27017';

// Database Name
const dbName = 'mydatabase';

// Create a new MongoClient
const client = new MongoClient(uri, { useNewUrlParser: true, useUnifiedTopology: true });

// Connect to the MongoDB server
client.connect(async err => {
  if (err) {
    console.error('Error occurred while connecting to MongoDB', err);
    return;
  }

  console.log('Connected to MongoDB server');

  const db = client.db(dbName);

  // Define a route to insert a new year
  app.post('/insertYear', async (req, res) => {
    try {
      const newYear = req.body.year; // Assuming you're sending the new year value in the request body

      // Insert the new year value into the collection
      const insertResult = await db.collection('mycollection').insertOne({ year: newYear });

      res.json(insertResult);
    } catch (err) {
      console.error('Error occurred while inserting year', err);
      res.status(500).send('Error occurred while inserting year');
    }
  });
});

// Start the server
app.listen(port, () => {
  console.log(`Server is running on http://localhost:${port}`);
});

```
