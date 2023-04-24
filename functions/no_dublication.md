### Remove duplicate code

Do your absolute best to avoid duplicate code.  
Duplicate code is bad because it means that there's more than one place to alter something if you need to change some logic.

Imagine if you run a restaurant and you keep track of your inventory: all your tomatoes, onions, garlic, spices, etc.  
If you have multiple lists that you keep this on, then all have to be updated when you serve a dish with tomatoes in them.  
If you only have one list, there's only one place to update!

Oftentimes you have duplicate code because you have two or more slightly different things, that share a lot in common, but their differences force you to have two or more separate functions that do much of the same things. Removing duplicate code means creating an abstraction that can handle this set of different things with just one function/module/class.

Getting the abstraction right is critical, that's why you should follow the [SOLID](#) principles. Bad abstractions can be worse than duplicate code, so be careful! Having said this, if you can make a good abstraction, do it! Don't repeat yourself, otherwise, you'll find yourself updating multiple places anytime you want to change one thing.

**Bad:**

```js
function showDeveloperList(developers: Developer[]) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();

    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers: Manager[]) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Good:**

```js
class Developer {
  // ...
  getExtraDetails() {
    return {
      githubLink: this.githubLink
    };
  }
}

class Manager {
  // ...
  getExtraDetails() {
    return {
      portfolio: this.portfolio
    };
  }
}

function showEmployeeList(employee: (Developer | Manager)[]) {
  employee.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const extra = employee.getExtraDetails();

    const data = {
      expectedSalary,
      experience,
      extra
    };

    render(data);
  });
}
```

You may also consider adding a union type, or common parent class if it suits your abstraction.

```js
class Developer {
  // ...
}

class Manager {
  // ...
}

type Employee = Developer | Manager

function showEmployeeList(employee: Employee[]) {
  // ...
  });
}

```

You should be critical about code duplication. Sometimes there is a tradeoff between duplicated code and increased complexity by introducing unnecessary abstraction. When two implementations from two different modules look similar but live in different domains, duplication might be acceptable and preferred over extracting the common code. The extracted common code, in this case, introduces an indirect dependency between the two modules.
