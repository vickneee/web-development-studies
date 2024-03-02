# Using MongoDB and Mongoose with Node.js

To create an Express.js project with MongoDB integration, you'll typically need several files and directories. Here's a basic project structure along with the files you'll commonly have:

**project-directory/** You can name the project directory anything you like.

**package.json:** This file holds metadata relevant to the project and manages project dependencies. You can create it by running npm init in your project directory.

**index.js:** This is your main application file where you initialize and configure your Express app, set up routes, and start the server.

**routes/:** This directory contains route handlers for different parts of your API. Each route handler file will handle specific API routes and their corresponding logic.

**models/:** This directory contains Mongoose models that define the structure of your MongoDB documents. Each model file corresponds to a collection in your MongoDB database.

**.env:** This file holds environment variables that your application needs. It should include sensitive information like database connection strings or API keys.

## Here's a basic structure for your project:

```bash
Copy code
project-root/
│
├── index.js
├── package.json
├── .env
├── routes/
│   └── movieRoutes.js
│
└── models/
└── Movie.js
```

Now, let's create these files:

## index.js:

```javascript
import express from 'express';
import mongoose from 'mongoose';
import dotenv from 'dotenv';
import movieRouter from './routes/movieRoutes.js';

const app = express();
dotenv.config();

// Middleware
app.use(express.json());

// Routes
app.use('/api/movies', movieRouter);

// MongoDB connection
mongoose.connect(process.env.MONGO_URL)
    .then(() => console.log('Connected to MongoDB'))
    .catch(err => console.error('Failed to connect to MongoDB', err));

// Start the server
const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`Server running on port ${port}`));
```

## package.json: 

You can create this file by running npm init in your project directory.

```shell
npm init
```

## .env:

Add port and your MongoDB URL. 

```makefile
PORT=3000
MONGO_URL=<your-mongodb-connection-url>

MONGO_URL_EXAMPLE=mongodb+srv://MovieDB:MovieDB@cluster0.4nkj1va.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0
````
## routes/movieRoutes.js:

```javascript
import express from 'express';
import Movie from '../models/Movie.js';

const router = express.Router();

// Define routes
router.get('/', async (req, res) => {
    try {
        const movies = await Movie.find();
        res.json(movies);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

export default router;
```

## models/Movie.js:

```javascript
import mongoose from 'mongoose';

const movieSchema = new mongoose.Schema({
title: { type: String, required: true },
director: String,
year: Number
});

const Movie = mongoose.model('Movie', movieSchema);

export default Movie;
```

Ensure you have MongoDB installed and running locally or replace MONGO_URL in the .env file with your MongoDB Atlas connection URL.


## Example of Blog Collections

The blog collections consist of three main entities: users, posts, and comments. Users store information about registered users, posts contain details of blog posts authored by users, and comments capture user-generated feedback on blog posts.

1. **users**
    - Stores information about users registered on the platform.
    - Example Fields:
        - `username`: (String) The unique username of the user.
        - `email`: (String) The email address of the user.
        - `password`: (String) The hashed password of the user.
        - `createdAt`: (Date) The date when the user account was created.

2. **posts**
    - Stores information about blog posts created by users.
    - Example Fields:
        - `title`: (String) The title of the blog post.
        - `content`: (String) The content of the blog post.
        - `author`: (ObjectId) The ID of the user who created the post.
        - `createdAt`: (Date) The date when the post was created.
        - `updatedAt`: (Date) The date when the post was last updated.

3. **comments**
    - Stores user made comments on blog posts.
    - Example Fields:
        - `content`: (String) The content of the comment.
        - `post_id`: (ObjectId) The ID of the post the comment belongs to.
        - `author`: (ObjectId) The ID of the user who made the comment.
        - `createdAt`: (Date) The date when the comment was created.
        - `updatedAt`: (Date) The date when the comment was last updated.


## Example of Booking Collections

The booking collections cater to a hotel booking platform, comprising four essential entities: users, hotels, rooms, and bookings. Users contain details of platform users, hotels store information about available accommodations, rooms represent individual rooms within hotels, and bookings record user reservations.

1. **users**
    - Stores information about users registered on the booking platform.
    - Example Fields:
        - `username`: (String) The unique username of the user.
        - `email`: (String) The unique email address of the user.
        - `password`: (String) The hashed password of the user.

```javascript
import mongoose from "mongoose";
const { Schema } = mongoose;

const UserSchema = new Schema(
    {
        // username: (String) // String is shorthand for {type: String}
        username: {
            type: String,
            required: true,
            unique: true,
        },
        email: {
            type: String,
            required: true,
            unique: true,
        },
        country: {
            type: String,
            required: true,
        },
        img: {
            type: String,
        },
        city: {
            type: String,
            required: true,
        },
        phone: {
            type: String,
            required: true,
        },
        password: {
            type: String,
            required: true,
        },
        isAdmin: {
            type: Boolean,
            default: false,
        },
    },
    {timestamps: true}
);

const User = mongoose.model("User", UserSchema);

export default User;
```

2. **hotels**
    - Stores information about hotels available for booking.
    - Example Fields:
        - `name`: (String) The name of the hotel.
        - `type`: (String) The type of the accommodation.
        - `location`: (String) The location of the hotel.
        - `description`: (String) A description of the hotel.
        - `rating`: (Number) The rating of the hotel (e.g., out of 5 stars).
        - `amenities`: (Array of Strings) The amenities offered by the hotel.
        - `featured`: (Boolean) Indicates whether the hotel is featured or not (true/false).

```javascript
import mongoose from "mongoose";
const { Schema } = mongoose;

const HotelSchema = new Schema(
    {
        // name: (String) // String is shorthand for {type: String}
        name: {
            type: String,
            required: true,
        },
        type: {
            type: String,
            required: true,
        },
        city: {
            type: String,
            required: true,
        },
        address: {
            type: String,
            required: true,
        },
        distance: {
            type: String,
            required: true,
        },
        photos: {
            type: [String], // Array of Strings
        },
        title: {
            type: String,
            required: true,
        },
        desc: {
            type: String,
            required: true,
        },
        rating: {
            type: Number,
            min: 0,
            max: 5,
        },
        rooms: {
            type: [String], // Array of Strings
        },
        cheapestPrice: {
            type: Number,
            required: true,
        },
        featured: {
            type: Boolean,
            default: false,
        },
    });

const Hotel = mongoose.model("Hotel", HotelSchema);

export default Hotel;
```

3. **rooms**
    - Stores information about rooms available within hotels.
    - Example Fields:
        - `hotel_id`: (ObjectId) The ID of the hotel to which the room belongs.
        - `type`: (String) The type of room (e.g., single, double, suite).
        - `price`: (Number) The price per night for the room.
        - `availability`: (Boolean) Indicates whether the room is currently available for booking.
        - `updatedAt`: (Date) The date when the room entry was last updated.

```javascript
import mongoose from "mongoose";
const { Schema } = mongoose;

const RoomSchema = new Schema(
    {
        // title: (String) // String is shorthand for {type: String}
        title: {
            type: String,
            required: true,
        },
        price: {
            type: Number,
            required: true,
        },
        maxPeople: {
            type: Number,
            required: true,
        },
        desc: {
            type: String,
            required: true,
        },
        roomNumbers: [{number: Number, unavailableDates: {type: [Date]}}],
    },
    {timestamps: true}
);

const Room = mongoose.model("Room", RoomSchema);

export default Room;
```

4. **bookings**
    - Stores information about bookings made by users.
    - Example Fields:
        - `user_id`: (ObjectId) The ID of the user who made the booking.
        - `room_id`: (ObjectId) The ID of the room booked.
        - `checkInDate`: (Date) The date when the user checks into the room.
        - `checkOutDate`: (Date) The date when the user checks out of the room.
        - `totalPrice`: (Number) The total price of the booking.
        - `createdAt`: (Date) The date when the booking was made.
        - `updatedAt`: (Date) The date when the booking was last updated.

```javascript
import mongoose from "mongoose";
const { Schema } = mongoose;

const BookingSchema = new Schema(
    {
        user_id: {
            type: Schema.Types.ObjectId,
            ref: "User",
            required: true,
        },
        room_id: {
            type: Schema.Types.ObjectId,
            ref: "Room",
            required: true,
        },
        checkInDate: {
            type: Date,
            required: true,
        },
        checkOutDate: {
            type: Date,
            required: true,
        },
        totalPrice: {
            type: Number,
            required: true,
        },
    },
    { timestamps: true }
);

const Booking = mongoose.model("Booking", BookingSchema);

export default Booking;

```