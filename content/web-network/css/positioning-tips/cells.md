
# Controlling cellpadding and cellspacing in CSS


Cellpadding refers to the space between the cell content and the cell wall, while, cellspacing refers to the space between table cells.

To control the "cellpadding" in CSS, apply `padding` on table cells, for example:

```css
td {
  padding: 10px;
}
```

To control the "cellspacing" in CSS, apply `border-spacing` and `border-collapse` to the table, for example:

```css
table {
  border-spacing: 10px;
  border-collapse: separate;
}
```

This is how the table would look with all the above mentioned properties:

![HtmlToSvg.svg](https://img.enkipro.com/5cd4ebde7bebedb1168c64d4f3d8ee61.png)

