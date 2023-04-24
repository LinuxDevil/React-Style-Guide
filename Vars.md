
## Use meaningful variable names

Distinguish names in such a way that the reader knows what the differences offer.

**Bad:**

```js
function between<T>(a1: T, a2: T, a3: T): boolean {
  return a2 <= a1 && a1 <= a3;
}
```

**Good:**

```js
function between<T>(value: T, left: T, right: T): boolean {
  return left <= value && value <= right;
}
```

## Use pronounceable variable names

If you can’t pronounce it, you can’t discuss it without sounding like an idiot.

**Bad:**

```js
type DtaRcrd102 = {
  genymdhms: Date;
  modymdhms: Date;
  pszqint: number;
};
```

**Good:**

```js
type Customer = {
  generationTimestamp: Date;
  modificationTimestamp: Date;
  recordId: number;
};
```

## Use the same vocabulary for the same type of variable

**Bad:**

```js
function getUserInfo(): User;
function getUserDetails(): User;
function getUserData(): User;
```

**Good:**

```js
function getUser(): User;
```

## Use searchable names

We will read more code than we will ever write. It's important that the code we do write must be readable and searchable. By _not_ naming variables that end up being meaningful for understanding our program, we hurt our readers. Make your names searchable. Tools like [ESLint](https://typescript-eslint.io/) can help identify unnamed constants.

**Bad:**

```js
// What the heck is 86400000 for?
setTimeout(restart, 86400000);
```

**Good:**

```js
// Declare them as capitalized named constants.
const MILLISECONDS_PER_DAY = 24 * 60 * 60 * 1000; // 86400000

setTimeout(restart, MILLISECONDS_PER_DAY);
```

## Use explanatory variables

**Bad:**

```js
declare const users: Map<string, User>;

for (const keyValue of users) {
  // iterate through users map
}
```

**Good:**

```js
declare const users: Map<string, User>;

for (const [id, user] of users) {
  // iterate through users map
}
```

## Avoid Mental Mapping

Explicit is better than implicit. _Clarity is king._

**Bad:**

```js
const u = getUser();
const s = getSubscription();
const t = charge(u, s);
```

**Good:**

```js
const user = getUser();
const subscription = getSubscription();
const transaction = charge(user, subscription);
```

## Don't add unneeded context

If your class/type/object name tells you something, don't repeat that in your variable name.

**Bad:**

```js
type Car = {
  carMake: string;
  carModel: string;
  carColor: string;
};

function print(car: Car): void {
  console.log(`${car.carMake} ${car.carModel} (${car.carColor})`);
}
```

**Good:**

```js
type Car = {
  make: string;
  model: string;
  color: string;
};

function print(car: Car): void {
  console.log(`${car.make} ${car.model} (${car.color})`);
}
```

## Use default arguments instead of short circuiting or conditionals

Default arguments are often cleaner than short circuiting.

**Bad:**

```js
function loadPages(count?: number) {
  const loadCount = count !== undefined ? count : 10;
  // ...
}
```

**Good:**

```js
function loadPages(count: number = 10) {
  // ...
}
```

## Use enum to document the intent

Enums can help you document the intent of the code. For example when we are concerned about values being  
different rather than the exact value of those.

**Bad:**

```js
const GENRE = {
  ROMANTIC: 'romantic',
  DRAMA: 'drama',
  COMEDY: 'comedy',
  DOCUMENTARY: 'documentary'
};

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
      // some logic to be executed
    }
  }
}
```

**Good:**

```js
enum GENRE {
  ROMANTIC,
  DRAMA,
  COMEDY,
  DOCUMENTARY
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
      // some logic to be executed
    }
  }
}
```
