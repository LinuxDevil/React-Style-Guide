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
