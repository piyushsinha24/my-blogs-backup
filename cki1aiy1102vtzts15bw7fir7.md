## Javascript: ES6 & Beyond

ECMAScript 2015(_also known as ES6_) is a major update to Javascript since ES5 which was standardized in 2009\. Since then, Javascript has come up with incremental updates every year. These significant updates from ES6 and beyond are commonly referred to as **_Modern Javascript_**.

<span class="ic fq bm id ie if"></span><span class="ic fq bm id ie if"></span><span class="ic fq bm id ie"></span>

### let and const

`let` allows you to declare variables that are limited to the scope of a block statement or expression on which it is used, unlike the `var` keyword, which declares a variable globally, or locally to an entire function regardless of block scope.


```
function varTest() {
  var x = 1
  {
    var x = 2 // same variable
    console.log(x) // 2
  }
  console.log(x) // 2
}
function letTest() {
  let x = 1
  {
    let x = 2 // different variable
    console.log(x) // 2
  }
  console.log(x) // 1
}
```


`const` are block<span id="rmm"><span id="rmm">-</span></span>scoped, much like variables declared using the `let` keyword. Once used, the variable can’t be reassigned. In other words, it’s an _immutable variable_ except when it used with objects.


```
const pi = 3.14
pi = 4 // TypeError, Attempted to assign to readonly property
const name = {firstname: "piyush"}
name.firstname = "sherlock"
name.lastname = "holmes"
console.log(name) 
// Object{firstname: "sherlock", lastname: "holmes}
```


### Template Literals

Template literals allow us to embed expressions in strings with a cleaner syntax.

> Template literals are enclosed by the backtick character instead of double or single quotes.


```
// ES5
let name = "Piyush"
let msg = "Hello," + " " + name + "." // Hello, Piyush.
// ES6
let name = "Piyush"
let msg = `Hello, ${name}.` // Hello, Piyush.
```


### Arrow Functions

Arrow functions are a syntactically compact alternative to a regular function expression. It makes your code more readable and structured.


```
// ES5
const isEven = function (num) {
 return num % 2 === 0;
}
// ES6
const isEven = num => num % 2 === 0;
```


Also, you can use Arrow functions with built-in functions like `map`, `filter`, and `reduce`.


```
const nums = [1, 2, 3, 4, 5, 6, 7, 8, 9];
const odds = nums.filter(n => n % 2 === 1);
console.log(nums); // [1, 2, 3, 4, 5, 6, 7, 8, 9]
console.log(odds); // [1, 3, 5, 7, 9]
```


The handling of `this` is also different in arrow functions compared to regular functions. In regular functions, the `this` keyword represented the object that called the function, which could be the window, the document or whatever. With arrow functions, the `this` keyword always represents the object that defined the arrow function.

### Default Parameters

It allows named parameters to be initialized with default values if no value or `undefined` is passed.


```
// ES5
function multiply(a, b) {
  b = typeof b !== 'undefined' ? b : 1;
  return a * b;
}
console.log(multiply(5, 2)); // 10
console.log(multiply(5)); // 5
// ES6
function multiply(a, b = 1) {
  return a * b;
}
console.log(multiply(5, 2)); // 10
console.log(multiply(5)); // 5
```


### Spread (…)

It allows an iterable such as an array expression or string to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected, or an object expression to be expanded in places where zero or more key-value pairs (for object literals) are expected.

#### Spread for function calls

Expands an iterable(array, string, etc) into a list of arguments.


```
const nums = [1, 3, 2, 7, 5];
Math.max(nums); // NaN
// Use spread!
Math.max(...nums); // 7
```


#### Spread for array literals

Create a new array using an existing array. Spreads the elements from one array into a new array.


```
const nums1 = [1, 2, 3];
const nums2 = [4, 5, 6];
[...nums1, ...nums2]
// [1, 2, 3, 4, 5, 6]
```


#### Spread for object literals

Copies properties from one object into another object literal.


```
const name = {firstname: "piyush", lastname: "sinha"};                       
const name = {firstname: "piyush", lastname: "sinha"};
const fullAddress = {...address, country: "india"};
// {city: "mumbai", state: "maharashtra", country: "india"}
const details = {...name, ...fullAddress};
// {firstname: "piyush", lastname: "sinha", city: "mumbai", state: "maharashtra", country: "india"}
```


### Destructuring

A short, clean syntax to _unpack_ values from arrays or properties from objects into distinct variables.

#### Array Destructuring


