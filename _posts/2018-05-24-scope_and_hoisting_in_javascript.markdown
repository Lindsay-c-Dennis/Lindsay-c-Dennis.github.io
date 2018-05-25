---
layout: post
title:      "Scope and Hoisting in Javascript"
date:       2018-05-24 16:33:34 -0400
permalink:  scope_and_hoisting_in_javascript
---




The need for variables in any programming language is self-evident. To be able to successfully manipulate data, values need to be able to be held in discrete little packages. In Javascript, creating a new variable is a two step process of first declaring the variable and then assigning it a value, though often these two steps happen on the same line. 

```
var five; 
five = 5;

var nine = 9;
```

In the context of a complex application, a programmer may need to declare hundreds of variables. Therefore, she must be very thoughtful in how these variables are created and used. If all variables in an application were available from any point within that application, they would all need to have distinct names to avoid conflicts. While it would be theoretically possible to accomplish this, it would make the task of programming quite arduous, as variable names would almost certainly be inadvertently reused as some point. This is where the concept of ‘scope’ comes in.

All variables exist within a particular context, which we call their ‘scope’. They have access to other objects within that same scope, and, through the ‘scope chain’, objects that are in a broader scope. The scope chain only works in one direction - local to global. Think of posting to your facebook wall. Setting your post to ‘public’ is like declaring a variable in the global scope. That post is available to anyone on facebook. If you wanted to be more selective, you could mark your post as ‘friends only’, so only people on your friends list could read it. If you’re feeling especially shy, you could mark your post ‘only me’, meaning you are the only person with access. People who don’t know you can’t see your ‘friends only’ posts, but your friends can see both the public and the friends only posts. You are able to see all types of post, because you are in the most ‘local’ scope position. 

Why should we care about keeping variables more private? Software developers like to follow the principle of least privilege, which dictates that all details not necessary for the current operation should be hidden. This helps avoid ‘collision’, or situations where the same variable name is being used for multiple distinct operations.

So, how are the boundaries of scope defined? Let’s look at an example, starting with var, the pre-E6 keyword for declaring variables. 
```
var catSpeak = “Meow!”

function cat(name) {
var kittenFact = “is just the cutest!”
console.log(`${name} ${kittenFact} ${catSpeak}`)
}

cat(“Mittens”) // “Mittens is just the cutest! Meow!”

console.log (catSpeak) // “Meow!”

console.log(kittenFact) // Uncaught ReferenceError: kittenFact is not defined
````
This code sample has two variables - catSpeak and kittenFact. catSpeak is a global variable, which is why it can be accessed inside the cat function and also in the global console.log statement. kittenFact, on the other hand, was declared in the scope of the cat function, and therefore can only be accessed from within the function. This is because variables declared with the keyword var are function-scoped. 

Variables declared with var also have a somewhat curious behavior known as hoisting. Hoisting occurs because javascript is interpreted in several phases. In the first phrase, the variables are declared, and space is created for them in the memory of the computer, but their value is not yet assigned, and they are given the placeholder value of ‘undefined’. Because of this, the variable declaration is interpreted at the top of the scope, while the variable assignment occurs at the place the code was originally written. For example:
```
console.log(x);  // undefined
var x = “Yup.”
````
Because the variable is used on a line before it’s definition, the declaration is hoisted to the top of the scope, but the value remains below, making it unavailable in the console.log statement. The above code is functionally identical to this: 
```
var x; 
console.log(x)
x = “Yup.”
```
This hoisting behavior can sometimes produce strange bugs. Best practices dictate that variable declarations and assignements should occur at the top of the scope.

In the last few years, the javascript language has undergone some significant changes, particularly with regard to how variables are declared. To address some issues with the functionality of the var keyword, two new variable declaration keywords were introduced: let and const.

Let and Const are both block-scoped instead of function-scoped. This means that their scope is smaller than that of a var variable, consisting of the space inside of a code block { } instead of a function. For example: 
```
let x = 5;

if (x > 3) {
	let y = 7;
} 

console.log(y) // Uncaught ReferenceError: y is not defined
```
If y had been declared with the var keyword, it would have been globally scoped, because it is not inside of a function, and the console.log statement would print the value 7. Instead, because let is block-scoped, the value of the variable is only accessible within the block in which it is declared (inside of the if condition). Variables declared with let are NOT hoisted. If hoisting occurred with let variables, the console.log statement would have printed undefined. Instead, it throws an error. Additionally, while the var keywords allow the same variable to be declared more than once, let will throw an error if a variable of that name has already been declared.

```
var chimney = “sooty”;
var chimney = “clean”; // valid

let mood = “happy”;
let mood = “sad”; // Uncaught SyntaxError: Identifier 'mood' has already been declared
```
Why the differences? The new keywords were created to solve some issues with var. Because they do not hoist, you do not run the risk of having a variable unexpectedly return ‘undefined’. Instead, if the variable declaration happens after it is used, the program will throw an error, hopefully allowing the programmer to catch the bug at an earlier stage. The space between the start of a block and the declaration of a variable is called the temporal dead zone, and should ideally be kept as small as possible to avoid errors. It is good practice to declare variables as close to the top of the scope as is practicable. 

By changing the scoping of variables from the function to the block, these new keywords more closely adhere to the principle of least privilege. Similarly, because the let and const keywords  cannot be declared more than once with the same identifier, they reduce the risk of variable collision.

Const has several features that make its variables distinct from those declared with var or let. First, const cannot be declared without assignment. While var and let variables can be declared on one line and have values assigned on a different line, const will throw an error if it is not immediately assigned a value. Const also cannot be overwritten.

So with these three different choices, which is appropriate in which circumstance? Ideally, const is the best choice. In most cases, variables will not need to be reassigned, and const will work nicely, keeping a tidy block scope and guarding against accidental overwriting. If the variable needs to be mutable, such as a counter, let is generally the best choice. Var should generally be avoided in modern usage. It can occasionally be useful in situations where function scope is preferable to block scope, such as a variable whose value is assigned based on the operation of a conditional statement. However, let can usually be declared at the top of the function, with the value being assigned in the block (without the keyword declaration). Additionally, the ES6 keywords may be potentially problematic with certain older browsers. This problem can be alleviated using a compiler such as Babel for backwards compatibility.

One last note on variable declaration: no matter which keyword you choose, make sure you use something when you declare your variable! Any variable declared without a keyword (ie, x=9) will be considered a global variable, regardless of the scope in which it appears. Keywords are important!

-----
Hoisting is also relevant to functions. Named functions are declared with the function keyword:
```
sayHello(“Jim”); // “Hello, Jim”

function sayHello(name) {
console.log(“Hello, “ + name);
} 


```
Functions declared with the function keyword are hoisted, meaning that they can be invoked before they are defined. Functions can also be declared as function expressions, which can be assigned to a variable or used as a callback in the parameters of a higher order function. Function expressions are not hoisted. They must be declared above their invocations. Function expressions are often anonymous, which can be more convenient when writing code, but makes for a more complicated debugging process, as they will be listed as ‘anonymous’ in the stack trace.

