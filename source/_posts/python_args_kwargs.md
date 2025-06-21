---
title: python中的args和kwargs
date: 2025-06-21 20:12:00
tags: python
---

简单来说，它们允许一个函数接收不定数量的参数。这在我们预先不知道会传递多少个参数给函数时非常有用。

-----

### `*args` (任意数量的位置参数)

`*args` 用于在一个函数中接收任意数量的**位置参数 (positional arguments)**。当你在函数定义中使用 `*args` 时，Python 会将所有传入的多余的位置参数收集到一个**元组 (tuple)** 中。

这个名字 `args` 只是一个约定俗成的惯例 (arguments 的缩写)，你也可以用别的名字，比如 `*numbers`，但关键在于前面的星号 `*`。

#### 作用：

收集所有未被其他形参接收的位置参数，并将它们打包成一个元组。

#### 举例说明：

假设你想写一个函数来计算任意多个数字的总和。

```python
# 定义一个函数，使用 *args 来接收任意数量的数字
def sum_all(*args):
    """这个函数计算所有传入参数的总和"""
    print(f"传入的参数被打包成一个元组: {args}")
    print(f"元组的类型是: {type(args)}")
    
    total = 0
    # 因为 args 是一个元组，我们可以遍历它
    for number in args:
        total += number
    return total

# 调用函数并传入不同数量的参数
print(f"总和是: {sum_all(1, 2, 3)}\n")
# 输出:
# 传入的参数被打包成一个元组: (1, 2, 3)
# 元组的类型是: <class 'tuple'>
# 总和是: 6

print(f"总和是: {sum_all(10, 20, 30, 40, 50)}\n")
# 输出:
# 传入的参数被打包成一个元组: (10, 20, 30, 40, 50)
# 元组的类型是: <class 'tuple'>
# 总和是: 150

print(f"总和是: {sum_all()}\n")
# 输出:
# 传入的参数被打包成一个元组: ()
# 元组的类型是: <class 'tuple'>
# 总和是: 0
```

在这个例子中，无论你传入多少个数字，`*args` 都会将它们全部收集到名为 `args` 的元组里，然后函数体可以方便地对这个元组进行处理。

-----

### `**kwargs` (任意数量的关键字参数)

`**kwargs` 用于接收任意数量的**关键字参数 (keyword arguments)**，也就是形如 `key=value` 的参数。当你在函数定义中使用 `**kwargs` 时，Python 会将这些关键字参数收集到一个**字典 (dictionary)** 中。

同样，`kwargs` 是 "keyword arguments" 的缩写，这只是一个惯例，关键在于前面的两个星号 `**`。

#### 作用：

收集所有未被其他形参接收的关键字参数，并将它们打包成一个字典。

#### 举例说明：

假设你想创建一个函数来打印用户信息，但用户信息的字段是不固定的。

```python
# 定义一个函数，使用 **kwargs 来接收不定的用户信息
def print_user_profile(**kwargs):
    """这个函数打印用户的个人信息"""
    print(f"传入的参数被打包成一个字典: {kwargs}")
    print(f"字典的类型是: {type(kwargs)}")
    
    # 检查 'name' 是否存在，如果不存在则给一个默认值
    name = kwargs.get('name', '匿名用户')
    print(f"\n用户信息 - {name}:")
    
    # 因为 kwargs 是一个字典，我们可以遍历它的键值对
    if not kwargs:
        print("没有提供任何信息。")
    else:
        for key, value in kwargs.items():
            print(f"  {key}: {value}")

# 调用函数并传入不同的关键字参数
print_user_profile(name="张三", age=30, city="北京")
# 输出:
# 传入的参数被打包成一个字典: {'name': '张三', 'age': 30, 'city': '北京'}
# 字典的类型是: <class 'dict'>
#
# 用户信息 - 张三:
#   name: 张三
#   age: 30
#   city: 北京

print("-" * 20)

print_user_profile(name="李四", occupation="工程师", status="在职", country="中国")
# 输出:
# 传入的参数被打包成一个字典: {'name': '李四', 'occupation': '工程师', 'status': '在职', 'country': '中国'}
# 字典的类型是: <class 'dict'>
#
# 用户信息 - 李四:
#   name: 李四
#   occupation: 工程师
#   status: 在职
#   country: 中国
```

