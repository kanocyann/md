# 零、基本的底层逻辑和简单的输入输出

## 基本的底层逻辑

python的变量在底层的存储逻辑类似于“标签”和“盒子”

例如：

```python
a = 2
print(id(a))
d = 2
print(id(d))
```

输出时会看到a和d这两个变量指向的地址是一样的

我们可以画图来理解

## 简单的输入输出

```python
# 输入语句input
a = input()
b = input('请输入一些信息：')

# 输出语句print
# print默认会输出换行,可以通过end参数来修改。
# print可以同时输出多个值，多个值之间需要使用,隔开
# print输出多个值时，默认用空格隔开
print(a)
print(a, end=" ")
print(a, b)
print(a, b, sep="+")

```



# 一、基础数据类型

## 数

数并不是一种显式的可以声明的类型，数是一种泛指。对于Python基础来说数只有两种：

一种是整数类型，一种是浮点数类型。数是不可变的数据类型！！！

不可变：**当该数据类型的对应变量的值发生了改变，那么它对应的内存地址也会发生改变，就称不可变数据类型**

用上面提到的标签论来理解的话，就是讲该变量“粘贴”到了另外的一个“盒子”上

```python
a = 1   # int   类型
b = 1.0 # float 类型
print(type(a))
print(type(b))
```



## 列表: list

列表是Python的主力数据类型之一，列表是一种类似数组的强大的数据类型，但是列表可以储存不同数据类型的对象，对象是什么暂且不考虑。可以先认为列表是一个可以储存不同类型数据的数组。这是普通的数组办不到的事情(忽略掉指针的因素，如果你不知道什么是指针也没有问题)。虽然列表中可以储存不同的数据，但是建议使用列表时不要再同一个列表中储存多种数据类型。

```  python
l = []                # 列表的定义。
l.append(obj)         # 在列表末尾添加一个元素。
l.pop(index)          # 删除列表中第index个元素，第index个元素后面的元素全部前移。
l.insert(index, obj)  # 在第index个位置插入obj，原来index位置的元素全部后移。
l.remove(obj)         # 在列表中删除元素obj，如果列表中不存在obj则报错。
l.index(obj)          # 获取元素obj在列表中的下标。
l.clear()             # 清空列表
# 这些只是列表方法中的一些，还有好多方法需要你们自己去查阅
nums = [1, 2, 3]
print(nums)
nums[0] = 100
print(nums)
nums.append(4)
print(nums)
nums.pop()
print(nums)
nums.pop(0)
print(nums)
index = nums.index(2)
print(index)
nums.insert(1, 10)
print(nums)
# 下面这几行代码可以看到列表中大部分可用的方法
func_list = [func for func in dir(list) if not func.startswith('_')]
for index, func in enumerate(func_list):
    print(func, end=' ')
    if index % 5 == 4:
        print()
print()
```



## 元组: tuple

元组这种数据类型很重要，但是对于初学者来说，只需要知道元组相当于不可变的数组就可以了，但是如果你深入去了解元组的话，你会发现并不是这样。元组是不可变的数据类型！！！

```python
a = (1, 2, 3)
a[0] = 5  #这样会报错
print(a)
b = (1,)  #如果一个元组中只有一个元素，一定要记得加上,否则不会被视为元组
```



## 字符串: str

字符串是一个很让人心烦的数据类型，当然让人心烦的不是它的操作有多么得复杂，而是因为编码的原因。如果你在用print方法输出一个字符串的时候报错的话，不要慌张因为那不是你的错，而是python本身编码处理机制的问题。不过如果你不需要处理一些奇奇怪怪的文本的话，那么就不需要考虑我所说的问题。

