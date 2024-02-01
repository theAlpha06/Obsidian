---
link: "[[JS Engine]]"
---

# Starved Event Loop

Below is an example of how code running in the current Call Stack can prevent
code on the Event Loop from being executed. aka; the Event Loop is _starved_.

---

Given the code

```javascript
setTimeout(() => { 
  console.log('bye')
}, 2)           
someSlowFn()
console.log('hi')
```

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  |                   |              | |               |
    console.log('bye')|                   |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |
```
To start, everything is empty

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | <global>          |              | |               |
    console.log('bye')|                   |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |
```
It starts executing the code, and pushes that fact onto the Call Stack (here named
`<global>`)

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
> setTimeout(() => {  | <global>          |              | |               |
    console.log('bye')| setTimeout        |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |
```
`setTimeout` is pushed onto the Call Stack

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
> setTimeout(() => {  | <global>          |              | | timeout, 2    |
    console.log('bye')| setTimeout        |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |
```
`setTimeout` triggers the timeout Web API

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | <global>          |              | | timeout, 2    |
    console.log('bye')|                   |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |
```
`setTimeout` is then finished executing, while the Web API waits for the
requested amount of time (2ms).

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | <global>          |              | | timeout, 2    |
    console.log('bye')| someSlowFn        |              | |               |
  }, 2)               |                   |              | |               |
> someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |
```

`someSlowFn` starts executing. Let's pretend this takes around 300ms to
complete. For that 300ms, JS can't remove it from the Call Stack

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | <global>          | function   <-----timeout, 2    |
    console.log('bye')| someSlowFn        |              | |               |
  }, 2)               |                   |              | |               |
> someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |
```
Meanwhile, the timeout has expired, so the Web API lets JS know by adding code
to the Event Loop.

`someSlowFn` is still executing on the Call Stack, and cannot be interrupted,
so the code to be executed by the timeout waits on the Event Loop for its turn.

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | <global>          | function     | |               |
    console.log('bye')| someSlowFn        |              | |               |
  }, 2)               |                   |              | |               |
> someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |
```

Still waiting for `someSlowFn` to finish...

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | <global>          | function     | |               |
    console.log('bye')|                   |              | |               |
  }, 2)               |                   |              | |               |
> someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |
```
`someSlowFn` finally finished!

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | <global>          | function     | |               |
    console.log('bye')| console.log       |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
> console.log('hi')   |                   |              | |               |
```

The next line is executed, pushing
`console.log` onto the Call Stack

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | <global>          | function     | |               |
    console.log('bye')|                   |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
> console.log('hi')   |                   |              | |               |

> hi
```

We see `hi` output on the console thanks to `console.log`

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  |                   | function     | |               |
    console.log('bye')|                   |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |

> hi
```

Nothing left to execute, so the special `<global>` is popped off the Call Stack.

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | function        <---function     | |               |
    console.log('bye')|                   |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |

> hi
```

This frees up the JS execution environment to check the Event Loop for any code
which needs to be executed.

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | function          |              | |               |
>   console.log('bye')| console.log       |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |

> hi
```
Executing the function results in `console.log` being called, also pushed onto
the Call Stack.

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | function          |              | |               |
    console.log('bye')|                   |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |

> hi
> bye
```

Once finished executing, `bye` is printed, and `console.log` is removed from the
Call Stack.

Notice that by this point, it is at least 300ms _after_ the code originally
requested the `setTimeout`. Meaning even though we asked for it to be executed
after only 2ms, we still had to wait for the Call Stack to empty before the
`setTimeout` code on the Event Loop could be executed

_Note: Even if we didn't have `someSlowFn`, `setTimeout` is clamped to 4ms as
the mimimum delay allowed in some cases_

---

```js
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  |                   |              | |               |
    console.log('bye')|                   |              | |               |
  }, 2)               |                   |              | |               |
  someSlowFn()        |                   |              | |               |
  console.log('hi')   |                   |              | |               |

> hi
> bye
```
Finally, there are no other commands to execute, so it too is taken off the Call
Stack.

Our program has now finished execution.

End.

_Note: It's also possible to [starve the event loop with Promises](https://gist.github.com/jesstelford/bbb30b983bddaa6e5fef2eb867d37678) via the "Microtask queue"_