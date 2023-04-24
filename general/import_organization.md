### Organize imports

With clean and easy to read import statements you can quickly see the dependencies of current code. Make sure you apply following good practices for import statements:

-   Import statements should be alphabetized and grouped.
    
-   Unused imports should be removed.
    
-   Named imports must be alphabetized (i.e. import {A, B, C} from 'foo';)
    
-   Import sources must be alphabetized within groups, i.e.: import * as foo from 'a'; import * as bar from 'b';
    
-   Prefer using import type instead of import when importing only types from a file to avoid dependency cycles, as these imports are erased at runtime
    
-   Groups of imports are delineated by blank lines.
    
-   Groups must respect following order:
    
    -   Polyfills (i.e. import 'reflect-metadata';)
        
    -   Node builtin modules (i.e. import fs from 'fs';)
        
    -   external modules (i.e. import { query } from 'itiriri';)
        
    -   internal modules (i.e import { UserService } from 'src/services/userService';)
        
    -   modules from a parent directory (i.e. import foo from '../foo'; import qux from '../../foo/qux';)
        
    -   modules from the same or a sibling's directory (i.e. import bar from './bar'; import baz from './bar/baz';)
        

**Bad:**

```js
import { TypeDefinition } from '../types/typeDefinition';
import { AttributeTypes } from '../model/attribute';
import { Customer, Credentials } from '../model/types';
import { ApiCredentials, Adapters } from './common/api/authorization';
import fs from 'fs';
import { ConfigPlugin } from './plugins/config/configPlugin';
import { BindingScopeEnum, Container } from 'inversify';
import 'reflect-metadata';
```

**Good:**

```js
import 'reflect-metadata';

import fs from 'fs';
import { BindingScopeEnum, Container } from 'inversify';

import { AttributeTypes } from '../model/attribute';
import { TypeDefinition } from '../types/typeDefinition';
import type { Customer, Credentials } from '../model/types';

import { ApiCredentials, Adapters } from './common/api/authorization';
import { ConfigPlugin } from './plugins/config/configPlugin';
```
