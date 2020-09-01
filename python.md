# Python 仕様メモ

## 組み込み関数

名前空間からキーを拾ってきてくれる組み込み関数 vars()があるので使う

```python
>>> print(list(name for name in vars(__builtins__)
... if (name[0].islower() and name not in ('quit', 'exit', 'copyright', 'credits', 'license',)) or name=='__import__'))
['bool', 'bytearray', 'bytes', 'complex', 'dict', 'float', 'int', 'sequenceiterator', 'list', 'memoryview', 'object', 'set', 'frozenset', 'slice', 'tuple', 'type', 'str', '__import__', 'super', 'isinstance', 'len', 'sorted', 'enumerate', 'classmethod', 'hasattr', 'setattr', 'getattr', 'property', 'staticmethod', 'any', 'all', 'iter', 'open', 'abs', 'ascii', 'chr', 'ord', 'pow', 'repr', 'hash', 'round', 'divmod', 'format', 'issubclass', 'delattr', 'next', 'id', 'callable', 'compile', 'eval', 'exec', 'range', 'map', 'filter', 'zip', 'min', 'max', 'reversed', 'globals', 'locals', 'input', 'print', 'sum', 'vars', 'dir', 'bin', 'oct', 'hex', 'help']
```
