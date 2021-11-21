## 命名空间（Namespaces）

- 所谓的Namespaces就是一个名字的集合，每个名字都映射到一个对应的函数对象上去，可以用Python里面的`dict`来理解（事实上也是这么实现的）
  - 不同Namespaces里面可以有相同的变量名
  - 多个Namespaces可以同时存在
- 当我们启动Python解释器的时候，它会自动开辟一个Built-in Namespaces，里面放所有的内置函数名，比如`print()`，`type()`之类
- 当我们载入模块的时候，会创建Global Namespaces，存放模块下的变量名
- 当我们调用一个函数的时候会创建一个Local Namespaces，存放这个函数里面所有的变量名。值得一提的是，如果函数中还有函数，那么这个子函数也会创建自己的Namespaces

## 变量作用域(Variable Scope)

- 多个namespaces可以共存，但是不是程序的每一行能访问的Namespace都是一样的。
- :bulb:当我们引用一个变量名的时候，查找的顺序是：Local Namespace –> Global Namespaces –> Built-in Namespaces
  - 如果有嵌套函数，就从嵌套函数的Local Namespaces–>父函数的Local Namespaces



## 举个例子

- 不用`global`，`nonlocal`修饰的情况下

~~~python
a = 1
def outer_function():
    a = 2
    def nested_function():
        a = 3
        print(f"I am in Nested_function {a}")
    nested_function()
    print(f"I am in outer_function {a}")
outer_function()
print(f"I am not in outer_function {a}")

# I am in Nested_function 3
# I am in outer_function 2
# I am not in outer_function 1
~~~

- 使用`global`修改的情况下，可以看出我们修改了全局变量的`a`，

~~~python
a = 1
def outer_function():
    a = 2
    def nested_function():
        global a
        a = 3
        print(f"I am in Nested_function {a}")
    nested_function()
    print(f"I am in outer_function {a}")
outer_function()
print(f"I am not in outer_function {a}")

# I am in Nested_function 3
# I am in outer_function 2
# I am not in outer_function 3
~~~

- 使用`nonlocal`修改的情况下，可以看出我们修改的是父函数中的`a`，这也符合我们之前所说的查找顺序，这里是从嵌套函数的Local Namespaces–>父函数的Local Namespaces

~~~python
a = 1
def outer_function():
    a = 2
    def nested_function():
        nonlocal a
        a = 3
        print(f"I am in Nested_function {a}")
    nested_function()
    print(f"I am in outer_function {a}")
outer_function()
print(f"I am not in outer_function {a}")

# I am in Nested_function 3
# I am in outer_function 3
# I am not in outer_function 1
~~~

