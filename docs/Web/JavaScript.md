# JavaScript

## Deciding `let` or `var`
`let` is **block-scoped** and should be used in modern JavaScript for variable declarations. It prevents issues like variable hoisting and accidental re-declarations.  
`var` is **function-scoped** and declarations are hoisted to the top of their scope, meaning you can use a var variable before its declaration in the code (though it will have a value of undefined). It is generally avoided in modern code due to its less predictable behavior.

**Prefer let or const over var**: let and const provide better scope management and are more explicit about variable mutability (whether the value can be changed).

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

