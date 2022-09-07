## 面向过程
	 核心是"过程"二字

	 过程的核心思想是将程序流程化
	 过程是流水线，用来分步骤解决问题


## 面向对象
	核心是"对象"二字

	对象的核心思想就是将程序"整合"，
	对象是"容器"，用来盛放数据与功能的

	类也是"容器"，该容器用来存放同类对象共有的数据与功能

### 程序 = 数据 + 功能
	 学生的容器 = 学生的数据 + 学生的功能
	 课程的容器 = 课程的数据 + 课程的功能


## 类
	先定义类
	再调用类产生对象
```python
# 类是对象相似数据与功能的集合体
# 所以类中最常见的是变量与函数的定义，但是类体可以包含任意其他代码
# 类体代码是在类定义阶段就会立即执行
# 类的名称空间是局部
class Student:
	# 1、变量的定义
	stu_school = 'oldboy'
	count = 0
	def __init__(self, x, y, z):
		Student.count += 1 # 设置成self的话是对象的属性，不是类的属性
		self.stu_name = x
		self.stu_age = y
		self.stu_gender = z
	# 2、功能的定义
	def tell(self):
		print('%s' %self.stu_name)
		pass
	def set_info(self,x):
		self.stu_name = x
print(Student.stu_school)  # 等价于print(Student.__dict__['stu_school'])



# 产生对象(类的实例化)
stu1_obj = Student('gavin','18','male') # 不会执行类体代码,但会调用Student.__init__()方法
stu2_obj = Student('bin','19','male')

Student.tell(stu1_dent) # 调用类中tell方法 #gavin
stu1_obj.tell() # 通过对象调用tell方法更简单直观，会将对象当做第一个参数传入tell方法，所以在类中设计函数时，要注意参数第一个要用来接收绑定对象
```

	类的数据属性是共享给所有对象用的
	改类中的数据属性，对象的数据属性跟着变，因为对象的数据属性内存地址指向类中的数据属性
	改其中一个对象的数据属性，类中的数据属性不会变


	类的函数属性是绑定给对象用的，虽然所以对象指向的都是相同的功能，但是绑定到不同的对象就是不同的绑定方法，内存地址各不相同。
	
``Student.tell(stu1_dent) # 等价于
``stu1_obj.tell()


## 封装
封装是面向对象三大特性最核心的一个特性-->整合

将封装的属性进行隐藏操作
	在属性名前加__前缀，就会实现一个对外隐藏属性效果
```python
class Foo:
	__x = 1 # _Foo__x

	def __f1(self): # _Foo__f1
		print('from test')

	def f2(self):
		print(self.__x) # print(self._Foo__x)
		print(self.__f1) #print(self._Foo__f1)
Foo.__y = 3 # 这个不会变形还是__y
```
在类外部无法直接访问双下划线开头的，但知道了类名和属性名就可以

这种隐藏对外不对内，因为__开头的属性会在检查类体代码语法时统一发生一次变形

### 为何要隐藏属性
	隐藏数据属性
		使用者无法直接访问，需通过类体的接口去访问数据属性
		在接口可以加自己的逻辑控制该接口操作

	隐藏函数属性/方法属性：目的是为了隔离复杂度


### property 装饰器

```python
# property 是一个装饰器，是用来将绑定给对象的方法伪造成一个数据属性

  
# 简单模板
class People1:

    def __init__(self, name, weight, height):

        self.name = name

        self.weight = weight

        self.height = height

    # bmi 是通过触发功能计算得到的

    # bmi 是随身高体重变化而动态变化的

    # 但是bmi听起来更像一个数据属性，而非功能

    @property

    def bmi(self):

        return self.weight / (self.height ** 2)

  

obj1 = People1('gavin', 58, 1.71)

print(obj1.bmi)

  
# 第一种实现
class People2:

    def __init__(self,name) -> None:

        self.__name = name

  

    def get_name(self):

        return self.__name

  

    def set_name(self, val):

        if type(val) is not str:

            print('必须传入str类型')

            return

        self.__name = val

  

    def del_name(self):

        print('不让删除')

  

    name = property(get_name, set_name, del_name)

obj1 = People2('gavin')

print(obj1.name)    # 如果name = property(del_name, set_name, get_name),会报错

obj1.name = 'Gavin'

print(obj1.name)

del obj1.name  

  
  
# 第二种实现
class People2:

    def __init__(self,name) -> None:

        self.__name = name

  

    @property # name = property(name)

    def name(self):

        return self.__name

  

    @name.setter

    def name(self, val):

        if type(val) is not str:

            print('必须传入str类型')

            return

        self.__name = val

  

    @name.deleter

    def name(self):

        print('不让删除')

  

obj1 = People2('gavin')

print(obj1.name)  

obj1.name = 'Gavin'

print(obj1.name)

del obj1.name
```



