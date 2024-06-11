# Using the Decimal type in JavaScript

Some programming languages provide a built-in Decimal type to represent non-repeating decimal fractions like 0.3 and -1.17 without rounding. This means that developers can perform arithmetic operations accurately on these fractions.

Unfortunately, JavaScript does not currently have a built-in Decimal type.

Fortunately, there's a library called [decimal.js](https://mikemcl.github.io/decimal.js/) that provides just that. It also comes with arithmetic methods, e.g. sum, div, log.

## Original code (without Decimal type)

```js
console.log(0.1 + 0.2); // outputs 0.30000000000000004
```

## New code (with Decimal type)

```js
let decimalSum = Decimal.sum(0.1, 0.2);
let numericSum = decimalSum.toNumber();

console.log(numericSum); // outputs 0.3
```

When dealing with any numeric calculation in JavaScript, especially money-related, it's safer to use decimal.js not to get any unexpected result.
