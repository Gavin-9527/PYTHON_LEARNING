## 文件
#### with open（）
	控制文件读写内容的模式：t，b
		强调：t和b不能单独使用，必须跟r/w/a连用
		t文本（默认的模式）
			读写都以str（Unicode）为单位
			文本文件
			必须指定encoding='utf-8'
		b二进制/bytes(通用)
	
	r:只读
	w:只写(会清空)
	a:只追加写
	r+:若文件不存在，则报错,文件存在，指针在开头，会覆盖原有内容
	w+:清空文件，指针在最后
	a+:指针在最后

#### 文件指针操作
	指针移动都是以bytes/字节为单位
	只有在t模式下read（n）代表的是字符个数
	f.seek(n，模式) n为移动过的字节个数（可为负数）
	模式：
	0：参照物是文件开头位置
	1：当前所在位置
	2：参照物是文件末尾
	强调:只有0模式能在t模式下使用



## 函数
#### 函数的定义
	定义发生的事
		申请内存空间保存函数体代码
		将上述代码内存地址绑定函数名
		定义函数不会执行函数体代码，但是会检测函数体语法

	调用函数发生的事
		通过函数名找到函数的内存地址
		然后加括号就是在出发函数体代码的执行 

	空函数：函数体为pass

#### 函数的调用
	函数调用可以当作参数

#### 函数返回值
	return :直接跳出函数体，并将return后的值作为本次运行的结果返回
	
	当返回多个值时，返回类型为元组



### 参数
```
形参（变量名）：在定义函数阶段定义的参数称为形式参数，简称形参
实参（变量值）：在调用函数阶段传入的值称为实际参数，简称实参
```
	位置实参（从左到右的顺序传入的参数）：必须被传值，不能多不能少
	
	关键字实参：在调用阶段，按照key=value的形式传入的值

	位置实参和关键字实参混用：位置实参必须放在关键字前，不能为同一个形参重复传值

	默认形参：在定义函数阶段，就已经被赋值的形参

	位置形参和默认形参混用：位置形参必须在默认形参前面

	默认参数的值实在函数定义阶段被赋值的，被赋值的是内存地址，所以不推荐使用可变类型（不行赋为None试试）



	可变长度的参数（指调用函数时，传入的值（实参）的个数不固定）

	可变长度的位置参数（）
	*形参名（通常为*args）：用来接收溢出的位置实参，溢出的位置实参会被*保存成元组的格式，然后赋值给形参名
	
	
	可变长度的关键字参数（）
	**形参名（通常为*kwargs）:用来接收溢出的关键字实参,溢出的位置实参会被*保存成字典的格式，然后赋值给形参名


	*可以用在实参中（类似for循环），实参中带*，先将*后的值解构成位置实参
```python
	*[1,2,3]  => 1, 2, 3
	```
	*or**：实参拆，形参封。

	**可以用在实参中（**后面跟的只能是字典），实参中带**，先将**后的值解构成关键字实参
```python
	**{'x':1,'y':2}  => x=1,y=2
	```

	形参组合使用：位置形参，默认形参，*args，命名关键字形参，**kwargs
```python
def func(x,y=111,*args,z,**kwargs)
```


### 名称空间
	内置名称空间，全局名称空间，局部名称空间（都在栈区里）
	作用域：内置名称空间和全局名称空间的作用范围称为全局域，局部名称空间的作用范围称为局部域


##### 内置名称空间
	存放python解释器内置的名字：print，input……

##### 全局名称空间
	运行顶级代码所产生的名字，即非函数内定义、也不是内置的，剩下即为全局名称空间

##### 局部名称空间
	在调用函数时，运行函数体代码过程中产生的函数内的名字

``名字的查找优先级：当前所在位置向上一层一层找，不会往下
``局部->全局->内置
``函数内名字的查找是以定义阶段为准

#### 测试案例
```python
input = 333
def func():
	input=444

func()
print(input)
#结果：333


def func() 
	print(x)

x=111
func() #运行到这全局已有名字
#结果：111


#名称空间的“嵌套”关系是以函数 定义阶段 为准，与调用位置无关
x=1
def func(): #定义阶段为局部，局部没有，找全局
	print(x)

def foo():
	x=222
	func()
foo()
#结果：1


a=111
def f1():
	def f2(): #
		print(a)
	a=222
	f2()
#结果：222
```


