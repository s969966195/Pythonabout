1. 下面这段代码的输出结果是什么？请解释。

```
def extendList(val, list=[]):
	list.append(val)
	return list
	
list1 = extendList(10)
list2 = extendList(123, [])
list3 = extendList('a')

print "list1 = %s" % list1
print "list2 = %s" % list2
print "list3 = %s" % list3
```

结果：

```
list1 = [10, 'a']
list2 = [123]
list3 = [10, 'a']
```

新的默认列表只在函数被定义的那一刻创建一次。

当extendList被没有指定特定参数list调用时，这组list的值随后将被使用。这是因为带有默认参数的表达式在函数被定义的时候被计算，不是在调用的时候被计算。因此list1和list3是在同一个默认列表上进行操作（计算）的，而list2是在一个分离的列表上进行操作（计算）的。（通过传递一个自由的空列表作为列表参数的数值）

修改：

```
def extendList(val, list=None):
	if list is None:
		list = []
	list.append(val)
	return list
```

结果：

```
list1 = [10]
list2 = [123]
list3 = ['a']
```
2. 下面这段代码的输出结果将是什么？请解释。

```
def multipliers():
	return [lambda x:i*x for i in range(4)]
	
print [m(2) for m in multipliers()]
```

结果：

```
[6, 6, 6, 6]
```

上述问题产生的原因是Python闭包的延迟绑定。这意味着内部函数被调用时，参数的值在闭包内进行查找。因此，当任何由multipliers()返回的函数被调用时，i的值将在附近的范围进行查找。那时，不管返回的函数是否被调用，for循环已经完成，i被赋予了最终的值3。
