## Getting Started with React Hooks

## What are Hooks?

`Hooks` are a new feature addition in React `16.8` that let you use `state` & other React features without writing a `class`. They are the special functions that `hooks` into React state and lifecycle features from functional components. They don’t fundamentally change how React works, and your knowledge of components, props, and top-down data flow is just as relevant.

## Why Hooks?

Here are some reasons why hooks were introduced in the first place:

- `Using state in functional components`: Before React 16.8, developers had to do the painful task of refactoring their functional component into a class component if suddenly they want the component to be stateful. With Hooks, you are all set to declare, read and update a state variable easily inside the functional component.

- `Class components are a mess`: Honestly, classes are a hindrance to learning React properly. It comes with a lot of boilerplate. We have to understand how `this` works in JavaScript, which is very different from how it works in most languages. We have to remember to `bind` the event handlers. Hooks solves the problem by allowing developers to use the best of React features without having to use classes.

## The Rules Of Hooks

Hooks are similar to JavaScript functions, but we need to follow these two rules when using them:

- `Only Call Hooks at the Top Level` - Don’t call Hooks inside `loops`, `conditions`, or `nested functions`. Instead, always use Hooks at the top level of your React function, before any early returns. By following this rule, we ensure that Hooks are called in the same order each time a component renders.

- `Only Call Hooks from React Functions` - Don’t call Hooks from regular JavaScript functions. Only use them from inside React Functional components or custom Hooks.

React Team also released an ESLint plugin called `eslint-plugin-react-hooks` that enforces the above rules. 
This plugin is included by default in `Create React App`.

## Some commonly used React Hooks

There are 10 in-built hooks that were shipped with React 16.8. Let's explore some common and useful ones:

- ### useState

The `useState` hook allows developers to declare, read and update a state variable easily inside the functional component without needing to convert it to a class component. 

The only argument to the `useState` Hook is the initial state. Unlike with classes, the state doesn’t have to be an object. We can keep a number or a string if that’s all we need. The call to `useState` Hook returns an array in the format `[value, setValue]` where `value` is the current value of the state variable and the `setValue` is the function that updates the value of the state variable.

The first thing we do is import the `useState` Hook:
```js
import React, { useState } from 'react';
```

Next, we call `useState` inside our functional component to declare and initialize the state variable:
```js
const [count, setCount] = useState(0);
```
In the above snippet, we are declaring `count` state variable and `setCount` is the function that updates the value of `count`. Think of it as `this.state.count` and `this.setState` in a class-based component. We pass `0` in `useState` as an argument, which initializes `count` to 0.

To increment the state variable, we make use of our update function declared earlier:
```js
setCount(count + 1)
```

We can read the current value of the state variable by using `count` directly:
```js
<p>`You clicked ${count} times!`</p>
```

And after fitting the pieces together, we have a counter example written with hooks:
```js
import React, { useState } from 'react';

function Counter() {
	const [count, setCount] = useState(0);

	return (
		<div>
			<h2>Counter App</h2>
			<p>You clicked {count} times!</p>
			<button onClick={() => setCount(count + 1)}>Increment</button>
		</div>
	);
}

export default Counter;
```

When the user clicks, we call `setCount` with a new value. React will then re-render the component, passing the new `count` value to it.

> Note: Unlike `this.setState` in a class, updating a state variable always `replaces` it instead of `merging` it.

- ### useEffect

When using hooks and functional components, we no longer have access to React lifecycle methods like `componentDidMount`, `componentDidUpdate`, and so on. These methods help us in dealing with `side effects` in React components. Data fetching, setting up a subscription, and manually changing the DOM in React components are all examples of side effects. Well, `useEffect` is there to help.

We can think of `useEffect` as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` combined. 

The `useEffect` Hook takes in two arguments:

- a callback function to perform the side effects.
- an array that contains a list of state variables to watch for. When changed, it will trigger the hook.

> Note: If the second argument isn't supplied, the hook will run after every render. 

Let's make use of this hook in our counter example:
```js
import React, { useState, useEffect } from 'react';

