#PyPy3 and Online judge tools

##basics

```bash
# install pypy3 using homebrew
$ brew install pypy3

# install online-judge-tools using pypy3 pip
$ pypy3 -m pip install online-judge-tools

# set PATH to use ojt
$ open ~/.zshrc
export PATH="$PATH:/usr/local/share/pypy3"

#install GNU time to show memory assumption
$ brew install gnu-time
$ open ~/.zshrc
export PATH="/usr/local/opt/gnu-time/libexec/gnubin:$PATH"

# login to atcoder
$ oj login -u {username} -p {password} "https://atcoder.jp/"

# login check command;
$ oj login --check "https://atcoder.jp/"

# download testcases;
$ oj dl -d {directory} {url}

# testing;
$ oj test -c "{command}" -d {directory}

# test using manual input;
$ pypy3 {source_filename} < {input_filename}
```

## advance

automating;
`touch input.txt`
`touch task.json`
`touch cptest.sh`

### shell tips

|arg|value|
|--|--|
|`$0`|script name|
|`$1-$9`|each arg with index|
|`$#`|args count|
|`$??`|and so on|



```sh
#!/bin/zsh

problem_name=$1
test_dir=test/${problem_name}
base_url=${problem_name%_*}

# make test directory
if [ ! -e ${test_dir} ]; then
    oj dl -d test/${problem_name} https://atcoder.jp/contests/${base_url}/tasks/${problem_name//-/_}
fi

if [ $# = 2 ];then
    script_name=$2
    oj test -c "pypy3 ${script_name}.py" -d test/${problem_name//-/_}
else
    oj test -c "pypy3 ${problem_name}.py" -d test/${problem_name//-/_}
fi
```

### chmod is needed
```
> Executing task: /Users/thanai/git/online-judge/atcoder-python/cptest.sh abc157_b <

zsh:1: permission denied: /Users/thanai/git/online-judge/atcoder-python/cptest.sh
ターミナル プロセス "/bin/zsh '-c', '/Users/thanai/git/online-judge/atcoder-python/cptest.sh abc157_b'" が起動に失敗しました (終了コード: 126)。

ターミナルはタスクで再利用されます、閉じるには任意のキーを押してください。
```

You can avoid this problem as below;
```bash
# 実行権限の付与
$ chmod +x cptest.sh
```

### 引数の意味
|記号|	意味|
|-|-|
|u|	所有者の権限|
|g|	グループの権限|
|o|	その他のユーザーの権限|
|a|	すべての権限|
|+|	後に記述した権限を付加する|
|-|	後に記述した権限を削除する|
|=|	後に記述した権限にする|
|r|	読み込み権限|
|w|	書き込み権限|
|x|	実行権限|
|s|	セットID|
|t|	スティッキ・ビット|


```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "test_atcoder_sample",
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "type": "shell",
            "command": "${workspaceFolder}/cptest.sh",
            "args": [
                "${fileBasenameNoExtension}"
                ]
        }
    ]
}
```
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "PyPy3: Current File",
            "type": "pypy3",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "args": [
                "<",
                "input.txt"
            ]
        }
    ]
}
```
