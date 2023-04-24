### Avoid Side Effects (part 2)

Browsers and Node.js process only JavaScript, therefore any TypeScript code has to be compiled before running or debugging. In JavaScript, some values are unchangeable (immutable) and some are changeable (mutable). Objects and arrays are two kinds of mutable values so it's important to handle them carefully when they're passed as parameters to a function. A JavaScript function can change an object's properties or alter the contents of an array which could easily cause bugs elsewhere.

Suppose there's a function that accepts an array parameter representing a shopping cart. If the function makes a change in that shopping cart array - by adding an item to purchase, for example - then any other function that uses that same cart array will be affected by this addition. That may be great, however it could also be bad. Let's imagine a bad situation:

The user clicks the "Purchase" button which calls a purchase function that spawns a network request and sends the cart array to the server. Because of a bad network connection, the purchase function has to keep retrying the request. Now, what if in the meantime the user accidentally clicks an "Add to Cart" button on an item they don't actually want before the network request begins? If that happens and the network request begins, then that purchase function will send the accidentally added item because the cart array was modified.

A great solution would be for the addItemToCart function to always clone the cart, edit it, and return the clone. This would ensure that functions that are still using the old shopping cart wouldn't be affected by the changes.

Two caveats to mention to this approach:

1.  There might be cases where you actually want to modify the input object, but when you adopt this programming practice you will find that those cases are pretty rare. Most things can be refactored to have no side effects! (see [pure function](https://en.wikipedia.org/wiki/Pure_function))
    
2.  Cloning big objects can be very expensive in terms of performance. Luckily, this isn't a big issue in practice because there are [great libraries](https://github.com/immutable-js/immutable-js) that allow this kind of programming approach to be fast and not as memory intensive as it would be for you to manually clone objects and arrays.
    

**Bad:**

```js
function addItemToCart(cart: CartItem[], item: Item): void {
  cart.push({ item, date: Date.now() });
}
```

**Good:**

```js
function addItemToCart(cart: CartItem[], item: Item): CartItem[] {
  return [...cart, { item, date: Date.now() }];
}
```
