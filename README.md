
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

### [Use getters and setters](/objects/getters_and_setters.md)
### [Make objects have private/protected members](/objects/access_modifiers.md)
### [Prefer immutability](/objects/close_for_modification.md)
### [type vs. interface](/objects/being_loosly_coupled.md)

---

## Classes

### [Classes should be small](/classes/classes_should_be_thin_and_do_one_thing.md)
### [High cohesion and low coupling](/classes/cohesion.md)
### [Prefer composition over inheritance](/classes/composition_over_inheritance.md)
### [Use method chaining](/classes/method_chaining.md)

----

## SOLID

### [Single Responsibility Principle (SRP)](/SOLID/SRP.md)
### [Open/Closed Principle (OCP)](/SOLID/OCP.md)
### [Liskov Substitution Principle (LSP)](/SOLID/LSP.md)
### [Interface Segregation Principle (ISP)](/SOLID/ISP.md)
### [Dependency Inversion Principle (DIP)](/SOLID/DIP.md)

----

## Testing

Testing is more important than shipping. If you have no tests or an inadequate amount, then every time you ship code you won't be sure that you didn't break anything.  
Deciding on what constitutes an adequate amount is up to your team, but having 100% coverage (all statements and branches)  
is how you achieve very high confidence and developer peace of mind. This means that in addition to having a great testing framework, you also need to use a good [coverage tool](https://github.com/gotwarlost/istanbul).

There's no excuse to not write tests. There are [plenty of good JS test frameworks](http://jstherightway.org/#testing-tools) with typings support for TypeScript, so find one that your team prefers. When you find one that works for your team, then aim to always write tests for every new feature/module you introduce. If your preferred method is Test Driven Development (TDD), that is great, but the main point is to just make sure you are reaching your coverage goals before launching any feature, or refactoring an existing one.

### The three laws of TDD

1.  You are not allowed to write any production code unless it is to make a failing unit test pass.
    
2.  You are not allowed to write any more of a unit test than is sufficient to fail, and; compilation failures are failures.
    
3.  You are not allowed to write any more production code than is sufficient to pass the one failing unit test.
    

### [F.I.R.S.T. rules](/testing/first_rule.md)
### [Single concept per test](/testing/single_concept_per_Test.md)
### [The name of the test should reveal its intention](/testing/naming.md)

---
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
