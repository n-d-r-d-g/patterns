# Alternative to Enums in TypeScript

**Enums** in TypeScript are often used to define a set of named constants [(TypeScriptLang Handbook, 2024)](https://www.typescriptlang.org/docs/handbook/enums.html).

> [! NOTE]
> JavaScript does not have a native implementation of `Enums` like C# or other languages, as `Objects` already exist.

While useful in certain scenarios, enums have notable drawbacks:
* **Runtime overhead:** Enums generate additional code after compilation.
* **Flexibility issues:** They may not integrate well with some patterns or libraries.

Even [Anders Hejlsberg](https://www.youtube.com/watch?v=vBJF0cJ_3G0&t=1012s), the lead architect of TypeScript, has expressed reservations about enums.

A cleaner alternative leverages `as const` and `typeof` for better performance and flexibility.

## Original code (using `enum` )

```typescript
enum Status {
    isLoading = "isLoading",
    isSuccess = "isSuccess",
    hasCompleted = "hasCompleted",
    hasFailed = "hasFailed",
}
```

### Transpiled JavaScript

Enums are converted to objects, resulting in additional runtime code. Example from [TypeScript Playground](https://www.typescriptlang.org/play/?#code/KYOwrgtgBAygLgQzmAzlA3gKCjqBLFAGQHsEATPEAcygF4oAiAk8yqhgGm1wJjAGN+wFGnpMUfQcJSduOABYIUAYWIQADgBtgcYGTqNFKtVp17ZuKEYBiCPNv1ibdh7IC+QA):

```js
"use strict";
var Status;
(function(Status) {
    Status["isLoading"] = "isLoading";
    Status["isSuccess"] = "isSuccess";
    Status["hasCompleted"] = "hasCompleted";
    Status["hasFailed"] = "hasFailed";
})(Status || (Status = {}));
```

## New code (using as const and typeof)

```typescript
const Status = {
    isLoading: "isLoading",
    isSuccess: "isSuccess",
    hasCompleted: "hasCompleted",
    hasFailed: "hasFailed",
} as const;

type TStatus = typeof Status[keyof typeof Status];
```

## Why use as `const` and `typeof` ?

* Zero runtime overhead: The compiled output only includes the Status object, without additional wrappers.
* Readonly safety: `as const` ensures the values are immutable.
* Interoperability: Works seamlessly with libraries and patterns requiring plain objects.

> [! TIP]
> When using `as const` on an object, the values are set to **readonly**.

### References

1. [Enums considered harmful](https://www.youtube.com/watch?v=jjMbPt_H3RQ)
2. [Typescript enums drawbacks and solutions](https://dev.to/ylerjen/use-typescript-const-assertion-instead-of-enums-mfn)
