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
