# [Professor Frisby's Mostly Adequate Guide to Functional Programming](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/content)

Exercises

## [Chapter 04: Currying](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/content/ch04.html#exercises)

### task 1

> Refactor to remove all arguments by partially applying the function.

```js
// words :: String -> [String]
const words = str => split(' ', str);
```

#### solution

```js
// words :: String -> [String]
const words = split(' ');
```

## task 2

> Refactor to remove all arguments by partially applying the functions.

```js
// filterQs :: [String] -> [String]
const filterQs = xs => filter(x => x.match(/q/i), xs);
```

#### solution

```js
// filterQs :: [String] -> [String]
const filterQs = filter(match(/q/i));
```

## task 3

> Considering the following function:

```js
const keepHighest = (x, y) => (x >= y ? x : y);
```

> Refactor `max` to not reference any arguments using the helper function `keepHighest`.

```js
// max :: [Number] -> Number
const max = xs => reduce((acc, x) => (x >= acc ? x : acc), -Infinity, xs);
```

#### solution

```js
// max :: [Number] -> Number
const max = reduce(keepHighest, -Infinity);
```
