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
