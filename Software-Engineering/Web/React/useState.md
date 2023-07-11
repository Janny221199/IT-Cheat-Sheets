- manage state in functional components
- let React update UI dynamic and reactive based on state changes 
- hook returns 2 variables, which we could get via destruction (the current value itself and a setter method)
- hook takes initial value

```js
const [count, setCount] = useState(0);
...
setCount(10);
```

# Update state based on previous older state
- because of reacts way of managing and calling the functions it could happen that it is not running in the right order
- Using the previous state in this way ensures that state updates are based on the most up-to-date value and prevents issues that could arise from asynchronous updates.
```js
const [count, setCount] = useState(0);
...
setCount(prevCount => prevCount + 1); // DO NOT setCount(count + 1);
```