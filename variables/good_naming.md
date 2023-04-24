## Use meaningful variable names

Distinguish names in such a way that the reader knows what the differences offer.

**Bad:**

```js
function between<T>(a1: T, a2: T, a3: T): boolean {
  return a2 <= a1 && a1 <= a3;
}
```

**Good:**

```js
function between<T>(value: T, left: T, right: T): boolean {
  return left <= value && value <= right;
}
```