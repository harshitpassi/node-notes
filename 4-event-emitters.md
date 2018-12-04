# Event Emitters

* Objects that emit events like TCP servers or file streams are called event emitters.
* Emitters allow programs to subscribe to events they are interested in.
* It is also possible to create custom event emitters through the `EventEmitter` pseudo-class.

## Callback Pattern
* Async programming uses continuation passing style (CPS).
* A function in CPS takes another argument which is a function called after it has finished executing.
```javascript
var fs = require('fs');
fs.readFile('etc/passwd', function(err, fileContent){
  if(err){
    throw err;
  }
  console.log(fileContent.toString());
});
```
## Event Emitter Pattern
* CPS does not work as well when several events take place or one event happens several times. Eg. being notified every time data is available on a socket.
* event emitter pattern separates the event emitter and event listener (binds to emitter and listens for events):
```javascript
var req = http.request(options, function(response){
  response.on('data', function(data){
    console.log(data);
  });
  response.end('end', function(){
    console.log('response ended');
  });
});
req.end();
```
* in example: http.request uses CPS. It finishes executing and passes a response object that is an event emitter. It can emit events like `data` and `end`. Then callbacks are registered on these events. There can be more than one listeners for an event emitter.
* CPS - use when you can to regain the flow after an event. Emitter patter - when even can happen multiple times.

## Event types
* Each event has a type string like `"data"`. Lowercase words with no spaces.
* No way to get the types of events an emitter emits. Needs to be documented along with callback argument signature to ensure all data gets passed.
* Emitter will invoke listener when event occurs.
* Event Emitter is a generic interface for all types of events. But there is one special case: error
* Most event emitter implementations will emit error events. If programmer chooses not to listen to error event and an error occurs, it will be an uncaught exception.
```javascript
var em = new (require('events').EventEmitter)();
em.emit('error', new Error('My mistake'));
Error: My mistake
    at repl:1:18
    at REPLServer.eval (repl.js:80:21)
    at repl.js:190:20
    at REPLServer.eval (repl.js:87:5)
    at Interface.<anonymous> (repl.js:182:12)
    at Interface.emit (events.js:67:17)
    at Interface._onLine (readline.js:162:10)
    at Interface._line (readline.js:426:8)
    at Interface._ttyWrite (readline.js:603:14)
    at ReadStream.<anonymous> (readline.js:82:12)
```
* program would have halted due to uncaught error. Always listen to and handle error events.

## Event Emitter API
* Event emitters implement these methods:
  * `.addListener` and `.on` - add event listeners on object
    * both methods are equivalent:
    ```javascript
    function receiveData(data) { console.log(data); }
    function receiveDataAgain(data) { console.log(data); }
    readStream.addListener('data', receiveData);
    readStream.on('data', receiveDataAgain);
    ```
    * Multiple listeners are allowed on same event type. Listeners are called in the order they are registered.
    * If one listener throws an error, subsequent events are not called.
  * `.once` - add event listener that will be called once only
    * adds event listener and removes it after first call.
  * `.removeEventListener` - Remove specific event listener
    * need to pass both event type and callback: `readStream.removeListener("data", receiveData);`
  * '.removeAllEventListeners' - Remove all listeners on given event type.
    * only need event type. Removes all listeners on that type `process.removeAllListeners("SIGTERM");`
    
## Creating Event Emitter
* Inherit from Node event emitter and then emit events:
```javascript
var util = require('util');
var EventEmitter = require('events').EventEmitter;

var MyClass = function(){}
MyClass.prototype.someMethod = function(){
  this.emit('custom', 'str', 'str2'); // emits custom event with two data objects
}

util.inherits(MyClass, EventEmitter); // sets up prototype chain to get EventEmitter methods on instances of MyClass
```
* To listen to events:
```javascript
var instance = new MyClass();
instance.on('custom', function(data1, data2){
  console.log(data1, data2);
})
```
* whenever `instance.someMethod()` is called, the above listener will execute.
