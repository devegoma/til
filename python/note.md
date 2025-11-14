# 自作例外
Exceptionクラスを継承した例外クラスを作成
```python
class MyException(Exception):
	pass
```

例外オブジェクトを出力したときに表示されるメッセージを例外クラスで指定することもできる
```python
class MyException(Exception):
	def __str__(self):
		return "エラー:MyException"
```

# セイウチ演算子(:=)
条件文内で代入ができないときに使う
```python
if (id := 1) == 1 
	print("これがセイウチ演算子")
```
可読性が下がるのであまり使わない
