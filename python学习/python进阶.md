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

	规范形参，形参后面加数据类型 如：name:str,age:int=18    表示age默认18
	def func()->int:  指返回的是整型类型

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

	nonlocal 变量名（不可变类型）：修改外部嵌套函数内的变量包含的名字对于的值
	nonlocal声名此变量与外层同名变量为相同的变量。


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
	@wraps(func) # 防止重写了我们函数的名字和注释文档
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


## 模块（包的本质是模块的一种形式，导包运行__init__.py文件）
#### 首次导入模块（如foo）会发生（无先后顺序）
	执行该文件foo.py
	产生文件foo.py的名称空间，会将foo.py运行过程中产生的名字放在名称空间里
	在当前文件里产生的有一个名字foo，该名字指向2中产生的名称空间
	(若是from …… import ……,则会在该全局名称空间产生导入的变量名)
	之后再导入，都是直接引用首次产生的foo.py的名称空间，不会重复执行代码

	变量foo.x与当前文件中变量x不冲突


	无论是查看还是修改都是以原模块为基准的，以调用位置无关(foo.x如果不修改则不会变动)


```
当foo.py被运行时，__name__的值为"__main__"
当foo.py被当作模块导入时，__name__的值为"foo"
所以
if __name__=='__main__':
```

### 软件开发规范
	ATM
		bin(启动文件)(或run.py)
			start.py
		conf(配置文件)
			settings.py  
		db
			db_handle.py
		lib(常用的自定义模块)
			common.py
		core(核心代码的逻辑)
			src.py
		setup.py(安装、部署、打包的脚本)
		requirements.txt(存放软件以来的外部Python包列表)
		README(项目说明文件)

- 相对导入仅限于包内使用
- 软件开发时，从别的文件夹导模块时，可以先`sys.path.append(r'路径')`将顶级目录路径加入python执行搜索路径中再导入别的文件夹的模块
- 直接加路径会写死
- \_\_file\_\_  会得到当前文件的绝对路径
- 所以在执行文件（bin下start.py）里
```python
import os
BASE_DIR=os.path.dirname(os.path.dirname(__file__))

# 上述python2，3都能用

# 下面最低python3.5
from pathlib import Path
root=Path(__file__)
BASE_DIR=root.parent.parent


import sys
sys.path.append(BASE_DIR)
```

```
在（非bin）其他文件夹导入其他文件夹：
路径放入settings里,路径导入也如上
```



## 常用模块

### time
	时间分为三种模式：
	1、时间戳：从1970年到现在经过的秒数
		time.time()
		作用：用于时间间隔的计算

	2、按照某种格式显示的时间：
		time.strftime('%Y-%m-%d %H:%M:%S %p')
									2022-07-29 16:39:01 PM
		作用：用于展示时间

	3、结构化时间
		time.localtime()  # 可以获取年份，月份等
		作用：用于单独获取时间的某一部分


### datetime
	datetime.now() # 类似与上述2的显示时间
	datetime.timedelta(days=3) #与now()+结合就是三天后的时间


### time和datetime使用掌握的操作
	时间格式的转换
```python
#时间戳转结构化
import time
tm=time.time()
time.localtime(tm)
# 或datetime.datetime.fromtimestamp(tm)

#结构化转格式化
tm=time.localtime()
time.strftime('%Y-%m-%d %H:%M:%S',tm)

#格式化转结构化
time.striptime('2022-07-29 16:39:01','%Y-%m-%d %H:%M:%S')

#真正需要掌握的
#格式化和时间戳
struct_time=time.strptime('2022-07-29 16:39:01','%Y-%m-%d %H:%M:%S') #先转结构化
timestamp=time.mktime(struct_time) #结构化转时间戳
timestamp+=7*86400 #加7天
time.strftime('%Y-%m-%d %H:%M:%S',time.localtime(timestamp)) #时间戳转会格式化
```



### random
	random.random() # (0,1)
	random.randint(1,3)  # [1,3]
	random.randrange(1,3) # [1,3)
	random.choise([1,'23',[4,5]]) # 1或者23或者[4,5]
	random.sample([1,'23',[4,5]],i) # 列表元素任意取i个值
	random.uniform(1,3) # 大于1小于3的小数
	random.shuffle(item) # 洗乱item的顺序，相当于洗牌


### os
	os.path.getsize() 获取文件大小
	os.remove() 删除文件
	os.rename() 重命名文件
	os.system() 运行shell命令
	os.environ['']=''
	os.path.dirname()  路径
	os.path.basename()   文件名
	os.path.isfile   是否是文件
	os.path.isdir    是否是文件夹



### 打印进度条
```python
'[%-50s]'    # 为%s传值，-左对齐，50固定宽度

import time

  
recv_size=0

total_size=33333

while recv_size < total_size:

    time.sleep(0.2)

    recv_size += 1024

    # 打印进度条

    percent=recv_size / total_size

    if percent > 1:

        percent = 1  

    res=int(50*percent) * '#'

    for i in range(50):

        print('\r[%-50s] %d%%' % (res,int(100*percent)),end='')   # \r表示不换行,在行首开始

                #%s是传参，-是左对齐，50是固定宽度
```



