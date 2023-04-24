## Use explanatory variables

**Bad:**

```js
declare const users: Map<string, User>;

for (const keyValue of users) {
  // iterate through users map
}
```

**Good:**

```js
declare const users: Map<string, User>;

for (const [id, user] of users) {
  // iterate through users map
}
```
