# Git notes

* remove tags [#git #tags #script]:

```bash
git push --delete origin v2.5.4
git tag -d v2.5.4
```

* get number of commits per author [#git #script]:

```bash
git shortlog -sne --no-merges
```
