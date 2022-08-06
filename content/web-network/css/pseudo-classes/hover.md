
# The hover Pseudo-Class



One of the most versatile and used pseudo-classes is `:hover`. Whenever an element on a website reacts to the mouse pointer being on top of it, usually it is because of the `:hover` pseudo-class matching.

In the previous insight, the example was:

```css
a:hover {
  color: yellow;
}
```

Unlike other pseudo-classes that are link-related (e.g. `:visited`) , you can use `:hover` for almost any element. *Buttons* are usually styled using this property to inform the user that by performing a click action, the webpage will respond in some way.

The drawback of this pseudo-class is that it doesn't work on most mobile devices, as the screen can't recognize a *hovering* state. Depending on the browser, the `:hover` pseudo-class might never match, match only for a very short time after touching the element or even continue lasting the user stopped touching the element.
