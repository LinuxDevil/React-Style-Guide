### Prefer composition over inheritance

As stated famously in [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four, you should _prefer composition over inheritance_ where you can. There are lots of good reasons to use inheritance and lots of good reasons to use composition. The main point for this maxim is that if your mind instinctively goes for inheritance, try to think if composition could model your problem better. In some cases it can.

You might be wondering then, "when should I use inheritance?" It depends on your problem at hand, but this is a decent list of when inheritance makes more sense than composition:

1.  Your inheritance represents an "is-a" relationship and not a "has-a" relationship (Human->Animal vs. User->UserDetails).
    
2.  You can reuse code from the base classes (Humans can move like all animals).
    
3.  You want to make global changes to derived classes by changing a base class. (Change the caloric expenditure of all animals when they move).
    

**Bad:**

```js
class Employee {
  constructor(private readonly name: string, private readonly email: string) {}
  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(name: string, email: string, private readonly ssn: string, private readonly salary: number) {
    super(name, email);
  }

  // ...
}
```

**Good:**

```js
class Employee {
  private taxData: EmployeeTaxData;

  constructor(private readonly name: string, private readonly email: string) {}
  setTaxData(ssn: string, salary: number): Employee {
    this.taxData = new EmployeeTaxData(ssn, salary);
    return this;
  }

  // ...
}

class EmployeeTaxData {
  constructor(public readonly ssn: string, public readonly salary: number) {}
  // ...
}
```
