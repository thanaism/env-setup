# Git commands

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
