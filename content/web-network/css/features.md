# Use SVG for icons

## Content

Since `SVG` (Scalable Vector Graphics) scales well for all resolution types, it is useful for icons:

```css
.logo {
  background: url("logo.svg");
}
```

`SVG` is supported in all browsers back to IE9.

# At-Rules @ 

## Content

The at-rule (`@`) informs CSS with instructions on how to behave.

The regular form for `@` is:

```css
@[KEYWORD] (RULE);
```

Regular @keywords include: `charset`, `import`, and `namespace`.

The nested form for `@` is:

```css
@[KEYWORD] {
/* Nested Statements */
};
```

Nested `@keywords` include: `document`, `font-face`, `keyframes`, `media`, `page`, and `supports`.

These are useful when you want to check if the browser supports a property:

```css
@supports (image-rendering and
    -moz-crisp-edges) {
 /* CSS code if supported*/
image-rendering:
   -webkit-optimize-contrast;
}
```

Or you want different image scaling depending on screen size:

```css
@media screen and (max-width:720px){
  .thumbnail{
    width: 10vh;
    height: 10vh;
  }
}

@media screen and (min-width: 720px){
  .thumbnail{
    width: 15vh;
    height: 15vh;
  }
}
```



# Simpler maths with calc()

## Content

For styling a 7-column grid you may use something like :

```css
.column-1-7 {
   width: 14.2857%;
}
.column-2-7 {
   width: 28.5714%;
}
.column-3-7 {
   width: 42.8571%;
}
```

Use the calc() function instead to make the maths behind the layout easier to understand :

```css
.column-1-7 {
   width: calc(100% / 7);
}
.column-2-7 {
   width: calc(100% / 7 * 2);
}
.column-3-7 {
   width: calc(100% / 7 * 3);
}
```

You can also mix units!

```css
.mixing {
   width: calc(50% + 30px);
}

```



# Combining selectors

## Content

Two selectors can be combined to refer to a certain element. The most common ones are '+' and '~'.  

### X+Y

```css
ul + p {
   color: orangered;
}

```

This is referred to as *an adjacent selector*. It will select only the element that is **immediately preceded** by the former element. In this case, only the first paragraph after each `ul` will have red text.

### X~Y

```css
ul ~ p {
   font-weight: bold;
}

```

This sibling combinator is similar to `X + Y`, however, it's less strict. While an adjacent selector `(ul + p)` will only select the first element that is immediately preceded by the former selector, this one is more generalized. It will select, referring to our example above, any p elements, as long as they follow a `ul`.

Consider the HTML code:

```html
<ul>
 <li>First list item.</li>
 <p> A paragraph nested inside the
                 list.</p>
</ul>
<p>A paragraph immediately after ul.</p>
<p>A second paragraph after ul.</p>
```

Both of the selectors are used in this example:

![HtmlToSvgmin.svg](https://img.enkipro.com/55077de02fb561f44f83e976dba488a8.png)


---

