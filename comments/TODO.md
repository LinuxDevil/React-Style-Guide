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
