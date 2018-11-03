# JavaScript Note

## Contents 
1. [Basic](#basic)
2. [Array](#array)
3. [Object](#object)
4. [Function](#function)
5. [Async JavaScript](#async-javascript)



# Basic
> Placing scripts at the bottom of the <font color="red">__&lt;body>__</font> element improves the display speed, because script compilation slows down the display.


But basicly, I won't put the javascript code at html file, so no big deal ~
## Primitive JavaScript Data Types
![enter image description here](https://i.imgur.com/VWaOZyd.png)

## Truthy and Falsy Values

Falsy values :
-  undefined
- null
- 0
- ' '
- NaN

Falsy values : NOT Falsy values
	
# Array
## Array.forEach()
The forEach() method calls a function (a callback function) once for each array element.

```javascript
var txt = "";  
var numbers = [45, 4, 9, 16, 25];  
numbers.forEach(myFunction);  
  
function myFunction(value, index, array) {  
txt = txt + value + "<br>";  
}
```
Note that the function takes 3 arguments:

-   The item value
-   The item index
-   The array itself

The example above uses only the value parameter. The example can be rewritten to:

```javascript
var txt = "";  
var numbers = [45, 4, 9, 16, 25];  
numbers.forEach(myFunction);  
  
function myFunction(value) {  
txt = txt + value + "<br>";  
}
```
# Object
## Object literal
```javascript
var john = {
firstName: 'John',
lastName: 'Smith',
birthYear: 1990,
family: ['Jane', 'Mark', 'Bob', 'Emily'],
job: 'teacher',
isMarried: false
};

console.log(john.firstName);
console.log(john['lastName']);
```
**You cannot get any value from an object that has not been finished being declared yet.* Like *
↓
```javascript
var john = {
firstName: 'John',
lastName: 'Smith',
birthYear: 1990,
age = 2018 - this.birthYear
};
```
This declaration will cause error.

# Function
## IIFE (Immediately Invoked Function Expressions)

```javascript
(function(x){
//do something...
})(x);
```

# Async JavaScript

## JavaScript Event Loop([resources](https://medium.com/front-end-hacking/javascript-event-loop-explained-4cd26af121d4))

![ALT](https://cdn-images-1.medium.com/max/1600/1*7GXoHZiIUhlKuKGT22gHmA.png)

- **Heap** - Objects are allocated in a heap which is just a name to denote a large mostly unstructured region of memory.
- **Stack** - This represents the <font color="red">single thread provided for JavaScript code execution</font>. Function calls form a stack of frames (more on this below)
- **Browser or Web APIs** are built into your web browser, and are able to expose data from the browser and surrounding computer environment and do useful complex things with it. <font color="red">They are not part of the JavaScript language itself, rather they are built on top of the core JavaScript language</font>, providing you with extra superpowers to use in your JavaScript code.

### Example
```javascript
function main(){
console.log('A');
setTimeout(
function exec(){ console.log('B'); }
,0);
console.log('C');
}
main();
// Output
// A
// C
// B
```    
![ATL](https://cdn-images-1.medium.com/max/2000/1*64BQlpR00yfDKsXVv9lnIg.png)
Inner working during the execution

 1. The call to the main function is first pushed into the stack (as a frame). Then the browser pushes the first statement in the main function into the stack which is console.log(‘A’). This statement is executed and upon completion that frame is popped out. Alphabet A is displayed in the console.

2.  The next statement (setTimeout() with callback exec() and 0ms wait time) is pushed into the call stack and execution starts. setTimeout function uses a Browser API to delay a callback to the provided function. The frame (with setTimeout) is then popped out once the handover to browser is complete (for the timer).

3.  console.log(‘C’) is pushed to the stack while the timer runs in the browser for the callback to the exec() function. In this particular case, as the delay provided was 0ms, the callback will be added to the message queue as soon as the browser receives it (**ideally**).

4.  After the execution of the last statement in the main function, the main() frame is popped out of the call stack, thereby making it empty. <font color="red">For the browser to push any message from the queue to the call stack, the call stack has to be empty first.</font> That is why even though the delay provided in the setTimeout() was 0 seconds, the callback to exec() has to wait till the execution of all the frames in the call stack is complete.

5.  Now the callback exec() is pushed into the call stack and executed. The alphabet B is display on the console. This is the event loop of javascript.

> *The delay parameter in setTimeout(function, delayTime) does not stand for the precise time delay after which the function is executed. It stands for the minimum wait time after which at some point in time the function will be executed.*


<br>
That's see another one deeper.
<br>


```javascript
function main(){
	console.log('A');
	setTimeout(function exec(){
		console.log('B');
	}, 0);
	
	runWhileLoopForNSeconds(3);
	console.log('C');
}
main();

function runWhileLoopForNSeconds(sec){
	let start = Date.now(), now = start;
	while (now - start < (sec*1000)) {
		now = Date.now();
	}
	console.log('Z');
}

// Output
// A
// C
// Z
// B
```
-   The function runWhileLoopForNSeconds() does exactly what its name stands for. It constantly checks if the elapsed time from the time it was invoked is equal to the number of seconds provided as the argument to the function. The main point to remember is that while loop (like many others) is a blocking statement meaning its execution happens on the call stack and does not use the browser APIs. So it blocks all succeeding statements until it finishes execution.
-   So in the above code, even though setTimeout has a delay of 0s and the while loop runs for 3s, the exec() call back is stuck in the message queue. The while loop keeps on running on the call stack (single thread) until 3s has elapsed. And after the call stack becomes empty the callback exec() is moved to the call stack and executed.
-   So the delay argument in setTimeout() does not guarantee the start of execution after timer finishes the delay. It serves as a minimum time for the delay part.

## Reference 

[Help, I'm stuck in an event loop](http://krasimirtsonev.com/blog/article/Help-Iam-stuck-in-an-event-loop-by-Philip-Roberts)

[js的單執行緒，非同步及回撥函式](https://codertw.com/ios/20423/)
