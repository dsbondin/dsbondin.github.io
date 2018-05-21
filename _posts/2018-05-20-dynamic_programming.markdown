---
layout: post
title:      "Dynamic Programming"
date:       2018-05-21 01:33:07 +0000
permalink:  dynamic_programming
---


[Dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming) is a method for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems **just once, and storing their solutions**. 

The part in bold is critical. Probably the best example of dynamic programming is a famous problem of calculating the n-th fibonacci number. 

```
function fib(n) {
  if (n === 1) {
    return 0;
  }
  if (n === 2) {
    return 1;
  }
  return fib(n - 1) + fib(n - 2);
}
```

This function works correctly but it's also extremely inefficient. `fib(42)` takes about 1 second to run on my computer, `fib(45)` - 9 seconds and `fib(48)` - 38 seconds. Time complexity of this algorithm is O(2 ** n), O of 2 to the power of n.
For example, to get fib(4) we need to add fib(3) to fib(2) which in turn is (fib(2) + fib(1)) + fib(2). fib(5) = fib(4) + fib(3) = (fib(2) + fib(1) + fib(2)) +(fib(2) + fib(1)). As you can see we are repeating a lot of calculations. Wouldn't it be good to store the previous calculations in an array? Let's see: 

```
let fibsArray = [];

function dynamicFib(n) {
  if (n === 1) {
    return 0;
  }

  if (n === 2) {
    return 1;
  }

  let fib = dynamicFib(n - 1) + (fibsArray[n - 2] || dynamicFib(n - 2));
  fibsArray[n] = fib;
  return fib;
}
```

The function is almost the same except that we replaced fib(n-2) with fibsArray[n-2] if it already exists. This function runs much faster as its time complexity is O(n).

Happy coding!
