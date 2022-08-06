# Animation basics in CSS


There are two main properties when it comes to animating : `animation` and `keyframes`.

`animation` : how an element should transition (duration, speed).

`keyframes` : the actions an element should follow throughout the animation.

`animation-delay`  : delays the execution of an animation by a specified amount of time.

Sample code of a circle moving inside a square:  

```css
#ball {
position: relative;
animation: ball 4s linear infinite;
animation-delay: 1s;
}
@keyframes ball {
    0% { top: 50px; left: 50px; }
    25% { top: 50px; left: 200px; }
    50% { top: 200px; left: 200px; }
    75% { top: 200px; left: 50px; }
    100% { top: 50px; left: 50px; }
}
```

*1s* is necessary for the ball to get from any point to the next. Because of the `infinite` value, the animation will not stop by itself.

![animationsvgmin.svg](https://img.enkipro.com/f3391cbe5ba0db6aab629bfd8a191e7a.png)

You can also *pause* and *play* CSS animation by changing its `animation-play-state` property.

Setting it to *paused* stops your animation in place, until you change `animation-play-state` to running, for example on hover.

```css
#ball {
    animation: spin 10s linear infinite;
    animation-play-state: paused;
}
#ball {
    animation-play-state: running;
}
```


# Shorthand Transitions


Declaring each transition property individually can be intensive, especially using `vendor prefixes`.

Instead, try the shorthand property `transition` that supports all the different values.

Using only this alone it is possible to set every transition value in order of `transition-property`, `transition-duration`, `transition-timing-function`, and lastly `transition-delay`.

```css
.box {
  background: #2db34a;
  border-radius: 6px;
  transition: background .2s linear;
}
.box:hover {
  background: #ff7b29;
  border-radius: 50%;
}
```

For setting many transitions at once, set every individual group of transition values, then use a comma to separate each group of additional ones.