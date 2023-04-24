### Function arguments (2 or fewer ideally)

Limiting the number of function parameters is incredibly important because it makes testing your function easier.  
Having more than three leads to a combinatorial explosion where you have to test tons of different cases with each separate argument.

One or two arguments is the ideal case, and three should be avoided if possible. Anything more than that should be consolidated.  
Usually, if you have more than two arguments then your function is trying to do too much.  
In cases where it's not, most of the time a higher-level object will suffice as an argument.

Consider using object literals if you are finding yourself needing a lot of arguments.

To make it obvious what properties the function expects, you can use the [destructuring](https://basarat.gitbook.io/typescript/future-javascript/destructuring) syntax.  
This has a few advantages:

1.  When someone looks at the function signature, it's immediately clear what properties are being used.
    
2.  It can be used to simulate named parameters.
    
3.  Destructuring also clones the specified primitive values of the argument object passed into the function. This can help prevent side effects. Note: objects and arrays that are destructured from the argument object are NOT cloned.
    
4.  TypeScript warns you about unused properties, which would be impossible without destructuring.
    

**Bad:**

```js
function createMenu(title: string, body: string, buttonText: string, cancellable: boolean) {
  // ...
}

createMenu('Foo', 'Bar', 'Baz', true);
```

**Good:**

```js
function createMenu(options: { title: string; body: string; buttonText: string; cancellable: boolean }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

You can further improve readability by using [type aliases](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases):

```js
type MenuOptions = { title: string; body: string; buttonText: string; cancellable: boolean };

function createMenu(options: MenuOptions) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```
