### 一、基本使用###

##### （一）、python的一些优势
Python 是一种解释型语言，不需要编译和链接，可以节省大量开发时间。它的解释器实现了交互式操作，轻而易举地就能试用各种语言功能，编写临时程序，或在自底向上的程序开发中测试功能。同时，它还是一个超好用的计算器。

Python 程序简洁、易读，通常比实现同种功能的 C、C++、Java 代码短很多，原因如下：
- 高级数据类型允许在单一语句中表述复杂操作；
- 使用缩进，而不是括号实现代码块分组；
- 无需预声明变量或参数。

##### (二)、python相关的一些七七八八

**一、 python基本变量定义与使用**
1. int、float与Java使用方式类似，除此之外，值得注意的是，Python中新定义了`Decimal` 与 `Fraction`类型，大致细节后续会详细介绍（后补）
---------------------------------------------------------------
2. 字符串类型：
		注：Python中具有`''`和`“”`两种类型，其单引号和双引号在使用中并未有什么区别，和Java中不同，`''`不表示字符，也可以用于定义字符串；其他的使用与Java中基本类似
如果不希望前置 `\` 的字符转义成特殊字符，可以使用 _原始字符串_，在引号前添加 `r` 即可：
```python
print('C:\some\name')  #输出为：C:\some
						#      ame
print(r'C:\some\name') #输出为：C:\some\name
```
字符串字面值可以实现跨行连续输入。实现方式是用三引号：`"""..."""` 或 `'''...'''`，字符串行尾会自动加上回车换行，如果不需要回车换行，在行尾添加 `\` 即可。示例如下：
```python
print("""\
... Usage: thingy [OPTIONS]
...      -h                        Display this usage message
...      -H hostname               Hostname to connect to
... """)

```
相邻的两个或多个 _字符串字面值_  会自动合并，该选项只适用于两个字面值，不能用于变量或表达式，变量或表达式需使用`+`号。
```python
>>> prefix = 'py'
>>> prefix 'thon'
>>> # File "<stdin>", line 1  prefix 'thon'  SyntaxError: invalid syntax 发生报错
>>> prefix + 'thon'  #正确用法
'python'
>>>
```
字符串具有 _缩引_ 特性，第一个字符的缩引是`0`，单字符没有专用的类型，就是长度为一的字符串，缩引还具有负值，可以从右至左查询：
```python
>>> word = 'Python'
>>> word[0]
'P'
>>> word[1]
'y'
>>> word[-1]
'n'
>>> word[-0]    #值得注意的是，0和-0一样，负数缩引从-1开始
'P'
```
除了索引，字符串还支持 _切片_。索引可以提取单个字符，_切片_ 则提取子字符串：
```python
>>> word[0:2]
'Py'
>>> word[0:5]
'Pytho'
>>>
```

注意，输出结果包含切片开始，但不包含切片结束。因此，`s[:i] + s[i:]` 总是等于 `s`：
```python
>>> word[:2] + word[2:]
'Python'
>>> word[:4]+word[4:]
'Python'
>>>
```
注：_索引_ 越界会报错，而 _切片_ 越界并不会报错，会自动处理越界索引：
```python
>>> word[42]  #Traceback (most recent call last):File "<stdin>", line 1, in <module> IndexError: string index out of range
>>> word[2:43]
'thon'
```
python中字符串与Java中相同，都是不可修改的，是 immutable 的。因此为字符串中某个索引进行赋值是会报错的，要生成不同的字符串应新建一个字符。

内置函数`len()`返回字符串的长度
```python
>>> s = 'gadogjaidgaweo'
>>> len(s)
14
```

```
在Python中，使用`end`可以取消输出后面的换行，或用另一个字符串结尾：
```python
>>> a,b = 0,1
>>> while a < 1000:
...     print(a,end=',')
...     a,b = b,a+b
...
0,1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,>>>
```