### json&pickle (注，json不识别python对象，即类的实例化)
	序列化
		内存中的数据类型-->序列化-->特定的格式（json或pickle）
	反序列化
		内存中的数据类型<--序列化<--特定的格式（json或pickle）

	用于存档：可是一种专用的格式=》pickle只有python可以识别
	跨平台数据交互：应该是一种通用，能够被所有语言识别的格式=》json

```python
import json
res=json.dumps(需要序列化的数据) # 序列化
l=json.loads(res) # 反序列化
with open('test.json',mode='wt',encoding='utf-8') as f:
	f.write(res)
# 将序列化结果写入文件的简单写法
with open('test.json',mode='wt',encoding='utf-8') as f:
	json.dump(需要序列化的数据,f, ensure_ascii=False) # 中文格式
with open('test.json',mode='rt',encoding='utf-8') as f2:
	json_res=f.read()
	l=json.loads(json_res)
	# l=json.load(f)
```

	json格式兼容的是所有语言都通用的数据类型，不能识别某一语言所特有的数据类型，如python的集合。

```python
import pickle
res=pickle.dumps(需要序列化的数据) # 序列化成二进制
l=pickle.loads(res) # 反序列化
```


### 猴子补丁
	在入口文件可以进行猴子补丁
```python
import json
import ujson
def monkey_patch_json():
	json.__name__ = 'ujson'
	json.dumps = ujson.dumps
	json.loads = ujson.loads
monkey_patch_json()
```


### configparser
	加载某种特定格式的配置文件(ini,cfg)
```python
config=configparser.ConfigParser()
config.read('aaa.ini')
# 获取sections
config.sections()
# 获取sections下的options
config.options('某个section')
# 获取items
config.items('某个section')
# 获取item
config.get('某个section','某个item')
# 还有getint,getboolean,getfloat
```


### 哈希
	hash是一类算法，该算法接收传入的内容，经过运算得到一串hash值
	hash值的特点
		1、只要传入的值一样，hash值就是一样
		2、只要使用的hash算法不变，传入的内容无论多大，hash值长度都不变
		3、不能由hash值反解成内容（但是如果撞库就不一定了）

```python
# 用途一：特点三可以用于密文的传输和验证
import hashlib
m=hashlib.md5()
m.update('hello',encode('utf-8'))
m.update('world',encode('utf-8'))
res=m.hexdigest() # 'helloworld'
f=open('a.txt',mode='rb')
f.seek(某个位置 )
f.read(2000)  # 文件太大可以只检验部分
m.update()
# 用途二：特点一二可以用于文件完整性校验
```


## 日志
```python
import logging

logging.basicConfig(

                      format='%(asctime)s - %(name)s - %(levelname)s - %(module)s: %(message)s',

                       datefmt='%Y-%m-%d %H:%M:%S %p',

                       #不指定，默认打到终端

                       filename='mymmmmmmmm.log',

                       #既想终端，又想输出到文件

                       level=10, #日志级别

                       )

  
  

logging.debug('调试debug') #10

logging.info('消息info')   #20

logging.warning('警告warn')#30

logging.error('错误error') #40

logging.critical('严重critical')#50
```
### 日志中用到的格式化串如下
	%(name)s   Logger的名字
	%(levelno)s   数字形式的日志级别
	%(levelname)s  文本形式的日志级别
	%(pathname)s  调用日志输出函数的模块的完整路径名，可能没有
	%(filename)s  调用日志输出函数的模块的文件名
	%(module)s   调用日志输出函数的模块名
	%(funcName)s  调用日志输出函数的函数名
	%(lineno)d   调用日志输出函数的语句所在的代码行
	%(created)f   当前时间，用UNIX标准的表示时间的浮点数表示
	%(relativeCreated)d   输出日志信息时的，自Logger创建以来的毫秒数
	%(asctime)s   字符串形式的当前时间
	%(thread)d   线程ID。可能没有
	%(threadName)s   线程名，可能没有
	%(process)d     进程ID。可能没有
	%(message)s    用户输出的信息




