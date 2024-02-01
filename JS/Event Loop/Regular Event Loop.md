---
link: "[[Starved Event Loop]]"
---

# Regular Event Loop

This shows the execution order given JavaScript's Call Stack, Event Loop, and
any asynchronous APIs provided in the JS execution environment (in this example;
Web APIs in a Browser environment)

---

Given the code

```javascript
setTimeout(() => { 
  console.log('hi')
}, 1000)           
```

The Call Stack, Event Loop, and Web APIs have the following relationship

```text
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  |                   |              | |               |
    console.log('hi') |                   |              | |               |
  }, 1000)            |                   |              | |               |
                      |                   |              | |               |
```
To start, everything is empty

---

```text
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | <global>          |              | |               |
    console.log('hi') |                   |              | |               |
  }, 1000)            |                   |              | |               |
                      |                   |              | |               |
```
It starts executing the code, and pushes that fact onto the Call Stack (here named
`<global>`)

---

```text
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
> setTimeout(() => {  | <global>          |              | |               |
    console.log('hi') | setTimeout        |              | |               |
  }, 1000)            |                   |              | |               |
                      |                   |              | |               |
```
Then the first line is executed. This pushes the function execution as the
second item onto the call stack.

Note that the Call Stack is a _stack_; The last item pushed on is the first
item popped off. Aka: Last In, First Out. (think; a stack of dishes)

---

```text
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
> setTimeout(() => {  | <global>          |              | | timeout, 1000 |
    console.log('hi') | setTimeout        |              | |               |
  }, 1000)            |                   |              | |               |
                      |                   |              | |               |
```
Executing `setTimeout` actually calls out to code that is _not_ part of JS.
It's part of a _Web API_ which the browser provides for us.
There are a different set of APIs like this available in node.

---

```text
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | <global>          |              | | timeout, 1000 |
    console.log('hi') |                   |              | |               |
  }, 1000)            |                   |              | |               |
                      |                   |              | |               |
```
`setTimeout` is then finished executing; it has offloaded its work to the Web
API which will wait for the requested amount of time (1000ms).

---

```text
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  |                   |              | | timeout, 1000 |
    console.log('hi') |                   |              | |               |
  }, 1000)            |                   |              | |               |
                      |                   |              | |               |
```

As there are no more lines of JS to execute, the Call Stack is now empty.

---

```text
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  |                   | function   <-----timeout, 1000 |
    console.log('hi') |                   |              | |               |
  }, 1000)            |                   |              | |               |
                      |                   |              | |               |
```
Once the timeout has expired, the Web API lets JS know by adding code to the
Event Loop.

It doesn't push onto the Call Stack directly as that could intefere with already
executing code, and you'd end up in weird situations.

The Event Loop is a _Queue_. The first item pushed on is the first
item popped off. Aka: First In, First Out. (think; a queue for a movie)

---

```text
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | function        <---function     | |               |
    console.log('hi') |                   |              | |               |
  }, 1000)            |                   |              | |               |
                      |                   |              | |               |
```
Whenever the Call Stack is empty, the JS execution environment occasionally checks
to see if anything is Queued in the Event Loop. If it is, the first item is moved
to the Call Stack for execution.

---

```text
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | function          |              | |               |
>   console.log('hi') | console.log       |              | |               |
  }, 1000)            |                   |              | |               |
                      |                   |              | |               |
```
Executing the function results in `console.log` being called, also pushed onto
the Call Stack.

---

```text
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  | function          |              | |               |
    console.log('hi') |                   |              | |               |
  }, 1000)            |                   |              | |               |
                      |                   |              | |               |
> hi
```
Once finished executing, `hi` is printed, and `console.log` is removed from the
Call Stack.

---

```text
        [code]        |   [call stack]    | [Event Loop] | |   [Web APIs]  |
  --------------------|-------------------|--------------| |---------------|
  setTimeout(() => {  |                   |              | |               |
    console.log('hi') |                   |              | |               |
  }, 1000)            |                   |              | |               |
                      |                   |              | |               |
> hi
```
Finally, the function has no other commands to execute, so it too is taken off
the Call Stack.

Our program has now finished execution.

End.