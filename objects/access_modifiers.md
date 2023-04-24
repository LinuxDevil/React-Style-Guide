### Make objects have private/protected members

TypeScript supports public _(default)_, protected and private accessors on class members.

**Bad:**

```js
class Circle {
  radius: number;

  constructor(radius: number) {
    this.radius = radius;
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**Good:**

```js
class Circle {
  constructor(private readonly radius: number) {}
  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```
