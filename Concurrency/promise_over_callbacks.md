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
