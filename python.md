## numpy基础
点积：np.dat(a,b) 。a,b为ndarray，就是计算a和b的内积，如果a和b符合矩阵乘积，则直接乘，不满足则将b取转置再乘,都不符合就报错。


## Function
在输入参数时，可以用*args和**kwargs来表示任意数量的位置参数和关键字参数。其区别就是前者是以元组的形式传入(只有值)，后者是以字典的形式传入(有键和值)。
### filter()
### map()
### reduce()
### lambda 



## Class

在Class中，变量分为实例变量(Instance Variable)和类变量(Class Variable)。实例变量属于对象，类变量属于类。实例变量可以被对象自身访问和修改并不影响其他对象，但是类变量可以被所有对象共享，修改会影响所有对象。

Class中的特殊方法为构造函数__init__(),析构函数__del__(),属性访问器__getattr__(),属性设置器__setattr__(),方法调用器__call__().其中构造函数__init__()是类实例化时调用的第一个方法，会在实例化过程自动执行，析构函数__del__()是类实例化后对象被垃圾回收时调用的最后一个方法。
在Class中，一般方法(函数)分为实例方法(Instance Method)和类方法(Class Method)还有静态方法(Static Method)。不加任何装饰的是实例方法，实例方法的第一个参数是隐性传递的实例化对象self，加了@classmethod装饰器的是类方法，类方法的第一个参数是隐性传递的类cls，加了@staticmethod装饰的的是静态方法，静态方法没有隐性传递。

```python
class MyClass:
    # 实例方法
    def instance_foo(self, arg):
        print(self, arg)
    
    # 类方法
    @classmethod
    def class_foo(cls, arg):
        print(cls, arg)

    # 静态方法
    @staticmethod
    def static_foo(arg):
        print(arg)

################### 类调用 ###################

# 会报错，报错的原因是实例方法通过类直接调用时，就是一个普通的函数，不发生隐性传递
# 此时，instance_foo函数需要2个参数，而只传入一个参数
MyClass.instance_foo("实例方法")  

# output: <class '__main__.MyClass'> 类方法
# MyClass作为cls参数被隐性传递
MyClass.class_foo("类方法")   

# output: 静态方法
# 静态方法既可以通过类调用，也可以通过实例化对象调用，且没有隐性传递
MyClass.static_foo("静态方法")


################### 实例化对象调用 ###################
mycls = MyClass()

# output: <__main__.MyClass object at 0x00000212144957C0> 实例方法
# 实例方法的一般用法
mycls.instance_foo("实例方法")

# output: <class '__main__.MyClass'> 类方法
# 类方法也可以通过实例化对象调用，程序会自动找到对应的类，然后隐性传递为cls
mycls.class_foo("类方法")

# output: 静态方法
# 静态方法既可以通过类调用，也可以通过实例化对象调用，且没有隐性传递
mycls.static_foo("静态方法")
```
- 实例方法通过类调用时，只是一个普通的函数，通过实例化对象调用是实例化方法的一般使用情况，此时实例化对象通过隐性传递为self参数；
- 类方法既可以通过类调用，也可以通过实例化对象调用，两者没什么本质区别，最终类会被隐性传递为cls参数；
- 静态方法既可以通过类调用，也可以通过实例化对象调用，两者没什么本质区别，且不发生隐性调用。
```python
class MyClass:
    a = "a"     # 类属性

    # 实例方法
    def instance_foo(self, arg):
        print(arg, self.a)
    
    # 类方法
    @classmethod
    def class_foo(cls, arg):
        print(arg, cls.a)

MyClass.class_foo("类方法")      # output: 类方法 a

mycls = MyClass()               # 实例化过程中，类属性复制给实例属性，即self.a = "a"
mycls.a = "b"                   # 改变实例属性
mycls.instance_foo("实例方法")   # output: 实例方法 b

MyClass.class_foo("类方法")      # 虽然上面改变了实例属性，但是不影响类属性
                                 # output: 类方法 a

```

#### 类的继承：
子类可以访问父类中定义的所有属性和方法，包括私有属性和方法。子类可以重写父类中的方法，也可以添加新的方法和属性。子类可以通过super()函数调用父类的方法。在实例化子类时，优先调用子类的构造函数，如果没有定义构造函数，则调用父类的构造函数。如果子类和父类都定义了构造函数，则子类的构造函数会覆盖父类的构造函数。

```python
class Parent:        # 父类
    def __init__(self):
        self.parent_attr = "父类属性"
        self._parent_private_attr = "父类私有属性"
        print("父类构造函数")

    def parent_method(self):
        print("父类方法")

    def _parent_private_method(self):
        print("父类私有方法")

class Child(Parent):  # 子类
    def __init__(self):
        #super().__init__()   # 调用父类的构造函数        
        self.child_attr = "子类属性"
        print("子类构造函数")

    def child_method(self):
        print("子类方法")

    def _child_private_method(self):
        print("子类私有方法")

################### 实例化对象调用 ###################

# 实例化子类
# output: 子类构造函数
child1 = Child()

```
但是对于多重继承，不同的父类有相同的函数，则按照解析顺序调用(Method Resolution Order, MRO)，即先调用最左边的父类，然后依次往右调用。
```python
class A:
    def foo(self):
        print("A.foo")    # 左边的父类
class B:
    def foo(self):
        print("B.foo")    # 右边的父类
class C(A,B):
    pass

################### 实例化对象调用 ###################

# output: A.foo
c = C()
c.foo()
```


