# Python 仕様メモ

## 組み込み関数

名前空間からキーを拾ってきてくれる組み込み関数 vars()があるので使う

```python
>>> print(list(name for name in vars(__builtins__)
... if (name[0].islower() and name not in ('quit', 'exit', 'copyright', 'credits', 'license',)) or name=='__import__'))
['bool', 'bytearray', 'bytes', 'complex', 'dict', 'float', 'int', 'sequenceiterator', 'list', 'memoryview', 'object', 'set', 'frozenset', 'slice', 'tuple', 'type', 'str', '__import__', 'super', 'isinstance', 'len', 'sorted', 'enumerate', 'classmethod', 'hasattr', 'setattr', 'getattr', 'property', 'staticmethod', 'any', 'all', 'iter', 'open', 'abs', 'ascii', 'chr', 'ord', 'pow', 'repr', 'hash', 'round', 'divmod', 'format', 'issubclass', 'delattr', 'next', 'id', 'callable', 'compile', 'eval', 'exec', 'range', 'map', 'filter', 'zip', 'min', 'max', 'reversed', 'globals', 'locals', 'input', 'print', 'sum', 'vars', 'dir', 'bin', 'oct', 'hex', 'help']
```

You'll also need to know about pyenv's PYTHON_CONFIGURE_OPTS, --with-tcltk-includes, and --with-tcltk-libs, e.g. from this comment.

Next, reinstall Python with the environment variables active:

※ pyenv uninstall 3.8.1
※ env \
 PATH="$(brew --prefix tcl-tk)/bin:$PATH" \
 LDFLAGS="-L$(brew --prefix tcl-tk)/lib" \
  CPPFLAGS="-I$(brew --prefix tcl-tk)/include" \
 PKG_CONFIG_PATH="$(brew --prefix tcl-tk)/lib/pkgconfig" \
  CFLAGS="-I$(brew --prefix tcl-tk)/include" \
 PYTHON_CONFIGURE_OPTS="--with-tcltk-includes='-I$(brew --prefix tcl-tk)/include' --with-tcltk-libs='-L$(brew --prefix tcl-tk)/lib -ltcl8.6 -ltk8.6'" \
 pyenv install 3.8.1
It should work now:

※ pyenv global 3.8.1
※ python
Python 3.8.1 (default, Feb 29 2020, 11:56:10)
[Clang 11.0.0 (clang-1100.0.33.17)] on darwin
Type "help", "copyright", "credits" or "license" for more information.

> > > import tkinter
> > > tkinter.TclVersion, tkinter.TkVersion
> > > (8.6, 8.6)
> > > tkinter.\_test()

# You should get a GUI

If you get the following error, you might be missing the PYTHON_CONFIGURE_OPTS env. var. above.

DEPRECATION WARNING: The system version of Tk is deprecated and may be removed in a future release. Please don't rely on it. Set TK_SILENCE_DEPRECATION=1 to suppress this warning.
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
File "/Users/factor/.pyenv/versions/3.8.1/lib/python3.8/tkinter/**init**.py", line 4552, in \_test
root = Tk()
File "/Users/factor/.pyenv/versions/3.8.1/lib/python3.8/tkinter/**init**.py", line 2263, in **init**
self.\_loadtk()
File "/Users/factor/.pyenv/versions/3.8.1/lib/python3.8/tkinter/**init**.py", line 2279, in \_loadtk
raise RuntimeError("tk.h version (%s) doesn't match libtk.a version (%s)"
RuntimeError: tk.h version (8.6) doesn't match libtk.a version (8.5)
