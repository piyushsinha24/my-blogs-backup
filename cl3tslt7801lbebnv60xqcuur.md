## Optimizing Performance in React Apps - II

Welcome to [Optimizing Performance in React Apps](https://piyushsinha.tech/series/optimizing-react), a series discussing tips & tricks to build an optimal interface.

In this article, we'll learn how to efficiently render large lists of data.

## My List is too big!

We often have a requirement to show a large amount of data on the screen. The conventional approach is to map through the data & return the JSX. Everything goes into the DOM and gets rendered. But if the list is too big, it's costly and will result in `slow load time` & a `sluggish interface`. Let me explain this with an example.

Here, we are generating `100` users using [Faker](https://fakerjs.dev/) & mapping them as list items:


```js
import React, { useState } from "react";
import { faker } from "@faker-js/faker";

function List() {
  const [users, setUsers] = useState([]);

  const generateUsers = () => {
    const fakeUsers = Array.from(Array(100), () => ({
      name: faker.name.findName(),
    }));
    setUsers(fakeUsers);
  };

  return (
    <div>
      <button onClick={generateUsers}>Generate Users</button>
      {users.map((user, index) => {
        return <li key={index}>{user.name}</li>;
      })}
    </div>
  );
}

export default List;
```

We'll make use of the `Profiler` to record the rendering details. Start profiling & click on the `Generate Users` button:

%[https://www.canva.com/design/DAFCHeN8asI/watch]

The list took `4.5ms` to render which isn't bad but what if the list is too big, say `10,000` users?

```js
import React, { useState } from "react";
import { faker } from "@faker-js/faker";

function List() {
  const [users, setUsers] = useState([]);

  const generateUsers = () => {
    const fakeUsers = Array.from(Array(10000), () => ({
      name: faker.name.findName(),
    }));
    setUsers(fakeUsers);
  };

  return (
    <div>
      <button onClick={generateUsers}>Generate Users</button>
      {users.map((user, index) => {
        return <li key={index}>{user.name}</li>;
      })}
    </div>
  );
}

export default List;
```

Start profiling & click on the `Generate Users` button:

%[https://www.canva.com/design/DAFCHj4FnnM/watch]

This time it took `153.9ms` & the delay in rendering is noticeable. This is expected if you push the `10,000`  list items in the DOM.

Also, we don't just map the data as simple `<li/>` tags in real projects. We do build a component for our list item which may be having its own `methods`, `event handlers`, etc. Think of the cost of rendering then & the poor user experience it brings.

So, what's the solution? `Pagination`? It will fix the `slow load time` but it has its cons. If you search for a product on Amazon and it shows only the first 10 results, as a customer you want more. But how many times do we click on pages 2, 3, etc? We love to `scroll` down the results.

Well, there's a better solution than pagination & it's known as `virtualization`/`windowing`.

## Virtualization/Windowing

In simple words, only rendering what's in the `viewport`. Instead of pushing everything in the DOM, we just load a subset of the data which is visible to the user. To get more clarity, see this diagram:

![virtualization/windowing](https://cdn.hashnode.com/res/hashnode/image/upload/v1653945340719/7zsfnuU6O.png align="left")

The list items currently in the viewport are `active` & loaded inside the DOM. Apart from this subset of data, the rest are `inactive` & not to be found in the DOM. As the user `scrolls`, active ones are replaced by inactive ones in the DOM. Hence, only rendering what's in the viewport.

To implement this, we have two packages - [react-virtualized](https://www.npmjs.com/package/react-virtualized) & [react-window](https://www.npmjs.com/package/react-window). We're gonna make use of `react-window` as it is very lightweight.

### react-window

It comes with a set of components which makes it easier to implement virtualization/windowing. We will make use of the `FixedSizeList` component as the data in our example is a long, one-dimensional list of equally-sized items.

The `FixedSizeList` component expects the following props:
- `height` - the height of the viewport
- `width` - the width of the viewport
- `itemCount` - the length of the list
- `itemSize` - the height of the row

& a function that renders the rows is passed as a `child`. This function gets called for `each` row and it receives `index` & `style` from the parent component.

> The `style` received is really important & must be attached to the row elements. It is used to position them while the list is being scrolled & items are being mounted/unmounted from the DOM.

The code goes like this:
```js
import React, { useState } from "react";
import { faker } from "@faker-js/faker";
import { FixedSizeList } from "react-window";

function EfficientList() {
  const [users, setUsers] = useState([]);

  const generateUsers = () => {
    const fakeUsers = Array.from(Array(10000), () => ({
      name: faker.name.findName(),
    }));
    setUsers(fakeUsers);
  };

  const renderRow = ({ index, style }) => (
    <li key={index} style={style}>
      {users[index]["name"]}
    </li>
  );

  return (
    <div>
      <button onClick={generateUsers}>Generate Users</button>
      <FixedSizeList
        height={600}
        width={600}
        itemCount={users.length}
        itemSize={30}
      >
        {renderRow}
      </FixedSizeList>
    </div>
  );
}

export default EfficientList;
```

Now, start profiling & check how long it takes for the same `10,000` users:

%[https://www.canva.com/design/DAFCNoPq9hk/watch]

Our efficient list took only `4.5ms` which is roughly `34 times faster` than the time taken by the conventional approach. Also, if we inspect our DOM, we can clearly see the virtualization/windowing in action:

%[https://www.canva.com/design/DAFCNgrF2x0/watch]

## Conclusion

That’s all for this article. We learnt how the conventional approach of rendering large lists of data can result in slow load time & a sluggish interface. And why virtualization/windowing should be the preferred way. Stay tuned & happy Coding!

If you have any questions, you can leave them in the comments section and I’ll be happy to answer them.