## 继承
	继承是一种创建新类的方式，新建的类可称为子类或派生类，父类又可称为基类或超类,子类会遗传弗雷的属性
	需要注意的是：python支持多继承

	解决类与类之间代码冗余问题
```python
  

class Parent1: # 没有继承任何类，会默认继承 object 类，所以python3中所有类都是新式类，没有经典类
	def __init__(self,name,age):
		self.name=name
		self,age=age
    pass
 

class Parent2:

    pass


class Sub1(Parent1): # 单继承

    pass
 

class Sub2(Parent1, Parent2): # 多继承
	def __init__(self,name,age,salary):
		Parent1.__init__(self, name, age) # 这个可以不依赖于继承
		super().__init__(name, age) # python2需super(Sub2, self)，严格依赖继承
		self.salary=salary

    pass


print(Sub1.__base__) # 查看继承的类

print(Sub2.__base__) # 查看继承的类
```
	调用super()会得到一个特殊的对象，所以不用传self，该对象会参照发起属性查找类的mro，去当前类的父类(下一个类)中去找属性，严格依赖继承
```PYTHON
class A:
	def test(self):
		print('from A')
		super().test()

class B:
	def test(self):
		print('from B')

class C(A, B):
	pass

obj = C()
obj.test() 
# from A
# from B
```

	优点：子类可以同时遗传多个父类的属性，最大限度的重用代码
	缺点：违背人的思维习惯
			代码可读性会变差
			不建议使用，有可能会引发可恶的菱形问题，拓展性变差
	如果真的涉及一个子类不可避免地要重用多个父类的属性，应该使用Mixins机制

### 菱形问题
	最终继承到一个非object类上面
	属性访问顺序参照MRO列表，可以通过类调用.mro()方法查看列表
	python3新式类是类似广度优先搜索（并非广度优先），找完所有分支（分支上的也会继续找下去），最后再去找最终继承的那个非object类

### 非菱形
	非菱形属性访问顺序是深度优先搜索，object是最后查找的
	经典类也是深度优先搜索


### mixins机制
	核心：在多继承背景下尽可能提升多继承的可读性
	满足人的思维习惯

	在类名后加上Mixin，able，ible，表示这个类不是子类的真正父类，只是为了加功能进去而创造的类


## 多态
	同一种事物有多种形态
	多态性指的是可以在不考虑对象具体类型的情况下而直接使用对象
```PYTHON
import abc

  

class Amimal(metaclass=abc.ABCMeta):   # 抽象基类，统一所有子类的标准

    @abc.abstractclassmethod  # 指示抽象方法的装饰器

    def say(self):

        pass

# obj = Amimal() 不能实例化抽象化基类 

class People(Amimal):

    def say(self):

        pass

  

obj = People() # 若People类没定义say方法会报错
```

	推崇鸭子类型：
			即不使用多态的继承实现多态，比如统一常用的函数名等
```python
class People:
	def say(self):
		pass

class Dog:
	def say(self):
		pass

class Pig:
	def say(self):
		pass
```


## 绑定方法：特殊之处在于调用者本身当做第一个参数自动传入
	绑定给对象的方法：调用者是对象，自动传入的是对象
	绑定给类的方法：调用者是类，自动传入的是类
```python
@classmethod # 将下面的函数装饰成绑定给类的方法
def aaa(cls):
	return cls(传cls类的参)
```


## 非绑定方法 (静态方法)
```python
@staticmethod # 将下述函数装饰成一个静态方法
def add(a,b):
	return a+b
```



