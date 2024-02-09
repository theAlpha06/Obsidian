---
link: "[[Async & await]]"
---

<mark>Promise is a placeholder which will be filled later with a value. In other words, it's a container for future value. Moreover, <span style="background-color: red;color: white">Promise is an object representing the eventual completion or failure of an asynchronous operation.</span></mark>
- Promises are used to handle async operations in javascript.
- Promise objects are immutable, if it's resolved.

```js
const cart = ["shoes", "pants", "kurta"];

// Using Callbacks is not a good idea.
createOrder(cart, function() {
	proceedToPayment(orderId);
}); 

// Using Promises
const promise = createOrder(cart);

// This will return an object with some data value in it. This data value will hold whatever this createOrder api will return.

//{data: undefined}
```
<mark>Whenever JS engine executes `const promise = createOrder(cart)  the `createOrder` will return us a `Promise`(nothing but an object) and program will go on executing. This promise object will be filled with data automatically and we will have order details in it.</mark>

> We attach a callback function to the promise object so that it can run once the data has been fetched. 

```js
// {data: orderDetails}

promise.then(function(orderId) {
	proceedToPayment(orderId);
});
```

- As soon as, the data is filled in the `Promise object` it will call the attached callback function with probability 1. (Guaranteed by JS). 

```js
const GITHUB_API = "https://api.github.com/users/theAlpha06";

cosnt user = fetch(GITHUB_API);

console.log(user);
```

### Handling callback hell using `Promise Chaining`

```js
const cart = ["shoes", "pants", "kurta"];

createOrder(cart, function(orderId) {
	proceedToPayment(orderId, function(paymentInfo) {
		showOrderSummary(paymentInfo, function() {
			updateWalletBalance();
		})
	})
})
```

Using `Promise`

```js
createOrder(cart)
.then(function(orderId) {
	return proceedToPayment(orderId);
})
.then(function(paymentInfo) {
	return showOrderSummary(paymentInfo);
})
.then(function (paymentInfo) {
	return updateWalletBalance(paymentInfo);
})
```

> Always return a Promise for further piping of data.

### `createOrder` function

```js

const cart = ["shoes", "pants", "kurta"];

createOrder(cart)
.then(function(orderId) {
	console.log(orderId);
	return orderId;
})
.then(function(orderId) {
	return proceedToPayment(orderId)
})
.then(function(paymentInfo) {
	console.log(paymentInfo);
})
.catch(function (err) {
	console.log(err.message);
})

// Attach callback function for error
// .catch() only handles the error for above .then codes.

function createOrder(cart) {
	const pr = new Promise(function(resolve, reject) {
		// orderId
		if(!validateCart(cart)) {
			reject(new Error("Cart validation failed"));
		} 
		// logic for createOrder
		const orderId = "12345";
		if(orderId) {
			resolve(orderId);
		} 
	});
	return pr;
}

function validateCart(cart) {
	return true;
}

function proceedToPayment(orderId) {
	return new Promise(function(resolve, reject) {
		resolve("Payment Successful");
	})
}
```