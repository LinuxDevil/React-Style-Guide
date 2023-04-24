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
