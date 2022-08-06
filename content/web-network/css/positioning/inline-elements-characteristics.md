

# Inline Elements Characteristics

An inline element has the following characteristics:

- It begins on the same line as its siblings
- Its `height`, `line-height`, `top-margin` and `bottom-margin` can't be changed
- Its width is as wide as the content and can't be modified

Examples of inline elements include `<span>`, `<a>`, `<label>`, `<input>`, `<img>`, `<strong>` and `<em>`.

Example of useless CSS:

```css
span {
  height: 20px; /* no effect! */
  top: 20px; /* no effect! */
}

```

