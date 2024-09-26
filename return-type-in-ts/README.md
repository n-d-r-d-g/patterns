# ReturnType in TypeScript

The `ReturnType` generic type resolves to the return type of a function.

In following code snippet, the `formatAge` function takes in an argument of type number and returns a string.

```ts
function formatAge(age: number) {
  return `You're ${age} years old.`;
}
```

`ReturnType` accepts `formatAge`'s type signature, so it can infer its return type.

```ts
type FunctionReturnType = ReturnType<typeof formatAge>;
// string
```

`ReturnType` also supports return values with const assertions.

```ts
function formatAge(age: number) {
  return `You're ${age} years old.` as const;
}

type FunctionReturnType = ReturnType<typeof formatAge>;
// `You're ${number} years old.`
```
