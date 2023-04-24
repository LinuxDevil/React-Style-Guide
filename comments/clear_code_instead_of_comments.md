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
