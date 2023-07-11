- use [useEffect](useEffect.md) and [useCallback](useCallback.md) for HTTP Requests  
- useEffect: Load data when components first runs -> provide function which gets called as dependency because it could happen that this function changed based on external data and we wanna fetch data again.  
- useCallback: to prevent infinity loop -> wrapped call function inside useEffect with useCallback

```js
import React, { useState, useEffect, useCallback } from 'react';
import './App.css';

function App() {
  const [data, setData] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const fetchHandler = useCallback(async () => {
    setIsLoading(true);
    setError(null);
    try {
      const response = await fetch('URL');
      if (!response.ok) {
        throw new Error('Handle the error message properly!');
      }

      const jsonData = await response.json();
      setData(jsonData);
    } catch (error) {
      setError(error.message);
    }
    setIsLoading(false);
  }, []);

  useEffect(() => {
    fetchHandler();
  }, [fetchHandler]);

  return (
    <>
      ...
    </>
  );
}

export default App;
```