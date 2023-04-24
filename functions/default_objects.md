### Set default objects with Object.assign or destructuring

**Bad:**

```js
type MenuConfig = { title?: string; body?: string; buttonText?: string; cancellable?: boolean };

function createMenu(config: MenuConfig) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable !== undefined ? config.cancellable : true;

  // ...
}

createMenu({ body: 'Bar' });
```

**Good:**

```js
type MenuConfig = { title?: string; body?: string; buttonText?: string; cancellable?: boolean };

function createMenu(config: MenuConfig) {
  const menuConfig = Object.assign(
    {
      title: 'Foo',
      body: 'Bar',
      buttonText: 'Baz',
      cancellable: true
    },
    config
  );

  // ...
}

createMenu({ body: 'Bar' });
```

Or, you could use the spread operator:

```js
function createMenu(config: MenuConfig) {
  const menuConfig = {
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true,
    ...config
  };

  // ...
}
```

The spread operator and Object.assign() are very similar.  
The main difference is that spreading defines new properties, while Object.assign() sets them. More detailed, the difference is explained in [this](https://stackoverflow.com/questions/32925460/object-spread-vs-object-assign) thread.

Alternatively, you can use destructuring with default values:

```js
type MenuConfig = { title?: string; body?: string; buttonText?: string; cancellable?: boolean };

function createMenu({ title = 'Foo', body = 'Bar', buttonText = 'Baz', cancellable = true }: MenuConfig) {
  // ...
}

createMenu({ body: 'Bar' });
```

To avoid any side effects and unexpected behavior by passing in explicitly the undefined or null value, you can tell the TypeScript compiler to not allow it.  
See --strictNullChecks option in TypeScript.
