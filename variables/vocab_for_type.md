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
