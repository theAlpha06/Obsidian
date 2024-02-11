---
link: "[[this]]"
---

### `Promise.all`

- We will pass an iterable(e.g. array).
- Return an array with the results of the API calls.
- It will wait for all of the promises to finish.
- As soon as any of the API call will be rejected, the output will be error which was returned by the rejected API. It will not even wait for other promises, it will throw error as soon as it is encountered.
- All other promises depends on their fate if they are accepted or not. But, `Promise.all()` fails.

```js
Promise.all([p1, p2, p3]);
```
### If I want results of only `settled` promise?

```js
Promise.allSetteled([p1, p2, p3]);

// returns an object of array
```

> Even if `p2` is rejected it will wait for other promises to get settled, irrespective of success or failure.

### `Promise.race()`

```js
Promise.race([p1, p2, p3]);
```

> As soon as first promise is settled, it will give the value of the first settled promise.

### `Promise.any()`

> Whenever, the promise is resolved only then the result will be returned.
> If all of the promise is failed, then an `AggregateError` is thrown, which contains all the errors.

> When we say that a promise is `settled` this means it's either `resolved` or `rejected`.

