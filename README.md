## Usage Example

### Initialisation

```js
const redis = require("redis").createClient();
const redisAdapter = require("redis-controller-adapter");
//initialise adapter with the redis client
redisAdapter.init(redis);
//set default expiry date for all keys
//can be overridden by providing induvidual expire time for each cache key in config
redisAdapter.setDefaultExpiry(60);
```

### Usage

```js
const redisAdapter = require("redis-controller-adapter");

class Example {
  constructor() {}

  getAll() {
    return new Promise((resolve, reject) => {
      resolve(["a", "b", "c"]);
    });
  }

  get(id) {
    return new Promise((resolve, reject) => {
      resolve({ url: id });
    });
  }
}

module.exports = redisAdapter.use(new Example(), {
  get: { cacheKey: "example:{0}" },
  getAll: { cacheKey: "examples", expiry: 20 }
});
```

### Configuration

The function of the controller which requires caching will be the key and an object containing the format of the cache key and it's expiry (optional) will be the value.

```js
const config = {
  get: { cacheKey: "example:{0}" },
  getAll: { cacheKey: "examples", expiry: 20 }
};
```

#### Example

Key - example:{0}:{1}
Function - get(id, value)

calling controller.get(1, 2); will produce example:1:2
