#### flex-flow

This is a shorthand for the `flex-direction` and `flex-wrap` properties, which together define the flex container’s main and cross axes. The default value is `row nowrap`.

```css
.container {
  flex-flow: column wrap;
}
```
#### flex-grow

![two rows of items, the first has all equally-sized items with equal flex-grow numbers, the second with the center item at twice the width because its value is 2 instead of 1.|500](https://css-tricks.com/wp-content/uploads/2018/10/flex-grow.svg)

  
This defines the ability for a flex item to grow if necessary. It accepts a unitless value that serves as a proportion. It dictates what amount of the available space inside the flex container the item should take up.

If all items have `flex-grow` set to `1`, the remaining space in the container will be distributed equally to all children. If one of the children has a value of `2`, that child would take up twice as much of the space either one of the others (or it will try, at least).

```css
.item {
  flex-grow: 4; /* default 0 */
}
```
#### gap, row-gap, column-gap

![|500](https://css-tricks.com/wp-content/uploads/2021/09/gap-1.svg)
#### justify-content

![flex items within a flex container demonstrating the different spacing options|500](https://css-tricks.com/wp-content/uploads/2018/10/justify-content.svg)
#### align-content

![examples of the align-content property where a group of items cluster at the top or bottom, or stretch out to fill the space, or have spacing.|500](https://css-tricks.com/wp-content/uploads/2018/10/align-content.svg)