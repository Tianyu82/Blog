---
title: '[Java] Custom Comparators Must Be Reference Types'
date: '2024-10-07'
---

When sorting arrays with custom rules in Java, new programmers often make the mistake of using custom comparators with primitive types. In Java, **custom comparators work only on reference types**, not on primitive types like `int`, `double`, or `char`. If you aren't aware of this, take a few minutes to read through this blog to avoid common errors when sorting arrays.

### Why Custom Comparators Don’t Work with Primitive Types

Java's `Comparator<T>` interface is designed to compare **objects**, not primitive types. Primitive types such as `int`, `double`, and `char` are not objects, and Java's comparator mechanism requires reference types. If you want to compare primitive values using a custom comparator, you must use their corresponding wrapper classes, like:
- `Integer` for `int`
- `Double` for `double`
- `Character` for `char`

### Example

Let’s say we want to sort an array of integers based on the leftmost digit in **decreasing** order. For example, 9 is greater than 36 because `'9'` is greater than `'3'`. Here's an incorrect implementation using primitives:

```java
int[] nums = {3, 26, 91, 44};
Arrays.sort(nums, (a, b) -> {  // Wrong: Custom comparators only work on reference types, not primitive types.
    int leftMost_a = getLeftMost(a);   
    int leftMost_b = getLeftMost(b);
    return Integer.compare(leftMost_a, leftMost_b) * -1;
});
```

The above code results in an error because `Arrays.sort` with a custom comparator cannot be applied to `int[]`. To fix this, we need to convert the array to a reference type, such as `String[]`.

### Correct Implementation

Here’s how you can modify the solution to use a reference type (in this case, `String`) instead of the primitive `int`:

```java
String[] strs = new String[nums.length];
for (int i = 0; i < strs.length; i++) {
    strs[i] = Integer.toString(nums[i]);
}

Arrays.sort(strs, (a, b) -> {  
    char leftMost_a = getLeftMostChar(a);   
    char leftMost_b = getLeftMostChar(b);
    return Character.compare(leftMost_a, leftMost_b) * -1;
});
```

