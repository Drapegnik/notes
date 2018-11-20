# JS scripts

- visualize source complexity with [plato](https://github.com/es-analysis/plato):

```bash
npx plato -d report -r -e .eslintrc src
```

- summarise all code annotation like TODO or FIXME with [code-notes](https://github.com/ahmadassaf/code-notes)

```bash
npx code-notes --git-ignore .gitignore
```

- `npm` check for updates

```bash
npx npm-check-updates '/storybook/' -u && npm install
```

- check dead code with webpack [#webpack #bundle #stats #analyze]:

```bash
npx webpack --json | npx webpack-unused -s src | grep '.js'
```

- measure bundle size [#webpack #bundle #size]:

```bash
npx bundlesize
```

```bash
npx size-limit
```

- why this module in a bundle? [#webpack #bundle #stats #analyze]:

```bash
npx whybundled stats.json
npx whybundled stats.json --by styled-components
```
