# Range function in JavaScript

There's no native function in JavaScript to generate a range of values. The code below is one way to solve this with a function that returns an iterable.

```js
function range(start, end, step = 1) {
  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      const done = start > end;
      const value = done ? end : start;

      start += step;

      return { done, value };
    },
  };
}
```

Since the `range` function returns an iterable, you can perform operations such as iteration and destructuring on it:

```js
for (const num of range(50, 100, 10)) {
  console.log(num);
} // 50 60 70 80 90 100

console.log([...range(3, 9, 3)]); // [3, 6, 9]
```

**P.S. I took inspiration from [Jeff Delaney's video](https://fireship.io/lessons/typescript-design-patterns/) to write this document.**