## Polymorphism
多态是指一个对象可以有不同的形态，而表现出不同的行为。在Python中，多态主要体现在方法的重载和重写。
### 1.Duck Typing
在Python中，Duck Typing是指"如果它看起来像鸭子，走起路来像鸭子，那么它就是鸭子"。Duck Typing的特点是只需要检查对象是否有某个方法或属性即可，不需要强制类型转换。其强调接口而非类型，因此可以实现灵活的多态。

```python
# 鸭子类
class Duck:

    def quack(self):
        print("这鸭子正在嘎嘎叫")

    def feathers(self):
        print("这鸭子拥有白色和灰色的羽毛")

# 人类
class Person:

    def quack(self):
        print("这人正在模仿鸭子")

    def feathers(self):
        print("这人在地上拿起1根羽毛然后给其他人看")


# 函数/接口
def in_the_forest(duck):
    duck.quack()
    duck.feathers()


################### 实例化对象调用 ###################

donald = Duck()  # 创建一个Duck类的实例
john = Person()  # 创建一个Person类的实例
in_the_forest(donald)  # 调用函数，传入Duck的实例
in_the_forest(john)    # 调用函数，传入Person的实例
```

### 2. Operator Overloading
重载运算符（Operator Overloading），可以对一个类进行没有运算符的定义或重定义操作。

```python
class student:
    def __init__(self, m1,m2):
        self.m1 = m1
        self.m2 = m2
    def __add__(self, other):
        return student(self.m1 + other.m1, self.m2 + other.m2)
    def __gt__(self, other):
        return self.m1 + self.m2 > other.m1 + other.m2


################### 实例化对象调用 ###################
s1 = student(53,64)
s2 = student(35,56)
s3 = s1 + s2
# output：88 120
print(s3.m1, s3.m2) 

# output：s1 is better than s2
if s1 > s2:
    print("s1 is better than s2")
else:
    print("s2 is better than s1")

```

### 3. Method Overriding and Overloading
方法重写（Method Overriding）是指子类重新定义了父类的方法，使得子类对象调用该方法时，实际调用的是子类的方法。方法重载（Method Overloading）是指同一个类中，存在多个同名的方法，而这些方法的参数个数或参数类型不同。  _而Python 本身不支持传统意义上的方法重载，即通过不同的参数签名（参数个数和类型）来定义多个同名函数。 但是，我们可以通过一些技巧来实现类似的功能。例如，可以使用默认参数、可变参数（*args, **kwargs）或修饰器来实现功能上的相似性，从而在一定程度上模拟重载的效果。这些方法允许函数根据传入的参数不同而执行不同的逻辑，达到类似重载的目的。_
- overrriding
```python
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(self.name + " is eating")

class Dog(Animal):
    def __init__(self, name):
        super().__init__(name)

    def bark(self):
        print(self.name + " is barking")

    def eat(self):
        print(self.name + " is eating dog food")

################### 实例化对象调用 ###################

dog1 = Dog("Buddy")
# output: Buddy is eating dog food
dog1.eat()
# output: Buddy is barking  
dog1.bark() 
```

- overloading
```python
class Calculator:
    def add(self, a=None, b=None, c=None):
        return a + b + c
################### 实例化对象调用 ###################
calc = Calculator()
# output: 6
print(calc.add(1, 2, 3))
# output: 3
print(calc.add(1, 2))
```

## Iterator
- 可迭代对象： 对象中有__iter__ 方法
- 迭代器：对象中有__iter__ 和 __next__方法
  
迭代器（Iterator）是访问集合元素的一种方式。迭代器是一个对象，它实现了__iter__()方法，该方法返回一个迭代器对象，该对象实现了__next__()方法，该方法返回集合的下一个元素，直到没有元素时抛出StopIteration异常。

```python
class MyIterator:
    def __init__(self, data):
        self.data = data
        self.index = 0
    def __iter__(self):
        return self
    def __next__(self):
        if self.index < len(self.data):
            value = self.data[self.index]
            self.index += 1
            return value
        else:
            raise StopIteration
################### 实例化对象调用 ###################
my_list = [1, 2, 3, 4, 5]
my_iterator = MyIterator(my_list)
# output: 1 2 3 4 5
for item in my_iterator:
    print(item)
```

## Generator
生成器（Generator）是一种特殊的迭代器，它是一种更高级的迭代器，它可以自动生成值，而无需用户自己编写循环。生成器是一种特殊的迭代器，它使用yield语句来返回值，而不用返回一个列表。
```python
def my_generator(n):
    for i in range(n):
        yield i
#######################################
my_gen = my_generator(5)
# output: 0 1 2 3 4
for item in my_gen:
    print(item)
```
## Decorator
装饰器（Decorator）是一种特殊的函数，它可以用来修改其他函数的行为。装饰器是一种特殊的函数，它使用@符号来修饰其他函数，从而在不改变原函数的情况下，为其添加额外的功能。
```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function call")
        result = func(*args, **kwargs)
        print("After function call")
        return result
    return wrapper

@my_decorator
def my_function():
    print("This is my function")

#######################################

# output: Before function call
#        This is my function
#        After function call
my_function()
```