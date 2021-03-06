## Description
Easy redux setup for react.
## Installation
### `yarn add react-reqq`
*its not a typo, its just thicc*

#### Feature Logs:
#### 0.3.7
* Added `cacheStorage` option in configureApi
  * INDEXEDDB (buggy)
  * LOCALSTORAGE
  * WEBSQL
* Added `clearCache()` function in `req`
* Added `cacheKey` option in `get`, `list` and `show`
#### 0.3.6
* Added `cancelAll()` function in `req`
#### 0.3.X
* Added Header override
* *Added transform state

## Setup
### Provider Setup
~~~~
import { configureApi } from 'react-reqq';

export const store = configureApi({
  endpoint: process.env.REACT_APP_END_POINT,
});

// import { Provider } from 'react-redux';
<Provider store={store}>
  ...
</Provider>
~~~~

### Import `req`
~~~~
import { req } from 'react-reqq`
~~~~

### GET
~~~~
req.get({
  key: 'foo',
  url: '/users',
  params: {
    q: 'juan',
  }
});
~~~~

### How to get the data
`useSelector` of `react-redux` or use the `helpers` *see below.
~~~~
import { useSelector } from 'react-redux';

const data = useSelector((state) => state.api.foo);
~~~~

### POST
~~~~
req.post({
  key: 'foo',
  url: '/users',
  payload: {
    name: 'juan',
  }
});
~~~~

### PUT
~~~~
req.put({
  key: 'foo',
  url: '/users/1',
  payload: {
    name: 'juan',
  }
});
~~~~

### REMOVE
~~~~
req.remove({
  key: 'foo',
  url: '/users/1'
});
~~~~

### SET
~~~~
req.set('isFoo', true);
~~~~

### LIST/SHOW
*must have `id` property in the response*
~~~~
req.list({
  key: 'foo',
  url: '/users',
  transform: res => ({
    data: res.users,      // array
    meta: res.meta, // for pagination *optional for meta
  }),
});
req.show({
  key: 'foo',
  url: '/users/1'
  id: 1,
});
~~~~

### React Hooks Helpers
useApiShow(`key`, `id`)
~~~~
import { useApiShow } from 'react-reqq';

const data = useApiShow('foo', '1');
~~~~
useApiLoading(`key`, `get|post|put|remove|list|?id`)
~~~~
import { useApiLoading } from 'react-reqq';

const isLoading = useApiLoading('foo', 'get');
~~~~

useApiList(`key`)
~~~~
import { useApiList } from 'react-reqq';

const [list, meta] = useApiList('foo');
~~~~

useApiShow(`key`, `id`)
~~~~
import { useApiShow } from 'react-reqq';

const data = useApiShow('foo', '1');
~~~~

### When to use LIST+SHOW instead of GET
Use `.list`+`.show` for data with `id`, `.show` updates the `.list` item if `id` is found in `.list`'s response.

### Other
#### configureApi
* endpoint [string] - Root endpoint [*http://localhost:8000*]
* requestHeaders [function] - Set headers on request.
* onError [function] - When an error occured during request.

#### GET | POST | PUT | REMOVE | LIST | SHOW
~~~~
  req.get({
    ...
    // Calls on success response
    onSuccess: () => { ... },

    // Calls on error response
    onError: () => { ... },
  });
~~~~

#### GET | LIST | SHOW
~~~~
  req.get({
    ...
    // Transforms response before storing
    transform: () => {
      // transform data here
      return { ... };
    },

    // Cache response to local storage. `false` clears cache
    cache: true,
  });
~~~~