### 作用域
##### 全局作用域:内置名称空间，全局名称空间
	全局存活
	全局有效:被所有函数共享
##### 局部作用域：局部名称空间
	临时存活
	局部有效：函数内有效

	函数内： 
	global 变量名（不可变类型）：即局部变量变成全局变量
	可变类型操作就是全局作用。

	nonlocal 变量名（不可变类型）：修改函数外层函数包含的名字对于的值


### 闭包函数（即名称空间与作用域+函数嵌套+函数对象）
	核心点：名字的查找关系是以函数定义阶段为准

#### 什么是闭包函数
	“闭”函数指的是该函数是内嵌函数
	“包”函数指的是该函数包含对外层函数作用域名字的引用（不是全局作用域）
![[Pasted image 20220720134513.png]]

**将内嵌函数丢到全局** **感觉像是局部变全局**

#### 闭包函数的应用
	两种为函数体传参的方式
	方式一：直接把函数体需要的参数定义成形参
```python
def f2(x):
	print(x)
f2(3) #调用
```
	方式二：通过闭包传参（当一个函数需要参数但又不能直接传入参数时）
```python
def f1(x):
	
	def f2():
		print(x)
	return f2
q=f1(3) 
q() #调用 跟上面结果一样
		
```



### 装饰器（多个装饰器加载顺序自下而上，执行顺序自上而下  ）
#### 什么是装饰器
	“装饰”指的是为其他事物添加额外的东西点缀，
	“器”指工具，可以定义成函数。
	即装饰器指的定义一个函数，该函数是用来为其他函数增加额外的功能
	

#### 为何要用装饰器
	开放封闭原则
		开放：指的是对拓展功能是开放的
		封闭：指的是对修改源代码是封闭的
		
	在不修改源代码并且不改变调用方式的基础上，为一个函数拓展新功能的工具。

	-装饰器简单实现
```python
'''
给index功能新增一个计时功能
'''
from functools import wraps
import time

def timmer(func):

    # func
	@wraps(func)
    def warpper(*args,**kwargs):

        start=time.time()

        res = func(*args,**kwargs)  #func=index  

        stop=time.time()

        print(stop-start)

        return res  #和func返回值保持一致

    return warpper

# （注意装饰器要先定义）在被装饰对象正上方单独写一行@装饰器名字 等价于 偷梁换柱

@timmer #index=timmer(index)
def index(x,y):

    time.sleep(3)

    print("index %s %s " %(x,y))

# 偷梁换柱
#index=timmer(index)

index(1,2)
```

	
	总结无参装饰器模板
```python
from functools import wraps

# 由于语法糖@的限制，outter函数只能有一个参数，并且该参数用来接收被装饰对象的内存地址 
def outter(func):
	# 将原函数的属性赋值给wrapper函数
	@wraps(func)
	def wrapper(*args,**kwargs):
		# 1、调用原函数
		# 2、为其增加新功能
		res=func(*args,**kwargs) 
		return res
	# 赋值属性手动本来在这
	return wrapper

@outter  # 等价于 index = outter(index)
def index():
	print('from index')
```
	


	有参装饰器
```python
def 有参装饰器(db_type):
	def outter(func):
		# 将原函数的属性赋值给wrapper函数
		@wraps(func)
		def wrapper(*args,**kwargs):
			if db_type='file':
				# 1、调用原函数
				# 2、为其增加新功能
				res=func(*args,**kwargs) 
				return res
			else:
				pass
		# 赋值属性手动本来在这
		return wrapper
	return outter
	
@有参装饰器(db_type='file') #先成@outter  即变成上述无参装饰器
def index():
	print('from index')
```



## 迭代器
	即迭代取值的工具，迭代是一个重复的过程，每次重复的过程都是基于上一次的结果而继续的，单纯的重复并不是迭代（如while true不是迭代）
	迭代器可以不依赖索引迭代取值

