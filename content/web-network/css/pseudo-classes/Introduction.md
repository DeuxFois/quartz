# Required and optional pseudo classes


Especially when creating a form, some fields in it are mandatory for user to complete.

All modern browsers support the `:required` and `:optional` pseudo classes:

```css
:required {
  border: 2px solid red;
}

:optional {
  border: 2px solid blue;
}

```

An example of a form they can be applied on:

```html
<form>
  <label>Name:</label>
  <input type="text"/>
  <br />
  <label>Username:</label>
  <input type="text" required/>
  <br/>
  <input type="submit"
              value="Submit">
</form>
```

`:required` and :`optional` can be chained together with other pseudo class selectors:

```css
input:required:focus {
  border: 1px solid pink;
  outline: none;
}

```

*Note*: Any element that doesn't have the `required` attribute is considered `optional`.


