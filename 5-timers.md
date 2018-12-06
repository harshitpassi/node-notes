# Scheduled Execution of Functions

* `setTimeout()` and `setInterval()` implemented in Node similar to browser.
* `setTimeout(func, time)` : function to be executed, first arg. Time to wait in ms, second arg. This call returns timeout handler, an internal object that is only used for canceling execution using `clearTimeout`.
```javascript
var timeoutTime = 1000; // one second
var timeout = setTimeout(function() {
  console.log("timed out!");
}, timeoutTime);
clearTimeout(timeout);
```
* `setInterval()` and `clearInterval` exactly the same.

## Process.nextTick()

*
