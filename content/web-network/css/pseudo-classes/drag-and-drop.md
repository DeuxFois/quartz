# Drag-and-Drop pseudo-class :drop



The `:drop` selector allows styling of the drop zone (the place where the element is supposed to be dropped), during the time when the user is dragging (or carrying) the element to be dropped.

```css
.spot {
  background: #ccc;
}

.spot:drop {
  background: hotpink;
}

```

The above CSS will apply a neutral gray background color to the .spot element when the user is not dragging. But when the user starts dragging to the `.spot` element, the background color will change and stay that way until the item is dropped.
