# Concatenating compound fields in JavaScript

A compound field is a group of related fields. E.g:

- The `Name` compound field can be made up of the following fields: `Salutation`, `First name`, `Other names` & `Last name`.
- The `Address` compound field can be made up of the following fields: `Building number`, `Street name`, `City` & `Country`.

Compound fields are often displayed by concatenating the related fields with either a space or comma in between.

```js
const salutation = "Mrs.";
const firstName = "Jane";
const otherNames = "Anne Marie";
const lastName = "Foster";
const name = [salutation, firstName, otherNames, lastName]
  .filter(Boolean)
  .join(" ");

console.log(name); // Mrs. Jane Anne Marie Foster
```

In the above code, `filter(Boolean)` filters out falsy values in the array. E.g. if `otherNames` was not provided, `name` would still be displayed as expected.

```js
const salutation = "Mrs.";
const firstName = "Jane";
const otherNames = ""; // could be any falsy value
const lastName = "Foster";
const name = [salutation, firstName, otherNames, lastName]
  .filter(Boolean)
  .join(" ");

console.log(name); // Mrs. Jane Foster
```
