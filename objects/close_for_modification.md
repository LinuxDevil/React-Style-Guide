### Prefer immutability

TypeScript's type system allows you to mark individual properties on an interface/class as _readonly_. This allows you to work in a functional way (an unexpected mutation is bad).  
For more advanced scenarios there is a built-in type Readonly that takes a type T and marks all of its properties as readonly using mapped types (see [mapped types](https://www.typescriptlang.org/docs/handbook/advanced-types.html#mapped-types)).

**Bad:**

```js
interface Config {
  host: string;
  port: string;
  db: string;
}
```

**Good:**

```js
interface Config {
  readonly host: string;
  readonly port: string;
  readonly db: string;
}
```

For arrays, you can create a read-only array by using ReadonlyArray<T>.  
It doesn't allow changes such as push() and fill(), but can use features such as concat() and slice() that do not change the array's value.

**Bad:**

```js
const array: number[] = [1, 3, 5];
array = []; // error
array.push(100); // array will be updated
```

**Good:**

```js
const array: ReadonlyArray<number> = [1, 3, 5];
array = []; // error
array.push(100); // error
```

Declaring read-only arguments in [TypeScript 3.4 is a bit easier](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#improvements-for-readonlyarray-and-readonly-tuples).

```js
function hoge(args: readonly string[]) {
  args.push(1); // error
}
```

Prefer [const assertions](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#const-assertions) for literal values.

**Bad:**

```js
const config = {
  hello: 'world'
};
config.hello = 'world'; // value is changed

const array = [1, 3, 5];
array[0] = 10; // value is changed

// writable objects is returned
function readonlyData(value: number) {
  return { value };
}

const result = readonlyData(100);
result.value = 200; // value is changed
```

**Good:**

```js
// read-only object
const config = {
  hello: 'world'
} as const;
config.hello = 'world'; // error

// read-only array
const array = [1, 3, 5] as const;
array[0] = 10; // error

// You can return read-only objects
function readonlyData(value: number) {
  return { value } as const;
}

const result = readonlyData(100);
result.value = 200; // error
```
