### Always use Error for throwing or rejecting

JavaScript as well as TypeScript allow you to throw any object. A Promise can also be rejected with any reason object.

It is advisable to use the throw syntax with an Error type. This is because your error might be caught in higher level code with a catch syntax.

It would be very confusing to catch a string message there and would make

[debugging more painful](https://basarat.gitbook.io/typescript/type-system/exceptions#always-use-error).

For the same reason you should reject promises with Error types.

**Bad:**

```js
function calculateTotal(items: Item[]): number {
  throw 'Not implemented.';
}

function get(): Promise<Item[]> {
  return Promise.reject('Not implemented.');
}
```

**Good:**

```js
function calculateTotal(items: Item[]): number {
  throw new Error('Not implemented.');
}

function get(): Promise<Item[]> {
  return Promise.reject(new Error('Not implemented.'));
}

// or equivalent to:

async function get(): Promise<Item[]> {
  throw new Error('Not implemented.');
}
```

The benefit of using Error types is that it is supported by the syntax try/catch/finally and implicitly all errors have the stack property which

is very powerful for debugging.

There are also other alternatives, not to use the throw syntax and instead always return custom error objects. TypeScript makes this even easier.

Consider the following example:

```js
type Result<R> = { isError: false; value: R };
type Failure<E> = { isError: true; error: E };
type Failable<R, E> = Result<R> | Failure<E>;

function calculateTotal(items: Item[]): Failable<number, 'empty'> {
  if (items.length === 0) {
    return { isError: true, error: 'empty' };
  }

  // ...
  return { isError: false, value: 42 };
}
```

For the detailed explanation of this idea refer to the [original post](https://medium.com/@dhruvrajvanshi/making-exceptions-type-safe-in-typescript-c4d200ee78e9).
