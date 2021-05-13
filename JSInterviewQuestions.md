# JavaScript Concepts & frequently asked Interview Questions

## Table of Contents  
[What is event loop in JavaScript?](#Q1)  
[Explain Closure with an example.](#Q2)  
[What is Hoisting in JavaScript?](#Q3)   
[bind()](#Q4)    
[call() and apply()](#Q5)    
[apply()](#Q6)    
[Do setTimeOut and setInterval always call the function associated with them?](#Q7)   
[null vs undefined](#Q8)    
[What are classes in JavaScript?](#Q9)   
[Explain debouncing in JavaScript.](#Q10)   
[Explain throttling in JavaScript.](#Q11)   
[Implement a Binary Search Tree class in JavaScript.](#Q12)   
[Implement a Stack class in JavaScript.](#Q13)    
[Implement a Queue class in JavaScript.](#Q14)   
[Implement a Linked List class in JavaScript.](#Q15)   
[How do we compare two objects in JavaScript.](#Q16)   
[Implement a function that returns the intersection of two arrays.](#Q17)   
[Explain Prototype in JavaScript with an example.](#Q18)   
[Explain Inheritance, Classical Inheritance, & Prototypal Inhertiance in JavaScript with an example.](#Q19)   
[What are classes in JavaScript. Explain with an example.](#Q20)



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

Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution. Remember that JavaScript only hoists declarations, not initialisation.

```
console.log(message); //output : undefined
var message = 'The variable Has been hoisted';
```
The above code looks like as below to the interpreter,
```
var message;
console.log(message);
message = 'The variable Has been hoisted';
```

<br>
<br>

<a name="Q4"/>

### bind()

The ```bind()``` method returns a new function, when invoked, has its ```this``` set to a specific value.

The following illustrates the syntax of the ```bind()``` method:
```fn.bind(thisArg[, arg1[, arg2[, ...]]])```

In this syntax, the ```bind()``` method returns a copy of the function ```fn``` with the specific ```this``` value (```thisArg```) and arguments (```arg1```, ```arg2```, …).

> Unlike the call() and apply() methods, the bind() method doesn’t immediately execute the function. It just returns a new version of the function whose this sets to thisArg argument.

#### Using ```bind()``` for function binding
When you pass a method an object is to another function as a callback, the this is lost. For example:

```
let person = {
    name: 'John Doe',
    getName: function() {
        console.log(this.name);
    }
};

setTimeout(person.getName, 1000);
```

Output:
```
undefined
```
As you can see clearly from the output, the ```person.getName()``` returns ```undefined``` instead of 'John Doe'. This is because ```setTimeout()``` received the function ```person.getName``` separately from the ```person``` object.

The statement:
```
setTimeout(person.getName, 1000);
```
can be rewritten as:
```
let f = person.getName;
setTimeout(f, 1000); // lost person context
```
The this inside the ```setTimeout()``` function is set to the ```global``` object in non-strict mode and ```undefined``` in the strict mode. Therefore, when the callback ```person.getName``` is invoked, the ```name``` does not exist in the ```global``` object, it is set to ```undefined```.

To fix this issue, use bind() to set the context:
```
let f = person.getName.bind(person);
setTimeout(f, 1000);
```
In this code:

First, bind the ```person.getName``` method to the ```person``` object.
Second, pass the bound function ```f``` with this value set to the person object to the ```setTimeout()``` function.

#### Using ```bind()``` to borrow methods from a different object
Suppose you have a runner object that has the run() method:
```
let runner = {
    name: 'Runner',
    run: function(speed) {
        console.log(this.name + ' runs at ' + speed + ' mph.');
    }
};
```

And the flyer object that has the fly() method:
```
let flyer = {
    name: 'Flyer',
    fly: function(speed) {
        console.log(this.name + ' flies at ' + speed + ' mph.');
    }
};
```

If you want the flyer object to be able to run, you can use the bind() method to create the run() function with the this  sets to the flyer object:
```
let run = runner.run.bind(flyer, 20);
run();
```
In this statement:

Call the bind() method of the runner.run() method and pass in the flyer object as the first argument and 20 as the second argument.
Invoke the run() function.

Output:
```
Flyer runs at 20 mph.
```
The ability to borrow a method of an object without making a copy of that method and maintain it in two separate places is very powerful in JavaScript.

#### Summary

-> The bind() method creates a new function, when invoked, has the this sets to a provided value.

-> The bind() method allows an object to borrow a method from another object without making a copy of that method. This is known as function borrowing in JavaScript.


<br>
<br>

<a name="Q5"/>

### call() and apply()

The call() method calls a function with a given this value and arguments provided individually.   
The apply() method calls a function with a given this value and arguments provided in form of an array.

```
var pokemon = {
    firstname: 'Pika',
    lastname: 'Chu ',
    getPokeName: function() {
        var fullname = this.firstname + ' ' + this.lastname;
        return fullname;
    }
};

var pokemonName = function(snack, hobby) {
    console.log(this.getPokeName() + ' loves ' + snack + ' and ' + hobby);
};

pokemonName.call(pokemon,'sushi', 'algorithms'); // Pika Chu  loves sushi and algorithms
pokemonName.apply(pokemon,['sushi', 'algorithms']); // Pika Chu  loves sushi and algorithms
```

The main differences between ```bind()``` and ```call()```/```apply()``` is that the ```call()```/```apply()``` methods:
  1. Accept additional parameters as well
  2. Execute the function it was called upon right away.
  3. Do not make a copy of the function it is being called on.

<br>
<br>

<a name="Q6"/>

### apply()

When calling `setTimeout` or `setInterval`, a timer thread in the browser starts counting down and when time up puts the callback function in JavaScript thread's execution stack. The callback function is not executed before other functions above it in the stack finishes. So if there are other time-consuming functions being executed when time up, the callback of `setTimeout` will not finish in time.

<br>
<br>

<a name="Q7"/>

### Do setTimeOut and setInterval always call the function associated with them?

When calling `setTimeout` or `setInterval`, a timer thread in the browser starts counting down and when time up puts the callback function in JavaScript thread's execution stack. The callback function is not executed before other functions above it in the stack finishes. So if there are other time-consuming functions being executed when time up, the callback of `setTimeout` will not finish in time.

<br>
<br>

<a name="Q8"/>

### null vs undefined

`null` is an assigned value. It means nothing. `undefined` typically means a variable has been declared but not defined yet. A variable initialized with ```undefined``` means that the variable has no value or object assigned to it while ```null``` means that the variable has been set to an object whose value is undefined.

<br>
<br>

<a name="Q9"/>

### What are classes in JavaScript?

Javascript classes are primarily syntactic sugar over JavaScript’s existing prototype-based inheritance. For example, the prototype based inheritance written in function expression as below:
```
function Bike(model,color) {
    this.model = model;
    this.color = color;
}

Bike.prototype.getDetails = function() {
    return this.model + ' bike has' + this.color + ' color';
};
```
Whereas ES6 classes can be defined as an alternative
```
class Bike{
  constructor(color, model) {
    this.color= color;
    this.model= model;
  }

  getDetails() {
    return this.model + ' bike has' + this.color + ' color';
  }
}
```

<br>
<br>

<a name="Q10"/>

### Explain debouncing in JavaScript.

The ```debounce()``` function forces a function to wait a certain amount of time before running again. The function is built to limit the number of times a function is called. Events such as scrolling, mouse movement, and keypress bring great overhead with them if they are captured every single time they fire. The function aims to reduce overhead by preventing a function from being called several times in succession.

The following implementation of ```debounce``` returns a function that, as long as it continues to be invoked, will not be triggered. The function will be called after it stops being called for d milliseconds.

```
<input type="text" onkeyup="returnedFunction()"/>
```

```
// Debouncing in Javascript
let counter = 0;
const getData = () => {
  // calls an API and gets Data
  console.log("Fetching Data ..", counter++);
}

const debounce = function (fn, d) {
  let timer;
  return function () {
    let context = this, args = arguments;
    clearTimeout(timer);
    timer = setTimeout(() => {
      getData.apply(context, arguments);
    }, d);
  }
}

const returnedFunction = debounce(getData, 300);
```

<br>
<br>

<a name="Q11"/>

### Explain throttling in JavaScript.

Throttling is a technique in which, no matter how many times the user fires the event, the attached function will be executed only once in a given time interval.


```
const loggerFunc = () => {
  console.count("Throttled Function");
}

const throttle = (fn, limit) => {
  let flag = true;
  return function(){
    let context = this, args = arguments;
    if(flag){
      fn.apply(context, args);
      flag = false;
      setTimeout(() => {
        flag=true;
      }, limit);
    }
  }
}

const returnedFunction = throttle(loggerFunc, 1000);

window.addEventListener("resize",returnedFunction);
```

<br>
<br>

<a name="Q12"/>

### Implement a Binary Search Tree class in JavaScript.

```
// Node class
class Node
{
	constructor(data)
	{
		this.data = data;
		this.left = null;
		this.right = null;
	}
}
```

```
// Binary Search tree class
class BinarySearchTree
{
    constructor()
    {
        // root of a binary seach tree
        this.root = null;
    }
    
    // helper method which creates a new node to be inserted and calls insertNode
    insert(data)
    {
      // Creating a node and initailising with data
      var newNode = new Node(data);

      // root is null then node will be added to the tree and made root.
      if(this.root === null)
        this.root = newNode;
      else
        // find the correct position in the tree and add the node
        this.insertNode(this.root, newNode);
    }

    // Method to insert a node in a tree it moves over the tree to find the location to insert a node with a given data
    insertNode(node, newNode)
    {
      // if the data is less than the node data move left of the tree
      if(newNode.data < node.data)
      {
        // if left is null insert node here
        if(node.left === null)
          node.left = newNode;
        else
          // if left is not null recur until null is found
          this.insertNode(node.left, newNode);
      }

      // if the data is more than the node data move right of the tree
      else
      {
        // if right is null insert node here
        if(node.right === null)
          node.right = newNode;
        else
          // if right is not null recur until null is found
          this.insertNode(node.right,newNode);
      }
    }
    
    // search for a node with given data
    search(node, data)
    {
      // if trees is empty return null
      if(node === null)
        return null;
      // if data is less than node's data move left
      else if(data < node.data)
        return this.search(node.left, data);
      // if data is less than node's data move left
      else if(data > node.data)
        return this.search(node.right, data);
      // if data is equal to the node data return node
      else
        return node;
    }


  
    // function to be implemented
    // remove(data)
                  
    // Helper function
    // findMinNode()
    // getRootNode()
    // inorder(node)
    // preorder(node)               
    // postorder(node)
}
```

Example to create a BinarySearchTree class and test the functions:

```
// create an object for the BinarySearchTree
var BST = new BinarySearchTree();
  
// Inserting nodes to the BinarySearchTree
BST.insert(15);
BST.insert(25);
BST.insert(10);
BST.insert(7);
BST.insert(22);
BST.insert(17);
BST.insert(13);
BST.insert(5);
BST.insert(9);
BST.insert(27);
                          
//          15
//         /  \
//        10   25
//       / \   / \
//      7  13 22  27
//     / \    /
//    5   9  17 
```

<br>
<br>

<a name="Q13"/>

### Implement a Stack class in JavaScript.

```
// Stack class
class Stack {

	// Array is used to implement stack
	constructor()
	{
		this.items = [];
	}
  
  push(element)
  {
    // push element into the items
    this.items.push(element);
  }
  
  pop()
  {
    // return top most element in the stack and removes it from the stack Underflow if stack is empty
    if (this.items.length == 0)
      return "Underflow";
    return this.items.pop();
  }
  
  peek()
  {
    // return the top most element from the stack but does'nt delete it.
    return this.items[this.items.length - 1];
  }
  
  isEmpty()
  {
    // return true if stack is empty
    return this.items.length == 0;
  }
  
  printStack()
  {
    var str = "";
    for (var i = 0; i < this.items.length; i++)
      str += this.items[i] + " ";
    return str;
  }
}
```

Example to create a Stack class and test the functions:

```
// creating object for stack class
var stack = new Stack();

// testing isEmpty and pop on an empty stack

// returns false
console.log(stack.isEmpty());

// returns Underflow
console.log(stack.pop());

// Adding element to the stack
stack.push(10);
stack.push(20);
stack.push(30);

// Printing the stack element
// prints [10, 20, 30]
console.log(stack.printStack());

// returns 30
console.log(stack.peek());

// returns 30 and remove it from stack
console.log(stack.pop());

// returns [10, 20]
console.log(stack.printStack());
```

<br>
<br>

<a name="Q14"/>

### Implement a Queue class in JavaScript.

```
// Queue class
class Queue
{
	// Array is used to implement a Queue
	constructor()
	{
		this.items = [];
	}
	
  enqueue(element)
  {	
    // adding element to the queue
    this.items.push(element);
  }
  
  dequeue()
  {
      // removing element from the queue; returns underflow when called  on empty queue
      if(this.isEmpty())
          return "Underflow";
      return this.items.shift();
  }
  
  front()
  {
      // returns the Front element of the queue without removing it.
      if(this.isEmpty())
          return "No elements in Queue";
      return this.items[0];
  }
  
  isEmpty()
  {
      // return true if the queue is empty.
      return this.items.length == 0;
  }
  
  printQueue()
  {
      var str = "";
      for(var i = 0; i < this.items.length; i++)
          str += this.items[i] +" ";
      return str;
  }
}
```

Example to create a Queue class and test the functions:

```
// creating object for queue class
var queue = new Queue();
			

// Testing dequeue and pop on an empty queue returns Underflow
console.log(queue.dequeue());

// returns true
console.log(queue.isEmpty());

// Adding elements to the queue => queue contains [10, 20, 30, 40, 50]
queue.enqueue(10);
queue.enqueue(20);
queue.enqueue(30);
queue.enqueue(40);
queue.enqueue(50);
queue.enqueue(60);

// returns 10
console.log(queue.front());

// removes 10 from the queue => queue contains [20, 30, 40, 50, 60]
console.log(queue.dequeue());

// returns 20
console.log(queue.front());

// removes 20 => queue contains [30, 40, 50, 60]
console.log(queue.dequeue());

// prints [30, 40, 50, 60]
console.log(queue.printQueue());
```

<br>
<br>

<a name="Q15"/>

### Implement a LinkedList class in JavaScript.

```
// User defined class node
class Node {
	// constructor
	constructor(element)
	{
		this.element = element;
		this.next = null
	}
}
```

```
class LinkedList {
	constructor()
	{
		this.head = null;
		this.size = 0;
	}
  
  // adds an element at the end of list
  add(element)
  {
    // creates a new node
    var node = new Node(element);

    // to store current node
    var current;

    // if list is Empty add the element and make it head
    if (this.head == null)
      this.head = node;
    else {
      current = this.head;

      // iterate to the end of the list
      while (current.next) {
        current = current.next;
      }

      // add node
      current.next = node;
    }
    this.size++;
  }
  
  // finds the index of element
  indexOf(element)
  {
    var count = 0;
    var current = this.head;

    // iterae over the list
    while (current != null) {
      // compare each element of the list with given element
      if (current.element === element)
        return count;
      count++;
      current = current.next;
    }

    // not found
    return -1;
  }
  
  // prints the list items
  printList()
  {
    var curr = this.head;
    var str = "";
    while (curr) {
      str += curr.element + " ";
      curr = curr.next;
    }
    console.log(str);
  }
  
  // gives the size of the list
  size_of_list()
  {
    console.log(this.size);
  }
  
  // checks the list for empty
  isEmpty()
  {
    return this.size == 0;
  }

	// functions to be implemented
	// insertAt(element, location)
	// removeFrom(location)
	// removeElement(element)
}
```

Example to create a LinkedList class and test the functions:

```
// creating an object for the Linkedlist class
var ll = new LinkedList();
 
// testing isEmpty on an empty list returns true
console.log(ll.isEmpty());
 
// adding element to the list
ll.add(10);
 
// prints 10
ll.printList();
 
// returns 1
console.log(ll.size_of_list());
 
// adding more elements to the list
ll.add(20);
ll.add(30);
ll.add(40);
ll.add(50);
 
// returns 10 20 30 40 50
ll.printList();
 
// prints 50 from the list
console.log("is element removed ?" + ll.removeElement(50));
 
// prints 10 20 30 40
ll.printList();
 
// returns 3
console.log("Index of 40 " + ll.indexOf(40));
```

<br>
<br>

<a name="Q16"/>

### How do we compare two objects in JavaScript.

Works when the order of the objects are the same.
```
 x = {a: 1, b: 2};
 y = {a: 1, b: 2};
 
 JSON.stringify(obj1) === JSON.stringify(obj2) 
 ```
 
 But when the order isn't the same.
 ```
var compareObjects = function(o1, o2){
  for(var p in o1){
      if(o1.hasOwnProperty(p)){
          if(o1[p] !== o2[p]){
              return false;
          }
      }
  }
  for(var p in o2){
      if(o2.hasOwnProperty(p)){
          if(o1[p] !== o2[p]){
              return false;
          }
      }
  }
  return true;
};
```

<br>
<br>

<a name="Q17"/>

### Implement a function to return the intersection of two arrays.

Example 1:
  Input: nums1 = [1,2,2,1], nums2 = [2,2]
  Output: [2]

Example 2:
  Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
  Output: [9,4]
  Explanation: [4,9] is also accepted.

```
var intersection = function(nums1, nums2) {
    nums1 = [...new Set(nums1)];
    nums2 = [...new Set(nums2)];
    nums1.sort((a, b) => a - b);
    nums2.sort((a, b) => a - b);
    return nums1.filter(value => nums2.includes(value));
};
```

<br>
<br>

<a name="Q18"/>

### Explain Prototype in JavaScript with an example.

Prototypes are the mechanism by which JavaScript objects inherit features from one another.

A prototype object is the shared space of a class instances. When any property of method of class is being accessed, JS interpreter first lookup that property in that object if it is not available, then it lookup into its prototype, if it is not there, then lookup into its parent’s prototype and so on.

Every object has ```__proto__``` property pronounced as “dunder proto” which holds reference of prototype object.

```
function Person(fName, lName) {
  this.firstName = fName;
  this.lastName = lName;
}
```
Using the prototype object:

```
Person.prototype.getFullName = function () {
  return this.firstName + ' ' + this.lastName;
}
```
Create an instance of Person:

```
var p1 = new Person('Rahul','Akarapu');
```
In the above example, method getFullName is created on prototype of Person. When we try to access p1.getFullName(), JS engine first try to access that property on that object, as it is not available on p1, it goes to __proto__ and try to access that property, once JS engine found getFullName, it execute it in that context.

```
p1.hasOwnProperty('firstName');  // Returns true
p1.hasOwnProperty('lastName');   // Returns true
p1.hasOwnProperty('getFullName') // Returns false
```
In the above example, p1 has its own properties firstName and lastName as they are exist on that object, but it doesn’t has method getFullName as it is not exist on that object but exist on its prototype.

<br>
<br>

<a name="Q19"/>

### Explain Inheritance, Classical Inheritance, & Prototypal Inhertiance in JavaScript with an example.

#### Inheritance basics
Inheritance is inheriting features of one class by another class. Inheritance supports reusability, when we want to create new class, when there is another class with some features, we create another class which derives from that class and add extra features over it.

#### Inheritance in JavaScript
JavaScript supports both, classical inheritance and prototypal inheritance. It follows same procedure as other Object oriented supported languages follows. JavaScript only supports single and multi-level inheritance.

Lets try with following example.

#### Problem statement:
Person is the base class which has two properties, firstName and lastName of the person. It also has a method getFullName which returns fitstName and lastName of the person.
Employee is the class which is inherited from Person class and has property empId and a method getEmpInfo which returns an array having values [empId, firstName, lastName].

Now, lets implement this scenario in JavaScript.

Create Person as base class:
```
function Person(fName, lName) {
  this.firstName = fName;
  this.lastName = lName;
}
```

Create methods on prototype of the Person class:
```
Person.prototype.getFullName = function () {
  return this.firstName + ' ' + this.lastName;
}
```

Create a child Employee class and call its base class with required parameters:
```
function Employee(fName, lName, eId) {
  Person.call(this, fName, lName);
  this.empId = eId;
}
```

In the above example, we are executing constructor of Person class from the constructor function of Employee class in Employee class’s scope. It is similar to calling super.

Inherit prototype object. Create prototype object for Employee class from Person class:
```
Employee.prototype = Object.create(Person.prototype);
```
Here we are creating new object from Person.prototype and assigning it to prototype object of Employee class.

We recreated prototype object for Employee class, but here we missed something. What?? We copied prototype from Person class and hence we lost constructor function of Employee class. Reassign constructor function.
```
Employee.prototype.constructor = Employee;
```

Now we are ready to add methods over the prototype of Employee class. Add methods on prototype of child class:
```
Employee.prototype.getEmpInfo = function () {
  return [this.empId, this.firstName, this.lastName];
}
```

Now we can create instance of Employee class and can access properties and methods of Person class too from that instance.
```
var e1 = new Employee('Rahul', 'Akarapu', 123);
e1.getFullName();
e1.getEmpInfo();
```

<br>
<br>

<a name="Q20"/>

### What are classes in JavaScript. Explain with an example.

Classes are a template for creating objects. They encapsulate data with code to work on that data. Classes in JS are built on prototypes but also have some syntax and semantics that are not shared with ES5 class-like semantics.

```
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```

An important difference between function declarations and class declarations is that function declarations are hoisted and class declarations are not. You first need to declare your class and then access it.
