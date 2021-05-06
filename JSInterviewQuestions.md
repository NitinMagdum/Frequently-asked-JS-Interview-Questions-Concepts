# JavaScript Interview Questions/Concepts

### What is event loop in JavaScript?
The ** __event loop__ ** is the secret behind JavaScript’s asynchronous programming. JS executes all operations on a single thread, but using a few smart data structures, it gives us the illusion of multi-threading. Let’s take a look at what happens on the back-end.

The call stack is responsible for keeping track of all the operations in line to be executed. Whenever a function is finished, it is popped from the stack.

### Do setTimeOut and setInterval always call the function associated with them?
When calling setTimeout or setInterval, a timer thread in the browser starts counting down and when time up puts the callback function in JavaScript thread's execution stack. The callback function is not executed before other functions above it in the stack finishes. So if there are other time-consuming functions being executed when time up, the callback of setTimeout will not finish in time.