##### （三）、Python中的数据类型和变量 
1. 空值
空值是Python里一个特殊的值，用`None`表示。`None`不能理解为`0`，因为`0`是有意义的，而`None`是一个特殊的空值。
2. 除法
Python中定义了三种除号`/`、`//`、`%`，其中三者区别如下：
```python
>>> 10 / 3 
3.3333333333333335    #结果是浮点数，即使是可整除的数依旧为浮点数
>>> 9 / 3
>>> 3.0
>>> 
>>> 10 // 3
3               #称为地板除，两个整数的除法仍然是整数，永远都是整数
>>> 10 % 3
1               #取余数
>>>
```

##### （四）、Python中的字符串
1. `ord()`函数：获取字符的整数表示； `chr()`函数：将整数转变成对应的字符表示
```python
>>> ord('A')
65
>>> ord('B')
66
>>> ord('a')
97
>>> chr(97)
'a'
>>> chr(65)
'A'
>>>
```
由于Python的字符串类型是`str`，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把`str`变为以字节为单位的`bytes`。
Python对`bytes`类型的数据用带`b`前缀的单引号或双引号表示：`x = b'ABC'`
要注意区分`'ABC'`和`b'ABC'`，前者是`str`，后者虽然内容显示得和前者一样，但`bytes`的每个字符都只占用一个字节。
以Unicode表示的`str`通过`encode()`方法可以编码为指定的`bytes`，例如：
```python
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
>>>
```
纯英文的`str`可以用`ASCII`编码为`bytes`，内容是一样的，含有中文的`str`可以用`UTF-8`编码为`bytes`。含有中文的`str`无法用`ASCII`编码，因为中文编码的范围超过了`ASCII`编码的范围，Python会报错。
反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是`bytes`。要把`bytes`变为`str`，就需要用`decode()`方法：
```python
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
>>>
```
2. 格式化
在Python中，采用的格式化方式和C语言是一致的，用`%`实现，举例如下：
```python
>>> 'hello,%s' % 'world'
'hello,world'
>>> 'hello,%s,%d,%s' % ('world',233,'zh')
'hello,world,233,zh'
>>>
```
其`%`运算符就是用来格式化字符串的。在字符串内部，`%s`表示用字符串替换，`%d`表示用整数替换，有几个`%?`占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个`%?`，括号可以省略。
常见的占位符有：
| %d | 整数 || %f | 浮点数 || %s | 字符串 || %x | 十六进制整数 |
若在字符串中的`%` 是一个普通字符，这时候需要 `%%` 进行转义：
```python
>>> 'growth rate:%d %%' % 7
'growth rate:7 %'
>>>
```
##### （五）、Python中的list和tuple
Python内置的一种数据类型是列表：list。list是一种有序的集合，可以随时添加和删除其中的元素。
```python
>>> classmates = ['Michael','Bob','Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']
>>>
```
list是一个可变的有序表，所以，可以往list中追加元素到末尾，亦可以使用索引号进行插入：
```python
>>> classmates
['Michael', 'Bob', 'Tracy']
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
>>> classmates.insert(1,'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
>>>
```
要删除list末尾的元素，用`pop()`方法，亦可以使用 `pop(索引号)`删除索引位置的值：
```python
>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']
>>> classmates.pop(2)
'Bob'
>>> classmates
['Michael', 'Jack', 'Tracy']
>>>
```
Python 支持多种 _复合_ 数据类型，可将不同值组合在一起。最常用的 _列表_ ，是用方括号标注，逗号分隔的一组值。_列表_ 可以包含不同类型的元素，但一般情况下，各个元素的类型相同：
```python
>>> squares = ['a',1,35,'gdagk']  #python支持不同类型的值结合在一起
>>> squares
['a', 1, 35, 'gdagk']
>>>
```
Python中的列表和字符串一样，支持切片和索引，并且提供合并功能，直接使用`+`进行合并便可；其与字符串不同的是，列表是 mutable 类型，其内容是可以修改的：
```python
>>> squares = ['a',1,35,'gdagk']
>>> squares
['a', 1, 35, 'gdagk']
>>> squares[1] = 2
>>> squares
['a', 2, 35, 'gdagk']
>>>
```
列表使用`append()`方法，可以在列表的结尾添加新元素：
```python
>>> squares.append("lll")
>>> squares
['a', 2, 35, 'gdagk', 'lll']
>>>
```
使用切片为列表进行赋值，可以改变其大小，甚至于可以清空整个列表：
```python
>>> squares[0:2] = [1,2,3]
>>> squares
[1, 2, 3, 35, 'gdagk', 'lll']
>>> squares[0:] = []
>>> squares
[]
>>>
```
内置函数`len()`函数也支持列表：
```python
>>> letters = ['a','b','c','d','e']
>>> len(letters)
5
>>>
```
列表可以进行嵌套：
```python
>>> a = ['a','b','c']
>>> n = [1,2,3]
>>> x = [a,n]
>>> x
[['a', 'b', 'c'], [1, 2, 3]]
>>> x[0]
['a', 'b', 'c']
>>> x[1]
[1, 2, 3]
>>> x[0][1]
'b'
```