在这个例子中，`**kwargs` 将所有 `key=value` 形式的参数收集到 `kwargs` 字典中，让函数可以灵活处理各种不同的属性。

-----

### 结合使用 `*args` 和 `**kwargs`

你可以在同一个函数中同时使用普通参数、`*args` 和 `**kwargs`。但必须遵循以下顺序：

1.  **标准位置参数 (Standard positional arguments)**
2.  `*args`
3.  `**kwargs`

#### 举例说明：

```python
def process_data(handler_name, *data_points, **options):
    """
    一个处理数据的函数示例。
    - handler_name: 一个必需的位置参数
    - *data_points: 任意数量的数据点
    - **options: 任意数量的配置选项
    """
    print(f"处理器名称: {handler_name}")
    print(f"收集到的数据点 (元组): {data_points}")
    print(f"收集到的配置项 (字典): {options}")

    print("\n--- 开始处理 ---")
    
    # 处理数据点
    for point in data_points:
        print(f"正在处理数据点: {point}")
        
    # 根据配置项执行操作
    if options.get('is_active', False):
        print("状态: 处理器已激活。")
        timeout = options.get('timeout', 10) # 获取配置项，若不存在则使用默认值
        print(f"超时设置为: {timeout}秒。")
    else:
        print("状态: 处理器未激活。")

# 调用函数
process_data(
    "温度传感器",           # 标准位置参数
    25.5, 26.1, 25.9,     # 这三个会进入 *args
    is_active=True,       # 这两个会进入 **kwargs
    unit="摄氏度",
    timeout=30
)
```

输出结果:

```
处理器名称: 温度传感器
收集到的数据点 (元组): (25.5, 26.1, 25.9)
收集到的配置项 (字典): {'is_active': True, 'unit': '摄氏度', 'timeout': 30}

--- 开始处理 ---
正在处理数据点: 25.5
正在处理数据点: 26.1
正在处理数据点: 25.9
状态: 处理器已激活。
超时设置为: 30秒。
```

-----

### 用 `*` 和 `**` 解包 (Unpacking)

`*` 和 `**` 还有一个相反的用途：在**调用函数**时，使用它们来“解包”列表/元组或字典，将其作为独立的参数传入。

#### 解包列表/元组 (`*`)

```python
def add(a, b, c):
    return a + b + c

numbers = [1, 2, 3]
# 使用 * 解包列表，相当于调用 add(1, 2, 3)
result = add(*numbers) 
print(result) # 输出: 6

numbers_tuple = (4, 5, 6)
result2 = add(*numbers_tuple)
print(result2) # 输出: 15
```

#### 解包字典 (`**`)

```python
def create_greeting(name, message):
    return f"你好, {name}! {message}"

user_data = {
    'name': '王五',
    'message': '欢迎来到Python的世界。'
}
# 使用 ** 解包字典，相当于调用 create_greeting(name='王五', message='欢迎来到Python的世界。')
greeting = create_greeting(**user_data)
print(greeting) # 输出: 你好, 王五! 欢迎来到Python的世界。
```

### 总结

| 关键字 | 在函数定义中 (打包) | 在函数调用中 (解包) | 数据类型 |
| :--- | :--- | :--- | :--- |
| **`*args`** | 将多个**位置参数**收集到一个**元组**中 | 将一个列表或元组“解开”成多个**位置参数** | 元组 (tuple) |
| **`**kwargs`** | 将多个**关键字参数**收集到一个**字典**中 | 将一个字典“解开”成多个**关键字参数** | 字典 (dict) |

`*args` 和 `**kwargs` 是 Python 中非常实用的特性，它们极大地增强了函数的灵活性和可重用性，在编写框架、装饰器和处理不确定输入的函数时尤其有用。
