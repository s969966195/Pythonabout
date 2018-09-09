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

解决一：
Python生成器

```
def multipliers():
	for i in range(4):
		yield lambda x : i * x
```

解决二：
闭包

```
def multipliers():
	return [lambda x, i = i : i * x for i in range(4)]
```

解决三：
偏函数

```
from functools import partial
from operator import mul
def multipliers():
	return [partial(mul, i) for i in range(4)]
```

## Python语言的发展
1. 动态语言，得益于互联网和计算机性能的提高
2. Python2.4，2.5，2.6缺陷主要是语法糖不够丰富
3. Python2有些没考虑清楚的地方，要做一个不兼容的版本
4. 15年很多库还不支持3；迁移动力不足，成本高，3的收益不明显

## Python的优势和劣势
**优势**

1. 语法简介，丰富的语法糖
2. 易学
3. 开发效率高，能更快地完成需求
4. 隐藏底层细节，比如内存管理部分开发者不需要关注，有更多的时间进行业务开发
5. 丰富的API、第三方库
6. 胶水语言。用来连接软件组件的程序设计语言。机器学习、深度学习很多库底层还是C++，但用Python表达起来是很方便的。
7. 各个领域都有建树
8. 缩进让代码格式规范
9. 可扩展性。关键部分可以用C++、Rust改写

**劣势**

1. 运行速度慢。比Java慢3-5倍，但硬件资源比开发人员便宜
2. **GIL(全局解释器锁)** CPython无法实现真正的并发。为什么？因为CPython内存管理不是线程安全的，需要GIL保证多个原生线程不会并发的执行Python的字节码。在单线程更快，不用考虑线程安全的问题。
3. 国内市场小。语言通常不是技术瓶颈。政治层面，BAT大佬去小公司会重构自己熟悉的技术栈，这些公司会成为细分领域比较靠前的。招聘的成本

## Python主要的企业应用场景
1. Web开发<br />
	Web框架：Flask, Django, Tornado, Pyramid, aiohttp, sanic
2. 爬虫开发<br />
	数据采集、舆情监测、API数据的抓取
3. 运维开发<br />
	DEVOPS 自动化平台，资源监控、上下线任务调度，持续集成、数据库开发、应用引擎
4. 数据分析
5. 图形GUI开发（TKinter、wxPython、PyQt）
6. 游戏开发

## Python2 or Python3?
**现状**

* 如今大部分生产环境使用Python2.7
* Python 3已准备好用于生产应用的部署
* Python2.7 直到2020前只会得到必要的安全更新
* "Python"涵盖Python3和Python2
* 越来越多的项目放弃对Python2的支持

Python3已达到商用级别，但是迁移是非常大的工程，收益并不大，造成生产环境推进缓慢。一是历史遗留，大部分公司都是KPI导向，升级到3的收益不大，软件的稳定性很重要，商业应用更新周期慢。二是基础设施是Python2

#### Python3是未来

**Python2和Python3的区别**

1. 统一了字符编码支持。
2. 增加了新的语法。print/exec等成为了函数，格式化字符串变量，类型标注，添加了nonlocal、yield from、async/await、yield for关键词和\__annotations\__、\__context\__、\__traceback\__、\__qualname\__等dunder方法。
3. 修改了一些语法。元类metaclass，raise、map、filter以及dict的items/keys/values方法返回迭代对象而不是列表，描述符协议，保存类属性定义顺序，保存关键字参数顺序
4. 去掉了一些语法。cmp、<>(也就是!=)、xrange（其实就是range）、不再有经典类
5. 增加了一些模块。concurrent.futures、venv、unittest.mock、asyncio、selectors、typing等
6. 修改了一些模块。主要是对模块添加函数/类/方法（如functools.lru_cache、threading.Barrier）或者参数。
7. 模块的改名。把一些相关的模块放进同一个包里面（如httplib, BaseHTTPServer, CGIHTTPServer, SimpleHTTPServer, Cookie, cookielib放进了http里面，urllib, urllib2, urlparse, robotparse放进了urllib里面），个例如SocketServer改成了socketserver，Queue改成queue等
8. 去掉了一些模块或者函数。gopherlib、md5、contextlib.nested、inspect.getmoduleinfo等。去掉的内容的原因主要是2点：1. 过时的技术产物，已经没什么人在用了；2. 出现了新的替代产物后者被证明存在意义不大。理论上对于开发者影响很小
9. 优化。重新实现了dict可以减少20%-25%的内存使用；提升pickle序列化和反序列化的效率；collections.OrderedDict改用C实现；通过os.scandir对glob模块中的glob()及iglob()进行优化，使得它们现在大概快了3-6倍等.. 这些都是喜大普奔的好消息，同样开发者不需要感知，默默的就会让结果变得更好。
10. 其他。构建过程，安全性，C的API

