---
link: "[[Scope]]"
---
# `window`

- It's a global object which is created along with the execution context.
- This is the task of JS engine to create the global object. `window` for browser and `global` for Node.js;

```js
this === window; // true
```

- Whenever we create any function or variable in global scope they get attached to the window object.
- `Not defined` v/s `undefined`
- Loosely typed
```js
a = undefined; // not recommended
// This means that it was not touched by the developer inside the code.
```

# `this`

- It also created during the creation of execution context.
