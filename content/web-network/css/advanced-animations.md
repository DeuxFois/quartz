

# Animations in Webkit browsers

## Content

Many Webkit browsers still use the `-webkit-prefixed` version of animations, keyframes, and transitions.

Until they fully adopt the standard version, it's good practice to include both versions (`unprefixed` & `webkit`) in your code:

```css
div {
  -webkit-animation-duration: 2s;
  animation-duration: 2s;
  -webkit-animation-name: bounceIn;
  animation-name: bounceIn;
}
```

`Autoprefixer` is a tool which calculates which prefixes are required and which are outdated. When autoprefixer adds prefixes to the code, it also fixes any differences the syntax may have.


---
# Making shapes with transform


`Rotate` certain shapes using `transform` can be used to obtain other shapes.

Applying the `transform` property on a regular square can create a diamond, for example:

```css
#diamond {
  transform: rotate(45deg);
}
```

The default rotation point is the center of the element. `transform-origin` can change that: it can take 2 (for 2D shapes) or 3 (for 3D shapes) values, as the `x` and `y` coordinates of the new rotation point.

The center of a 2D shape is:

```css
transform-origin: 50% 50%;
/* or */
transform-origin: center center;
/* for known dimensions: 500px × 20em*/
transform-origin: 250px 10em;
```

Other values :

```css
transform-origin: top left;
transform-origin: right bottom;
transform-origin: left center;
```

This is the default rotation around its center:

![diamondtransform.svg](https://img.enkipro.com/afacbc50fedc953093d5d1aba5b4d385.png)

# Manipulating shapes using border


You can make shapes by defining the properties of the `border`, as seen below:

```css
#triangle {
  width: 0;
  height: 0;
  border-left: 80px solid white;
  border-right: 80px solid transparent;
  border-top: 80px solid transparent;
  border-bottom: 80px solid transparent;
}
```

By setting the `background` of three `borders` to be `transparent`, the shape of a triangle is mimicked.

For the following image, these are the borders that are not transparent:

- **A** : *border-left* (code snippet above)
- **B** : *border-right*
- **C** : *border-top*
- **D** : *border-bottom*

![HtmlToSvg.svg](https://img.enkipro.com/34ca2aafa9de4ed519daa02ad9052127.png)

# Manipulating shapes using clip-path


You can use functions that are applied to `shape-outside` to `clip-path`, ie, `inset()`, `polygon()`, `ellipse()`.
Below is an example of `clip-path` in action:

```css
#element {
  width: 300px;
  height: 150px;
  clip-path:
    polygon(0% 0%, 100% 100%, 0% 100%);
}
```

The list separated by commas define the *(x,y)* coordinates for each point. Translated into *px*, its equivalent would be `polygon(0px 0px, 300px 150px, 0px 150px)`. Three points will determine a triangle.

![clippathtriangle.svg](https://img.enkipro.com/9b9a4914d020ad42c618f59d8fe30abf.png)

If you want text to wrap around a shape, you have to combine `clip-path` with the `shape-outside` property.

> 💡 Top-left corner of the `element` is located at `(0%,0%)`, while the bottom-right one at `(100%, 100%)`.

Take a look at these two square Enki logos:

![manipulating-shapes-enki-logo-example](https://img.enkipro.com/3dc5e3eea131276d0e75a779141a2fc9.png)

Here's the HTML code:
```html
<!-- Regular Enki Logo -->
<img src="https://img.enkipro.com/3369b724e5749ae19442e4677362c1e8.png">

<!-- Enki Logo with a class called "clipped" -->
<img class="clipped" src="https://img.enkipro.com/3369b724e5749ae19442e4677362c1e8.png">
```

Let's see what happens when we use `ellipse` instead of `polygon`:
```css
.clipped { 
  clip-path: ellipse(65px 30px at 125px 40px);
}
```
![enki-image-output-for-manipulating-shapes](https://img.enkipro.com/a50732d668d8f66212f89a8420942551.png)

# Manipulating shapes using shape-outside


You can wrap pieces of text around shapes using `shape-outside`. As of `CSS3`, this property only works on elements that have the property of `float` applied upon it.

Example:

```css
#element {
  float: left;
  shape-outside: circle(50%);
  width: 100px;
  height: 100px;
}
```

This is the visual equivalent of the above snippet:

![shapeoutsidecircle.svg](https://img.enkipro.com/bf10605b36534f1e04e0fd2e3d2972a7.png)

Other functions instead of `circle()` include: `ellipse()`, `polygon()`, `inset()`.

However, this property is not supported by IE and does not change the actual shape of the element.

# Pause and play CSS animations


You can *pause* and *play* CSS animation by changing its `animation-play-state` property.

Setting it to *paused* stops your animation in place, until you change `animation-play-state` to running, for example on hover.

```css
.animating_thing {
    animation: spin 10s linear infinite;
    animation-play-state: paused;
}
.animating_thing:hover {
    animation-play-state: running;
}
```
