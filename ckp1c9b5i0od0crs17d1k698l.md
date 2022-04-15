## JS ES2021 - It's Almost Here

Every year, Javascript update adds new features. ES2021 *(also known as ES12)* is planned to be released in June this year. New features that are added each year go through a four-stage process. All the features listed below, at the time of writing have already reached the last stage and are very much ready for the release.

### String.prototype.replaceAll

In Javascript, `replace()` method only replaces the first occurrence of a pattern in a string. If we want to replace all the matches of a pattern in a string, the only way to achieve that is we supply the pattern as a `regular expression`.

```js
const str = "macOS is way better than windows. I love macOS.";
const newStr = str.replace("macOS", "Linux");
console.log(newStr);
// Linux is way better than windows. I love macOS.

const newStr2 = str.replace(/macOS/g,"Linux");
console.log(newStr2);
// Linux is way better than windows. I love Linux.
```

The proposed method `replaceAll()` returns a new string with all matches of a pattern replaced by a replacement.

```js
const str = "macOS is way better than windows. I love macOS.";
const newStr = str.replaceAll("macOS", "Linux");
console.log(newStr);
// Linux is way better than windows. I love Linux.
```

### Logical Assignment Operator

With the newly proposed logical assignment operators - `&&=`, `||=` and `??=`, we can assign a value to a variable based on a logical operation. It combines the logical operation with the assignment expression. 

#### Logical AND assignment (&&=)

The Logical AND assignment operator performs the assignment only when the left operand is `truthy`. Otherwise, if the left operand is `falsy` (`false`, `0`, `-0`, `0n`, `“”`, `null`, `undefined` and `NaN`), the assignment is not performed.

```js
let x = 10;
let y = 15;

x &&= y;
// Equivalent: x && (x = y)

console.log(x);
// 15

x = 0;
x &&= y;
console.log(x);
// 0
```

If it’s hard to grasp the operator, think of it as `if(x) { x = y; }`.

#### Logical OR assignment (||=)

The Logical OR assignment operator performs the assignment only when the left operand is `falsy`(`false`, `0`, `-0`, `0n`, `“”`, `null`, `undefined` and `NaN`). Otherwise, if the left operand is `truthy`, the assignment is not performed.

```js
let x = null;
let y = 15;

x ||= y;
// Equivalent: x || (x = y)

console.log(x);
// 15

x = 10;
x ||= y;
console.log(x);
// 10
```

If it’s hard to grasp the operator, think of it as `if(!x) { x = y; }`.

#### Logical Nullish assignment (??=)

The Logical Nullish assignment operator performs the assignment only when the left operand is `nullish`(`undefined` or `null`). Otherwise, the assignment is not performed.

```js
let x = null;
let y = 15;

x ??= y;
// Equivalent: x ?? (x = y)

console.log(x);
// 15

x = 10;
x ??= y;
console.log(x);
// 10
```

If it’s hard to grasp the operator, think of it as `if(x == null || x == undefined) { x = y; }`.

### Numeric Separators

Large numeric literals are difficult for the human eye to parse quickly. For example, consider the number `1019436871.42`. We have to pay close attention to see that it’s billion something. 

To improve the readability, this new addition to Javascript language enables `underscores` as separators in numeric literals. We can re-write the same number as `1_019_436_871.42`. And it works for all kinds of numeric literals:

```js
// A decimal integer literal with its digits grouped per thousand:
1_000_000_000_000
// A decimal literal with its digits grouped per thousand:
1_000_000.220_720
// A binary integer literal with its bits grouped per octet:
0b01010110_00111000
// A binary integer literal with its bits grouped per nibble:
0b0101_0110_0011_1000
// A hexadecimal integer literal with its digits grouped by byte:
0x40_76_38_6A_73
// A BigInt literal with its digits grouped per thousand:
4_642_473_943_484_686_707n
```

> 
Note: It does not affect the outcome. Just improves readability.

### WeakRef

In JavaScript, the object is not collected by the garbage collector as long as a reference to that object exists. It stops the garbage collector from re-claiming the memory. `ES12` is coming with a solution - `WeakRef`. 

A `WeakRef` object contains a weak reference to an object, which is called its `target` or `referent`. Unlike a strong reference, it doesn't prevent the object from being collected by the garbage collector. 

We can create a Weak Reference by using `new WeakRef()`:

```js
const myObject = new WeakRef({
     name: "John Doe",
     gender: "Male",
     age: "24"
});

myObject.deref();
// {name: "John Doe", gender: "Male", age: "24"}
myObject.deref().name;
// "John Doe"
```

The `WeakRef` can be useful in some cases, the `TC39 Proposal ` advises avoiding it if possible.

### Promise.any

ES2021 will introduce `Promise.any()` method which short-circuits and returns a value, as soon as it hits the `first resolved promise` from the list/array of promises. If all the promises are rejected then it will throw an `AggregateError`, a new subclass of `Error` that groups together individual errors.

Unlike the `Promise.race()` method which focuses on the promise which `settles` the first, the `Promise.any()` method focuses on the promise which `resolves` the first.

```js
const promise1 = Promise.reject(0);
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'quick'));
const promise3 = new Promise((resolve) => setTimeout(resolve, 500, 'slow'));

const promises = [promise1, promise2, promise3];

Promise.any(promises).then((value) => console.log(value));
// quick
```
 
```js
const promise1 = Promise.reject(0);
const promise2 = Promise.reject(0);
const promise3 = Promise.reject(0);

const promises = [promise1, promise2, promise3];

Promise.any(promises).then((value) => console.log(value));
// AggregateError: All promises were rejected
```

### Conclusion

As a developer, it is important to stay up to date with the new features of a language. I hope I was able to introduce you to the new features coming to Javascript with ES2021. 

### References

- [TC39 Finished Proposals](https://github.com/tc39/proposals/blob/master/finished-proposals.md)
- [Catalin's Javascript ES2021](https://catalins.tech/javascript-es2021-you-need-to-see-these-ecmascript-2021-features)