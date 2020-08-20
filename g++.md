# How to install g++ via homebrew

## terminal commands

### install gcc via brew
- ```% brew install gcc```
### check version of gcc
- ```% g++ -v```
  - この時点ではXcodeでインストールされたClangのversionが表示される

### then check the install folder
- ```% ls /usr/local/bin | grep gcc```  
```% ls /usr/local/bin | grep g++```
  - homebrewでインストールしたgccのversionがわかる

### make symbolic links
- ```% ln -s /usr/local/bin/gcc-10 /usr/local/bin/gcc```
- ```% ln -s /usr/local/bin/g++-10 /usr/local/bin/g++```

### change $PATH priority
- ```% export PATH=$PATH:/usr/local/bin```
  - now gcc via homebrew is called prior to Clang