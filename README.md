# Barely Functional
*A tiny functional programming library*

**barely-functional** is a small, highly modular library for doing functional programming. It includes `curry`
and `compose`, and wraps several native ES5 methods including `.map()` `.reduce()` and `.filter()` and several ES6
methods such as `.every()` and `.find()` with an iteratee first, autocurried API. Several 
non-native functions inspired by (READ: stolen from) [ramdajs](http://ramdajs.com/) are also included.

**barely-functional** also attempts to guard against unsafe operations by 
returning a [`Maybe`](https://downloads.haskell.org/~ghc/7.4-latest/docs/html/libraries/base-4.5.1.0/Data-Maybe.html) for operations 
that can fail. The `Maybe` type returned is compliant with the [Fantasy Land]
(https://github.com/fantasyland/fantasy-land) specification. 

All functions may be imported individually, thus reducing the footprint of the library even further:

```js
const map = require('barely-functional/map')
```


## Installing

`npm i barely-functional`

## Using
```js
const _ = require('barely-functional')

const sumOfEven = _.compose(_.sum, _.filter((e) => e % 2 === 0))
```

*Barely functional is written in es6. It will run on node.js v4.0 and up. If you wish to use it in the browser you need to use babel and (webpack/browserify)*


## But ramda is way better...
Yes, it is, but it is also a lot bigger. If you a writing a small module, you might not want to include all of [Ramda](http://ramdajs.com).

## API

### append :: a -> [a] -> [a]
Adds the specified value to the end of the supplied array.
```js
_.append(5, [1, 2, 3, 4]);
// => [1, 2, 3, 4, 5]
```

### compose :: ((y -> z), (x -> y) ... (o -> p)(a, b, ..., n -> o)) -> (a, b, ..., n -> z)
Performs right-to-left function composition. The rightmost function may have any arity; the remaining functions must be unary.

```js
const add1  =  (n) => n + 1
const double  =  (n) => n * 2

const add1AndDouble = _.compose(double, add1)

add1AndDouble(5)
// => 12

```

### concat :: [a] -> [a] - [a]
Concatenates two arrays.
```js
_.concat([1, 2, 3], [4, 5, 6]);
// => [1, 2, 3, 4, 5, 6];
```

### curry :: (\* -> a) -> (\s* -> a)
Returns a curried equivalent of the provided function. The curried function has two unusual capabilities. First, its arguments needn't be provided one at a time. If `f` is a ternary function and `g` is `_.curry(f)`, the following are equivalent:

```js
const sum3 = _.curry((a, b, c) => a + b + c)
sum3(1)(2)(3)
// => 6
sum3(1)(2, 3)
// => 6
sum3(1, 2)(3)
// => 6
sum3(1, 2, 3)
// => 6
```

### drop :: Int -> [a] -> [a]
Returns a new list without the first `n` elements

```js
_.drop(2, [1, 2, 3, 4])
// => [3, 4]
```

### every :: (a -> Boolean) -> [a] -> Boolean
Returns true if every element in the list satisfies the predicate function.

```js
_.every(n => n > 2, [2, 3, 4])
// => false

_.every(n => n > 2, [3, 4, 5])
// => true
```

### filter :: (a -> Boolean) -> [a] -> [a]
Returns a new list containing only the elements in the list that satisfy the predicate function.

```js
_.filter(n => n > 2, [1, 2, 3, 4])
// => [3, 4]
```

### find :: (a -> Boolean) -> [a] -> Maybe a
Returns `Just` the first element satisfying the predicate function. If no 
element can be found it returns `Nothing()`.
```js
_.find(n => n % 2 === 0, [1, 2, 3, 4])
// => Just(2)
_.find(n => n % 2 === 0, [1, 3, 5])
// => Nothing()
```

### findIndex :: (a -> Boolean) -> [a] -> Maybe Int
Returns `Just` the index of first element satisfying the predicate function. 
If no element can be found it returns `Nothing()`.
```js
_.findIndex(n => n % 2 === 0, [1, 2, 3, 4])
// => Just(1)
_.findIndex(n => n % 2 === 0, [1, 3, 5])
// => Nothing()
```

### fromMaybe :: a -> Maybe a -> a
Returns a default if second argument is Nothing(); otherwise unwraps the Just.

```js
_.fromMaybe(5, Just(1))
// => Just(1)
_.fromMaybe(5, Nothing())
// => 5
```

### has :: String -> Object -> Boolean
Returns `true` if the given object has a property with the specified name; `false` otherwise.
```js
_.has('a', {a:2})
// => true
_.has('b', {a:2})
// => false
```

### head :: [a] -> a
Returns the first element of a list.
```js
_.head([1, 2, 3])
// => 1
```

### includes :: a -> [a] -> Boolean
Returns `true` if the list includes the given element; `false` otherwise.
```js
_.includes('a', ['a', 'b', 'c'])
// => true
```

### indexOf :: a -> [a] -> Maybe Int
Returns `Just` the index of the specified element in the list. Returns 
`Nothing()` if the element is not in the list.
```js
_.indexOf(1, [0, 1, 2, 3])
// => Just(1)
_.indexOf(5, [0, 1, 2, 3])
// => Nothing()
```

### init :: [a] -> [a]
Returns all elements of the list except the last.
```js
_.init([1, 2, 3, 4]])
// => [1, 2, 3]
```

### join :: String -> [a] -> String
Returns a string by concatenating all elements of the list interlaced with the specified string.

```js
_.join('|', ['a', 'b', 'c'])
// => "a|b|c"
```

### keys :: Object -> [String]
Returns a list of the given object's keys.

```js
_.keys({a: 1, b: 2, c: 3})
// => ["a", "b", "c"]
```

### last :: [a] -> a
Returns the last element of the list.

```js
_.last([1, 2, 3])
// => 3
```

### length :: [a] | String | Function -> Int
Returns the length of a list / function / string.

```js
_.length([1, 2, 3])
// => 3
```

### map :: (a -> b) -> [a] -> [b]
Returns a new list, constructed by applying the supplied unary function to every 
element of the supplied list.

```js
_.map(n => n + 1, [1, 2, 3])
 // => [2, 3, 4]
```

### match :: RegExp -> String -> [String | void 0]
Tests a regular expression against a string and returns a list of matches.

```js
_.match(/a./g, 'falafel')
// => ['al', 'af']
```

### merge :: Object -> Object -> Object
Merges two objects into a new object; duplicate keys are resolved as last in wins. As per
`Object.assign`, nested arrays and objects are copied by reference.

```js
_.merge({a: 1}, {b: 2});
// => {a: 1, b: 2}

_.merge({a: 1, b: 1}, {b: 2});
// => {a: 1, b: 2}
```

### mergeAll :: [Object] -> Object
Merges a list of objects into a new object; duplicate keys are resolved as last in wins. As per
`Object.assign`, nested arrays and objects are copied by reference.

```js
_.mergeAll([{a: 1}, {b: 2}, {c: 3}]);
// => {a: 1, b: 2, c: 3}

_.mergeAll([{a: 1}, {b: 2}, {b: 3}]);
// => {a: 1, b: 3}
```

### nth :: Int -> [a] -> Maybe a
Returns `Just` the `nth` element of a list. If the index queried is greater than
the length of the list then return `Nothing()`.
```js
_.nth(3, [1, 2, 3, 4, 5])
// => Just(4)
_.nth(10, [1, 2, 3, 4, 5])
// => Nothing()
```

### pluck :: String -> [Object] -> [a | void 0]
Returns the values associated with the provided key from each object in list of objects.

```js
_.pluck('a', [{a:1}, {a: 2}, {a: 3}])
// => [1, 2, 3]
```

### prop :: String -> Object -> a
Returns the value of the specified property from the given object.

```js

_.prop('msg', {msg: 'hello wold'})
// => 'hello world'

```
### props :: [String] -> Object -> [a | void 0]
Returns a list of values related to the specified properties.

```js
_.props(['a', 'b', 'c'], {a:1 b: 2, c: 3})
// => [1, 2, 3]
```

### range :: Int -> Int -> [Int]
Returns a list of all integers between the two specified numbers.
The last number is not included.

```js
_.range(3, 7)
//=> [3, 4, 5, 6]
```

### reduce :: (a -> b -> a) -> a -> [b] -> a
Returns a single item by iterating through the list, successively calling the iterator function and passing it an accumulator value and the current value from the array, and then passing the result to the next call.
The iterator function receives two values: (acc, value).

```js
_.reduce((a, b) => a + b, 10, [1, 2, 3])
//=> 16
```

### reduceRight :: (a -> b -> a) -> a -> [b] -> a
Returns a single item by iterating through the list, successively calling the iterator function and passing it an accumulator value and the current value from the array, and then passing the result to the next call.

Similar to reduce, except moves through the input list from the right to the left.

The iterator function receives two values: (acc, value)

```js
_.reduceRight((acc, value) => acc.concat(value), [], [1, 2, 3, 4, 5])
// => [5, 4, 3, 2, 1]
```


### replace :: RegExp | String -> String -> String
Returns a new string by replacing each substring or regex match with a replacement.
When given a string as the first argument, only the first occurance of the substring will be replaced.

```js
_.replace(/\d/g, 5, '1, 2, 3, 4')
// => '5, 5, 5, 5'
```
### reverse :: [a] -> [a]
Returns a new list with the same elements in reverse order.

```js
_.reverse([1, 2, 3])
// => [3, 2, 1]
```

### slice :: Int -> Int -> [a] -> [a]
Returns a new list containing only the elements from the first index (inclusive) to the last index (exclusive).

```js
_.slice(1, 4, [1, 2, 3, 4, 5])
// => [2, 3, 4]
```


### some :: (a -> Boolean) -> [a] -> Boolean
Returns `true` if any of the elements in the list satisfy the predicate function.

```js
_.some(n => n > 2, [0, 1, 2, 3])
// => true
_.some(n => n > 2, [0, 1, 2])
// => false
```

### sort :: [a] -> [a]
Returns a lexigraphically sorted copy of the given list.

```js
_.sort(['z', 'a', 'h', 'd'])
// => ['a', 'd', 'h', 'z']
```

### sortBy :: (a -> Int) -> [a] -> [a]
Returns a sorted copy of the given list based on the sorting function.

```js
_.sortBy((a, b) => a - b, [2, 3, 1, 4])
// => [1, 2, 3, 4]
```

### split :: String | RegExp -> String -> [String]
Returns a list of strings based on the given separator.

```js
_.split(/\d/, "I have 3 apples and 2 oranges")
//=> ["I have ", " apples and ", " oranges"]
```

### sum :: [Number] -> Number
Returns the sum of all numbers in the list.

```js
_.sum([1, 2, 3])
// => 6
```

### tail :: [a] -> [a]
Returns a copy of the list without the first element.
```js
_.tail([1, 2, 3])
//=> [2, 3]
```

### take :: Int -> [a] -> [a]
Returns the first `n` elements of a list.

```js
_.take(2, [1, 2, 3, 4])
// => [1, 2]
```

### test :: RegExp -> String -> Boolean
Returns `true` if the RegExp matches the string; `false` otherwise.

```js
_.test(/\d/, 'I have 3 Apples')
// => true
_.test(/\d/, 'I have three Apples')
// => false
```

### toLowerCase :: String -> String
Returns a new string where all letters are lower case.

```js
_.toLowerCase('I have a dream')
// => 'i have a dream'
```

### toMaybe :: a | null | void 0 -> Maybe a
Returns a Nothing if the supplied value is null; A Just otherwise.

```js
_.toMaybe(1)
// => Just(1)
_.toMaybe(null)
// => Nothing
```

### toUpperCase :: String -> String
Returns a new string where all letters are upper case.

```js
_.toLowerCase('I have a dream')
// => 'I HAVE A DREAM'
```

### trim :: String -> String
Returns a new string where all leading and trailing spaces have been removed.

```js
_.trim(' I have a dream   ')
// => 'I have a dream'
```

### values :: Object -> [a]
Returns a list of all values in the given object.

```js
_.values({x: 1, y: 2, x: 3})
// => [1, 2, 3]
```

## 2.2Kb is too big!
basic.js is just 1.1KB. It contains all the native es5/es6 functions as well as curry and compose.

```js
const _ = require('barely-functional/basic')
```