# 反射
	指的是在程序运行过程中可以“动态”获取对象的信息
```python
class People:
	def __init__(self, name, age):
		self.name = name
		self.age = age

	def say(self):
		print('<%s:%s>' %(self.name, self.age))

obj = People('辣白菜', 18)


# 实现反射机制的步骤
# 1 先通过dir，查看某一个对象下可以.出哪些属性来
print(dir(obj))

# 2 可以通过字符串反射到真正的属性上，得到属性值
print(obj.__dict__[dir(obj)[-2]])  # 辣白菜


# 四个内置函数的使用:通过字符串来操作属性值
print(hasattr(obj, 'name'))  # True
	
print(getattr(obj, 'name')) # 辣白菜

setattr(obj, 'name', 'gavin')  
print(obj.name) # gavin

delattr(obj, 'name')

getattr(obj,'say')()# 也可通过字符串操作方法
```


# 内置方法
	定义在类内部，以__开头__结尾的方法
	会在某种情况下自动触发执行  
```python
# __str__ # 打印对象什么都不加的时候就会触发
	# 用来打印对象信息
# __del__ # 在删除对象时触发，会先执行该方法
	# 发起系统调用，告诉操作系统回收相关的系统资源
```


# 元类
	就是用来实例化产生类的类
	元类----实例化----》类----实例化----》对象

	内置的元类是type
	class关键字定义的所有的类以及内置的类都是由type实例化产生的


	class关键字创造People类的步骤
```python
# 1、类名
class_name = 'People'
# 2、类的基类
class_bases = (object, )
#  3、执行类体代码拿到类的名称空间
class_dic = {}
class_body = """
	def __init__(self, name, age):
		self.name = name
		self.age = age

	def say(self):
		print('<%s:%s>' %(self.name, self.age))"""
exec(class_body, {}, class_dic)  # 类体代码，全局名称空间，类体名称空间

# 4、调用元类 
People = type(class_name, class_bases, class_dic)
```


```python
class People(metaclass=type):
	pass

```


## 自定义元类
```python
class Mymeta(type): # 只有继承了type类的类才是元类
	#           空对象,class_name, class_bases, class_dic
	def __init__(self,class_name, class_bases, class_dic):
		if not class_name.istitle():
			raise NameError('类名首字母必须大写')
	#         当前所在类，调用类时所传入的参数
	def __new__(cls, *args, **kwargs):
		# 造Mymeta的对象
		return super().__new__(cls, *args, **kwargs)
		# return type.__new__(cls, *args, **kwargs) # 两种方式均可

	def __call__(self, *args, **kwargs):
		people_obj = self.__new__(self)

		self.__init__(people_obj, *args, **kwargs)

		return people_obj



```

	调用Mymeta发生三件事,调用Mymeta就是type.__call__
	先造一个空对象=》People，调用类内的__new__方法
	调用Mymeta这个类内的__init__方法，完成初始化对象的操作
	返回初始化好的对象

	   __call__
```python
class People(metaclass=Mymeta):
	
	def __init__(self, name, age):
		self.name = name
		self.age = age

	def say(self):
		print('%s:%s' %(self.name, self.zge))
		
	def __new__(cls, *args, **kwargs):
		# 产生真正对象
		return object.__new__(cls)

	# 如果想让一个对象可以加括号调用，需要在该对象的类中添加一个方法__call__
```


# 异常
### 语法错误SyntaxError
	方式一、必须在程序运行前就改正	


### 逻辑错误
	错误发生的条件是可以预知的
		逻辑判断

	错误发生的条件是无法预知的
```python
try:
	子代码块 # 有可能抛出异常的代码,抛出异常后，该行代码同级别的后续子代码不会再运行
except 异常类型1 as e:
	pass
except 异常类型2 as e:
	pass
# 若处理异常方式相同，则
except (异常类型1, 异常类型2) as e:
	pass
except Exception as e: # 万能异常
	pass
...
else:
	如果被检测的子代码块没有异常发生，则会执行else的子代码(不能单独与try使用,必须搭配except)
finally:
	无论被检测的子代码块有无异常发生，都会执行finally的子代码(不会处理异常，可以单独与try使用)
```