python中另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改，比如同样是列出同学的名字：
```python
>>> classmates = ('Michael','Bob','Tracy')
>>> classmates
('Michael', 'Bob', 'Tracy')
>>> classmates[0]
'Michael'
>>> classmates[3]   #索引越界
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: tuple index out of range
>>> classmates[1] = 'a'   #tuple中的值不可修改
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> classmates.append('a')  #tuple中没有append方法
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'tuple' object has no attribute 'append'
>>> classmates.insert(1,'a')   #tuple中没有insert方法
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'tuple' object has no attribute 'insert'
>>>
```
注意：当定义一个空tuple的时候，相对简单，但当定义只有一个元素的tuple，需要在后面加上 `,` 进行区分，不然就会发生歧义：
```python
>>> a = tuple()    #定义一个空tuple
>>> a    
()
>>> t = (1)    #此时并不表示tuple类型，而是表示数值型的数值为 `1`  (1)+(2)
>>> t
1
>>> t = (1,)  #此时才表示，定义了一个元素的tuple，其中只包括一个元素，Python在返回时，也会加上 `,` 以便用户区分
>>> t
(1,)
>>>
```

注意：在tuple中并不是tuple的值不变，而是tuple的指向不变，当tuple中的值是一个可变类似，如 `list` 那当tuple的指向不改变时，改变 `list` 中的值是可行的：
```python
>>> a = ('a','b',['zs','ls'])
>>> a
('a', 'b', ['zs', 'ls'])
>>> a[2][0] = 'ls'
>>> a[2][1] = 'zs'
>>> a
('a', 'b', ['ls', 'zs'])
>>>
```

##### （六）、Python中的条件判断
在Python中遵循缩进规则，即与Java中不同，不使用 `{}`进行代码块限定，而是根据缩进进行判断
```python
age = 20
if age >= 18:
	print('your age is',age)
	print('adult')
your age is 20   #输出
adult
>>>
```
以上根据Python的缩进规则，如果`if`语句判断是`True`，就把缩进的两行print语句执行了，否则，什么也不做。
但当以上代码在 `java` 中，当不满足条件时，则会有所不同，依旧会执行最后一句
```java
int age = 18;  
if(age > 18)  
    System.out.println("your age is:"+age);  
    System.out.println("adult");

//输出：adult 在Java中，不规范使用 {} 会导致错误代码，造成逻辑混乱，这是不规范的代码行为，是不被允许的
```

Python中对 `else if` 关键字进行了缩写，变成 `elif` 减少了代码的输入：
```python
age = 3;
if age >= 18:
	print('adult')
elif age >= 6:
	print('teenager')
else:
	print('kid')
```

