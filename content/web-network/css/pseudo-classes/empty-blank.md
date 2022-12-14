
# :empty and :blank



With `:empty` you can select an element based on there being no children in it, whether that be elements, text nodes, or even white space nodes. So with `:empty`, even if the element contains a single space and nothing else, it will not be considered “empty”.

The `:blank` pseudo-class, however, will select an element as long as it has no text and no other child elements, regardless of white space. So it could contain white space, line breaks, etc., and it would still qualify.

Example:

HTML:

```html
<p></p>
<p> </p>

```

CSS:

```css
p:blank {
  outline: solid 1px red;
}

p:empty {
  border: solid 1px green;
}

```

The `:empty` pseudo-class will select only the first element, because it’s completely empty. But the `:blank` pseudo-class will apply to both, because they are both “blank” with respects to text and elements.

