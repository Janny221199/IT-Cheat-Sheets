
- representation of the parsed HTML & CSS content, that can be queried or manipulated via JavaScript
	- inside `window` object and accessible via: `window.document` or just `document` because `window` properties and functions are accessible directly 
	- with `console.log(document)` it prints a html like representation 
	- with `console.dir(document)` it prints the real js object representation 
- Why DOM? Via the DOM, our JavaScript code is able to query, read or manipulate what's displayed on the screen - this allows us to build interactive websites.


# Access Dom Elements
```html
...
<body>
	<h1>H1<h1>
	<p>This is a <a id="my-link" class="my-class" href="#">link</a></p>
</body>
...
```

## Old way by drilling deep
```js
	document.children[1].children[0].href = 'https://sanker.io'
```
Problem: nit dynamic, if we change the html structure

## By Selectors and Queries 

```js
	document.getElementById('my-id');
	document.getElementByClassName('my-class'); // first with that class name
	document.querySelector('p a'); // first by css selector (here first a element in a paragraph). also by id ('.my-id'), class and everything else
	document.querySelectorAll('p a'); // all a elements inside a paragraph
```



# Create Element in DOM

```js
// create element
let newAnchorElement = document.createElement('a');
newAnchorElement.href = 'https://sanker.io'
// ... assign text and so on

// get Element where it should added to
let paragraphElement = document.getElementById('my-paragraph');

// append created a element (added as last children)
paragraphElement.append(newAnchorElement);
```

# Remove Element from the DOM
```js
// get Element which should be deleted
let element = document.getElementById('my-id');

// remove it 
element.remove(); // in very old browsers not supported -> element.parentElement.removeChild(element);
```


# innerHTML
contains text AND html elements. (textContent only the text)

```js
let element = document.getElementById('my-id');
element.innerHTML = 'New content of <strong>element</strong>.'
```