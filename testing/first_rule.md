### F.I.R.S.T. rules

Clean tests should follow the rules:

-   **Fast** tests should be fast because we want to run them frequently.
    
-   **Independent** tests should not depend on each other. They should provide same output whether run independently or all together in any order.
    
-   **Repeatable** tests should be repeatable in any environment and there should be no excuse for why they fail.
    
-   **Self-Validating** a test should answer with either _Passed_ or _Failed_. You don't need to compare log files to answer if a test passed.
    
-   **Timely** unit tests should be written before the production code. If you write tests after the production code, you might find writing tests too hard.

** Good

```typescript
// add.ts

export const add = (a: number, b: number): number => {
  return a + b;
};

// add.test.ts

import { add } from './add';

describe('add function', () => {
  // Fast: This test is very quick to run.
  // Independent: This test does not depend on any other tests.
  // Repeatable: This test is repeatable, as it will always produce the same result given the same inputs.
  // Self-Validating: The test itself will report if it passes or fails.
  // Timely: The test is written before the actual function (in a TDD manner).

  it('correctly adds two numbers', () => {
    const result = add(1, 2);
    expect(result).toBe(3);
  });

  // We can also add more tests to cover more cases
  it('correctly adds two negative numbers', () => {
    const result = add(-1, -2);
    expect(result).toBe(-3);
  });

  it('correctly adds a positive and a negative number', () => {
    const result = add(-1, 2);
    expect(result).toBe(1);
  });
});
```

** Bad

```typescript
// add.test.ts

import { add } from './add';

let previousResult = 0;

describe('add function', () => {
  it('correctly adds two numbers', () => {
    const result = add(1, 2);
    previousResult = result;
    expect(result).toBe(3);
  });

  it('correctly adds two other numbers', () => {
    const result = add(previousResult, 5);
    expect(result).toBe(8);
  });
});
```
