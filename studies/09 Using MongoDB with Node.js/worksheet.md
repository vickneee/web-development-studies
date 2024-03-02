# Setting up a React project with MongoDB and Mongoose

## Introduction to MongoDB Database Schemas

In the following examples, we outline the schema structures for two distinct collections: one for managing a blog platform and another for handling bookings within a hotel reservation system.

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
    - Stores comments made by users on blog posts.
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

## Using Local Database in React Components

In this example:

- We iterate over each skillsSet in the skills array using the map function, providing a unique setIndex for each set.
- Inside each skillsSet, we use another map function to iterate over the skillsList array, providing a unique skillIndex for each skill within the set.
- Each skill is then rendered with its corresponding data, including the link, imgSrc, imgAltText, and skillName.

**Skills-DB.js:**

```javascript
import SUBLIMETEXT from "../../assets/img/skills/sublime-text.png"
import VISUAL_STUDIO_CODE from "../../assets/img/skills/vscode.png"

// Skills data
export const skills = {
    skillsList: [
        {
            link: "https://www.sublimetext.com/",
            imgAltText: "Sublime Text",
            imgSrc: SUBLIMETEXT,
            skillName: "Sublime Text",
        },
        {
            link: "https://code.visualstudio.com/",
            imgAltText: "Visual Studio Code",
            imgSrc: VISUAL_STUDIO_CODE,
            skillName: "Visual Studio Code",
        },

    ],
};
```

**Skills.js**

```javascript
import {skills} from "./Skills-DB";

const Skills = () => {
    return (
        <div className="flex justify-evenly h-full m-auto">
            <div className="p-40" id="skills">
                <h1 className="pb-3 text-7xl">Tools and Tech Skills</h1>
                <div className="flex shadow-md justify-evenly">
                        <div className="flex p-20 gap-40">
                            {skills.skillsList.map((skill, index) => (
                                <span key={index}>
                                    <h2 className="text-4xl mb-6">{skill.skillName}</h2>
                                        <a href={skill.link}
                                           target="_blank" rel="noopener noreferrer">
                                            <img src={skill.imgSrc} alt={skill.imgAltText} className="h-20 pb-8"></img>
                                        </a>
                                    </span>
                            ))}
                        </div>
                    </div>
                </div>

        </div>
    );
};

export default Skills;
```
