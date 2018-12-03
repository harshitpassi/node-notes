# Node.js

## Traditional approach
* I/O similar to function calls. Processing can't continue till operation finishes
* Blocking I/O. One process per user. Managing multiple processes difficult for OS.
* Multi-threading introduced. Thread - lightweight process sharing memory with other threads in same process.
* allowed several concurrent threads of execution. When 1 thread is waiting for I/O, other can use CPU.
* programmers don't know which thread is running, need to use sync primitives like semaphores and flags..
* cooperative multithreading - programmers explicitly schedule threads - sync requirements relaxed. Still complex and error prone.

## Event Driven/Asynchronous Programming
* flow of execution determined by events handled by callbacks.
* Event callback - function invoked when event happens - result of db query or user input.
* Means current process will not block when waiting for I/O. Several I/O operations can occur in parallel, with respective callback executed on result.
* Event loop - two things repeatedly: event detection and event handler triggering. In every run, event loop detects all events that happened and executes callbacks.
* Event loop is one thread in one process, hence:
  * Only one event handler runs at any time
  * Every handler runs to completion without interruption
  
##Example
* Traditional Approach:
```
result = query('select * from table where id=1');
do_something(result);
```
* Event Driven Approach:
```
query_finished = function(result){
  do_something(result);
};
query('select * from table where id=1', query_finished);
```
    
## Asynchronous Programming with Node
* Node written in JavaScript due to closures and first-class functions making event driven programming easier.
* Closures - functions inheriting variables from enclosing environment. Simple terms:
  * Entire context of a function is remembered whenever it is executed, even if the context is no longer available.
  * In terms of event driven programming - when a callback is executed, all parent variables are available to it whenever it is executed:
  ```javascript
  var isButtonClicked = false; // Not a local var of callback
  document.getElementById('someElement').onclick = function(){
      isButtonClicked = true;
      console.log(isButtonClicked);
  }; 
  ```
* Functions - first class objects in js. Can be passed and returned to/from other functions. Callbacks have every variable in scope at time of declaration
* declaring var in global scope is bad practice as they can collide with other functionality. Better example:
  ```javascript
  (function(){
  var isButtonClicked = false; // Not a local var of callback
  document.getElementById('someElement').onclick = function(){
      isButtonClicked = true;
      console.log(isButtonClicked);
  };
  }();)
  ```
## Closures in relation to Async Programming
* functions in js not processed in isolation, but in the context where declared.
* Callback, when passed as argument will be invoked at a later time, but has access to all variables even if parent scope has returned.
* Helps to do event driven programming without having to pass state around. Closure automatically keeps state.
