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
#!/bin/bash

problem_name=$1
test_dir=test/${problem_name}
base_url=${problem_name%_*}

# make test directory
if [ ! -e ${test_dir} ]; then
    oj dl -d test/${problem_name} https://atcoder.jp/contests/${base_url}/tasks/${problem_name//-/_}
fi

oj test -c "python3 problems/${problem_name}.py" -d test/${problem_name}
```


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
