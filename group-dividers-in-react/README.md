# Group dividers in React

In JavaScript, adding a character between elements in an array is trivial.

```js
["kebab", "case", "magic"].join("-");
// kebab-case-magic
```

We can achieve the same behavior when dealing with grouped array elements.

```js
[[1, 2], [3], [4, 5, 6]].join("##");
// "1,2##3##4,5,6"
```

Notice, however, the return value is a string and a comma is automatically injected between elements in nested arrays.

`reduce` would be better suited than `join`.

```js
[[1, 2], [3], [4, 5, 6]].reduce((previousContent, element, index) => {
  if (index === 0) return element;

  return [...previousContent, "##", ...element];
}, []);
// [ 1, 2, "##", 3, "##", 4, 5, 6 ]
```

The same logic can be used for JSX, say a menu with grouped items.

```tsx
[
  [<Item1 key="item-1" />, <Item2 key="item-2" />],
  [<Item3 key="item-3" />],
  [<Item4 key="item-4" />, <Item5 key="item-5" />, <Item6 key="item-6" />],
].reduce((previousContent, group, index) => {
  if (index === 0) return group;

  return [...previousContent, <Divider key={`divider-${index}`} />, ...group];
}, []);
/*
[
  [<Item1 key="item-1"/>, <Item2 key="item-2"/>],
  <Divider key="divider-1"/>,
  [<Item3 key="item-3"/>],
  <Divider key="divider-2"/>,
  [<Item4 key="item-4"/>, <Item5 key="item-5"/>, <Item6 key="item-6"/>]
]
*/
```

One issue is in case the items are conditionally rendered, dividers will be added unnecessarily if groups are empty.

```tsx
const showItem1 = false;
const showItem2 = false;
const showItem3 = false;
const showItem4 = false;
const showItem5 = true;
const showItem6 = true;

return [
  [showItem1 && <Item1 key="item-1" />, showItem2 && <Item2 key="item-2" />],
  [showItem3 && <Item3 key="item-3" />],
  [
    showItem4 && <Item4 key="item-4" />,
    showItem5 && <Item5 key="item-5" />,
    showItem6 && <Item6 key="item-6" />,
  ],
].reduce((previousContent, group, index) => {
  if (index === 0) return group;

  return [...previousContent, <Divider key={`divider-${index}`} />, ...group];
}, []);
/*
[
  [false, false],
  <Divider key="divider-1"/>,
  [false],
  <Divider key="divider-2"/>,
  [false, <Item5 key="item-5"/>, <Item6 key="item-6"/>]
]
*/
```

The following logic works better in this case.

```tsx
const showItem1 = false;
const showItem2 = false;
const showItem3 = false;
const showItem4 = false;
const showItem5 = true;
const showItem6 = true;

return [
  [showItem1 && <Item1 key="item-1" />, showItem2 && <Item2 key="item-2" />],
  [showItem3 && <Item3 key="item-3" />],
  [
    showItem4 && <Item4 key="item-4" />,
    showItem5 && <Item5 key="item-5" />,
    showItem6 && <Item6 key="item-6" />,
  ],
].reduce((previousContent, group, index) => {
  const sanitizedGroup = group.filter(Boolean);

  if (sanitizedGroup.length === 0) return previousContent;

  if (previousContent.length === 0) return sanitizedGroup;

  return [...acc, <Divider key={`divider-${index}`} />, ...sanitizedGroup];
}, []);
```

The boolean flags, i.e. showItem1, showItem2, etc. will not work inside the Item components as they will be computed at runtime, therefore at compile time, the array will contain a JSX element at every position, regardless of their return value.

```tsx
// Item1.tsx
export const Item1 = () => {
  const showItem1 = false;

  if (showItem1) return null;

  return <p>Item 1</p>;
};

// App.tsx
console.log([<Item1 />]);
// [{type: f Item1() {}, ...}]
```
