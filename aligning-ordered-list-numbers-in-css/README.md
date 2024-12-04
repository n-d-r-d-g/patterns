# Aligning ordered list numbers in CSS

`<ol>` comes with automatic numbering right out of the box, without any styling required.

However, the items are not vertically aligned. Even if `list-style-position: outside;` can address this, we still need to hard code a `padding-inline-start` on the `<ol>`.

One solution is to combine `subgrid` and `counter`.

```html
<ol>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
  <li>Item 4</li>
  <li>Item 5</li>
  <li>Item 6</li>
  <li>Item 7</li>
  <li>Item 8</li>
  <li>Item 9</li>
  <li>Item 10</li>
</ol>
```

```css
ol {
  padding: 0;
  display: grid;
  grid-template-columns: auto 1fr;
  counter-reset: list-counter;
}

li {
  grid-column: span 2;
  display: grid;
  grid-template-columns: subgrid;
  gap: 1ch;
}

li::before {
  content: counter(list-counter) ".";
  display: inline-block;
  text-align: end;
  counter-increment: list-counter;
}
```

The `counter()` function accepts a second argument, which determines the list style type, e.g. `counter(list-counter, lower-alpha)` displays lowercase alphabets instead of numbers. Check out more list style types on [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/list-style-type#values) or [W3C](https://www.w3.org/TR/css-counter-styles-3/#predefined-counters).
