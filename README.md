
# Code Principles

### Hi All ❤️

There are a few of these principles that will be universally accepted, but not all of them must be completely adhered to. These are merely guidelines,  
but ones that have been formalized over a long period of time.

One more thing: even if you have years of experience with them, knowing them won't make you a better software developer right away.  
Like wet clay being molded into its final form, every line of code begins as a first draft.  
When we review it with our colleagues/friends, we finally chisel out the flaws. Don't be hard on yourself if your early drafts need work. Instead, abuse the code!

#### Let's code our wisdom like a melodic poetry ❤️


### To keep in mind:

-   Use type annotations to explicitly declare the types of variables, function arguments, and return values. This makes it clear to other developers (and to the TypeScript compiler) what the expected types are, and can help prevent type errors.
    
-   Use interfaces to define the shape of complex types, such as objects with multiple properties or arrays with specific elements. This can make your code more self-documenting and easier to understand.
    
-   Use classes and encapsulation to organize your code into logical units and protect the internal state of your objects from being modified by external code.
    
-   Follow the SOLID principles of object-oriented design to create code that is easy to understand, maintain, and extend.
    

## Most important Principles to keep in mind is:

1.  [Extracting functions outside the params so that it wont get evaluated every time](./extracting_functions.md)

## [DRY (Don't Repeat Yourself)](./DRY.md)


## [ KISS (Keep it simple, stupid)]()


## [YAGNI (You ain't gonna need it)](./YAGNI.md)

----

## [Variables](./Vars.md)

## Functions


### [Function arguments (2 or fewer ideally)](./functions/arguments.md)
### [Functions should do one thing](./functions/one_job.md)
### [Function names should say what they do](/functions/meaningful_names.md)
### [Functions should only be one level of abstraction](/functions/one_abstraction_lvl.md)
### [Remove duplicate code](/functions/no_dublication.md)
### [Set default objects with Object.assign or destructuring](/functions/default_objects.md)
### [Don't use flags as function parameters](/functions/flags_as_parameters.md)
### [Avoid Side Effects (part 1)](/functions/no_side_effects_usage_1.md)
### [Avoid Side Effects (part 2)](/functions/no_side_effect_usage_2.md)
### [Don't write to global functions](/functions/no_globals.md)
### [Favor functional programming over imperative programming](/functions/functional_over_imerative.md)
### [Encapsulate conditionals](/functions/encapsulate_conditions.md)
### [Avoid negative conditionals](/functions/avoid_negative_conditions.md)
### [Avoid conditionals](/functions/avoid_conditionals.md)
### [Avoid type checking](/functions/avoid_type_checking.md)
### [Don't over-optimize](/functions/don't_over_optimize.md)
### [Remove dead code](/functions/dead_code.md)
### [Use iterators and generators](/functions/iterators_and_generators.md)

----

## Objects and Data Structures

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

### Make objects have private/protected members

TypeScript supports public _(default)_, protected and private accessors on class members.

**Bad:**

