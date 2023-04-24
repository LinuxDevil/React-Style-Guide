### Use iterators and generators

Use generators and iterables when working with collections of data used like a stream.  
There are some good reasons:

-   decouples the callee from the generator implementation in a sense that callee decides how many  
    items to access
    
-   lazy execution, items are streamed on-demand
    
-   built-in support for iterating items using the for-of syntax
    
-   iterables allow implementing optimized iterator patterns
    

**Bad:**

```js
function fibonacci(n: number): number[] {
  if (n === 1) return [0];
  if (n === 2) return [0, 1];

  const items: number[] = [0, 1];
  while (items.length < n) {
    items.push(items[items.length - 2] + items[items.length - 1]);
  }

  return items;
}

function print(n: number) {
  fibonacci(n).forEach((fib) => console.log(fib));
}

// Print first 10 Fibonacci numbers.
print(10);
```

**Good:**

```js
// Generates an infinite stream of Fibonacci numbers.
// The generator doesn't keep the array of all numbers.
function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

function print(n: number) {
  let i = 0;
  for (const fib of fibonacci()) {
    if (i++ === n) break;
    console.log(fib);
  }
}

// Print first 10 Fibonacci numbers.
print(10);
```

There are libraries that allow working with iterables in a similar way as with native arrays, by  
chaining methods like map, slice, forEach etc. See [itiriri](https://www.npmjs.com/package/itiriri) for  
an example of advanced manipulation with iterables (or [itiriri-async](https://www.npmjs.com/package/itiriri-async) for manipulation of async iterables).

```js
import itiriri from 'itiriri';

function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

itiriri(fibonacci())
  .take(10)
  .forEach((fib) => console.log(fib));
```
