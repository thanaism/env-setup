# GitHub CLI installaion

## How to install
You can install GitHub-CLI via homebrew
```
% brew install github/hub/hub
```
Firstly, you need authentification.
```
% gh issue list
Notice: authentication required
Press Enter to open github.com in your browser...
```
Do something on your browser. After that;
```
Authentication complete. Press Enter to continue... 
no git remotes found
```
To validate command completion, you can add script below to `.zshrc`
```
eval "$(gh completion -s zsh)"
```

Now you can use GitHub on your shell !

## How to use
### Make a repository
```
gh repo create $REPO_NAME
```
If you want make a public repository, you can use option `--public`

### Make a pull request
```
gh pr create
```
