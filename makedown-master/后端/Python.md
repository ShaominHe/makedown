# Python

## 一、python语法

在其他语言中，使用括号来标示代码块。而在python中使用缩进来表示

## 二、注释

python中使用#号开头，#号后面的为注释。多行注释，可以为每一行添加#，也可以使用"""。用三引号包裹

```
单行注释
#print("Hello, World!")
多行注释
#This is a comment
#written in
#more than just one line
或者
"""
This is a comment
written in 
more than just one line
"""
```

## 三、python的变量

### 变量

python的变量不需要声明，首次为其赋值时，就会创建变量。

```
x = 10
y = "Bill"
print(x)
print(y)
```

变量不需要使用任何特定类型声明，甚至可以在设置后更改其类型

Python变量命名规则：

​	 变量名必须以字母或下划线字符开头 

 	变量名称不能以数字开头 

​	 变量名只能包含字母数字字符和下划线（A-z、0-9 和 _） 

​	 变量名称区分大小写（age、Age 和 AGE 是三个不同的变量） 

Python中允许在一行中为多个变量赋值

```
x, y, z = "Orange", "Banana", "Cherry"
print(x)
print(y)
print(z)
```

也可以在一行中为多个变量分配相同的值

```
x = y = z = "Orange"
print(x)
print(y)
print(z)
```

### 输出语句

print()函数可以输出语句

type()函数可以查看数据的类型

### 全局变量

使用 global  关键字定义全局变量

## 四、Python的数据类型

| 名称   | 类型       | 说明                                           |
| ------ | ---------- | ---------------------------------------------- |
| 整数   | int        | 不带小数点的数：5、16、-24、0                  |
| 小数   | float      | 带小数点的数：3.14、-6.7、4.0                  |
| 字符串 | string     | 有序的字符序列："你好"、'Hello'、"100"、‘3.14’ |
| 布尔值 | bool       | 用于逻辑运算的类型，值只有True或者False        |
| 列表   | list       | 有序序列：[100，"Hello"，-12.5，100]           |
| 字典   | dictionary | 无序的键值对：{"小明":"66分", "小红":"23分"}   |
| 元组   | tuple      | 有序且不可变序列：(100, "Hello", -12.5, 100)   |
| 集合   | set        | 无序且无重复元素：{"A", "B", "C"}              |

