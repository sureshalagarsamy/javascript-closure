### Javascript closures

---

Javascript closures are a really powerful feature of the javascript language. Closures are created when a function that’s nested inside another function accesses a variable from its parent’s scope. This is really useful for passing state around your application when the inner function is called after the outer function has exited.

Even if you’ve never heard of closures before, you’ve probably done this without thinking about it too much when you set up an event handler.

```javascript
function myFunction(){
    var button = document.getElementById("myButton");
    button.addEventListener("click", function()
    {
        alert("I am a function nested inside another function");
    }, false);
}
```

Please note this

```
A closure is created when an inner function accesses variables from the outer function
```

closure is just a link from the inner function to the outer function from the time when the outer function exited.

A closure is created when an inner function accesses variables from the outer function.

```javascript
function outerFunction(){
    var button = document.getElementById("myButton");
    var importantPieceOfState = "View Cart";
    // declare the inner event
    button.addEventListener("click", function()
    {
        alert(importantPieceOfState);
    }, false);
}
```

When you run the button click handler code, the alert will say “View Cart” even though the importantPieceOfState variable is only declared in the outerFunction()’s scope.

> Notice that importantPieceOfState isn’t declared anywhere inside the click event handler and that it isn’t a global variable. It is only declared as a local variable to the outerFunction() function. The variable is available because when we run outerFunction() the javascript engine notices that the click handler has a reference to one of the variables in outerFunction(). It creates a closure to save the current state of outerFunction() for when the click event is run.

### setTimeout Inside a For Loop

Let’s say we would like to display numbers 1 to 5 at once sequentially. We can accomplish this easily with a simple for loop:

```javascript
for (var i = 1; i <= 5; i++) {
    console.log(i);
}
//Output : 1 2 3 4 5
```
But what if we want to display the same output one another one after 1 second

```javascript
for (var i = 1; i <= 5; i++) {
    setTimeout(function() { console.log(i); }, 1000*i);
}
//Output: 6 6 6 6 6
```

The above code is the answer for that. But it is giving the result as 6 for each time.

In this case, we’re simply passing the reference to the variable i, and not the actual value at the moment inside each loop. By the time the ``setTimeout()`` function is executed (after 1, 2, 3, 4, and 5 seconds in this case), the for statement has already been executed and incremented i to the final value of 6.

To properly address this issue we need to pass into ``setTimeout()`` the actual value of i at the moment of each loop execution in the for statement.

```javascript
for (var i = 1; i <= 5; i++) {
    setTimeout(function(x) { 
		return function() { 
			console.log(x); 
		}; 
	}(i), 1000*i);
}
//Output: 1 2 3 4 5
```
In the outer anonymous function ``setTimeout()`` we take in a parameter x and pass x into console.log().We execute the outer function immediately during each loop and pass the value of i into it. The outer function then returns the original anonymous function to be used for setTimeout() with the value of i captured in its private variable x.Now when we run this final code block, we’ll get the expected output.

#### More Closure example

```javascript
function greeting(message) {
   return function(name){
        return message + ' ' + name;
   }
}
let sayHi = greeting('Hi');
let sayHello = greeting('Hello');

console.log(sayHi('Suresh')); // Hi Suresh
console.log(sayHello('Ram')); // Hello Ram
```

To understand more about closures, you need to know how the ``lexical scoping`` works first.
### Lexical scoping
---


```javascript
let name = 'John';

function greeting() { 
    let message = 'Hi';
    console.log(message + ' '+ name);
}
```

The variable ``name`` is a global variable. It is accessible from anywhere including within the ``greeting()`` function.

If you try to access the ``message`` variable outside the ``greeting()`` function, you will get an error.

So the JavaScript engine uses the scope to manage the variable accessibility.

According to lexical scoping, the scopes can be nested and the inner function can access the variables declared in its outer scope. For example:

```javascript
function greeting() {
    let message = 'Hi';
    function sayHi() {
        console.log(message);
    }
    sayHi();
}
greeting();
```

The ``greeting()`` function creates a local variable named ``message`` and a function named ``sayHi()``.

The ``sayHi()`` is the inner function that is available only within the body of the ``greeting()`` function.

The ``sayHi()`` function can access the variables of the outer function such as the ``message`` variable of the ``greeting()`` function.

Inside the ``greeting()`` function, we call the ``sayHi()`` function to display the message ``Hi``.

#### Closure
So here we are going to use a closure. Let’s modify the ``greeting()`` function.

```javascript
function greeting() {
    let message = 'Hi';
    function sayHi() {
        console.log(message);
    }
    return sayHi;
}
let hi = greeting();
hi(); // still can access the message variable
```

### 1) Using the IIFE solution
immediately invoked function expression

```javascript
for (var index = 1; index <= 3; index++) {
    (function (index) {
        setTimeout(function () {
            console.log('after ' + index + ' second(s):' + index);
        }, index * 1000);
    })(index);
}
```
Output
```javascript
after 1 second(s):1
after 2 second(s):2
after 3 second(s):3
```

### 2) Using let keyword in ES6

In ``ES6``, you can use the ``let`` keyword to declare a variable that is block-scoped.

If you use the ``let`` keyword in the ``for-loop``, it will ``create a new lexical scope`` in each iteration. In other words, you will have a new ``index`` variable in each iteration.

```javascript
for (let index = 1; index <= 3; index++) {
    setTimeout(function () {
        console.log('after ' + index + ' second(s):' + index);
    }, index * 1000);
}
```
Output
```javascript
after 1 second(s):1
after 2 second(s):2
after 3 second(s):3
```