python 中的输入 `input()`函数：
需要注意的是Python中的 `input()`函数的返回值，当返回值为 `str`时， `str`不能直接和整数比较，必须先把`str`转换成整数。Python提供了`int()`函数来完成这件事情：
```python
s = input('birth:')  
birth = int(s)  
  
if birth < 2000:  
    print('00前')  
else:  
    print('00后')
```

##### （七）、循环
Python的循环有两种，一种是for…in循环，可依次把list或tuple中的每个元素迭代出来，如下
```python
names = ['Michael', 'Bob', 'Tracy']  
for name in names:  
    print(name)

sum = 0  
for x in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:  
    sum = sum + x  
print(sum)
```

`range()`函数，可以生成一个整数序列，再通过`list()`函数可以转换为list。
```python
>>> list(range(5))
[0, 1, 2, 3, 4]
>>> list(range(101))
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99, 100]

sum = 0
for x in range(101):
	sum = sum + x
print(sum)

```

第二种循环是while循环，只要条件满足，就不断循环，条件不满足时退出循环。比如我们要计算100以内所有奇数之和，可以用while循环实现：

```python
sum = 0  
n = 99  
while n > 0:  
    sum = sum +n  
    n = n - 2  
print(sum)
```
##### （八）、使用dict、set

**一、dict**
Python内置了字典：dict的支持，dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。
```python
d = {'Michael':95,'Bob':75,'Tracy':85}   #初始化定义
  
d['Michael'] = 95   #使用key进行定义
   
d['Michael'] = 70   #当对同一个key进行多次赋值时，后赋值的值将会覆盖前面的值
  
print(d['Michael'])  #输出 70
```
当key的值不存在时，会报错，一般可以使用 `in` 关键子进行判断，除此之外，还可以使用 `get()` 方法进行判断，当不存在时会返回None，或自行设定返回值：
```python  
print(d['tomas'])
#KeyError: 'tomas'
print('tomas' in d)
# FALSE 当存在时会返回TRUE

print(d.get('tomas'))    #找不到Tomas因此返回None
print(d.get('tomas',-1))   #返回-1

```
`dict` 与 `list` 相同，都具有 `pop()`函数，可以进行删除，当删除的 `key`值不存在时，会报错：
```python
print(d)
d.pop('Tracy')  
print(d)
#{'Michael': 70, 'Bob': 75, 'Tracy': 85}
#{'Michael': 70, 'Bob': 75}

d.pop('tomas')  #当key值不存在，找不到'tomas'
#KeyError: 'tomas'
```

**`dict` 与 `list` 的区别**
- 查找和插入的速度极快，不会随着key的增加而增加；
- 需要占用大量的内存，内存浪费多。  
而list相反：
- 查找和插入的时间随着元素的增加而增加；
- 占用空间小，浪费内存很少。  
所以，dict是用空间来换取时间的一种方法。
dict可以用在需要高速查找的很多地方，在Python代码中几乎无处不在，正确使用dict非常重要，需要牢记的第一条就是dict的key必须是**不可变对象**。

这是因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。这个通过key计算位置的算法称为哈希算法（Hash）。

要保证hash的正确性，作为key的对象就不能变。在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key
```python
key = [1,3,5]  
d[key] = 'a list'

#TypeError: unhashable type: 'list'
```

**二、set**
set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。
要创建一个set，需要提供一个list作为输入集合：
```python
s = set([1,2,3])  
print(s)
#{1, 2, 3}
```
注意，传入的参数`[1, 2, 3]`是一个list，而显示的`{1, 2, 3}`只是告诉你这个set内部有1，2，3这3个元素，显示的顺序也不表示set是有序的。
当 `set`集合中有重复元素时，`set`会自动过滤：
```python
s = set([1,2,2,1,2,3,3,3,4])  
print(s)
#{1,2,3,4}
```
`set` 中提供了 `add()`方法进行添加元素，可以重复添加，不会引发报错，但并没有任何效果
```python
s.add(7)  
print(s)
#{1, 2, 3, 4, 7}

#以下代码报错！！！！
s.add(1,2,3,3,4,5)
print(s)
#TypeError: set.add() takes exactly one argument (7 given)
#注：以上代码会引发报错，在set中使用add()方法，一次只能添加一个元素
```

