

# Clearfix for layouts


Use `clearfix` to make an element automatically clear its child elements, so there would be no need for additional markup :

```css
.clearfix:after {
  content: "";
  clear: both;
}
```

This hack is useful in cases like where `float` is used to arrange elements one after the other. Because of how `floats` work (they make other elements wrap around them), their container won't resize to surround them.

Here is an example, where the left child has `float:left` property, and the right one, `float:right`:

![newclearfix.svg](https://img.enkipro.com/f547238149eddde20aafdd25e528d22f.png)

All you have to do is add `clearfix` class to the container and the floating element:

```html
<div class="clearfix">
   <div style="float: left;"
   class="clearfix">Sidebar</div>
</div>
```

The hack forces the content after the floats to render below them (`clear` property specifies on which side of the element floating elements are not allowed).

Now, the parent will resize itself to surround the floating children.

