### Avoid negative conditionals

**Bad:**

```js
function isEmailNotUsed(email: string): boolean {
  // ...
}

if (isEmailNotUsed(email)) {
  // ...
}
```

**Good:**

```js
function isEmailUsed(email: string): boolean {
  // ...
}

if (!isEmailUsed(email)) {
  // ...
}
```
