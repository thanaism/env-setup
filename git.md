# Git commands

## グローバル設定について

グローバル設定の確認

```bash
$ git config --list
```

設定の削除

```bash
$ git config --global --unset {key}

#example
$ git config --list
init.templatedir=~/.git_template

$ git config --global --unset init.templatedir
```

設定の追加

```bash
$ git config --global {key} {value}
$ git config --global init.templatedir ~/.git_template
```

## rename a branch

```
% git branch -m $BEFORE $AFTER
```

if you on `$BEFORE` branch;

```
% git branch -m $AFTER
```

## delete a branch

Remote;

```
% git push origin :$BRANCH
```

Local;

```
% git branch -D $BRANCH
```

## stash

### push a stash

```
% git stash
```

### list stashes

```
% git stash list
```

### delete a stash

```
% git stash drop @{$INDEX}
```

### pop a stash

```
% git stash pop @{$INDEX}
```

## git checkout -

you can switch branch to previous one.
