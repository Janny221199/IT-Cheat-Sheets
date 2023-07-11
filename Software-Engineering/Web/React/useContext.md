- provide data across components without a big prop chain and lifting data up and so on...
- used to access values from the nearest ancestor `Context` component. It provides a way to share data or state across multiple components without the need for prop drilling
- **NOT optimised for high frequently changes** -> [Redux](Redux.md)
- `React.createContext();` often used in a separate file with a custom context provider (with functions and so on provided to overall App Component)

```js
import React, { useContext } from 'react';

// Create a context
const ThemeContext = React.createContext();

// Define a component that uses the context
const ThemeDisplay = () => {
  // Access the context value using useContext
  const theme = useContext(ThemeContext);

  return <p>Current theme: {theme}</p>;
};

// Define another component that uses the context
const ThemeToggler = () => {
  // Access the context value using useContext
  const theme = useContext(ThemeContext);

  const toggleTheme = () => {
    // Perform some action based on the theme value
    // ...
  };

  return (
    <div>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </div>
  );
};

// Define the parent component that provides the context value
const App = () => {
  const theme = 'light';

  return (
    <ThemeContext.Provider value={theme}>
      <div>
        <ThemeDisplay />
        <ThemeToggler />
      </div>
    </ThemeContext.Provider>
  );
};

export default App;


```