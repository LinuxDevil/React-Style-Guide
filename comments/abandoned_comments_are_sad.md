### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**Bad:**

```js
type User = {
  name: string;
  email: string;
  // age: number;
  // jobPosition: string;
};
```

**Good:**

```js
type User = {
  name: string;
  email: string;
};
```

