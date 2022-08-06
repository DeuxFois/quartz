

# Styling elements with ::before & ::after


Use the `::before` selector to add and style content just before the first child of an element.
Similarly, use the `::after` selector to add and style content after the last child of the element.

Consider the following HTML code:

```html
  <p>First</p>
  <p>Second</p>
  <p>Third</p>
```

And the following CSS snippet:

```css
  p::before{
    content: '#';
    color: red;
  }

  p::after{
    content: '?';
    color: aqua;
  }
```

This adds a red **#** at the start of every `p` element and a blue **?** at the end of them.

![HtmlToSvgmin.svg](https://img.enkipro.com/04042139dfbb5bbe310b0eba0b903359.png)

`content` is mandatory to display the element but can be empty (`content:"";`).

Both `::before` and `::after` can be used to display shapes, images or even borders.

[codepen.io](http://codepen.io/anon/pen/MKgrXB)


