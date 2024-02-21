## MongoDB Database Schema

### Example of Booking Collections

1. **users**
    - Stores information about users registered on the booking platform.
    - Example Fields:
        - `username` (String): The unique username of the user.
        - `email`(String): The unique email address of the user.
        - `password`(String): The hashed password of the user.

```javascript
import mongoose from "mongoose";

const UserSchema = new mongoose.Schema(
        {
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
        - `name` (String): The name of the hotel.
        - `type` (String): The type of the accommodation.
        - `location` (String): The location of the hotel.
        - `description` (String): A description of the hotel.
        - `rating` : (Number) The rating of the hotel (e.g., out of 5 stars).
        - `amenities` (Array of Strings): The amenities offered by the hotel.
        - `featured` (Boolean): Indicates whether the hotel is featured or not (true/false).

```javascript
import mongoose from "mongoose";

const HotelSchema = new mongoose.Schema({
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
        - `hotel_id` (ObjectId): The ID of the hotel to which the room belongs.
        - `type` (String): The type of room (e.g., single, double, suite).
        - `price` (Number): The price per night for the room.
        - `availability` (Boolean): Indicates whether the room is currently available for booking.
        - `updatedAt` (Date): The date when the room entry was last updated.

```javascript
import mongoose from "mongoose";

const RoomSchema = new mongoose.Schema(
    {
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
        - `user_id` (ObjectId): The ID of the user who made the booking.
        - `room_id` (ObjectId): The ID of the room booked.
        - `checkInDate` (Date): The date when the user checks into the room.
        - `checkOutDate` (Date): The date when the user checks out of the room.
        - `totalPrice` (Number): The total price of the booking.


### Example of Blog Collections

1. **users**
    - Stores information about users registered on the platform.
    - Example Fields:
        - `username` (String): The unique username of the user.
        - `email` (String): The email address of the user.
        - `password` (String): The hashed password of the user.
        - `createdAt` (Date): The date when the user account was created.

2. **posts**
    - Stores information about blog posts created by users.
    - Example Fields:
        - `title` (String): The title of the blog post.
        - `content` (String): The content of the blog post.
        - `author` (ObjectId): The ID of the user who created the post.
        - `createdAt` (Date): The date when the post was created.
        - `updatedAt` (Date): The date when the post was last updated.

3. **comments**
    - Stores comments made by users on blog posts.
    - Example Fields:
        - `content` (String): The content of the comment.
        - `post_id` (ObjectId): The ID of the post the comment belongs to.
        - `author` (ObjectId): The ID of the user who made the comment.
        - `createdAt` (Date): The date when the comment was created.
        - `updatedAt` (Date): The date when the comment was last updated.