function Counter() {
	const [count, setCount] = useState(0);
    useEffect(() => {
        console.log('Counter component mounted!');
    });
	return (
		<div>
			<h2>Counter App</h2>
			<p>You clicked {count} times!</p>
			<button onClick={() => setCount(count + 1)}>Increment</button>
		</div>
	);
}

export default Counter;
```

In the above snippet, our `useEffect` hook runs on every render and re-render. If we want to make it run only once on the first render, we need to pass an `empty array []` as the second argument to our hook. 
```js
useEffect(() => {
        console.log('Counter component mounted!');
    }, []);
```

The hook has nothing in the array to watch for, so it will run only once. This is the behaviour we see in `componentDidMount`.

What if we want to run this hook whenever the state changes? We can achieve this by passing an array containing the state variable as the second argument. This will trigger the hook after every state change.
```js
 useEffect(() => {
        console.log('Counter component updated!');
    }, [count]);
```

The Hook will run on the first render and every time the state variable `count` has changed its value. This is the behaviour we see in `componentDidUpdate`.

For the Hook to run right before the component unmounts, we just have to `return` a function from the `useEffect` hook. 
```js
useEffect(() => {
        console.log('Hi from the effect hook!');
        return () => {
            console.log('Counter component unmounting...');
        };
    });
```

This function is usually used to clean up the component by cancelling network requests or invalidating timers. This is the behaviour we see in `componentWillUnmount`.

- ### useContext

The `useContext` hook works with `React's Context API`. It accepts a `context` object, i.e the value that is returned from `React.createContext`, and returns the current context value for that context. Before jumping straight into the implementation, we need to know about `Context`.

Say we have a React app with a single parent component that contains many levels of child components inside of it. Now, imagine passing data from the uppermost component all the way down to the last child component. In React, data is passed top-down from one component to another through props. We have to pass that data through each and every component, through their props, until we reach the last child component. And the process is tiresome & prone to errors. 

The `Context` allows us to directly access data at different levels of the component tree, without having to manually drill down the props through every level.

It's time for the implementation. Let's go step by step.

#### Step 1. Create a context

The first step is to create a `Context` object using `React.createContext` in `App.js`:
```js
import React from 'react';
import './App.css';
import ComponentA from './components/ComponentA.js';

const UserContext = React.createContext();

function App() {
  return (
    <div className="App">
      <ComponentA />
    </div>
  );
}

export default App;
```

#### Step 2. Create a context provider

The second step is to provide the `Context` with a value and the `Provider` must wrap the children components for the value to be available.
```js
import React from 'react';
import './App.css';
import ComponentA from './components/ComponentA.js';

const UserContext = React.createContext();

function App() {
  return (
    <div className="App">
      <UserContext.Provider value={'John Doe'}>
        <ComponentA />
      </UserContext.Provider>
    </div>
  );
}

export default App;
```

#### Step 3. Consume the context value

The third & final step is to consume the `context` value in the consumer component. Export the `UserContext` from `App.js` and import it inside the consumer component. Pass the `UserContext` as an argument to `useContext`. The hook returns the current context value of the `UserContext`. 
```js
export const UserContext = React.createContext();
```
 
```js
import React, { useContext } from 'react';
import { UserContext } from '../App.js';

function ComponentB() {

    const user = useContext(UserContext);

    return (
        <div>
            Username: {user}
        </div>
    );
}

export default ComponentB;
```

That’s all we need to do in order to display our value.

> Note: Don’t forget that the argument that is passed to the `useContext` hook must be the `context` object itself and any component calling the `useContext` will always re-render when the `context` value changes.

## Conclusion

Given all the benefits of using React Hooks, I highly recommend switching from Class components to Functional Components if haven't done it already. You can always learn more about React Hooks from the references below:

- @[Victoria Lo](@victoria)'s [A Look At React Hooks](https://lo-victoria.com/series/a-look-at-react-hooks).
- [Codevolution's React Hooks Tutorial](https://youtu.be/cF2lQ_gZeA8)

If you have any questions, you can leave them in the comments section and I’ll be happy to answer them.


