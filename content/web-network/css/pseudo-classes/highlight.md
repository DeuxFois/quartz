

# Highlight input forms using :focus pseudo-class


Responsiveness can make the forms more user-friendly and easier to read.

The `:focus` pseudo-class allows us to target the form element that is clicked on. This means we can change how the input is displayed to better inform the user on what to input.  

```css
input:focus{
  background-color: red;
}
```

Pseudo-classes can be combined with classes, or other selectors, to specify different elements:

```css
.name:focus{
  background-color: red;
}

.surname:focus{
  background-color: blue;
}
```


![](Pasted%20image%2020220806160726.png)