> Python 2/3的思想基本是共通的，只有少量的语法有差别甚至不兼容。当对Python熟悉到
> 一定程度时， 即使只会Python 2也可以在很短的时间就能写Python 3的代码。

## 学习经历和经验
《编程人生》
##### 自学Python
二八原则：工作中80%的内容用的是20%语言常用的知识
##### 使用Python工作
##### 代码评审和贡献代码
##### 看更多的书和博客
1. [程序员技术练级攻略](https://coolshell.cn/articles/4990.html)
2. 课外书籍

* 《重构》
* 《代码大全》
* 《黑客与画家》
* 《代码整洁之道》
* 《程序员修炼之道》
* 《集体智慧编程》
* 《人月神话》

##### 读/贡献Python标准库 优秀开源项目代码
1. 《Python标准库》
2. [Python 3 Module of the Week](https://pymotw.com/3/)

## Python安装

##### Python环境搭建(安装或升级最新版本)
1. 使用系统自带的包管理工具 </br>
	二进制文件，不需要编译 </br>
	`sudo apt-get install python3.6` </br>
	软件更新版本不及时
	官网没有出包，可以使用PPA找个人用户有没有制作
	
	```
	sudo apt-get install software-properties-common
	sudo add-apt-repository ppa:XXX/ppa
	sudo apt-get update
	sudo apt-get install python3.7
	```
2. 官网找对应的平台安装包
3. 源码安装</br>
**优点**：</br>
(1). 普适</br>
(2). 可定制</br>
(3). 及时体验最新版本</br>
**缺点**：</br>
(1). 编译很慢</br>
(2). 没有对应平台做过优化和设置</br>
`./configure && make && sudo make install`

## 使用Python自带交互式环境

##### REPL(Read-Eval-Print Loop)
读入-求值-输出 循环</br>
`python`
##### IDLE(Python软件包自带的开发环境)
`sudo apt-get install idle3`

## 简单数据类型

##### 字符串
**In:** 

```
"""
This
is
a Test
"""
```
**Out:**

```
\nThis\nis\na Test\n
```

```
string.upper/lower() # 大写
string.endswith/startswith('c') # 是否以某个字符开头或结尾
string.split()
string.rsplit(None, 1) # 从右向左分隔 并且保留第一个分割项
”a b c“.rsplit(None, 1) => ['a b', 'c']
string.strip/lstrip/rstrip() # 也可以指定其他字符
string.replace('a', 'b')
'/a/b/c'.partition('/') => ('', '/', 'a/b/c') # 头部，分割符，尾部
string.rpartition() # 从右向左分区
','.join(['a', 'b'])
'thi{}'.format('s')

dir('') # 内置方法 

转义
'他说：\'你好\'' => "他说：'你好'"

f
name = "Fred"
f'I am {name}' => 'I am Fred'
import datetime
anniversary = datetime.date(1991, 10, 12)
f'It\'s {anniversary:%A, %B %d, %Y}' => "It's Saturday, October 12, 1991"
```

**整型** </br>
2 ** 3 = 8</br>
2 / 3 = 0.6666666666666666

**浮点数**</br>
1.1 - 0.2 = 0.900000000000000001</br>
1.1 * 0.2 = 0.22000000000000000000000003</br>
可以使用

```
from decimal import Decimal
Decimal['1.1'] * Decimal['0.2']
```

**类型转换**</br>
str(1) int('1') float(1)

**Python 2的除法**

```
2 / 3 => 0
3 / 2 => 1 整数除法的结果只保留了整数部分

float(3) / 2 => 1.5 让分子或分母成为浮点数
```

**布尔值**

[https://www.python.org/dev/peps/pep-3101](https://www.python.org/dev/peps/pep-3101)
[https://www.python.org/dev/peps/pep-0498/](https://www.python.org/dev/peps/pep-0498/)
PEP Python增强建议书
[https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)

## 变量和关键字

```
import keyword
', '.join(keyword.kwlist)
```

**延伸阅读**
[https://www.programiz.com/python-programming/keyword-list](https://www.programiz.com/python-programming/keyword-list)

## 列表
**数据结构**是计算机存储、组织数据的方式。数据结构是指相互之间存在一种或多种特定关系的数据元素的集合。

**序列(SequenceSequence)**</br>
序列是Python中最基本的数据结构，序列每个元素会被分配一个序号，也就是元素的位置，叫做索引。

内置序列类型

1. List 列表
2. Tuple 元组
3. Ranges range函数
4. Str 文本序列
5. Binary 二进制(bytes, bytearray, memoryview)
6. Set, frozenset 集合
7. Dict 字典

####列表
**append**

```
firends = []                                 
firends.append('David')
Out: ['David']
```

**extend** 一次性添加多个

```
firends.extend(['Chris', 'Amy'])
['David', 'Chris', 'Amy']
```

**列表分片**

```
In : firends
Out: ['David', 'Chris', 'Amy'] 
In : firends[2]                                         
Out: 'Amy'
In : firends[-1]                                        
Out: 'Amy'
In : firends[1:3]                                       
Out: ['Chris', 'Amy']
In : firends[1:]                                        
Out: ['Chris', 'Amy']
In : firends[:1]                                        
Out: ['David']  
```

**分片步长** 默认为1

```
In : [0, 1, 2, 3, 4, 5][:]                              
Out: [0, 1, 2, 3, 4, 5]                                 
In : [0, 1, 2, 3, 4, 5][0:6:1]                          
Out: [0, 1, 2, 3, 4, 5]                                 
In : [0, 1, 2, 3, 4, 5][0:6:2]                          
Out: [0, 2, 4]                                          
In : [0, 1, 2, 3, 4, 5][::-1]                           
Out: [5, 4, 3, 2, 1, 0]
```

**修改元素**

```
In : firends[0] = 'Andy'                                
In : firends
Out: ['Andy', 'Chris', 'Amy']                           
In : firends[1:3] = ['Sophia', 'Emma', 'Sarah']         
In : firends
Out: ['Andy', 'Sophia', 'Emma', 'Sarah'] 
```

**insert** 指定位置添加

```
In : firends.insert(1, 'Olivia')                        
In : firends
Out: ['Andy', 'Olivia', 'Sophia', 'Emma', 'Sarah']
```

**len**

```
In : l = [0, 1, 2, 3, 4, 5]                             
In : len(l)                                             
Out: 6
```

**删除元素**

```
In : firends = ['Andy', 'Olivia', 'Sophia', 'Sarah', 'Chris']   
In : del firends[0] # 明确知道索引In : firends
Out: ['Olivia', 'Sophia', 'Sarah', 'Chris'] 
In : firends.pop() # 从尾部去掉一个元素
Out: 'Chris'                                                    
In : firends
Out: ['Olivia', 'Sophia', 'Sarah'] 
In : firends.pop(0) # 弹出特定索引Out: 'Olivia'                                                   In : firends
Out: ['Sophia', 'Sarah']
```

**remove**

```
In : firends.remove('Sophia')In : firends
Out[21]: ['Sarah']
```

**搜索元素**

```
In : l = [1, 2, 1, 3]
In : l.index(1)
Out: 0
In : l.index(1, 1)
Out: 2
In : 3 in l
Out: True
In : 6 in l
Out: False
```

**排序**

```
In : l = [1, 3, 2]                                      
In : sorted(l)                                          
Out: [1, 2, 3]                                          
In : l
Out: [1, 3, 2]                                          
In : l.sort()                                           
In : l
Out: [1, 2, 3]
```

**reverse**

```
In : list(reversed(l)) 
Out: [3, 2, 1]
In : l.reverse()
In : l
Out: [3, 2, 1] 
In : sorted([1, 3, 2], reverse=False)
Out: [1, 2, 3]
In : sorted([1, 3, 2], reverse=True)
Out: [3, 2, 1]
```

**延伸阅读**

1. [https://developers.google.com/edu/python/lists](https://developers.google.com/edu/python/lists)
2. [https://developers.google.com/edu/python/sorting](https://developers.google.com/edu/python/sorting)

## 元组
列表适合存储可变化的数据集，元组不可被修改

```
In : tup = (1, 2)In : tuple([1, 2])
Out: (1, 2)
In : list((1, 2))
Out: [1, 2] 
```

##### 为什么需要？
不可变的对象可以做一些优化，出于性能和内存占用</br>
timeit用来测试性能

```
❯ python -m timeit '["fee", "fie", "fo", "fum"]'
10000000 loops, best of3: 0.0844 usec per loop
❯ python -m timeit '("fee", "fie", "fo", "fum")'
10000000 loops, best of3: 0.019 usec per loop
```

直接用圆括号会被当做顺序运算，要加逗号

```
In : tup = (1,)
In : type(tup)
Out: tuple 
```

可以通过下标访问，但不能修改其中的元素</br>
下面代码虽然报错，但是赋值成功了，因为分了两步，列表赋值成功，元组失败，变量赋值采用对象引用，传递的是对象内存地址，不能干涉对象的修改

```
In : tup = (1, 2, [3, 4])
In : tup[2] += [5, 6]

---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-112-cd710f9ae250> in <module>()
----> 1 tup[2] += [5, 6]
TypeError: 'tuple' object does not support item assignment
In : tupOut: (1, 2, [3, 4, 5, 6])
```

不能删除，但可以合并。index查找(值，开始，结束)，默认返回第一个。count获得符合的数量。

```
In : (1, 2) + (3, 4)
Out: (1, 2, 3, 4)
In : tup = (1, 2, 'a', 1)
In : tup.index(1)
Out: 0
In : tup.index(1, 1)
Out: 3
In : tup.index(1, 1, 4)
Out: 3
In : tup.count('a')
Out: 1
```

##### 延伸阅读
1. [https://docs.python.org/3/library/timeit.html](https://docs.python.org/3/library/timeit.html)
2. [http://t.cn/RnvaZ6j](http://t.cn/RnvaZ6j)

## 字典
**元组做键**

```
In : dct = {}
In : dct['{}_{}'.format('a', 'c')] = 'T'
In : dct = {                                            'a': {'c': 'T', 'd': 'N'}                           }                                                 
In : dct['a']['c']                                      
Out: 'T'
In : dct[('a', 'c')] = 'T'
In : dct[('a', 'c')]
Out: 'T'
```

**get**<br/>
获取如果没有键，不会报错，还可以设置第二个参数，如果找不到就默认返回

```
In : dct.get('a')                                                               
Out: 1In : dct.get('b')
In : dct['b']                                                                   
---------------------------------------------------------------------------     
KeyError                                  Traceback (most recent call last)     <ipython-input-140-8466ca6f70f2> in <module>()                                  
----> 1 dct['b']                                                                
KeyError: 'b'
In : dct.get('b', 2)                                                            
Out: 2
```

**update**<br/>
没有就添加，有就更新

```
In : dct.update(a=1, b=2, c=3)                          
In : dct
Out: {'a': 1, 'b': 2, 'c': 3} 
```

**del**<br/>
可以删掉，in判断是否在字典中

```
In : 'a' in dct                                         
Out: TrueIn : del dct['a']                                       
In : 'a' in dct                                         
Out: False
```

python2中键值对顺序不保证，使用OrderedDict,在python3.6之后会记忆这个顺序

```
>>> from collections import OrderedDict
>>> dct = OrderedDict(a=1)                              
>>> dct['b'] = 2
>>> dct['c'] = 3
>>> dct                                                 
OrderedDict([('a', 1), ('b', 2), ('c', 3)]) 
```

获取全部的键、全部的值、全部的键值对

```
In : dct = dict(a=1, c=2)                               
In : dct.keys()                                         
Out: dict_keys(['a', 'c'])                              
In : dct.values()                                       
Out: dict_values([1, 2])                                
In : dct.items()                                        
Out: dict_items([('a', 1), ('c', 2)]) 
```

##### 延伸阅读
1. [https://docs.python.org/2/library/collections.html#collections.OrderedDict](https://docs.python.org/2/library/collections.html#collections.OrderedDictl)
2. [https://developers.google.com/edu/python/dict-files](https://developers.google.com/edu/python/dict-files)

## 集合
无序，也使用{}，但没有:<br/>
和列表直接可以互相转换，如果不考虑顺序，可以使用set来去重

```
In : set([1, 3, 2, 1])                                  
Out: {1, 2, 3}                                          
In : list({1, 3, 2, 1})                                 
Out: [1, 2, 3]
```

初始化不能使用{}，这是一个空字典，要使用set()<br/>

**添加和更新**

```
In : s.add(1)                                           
In : s
Out: {1}                                                
In : s.add(1)                                           
In : s
Out: {1} 
In : s.update([2, 3])                                   
In : s
Out: {1, 2, 3}                                          
In : s.update([3, 4])                                   
In : s
Out: {1, 2, 3, 4} 
```

**删除**<br/>
discard不会报错

```
In : s.remove(4)                                                                
In : s
Out: {1, 2, 3}                                                                  
In : s.remove(4)                                                                
---------------------------------------------------------------------------     
KeyError                                  Traceback (most recent call last)     <ipython-input-33-737bdeaad795> in <module>()                                   
----> 1 s.remove(4)                                                             
KeyError: 4
In : s.discard(4)
```

也有pop，但因为集合无序，返回的元素不确定

**issubset/issuperset**<br/>
判断两个集合的超集和子集

```
In : set([1]).issubset(set([1, 2]))                     
Out: True
In : set([1]).issuperset(set([1, 2]))                   
Out: False
In : set([1, 2]).issuperset(set([1]))                   
Out: True
```

**交并差**

```
In : s1 = set([1, 2, 3])                                
In : s2 = set([3, 4, 1])                                
In : s1 & s2  # 交集
Out: {1, 3}                                             
In : s1 | s2  # 并集
Out: {1, 2, 3, 4}                                       
In : s1 - s2  # 差集
Out: {2}                                                
In : s2 - s1                                            
Out: {4}
```

**不变集合**

```
In : fs = frozenset('hello')                                                    
In : fs
Out: frozenset({'e', 'h', 'l', 'o'})                                            
In : fs.add('a')                                                                
---------------------------------------------------------------------------     
AttributeError                            Traceback (most recent call last)     
<ipython-input-38-09a9d04c7ae3> in <module>()                                   
----> 1 fs.add('a')                                                             
AttributeError: 'frozenset' object has no attribute 'add'
```

##### 延伸阅读
1. [https://www.geeksforgeeks.org/sets-in-python/](https://www.geeksforgeeks.org/sets-in-python/)


## 控制流

字典迭代 键 或 键和值

```
In : d = {'a': 1, 'b': 2}                               
In : for k in d:                                        
	...:     print(k, d[k])                                 
	...:                                                    
a 1
b 2
In : for k, v in d.items():                                
	...:     print(k, v)                                    
	...:                                                 
a 1
b 2
```

**else**<br/>
除了在if中使用，在循环for或while中也可以使用，while变为false而非break语句中止的

```
In : for i in l:                                        
....:     continue                                      
....: else:                                             
....:     print('do else')                              
....:                                                   
do else

In : for i in l:                                        
....:     if i == 3:                                    
....:         break                                     
....: else:                                             
....:     print('do else')                              
....:

In : i = 1
In : while 1:                                           
....:     print(i)                                      
....:     if i > 2:                                     
....:         break                                     
....:     i += 1
....: else:                                             
....:     print('do else')                              
....:                                                   
1
2
3

```

##### 延伸阅读
1. [https://docs.python.org/3/tutorial/controlflow.html](https://docs.python.org/3/tutorial/controlflow.html)
2. [https://python.swaroopch.com/control_flow.html](https://python.swaroopch.com/control_flow.html)

# 程序分解方法
## 函数
**实参类型**

1.位置参数 (positional argument)<br/>
实参直接对应形参位置

```
In : def hello(*names):                                 
...:     print(names)                                   
...:                                                    
In : hello()                                            
()                                                      
In : hello(1)                                           
(1,)                                                    
In : hello(1, 2)                                        
(1, 2) 
```
2.强制关键字参数</br>
python3.6 block前加了*，必须使用关键字形式的参数，不能使用位置参数

```
In : def hello(name='World'):                           
...:     print(f'Hello, {name}!')                       
...:                                                    
In : hello()                                            
Hello, World!                                           
In : hello('Amy')                                       
Hello, Amy!                                             
In : hello(name='Chris')                                
Hello, Chris! 
```
3.** 变长关键字参数

```
In : def run(a, b=1, **kwargs):
...:     print(kwargs)
...:     
In : run(1)
{}
In : run(1, b=2)
{}
In : run(1, c=1)
{'c': 1}
```

如果混合使用，位置参数必须位于关键字参数之前，顺序：常规参数、默认参数、变长元祖参数、变长关键字参数

```
In : def hello(name, default='World'):                                  
...:     print(f'Hello, {name or default}!')                            
...:

In : def func(a, b=0, *args, **kwargs):                                 
...:     print('a =', a, 'b =', b, 'args =', args, 'kwargs =', kwargs)  
...: 
```

参数可以为函数

```
In : defhello(name):
...:     print(f'Hello {name}!')                        
...:                                                    

In : deftest(func, name='World'):
...:     func(name)                                     
...:                                                    

In : test(hello, 'Amy')                                 
Hello Amy! 
```
在函数中给全局变量赋值会出现错误<br/>
*错误：*

```
In : g = 0 
                                                                    
In : def run():                                                                
...:     print(g)                                                              
...:     g = 2                                                                 
...:     print(g) 
```
*解决方法：* **global** 不推荐使用

```
In : def run():                                         
...:     global g                                       
...:     g += 2
...:     print(g)                                       
...:                                                    

In : run()                                              
2

In : g
Out: 2

In : run()                                              
4

In : g
Out: 4
```

**作用域(scope)**<br/>
B：build-in 系统变量<br/>
G：global 全局变量<br/>
E：enclosing 嵌套作用域<br/>
L：local 本地作用域

**系统变量**<br/>
```
In : import builtins
```  

**Python 2 系统变量**

```
>>> import __builtin__                                  
>>> dir(__builtin__) 
```

**嵌套作用域**
把run2赋值给f

```
In : g = 0

In : def run():                                         
...:    g = 2
...:    def run2():                                     
...:        print(g)                                    
...:    return run2                                     
...:                                                    

In : f = run()                                          

In : f()                                                
2
```

**闭包(Closure)**<br/>
闭包指延伸了作用域的函数，其中包含函数定义体中引用，但是不在定义体中定义的非全局变量，它能访问定义体之外定义的非全局变量

maker也叫做工厂函数，f，g都是它生产的函数

```
In : def maker(n):                                      
...:     def action(m):                                 
...:         return m * n                               
...:     return action                                  
...:                                                    

In : f = maker(3)                                       

In : f(2)                                               
Out: 6

In : g = maker(10)                                      

In : g(2)                                               
Out: 20
```

**nonlocal**

```
In : def run():                         
...:     g = 2
...:     def run2():                    
...:         g = 4
...:         print('inner ---> ', g)    
...:     run2()                         
...:     print('outer --->', g)         
...:                                    

In : run()                              
inner --->  4
outer ---> 2
```
修改嵌套作用域里面的变量，对父级作用域的变量做修改

```
In : def run():                         
...:     g = 2
...:     def run2():   
...:         nonlocal g                 
...:         g = 4
...:         print('inner ---> ', g)    
...:     run2()                         
...:     print('outer --->', g)         
...:                                    

In : run()                              
inner --->  4
outer ---> 4
```

**匿名函数**<br/>
lambda 临时性，小巧的函数

```
In : f = lambda n: n * 2

In : f(10)                              
Out: 20

In : list(map(lambda x: x * 2, l1))                     
Out: [2, 6, 8]  

In : l = [[2, 4], [1, 1], [9, 3]]                       

In : sorted(l)                                          
Out: [[1, 1], [2, 4], [9, 3]] 

In : sorted(l, key=lambda x:x[1])                       
Out: [[1, 1], [9, 3], [2, 4]]  

In : l3 = ['/boot/grub', '/usr/local', '/home/dongwm']  

In : sorted(l3, key=lambda x: x.rsplit('/')[2])         
Out: ['/home/dongwm', '/boot/grub', '/usr/local'] 
```

**map**<br/>
对l1中的每个元素使用double

```
l1 = [1, 3, 4] 
rs = map(double, l1)
```

**filter**<br/>
过滤掉符合is_odd的元素

```
def is_odd(x):                                                     
    return x % 2 == 1                                                                   

rs = filter(is_odd, l1) 

In : rsOut: <filter at0x105986d68>                                            

In : list(rs)                                                           
Out: [1, 3]
``` 

**reduce**
将每个元素执行add函数

```
In : def add(a, b):                                     
...:     return a + b                                   
...:                                                    

In : from functools import reduce                       

In : reduce(add, [1, 2, 3])                             
Out: 6

In : reduce(add, [1, 2, 3], 10)                         
Out: 16
```

**zip**<br/>
两列表中对应下标的元素组合成元组

```
In : a = [1, 2, 3]

In : b = [4, 5, 6]

In : c = [7, 8, 9, 10]

In : zip(a, b)
Out: <zip at 0x10f305a48>

In : list(zip(a, b))
Out: [(1, 4), (2, 5), (3, 6)]

In : list(zip(a, c))
Out: [(1, 7), (2, 8), (3, 9)]

In : list(zip(*zip(a, b))) 
Out: [(1, 2, 3), (4, 5, 6)]
```

**开发陷阱(一) 可变默认参数**

```
In : def append_to(element, to=[]):     
...:    to.append(element)              
...:    return to                       
...:                                    

In : my_list = append_to(12)            

In : my_list
Out[83]: [12]                           

In : my_other_list = append_to(42)      

In : my_other_list
Out: [12, 42]
```
不在列表后继续加

```
In : def append_to(element, to=None):   
...:    if to is None:                  
...:        to = []                     
...:    to.append(element)              
...:    return to
...: 
```

**开发陷阱(二) 闭包变量绑定**

```
In : def create_multipliers():                          
...:    return [lambda x : i * x for i inrange(5)]     
...:                                                    

In : for multiplier in create_multipliers():            
...:    print(multiplier(2))                            
...:                                                    
8
8
8
8
8
```
解决1：函数默认值

```
In : def create_multipliers():                                  
....:    return [lambda x, i=i : i * x for i inrange(5)]       
....: 
```
解决2：偏函数partial

```
In : from functools import partial
In : from operator import mul                                   

In : def create_multipliers():                                  
....:    return [partial(mul, i) for i in range(5)]             
....: 
```

##### 延伸阅读
1. [https://www.learnpython.org/en/Functions](https://www.learnpython.org/en/Functions)
2. [http://book.pythontips.com/en/latest/map_filter.html](http://book.pythontips.com/en/latest/map_filter.html)
3. [https://docs.python.org/3/library/functools.html](https://docs.python.org/3/library/functools.html)