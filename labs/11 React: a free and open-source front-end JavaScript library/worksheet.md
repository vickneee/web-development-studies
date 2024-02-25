# React

React (also known as React.js or ReactJS) is a free and open-source front-end JavaScript library for building user interfaces based on components. It is maintained by Meta (formerly Facebook) and a community of individual developers and companies

## Props in React

Props (short for "properties") are inputs to React components. They are passed from a parent component down to its children and are immutable (read-only) within the child component. Here's how you use props:

```jsx
// ParentComponent.js
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
// ChildComponent.js
import React from 'react';

function ChildComponent(props) {
    return (
        <div>
            <p>Name: {props.name}</p>
            <p>Age: {props.age}</p>
        </div>
    );
}

export default ChildComponent;
```
In the child component, props are accessed as properties of the function argument.

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

## State for User Input

You can use state to manage user input in React. Here's an example of a simple form component that allows a user to enter their name:

```jsx
import React, { useState } from 'react';

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

export default NameForm;
```

In this example, the component maintains a state variable name to store the value of the input field. The handleChange function is called whenever the input value changes, updating the name state accordingly. The handleSubmit function is called when the form is submitted, alerting the submitted name.