```js
class Circle {
  radius: number;

  constructor(radius: number) {
    this.radius = radius;
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**Good:**

```js
class Circle {
  constructor(private readonly radius: number) {}
  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

### Prefer immutability

TypeScript's type system allows you to mark individual properties on an interface/class as _readonly_. This allows you to work in a functional way (an unexpected mutation is bad).  
For more advanced scenarios there is a built-in type Readonly that takes a type T and marks all of its properties as readonly using mapped types (see [mapped types](https://www.typescriptlang.org/docs/handbook/advanced-types.html#mapped-types)).

**Bad:**

```js
interface Config {
  host: string;
  port: string;
  db: string;
}
```

**Good:**

```js
interface Config {
  readonly host: string;
  readonly port: string;
  readonly db: string;
}
```

For arrays, you can create a read-only array by using ReadonlyArray<T>.  
It doesn't allow changes such as push() and fill(), but can use features such as concat() and slice() that do not change the array's value.

**Bad:**

```js
const array: number[] = [1, 3, 5];
array = []; // error
array.push(100); // array will be updated
```

**Good:**

```js
const array: ReadonlyArray<number> = [1, 3, 5];
array = []; // error
array.push(100); // error
```

Declaring read-only arguments in [TypeScript 3.4 is a bit easier](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#improvements-for-readonlyarray-and-readonly-tuples).

```js
function hoge(args: readonly string[]) {
  args.push(1); // error
}
```

Prefer [const assertions](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#const-assertions) for literal values.

**Bad:**

```js
const config = {
  hello: 'world'
};
config.hello = 'world'; // value is changed

const array = [1, 3, 5];
array[0] = 10; // value is changed

// writable objects is returned
function readonlyData(value: number) {
  return { value };
}

const result = readonlyData(100);
result.value = 200; // value is changed
```

**Good:**

```js
// read-only object
const config = {
  hello: 'world'
} as const;
config.hello = 'world'; // error

// read-only array
const array = [1, 3, 5] as const;
array[0] = 10; // error

// You can return read-only objects
function readonlyData(value: number) {
  return { value } as const;
}

const result = readonlyData(100);
result.value = 200; // error
```

### type vs. interface

Use type when you might need a union or intersection. Use an interface when you want extends or implements. There is no strict rule, however, use the one that works for you.  
For a more detailed explanation refer to this [answer](https://stackoverflow.com/questions/37233735/typescript-interfaces-vs-types/54101543#54101543) about the differences between type and interface in TypeScript.

**Bad:**

```js
interface EmailConfig {
  // ...
}

interface DbConfig {
  // ...
}

interface Config {
  // ...
}

//...

type Shape = {
  // ...
};
```

**Good:**

```js
type EmailConfig = {
  // ...
};

type DbConfig = {
  // ...
};

type Config = EmailConfig | DbConfig;

// ...

interface Shape {
  // ...
}

class Circle implements Shape {
  // ...
}

class Square implements Shape {
  // ...
}
```

## Classes

### Classes should be small

The class' size is measured by its responsibility. Following the _Single Responsibility principle_ a class should be small.

**Bad:**

```js
class Dashboard {
  getLanguage(): string {
    /* ... */
  }
  setLanguage(language: string): void {
    /* ... */
  }
  showProgress(): void {
    /* ... */
  }
  hideProgress(): void {
    /* ... */
  }
  isDirty(): boolean {
    /* ... */
  }
  disable(): void {
    /* ... */
  }
  enable(): void {
    /* ... */
  }
  addSubscription(subscription: Subscription): void {
    /* ... */
  }
  removeSubscription(subscription: Subscription): void {
    /* ... */
  }
  addUser(user: User): void {
    /* ... */
  }
  removeUser(user: User): void {
    /* ... */
  }
  goToHomePage(): void {
    /* ... */
  }
  updateProfile(details: UserDetails): void {
    /* ... */
  }
  getVersion(): string {
    /* ... */
  }
  // ...
}
```

**Good:**

```js
class Dashboard {
  disable(): void {
    /* ... */
  }
  enable(): void {
    /* ... */
  }
  getVersion(): string {
    /* ... */
  }
}

// split the responsibilities by moving the remaining methods to other classes
// ...
```

### High cohesion and low coupling

Cohesion defines the degree to which class members are related to each other. Ideally, all fields within a class should be used by each method.  
We then say that the class is _maximally cohesive_. In practice, this, however, is not always possible, nor even advisable. You should however prefer cohesion to be high.

Coupling refers to how related or dependent are two classes toward each other. Classes are said to be low coupled if changes in one of them don't affect the other one.

Good software design has **high cohesion** and **low coupling**.

**Bad:**

```js
class UserManager {
  // Bad: each private variable is used by one or another group of methods.
  // It makes clear evidence that the class is holding more than a single responsibility.
  // If I need only to create the service to get the transactions for a user,
  // I'm still forced to pass and instance of `emailSender`.
  constructor(private readonly db: Database, private readonly emailSender: EmailSender) {}
  async getUser(id: number): Promise<User> {
    return await db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await db.transactions.find({ userId });
  }

  async sendGreeting(): Promise<void> {
    await emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**Good:**

```js
class UserService {
  constructor(private readonly db: Database) {}
  async getUser(id: number): Promise<User> {
    return await this.db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await this.db.transactions.find({ userId });
  }
}

class UserNotifier {
  constructor(private readonly emailSender: EmailSender) {}
  async sendGreeting(): Promise<void> {
    await this.emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await this.emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

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

### Use method chaining

This pattern is very useful and commonly used in many libraries. It allows your code to be expressive, and less verbose. For that reason, use method chaining and take a look at how clean your code will be.

**Bad:**

```js
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): void {
    this.collection = collection;
  }

  page(number: number, itemsPerPage: number = 100): void {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
  }

  orderBy(...fields: string[]): void {
    this.orderByFields = fields;
  }

  build(): Query {
    // ...
  }
}

// ...

const queryBuilder = new QueryBuilder();
queryBuilder.from('users');
queryBuilder.page(1, 100);
queryBuilder.orderBy('firstName', 'lastName');

const query = queryBuilder.build();
```

**Good:**

```js
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): this {
    this.collection = collection;
    return this;
  }

  page(number: number, itemsPerPage: number = 100): this {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
    return this;
  }

  orderBy(...fields: string[]): this {
    this.orderByFields = fields;
    return this;
  }

  build(): Query {
    // ...
  }
}

// ...

const query = new QueryBuilder().from('users').page(1, 100).orderBy('firstName', 'lastName').build();
```

## SOLID

### Single Responsibility Principle (SRP)

As stated in Clean Code, "There should never be more than one reason for a class to change". It's tempting to jam-pack a class with a lot of functionality, like when you can only take one suitcase on your flight. The issue with this is that your class won't be conceptually cohesive and it will give it many reasons to change. Minimizing the amount of time you need to change a class is important. It's important because if too much functionality is in one class and you modify a piece of it, it can be difficult to understand how that will affect other dependent modules in your codebase.

**Bad:**

```js
class UserSettings {
  constructor(private readonly user: User) {}
  changeSettings(settings: UserSettings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Good:**

```js
class UserAuth {
  constructor(private readonly user: User) {}
  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  private readonly auth: UserAuth;

  constructor(private readonly user: User) {
    this.auth = new UserAuth(user);
  }

  changeSettings(settings: UserSettings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

### Open/Closed Principle (OCP)

As stated by Bertrand Meyer, "software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification." What does that mean though? This principle basically states that you should allow users to add new functionalities without changing existing code.

**Bad:**

```js
class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {}
  async fetch<T>(url: string): Promise<T> {
    if (this.adapter instanceof AjaxAdapter) {
      const response = await makeAjaxCall<T>(url);
      // transform response and return
    } else if (this.adapter instanceof NodeAdapter) {
      const response = await makeHttpCall<T>(url);
      // transform response and return
    }
  }
}

function makeAjaxCall<T>(url: string): Promise<T> {
  // request and return promise
}

function makeHttpCall<T>(url: string): Promise<T> {
  // request and return promise
}
```

**Good:**

```js
abstract class Adapter {
  abstract async request<T>(url: string): Promise<T>;

  // code shared to subclasses ...
}

class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T> {
    // request and return promise
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T> {
    // request and return promise
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {}
  async fetch<T>(url: string): Promise<T> {
    const response = await this.adapter.request<T>(url);
    // transform response and return
  }
}
```

### Liskov Substitution Principle (LSP)

This is a scary term for a very simple concept. It's formally defined as "If S is a subtype of T, then objects of type T may be replaced with objects of type S (i.e., objects of type S may substitute objects of type T) without altering any of the desirable properties of that program (correctness, task performed, etc.)." That's an even scarier definition.

The best explanation for this is if you have a parent class and a child class, then the parent class and child class can be used interchangeably without getting incorrect results. This might still be confusing, so let's take a look at the classic Square-Rectangle example. Mathematically, a square is a rectangle, but if you model it using the "is-a" relationship via inheritance, you quickly get into trouble.

**Bad:**

```js
class Rectangle {
  constructor(protected width: number = 0, protected height: number = 0) {}
  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  setWidth(width: number): this {
    this.width = width;
    return this;
  }

  setHeight(height: number): this {
    this.height = height;
    return this;
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width: number): this {
    this.width = width;
    this.height = width;
    return this;
  }

  setHeight(height: number): this {
    this.width = height;
    this.height = height;
    return this;
  }
}

function renderLargeRectangles(rectangles: Rectangle[]) {
  rectangles.forEach((rectangle) => {
    const area = rectangle.setWidth(4).setHeight(5).getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Good:**

```js
abstract class Shape {
  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  abstract getArea(): number;
}

class Rectangle extends Shape {
  constructor(private readonly width = 0, private readonly height = 0) {
    super();
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(private readonly length: number) {
    super();
  }

  getArea(): number {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes: Shape[]) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

### Interface Segregation Principle (ISP)

ISP states that "Clients should not be forced to depend upon interfaces that they do not use.". This principle is very much related to the Single Responsibility Principle.  
What it really means is that you should always design your abstractions in a way that the clients that are using the exposed methods do not get the whole pie instead. That also include imposing the clients with the burden of implementing methods that they don’t actually need.

**Bad:**

```js
interface SmartPrinter {
  print();
  fax();
  scan();
}

class AllInOnePrinter implements SmartPrinter {
  print() {
    // ...
  }

  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements SmartPrinter {
  print() {
    // ...
  }

  fax() {
    throw new Error('Fax not supported.');
  }

  scan() {
    throw new Error('Scan not supported.');
  }
}
```

**Good:**

```js
interface Printer {
  print();
}

interface Fax {
  fax();
}

interface Scanner {
  scan();
}

class AllInOnePrinter implements Printer, Fax, Scanner {
  print() {
    // ...
  }

  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements Printer {
  print() {
    // ...
  }
}
```

### Dependency Inversion Principle (DIP)

This principle states two essential things:

1.  High-level modules should not depend on low-level modules. Both should depend on abstractions.
    
2.  Abstractions should not depend upon details. Details should depend on abstractions.
    

This can be hard to understand at first, but if you've worked with Angular, you've seen an implementation of this principle in the form of Dependency Injection (DI). While they are not identical concepts, DIP keeps high-level modules from knowing the details of its low-level modules and setting them up. It can accomplish this through DI. A huge benefit of this is that it reduces the coupling between modules. Coupling is a very bad development pattern because it makes your code hard to refactor.

DIP is usually achieved by a using an inversion of control (IoC) container. An example of a powerful IoC container for TypeScript is [InversifyJs](https://www.npmjs.com/package/inversify)

**Bad:**

```js
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
};

class XmlFormatter {
  parse<T>(content: string): T {
    // Converts an XML string to an object T
  }
}

class ReportReader {
  // BAD: We have created a dependency on a specific request implementation.
  // We should just have ReportReader depend on a parse method: `parse`
  private readonly formatter = new XmlFormatter();

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader();
const report = await reader.read('report.xml');
```

**Good:**

```js
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
};

interface Formatter {
  parse<T>(content: string): T;
}

class XmlFormatter implements Formatter {
  parse<T>(content: string): T {
    // Converts an XML string to an object T
  }
}

class JsonFormatter implements Formatter {
  parse<T>(content: string): T {
    // Converts a JSON string to an object T
  }
}

class ReportReader {
  constructor(private readonly formatter: Formatter) {}
  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader(new XmlFormatter());
const report = await reader.read('report.xml');

// or if we had to read a json report
const reader = new ReportReader(new JsonFormatter());
const report = await reader.read('report.json');
```

## Testing

Testing is more important than shipping. If you have no tests or an inadequate amount, then every time you ship code you won't be sure that you didn't break anything.  
Deciding on what constitutes an adequate amount is up to your team, but having 100% coverage (all statements and branches)  
is how you achieve very high confidence and developer peace of mind. This means that in addition to having a great testing framework, you also need to use a good [coverage tool](https://github.com/gotwarlost/istanbul).

There's no excuse to not write tests. There are [plenty of good JS test frameworks](http://jstherightway.org/#testing-tools) with typings support for TypeScript, so find one that your team prefers. When you find one that works for your team, then aim to always write tests for every new feature/module you introduce. If your preferred method is Test Driven Development (TDD), that is great, but the main point is to just make sure you are reaching your coverage goals before launching any feature, or refactoring an existing one.

### The three laws of TDD

1.  You are not allowed to write any production code unless it is to make a failing unit test pass.
    
2.  You are not allowed to write any more of a unit test than is sufficient to fail, and; compilation failures are failures.
    
3.  You are not allowed to write any more production code than is sufficient to pass the one failing unit test.
    

### F.I.R.S.T. rules

Clean tests should follow the rules:

-   **Fast** tests should be fast because we want to run them frequently.
    
-   **Independent** tests should not depend on each other. They should provide same output whether run independently or all together in any order.
    
-   **Repeatable** tests should be repeatable in any environment and there should be no excuse for why they fail.
    
-   **Self-Validating** a test should answer with either _Passed_ or _Failed_. You don't need to compare log files to answer if a test passed.
    
-   **Timely** unit tests should be written before the production code. If you write tests after the production code, you might find writing tests too hard.
    

### Single concept per test

Tests should also follow the _Single Responsibility Principle_. Make only one assert per unit test.

**Bad:**

```js
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles date boundaries', () => {
    let date: AwesomeDate;

    date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));

    date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));

    date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

**Good:**

```js
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles 30-day months', () => {
    const date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));
  });

  it('handles leap year', () => {
    const date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));
  });

  it('handles non-leap year', () => {
    const date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

### The name of the test should reveal its intention

When a test fails, its name is the first indication of what may have gone wrong.

**Bad:**

```js
describe('Calendar', () => {
  it('2/29/2020', () => {
    // ...
  });

  it('throws', () => {
    // ...
  });
});
```

**Good:**

```
describe('Calendar', () => {
  it('should handle leap year', () => {
    // ...
  });

  it('should throw when format is invalid', () => {
    // ...
  });
});
```

## Concurrency

### Prefer promises vs callbacks

Callbacks aren't clean, and they cause excessive amounts of nesting _(the callback hell)_.  
There are utilities that transform existing functions using the callback style to a version that returns promises  
(for Node.js see util.promisify, for general purpose see [pify](https://www.npmjs.com/package/pify), [es6-promisify](https://www.npmjs.com/package/es6-promisify))

**Bad:**

```js
import { get } from 'request';
import { writeFile } from 'fs';

function downloadPage(url: string, saveTo: string, callback: (error: Error, content?: string) => void) {
  get(url, (error, response) => {
    if (error) {
      callback(error);
    } else {
      writeFile(saveTo, response.body, (error) => {
        if (error) {
          callback(error);
        } else {
          callback(null, response.body);
        }
      });
    }
  });
}

downloadPage('<https://en.wikipedia.org/wiki/Robert_Cecil_Martin',> 'article.html', (error, content) => {
  if (error) {
    console.error(error);
  } else {
    console.log(content);
  }
});
```

**Good:**

```js
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url).then((response) => write(saveTo, response));
}

downloadPage('<https://en.wikipedia.org/wiki/Robert_Cecil_Martin',> 'article.html')
  .then((content) => console.log(content))
  .catch((error) => console.error(error));
```

Promises supports a few helper methods that help make code more concise:

Pattern

Description

Promise.resolve(value)

Convert a value into a resolved promise.

Promise.reject(error)

Convert an error into a rejected promise.

Promise.all(promises)

Returns a new promise which is fulfilled with an array of fulfillment values for the passed promises or rejects with the reason of the first promise that rejects.

Promise.race(promises)

Returns a new promise which is fulfilled/rejected with the result/error of the first settled promise from the array of passed promises.

Promise.all is especially useful when there is a need to run tasks in parallel. Promise.race makes it easier to implement things like timeouts for promises.

### Async/Await are even cleaner than Promises

With async/await syntax you can write code that is far cleaner and more understandable than chained promises. Within a function prefixed with async keyword, you have a way to tell the JavaScript runtime to pause the execution of code on the await keyword (when used on a promise).

**Bad:**

```js
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = util.promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url).then((response) => write(saveTo, response));
}

downloadPage('<https://en.wikipedia.org/wiki/Robert_Cecil_Martin',> 'article.html')
  .then((content) => console.log(content))
  .catch((error) => console.error(error));
```

**Good:**

```js
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

async function downloadPage(url: string): Promise<string> {
  const response = await get(url);
  return response;
}

// somewhere in an async function
try {
  const content = await downloadPage('<https://en.wikipedia.org/wiki/Robert_Cecil_Martin');>
  await write('article.html', content);
  console.log(content);
} catch (error) {
  console.error(error);
}
```

## Error Handling

Thrown errors are a good thing! They mean the runtime has successfully identified when something in your program has gone wrong and it's letting you know by stopping function

execution on the current stack, killing the process (in Node), and notifying you in the console with a stack trace.

### Always use Error for throwing or rejecting

JavaScript as well as TypeScript allow you to throw any object. A Promise can also be rejected with any reason object.

It is advisable to use the throw syntax with an Error type. This is because your error might be caught in higher level code with a catch syntax.

It would be very confusing to catch a string message there and would make

[debugging more painful](https://basarat.gitbook.io/typescript/type-system/exceptions#always-use-error).

For the same reason you should reject promises with Error types.

**Bad:**

```js
function calculateTotal(items: Item[]): number {
  throw 'Not implemented.';
}

function get(): Promise<Item[]> {
  return Promise.reject('Not implemented.');
}
```

**Good:**

```js
function calculateTotal(items: Item[]): number {
  throw new Error('Not implemented.');
}

function get(): Promise<Item[]> {
  return Promise.reject(new Error('Not implemented.'));
}

// or equivalent to:

async function get(): Promise<Item[]> {
  throw new Error('Not implemented.');
}
```

The benefit of using Error types is that it is supported by the syntax try/catch/finally and implicitly all errors have the stack property which

is very powerful for debugging.

There are also other alternatives, not to use the throw syntax and instead always return custom error objects. TypeScript makes this even easier.

Consider the following example:

```js
type Result<R> = { isError: false; value: R };
type Failure<E> = { isError: true; error: E };
type Failable<R, E> = Result<R> | Failure<E>;

function calculateTotal(items: Item[]): Failable<number, 'empty'> {
  if (items.length === 0) {
    return { isError: true, error: 'empty' };
  }

  // ...
  return { isError: false, value: 42 };
}
```

For the detailed explanation of this idea refer to the [original post](https://medium.com/@dhruvrajvanshi/making-exceptions-type-safe-in-typescript-c4d200ee78e9).

### Don't ignore caught errors

Doing nothing with a caught error doesn't give you the ability to ever fix or react to said error. Logging the error to the console (console.log) isn't much better as often it can get lost in a sea of things printed to the console. If you wrap any bit of code in a try/catch it means you think an error may occur there and therefore you should have a plan, or create a code path, for when it occurs.

**Bad:**

```js
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}

// or even worse

try {
  functionThatMightThrow();
} catch (error) {
  // ignore error
}
```

**Good:**

```js
import { logger } from './logging';

try {
  functionThatMightThrow();
} catch (error) {
  logger.log(error);
}
```

### Don't ignore rejected promises

For the same reason you shouldn't ignore caught errors from try/catch.

**Bad:**

```js
getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    console.log(error);
  });
```

**Good:**

```js
import { logger } from './logging';

getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    logger.log(error);
  });

// or using the async/await syntax:

try {
  const user = await getUser();
  await sendEmail(user.email, 'Welcome!');
} catch (error) {
  logger.log(error);
}
```

\_## Formatting

Formatting is subjective. Like many rules herein, there is no hard and fast rule that you must follow. The main point is _DO NOT ARGUE_ over formatting. There are tons of tools to automate this. Use one! It's a waste of time and money for engineers to argue over formatting. The general rule to follow is _keep consistent formatting rules_.

Refer also to this great [TypeScript StyleGuide and Coding Conventions](https://basarat.gitbook.io/typescript/styleguide) source.\_

### Migrating from TSLint to ESLint

If you are looking for help in migrating from TSLint to ESLint, you can check out this project: <[https://github.com/typescript-eslint/tslint-to-eslint-config](https://github.com/typescript-eslint/tslint-to-eslint-config) >

### Use consistent capitalization

Capitalization tells you a lot about your variables, functions, etc. These rules are subjective, so your team can choose whatever they want. The point is, no matter what you all choose, just _be consistent_.

**Bad:**

```js
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

type animal = {
  /* ... */
};
type Container = {
  /* ... */
};
```

**Good:**

```js
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

const discography = getArtistDiscography('ACDC');
const beatlesSongs = SONGS.filter((song) => isBeatlesSong(song));

function eraseDatabase() {}
function restoreDatabase() {}

type Animal = {
  /* ... */
};
type Container = {
  /* ... */
};
```

Prefer using PascalCase for class, interface, type and namespace names.

Prefer using camelCase for variables, functions and class members.

Prefer using capitalized SNAKE_CASE for constants.

### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source file. Ideally, keep the caller right above the callee.

We tend to read code from top-to-bottom, like a newspaper. Because of this, make your code read that way.

**Bad:**

```js
class PerformanceReview {
  constructor(private readonly employee: Employee) {}
  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**Good:**

```js
class PerformanceReview {
  constructor(private readonly employee: Employee) {}
  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

### Organize imports

With clean and easy to read import statements you can quickly see the dependencies of current code. Make sure you apply following good practices for import statements:

-   Import statements should be alphabetized and grouped.
    
-   Unused imports should be removed.
    
-   Named imports must be alphabetized (i.e. import {A, B, C} from 'foo';)
    
-   Import sources must be alphabetized within groups, i.e.: import * as foo from 'a'; import * as bar from 'b';
    
-   Prefer using import type instead of import when importing only types from a file to avoid dependency cycles, as these imports are erased at runtime
    
-   Groups of imports are delineated by blank lines.
    
-   Groups must respect following order:
    
    -   Polyfills (i.e. import 'reflect-metadata';)
        
    -   Node builtin modules (i.e. import fs from 'fs';)
        
    -   external modules (i.e. import { query } from 'itiriri';)
        
    -   internal modules (i.e import { UserService } from 'src/services/userService';)
        
    -   modules from a parent directory (i.e. import foo from '../foo'; import qux from '../../foo/qux';)
        
    -   modules from the same or a sibling's directory (i.e. import bar from './bar'; import baz from './bar/baz';)
        

**Bad:**

```js
import { TypeDefinition } from '../types/typeDefinition';
import { AttributeTypes } from '../model/attribute';
import { Customer, Credentials } from '../model/types';
import { ApiCredentials, Adapters } from './common/api/authorization';
import fs from 'fs';
import { ConfigPlugin } from './plugins/config/configPlugin';
import { BindingScopeEnum, Container } from 'inversify';
import 'reflect-metadata';
```

**Good:**

```js
import 'reflect-metadata';

import fs from 'fs';
import { BindingScopeEnum, Container } from 'inversify';

import { AttributeTypes } from '../model/attribute';
import { TypeDefinition } from '../types/typeDefinition';
import type { Customer, Credentials } from '../model/types';

import { ApiCredentials, Adapters } from './common/api/authorization';
import { ConfigPlugin } from './plugins/config/configPlugin';
```

### Use typescript aliases

Create prettier imports by defining the paths and baseUrl properties in the compilerOptions section in the tsconfig.json

This will avoid long relative paths when doing imports.

**Bad:**

```js
import { UserService } from '../../../services/UserService';
```

**Good:**

```js
import { UserService } from '@services/UserService';
```

```js
// tsconfig.json
...
  "compilerOptions": {
    ...
    "baseUrl": "src",
    "paths": {
      "@services": ["services/*"]
  }
    ...
  }
...
```

## Comments

The use of a comments is an indication of failure to express without them. Code should be the only source of truth.

> Don’t comment bad code—rewrite it.
> 
> — _Brian W. Kernighan and P. J. Plaugher_

### Prefer self explanatory code instead of comments

Comments are an apology, not a requirement. Good code _mostly_ documents itself.

**Bad:**

```js
// Check if subscription is active.
if (subscription.endDate > Date.now) {
}
```

**Good:**
```js
const isSubscriptionActive = subscription.endDate > Date.now;
if (isSubscriptionActive) {
  /* ... */
}
```

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**Bad:**

```js
type User = {
  name: string;
  email: string;
  // age: number;
  // jobPosition: string;
};
```

**Good:**

```js
type User = {
  name: string;
  email: string;
};
```

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code, and especially journal comments. Use git log to get history!

**Bad:**

```js
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Added type-checking (LI)
 * 2015-03-14: Implemented combine (JR)
 */
function combine(a: number, b: number): number {
  return a + b;
}
```

**Good:**

```js
function combine(a: number, b: number): number {
  return a + b;
}
```

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the proper indentation and formatting give the visual structure to your code.

Most IDE support code folding feature that allows you to collapse/expand blocks of code (see Visual Studio Code [folding regions](https://code.visualstudio.com/updates/v1_17#_folding-regions)).

**Bad:**

```js
////////////////////////////////////////////////////////////////////////////////
// Client class
////////////////////////////////////////////////////////////////////////////////
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  ////////////////////////////////////////////////////////////////////////////////
  // public methods
  ////////////////////////////////////////////////////////////////////////////////
  public describe(): string {
    // ...
  }

  ////////////////////////////////////////////////////////////////////////////////
  // private methods
  ////////////////////////////////////////////////////////////////////////////////
  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
}
```

**Good:**

```js
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  public describe(): string {
    // ...
  }

  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
}
```

### TODO comments

When you find yourself that you need to leave notes in the code for some later improvements,

do that using // TODO comments. Most IDE have special support for those kinds of comments so that

you can quickly go over the entire list of todos.

Keep in mind however that a _TODO_ comment is not an excuse for bad code.

**Bad:**

```js
function getActiveSubscriptions(): Promise<Subscription[]> {
  // ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**Good:**

```js
function getActiveSubscriptions(): Promise<Subscription[]> {
  // TODO: ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```
