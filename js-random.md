# JS random notes

## Interview

* [@vvscode/js--interview-questions](https://github.com/vvscode/js--interview-questions)
* [telegram/@callforward](https://t.me/callforward)

## Commands

* check dead code with webpack [#webpack #bundle #stats #analyze]:

```bash
npx webpack --json | npx webpack-unused -s src
```

* measure bundle size [#webpack #bundle #size]:

```bash
npx bundlesize
```

```bash
npx size-limit
```

* why this module in a bundle? [#webpack #bundle #stats #analyze]:

```bash
npx whybundled stats.json
npx whybundled stats.json --by styled-components
```

## Api

* [`Intl`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl) - [#i18n #intl #native]