```python
# 字符串定义形式分为两种：普通字符串与长字符串
# 普通字符串有两种定义形式, ''与""均可定义普通字符串
s1 = 'abc'
s2 = "abc"
# 长字符串只有一种定义形式
s3 = """abc"""

s = 'sadfhgf fdsjfs jdsfs'
print(s)
print('-'*100)

a = s.replace('a', '1')

print(s)
print(a)
print('-'*100)

s = s.split()
print(s)
# s[1] = 2
# print(s)
print('-'*100)

s = '+'.join(s)
print(s)

print(s.find('aa'))
print(s.index('aa'))
# 下面这几行代码可以看到字符串中大部分可用的方法
func_list = [func for func in dir(str) if not func.startswith('_')]
for index, func in enumerate(func_list):
    print(func, end=' ')
    if index % 5 == 4:
        print()
```

### 切片

切片是一种常见的操作，切片适用于许多数据类型，就目前而言，只需要知道字符串、列表和元组都可以使用切片操作就可以了。那么切片操作是什么呢？，切片操作可以获取一个序列中一段区间内的元素（全部元素也是可以的）。

可用于所有支持切片的数据类型

```python
#切片操作的语法 obj[start:end:step]
# obj为一个可进行切片操作的对象，目前可认为是列表或者元组
# start与end均为下标，取值范围为[start, end),step为步长
# start不写默认为0，end不写默认为最后一个元素的下标,step不写默认为1


l = [1, 2, 3, 4, 5]
print(l[1:3]) # 结果为[2, 3]
l = [1, 2, 3, 4, 5, 6, 7, 8, 9]
print(l[0:9:2]) # 结果为[1, 3, 5, 7, 9]
t = (1, 2, 3, 4, 5)
print(t[1:3]) # 结果为(2, 3)
t = (1, 2, 3, 4, 5, 6, 7, 8, 9)
print(t[0:9:2]) # 结果为(1, 3, 5, 7, 9)
s = '123456789'
print(s[1::2])
```

### 列表推导（List comprehension）

是一种在编程中用来创建新的列表的简洁方法。它允许你使用一行代码来生成一个新列表，同时可以对原始列表进行筛选、转换或组合操作。

new_list = [expression for item in old_list if condition]

- `expression` 是用来对 `item` 进行转换或运算得到新元素的表达式。
- `item` 是从 `old_list` 中取出的每个元素。
- `condition` 是一个可选的条件，只有满足条件的元素才会被包含在新列表中。

创建一个新的列表，其中包含原始列表中每个元素的平方：

```python
old_list = [1, 2, 3, 4, 5]
new_list = [x ** 2 for x in old_list]
# 输出: [1, 4, 9, 16, 25]
```

创建一个新的列表，其中仅包含原始列表中大于3的元素的平方：

```python
old_list = [1, 2, 3, 4, 5]
new_list = [x ** 2 for x in old_list if x > 3]
# 输出: [16, 25]
```

使用条件表达式对原始列表中的元素进行转换：

```python
old_list = [1, 2, 3, 4, 5]
new_list = ["even" if x % 2 == 0 else "odd" for x in old_list]
# 输出: ['odd', 'even', 'odd', 'even', 'odd']
```

## 字典: dict

```python
d = {}
"""
存储形式：key:value
key: 不可变数据类型
value: 啥都行
"""
d = {1: 'asdf', 'a': [1, 2, 3], (2, ): 7}
print(d[1])
print(d['a'])
print(d[(2, )])
d[2] = 456
print(d[2])
print(d)
d[2] = 789
print(d[2])
print(d)
print(d.get(1))
print(d.get(10))
d.clear()
d.setdefault(1, 2)
d.setdefault(3, 2)
print(d)
m = d.fromkeys([1, 2, 3, 4])
print(m)
d = {1: 1, 2: 2, 3: 3}
new_d = {4: 4, 5: 5, 6: 6, 1: 10}

d.update(new_d)
print(d)

# 字典视图 可看不可改
# 可迭代对象
print(d.keys())
print(d.values())
print(d.items())

# 下面这几行代码可以看到字典中大部分可用的方法
func_list = [func for func in dir(dict) if not func.startswith('_')]
for index, func in enumerate(func_list):
    print(func, end=' ')
    if index % 5 == 4:
        print()
print()
```



