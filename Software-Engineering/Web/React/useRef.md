- hook is used to create a mutable reference that persists across component renders
- Unlike state variables, changes to the `useRef` value do not trigger a re-render of the component
- It provides a way to access and manipulate DOM elements (but be careful, mostly you dont want to edit the DOM manually and let that do react)
- bind jsx element with `ref` to the useRef

```js
import React, { useRef } from 'react';

const TextInput = () => {
  const inputRef = useRef(null);

  const handleClick = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input type="text" ref={inputRef} />
      <button onClick={handleClick}>Focus Input</button>
    </div>
  );
};

export default TextInput;

```