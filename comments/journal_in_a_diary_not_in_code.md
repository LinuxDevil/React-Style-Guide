### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code, and especially journal comments. Use git log to get history!

**Bad:**

```js
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Added type-checking (LI)
 * 2015-03-14: Implemented combine (JR)
 */
function combine(a: number, b: number): number {
  return a + b;
}
```

**Good:**

```js
function combine(a: number, b: number): number {
  return a + b;
}
```
