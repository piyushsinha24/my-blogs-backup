## Optimizing Performance in React Apps - I

Building web apps is a fun thing to do but if we also invest some time thinking about the performance, it's a cherry on top. Everyone hates a sluggish interface. In this series, we'll go through some of the tips and tricks to build a optimal interface.


## Unnecessary Re-renders

React uses the concept of virtual DOM to minimize the performance cost of re-rendering but is it enough to optimize the performance?

While writing components, we are implicitly building a component tree and the updates propagates all the way down. Because of this, re-rendering is happening in more place than it needs to. Let me explain in the example below:

Here we have a `Parent` component with a counter logic & two child components. `Child1` takes Parent's state variable - `count` as a prop and `Child2` has nothing to do with Parent's state:

```js
import React, { useState } from "react";
import Child1 from "./Child1";
import Child2 from "./Child2";

function Parent() {
  const [count, setCount] = useState(0);
  return (
    <div>
     <button type="button" onClick={() => setCount((count) => count + 1)}>
        Counter
      </button>
      <Child1 count={count} />
      <Child2/>
    </div>
  );
}

export default Parent;
```

```js
import React from 'react';

function Child1(props) {
  return (
    <div>Counter1: {props.count}</div>
  )
}

export default Child1;
```

```js
import React from "react";

function Child2() {
  return <div>This is Child2 component</div>;
}

export default Child2;
```

> Make sure `React Developer Tools` is installed as we're gonna use the `Profiler` to record the rendering details. 

Start profiling and click on `Counter` button:

![Profiler Tab with no memoization](https://cdn.hashnode.com/res/hashnode/image/upload/v1652981621195/6e94WYdYC.png align="center")

The chart above shows that `Parent`, `Child1` & `Child2` are re-rendered. `Child1` getting re-rendered is fine as it depends on `Parent's` state changes but `Child2` doesn't need to be re-rendered here. This can impact the performance significantly if we were doing some time taking operations in `Child2`. 

To fix this, we can make use of `memoization`.

## Memoization

Memoization is an optimization technique to speed up applications by storing the results of expensive function calls and returning the `cached` result when the same inputs occur again.

In React apps, we can implement this behaviour using `React.memo`. 

### React.memo

It is a higher order component that takes a component and returns a `memoized` component. This means that React will skip rendering the component, and reuse the last rendered result if there's no change in props.

Let's wrap our Child2 component with React.memo:

```js
import React from "react";

function Child2() {
  return <div>This is Child2 component</div>;
}

export default React.memo(Child2);
```
Start profiling and click on `Counter` button:

![Profiler Tab with memoization](https://cdn.hashnode.com/res/hashnode/image/upload/v1652987032318/y41xc49yd.png align="center")

We can see that only `Parent` & `Child1` is re-rendered. Hence, improving the performance. 

However, this solution won't stop all the unnecessary re-renders. It works only when primitive values(*`number`, `string`, `boolean`, `null`, `undefined`*) are passed down as props. Why? Because React.memo make use of `referential equality` meaning checking if the reference has changed or not. 

If we talk about non-primitive values(*`objects`*) as props, referential equality is always `false` as anytime the parent is re-rendered, a new reference to the object is created.

This can be seen in our example if we pass an object as prop to our Child2 component:

```js
import React, { useState } from "react";
import Child1 from "./Child1";
import Child2 from "./Child2";

function Parent() {
  const [count, setCount] = useState(0);
  const person = { name: "Piyush" };
  return (
    <div>
      <button type="button" onClick={() => setCount((count) => count + 1)}>
        Counter
      </button>
      <Child1 count={count} />
      <Child2 person={person} />
    </div>
  );
}

export default Parent;
```

```js
import React from "react";

function Child2(props) {
  return <div>This is Child2 component by {props.person.name}</div>;
}

export default React.memo(Child2);
```

Start profiling and click on `Counter` button:

![Memoization with object](https://cdn.hashnode.com/res/hashnode/image/upload/v1652991811992/7VoUJEoR1.png align="center")

We can see all of them getting re-rendered. So, what now? `Hooks` to the rescue. 

### useMemo

If an `object` is needed to be passed down as a prop, we can first memoize that object & then pass it down to the child component. Let's make the change in our example:

```js
import React, { useMemo, useState } from "react";
import Child1 from "./Child1";
import Child2 from "./Child2";

function Parent() {
  const [count, setCount] = useState(0);
  const person = { name: "Piyush" };
  const memoizedPerson = useMemo(() => person, []);
  return (
    <div>
      <button type="button" onClick={() => setCount((count) => count + 1)}>
        Counter
      </button>
      <Child1 count={count} />
      <Child2 person={memoizedPerson} />
    </div>
  );
}

export default Parent;
```

Start profiling and click on `Counter` button:

![usememo_objects.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1652993449436/C44t7YfZ_.png align="center")

We can see that only `Parent` & `Child1` is re-rendered.

### useCallback

It is also common to pass down `functions` as props. As it is also a non-primitive value, React.memo doesn't optimize the re-renders:

```js
import React, { useMemo, useState } from "react";
import Child1 from "./Child1";
import Child2 from "./Child2";

function Parent() {
  const [count, setCount] = useState(0);
  const person = { name: "Piyush" };
  const memoizedPerson = useMemo(() => person, []);
  const handleClick = () => {};
  return (
    <div>
      <button type="button" onClick={() => setCount((count) => count + 1)}>
        Counter
      </button>
      <Child1 count={count} />
      <Child2 person={memoizedPerson} handler={handleClick} />
    </div>
  );
}

export default Parent;
```

Start profiling and click on `Counter` button:

![memoization_with_functions.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1652994472790/PBIO1TzwX.png align="center")

`useCallback` hook can be used here to return the memoized callback function & pass that as a prop. Let's make the change in our example:

```js
import React, { useCallback, useMemo, useState } from "react";
import Child1 from "./Child1";
import Child2 from "./Child2";

function Parent() {
  const [count, setCount] = useState(0);
  const person = { name: "Piyush" };
  const memoizedPerson = useMemo(() => person, []);
  const handleClick = () => {};
  const memoizedHandleClick = useCallback(handleClick, []);
  return (
    <div>
      <button type="button" onClick={() => setCount((count) => count + 1)}>
        Counter
      </button>
      <Child1 count={count} />
      <Child2 person={memoizedPerson} handler={memoizedHandleClick} />
    </div>
  );
}

export default Parent;
```
Start profiling and click on `Counter` button:

![usecallback_functions.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1652994874659/nuVEzF9KZ.png align="center")

We can see that only `Parent` & `Child1` is re-rendered. Hence, performance improved by skipping unnecessary re-renders.

## Conclusion

That’s all for this article. We learnt about unnecessary re-renders & how memoization can help us to skip them. Stay tuned & happy Coding!

If you have any questions, you can leave them in the comments section and I’ll be happy to answer them.











