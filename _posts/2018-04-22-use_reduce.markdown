---
layout: post
title:      "Use reduce()"
date:       2018-04-23 00:48:53 +0000
permalink:  use_reduce
---


We all used reduce() method at least once, most likely to sum up an array of numbers. However this method is a lot more powerful and in this post I'd like to show some ways to achieve very cool things. 

Reduce() got its name for a reason: this method takes an array it is applied to and reduces it to a single value. The value could be a number, a string, another array or even an object. Here's the most basic example:

```
[1, 2, 3, 4].reduce((prev, next) => {
  return prev + next;
}, 0)

// => 10
```

On the first iteration `prev` is 0 and `next` is 1, the returned sum becomes `prev` in the second iteration and `next` is 2 and so on. Using the second argument (0) is optional, if none was passed iteration would start with the first element of the array.

Here's an example of using reduce() as a filter: 

```
[1, 2, 3, 4].reduce((prev, next) => {
  if (next % 2 === 0) prev.push(next);
  return prev;
}, [])

// => [ 2, 4 ]
```

The initial value is an empty array in which we push only even numbers of the input array.

Lets say we're given this object and we need to get the city with the hottest weather:

```
const weather = {
  "New York": 65,
  "Washington, DC": 67,
  "Miami": 78,
  "San Francisco": 62
}

const hottest = Object.keys(weather)
  .reduce((prev, next) => {
    return weather[prev] > weather[next] ? prev : next;
  });

console.log(hottest)

// Miami
```

Here we are comparing the temperatures of the first and the second city and returning the higher one, then compare the returned with the third one and so on. 

In the following example we need to transform an array of producs into an object of products sorted by manufacturer: 

```
const products = ['Apple, iPhone, $699',
                  'Apple, iMac, $1899',
                  'Apple, airPods, $159',
                  'Apple, iPod, $499',
                  'Microsoft, Surface Pro, $799',
                  'Microsoft, XBox One, $499']

let output = products
  .map(line => line.split(', '))
  .reduce((manufacturer, product) => {
    manufacturer[product[0]] = manufacturer[product[0]] || [];
    manufacturer[product[0]].push({productName: product[1], price: product[2]})
    return manufacturer;
  }, {});

console.log(JSON.stringify(output, null, 2));

/*
{
  "Apple": [
    {
      "product": "iPhone",
      "price": "$699"
    },
    {
      "product": "iMac",
      "price": "$1899"
    },
    {
      "product": "airPods",
      "price": "$159"
    },
    {
      "product": "iPod",
      "price": "$499"
    }
  ],
  "Microsoft": [
    {
      "product": "Surface Pro",
      "price": "$799"
    },
    {
      "product": "XBox One",
      "price": "$499"
    }
  ]
}
*/
```

First split each string of the original array using map method to transform it into such format: 

```
[ [ 'Apple', 'iPhone', '$699' ],
  [ 'Apple', 'iMac', '$1899' ],
  [ 'Apple', 'airPods', '$159' ],
  [ 'Apple', 'iPod', '$499' ],
  [ 'Microsoft', 'Surface Pro', '$799' ],
  [ 'Microsoft', 'XBox One', '$499' ] ]
```

Then start iterating through each element taking an empty object as an initial value. Then create a key of that object that takes a name of `product[0]` - 'Apple' in the first iteration. The corresponding value is an empty array where we push product name and product price. This might take some time to fully understand how it works but the result is well worth it :). 

Happy coding!