## 集合: set()

这个数据类型就是字面上的意思，与数学上的集合功能一致。而且经过几个版本的优化，现在版本的集合性能更加优秀了。虽然没有特殊需求的话还是没人用这个数据类型（因为都去用列表和字典了）。但是用集合去重是个不错的选择。

```python
s = set() # 集合定义
s = {1, 2, 3}
# 下面这几行代码可以看到集合中大部分可用的方法（方法自己看吧，我写不动了）
func_list = [func for func in dir(set) if not func.startswith('_')]
for index, func in enumerate(func_list):
    print(func, end=' ')
    if index % 5 == 4:
        print()
print()
```

### python的内存管理

#### 内存分配

Python解释器在运行时为程序分配内存。当程序创建变量、对象或函数时，解释器会在内存中为其分配空间。Python使用堆来管理内存，这意味着内存分配和释放是动态的。

#### 垃圾回收

Python的垃圾回收机制用于自动释放不再使用的内存。当一个对象没有任何引用指向它时，解释器会自动将其标记为垃圾，并在适当的时候将其回收。这种机制可以避免内存泄漏问题。

#### 引用计数

Python使用引用计数来跟踪对象的引用数量。每当一个对象被引用时，其引用计数会增加；当一个引用被删除时，其引用计数会减少。当一个对象的引用计数为0时，解释器会将其标记为垃圾并回收其内存。

#### 循环引用

在Python中，循环引用是一种常见的问题。当两个或多个对象相互引用时，它们的引用计数永远不会为0，因此无法被垃圾回收。Python通过周期检测来解决循环引用问题。当检测到循环引用时，解释器会将其标记为垃圾并回收其内存。



# 二、分支循环

### 分支

记住python中没有switch,也没有goto！！！

```python
# if elif else
a = int(input('请输入一个数字：'))

if a == 5:
    print('abc')
elif a == 6:
    print('bcd')
elif a == 7:
    print('edc')
else:
    print('asdfg')
    
if a == 5 and a == 6:
    print('abc')
elif a <= 6:
    print('bcd')
elif a >= 7:
    print('edc')
else:
    print('asdfg')

a = [1, 2, 3]
b = [1, 2, 3]
print(a == b)
print(a is b)
if a == b or a == 7:
    print(a is b)

c = {1: 4, 2: 5, 3: 6}

if 5 in c:
    print('5 在 c 里')
else:
    print('5 不在 c 里')

print(list(c))

# 有很多种可以使条件为假得情况，但是这些情况一般都不等价
if "": print('这是真1')
if '': print('这是真2')
if None: print('这是真3')
if {}: print('这是真4')
if []: print('这是真5')
if """""": print('这是真6')
if 0: print('这是真7')
if False: print('这是真8')
if 1: print('这是真9')
if 10: print('这是真10')

    
# 三元表达式
a = [4, 5, 6]

n = 7 if len(a) > 3 else 5
print(n)
```



### 循环

python中的循环有两种：一种是while，一种是for...in...

#### while循环

```python
"""
while 条件:
	循环体
"""

a = 10
while a:
    a -= 1
print("完成")
```



#### for ... in ... 循环

```python
"""
for 临时变量 in 可迭代对象:
	循环体
break
continue
range(start, end, step)       # 生成一个列表
enumerate(可迭代对象)           # 获取下标迭代
zip(可迭代对象, 可迭代对象, ...)  # 并行迭代
"""

print(list(range(0, 10, 2)))
for i in range(10):
    print(i)

a = [(4, 4), (5, 5), (6, 6)]

for index, obj in enumerate(a):
    print(index, obj)

print('-'*10)
index, obj = (0, (4, 4))
print(index, obj)

a, b = (4, 4)
print(a, b)

a = [4, 5, 6]
b = [7, 8, 9, 10]
c = [7, 8, 9, 10]

for i, j, k in zip(a, b, c):
    print(i, j, k)
```

