html:
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="styles.css" />
    <title>Grid</title>
  </head>
  <body>
    <main>
        <ul>
          <li>Card 1</li>
          <li>Card 2</li>
          <li>Card 3</li>
          <li>Card 4</li>
          <li>Card 5</li>
          <li>Card 6</li>
          <li>Card 7</li>
          <li>Card 8</li>
        </ul>
      </main>
  </body>
</html>

```

css:
```css
main {
  background-color: rgb(82, 23, 81);
  padding: 10px;
  width: 1000px;
  margin: auto;
}

ul {
  background-color: rgb(250, 146, 63);
  list-style: none;
  width: 800px;
  margin: 10px auto;
  padding: 10px;
}

li {
  background-color: rgb(238, 212, 192);
  text-align: center;
  width: 200px;
  margin: 20px auto;
}

```

view:
![](attachments/Pasted%20image%2020230619044700.png)


# Adding Grid

```css
ul {
  background-color: rgb(250, 146, 63);
  list-style: none;
  width: 800px;
  margin: 10px auto;
  padding: 10px;
  display: grid;
}
```

![](attachments/Pasted%20image%2020230619045053.png)


## Adding columns
- `grid-template-columns` takes a set of width for each column. Here we specified 400px 2 times because we have an overall width of 800px
- with `fr` we can let a column take all available space (here 800px) e.g. `grid-template-columns: 1fr 200px;`, where 600px are available for column 1
	-  with more than one `fr` assigned, it will calculate each space relatively to the amount of `fr` eg: `grid-template-columns: 2fr 1fr;`
	- or `grid-template-columns: 1fr 1fr;` for the same width for each column
	- or `grid-template-columns: 30% auto;` for the first column 30% and the rest available space

```css
ul {
  background-color: rgb(250, 146, 63);
  list-style: none;
  width: 800px;
  margin: 10px auto;
  padding: 10px;
  display: grid;
  /* grid-template-columns: 400px 400px; */
  grid-template-columns: 1fr 1fr;
}
```

![](attachments/Pasted%20image%2020230619045353.png)


## Space between Grid elements
- css attribute `gap` which defines the space between each row and column

```css
ul {
  background-color: rgb(250, 146, 63);
  list-style: none;
  width: 800px;
  margin: 10px auto;
  padding: 10px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 100px 200px;
}
```

![](attachments/Pasted%20image%2020230619050339.png)


## Dynamically calculated by browser
- as many as possible with a minimum of 15rem and all the same column width

```css
ul {
  ...
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(15rem, 1fr));
  ...
}
```

# Targeting a nth item of the grid

## Targeting the first item

```css
li:first-of-type {
	background-color: yellow;
}
```
![](attachments/Pasted%20image%2020230619050722.png)

## Targeting the 3rd element

```css
li:nth-of-type(3) {
	background-color: yellow;
}
```
![](attachments/Pasted%20image%2020230619050920.png)

# Let One Element occupy a whole row
![](attachments/Pasted%20image%2020230619051427.png)

# selecting number of columns 

- from begin on column 1 (see 1 in image above) to the end of column 2 (see 3 in image above)

```css
li:nth-of-type(3) {
	background-color: yellow;
	grid-column: 1 / 3;
}
```

## specify amount of columns

- it just takes the amount of columns (from begin of column 1 (see 1 in image above) up to 2 columns)

```css
li:nth-of-type(3) {
	background-color: yellow;
	grid-column: 1 / span 2;
}
```