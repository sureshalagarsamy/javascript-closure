### Javascript closures

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
