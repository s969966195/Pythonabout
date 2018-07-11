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