##### 元组拆包

这是一个很重要的操作

```python
# 两数交换
a = 3
b = 5
a, b = b, a

```



# 三、函数

python的函数只有一个返回值，不论函数中return写成什么样，返回值始终只有一个，是一个元组

```python
# 裴波那契数列
def f(n):
    if  n <= 2:
        return 1
    else:
        return f(n - 1) + f(n - 2)
def a():
    """
    默认返回值为None
    """
	print('no return')
    
def b():
    return 1, 2, 3, 4

ret, ret2, ret3, ret4 = b()
print(ret, ret2, ret3, ret4)

def c():
    return 1, 2, 3, 4, 5, 6, 7

ret, *ret2, ret3= a()
print(ret, ret2, ret3)

"""
weight有99%的时候都为0.9，所以weight设置默认值为0.9放在参数收集后面值得
"""

def nums_add(a=1, b=1, *c, weight=0.9):
    ret = a + b

    for i in c:
        ret += i

    return ret * weight


print(nums_add(1, 2, 3, 4,))


def final_nums_add(a=0, b=0, *args, **kwargs):
    weight = 1
    if kwargs.get('weight'):
        weight = kwargs.get('weight')
    ret = a + b
    for arg in args:
        ret += arg

    return ret * weight


"""
递归函数的写法
0.把一个问题拆解若干个操作相同的步骤
1.函数是干什么的？(为了解决操作相同的步骤，！！！只解决一步！！！)
2.操作什么时候停止
"""

def jie_cheng(n):

    if n == 1:
        return 1
    else:
        return n * jie_cheng(n - 1)

#抽象
#函数就是一种抽象
```



# 四、类

这是一个很重要的概念，这涉及到面向过程编程思想到面向对象思想编程的转换

类的定义 

类是将同类对象的共同属性和行为抽象出来形成的一个相对复杂的数据类型, 这与结构体一样，都是为了描述一个相对复杂的对象。

对象（object）：是该类型的一个实例。

类的属性指类的特征，方法指类的行为。

举例：

类 抽象
车类 能开 能驾驶
油车 我能加油
电车 我能加电
坦克 还能开炮
大众 我是大众
比亚迪 我是比亚迪、

属性和方法
人 属性：我有性别 我有身高和体重 我有身份证号 方法：我可以吃 我可以喝

需求：黑龙江大学计软学院要求参加一项竞赛，黑大学生需要注册，计软学院的同学都需要完成竞赛A，科协的同学需要在完成竞赛A的同时完成竞赛B

黑大学生类 属性 学号 姓名 专业 班级 方法 能完成注册
计软学生类 方法：参加竞赛A
科协学生类 属性 科协内组别 方法：完成竞赛B

```python
"""
class 类名():
	属性
    方法
"""
#类与对象的关系
#A继承B
#A有B的所有东西
# 实际意义上B包含A，因为A是B的特例
# 在代码上A不仅要具有B有的东西，还要跟B有明显的区别

# 继承
# 封装
# 多态
```



## 链表

