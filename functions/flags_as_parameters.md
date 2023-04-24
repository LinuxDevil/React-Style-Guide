### Don't use flags as function parameters

Flags tell your user that this function does more than one thing.  
Functions should do one thing. Split out your functions if they are following different code paths based on a boolean.

**Bad:**

```js
function createFile(name: string, temp: boolean) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good:**

```js
function createTempFile(name: string) {
  createFile(`./temp/${name}`);
}

function createFile(name: string) {
  fs.create(name);
}
```
