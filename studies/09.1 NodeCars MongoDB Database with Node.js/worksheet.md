# NodeCars MongoDB Database with Node.js

To create an Express.js project with MongoDB integration, you'll typically need several files and directories. Here's a basic project structure along with the files you'll commonly have:

**project-root/:** You can name the project directory anything you like (nodecars-mongodb).

**controllers/:** This directory contains controller files responsible for handling the business logic of your application. Controllers interact with models to perform CRUD operations and handle requests and responses.

**models/:** This directory contains Mongoose models that define the structure of your MongoDB documents. Each model file corresponds to a collection in your MongoDB database.

**routes/:** This directory contains route handlers for different parts of your API. Each route handler file will handle specific API routes and their corresponding logic.

**.env:** This file holds environment variables that your application needs. It should include sensitive information like database connection strings or API keys.

**index.js:** This is your main application file where you initialize and configure your Express app, set up routes, and start the server.

**package.json:** This file holds metadata relevant to the project and manages project dependencies. You can create it by running npm init in your project directory.

## Here's a basic structure for your project:

```shell
nodecars-mongodb/
├── controllers/
│   └── CarController.js
├── models/
│   └── Car.js
├──  routes/
│   └── CarRoutes.js
├── .env
├── index.js
└── package.json

```

Now, let's create these files:

## package.json:

You can create this file by running npm init in your project directory (nodecars-mongodb).

```shell
npm init
```

## .env:

Add port and your MongoDB URL.

```makefile
PORT=3000
MONGO_URL=<your-mongodb-connection-url>

MONGO_URL_EXAMPLE=mongodb+srv://CarDB:CarDB@cluster0.4nkj1va.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0
```

## Custom database name

You can consider it a custom database name because it's the name you specify for your MongoDB database. This name can be anything you choose, and it's typically reflective of the purpose or content of the database.

If you're using custom database name in MongoDB Atlas, the URI might look something like this (**Note!** **your_database_name_here**):

```
MONGO_URL=mongodb+srv://username:password@clustername.mongodb.net/your_database_name_here?retryWrites=true&w=majority
```

Example:

```
MONGO_URL=mongodb+srv://CarDB:CarDB@cluster0.4nkj1va.mongodb.net/carDB?retryWrites=true&w=majority&appName=Cluster0
```

## index.js:

```javascript
// index.js
import express from 'express';
import mongoose from 'mongoose';
import dotenv from 'dotenv';

import CarRoutes from './routes/CarRoutes.js'; // Import car routes

dotenv.config(); // Load environment variables

const app = express();

// Middleware
app.use(express.json());

// Routes
app.use('/', CarRoutes); // Car routes

// MongoDB connection
mongoose.connect(process.env.MONGO_URL)
    .then(() => console.log('Connected to MongoDB'))
    .catch(err => console.error('Failed to connect to MongoDB', err));

// Start the server
const port = process.env.PORT || 3000;
    app.listen(port, () => console.log(`Server running on port ${port}`));
```

## controllers/carsController.js

```javascript
import Car from '../models/Car.js';

const CarsController = {
    // Get all cars
    getAllCars: async (req, res) => {
        try {
            const cars = await Car.find();
            res.json(cars);
        } catch (error) {
            res.status(500).json({ error: error.message });
        }
    },

    // Get a car by ID
    getCarById: async (req, res) => {
        try {
            const car = await Car.findById(req.params.id);
            if (!car) {
                return res.status(404).json({ message: "Car not found" });
            }
            res.json(car);
        } catch (error) {
            res.status(500).json({ error: error.message });
        }
    },

    // Create a new car
    createCar: async (req, res) => {
        try {
            const car = new Car(req.body);
            const newCar = await car.save();
            res.status(201).json(newCar);
        } catch (error) {
            res.status(400).json({ error: error.message });
        }
    },

    // Update a car by ID
    updateCarById: async (req, res) => {
        try {
            const car = await Car.findByIdAndUpdate(req.params.id, req.body, { new: true });
            if (!car) {
                return res.status(404).json({ message: "Car not found" });
            }
            res.json(car);
        } catch (error) {
            res.status(400).json({ error: error.message });
        }
    },

    // Delete a car by ID
    deleteCarById: async (req, res) => {
        try {
            const car = await Car.findByIdAndDelete(req.params.id);
            if (!car) {
                return res.status(404).json({ message: "Car not found" });
            }
            res.json({ message: "Car deleted" });
        } catch (error) {
            res.status(500).json({ error: error.message });
        }
    }
};

export default CarsController;
```

## models/Car.js:

```javascript
import mongoose from 'mongoose';

const CarSchema = new mongoose.Schema({
    brand: { 
        type: String, 
        required: true },
    model: { 
        type: String, 
        required: true },
    color: { 
        type: String, 
        required: true },
    year: { 
        type: Number, 
        required: true }
});

const Car = mongoose.model('Car', CarSchema);

export default Car;
```

## routes/carRoutes.js:

```javascript
import express from 'express';
import CarsController from '../controllers/CarsController.js';

const router = express.Router();

// Create a new car
router.post('/cars', CarsController.createCar);

// Get all cars
router.get('/cars', CarsController.getAllCars);

// Get a car by ID
router.get('/cars/:id', CarsController.getCarById);

// Update a car by ID
router.put('/cars/:id', CarsController.updateCarById);

// Delete a car by ID
router.delete('/cars/:id', CarsController.deleteCarById);

export default router;
```
