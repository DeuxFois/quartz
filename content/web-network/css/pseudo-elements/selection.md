

# Change selected area color



Highlighted text area colors can be easily change with the `::selection` pseudo element.

Apply `::selection` on a paragraph:

```css
p::selection {
  background: black;
  color: white;
}
```

Gecko is the only engine requiring the prefix, so adding an other rule is required to support all browsers :

```css
p::-moz-selection {
  background: black;
  color: white;
}
```

