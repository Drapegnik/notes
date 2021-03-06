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

## [Chapter 10: Applicative Functors](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/ch10.html)

### task 1

> Write a function that adds two possibly null numbers together using `Maybe` and `ap`.

#### solution

```js
// safeAdd :: Maybe Number -> Maybe Number -> Maybe Number
const safeAdd = curry((a, b) =>
  Maybe.of(add)
    .ap(a)
    .ap(b)
);
```

### task 2

> Rewrite `safeAdd` from task 1 to use `liftA2` instead of `ap`.

#### solution

```js
// safeAdd :: Maybe Number -> Maybe Number -> Maybe Number
const safeAdd = liftA2(add);
```

### task 3

> For the next exercise, we consider the following helpers:

```js
const localStorage = {
  player1: { id: 1, name: "Albert" },
  player2: { id: 2, name: "Theresa" }
};

// getFromCache :: String -> IO User
const getFromCache = x => new IO(() => localStorage[x]);

// game :: User -> User -> String
const game = curry((p1, p2) => `${p1.name} vs ${p2.name}`);
```

> Write an IO that gets both player1 and player2 from the cache and starts the game.

#### solution

```js
// startGame :: IO String
const startGame = liftA2(
  game,
  getFromCache("player1"),
  getFromCache("player2")
);
```

## [Chapter 11: Transform Again, Naturally](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/ch11.html)

### task 1

> Write a natural transformation that converts `Either b a` to `Maybe a`

```js
// eitherToMaybe :: Either b a -> Maybe a
const eitherToMaybe = undefined;
```

#### solution

```js
// eitherToMaybe :: Either b a -> Maybe a
const eitherToMaybe = either(nothing, Maybe.of);
```

### task 2

```js
// eitherToTask :: Either a b -> Task a b
const eitherToTask = either(Task.rejected, Task.of);
```

> Using `eitherToTask`, simplify `findNameById` to remove the nested `Either`.

```js
// findNameById :: Number -> Task Error (Either Error User)
const findNameById = compose(
  map(map(prop("name"))),
  findUserById
);
```

#### solution

```js
const findNameById = compose(
  map(prop("name")),
  chain(eitherToTask),
  findUserById
);
```

### task 3

```
split :: String -> String -> [String]
intercalate :: String -> [String] -> String
```

> Write the isomorphisms between String and [Char].

```js
// strToList :: String -> [Char]
const strToList = undefined;

// listToStr :: [Char] -> String
const listToStr = undefined;
```

#### solution

```js
// strToList :: String -> [Char]
const strToList = split("");

// listToStr :: [Char] -> String
const listToStr = intercalate("");
```

## [Chapter 12: Traversing the Stone](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/ch12.html)

### task 1

> Considering the following elements:

```js
// httpGet :: Route -> Task Error JSON

// routes :: Map Route Route
const routes = new Map({ "/": "/", "/about": "/about" });
```

> Use the traversable interface to change the type signature of `getJsons` to Map Route Route → Task Error (Map Route JSON)

```js
// getJsons :: Map Route Route -> Map Route (Task Error JSON)
const getJsons = map(httpGet);
```

#### solution

```js
// getJsons :: Map Route Route -> Task Error (Map Route JSON)
const getJsons = traverse(Task.of, httpGet);
```

### task 3

> We now define the following validation function:

```js
// validate :: Player -> Either String Player
const validate = player =>
  player.name ? Either.of(player) : left("must have name");
```

> Using traversable, and the `validate` function, update `startGame` (and its signature) to only start the game if all players are valid

```js
// startGame :: [Player] -> [Either Error String]
const startGame = compose(
  map(always("game started!")),
  map(validate)
);
```

#### solution

```js
// startGame :: [Player] -> Either Error String
const startGame = compose(
  map(always("game started!")),
  traverse(Either.of, validate)
);
```

### task 3

> Finally, we consider some file-system helpers:

```
// readfile :: String -> Task Error String
// readdir :: String -> Task Error [String]
```

> Use traversable to rearrange and flatten the nested Tasks & Maybe

```js
// readFirst :: String -> Task Error (Task Error (Maybe String))
const readFirst = compose(
  map(map(readfile("utf-8"))),
  map(safeHead),
  readdir
);
```

#### solution

```js
// readFirst :: String -> Task Error (Maybe String)
const readFirst = compose(
  chain(traverse(Task.of, readfile("utf-8"))),
  map(safeHead),
  readdir
);
```
