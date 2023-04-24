### Don't write to global functions

Polluting globals is a bad practice in JavaScript because you could clash with another library and the user of your API would be none-the-wiser until they get an exception in production. Let's think about an example: what if you wanted to extend JavaScript's native Array method to have a diff method that could show the difference between two arrays? You could write your new function to the Array.prototype, but it could clash with another library that tried to do the same thing. What if that other library was just using diff to find the difference between the first and last elements of an array? This is why it would be much better to just use classes and simply extend the Array global.

**Bad:**

```js
declare global {
  interface Array<T> {
    diff(other: T[]): Array<T>;
  }
}

if (!Array.prototype.diff) {
  Array.prototype.diff = function <T>(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter((elem) => !hash.has(elem));
  };
}
```

**Good:**

```js
class MyArray<T> extends Array<T> {
  diff(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter((elem) => !hash.has(elem));
  }
}
```
