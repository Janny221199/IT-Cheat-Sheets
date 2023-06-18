Example html:
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="styles.css">
    <title>Position</title>
  </head>
  <body>
    <div id="container">
      <p id="one">Element 1</p>
      <p id="two">Element 2</p>
      <p id="three">Element 3</p>
    </div>
    <div>
      <p>Element</p>
      <p>Element</p>
      <p>Element</p>
    </div>
    <div>
      <p>Element</p>
      <p>Element</p>
      <p>Element</p>
    </div>
  </body>
</html>
```

Example css:
```css
div {
  background-color: #c7c7c7;
  padding: 20px;
  text-align: center;
}

p {
  color: black;
  padding: 50px;
  margin: 0;
}

#container {
  background-color: #521751;
}

#container p {
  color: white;
}

#one {
  background-color: #943092;
}

#two {
  background-color: #c95bc7;
}

#three {
  background-color: #dd61db;
}
```

View:
![](attachments/Pasted%20image%2020230618060831.png)

# Move Elements Down in the Document Flow (Relative)
- normal elements follow document flow strictly
- can be changed with position
- default is `position: static;`
- to change it use `position: relative;`
- but element is still in the document flow (`Element 3` is still at the position as `Element 2` would still be there)

```css
#two {
  background-color: #c95bc7;
  position: static;
}
```

## Move Down 
```css
#two {
  background-color: #c95bc7;
  position: relative;
  top: 20px;
}
```

or

```css
#two {
  background-color: #c95bc7;
  position: relative;
  bottom: -20px;
}
```

![](attachments/Pasted%20image%2020230618061320.png)

## Move Right 
```css
#two {
  background-color: #c95bc7;
  position: relative;
  left: 20px;
}
```

or

```css
#two {
  background-color: #c95bc7;
  position: relative;
  right: -20px;
}
```

![](attachments/Pasted%20image%2020230618061557.png)

# Move Elements with absolute
- removed from document flow (as it would not be existing)
```css
#two {
  background-color: #c95bc7;
  position: absolute;
}
```
![](attachments/Pasted%20image%2020230618061925.png)

`Element 3` is behaving as if `Element 2` is not there anymore (it also takes the full space also "under"  `Element 2`)

## Move Absolutely
- with a position (top, right, bottom, left) it is not set relatively to the object itself in the document flow (see [Move Elements Down in the Document Flow (Relative)](Positioning%20Elements.md#Move%20Elements%20Down%20in%20the%20Document%20Flow%20(Relative))), because its out of the document flow -> it is relative to to root relative ancestor
```css
#two {
  background-color: #c95bc7;
  position: absolute;
  bottom: 20px;
}
```
![](attachments/Pasted%20image%2020230618062559.png)

## Scrolling with absolute
- it is moved out of a page if we scroll because its is "sticky"
```css
#two {
  background-color: #c95bc7;
  position: absolute;
  top: 10px;
}
```

![](attachments/Pasted%20image%2020230618063049.png)

# Move Elements with fixed
- as well as with absolute [Move Elements with absolute](Positioning%20Elements.md#Move%20Elements%20with%20absolute) it is out of the document flow, but it fixed on the screen if we scroll. so it gets not "scrolled out of the screen"
```css
#two {
  background-color: #c95bc7;
  position: fixed;
  bottom: -20px;
}
```
![](attachments/Pasted%20image%2020230618063401.png)

## Scrolling with fixed 
- it gets not "scrolled out of the screen"
- it stays there fixed
```css
#two {
  background-color: #c95bc7;
  position: fixed;
  top: 10px;
}
```
![](attachments/Pasted%20image%2020230618063221.png)

