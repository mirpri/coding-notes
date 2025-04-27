# JavaScript

> 📜 **Reference**
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

## type
JavaScript uses dynamic typing. Variable's type is set when you assign a value to it and be automatically coverted during operations.

You cannot explicitly assign types to variables. ([TypeScript](../typescript) can do that!)

> 🧩 Dynamic typing provides convenience and flexibility but may also lead to confusion. Some TypeScript will be used in this document to annotate types to make it clearer.

### Checking types
use `typeof variable` to check the type.
```js
console.log(typeof 10) // number
console.log(typeof "10") // string
console.log(typeof true) // boolean
console.log(typeof undefined) // undefined
console.log(typeof null) // object

let a="10"
let b=10
console.log(typeof a) // string
console.log(typeof b) // number
console.log(typeof (a+b)) // string
console.log(typeof (a-b)) // number
```

### `==` and `===` compared
`==` performs **type coercion**, meaning it converts the operands to the same type before comparing.  
`===` is a strict equality operator that compares both value and type, making it more predictable and preferred in most cases.

## Array
Arrays in JavaScript are used to store multiple values in a single variable. They are zero-indexed and come with built-in methods like `push`, `pop`, `map`, `filter`, and `reduce` for manipulation.

JavaScript arrays are resizable and **can contain a mix of different data types**. (When those characteristics are undesirable, use typed arrays instead.)

### Create
Creating an array with size 10:
```js
let a=Array(10); // array of 10 empty slots
let b=Array(10).fill(0); // array of 10 zeros
```
`a=Array(10)` is the same as:
```js
let a=[];
a.length=10;
//a.fill(0) won't work here!
```
Both ways create an array with 10 empty slots — not undefined, not null, just empty (sparse).


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

### Creating a promise chain
```ts
function addOne(x: number) {
  return new Promise<number>((resolve, reject) => {
    if (x > 0) resolve(x + 1);
    else reject("x is not positive");
  });
}

function multiplyByTwo(x: number) {
  return new Promise<number>((resolve, reject) => {
    if (x > 0) resolve(x * 2);
    else reject("x is not positive");
  });
}

function outputResult(result: number) {
  console.log(`The result is ${result}`);
}

function outputError(error: string) {
  console.error(`Error: ${error}`);
}

addOne(5).then(multiplyByTwo).then(addOne)
  .then(outputResult).catch(outputError);
// (5+1)*2+1=13

addOne(-5).then(multiplyByTwo).then(addOne)
  .then(outputResult).catch(outputError);
// Error
```
### `then` and `catch`
You can use the second parameter of `.then()` or `.catch()` to handle errors.

Use `.catch()` at the end of a chain to provide centralized error handling is cleaner and avoids repetitive error handling code.
Use `.then()` with Two Parameters for Local Error Handling immediately within the same `.then()`.

Priorty:
- The first parameter of `.then()` (the success handler) has the highest priority. It is called if the Promise resolves successfully.
- The second parameter of `.then()` (the failure handler) has the next highest priority. It is called if the Promise rejects and a failure handler is provided.
- The trailing `.catch()` has the lowest priority. It is used to handle failures and catches errors from anywhere in the chain, including errors thrown in the success handlers of .then().

```ts
function output1() { console.log("output1!"); }
function output2() { console.log("output2!"); }
function output3() { console.log("output3!"); }

addOne(-5).then(output1, output2).catch(output3);
// output2!
```
### Summary
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