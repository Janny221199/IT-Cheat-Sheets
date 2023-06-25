
# Transform
- applies movement, rotation, skewing, and scaling to the HTML elements in 2D or 3D
- mostly on events like `:hover`



# Transition
- helps the change to take place smoothly and swiftly
- NOT on events like `:hover` as transformations. instead in the initial "state" 

```
transition: [property] [duration] [timing-function] [delay];
```

- property: the changing property e.g. `transform` or  `background-color` which is changed (in the event)
- duration: how long transition takes
- time-function: ease, ease-in, ease-out etc... How the the animation should be displayed (evenly, begin fast and end slower, begin slow and end faster)
- delay: delay until transition take action


# Example

```css
div {
	transition: transform 0.5s ease-in;
}

div:hover{
	transform: scale(1.05);
}
```