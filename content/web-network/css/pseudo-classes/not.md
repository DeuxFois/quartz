

# Use :not() to apply/unapply styles


Rather than adding a border to a navigation bar, and then removing it for the last element:

```css
/* add border */
.nav li {
  border-right: 2px solid #FFF;
}

/* last element */
.nav li:last-child {
  border-right: none;
}
```

Use the `:not()` pseudo-class to only apply to the elements you want:

```css
.nav li:not(:last-child) {
  border-right: 2px solid #FFF;
}
```


