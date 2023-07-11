- useful for side effects 
- instead of useState which sets the state to an initial value after component rebuild, it could load specific things at certain times (http requests, local storage)

# useEffect with no dependency
```js
useEffect(() => {
	console.log('Runs on each onMount, onUnmount and render / state lifecycle update');
});
```

# useEffect with empty dependency
```js
useEffect(() => {
	console.log('Runs on each onMount and onUnmount');
}, []);
```

# useEffect with specified dependency
- **DON'T need to add state updating functions** e.g. setXxx
- **DON'T need to add "built-in" APIs or functions** e.g. fetch() or localStorage
- **DON'T need to add variables or functions** you might've **defined OUTSIDE of your components** e.g. helper function in separate file
```js
useEffect(() => {
	console.log('Runs on each onMountand, onUnmount and when someVar or someOtherVar changes');
}, [someVar, someOtherVar]);
```

# Cleanup
- good e.g. to `removeEventListeners()` (when you `addEventListener()` in useEffect) or when you `setTimeout()` to clean it with `clearTimeout()`

```js
useEffect(() => {
	console.log('Runs on each onMountand, onUnmount and when someVar or someOtherVar changes');
	return () => {
		console.log('CLEANUP: Runs when component is destroyed and before each useEffect (onMountand, onUnmount and when someVar or someOtherVar changes) ');
	}
}, [someVar, someOtherVar]);
```