
# Matches-any pseudo-class :is


The `:is` pseudo-class allows the application of rules to groups of selectors.

```css
p:is(.alert,.error,.warn){
    color:red;
}
```

The above will make the text of all elements matching `.alert`, `.error` and `.warn` red.

It could be used to apply a particular style rule to a similar element group.

> 💡 `:is()` used to be called `:matches()` but was renamed. However, older browser versions still support it to provide backwards compatibility.