`set`中提供了 `remove(key)`方法对元素进行删除:
```python
s = set([1,2,2,1,2,3,3,3,4])  
print(s)
s.remove(4)  
print(s)
#{1, 2, 3, 4}
#{1, 2, 3}
```
`set`可看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作：
```python
s1 = set([1,2,3])  
s2 = set([2,3,4])  
print(s1 & s2)    #{2,3}
print(s1|s2)      #{1,2,3,4}
```
set和dict的唯一区别仅在于没有存储对应的value，但是，set的原理和dict一样，所以，同样不可以放入可变对象，因为无法判断两个可变对象是否相等，也就无法保证set内部“不会有重复元素”.
```python
s1 = set([2,3,4])   #创建set，其中的list是作为输入集合
s2 = set([[1,2,3],[2,3,4]])   #发生报错，与s1不同，该list列表中是list列表，属于可变类型，因此会发生报错，set中不可以是可变类型
#TypeError: unhashable type: 'list'

```

使用 `tuple`不可变量对 `dict` 与 `set` 进行赋值：
```python
# 定义一个tuple 属于不可变类型，可赋值给dict与set  
a = (1,2,3)  
b = {a:'ll'}  
print(b) #{(1, 2, 3): 'll'}  

c = ([a])  
print(c)  #[(1, 2, 3)]
```
但当 `tuple`中的值包含 `list`列表时，不可使其对 `dict` 与 `set`进行赋值：
```python
#当tuple中包含可变量list时  
d = (1,[2,3])  
e = {d,'abc'}  
print(e)  #发生报错  TypeError: unhashable type: 'list'  
f = set(d)  
print(f) #发生报错  TypeError: unhashable type: 'list'
```

##### （九）、函数

在Python中，定义一个函数要使用`def`语句，依次写出函数名、括号、括号中的参数和冒号`:`，然后，在缩进块中编写函数体，函数的返回值用`return`语句返回。
```python
def my_abs(x):   #与Java还是有一定的区别，注意定义时不需也不能注明变量类型
	if x >= 0:   #冒号一定不能省略或忘记
		return x
	else:        #依然是冒号的问题，容易忘记
		return -x
```

Python中可以定义空函数
```python
if age >= 18
	pass
```
`isinstance()`方法  可以判断指定变量是否为对应的类型：
```python
def my_abs(x):  
    if not isinstance(x,(int,float)):  
        raise TypeError('bad operand type')  #抛出异常
    if x >= 0:  
        return x  
    else:  
        return -x
```
定义函数中的默认参数：
```python
def my_power(x,n = 2):  
    s = 1  
    while n > 0:  
        n = n-1  
        s = s * x  
    return s

# 注：需要注意的是，定义默认参数要牢记一点：默认参数必须指向不变对象！比如None
def add_end(L=[]):
	L.append('END')
	return L
# 以上函数在多次调用时，会出现bug，返回多个'END'，因此需要对变量列表[]赋值为不可变量None
def add_end(L=None):  
    if L is None:  
        L = []  
    L.append('END')  
    return L

```
定义函数中的可变参数：
```python
def calc(numbers):  
    sum = 0  
    for n in numbers:  
        sum = sum + n * n  
    return sum
print(calc([1,2,3]))  #14 可变参数可以传入任意个参数，在调用时自动组装为一个tuple

```
关键字参数：
可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。
```python
def person(name,age,**kw):#可任意传入0个或任意个含参数名的参数自动组装为dict
    print('name:',name,'age:',age,'other:',kw)  
    
print(person('Michael', 30,city = 'beijing'))
#name: Michael age: 30 other: {'city': 'beijing'}
```

