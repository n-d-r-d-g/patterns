# Inline CSS variables in Tailwind CSS

CSS variables can be a powerful tool to achieve smoother code maintainability. Tailwind CSS enables the use of inline CSS variables.

**Check out a preview of the code blocks below, [here](https://play.tailwindcss.com/tMP9ZZvFHg).**

```html
<section
  class="relative before:absolute before:left-0 before:top-0 before:h-full before:w-[2rem] before:bg-gradient-to-r before:from-white before:content-[''] after:absolute after:right-0 after:top-0 after:h-full after:w-[2rem] after:bg-gradient-to-l after:from-white after:content-['']"
>
  <p class="overflow-x-auto text-nowrap px-[2rem] py-3">
    Lorem Ipsum is simply dummy text of the printing and typesetting industry.
    Lorem Ipsum has been the industry's standard dummy text ever since the
    1500s, when an unknown printer took a galley of type and scrambled it to
    make a type specimen book. It has survived not only five centuries, but also
    the leap into electronic typesetting, remaining essentially unchanged.
  </p>
</section>
```

The above code block features a paragraph of text that scrolls horizontally. Pseudo elements are absolutely positioned on the left and right of the text to produce a fade effect on scroll. We need the horizontal padding of the `p` element to match the width of the pseudo elements, i.e. `2rem`. However, this value has been hard coded in 3 places. That's not great for maintainability.

```tsx
<section class="relative [--offset:2rem] before:absolute before:left-0 before:top-0 before:h-full before:w-[--offset] before:bg-gradient-to-r before:from-white before:content-[''] after:absolute after:right-0 after:top-0 after:h-full after:w-[--offset] after:bg-gradient-to-l after:from-white after:content-['']">
  <p class="overflow-x-auto text-nowrap px-[--offset] py-3">
    Lorem Ipsum is simply dummy text of the printing and typesetting industry.
    Lorem Ipsum has been the industry's standard dummy text ever since the
    1500s, when an unknown printer took a galley of type and scrambled it to
    make a type specimen book. It has survived not only five centuries, but also
    the leap into electronic typesetting, remaining essentially unchanged.
  </p>
</section>
```

We can DRY this out through the use of [arbitrary properties](https://tailwindcss.com/docs/adding-custom-styles#arbitrary-properties) in Tailwind CSS. In the above code block, the `offset` CSS variable is created in the `section` element, then referenced by the pseudo elements and the `p` element.
