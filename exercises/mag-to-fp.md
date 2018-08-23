# [Professor Frisby's Mostly Adequate Guide to Functional Programming](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/content)

Exercises

## [Chapter 04: Currying](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/content/ch04.html#exercises)

### task 1

> Refactor to remove all arguments by partially applying the function.

```js
// words :: String -> [String]
const words = str => split(" ", str);
```

#### solution

```js
// words :: String -> [String]
const words = split(" ");
```

### task 2

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

### task 3

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

## [Chapter 05: Coding by Composing](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/content/ch05.html)

> In each following exercise, we'll consider Car objects with the following shape:

```js
{
  name: 'Aston Martin One-77',
  horsepower: 750,
  dollar_value: 1850000,
  in_stock: true,
}
```

### task 1

> Use `compose()` to rewrite the function below.

```js
// isLastInStock :: [Car] -> Boolean
const isLastInStock = cars => {
  const lastCar = last(cars);
  return prop("in_stock", lastCar);
};
```

#### solution

```js
// isLastInStock :: [Car] -> Boolean
const isLastInStock = compose(
  prop("in_stock"),
  last
);
```

### task 2

> Considering the following function:

```js
const average = xs => reduce(add, 0, xs) / xs.length;
```

> Use the helper function `average` to refactor `averageDollarValue` as a composition.

```js
// averageDollarValue :: [Car] -> Int
const averageDollarValue = cars => {
  const dollarValues = map(c => c.dollar_value, cars);
  return average(dollarValues);
};
```

#### solution

```js
// averageDollarValue :: [Car] -> Int
const averageDollarValue = compose(
  average,
  map(prop("dollar_value"))
);
```

### task 3

> Refactor `fastestCar` using `compose()` and other functions in pointfree-style. Hint, the `flip` function may come in handy.

```js
// fastestCar :: [Car] -> String
const fastestCar = cars => {
  const sorted = sortBy(car => car.horsepower, cars);
  const fastest = last(sorted);
  return concat(fastest.name, " is the fastest");
};
```

#### solution

```js
// fastestCar :: [Car] -> String
const fastestCar = compose(
  flip(concat, " is the fastest"),
  prop("name"),
  last,
  sortBy(prop("horsepower"))
);
```

## [Chapter 08: Tupperware](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/content/ch08.html)

### task 1

> Use `add` and `map` to make a function that increments a value inside a functor.

#### solution

```js
// incrF :: Functor f => f Int -> f Int
const incrF = map(add(1));
```

### task 2

> Given the following User object:

```js
const user = { id: 2, name: "Albert", active: true };
```

> Use `safeProp` and `head` to find the first initial of the user.

#### solution

```js
// initial :: User -> Maybe String
const initial = compose(
  map(head),
  safeProp("name")
);
```

### task 3

> Given the following helper functions:

```js
// showWelcome :: User -> String
const showWelcome = compose(
  concat("Welcome "),
  prop("name")
);

// checkActive :: User -> Either String User
const checkActive = function checkActive(user) {
  return user.active ? Either.of(user) : left("Your account is not active");
};
```

> Write a function that uses `checkActive` and `showWelcome` to grant access or return the error.

#### solution

```js
// eitherWelcome :: User -> Either String String
const eitherWelcome = compose(
  map(showWelcome),
  checkActive
);
```

## [Chapter 09: Monadic Onions](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/ch09.html)

### task 1

> Considering a User object as follow:

```js
const user = {
  id: 1,
  name: "Albert",
  address: {
    street: {
      number: 22,
      name: "Walnut St"
    }
  }
};
```

> Use `safeProp` and `map/join` or `chain` to safely get the street name when given a user

#### solution

```js
// getStreetName :: User -> Maybe String
const getStreetName = compose(
  chain(safeProp("name")),
  chain(safeProp("street")),
  safeProp("address")
);
```

### task 2

> We now consider the following functions

```js
// getFile :: () -> IO String
const getFile = () => IO.of("/home/mostly-adequate/ch9.md");

// pureLog :: String -> IO ()
const pureLog = str => new IO(() => console.log(str));
```

> Use `getFile` to get the filepath, remove the directory and keep only the basename, then purely log it. Hint: you may want to use `split` and `last` to obtain the basename from a filepath.

#### solution

```js
// getBasename :: String -> String
const getBasename = compose(
  last,
  split("/")
);
// logFilename :: IO ()
const logFilename = compose(
  chain(pureLog),
  map(getBasename),
  getFile
);
```

### task 3

> For this exercise, we consider helpers with the following signatures:

```js
// validateEmail :: Email -> Either String Email

// addToMailingList :: Email -> IO([Email])

// emailBlast :: [Email] -> IO ()
```

> Use `validateEmail`, `addToMailingList` and `emailBlast` to create a function which adds a new email to the mailing list if valid, and then notify the whole list.

#### solution

```js
// joinMailingList :: Email -> Either String (IO ())
const joinMailingList = compose(
  map(
    compose(
      chain(emailBlast),
      addToMailingList
    )
  ),
  validateEmail
);
```

<!-- ## []()

### task

>

```js
```

#### solution

```js
``` -->
