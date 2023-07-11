- memoize a function, similar to how [useMemo](useMemo.md) memoizes a value
- useful when you want to avoid unnecessary re-creations of functions, especially when passing them as props to child components

```js
import React, { useCallback, useState } from 'react';

const ParentComponent = () => {
  const [count, setCount] = useState(0);

  // Memoized callback function
  const handleClick = useCallback(() => {
    console.log('Button clicked!');
    setCount(count + 1);
  }, [count]); // Re-create when count changes

  return (
    <div>
      <p>Count: {count}</p>
      <ChildComponent onClick={handleClick} />
    </div>
  );
};

const ChildComponent = ({ onClick }) => {
  return <button onClick={onClick}>Click me</button>;
};

export default ParentComponent;
```

In this example, we have a parent component called `ParentComponent` and a child component called `ChildComponent`. The parent component has a state variable called `count`, which represents a counter. Whenever the count changes, we want to re-create the `handleClick` callback function.

By using `useCallback`, we wrap the callback function and specify `[count]` as the dependency array. This means that the function will only be re-created if `count` changes. The initial creation and subsequent re-creations are logged in the console when the button is clicked.

By memoizing the callback function with `useCallback`, we ensure that the child component `ChildComponent` receives the same function reference across renders unless its dependencies change. This can be useful to optimize performance, as it prevents unnecessary re-renders of child components that depend on the callback function.