# Destructuring arrays by index in JavaScript

Destructuring is often used as a shorthand to retrieve specific elements from arrays and objects.

## Original code (array destructuring)

```js
const fruits = ["apple", "banana", "cherry"];
const [firstFruit, secondFruit, thirdFruit] = fruits;
console.log(thirdFruit); // "cherry"
```

In the above example, `"cherry"` gets stored in variable `thirdFruit`. However, variables `firstFruit` & `secondFruit` are never used, which means we're wasting memory.

## New code (object destructuring)

```js
const fruits = ["apple", "banana", "cherry"];
const { 2: thirdFruit } = fruits;
console.log(thirdFruit); // "cherry"
```

Destructuring the array as an object does not waste memory on usused variables. This works because under the hood, JavaScript stores arrays as objects, where the keys are the indices and values are the actual array elements, i.e. fruits is stored as follows:

```js
{
  0: "apple",
  1: "banana",
  2: "cherry"
}
```
