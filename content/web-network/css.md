[centring a div](web-network/css/centring.md)

[flexbox](web-network/css/flexbox.md)

[pseudo-classes](web-network/css/pseudo-classes.md)


### Selectors

| Selector          | Description  |
| ----------------- | ------------ |
| `*`               | All elements |
| `div`             | Element      |
| `.class`          | Class        |
| `#id`             | ID           |
| `[disabled]`      | Attribute    |
| `[role="dialog"]` | Attribute    |

### Combinators
| Selector            | Description       |
| ------------------- | ----------------- |
| `.parent .child`    | Descendant        |
| `.parent > .child`  | Direct descendant |
| `.child + .sibling` | Adjacent sibling  |
| `.child ~ .sibling` | Far sibling       |
| `.class1.class2`    | Have both classes |

### Attribute selectors

| Selector           | Description                         |
| ------------------ | ----------------------------------- |
| `[role="dialog"]`  | `=` Exact                           |
| `[class~="box"]`   | `~=` Has word                       |
| `[class|="box"]`   | `|=` Exact or prefix (eg, `value-`) |
| `[href$=".doc"]`   | `$=` Ends in                        |
| `[href^="/index"]` | `^=` Begins with                    |
| `[class*="-is-"]`  | `*=` Contains                       |

### Pseudo-classes

| Selector             | Description                                |
| -------------------- | ------------------------------------------ |
| `:target`            | eg, `h2#foo:target`                        |
| ---                  | ---                                        |
| `:disabled`          |                                            |
| `:focus`             |                                            |
| `:active`            |                                            |
| ---                  | ---                                        |
| `:nth-child(3)`      | 3rd child                                  |
| `:nth-child(3n+2)`   | 2nd child in groups of 3                   |
| `:nth-child(-n+4)`   |                                            |
| ---                  | ---                                        |
| `:nth-last-child(2)` |                                            |
| `:nth-of-type(2)`    |                                            |
| ---                  | ---                                        |
| `:checked`           | Checked inputs                             |
| `:disabled`          | Disabled elements                          |
| `:default`           | Default element in a group                 |
| ---                  | ---                                        |
| `:empty`             | Elements without children                  |

### Pseudo-class variations

| Selector          |
| ----------------- |
| `:first-of-type`  |
| `:last-of-type`   |
| `:nth-of-type(2)` |
| `:only-of-type`   |
| ---               |
| `:first-child`    |
| `:last-child`     |
| `:nth-child(2)`   |
| `:only-child`     |

## Fonts

### Properties

| Property           | Description                          |
| ------------------ | ------------------------------------ |
| `font-family:`     | `<font>, <fontN>`                    |
| `font-size:`       | `<size>`                             |
| `letter-spacing:`  | `<size>`                             |
| `line-height:`     | `<number>`                           |
| ---                | ---                                  |
| `font-weight:`     | `bold` `normal`                      |
| `font-style:`      | `italic` `normal`                    |
| `text-decoration:` | `underline` `none`                   |
| ---                | ---                                  |
| `text-align:`      | `left` `right` `center` `justify`    |
| `text-transform:`  | `capitalize` `uppercase` `lowercase` |


### Shorthand

|         | style    | weight | size (required) |     | line-height | family            |
| ------- | -------- | ------ | --------------- | --- | ----------- | ----------------- |
| `font:` | `italic` | `400`  | `14px`          | `/` | `1.5`       | `sans-serif`      |
|         | style    | weight | size (required) |     | line-height | family (required) |

## Background
### Properties

| Property                 | Description                              |
| ------------------------ | ---------------------------------------- |
| `background:`            | _(Shorthand)_                            |
| ---                      | ---                                      |
| `background-color:`      | `<color>`                                |
| `background-image:`      | `url(...)`                               |
| `background-position:`   | `left/center/right` `top/center/bottom`  |
| `background-size:`       | `cover` `X Y`                            |
| `background-clip:`       | `border-box` `padding-box` `content-box` |
| `background-repeat:`     | `no-repeat` `repeat-x` `repeat-y`        |
| `background-attachment:` | `scroll` `fixed` `local`                 |

### Shorthand

|               | color  | image         | positionX | positionY |     | size           | repeat      | attachment |
| ------------- | ------ | ------------- | --------- | --------- | --- | -------------- | ----------- | ---------- |
| `background:` | `#ff0` | `url(bg.jpg)` | `left`    | `top`     | `/` | `100px` `auto` | `no-repeat` | `fixed;`   |
| `background:` | `#abc` | `url(bg.png)` | `center`  | `center`  | `/` | `cover`        | `repeat-x`  | `local;`   |
|               | color  | image         | positionX | positionY |     | size           | repeat      | attachment |

### Multiple backgrounds

```css
background: linear-gradient(to bottom, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)),
  url('background.jpg') center center / cover, #333;
```

## Animation

### Properties

| Property                     | Value                                                    |
| ---------------------------- | -------------------------------------------------------- |
| `animation:`                 | _(shorthand)_                                            |
| `animation-name:`            | `<name>`                                                 |
| `animation-duration:`        | `<time>ms`                                               |
| `animation-timing-function:` | `ease` `linear` `ease-in` `ease-out` `ease-in-out`       |
| `animation-delay:`           | `<time>ms`                                               |
| `animation-iteration-count:` | `infinite` `<number>`                                    |
| `animation-direction:`       | `normal` `reverse` `alternate` `alternate-reverse`       |
| `animation-fill-mode:`       | `none` `forwards` `backwards` `both` `initial` `inherit` |
| `animation-play-state:`      | `normal` `reverse` `alternate` `alternate-reverse`       |


### Shorthand

|              | name     | duration | timing-function | delay   | count      | direction           | fill-mode | play-state |
| ------------ | -------- | -------- | --------------- | ------- | ---------- | ------------------- | --------- | ---------- |
| `animation:` | `bounce` | `300ms`  | `linear`        | `100ms` | `infinite` | `alternate-reverse` | `both`    | `reverse`  |
|              | name     | duration | timing-function | delay   | count      | direction           | fill-mode | play-state |


### View more
[animations](web-network/css/animations.md)  
[advanced-animations](web-network/css/advanced-animations.md)