Synopsis
> Higher-order functions are beautifully concise yet expressive when dealing with data. Elevate your functional programming skills by learning ES6 higher-order functions for Arrays!

JavaScript has great support for **higher-order functions**, in such a way it feels natural to work with them. This special kind of function, which either take functions as input or outputs another function, is widely used for event callbacks, Node.js middlewares and data manipulation.

I have covered the fundamentals of higher-order functions in JavaScript in my previous post **Catching up with JavaScript higher-order functions**<sup>[Catching up with JavaScript higher-order functions](https://www.airpair.com/javascript/posts/catching-up-with-javascript-higher-order-functions)</sup>. 

Let's give them some real meaning cracking ES6 higher-order functions for Arrays!

## ES6 Arrow notation 101

Before we go any further, be advised we will be using ES6 Arrow notation for functions. If you are already familiar with this new notation, here's a quick refresher.

ES6 Arrow notation is fairly simple. It's just a brand new way to write short functions, which will basically make the code uncluttered, making it easier to read and write higher-order functions - which goes great with functional programming.

There are just 2 things you need to know about the arrow notation at this point:

### 1) Statement block, a.k.a. arrow function with a body

Given some ordinary functions:

```javascript
var whatTimeIsIt = function () {
    return new Date();
};

var averageOfThree = function (a, b, c) {
    var sum = a + b + c;
    return sum / 3;
};

var deconstruct = function (number) {
    return {
        integral: Math.trunc(number),
        fractional: Number(number.toString().split('.')[1] || 0)
    };
};
```

The same functions can be rewritten in arrow notation using statement block, by just replacing the declaration ```function (...)``` with ```(...) =>```, and the body remains the same:

```javascript
var whatTimeIsIt = () => {
    return new Date();
};

var averageOfThree = (a, b, c) => {
    var sum = a + b + c;
    return sum / 3;
};

// When there is just 1 parameter, the parenthesis before the => can be omitted
var deconstruct = number => {
    return {
        integral: Math.trunc(number),
        fractional: Number(number.toString().split('.')[1] || 0)
    };
};
```

### 2) Expression block, a.k.a. arrow function without a body 

When your function is just meant to return something in one-line, you can use the expression block to simplify it even further, by just replacing ```{ ... }``` with the expression to be returned:

```javascript
var whatTimeIsIt = () => new Date();

var averageOfThree = (a, b, c) => (a + b + c) / 3;

// When the return is an object, a parenthesis after the => is needed
var deconstruct = number => ({
    integral: Math.trunc(number),
    fractional: Number(number.toString().split('.')[1] || 0)
});
```

### More on Arrow functions

There are many other cool things about arrow functions, like the **lexical this binding**. Learn more with two great and free online books: **Exploring ES6**<sup>[Exploring ES6 from Dr. Axel Rauschmayer](http://exploringjs.com/es6/ch_arrow-functions.html)</sup> from Dr. Axel Rauschmayer and **Understanding ECMAScript 6**<sup>[Understanding ECMAScript 6 from Nicholas Zakas](https://leanpub.com/understandinges6/read/#leanpub-auto-arrow-functions)</sup> from Nicholas Zakas.

## Higher-order functions for Arrays

Now, let's have a look on some ES6 high-order functions for Arrays. They will allow you to write code using Functional approach, so that you will no longer need to use Imperative ```for```/```while``` loops to process arrays. You will soon enough understand how much better it is to work with those high-order functions compared to loops, as they are much more expressive, readable, reusable, maintainable and consequentially more elegant.

In order to illustrate, let's take the following Imperative program and rewrite it:

```javascript
var starships = require('./starships.json');

// How many civilians can be transported in an emergency escape operation?
// 5 Imperial shuttle, 10 Theta-class T-2c shuttle, 2 Sentinel-class landing craft
var civilians = 0;
for (var i = 0; i < starships.length; i++) {
    var starship = starships[i];
    if (starship.name === 'Imperial shuttle') {
        civilians += 5 * parseInt(starship.passengers, 10);
    }
    else if (starship.name === 'Theta-class T-2c shuttle') {
        civilians += 10 * parseInt(starship.passengers, 10);
    }
    else if (starship.name === 'Sentinel-class landing craft') {
        civilians += 2 * parseInt(starship.passengers, 10);
    }
}
```

See how difficult it is to grasp the functioning of this code in a glance. There is some code repetition (```if```/```else``` blocks), which makes it harder to notice important varying details (the number of starships per type). It is also not crystal clear what is the output of this code.

### 1) Array.prototype.forEach

Syntax: ```array.forEach(callback)```

Introduced in ES5, this higher-order function executes the provided ```callback``` once for each element present in the array in ascending order.

The callback receives three arguments:

1. The current element value
1. The current element index
1. The array being traversed

This is how it works:

1. ```callback``` is applied to the 1st element of the original array.
1. ```callback``` is applied to the 2nd element of the original array.
1. And so on... until the last element of the array is processed.

Now, let's transform that old-fashioned ```for``` loop to ```forEach```:

```javascript
var starships = require('../starships.json');

// How many civilians can be transported in an emergency escape operation?
// 5 Imperial shuttle, 10 Theta-class T-2c shuttle, 2 Sentinel-class landing craft
var civilians = 0;
starships.forEach( starship => {
    if (starship.name === 'Imperial shuttle') {
        civilians += 5 * parseInt(starship.passengers, 10);
    }
    else if (starship.name === 'Theta-class T-2c shuttle') {
        civilians += 10 * parseInt(starship.passengers, 10);
    }
    else if (starship.name === 'Sentinel-class landing craft') {
        civilians += 2 * parseInt(starship.passengers, 10);
    }
} );
```

For more details on ```Array.prototype.forEach``` see the **Mozilla Developer Network docs**<sup>[MDN - Array.prototype.forEach()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)</sup>.

### 2) Array.prototype.filter

Syntax: ```array.filter(callback)```

Introduced in ES5, this higher-order function returns a new subarray with every element from the original array that passes on the test provided by ```callback```, i.e., if ```callback``` returns  ```true``` for a particular element, then this element gets included in the subarray to be returned.

The callback receives three arguments:

1. The current element value
1. The current element index
1. The array being traversed

This is how it works:

1. ```callback``` is applied to the 1st element of the original array, and if the result is ```true```, then this element gets included in the new array to be returned.
1. ```callback``` is applied to the 2nd element of the original array, and if the result is ```true```, then this element gets included in the new array to be returned.
1. And so on... until the last element of the array is processed, then the new array is returned as the result of ```filter```.

This time, we will modify the program by first filtering out the elements that aren't part of the operation. This way, ```filter``` will result in a subarray containing just the elements known to be part of the operation, so that we will no longer need the ```if```/```else``` blocks. Then, this subarray will call  ```forEach``` to continue with the calculation of civilians.

```javascript
var starships = require('../starships.json');

var operation = {
    'Imperial shuttle': 5,
    'Theta-class T-2c shuttle': 10,
    'Sentinel-class landing craft': 2
};

// How many civilians can be transported in an emergency escape operation?
var civilians = 0;
starships
    .filter( starship => starship.name in operation )
    .forEach( starship => {
        civilians += operation[starship.name] * parseInt(starship.passengers, 10);
    } );
```

For more details on ```Array.prototype.filter``` see the **Mozilla Developer Network docs**<sup>[MDN - Array.prototype.filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)</sup>.

### 3) Array.prototype.map

Syntax: ```array.map(callback)```

Introduced in ES5, this higher-order function returns a new array with the results of ```callback``` applied to every element from the original array, in the same order. 

The callback receives three arguments:

1. The current element value
1. The current element index
1. The array being traversed

This is how it works:

1. ```callback``` is applied to the 1st element of the original array, and the result is stored as the 1st element of the new array to be returned.
1. ```callback``` is now applied to the 2nd element of the original array, and the result is stored as the 2nd element of the new array to be returned.
1. And so on... until the last element of the array is processed, then the new array is returned as the result of ```map```.

Next we will modify again the program by mapping the calculation of civilians in a separated step, right after ```filter```. This way, ```map``` will result in a new array containing the number of civilians, in the same order of the starships elements right after ```filter```. Now, ```forEach``` can be called over the  civilian numbers, simply to sum up the civilians total.

```javascript
var starships = require('../starships.json');

var operation = {
    'Imperial shuttle': 5,
    'Theta-class T-2c shuttle': 10,
    'Sentinel-class landing craft': 2
};

// How many civilians can be transported in an emergency escape operation?
var civilians = 0;
starships
    .filter( starship => starship.name in operation )
    .map( starship => operation[starship.name] * parseInt(starship.passengers, 10) )
    .forEach( number => { civilians += number; } );
```

For more details on ```Array.prototype.map``` see the **Mozilla Developer Network docs**<sup>[MDN - Array.prototype.map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)</sup>.

### 4) Array.prototype.reduce

Syntax: ```array.reduce(callback, initialValue)```

Introduced in ES5, this higher-order function reduces an array into a single value, as explained in **Understand Javascript Array Reduce in 1 Minute**<sup>[Understand Javascript Array Reduce in 1 Minute](https://www.airpair.com/javascript/javascript-array-reduce)</sup>. It first applies ```callback``` against ```initialValue``` and then consecutively on each value of the array, from left to right.

The callback receives four arguments:

1. The previous value
1. The current value
1. The current index
1. The array being traversed

This is how it works:

1. ```callback``` is applied to ```initialValue```, which comes as the 2nd argument (the current value).
1. ```callback``` is now applied to the 1st element of the array (which comes as the 2nd argument, the current value), and the result of the previous step comes as the 1st argument, the previous value).
1. ```callback``` is now applied to the 2st element of the array (which comes as the 2nd argument, the current value), and the result of the previous step comes as the 1st argument, the previous value).
1. And so on... until the last element of the array is processed, and this final result is then returned as the result of ```reduce```.

Once more we will modify the program by reducing the calculation of civilians in a separated step, encompassing both the calculation of civilians per starship type (previously covered by ```map```) and summing up the civilians total (previously covered by ```forEach```).

```javascript
var starships = require('../starships.json');

var operation = {
    'Imperial shuttle': 5,
    'Theta-class T-2c shuttle': 10,
    'Sentinel-class landing craft': 2
};

// How many civilians can be transported in an emergency escape operation?
var civilians = starships
    .filter( starship => starship.name in operation )
    .reduce( (previous, current) => 
        previous + operation[current.name] * parseInt(current.passengers, 10), 0 );
```

For more details on ```Array.prototype.reduce``` see the **Mozilla Developer Network docs**<sup>[MDN - Array.prototype.reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)</sup>.

### 5) Array.prototype.some

Syntax: ```array.some(callback)```

Introduced in ES5, this higher-order function returns ```true``` if there is any element in the array that passes on the test provided by ```callback```, and ```false``` otherwise.

The callback receives three arguments:

1. The current element value
1. The current element index
1. The array being traversed

This is how it works:

1. ```callback``` is applied to the 1st element of the array, and if the result is ```true```, then ```some``` returns ```true```.
1. Otherwise, ```callback``` is applied to the 2nd element of the array, and again if the result is ```true```, then ```some``` returns ```true```.
1. And so on... until the last element of the array is processed, if ```true``` was never returned then ```some``` returns ```false```.

Now, a new program to check if can any ship travel faster than 1000 mph, using ```filter``` and ```some```:

```javascript
var starships = require('../starships.json');

var operation = {
    'Imperial shuttle': 5,
    'Theta-class T-2c shuttle': 10,
    'Sentinel-class landing craft': 2
};

var canAnyShipTravelFasterThan1000mph = starships
    .filter( starship => starship.name in operation )
    .some( starship => parseInt(starship.max_atmosphering_speed, 10) >= 1000 );
```

For more details on ```Array.prototype.some``` see the **Mozilla Developer Network docs**<sup>[MDN - Array.prototype.some()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some)</sup>.

### 6) Array.prototype.every

Syntax: ```array.every(callback)```

Introduced in ES5, this higher-order function returns ```true``` if every element in the array passes on the test provided by ```callback```, and ```false``` otherwise.

The callback receives three arguments:

1. The current element value
1. The current element index
1. The array being traversed

This is how it works:

1. ```callback``` is applied to the 1st element of the array, and if the result is ```false```, then ```every``` returns ```false```.
1. Otherwise, ```callback``` is applied to the 2nd element of the array, and again if the result is ```false```, then ```every``` returns ```false```.
1. And so on... until the last element of the every is processed, if ```false``` was never returned then ```every``` returns ```true```.

Now, a new program to check if can the whole operation travel faster than 1000 mph, using ```filter``` and ```every```:

```javascript
var starships = require('../starships.json');

var operation = {
    'Imperial shuttle': 5,
    'Theta-class T-2c shuttle': 10,
    'Sentinel-class landing craft': 2
};

var canOperationTravelFasterThan1000mph = starships
    .filter( starship => starship.name in operation )
    .every( starship => parseInt(starship.max_atmosphering_speed, 10) >= 1000 );
```

For more details on ```Array.prototype.every``` see the **Mozilla Developer Network docs**<sup>[MDN - Array.prototype.every()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every)</sup>.

### 7) Array.prototype.find and Array.prototype.findIndex

Syntax: ```array.find(callback)``` and ```array.findIndex(callback)```

Introduced in ES6, these two higher-order functions return the first element value (for ```find```) or index (for ```findIndex```) in the array that passes on the test provided by ```callback```. If there is no match, it returns ```undefined``` (for ```find```) or ```-1``` (for ```findIndex```).

The callback receives three arguments:

1. The current element value
1. The current element index
1. The array being traversed

This is how it works:

1. ```callback``` is applied to the 1st element of the original array, and if the result is ```true```, then this element is returned as the result of ```find```/```findIndex```.
1. Otherwise, ```callback``` is applied to the 2nd element of the original array, and again if the result is ```true```, then this element is returned as the result of ```find```/```findIndex```.
1. And so on... until the last element of the array is processed, if ```true``` was never returned then ```find``` returns ```undefined``` / ```findIndex``` returns ```-1```.

Now, a new program to check which ship can travel faster than 2000 mph, using ```find``` for the value and ```findIndex``` for the index:

```javascript
var starships = require('../starships.json');

var operation = {
    'Imperial shuttle': 5,
    'Theta-class T-2c shuttle': 10,
    'Sentinel-class landing craft': 2
};

// Find ship
var firstShipThatCanTravelFasterThan2000mph = starships
    .find( starship => 
        starship.name in operation
        && parseInt(starship.max_atmosphering_speed, 10) >= 2000 );

// Find index
var firstShipIndexThatCanTravelFasterThan2000mph = starships
    .findIndex( starship => 
        starship.name in operation
        && parseInt(starship.max_atmosphering_speed, 10) >= 2000 );
```

For more details on ```Array.prototype.find``` and ```Array.prototype.findIndex``` see the **Mozilla Developer Network docs**<sup>[MDN - Array.prototype.find()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find) and [MDN - Array.prototype.findIndex()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)</sup>.

## Your own higher-order function for Array

bla bla bla