```
可迭代对象：但凡内置有__iter__方法的都称之为可迭代的对象
调用可迭代对象下的__iter__方法会将其转换成迭代器对象
调用__next__方法就是取迭代器下一个值，超出范围则抛出异常  
在同一个迭代器取值完后，再取值会取不到
```
	可迭代对象（可以转换为迭代器的对象）：内置有__iter__方法
						可迭代对象.__iter__()得到迭代器对象
						
	迭代器对象：内置有__next__方法并且内置有__iter__方法
						迭代器对象.__next__()方法得到迭代器的下一个值
						迭代器对象.__iter__()方法得到迭代器本身（即调没调一样）,这是为了for循环统一


``打开的文件是迭代器对象``



### for 循环工作原理 for循环可以称为迭代器循环
```python
l = [1,2,3,4,5]
#1、l.__iter__()得到一个迭代器对象
#2、迭代器对象.__next__()拿到一个返回值，然后将该返回值赋值给k
#3、循环反复步骤2，直到抛出异常StopIteration异常for循环会捕捉异常然后结束循环
for k in l:
	print(k)


#换成while循环理解
l.iterator = l.__iter__()  #=》iter(l)
while True:
	try:
		print(l.iterator.__next__())  #=>next(l.iterator)
	except StopIteration:
		break
```


### 自定义迭代器（生成器）
	生成器就是迭代器
```python

def func():
	print('第一次')
	yield 1
	print('第二次')
	yield 2
	print('第三次')
	yield 3
	print('第四次')
g = func()

# 会触发函数体代码的运行，然后遇到yield 停下来，将yield后的值当作本次调用的结果返回
# g.__next__()  #得到 第一次
res = g.__next__()
print(res) #第一次 
		   #1
```

	生成器.send(值)  : 可以赋值
	g.close()  关闭之后无法传值



### 三元表达式
```python
def func():
	if x>y:
		return x
	else:
		return y 

# 三元表达式
res = x if x > y else y
```


### 列表生成式（[]改成（）就是生成器表达式，可防止内存爆炸）
```python
l = ['tony_dsb','jany_dsb','jack']
new_l=[]
for name in l:
	if name.endswith('dsb'):
		new_l.append(name)

# 等价于
new_l=[name for name in l if name.endswith('dsb')]
```


### 函数的递归调用：函数嵌套调用的一种特殊形式
	本质就是循环
	默认限制一千层


	递推：一层层调用下去
	回溯：满足某种结束条件，结束递归调用，然后一层一层返回



## 匿名函数
```python
# def定义有名函数
def func(x,y):
	return x+y

# lambda定义匿名函数
lambda x,y:x+y
# 调用匿名函数
# 方式一:
res=(lambda x,y:x+y)(1,2)

# 方式二:
func=lambda x,y:x+y
func(1,2)

# 以上都不是匿名，匿名函数定义时由于没绑定内存地址，所以都是临时用一次，然后回收，更多是将匿名函数与其他函数配合使用


```

### map
```python
l=['tom','jack','jerry']
new_l(name+'_dsb' for name in l)
# 等价于
# map(函数,可迭代对象)
map(lambda name:name+'_dsb',l) #得到生成器
```


### filter(过滤)
```python
l=['tom_dsb','jack_dsb','jerry']
res=(name for name in l if name.endswith('dsb'))
# 等价于
# filter(函数,可迭代对象)
filter(lambda name:name.endwith('dsb'),l)  #得到生成器

```


### reduce(了解，python3 不是内置函数了)
```python
from functools import reduce
reduce(lambda x,y:x+y,[1,2,3],10)
```


## 模块
#### 首次导入模块（如foo）会发生（无先后顺序）
	执行该文件foo.py
	产生文件foo.py的名称空间，会将foo.py运行过程中产生的名字放在名称空间里
	在当前文件里产生的有一个名字foo，该名字指向2中产生的名称空间
	之后再导入，都是直接引用首次产生的foo.py的名称空间，不会重复执行代码

	变量foo.x与当前文件中变量x不冲突


	无论是查看还是修改都是以原模块为基准的，以调用位置无关(foo.x如果不修改则不会变动)


