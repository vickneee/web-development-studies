# React Open-Source Front-End JavaScript Library

React (also known as React.js or ReactJS) is a free and open-source front-end JavaScript library for building user interfaces based on components. It is maintained by Meta (formerly Facebook) and a community of individual developers and companies

## Props in React

Props (short for "properties") are inputs to React components. They are passed from a parent component down to its children and are immutable (read-only) within the child component. Here's how you use props:

```jsx
// ParentComponent.jsx
import React from 'react';
import ChildComponent from './ChildComponent';

function ParentComponent() {
    return (
        <div>
            <ChildComponent name="John" age={30} />
        </div>
    );
}

export default ParentComponent;
```

```jsx
// ChildComponent.jsx
import React from 'react';
import PropTypes from 'prop-types'; // import PropTypes from prop-types 

function ChildComponent(props) {
    return (
        <div>
            <p>Name: {props.name}</p> 
            {/* To avoid 'name' is missing in props validation. */}
            <p>Age: {props.age}</p> 
            {/* To avoid 'age' is missing in props validation. */} 
        </div>
    );
}

ChildComponent.propTypes = {
    name: PropTypes.string.isRequired, // React performs prop validation, issuing warnings in development for missing or incorrect types.
    age: PropTypes.number.isRequired, // React performs prop validation, issuing warnings in development for missing or incorrect types.
};

export default ChildComponent;
```

In this code snippet:

- We import PropTypes from the 'prop-types' package.
- We use ChildComponent.propTypes to define the PropTypes for the name and age props.
- We specify that both name and age are required (isRequired), and name should be a string while age should be a number.

## State in React

State represents the data a component needs to manage internally. Unlike props, state is mutable and controlled by the component itself. Here's how you use state:

```jsx
import React, { useState } from 'react';

function Counter() {
    const [count, setCount] = useState(0);

    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>Increment</button>
        </div>
    );
}

export default Counter;
```

In the example above, useState is a hook that allows functional components to have state. The count variable holds the state value, and setCount is a function used to update that state.

# Conditional Rendering

In React, conditional rendering allows you to render different components or elements based on certain conditions.

```jsx
import React from 'react';

function Greeting(props) {
    const isLoggedIn = props.isLoggedIn;
  
  // Conditional rendering
  if (isLoggedIn) {
      return <UserGreeting />;
    }
    return <GuestGreeting />;
}

// UserGreeting
function UserGreeting() {
    return <h1>Welcome back!</h1>;
}

// GuestGreeting
function GuestGreeting() {
    return <h1>Please sign up.</h1>;
}

function App() {
    const isLoggedIn = true; // or false
    
    return (
        <div>
            <Greeting isLoggedIn={isLoggedIn} />
        </div>
    );
}

export default App;
```

```jsx
import { useState } from 'react';

function App() {
    const [count, setCount] = useState(0);

    return (
        <>
        {count <= 6 
            ? <p>You have pressed the button {count} times</p> 
            : <p>You have pressed the button more than 6 times</p>
        }
        <button onClick={() => setCount(count + 1)}>Press me</button>
        </>
    );
}

export default App;
```

## State with Additional Functionalities

```jsx
import React, { useState } from 'react';

function CounterApp() {
    const [count, setCount] = useState(0);

    const increment = () => {
        setCount((prevCount) => prevCount + 1);
    };

    const decrement = () => {
        setCount((prevCount) => prevCount - 1);
    };

    const reset = () => {
        setCount(0);
    };

    return (
        <div>
        <h2>Counter: {count}</h2>
        <button onClick={increment}>Increment</button>
        <button onClick={decrement}>Decrement</button>
        <button onClick={reset}>Reset</button>
        </div>
    );
}

export default CounterApp;
```

## State for Changing Color

```jsx
import { useState } from "react";
import "./App.css";

function App() {
    const [color, setColor] = useState("black");

    const ChangeColor = () => {
        setColor(color === "black" ? "red" : "black");
    };

    return (
        <>
            <h1 style={{ color: color }}>Hello World! I love React!</h1>
            <button onClick={ChangeColor}>Change Text Color</button>
        </>
    );
}

export default App;
```

## State for User Input

You can use state to manage user input in React. Here's an example of a simple form component that allows a user to enter their name:

```jsx
import React, { useState } from 'react';
import PropTypes from 'prop-types';

function NameForm() {
    const [name, setName] = useState('');

    const handleChange = (event) => {
        setName(event.target.value);
    };

    const handleSubmit = (event) => {
        event.preventDefault();
        alert('Submitted name: ' + name);
    };

    return (
        <form onSubmit={handleSubmit}>
            <label>
                Name:
                <input type="text" value={name} onChange={handleChange} />
            </label>
            <button type="submit">Submit</button>
        </form>
    );
}

NameForm.propTypes = {
    name: PropTypes.string.isRequired,
};

export default NameForm;
```

In this example we import PropTypes from the 'prop-types' package and use NameForm.propTypes to define the PropTypes for the 'name' prop. We specify that 'name' is required and should be a string. This helps ensure that the component is used correctly and provides helpful warnings in development if the props are incorrect.

In this example, the component maintains a state variable name to store the value of the input field. The handleChange function is called whenever the input value changes, updating the name state accordingly. The handleSubmit function is called when the form is submitted, alerting the submitted name.


## State for Multiple User Inputs

```jsx
import { useState } from "react";

function MultipleInputForm() {
    const [formData, setFormData] = useState({
        firstName: "",
        lastName: "",
        email: "",
        password: "",
    });

    const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData({ ...formData, [name]: value });
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        // You can handle form submission here, e.g., send data to server or perform validation
        console.log(formData);
    };

    return (
        <form onSubmit={handleSubmit}>
            <div>
                <label>
                    First Name:
                    <input
                        type="text"
                        name="firstName"
                        value={formData.firstName}
                        onChange={handleChange}
                    />
                </label>
            </div>
            <div>
                <label>
                    Last Name:
                    <input
                        type="text"
                        name="lastName"
                        value={formData.lastName}
                        onChange={handleChange}
                    />
                </label>
            </div>
            <div>
                <label>
                    Email:
                    <input
                        type="email"
                        name="email"
                        value={formData.email}
                        onChange={handleChange}
                    />
                </label>
            </div>
            <div>
                <label>
                    Password:
                    <input
                        type="password"
                        name="password"
                        value={formData.password}
                        onChange={handleChange}
                    />
                </label>
            </div>
            <button type="submit">Submit</button>
        </form>
    );
}

export default MultipleInputForm;
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