---
title: '[JavaScript] To Be Null or To Be Undefined: That’s a Big Problem'
date: '2024-10-04'
---

Beginners often confuse `null` with `undefined` in JavaScript. In this article, I’ll explain the differences and use cases of these two similar concepts, which can both be thought of as meaning "nothing" in natural language.

### What is `null`?
The value ***`null`*** represents the intentional absence of any object value. It is one of JavaScript’s primitive values and is treated as "falsy" in boolean contexts. `null` is an **assignment value**, meaning it is explicitly assigned to a variable to indicate that it has no value.

### What is `undefined`?
The value ***`undefined`*** represents the absence of an assigned value. It is also a primitive type in JavaScript. `undefined` occurs when:
- A variable is declared but not initialized.
- A function does not explicitly return a value.
- Attempting to access a property that doesn’t exist on an object.

### Key Differences
`null` and `undefined` are distinct types:
- `undefined` is its own type (`undefined`), while `null` is an object.
  
### Proof:
```javascript
console.log(null === undefined);  // false (not the same type)
console.log(null == undefined);   // true (same value after type coercion)
console.log(null === null);       // true (both type and value are the same)
```

### Metaphor for Understanding
I came across a great metaphor on Stack Overflow to explain the difference between `null` and `undefined`:

- A non-zero value is like a toilet paper holder with a roll of toilet paper on it.
- A zero value is like a holder with an empty toilet paper tube.
- A `null` value is like a holder with no tube at all.
- An `undefined` value is like the holder itself being missing entirely.

This analogy helps illustrate how `null` and `undefined` differ in representing "nothing."

### Source:
[Stack Overflow: What is the difference between null and undefined in JavaScript?](https://stackoverflow.com/questions/5076944/what-is-the-difference-between-null-and-undefined-in-javascript)