```python
class Node:

    def __init__(self, data):

        self.data = data
        self.next = None

    def get_data(self):

        return self.data

    def get_next(self):

        return self.next

    def set_next(self, next_node):

        self.next = next_node

    def set_data(self, new_data):

        self.data = new_data


class LinkList:

    def __init__(self):

        self.length = 0
        self.head = None

    def is_empty(self):

        return self.head is None

    def get_length(self):

        return self.length

    def __len__(self):

        return self.length

    def add(self, data):

        item = Node(data)
        item.set_next(self.head)
        self.head = item
        self.length += 1

    def show(self):

        temp = self.head
        while temp is not None:
            print(temp.get_data(), end=' ')
            temp = temp.get_next()
        print()

    def remove(self, data):

        previous = None
        found = False
        temp = self.head
        while not found and temp is not None:

            if temp.get_data() == data:
                found = True
            else:
                previous = temp
                temp = temp.get_next()

        if found:
            if previous:
                previous.set_next(temp.get_next())
            else:
                self.head = temp.get_next()
            self.length -= 1
        return found

    def search(self, data):

        temp = self.head
        found = False
        while not found and temp is not None:

            if temp.get_data() == data:
                found = True
            temp = temp.get_next()

        return found

    def change(self, old_data, new_data):

        temp = self.head
        found = False
        while not found and temp is not None:

            if temp.get_data() == old_data:
                found = True
                temp.set_data(new_data)
            temp = temp.get_next()

        return found


if __name__ == '__main__':

    head = LinkList()
    print(head.is_empty())
    head.add(1)
    head.add(2)
    head.add(3)
    head.show()
    print(head.remove(3))
    head.show()
    print(head.is_empty())
    print(head.search(5))
    print(head.search(2))
    print(head.change(5, 8))
    head.show()
    print(head.change(1, 10))
    head.show()
    print(len(head))
    print(head.get_length())




```

## 栈

```python
class Stack:

    def __init__(self):
        self.items = []

    def is_empty(self):

        return self.items == []

    def push(self, item):

        self.items.append(item)

    def pop(self):

        return self.items.pop()

    def peek(self):

        return self.items[-1]

    def size(self):

        return len(self.items)
```



# 五、异常

异常捕捉可以使用 **try/except** 语句。

try 语句按照如下方式工作；

- 首先，执行 try 子句（在关键字 try 和关键字 except 之间的语句）。
- 如果没有异常发生，忽略 except 子句，try 子句执行后结束。
- 如果在执行 try 子句的过程中发生了异常，那么 try 子句余下的部分将被忽略。如果异常的类型和 except 之后的名称相符，那么对应的 except 子句将被执行。
- 如果一个异常没有与任何的 except 匹配，那么这个异常将会传递给上层的 try 中。

一个 try 语句可能包含多个except子句，分别来处理不同的特定的异常。最多只有一个分支会被执行。

处理程序将只针对对应的 try 子句中的异常进行处理，而不是其他的 try 的处理程序中的异常。

一个except子句可以同时处理多个异常，这些异常将被放在一个括号里成为一个元组

**try/except** 语句还有一个可选的 **else** 子句，如果使用这个子句，那么必须放在所有的 except 子句之后。

else 子句将在 try 子句没有发生任何异常的时候执行。

try-finally 语句

try-finally 语句无论是否发生异常都将执行最后的代码。

```python
while True:
	a = input('请输入一个表达式：')
	if a:
		try:
			print(eval(a))
		except Exception as e:
			print('请输入一个正确的表达式')
	else:
		print('已退出')
		break

while True:
	a = input('请输入一个表达式：')
	if a:
		try:
			print(eval(a))
		except ValueError as e:
			print(e)
		except TypeError as e:
			print(e)
		except Exception as e:
			print('请输入一个正确的表达式')
		else:
			print('else')
		finally:
			print('计算完毕')
	else:
		print('已退出')
		break
		
```

万能的else

# 六、魔法方法、特性、与迭代器

魔法方法（Magic Methods）是在 Python 中特殊命名的方法，它们以双下划线开头和结尾，用于定义对象的行为和特性。通过实现这些魔法方法，我们可以自定义类的行为方式，并使其具有类似内置类型的功能。

以下是一些常用的魔法方法：

- `__init__(self, ...)`: 初始化方法，在创建对象时调用。
- `__str__(self)`: 返回对象的字符串表示。
- `__repr__(self)`: 返回对象的可打印字符串表示。
- `__len__(self)`: 返回对象的长度。
- `__getitem__(self, key)`: 定义获取元素的行为，通过索引或键访问元素。
- `__setitem__(self, key, value)`: 定义设置元素的行为，通过索引或键修改元素。
- `__delitem__(self, key)`: 定义删除元素的行为，通过索引或键删除元素。
- `__iter__(self)`: 返回一个迭代器对象，用于支持对象的迭代。
- `__next__(self)`: 返回迭代器的下一个值。

