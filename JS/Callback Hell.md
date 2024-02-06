- Use of callback in asynchronous behaviour

```js

const cart = [ "shoes", "pants", "kurta" ];

api.createOrder(cart, function() {
	api.proceedToPayment(function () {
		api.showOrderSummary();
	})
})
```

In the above given code snippet the functions `createOrder`, `proceedToPayment` and `showOrderSummary` will run only after the former function has completed it's execution and will then call the callback function.

- What if we have another function `updateWallet`, now the code is starting growing horizontally instead of vertical. This is called `callback hell`.
- Unreadable & unmaintainable.
- This structure is also know as `Pyramid of DOOM`.
- Another problem with callback's is `inversion of control`. In this <mark>you loose control over your code.</mark>

```js

api.createOrder(cart, function() {
	api.proceedToPayment();
})
```

> <mark style="background-color:red;color: white">Here, we have given the control of `proceedToPayment` to `createOrder` function which is very risky. What if the callback function was never called.</mark> 
