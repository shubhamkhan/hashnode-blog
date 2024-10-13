---
title: "Explained the Secrets of JavaScript Hoisting: Avoid Common Pitfalls"
seoTitle: "Explained the Secrets of JavaScript Hoisting"
seoDescription: "If you've just started learning JavaScript, you may have encountered confusing issues like variables showing up as undefined or mysterious ReferenceError."
datePublished: Sun Oct 06 2024 13:20:56 GMT+0000 (Coordinated Universal Time)
cuid: cm1xm2rn1000209jq4qd09gjp
slug: javascript-hoisting
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1728835383190/8eef7e11-a0dd-4d30-8911-fdfb3d385bff.jpeg
tags: javascript, web-development, webdev, javascript-framework, frontend-development, hoisting, codenewbies, javascript-hoisting

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728220159313/6cdebf89-ac92-4379-a45b-95b2e98b91bb.gif align="center")

If you've just started learning JavaScript, you may have encountered confusing issues like variables showing up as `undefined` or mysterious `ReferenceError` messages. This might be due to a concept called "hoisting." But what is hoisting, and how does it affect how your code runs?

In this guide, we’ll explain JavaScript hoisting in simple terms, so you can avoid common issues and write cleaner code. By the end, you'll understand what hoisting is and how it works in JavaScript.

### What Exactly is Hoisting?

In simple terms, **hoisting** is JavaScript’s behaviour of moving variable and function declarations to the top of their scope before the code is executed. This means that variable and function declarations are processed **before** the actual code runs.

When your code is executed, the JavaScript engine first goes through a **creation phase**, during which it allocates memory for all variables and functions. However it treats functions and variables differently during this phase.

### Function Hoisting: Full Definitions Are Moved

When you declare a function using the `function` keyword, JavaScript stores the entire function in memory during the creation phase. As a result, you can call the function **before** its declaration in your code without any issues.

For example:

```javascript
sum(2, 3); // Output: 5

function sum(x, y) {
  console.log(x + y);
}
```

In this case, even though the `sum()` function is called before it’s defined, the code works perfectly because JavaScript has already hoisted the entire function definition to the top of the scope.

### Variable Hoisting: Different Rules for `var`, `let`, and `const`

Unlike functions, variables behave differently when hoisted. Let's explore the difference between `var`, `let`, and `const` declarations.

#### 1\. `var` Declarations

When you declare a variable using `var`, the variable is hoisted with a default value of `undefined`. This means that you can reference the variable before its declaration, but until the assignment happens, its value will be `undefined`.

```javascript
console.log(city); // Output: undefined
var city = "San Francisco";
console.log(city); // Output: "San Francisco"
```

In the code above, `city` is hoisted to the top of the scope with a value of `undefined`. That’s why the first `console.log()` returns `undefined`. After the variable is assigned a value, `city` is updated, and the second `console.log()` outputs "San Francisco."

#### 2\. `let` and `const` Declarations

With the introduction of ES6, JavaScript added `let` and `const` for variable declarations. Unlike `var`, these variables are hoisted but not initialized. This means you cannot access them before their declaration, or you'll get a `ReferenceError`.

```javascript
console.log(name); // ReferenceError: Cannot access 'name' before initialization
const name = "Lydia Hallie";
```

This happens because `let` and `const` are in a state known as the **Temporal Dead Zone (TDZ)**, which exists from the start of the block or function until the variable is declared. In this zone, accessing the variable results in a `ReferenceError`.

#### Key Difference Between `let` and `const`

Both `let` and `const` are hoisted, but the difference is that `const` requires an initial value at the time of declaration, while `let` does not. For example:

```javascript
const name = "Lydia Hallie"; // Must be initialized
let info; // Can be declared without initialization
```

### Hoisting in Action

Let’s combine both function and variable declarations in an example to see hoisting in action:

```javascript
console.log(city); // Output: undefined
sum(2, 3);    // Output: 5

function sum(x, y) {
  console.log(x + y);
}

var city = "San Francisco";
console.log(city); // Output: "San Francisco"
```

Here, the `sum` function is hoisted with its full definition, allowing it to be called even before the function is declared. However, the `city` variable is hoisted with a value of `undefined`, so the first `console.log()` outputs `undefined`. After the assignment, the second `console.log()` correctly outputs "San Francisco."

### Avoiding Common Hoisting Issues

Understanding hoisting can help you avoid common issues, especially when using `var`. Here are some best practices:

1. **Use** `let` or `const` instead of `var` to avoid accessing variables before their declaration.
    
2. **Always declare variables and functions at the top** of your scope to make the flow of your code predictable and easy to read.
    

### Key Takeaways

* **Hoisting** moves variable and function declarations to the top of their scope during the creation phase.
    
* **Functions** declared with `function` are hoisted with their entire definition, so you can call them before they are declared.
    
* **Variables declared with** `var` are hoisted with a default value of `undefined`, while variables declared with `let` and `const` are hoisted but remain uninitialized until their declaration.
    
* **Temporal Dead Zone (TDZ)** applies to `let` and `const`, which prevents you from using the variable before its declaration.
    

By understanding and applying these concepts, you can write more reliable and predictable JavaScript code. With practice, hoisting will become second nature!

Feel free to ask questions if anything is unclear—I'm happy to help! 😊