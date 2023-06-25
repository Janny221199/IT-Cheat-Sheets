Example html:
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="styles.css" />
    <title>Flexbox</title>
  </head>
  <body>
    <header id="container">
      <div>
        <p>Item 1</p>
      </div>
      <nav>
        <ul>
          <li>Item 2.1 (List)</li>
          <li>Item 2.2 (List)</li>
        </ul>
      </nav>
    </header>
  </body>
</html>

```

Example css:
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
}

p {
  background-color: rgb(211, 88, 209);
  margin: 10px;
  width: 250px;
}

ul {
  background-color: rgb(250, 146, 63);
  list-style: none;
  width: 250px;
  margin: 10px;
  padding: 0;
}

li {
  background-color: rgb(238, 212, 192);
  width: 100px;
}

```

View: 
![](attachments/Pasted%20image%2020230618053749.png)

# Adding Flexbox
- in container to manage children

adding it to the header to display everything in a row:
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
}
```
![](attachments/Pasted%20image%2020230618054100.png)

## showing flex in developer tools
click on flex:
![](attachments/Pasted%20image%2020230618054146.png)

![](attachments/Pasted%20image%2020230618054157.png)

## Flex Direction

### row (default)
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
  flex-direction: row;
}
```
![](attachments/Pasted%20image%2020230618054527.png)
Main-Axis from left to right
Cross-Axis from top to bottom

### row-reverse
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
  flex-direction: row-reverse;
}
```
![](attachments/Pasted%20image%2020230618054550.png)
Main-Axis from right to left
Cross-Axis from top to bottom

### column
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
  flex-direction: column;
}
```
![](attachments/Pasted%20image%2020230618054610.png)
Main-Axis from top to bottom
Cross-Axis from left to right

### column-reverse
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
  flex-direction: column-reverse;
}
```
![](attachments/Pasted%20image%2020230618054627.png)
Main-Axis from bottom to top
Cross-Axis from left to right


## Flex Wrap
- if the elements are break down to the "line" when the screen is shrinking
- i removed the width property

### nowrap (default)
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  /* width: 600px; */
  display: flex;
  flex-wrap: nowrap;
}
```
![](attachments/Pasted%20image%2020230618055437.png)

### wrap 
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  /* width: 600px; */
  display: flex;
  flex-wrap: nowrap;
}
```
![](attachments/Pasted%20image%2020230618055420.png)


## Aligning Flex Items in Cross Axis
- aligning items on **cross axis**

### center (default)
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
  align-items: center;
}
```
![](attachments/Pasted%20image%2020230618055652.png)

## Justify Flex Items
- aligning items on the **main axis**

### start (default)
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
  align-items: center;
  justify-content: start;
}
```
![](attachments/Pasted%20image%2020230618060233.png)

### end
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
  align-items: center;
  justify-content: end;
}
```
![](attachments/Pasted%20image%2020230618060309.png)

### center
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
  align-items: center;
  justify-content: center;
}
```
![](attachments/Pasted%20image%2020230618060255.png)

### space between
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
```
![](attachments/Pasted%20image%2020230618055954.png)

### space around
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
  align-items: center;
  justify-content: space-around;
}
```
![](attachments/Pasted%20image%2020230618060108.png)

### space evenly
```css
#container {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 600px;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
}
```
![](attachments/Pasted%20image%2020230618060405.png)