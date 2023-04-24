### Encapsulate conditionals

**Bad:**

```js
if (subscription.isTrial || account.balance > 0) {
  // ...
}
```

**Good:**

```js
function canActivateService(subscription: Subscription, account: Account) {
  return subscription.isTrial || account.balance > 0;
}

if (canActivateService(subscription, account)) {
  // ...
}
```

