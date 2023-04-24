### Use getters and setters

TypeScript supports getter/setter syntax.  
Using getters and setters to access data from objects that encapsulate behavior could be better than simply looking for a property on an object.  
"Why?" you might ask. Well, here's a list of reasons:

-   When you want to do more beyond getting an object property, you don't have to look up and change every accessor in your codebase.
    
-   Makes adding validation simple when doing a set.
    
-   Encapsulates the internal representation.
    
-   Easy to add logging and error handling when getting and setting.
    
-   You can lazy load your object's properties, let's say getting it from a server.
    

**Bad:**

```js
type BankAccount = {
  balance: number;
  // ...
};

const value = 100;
const account: BankAccount = {
  balance: 0
  // ...
};

if (value < 0) {
  throw new Error('Cannot set negative balance.');
}

account.balance = value;
```

**Good:**

```js
class BankAccount {
  private accountBalance: number = 0;

  get balance(): number {
    return this.accountBalance;
  }

  set balance(value: number) {
    if (value < 0) {
      throw new Error('Cannot set negative balance.');
    }

    this.accountBalance = value;
  }

  // ...
}

// Now `BankAccount` encapsulates the validation logic.
// If one day the specifications change, and we need extra validation rule,
// we would have to alter only the `setter` implementation,
// leaving all dependent code unchanged.
const account = new BankAccount();
account.balance = 100;
```