```python
import os
import logging.config   #不能只导入logging
BASE_DIR=os.path.dirname(os.path.dirname(__file__))
# DB_PATH=r'%sdbdb.txt' %BASE_DIR
DB_PATH=r'%sdb' %BASE_DIR

# 定义日志文件的路径
LOG_PATH=r'%slogaccess.log' %BASE_DIR
BOSS_LOG_PATH=r'%slogboss.log' %BASE_DIR

# 定义三种日志输出格式 开始
standard_format = '[%(asctime)s][%(threadName)s:%(thread)d][task_id:%(name)s][%(filename)s:%(lineno)d]' 
                  '[%(levelname)s][%(message)s]' #其中name为getlogger指定的名字
simple_format = '[%(levelname)s][%(asctime)s][%(filename)s:%(lineno)d]%(message)s'

id_simple_format = '[%(levelname)s][%(asctime)s] %(message)s'
# 定义日志输出格式 结束
BASE_DIR=os.path.dirname(os.path.dirname(__file__))
logfile_dir = os.path.join(BASE_DIR, 'log')  # log文件的目录
logfile_name = 'all2.log'  # log文件名

# 如果不存在定义的日志目录就创建一个
if not os.path.isdir(logfile_dir):
    os.mkdir(logfile_dir)

# log文件的全路径
logfile_path = os.path.join(logfile_dir, logfile_name)

# log配置字典
LOGGING_DIC = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'standard': {
            'format': standard_format
        },
        'simple': {
            'format': simple_format
        },
        'id_simple': {
            'format': id_simple_format
        },
    },
    'filters': {},
    # handlers是日志的接收者，不同的handler会将日志输出到不同的位置
    'handlers': {
        #打印到终端的日志
        'stream': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',  # 打印到屏幕
            'formatter': 'simple'
        },
        #打印到文件的日志,收集info及以上的日志
        'access': {
            'level': 'DEBUG',
            'class': 'logging.handlers.RotatingFileHandler',  # 轮转保存到文件，若为FileHandler则只是普通保存到未见
            'formatter': 'standard',
            #可以定制日志文件路径
            # BASE_DIR = os.path.dirname(os.path.abspath(__file__))
            # LOG_PATH = os.path.join(BASE_DIR,'a1.loh')
            'filename': logfile_path,  # 日志文件
            'maxBytes': 1024*1024*5,  # 日志大小 5M
            'backupCount': 5,  #若为轮转，则表示最多保存几份文件，多则删除
            'encoding': 'utf-8',  # 日志文件的编码，再也不用担心中文log乱码了
        },
        #打印到文件的日志,收集error及以上的日志
        'boss': {
                    'level': 'ERROR',
                    'class': 'logging.handlers.RotatingFileHandler',  # 保存到文件
                    'formatter': 'id_simple',
                    'filename': BOSS_LOG_PATH,  # 日志文件
                    # 'maxBytes': 1024*1024*5,  # 日志大小 5M
                    'maxBytes': 300,  # 日志大小 5M
                    'backupCount': 5,
                    'encoding': 'utf-8',  # 日志文件的编码，再也不用担心中文log乱码了
                },
    },
    # loggers是日志的产生者，产生的日志会传递给handler然后控制输出
    'loggers': {
        #logging.getLogger(__name__)拿到的logger配置
        'aaa': {
            'handlers': ['stream', 'access','boss'],  # 这里把上面定义的两个handler都加上，即log数据既写入文件又打印到屏幕
            'level': 'DEBUG',
            'propagate': False,  #默认为True， 向上（更高level的logger）传递
        },
        'bbb': {
            'handlers': ['stream', 'access','boss'],  # 这里把上面定义的两个handler都加上，即log数据既写入文件又打印到屏幕
            'level': 'DEBUG',
            'propagate': False,  #默认为True， 向上（更高level的logger）传递
        },
        # 若为空,logger若找不到其他logger的话可以取任意名
        '': {
            'handlers': ['stream', 'access','boss'],  # 这里把上面定义的两个handler都加上，即log数据既写入文件又打印到屏幕
            'level': 'DEBUG',
            'propagate': False,  #默认为True， 向上（更高level的logger）传递
        },
        #这样我们再取logger对象时logging.getLogger(__name__)，不同的文件__name__不同，这保证了打印日志时标识信息不同，
        # 但是拿着该名字去loggers里找key名时却发现找不到，于是默认使用key=''的配置
    },
}

def load_my_logging_cfg():
    logging.config.dictConfig(LOGGING_DIC)  # 导入上面定义的logging配置
    logger = logging.getLogger(__name__)  # 生成一个log实例
    logger.info('It works!')  # 记录该文件的运行状态

if __name__ == '__main__':
    load_my_logging_cfg()
```

```python
# 在别的文件拿到日志的产生者即loggers如aaa，bbb
import 模块
from logging import config,getLogger #(不能import logging  ,logging.config,因为config是logging的一个子文件夹)
# 也可以import logging.config
config.dictConfig(模块.LOGGING_DIC)
logger1=getLogger('aaa')
# 产生日志
logger1.info('这是一条info日志')
```


### 日志名的命名
	就是那个配置文件里的loggers
	根据使用需求去命名，如用户交易，终端提示，测试
	若loggers中有个为空,则可以起不存在的loggers外任意名。


### 日志轮转
```python
	'class': 'logging.handlers.RotatingFileHandler',  # 轮转保存到文件，若为FileHandler则只是普通保存到未见
    'formatter': 'standard',
	'maxBytes': 1024*1024*5,  # 日志大小 5M
    'backupCount': 5,  #若为轮转，则表示最多保存几份文件，多则删除
```
