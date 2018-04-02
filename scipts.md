# useful scripts

* find unused files:

```bash
npx webpack --json | npx webpack-unused -s src
```

* checkout `env` vars:

```bash
printenv | grep PATH
```
