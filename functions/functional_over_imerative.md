### Favor functional programming over imperative programming

Favor this style of programming when you can.

**Bad:**

```js
const contributions = [
  { name: 'Uncle Bobby', linesOfCode: 500 },
  {
    name: 'Suzie Q',
    linesOfCode: 1500
  },
  {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  },
  {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < contributions.length; i++) {
  totalOutput += contributions[i].linesOfCode;
}
```

**Good:**

```js
const contributions = [
  { name: 'Uncle Bobby', linesOfCode: 500 },
  {
    name: 'Suzie Q',
    linesOfCode: 1500
  },
  {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  },
  {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

const totalOutput = contributions.reduce((totalLines, output) => totalLines + output.linesOfCode, 0);
```
