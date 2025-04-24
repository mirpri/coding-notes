# JavaScript

> **Reference**
>
> [ES6 Tutorial](https://www.javascripttutorial.net/es6/)
> [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript)


## Deciding `let` or `var`
`let` is **block-scoped** and should be used in modern JavaScript for variable declarations. It prevents issues like variable hoisting and accidental re-declarations.  
`var` is **function-scoped** and declarations are hoisted to the top of their scope, meaning you can use a var variable before its declaration in the code (though it will have a value of undefined). It is generally avoided in modern code due to its less predictable behavior.

**Prefer let or const over var**: let and const provide better scope management and are more explicit about variable mutability (whether the value can be changed).

```js
for(let v=0;v<5;v++){
    setTimeout(() => {
        console.log(v);
    }, 1000);
}
```
output: `0 1 2 3 4`
```js
for(var v=0;v<5;v++){
    setTimeout(() => {
        console.log(v);
    }, 1000);
}
```
output: `5 5 5 5 5`

## `==` and `===` compared
`==` performs **type coercion**, meaning it converts the operands to the same type before comparing.  
`===` is a strict equality operator that compares both value and type, making it more predictable and preferred in most cases.

## Array
Arrays in JavaScript are used to store multiple values in a single variable. They are zero-indexed and come with built-in methods like `push`, `pop`, `map`, `filter`, and `reduce` for manipulation.

Creating an array with size 10:
```js
let a=Array(10); // array of 10 empty item
let b=Array(10).fill(0); // array of 10 zeros
```

## Promise
A JavaScript Promise is an object that represents the eventual completion (or failure) of an asynchronous operation and its resulting value. It's a way to handle operations that might take some time to complete, like fetching data from an API or reading a file.

Here is an simple example:
```ts
function judgeOddNumber(x: number) {
  return new Promise<string>((resolve, reject) => {
    if (x % 2 !== 0) {
      resolve(x+" is odd number"); //simulate success
    }
    else {
      reject(x+" is even number"); //simulate an error
    }
  });
}


// const promise = hi;
function successCallback(result: string) {
  console.log(`success: ${result}`);
}

function failureCallback(error: string) {
  console.error(`error: ${error}`);
}

judgeOddNumber(3).then(successCallback, failureCallback);
judgeOddNumber(10).then(successCallback, failureCallback);
```
`Promise` objects manage asynchronous execution flow based on operation outcomes. When the operation succeeds and `resolve()` is invoked, the Promise transitions to fulfilled state, executing the first callback passed to `.then()` (in this example, `successCallback`). Conversely, when the operation fails and `reject()` is called, the Promise enters rejected state, triggering the second callback in `.then()` (here, `failureCallback`). 
JavaScript Promises offer several key benefits when dealing with asynchronous operations:

**1. Improved Readability and Maintainability:**

- flatten nested callback, avoid *"Callback Hell"* structures. 

- chaining with .then() provides a clear, sequential flow for asynchronous operations. 

**2. Enhanced Error Handling:**

- Centralized Error Catching with `.catch()` at the end

- Provide a consistent way to handle errors in asynchronous code. 

**3. Facilitates Asynchronous Code Management:**

- Chain multiple asynchronous operations in a specific order. 

Parallel Execution (with Promise.all()): You can run multiple asynchronous tasks concurrently using Promise.all() and wait for all of them to resolve before proceeding. 

- Allow for more flexible composition of asynchronous operations. 

**4. Integration with Modern Features:**

- *Foundation for the async/await syntax*, which simplifies asynchronous code further. 