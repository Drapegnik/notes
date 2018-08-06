# return true to win

[alf.nu/ReturnTrue](https://alf.nu/ReturnTrue)

## season 1

### id

```js
function id(x) {
  return x;
}
```

```js
▶ id(!0)
▷ true  // 2 char
```

### reflexive

```js
function reflexive(x) {
  return x != x;
}
```

```js
▶ reflexive(NaN)
▷ true  // 3 chars
```

### infinity

```js
function infinity(x, y) {
  return x === y && 1 / x < 1 / y;
}
```

```js
▶ infinity(-0,0)
▷ true  // 4 chars
```

### transitive

```js
function transitive(x, y, z) {
  return x && x == y && y == z && x != z;
}
```

```js
▶ transitive([],0,[])
▷ true  // 7 chars
```

### counter

```js
function counter(f) {
  var a = f(),
    b = f();
  return a() == 1 && a() == 2 && a() == 3 && b() == 1 && b() == 2;
}
```

```js
▶ counter((c=1)=>a=>c++)
▷ true // 13 chars
```

### peano

```js
function peano(x) {
  return x++ !== x && x++ === x;
}
```

```js
▶ peano(9007199254740991)
▷ true  // 16 chars
```

### array

```js
function array(x, y) {
  return Array.isArray(x) && !(x instanceof Array) && !Array.isArray(y) && y instanceof Array;
}
```

```js
▶ array([].__proto__,{__proto__:[]})
▷ true // 27 chars
```
