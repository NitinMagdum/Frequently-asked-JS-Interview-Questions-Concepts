# JavaScript Concepts & frequently asked Interview Questions

## Table of Contents  
[What is event loop in JavaScript?](#Q1)  
[Explain Closure with an example.](#Q2)  
[What is Hoisting in JavaScript?](#Q3)   
[Do setTimeOut and setInterval always call the function associated with them?](#Q4)   
[null vs undefined](#Q5)

<a name="Q1"/>

### What is event loop in JavaScript?
The **_event loop_** is the secret behind JavaScript’s asynchronous programming. JS executes all operations on a single thread, but using a few smart data structures, it gives us the illusion of multi-threading. Let’s take a look at what happens on the back-end.

The **_call stack_** is responsible for keeping track of all the operations in line to be executed. Whenever a function is finished, it is popped from the stack.

<br>
<p align="center">
  <img src="CallStack.png" alt="Call Stack" height="300" width="200">
</p>

The **_event queue_** is responsible for sending new functions to the track for processing. It follows the queue data structure to maintain the correct sequence in which all operations should be sent for execution.

<br>
<p align="center">
  <img src="EventQueue.png" height="200" width="400" alt="Event Queue"/>
</p>

Whenever an async function is called, it is sent to a browser API. These are APIs built into the browser. Based on the command received from the call stack, the API starts its own single-threaded operation.

An example of this is the `setTimeout` method. When a `setTimeout` operation is processed in the stack, it is sent to the corresponding API which waits till the specified time to send this operation back in for processing.

Where does it send the operation? The event queue. Hence, we have a cyclic system for running async operations in JavaScript. The language itself is single-threaded, but the browser APIs act as separate threads.

The **_event loop_** facilitates this process; it constantly checks whether or not the call stack is empty. If it is empty, new functions are added from the event queue. If it is not, then the current function call is processed.

<br>
<p align="center">
  <img src="EventLoopVisualized.png" height="400" alt="EL Visualized"/>
</p>

<br>
<br>

<a name="Q2"/>

### Closure with an example.
Suppose, you want to count the number of times user clicked a button on a webpage.
For this, you are triggering a function on onclick event of button to update the count of the variable.

`<button onclick="updateClickCount()">click me</button>`

Now there could be many approaches like:

1. You could use a global variable, and a function to increase the counter:
```
 var counter = 0;

 function updateClickCount() {
     ++counter;
     // Do something with counter
 }
```
  But, the probblem with this approach is that any script on the page can change the counter, without calling updateClickCount().

2. Now, you might be thinking of declaring the variable inside the function:
```
 function updateClickCount() {
     var counter = 0;
     ++counter;
     // Do something with counter
 }
 ```
  But, hey! Every time updateClickCount() function is called, the counter is set to 1 again.
  
3. Thinking about nested functions?

  Nested functions have access to the scope "above" them.

  In this example, the inner function updateClickCount() has access to the counter variable in the parent function countWrapper():
  ```
   function countWrapper() {
       var counter = 0;
       function updateClickCount() {
           ++counter;
           // Do something with counter
       }
       updateClickCount();
       return counter;
   }
   ```
  This could have solved the counter dilemma, if you could reach the updateClickCount() function from the outside and you also need to find a way to execute counter = 0 only once not everytime.
  
  4. Closure to the rescue! (self-invoking function):
   ```
   var updateClickCount = (function(){
       var counter = 0;

       return function(){
           ++counter;
           // Do something with counter
       }
   })();
   ```
  The self-invoking function only runs once. It sets the counter to zero (0), and returns a function expression. This way updateClickCount becomes a function. The "wonderful" part is that it can access the counter in the parent scope. This is called a JavaScript closure. It makes it possible for a function to have "private" variables. The counter is protected by the scope of the anonymous function, and can only be changed using the add function!
  
#### A more lively example on closures
```
<script>
var updateClickCount = (function(){
    var counter = 0;

    return function(){
        ++counter;
        document.getElementById("spnCount").innerHTML = counter;
    }
})();
</script>

<html>
<button onclick="updateClickCount()">click me</button>
<div> you've clicked
    <span id="spnCount"> 0 </span> times!
</div>
</html>
```

<br>
<br>

<a name="Q3"/>

### What is hoisting in JavaScript?

Hoisting in JS is a phenomenon of accessing variables even before they are initialized. Before JS starts executing a piece of code, it skims through the code and allocates memory for all the variables and functions. So, if somewhere in the code, a variable x is defined and initialized; JS allocates the memory and assigns it a keyword undefined before even it executes that code. Hence, when you try to access it before it is initialized, it'll be undefined.

<br>
<br>

<a name="Q4"/>

### Do setTimeOut and setInterval always call the function associated with them?

When calling `setTimeout` or `setInterval`, a timer thread in the browser starts counting down and when time up puts the callback function in JavaScript thread's execution stack. The callback function is not executed before other functions above it in the stack finishes. So if there are other time-consuming functions being executed when time up, the callback of `setTimeout` will not finish in time.

<br>
<br>

<a name="Q5"/>

### null vs undefined

`null` is an assigned value. It means nothing. `undefined` typically means a variable has been declared but not defined yet.
