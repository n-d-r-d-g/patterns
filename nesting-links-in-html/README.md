# Nesting links in HTML

According to the official [HTML standard](https://html.spec.whatwg.org/multipage/text-level-semantics.html#the-a-element) (under the `Content model` section), an `a` element cannot be nested inside another `a` element.

A common use case where I need to nest links is cards that need to redirect the user somewhere and these cards contain other links inside.

## Original code (invalid HTML)

```html
<a href="/card-link">
  <p>Card title</p>
  <p>Card description</p>
  <a href="/nested-link">Some nested link</a>
</a>
```

The above code is invalid HTML. A common approach is to use CSS absolute positioning to avoid nesting `a` elements.

## New code (valid HTML)

```html
<div class="card-container">
  <a href="/card-link" class="card-container-link"></a>
  <p>Card title</p>
  <p>Card description</p>
  <a href="/nested-link">Some nested link</a>
</div>
```

```css
.card-container {
  position: relative;
}

.card-container-link {
  position: absolute;
  inset: 0;
}
```

The above solution requires more code than the original one, but it is valid HTML and still respects accessibility guidelines.

**P.S. Credit goes to [MrSunshyne](https://github.com/MrSunshyne) for showing me this approach.**
