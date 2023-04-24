### Remove dead code

Dead code is just as bad as duplicate code. There's no reason to keep it in your codebase.  
If it's not being called, get rid of it! It will still be safe in your version history if you still need it.

**Bad:**

```js
function oldRequestModule(url: string) {
  // ...
}

function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**Good:**

```js
function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```
