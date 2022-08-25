## Using Grid
```css
#center {
  display: grid;
  place-content: center;
}
```
## Using flexbox
Use `flexbox` to center anything vertically:

```css
#center-parent {
  align-items: center;
  display: flex;
}
```

There may be some unwanted behaviour in IE11.

## Using translation

```css
#center {
  position: absolute;
  top: 50%; 
  left: 50%; 
  transform: translate(-50%, -50%);
}
```




## Horizontal-centering with margin



To horizontally center **block elements** 
```css
#horizontal-center {
  margin: auto;
}
```

