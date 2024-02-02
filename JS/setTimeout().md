- setTimeout() might take more than given time, it all depends on the `call stack`.

```js
console.log("Start");

setTimeout(function cb() {
	console.log("Callback");
}, 5000);

console.log("End");

// Blocking the main thread for 10s
let startDate = new Date().getTime();
let endDate = startDate;
while(endDate < startDate + 10000) {
  endDate = new Date().getTime();
}

console.log("While expires");
```


### setTimeout() with 0 ms

```js
console.log("Start");

setTimeout(function cb() {
	console.log("Callback");
}, 0);

console.log("End");
```

<kbd>
	Console
</kbd>

```console
Start
End
Callback
```

> We can use this to `defer` the function so that once every thing in main thread has been completed.