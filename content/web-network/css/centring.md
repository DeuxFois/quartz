# Horizontal centering fluid blocks

To horizontally center blocks of unknown width:

```css
#horizontal-center {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
```

The `left` property relates to the size of the parent so the left of child will be in the middle. The `transform` property relates to the child size. Hence it will be centered.

You can check out the evolution here:

![HtmlToSvg.svg](https://img.enkipro.com/ff255695f9032f0cff8fb0417b180705.png)

# Vertical centering fluid blocks

To vertically center blocks of unknown height:

```css
.elem {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
```

This is how the properties would position the element:

![HtmlToSvg.svg](https://img.enkipro.com/26ef6523907e513af1de985571a360cf.png)

The `top` property relates to the size of `.elem`'s parent. Setting it to 50% means the top of `.elem` will be in the middle. The `transform` property relates to the size of `.elem` itself.   

Therefore moving it up by 50% of its height, means the middle of `.elem` will be in the middle of its parent element.



# Horizontal centering with margin: 0 auto;


To horizontally center **block elements** of a known width:

```css
#horizontal-center {
  width: 960px;
  margin: 0 auto;
}
```

The 2 value shorthand notation of `margin` targets `top/bottom-margins` and `left/right-margins`. Because the element is **block**, it occupies the whole width of the container.

The element also has a known width, so the browser can calculate the container's unused width. The `auto` value tells the browser to equally distantiate the block element from the left and the right margin, which effectively centers it horizontally.



# Vertical centering with margin-top

To vertically center block elements of known height:

```css
.vertical-center {
  position: absolute;
  top: 50%;
  height: 400px;
  margin-top: -200px;
}
```

You can check out the evolution here:

![HtmlToSvg.svg](https://img.enkipro.com/3179ba1e9d0b35a33abec9b3d20ce4d5.png)

`top: 50%` will put the top of the element in the center of its parent. `margin-top: -200px` will make the content of the element start 200px before its top. By combining both, we manage to center an element of known height.



# Vertically-center anything


Use `flexbox` to center anything vertically:

```css
html, body {
  height: 100%;
  margin: 0;
}

body {
  align-items: center;
  display: flex;
}
```

There may be some unwanted behaviour in IE11.