特性（Properties）是一种用于定义访问和修改对象属性的方法，它可以隐藏实际的属性访问细节，并在访问属性时执行特定的代码。通过使用特性，我们可以将属性访问看作是对方法的调用，从而提供更好的灵活性和可维护性。

以下是一个使用特性的示例：

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius

    @radius.setter
    def radius(self, value):
        if value >= 0:
            self._radius = value
        else:
            raise ValueError("半径必须大于等于0")

circle = Circle(5)
print(circle.radius)  # 输出: 5
circle.radius = 10
print(circle.radius)  # 输出: 10
```

迭代器（Iterator）是用于遍历可迭代对象（Iterable）中元素的对象。可迭代对象是指实现了 `__iter__` 魔法方法的对象，而迭代器是实现了 `__iter__` 和 `__next__` 魔法方法的对象。迭代器通过迭代协议提供了一个统一的接口，使得我们可以使用 `for` 循环和其他迭代工具来遍历可迭代对象。

以下是一个迭代器的示例：

```python
class MyIterator:
    def __init__(self, data):
        self._data = data
        self._index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self._index >= len(self._data):
            raise StopIteration()
        value = self._data[self._index]
        self._index += 1
        return value

my_list = [1, 2, 3, 4, 5]
my_iterator = MyIterator(my_list)
for item in my_iterator:
    print(item)  # 依次输出: 1, 2, 3, 4, 5
```

通过实现迭代器，我们可以自定义遍历顺序和停止条件，并支持惰性计算，从而提供更高效和灵活的迭代方式。

### 八皇后问题

```python
#encoding=utf-8


# 检测冲突
# state为包含之前放好皇后横坐标的元组
# nextX为下一个皇后可能放的位置的横坐标
def conflict(state, nextX):
    nextY = len(state)
    for i in range(nextY):
        # state[i] - nextX 是计算出第i个皇后与你下一个要放的皇后的水平距离
        # nextY - i 是第i个皇后与你下一个要放的皇后的垂直距离
        if abs(state[i] - nextX) in (0, nextY - i):
            return True
    return False


# 基线条件
# num为下一个皇后的编号
# state为包含之前放好皇后横坐标的元组
def queens(num, state):

    if len(state) == num - 1:
        for pos in range(num):
            if not conflict(state, pos):
                yield pos


# 递归条件
# num为皇后的数量
# state为包含之前放好皇后横坐标的元组
def queens2(num=8, state=()):

    for pos in range(num):
        if not conflict(state, pos):
            if len(state) == num - 1:
                yield (pos,)
            else:
                for result in queens2(num, state + (pos,)):
                    yield (pos,) + result


# 扫尾工作
def prettyprint(soultion):

    def line(pos, length=len(soultion)):
        return '. ' * (pos) + 'X ' + '. ' * (length - pos - 1)

    for pos in soultion:
        print(line(pos))


if __name__ == '__main__':

    for result in list(queens2()):
        prettyprint(result)
        print()
```



# 七、开箱即用

在Python中，有很多内置的模块和函数可以直接使用，这就是所谓的“开箱即用”（batteries included）。这些内置的功能使得Python成为一个强大而灵活的编程语言，无需额外安装或配置即可开始使用。



# 八、文件

```python
try:
	f = open('1第一章 血尸.txt', 'r', encoding='utf-8')
	lines = f.readlines()
	for line in lines:
		print(line, end='')
except Exception:
	print("文件打开失败")
finally:
	f.close()
    
    
text = 'dsaifjjflhdskh'

with open('1.txt', 'w', encoding='utf-8') as f:
	f.write(text)
```



# 九、Python拓展