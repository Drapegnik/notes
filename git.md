# Git notes

- [git-change-date](https://github.com/bitriddler/git-change-date) - cli to change old commits author and committer dates - [#git #cli #rebase]

- clean up branches that have been merged into master:

```bash
# for remote
git-sweep cleanup
# for local
git remote add local $(pwd)
git-sweep cleanup --origin=local
```

- remove tags [#git #tags #script]:

```bash
git push --delete origin v2.5.4
git tag -d v2.5.4
```

- get number of commits per author [#git #script #count]:

```bash
git shortlog -sne --no-merges
```

- get number of files in repo [#git #script #count]:

```bash
git ls-files | wc -l
```

- get number of lines in repo [#git #script #count]:

```bash
git ls-files | xargs cat | wc -l
git ls-files | xargs wc -l # detailed
```

- work with `stash` [#git #stash]:

```bash
git stash show -p stash@{2}
git stash apply stash@{2}
git stash drop stash@{2}
```

- patch from uncommited changes [#git #patch]:

```bash
git diff --cached > mypatch.patch # --cached for staged changes
```

- revert modified file [#git #revert]:

```bash
git checkout -- index.js
```
