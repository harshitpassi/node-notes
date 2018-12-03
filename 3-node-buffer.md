# Node.js Core API Basics

## Buffers

* To handle binary data easily, Node includes a binary Buffer pseudo-class.
* Buffer length is specified in bytes and it is possible to get and set bytes randomly.
* Creating Buffers:
  * Can create from UTF-8 encoded string: `var buf = new Buffer('hello world');`
  * Also with other strings, specifiying encoding in second param: `var buf = new Buffer('8b76fde713ce', 'base64');` ascii, utf8 and base64 allowed.
  * Can also create buffers without initial content, with capacity for future content: `var buf = new Buffer(1024); // 1024 bytes in length`
* Getting and Setting bytes in Buffer:
  * Byte values can be accessed using the [] operator.
  ```javascript
  var buf = new Buffer('my buffer content');
  console.log(buf[9]); // -> 99
  // empty buffers will have junk values, not 0
  var buf2 = new Buffer(1024);
  console.log(buf2[99]); // -> 5 (some random number)
  ```
  * can manipulate by setting : `buf[99] = 125; //100th byte set to 125`
  * can also get length with `buf.length`
* Slicing a buffer:
  * Smaller buffers can be sliced from buffers by specifying start and end positions. `buffer.slice(8, 19);`
  * No new memory is allocated and nothing is copied. New buffer just references diff start and end positions.
  * Parent buffer changed, child buffer will also change.
  * JS objects are different, memory addressed is same.
  * Parent buffer should not be reclaimed by garbage collector whild child is active.
* Copying Buffer:
  * Can also copy part of one buffer into another buffer:
  ```javascript
  var buffer1 = new Buffer('this is the content of my buffer');
  var buffer2 = new Buffer(11);
  
  var targetStart = 0;
  var sourceStart = 8;
  var sourceEnd = 19;
  
  buffer1.copy(buffer2, targetStart, sourceStart, sourceEnd);
  
  console.log(buffer2.toString()); // -> "the content"
  ```
* Decoding Buffer:
  * can be converted to utf-8 and base64 strings:
  ```javascript
  var utf8String = 'my string';
  var buf = new Buffer(utf8String);
  var base64String = buf.toString('base64');
  ```
