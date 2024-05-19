# Using object `getters` in JavaScript

An object getter can be used to define a `readonly` property on an object. It has another nice caveat to it: it can access other properties on the same object.

I found a use case where this can be really useful.

## Original code (without getters)

```js
const seoMetaData = {
  title: "A blog article's title",
  description: "A blog article's description",
  ogImage: "https://dummyimages.com/image1",
  twitterCard: "summary_large_image",
  ogTitle: "A blog article's title",
  twitterTitle: "A blog article's title",
  ogDescription: "A blog article's description",
  twitterDescription: "A blog article's description",
  ogImageUrl: "https://dummyimages.com/image1",
  twitterImage: "https://dummyimages.com/image1",
};
```

Notice that the following properties contain duplicate values:

- `title`, `ogTitle` & `twitterTitle`
- `description`, `ogDescription` & `twitterDescription`
- `ogImage`, `ogImageUrl` & `twitterImage`

## New code (with getters)

```js
const seoMetaData = {
  title: "A blog article's title",
  description: "A blog article's description",
  ogImage: "https://dummy.test/image1",
  twitterCard: "summary_large_image",
  get ogTitle() {
    return this.title;
  },
  get twitterTitle() {
    return this.title;
  },
  get ogDescription() {
    return this.description;
  },
  get twitterDescription() {
    return this.description;
  },
  get ogImageUrl() {
    return this.ogImage;
  },
  get twitterImage() {
    return this.ogImage;
  },
};
```

Using getters makes the code a little chunkier, however, now you can rest assured whenever someone needs to change a regular property's value, e.g. change `ogImage`'s value to "https://dummy.test/image2". `ogImageUrl` and `twitterImage` will always reference `ogImage`'s current value.

Another plus point is you can still access getters just like regular properties, e.g. `seoMetaData.twitterImage` (without round brackets).
