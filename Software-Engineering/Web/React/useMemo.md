- there for memoize / store data (e.g. the result of a function call)
- useful for data which is performance intense to calculate on every component update function re-run (e.g. sorting a list)
- the result doesn't change unless its dependencies change
- can prevent unnecessary re-computations and improve the performance of your application

```js
import React, { useMemo, useState } from 'react';

const ExpensiveComponent = () => {
  const [count, setCount] = useState(0);

  // Expensive computation
  const expensiveValue = useMemo(() => {
    console.log('Calculating expensive value...');
    let result = 0;
    for (let i = 0; i < count; i++) {
      result += i;
    }
    return result;
  }, [count]); // Re-compute when count changes

  return (
    <div>
      <p>Count: {count}</p>
      <p>Expensive Value: {expensiveValue}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};

export default ExpensiveComponent;

```

In this example, we have a component called `ExpensiveComponent`. It has a state variable called `count`, which represents the number of iterations for our expensive computation. Whenever the count changes, we want to re-compute the expensive value.

By using `useMemo`, we wrap the expensive computation function and specify `[count]` as the dependency array. This means that the function will only be re-computed if `count` changes. The initial computation and subsequent re-computations are logged in the console.