```
const raceResults = ["Jazz", "Ibtesam", "Farhaz", "Kunal"];
const [gold, silver, bronze] = raceResults;
gold; // "Jazz"
silver; // "Ibtesam"
bronze; // "Farhaz"
const [fastest, ...everyoneElse] = raceResults;
fastest; // "Jazz"
everyoneElse; // ["Ibtesam", "Farhaz", "Kunal"]
```


#### Object Destructuring


```
const runner = {
  first: "Piyush",
  last: "Sinha", 
  country: "India"
}
const {first, last, country} = runner;
first; // "Piyush"
last; // "Sinha"
country; // "India"
```


#### Parameters Destructuring


```
const fullName = ({first, last}) => {
 return `${first} ${last}`
}
const runner = {
  first: "Piyush",
  last: "Sinha", 
  country: "India"
}
fullName(runner); // "Piyush Sinha"
```


### Iteration over Arrays/Objects

#### for…of Loop

A nice and easy way of iterating over arrays.


```
const gamers = ["Piyush", "Jazz", "Ibtesam", "Farhaz", "Kunal"];
for(const player of gamers) {
  console.log(player);
}
// Piyush
// Jazz
// Ibtesam
// Farhaz
// Kunal
```


#### for…in Loop

A nice and easy way of iterating over all enumerable properties of an object.


```
const scores = {
  piyush: 80, 
  jazz: 86, 
  ibtesam: 92,
  farhaz: 90, 
  kunal: 88
}
for(const score in scores) {
  console.log(scores[score]);
}
// 80
// 86
// 92
// 90
// 88
```


### Promises

A `Promise` is an object representing the eventual completion or failure of an asynchronous operation. It is a returned object to which you attach callbacks instead of passing callbacks into a function. They are easy to manage when dealing with multiple asynchronous operations where callbacks can create callback hell leading to unmanageable code.

The constructor syntax for a promise object is:


```
let promise = new Promise(function(resolve, reject) {  
 // do something
});
```


The constructor takes only one argument, a callback function. The callback function takes two arguments, _resolve_ and _reject._ When the operation obtains the result, be it soon or late, doesn’t matter, it should call one of these callbacks:

*   `resolve(value)`: if the operation finished successfully, with result `value`.
*   `reject(error)`: if an error occurred, `error` is the error object.

The `promise` object returned by the `new Promise` constructor has these internal properties:

*   `state` — initially `"pending"`, then changes to either `"resolved"` when `resolve` is called or `"rejected"`when `reject` is called.
*   `result` — initially `undefined`, then changes to `value` when `resolve(value)` called or `error` when `reject(error)` is called.

Let’s demonstrate a fake request using Promises to get more clarity:


```
const fakeRequestPromise = (url) => {
  return new Promise((resolve, reject) => {
    const delay = Math.floor(Math.random()*(4500)) + 500;
    setTimeout(() => {
      if(delay>4000) {
        reject("Connection Timeout!");
      } else {
        resolve(`Here is your fake data from ${url}`);
      }
    }, delay)
  })
}
```


I am using a random delay in setTimeout() to mimic an API request which has a probability for success as well as failure.

If delay < 4000,


```
fakeRequestPromise("www.fakeapi.com/page1")
// Promise {[[PromiseStatus]]: pending, [[PromiseValue]]: undefined}
// Promise {[[PromiseStatus]]: resolved, [[PromiseValue]]: "Here is your fake data from www.fakeapi.com/page1"}
```


else,


```
fakeRequestPromise("www.fakeapi.com/page1")
// Promise {[[PromiseStatus]]: pending, [[PromiseValue]]: undefined}
// Promise {[[PromiseStatus]]: rejected, [[PromiseValue]]: "Connection Timeout!"}
```


The returned promise object can be consumed by registering functions using _.then_ and _.catch_ methods.


```
const request = fakeRequestPromise("www.fakeapi.com/page1");
request
   .then(() => {
      console.log("Promise Resolved");
      console.log("IT WORKED !!!");
   })
   .catch(() => {
      console.log("Promise Rejected");
      console.log("OH NO, ERROR !!!");
   })
//If delay < 4000
Promise Resolved
IT WORKED !!!
//else
Promise Rejected
OH NO, ERROR !!!
```


Promises are the ideal choice for handling asynchronous operations in the simplest manner. They can handle multiple asynchronous operations easily and provide better error handling than callbacks and events.

<span class="ic fq bm id ie if"></span><span class="ic fq bm id ie if"></span><span class="ic fq bm id ie"></span>

### Conclusion

There is much, much more that ES6 offers us to make our code syntactically cleaner and smarter. I hope I was able to introduce you to some of the ES6 features.

If you have any problems, just respond to this article. Happy Coding!

%%[buycoffee]
