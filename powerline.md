# Powerline instrallation

## install Powerline via pip3
It requires python3 and pip3 and homebrew.

```
% pip3 install powerline-status
```

Then, add below to .zshrc

```
. {!--path powerline installed--!}/powerline/bindings/zsh/powerline.zsh
```

You can know the Powerline directory path via: `pip3 show powerline-status`.
In my case, the full text was as below;

```
. /usr/local/lib/python3.8/site-packages/powerline/bindings/zsh/powerline.zsh
```

Let's edit and save `.zshrc` !

```
% open ~/.zshrc
# edit and save
% source ~/.zshrc
```

## install Powerline font via brew
```
% brew tap sanemat/font
% brew install ricty --with-powerline
# tap: git clone via brew !--maybe.
```

In the end of installation, below cmd will be recommend.

```
% cp -f /usr/local/opt/ricty/share/fonts/Ricty*.ttf ~/Library/Fonts/
% fc-cache -vf
```

## set powerline font
You can set powerline font to your terminal in the system GUI config.
After configuration, reboot the shell.