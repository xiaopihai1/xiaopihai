## 可变与不可变类型

* 可变类型
  * 列表(序列类型)
  * 字典(序列类型)
* 不可变类型
  * 数字
  * 自符串
  * 元组(序列类型)

**这里的可变不可变，是指内存中的那块内容（value）是否可以被改变**

**不可变序列类型普遍实现而可变序列类型未实现的唯一操作就是对hash() 内置函数的支持。 这种支持允许不可变类型，例如tuple 实例被用作dict 键，以及存储在set 和frozenset 实例中。 尝试对包含有不可哈希值的不可变序列进行哈希运算将会导致TypeError。 **

## 浅拷贝与深拷贝的实现方式

**浅层复制和深层复制之间的区别仅与复合对象 (即包含其他对象的对象，如列表或类的实例) 相关: **

* 一个 浅层复制会构造一个新的复合对象，然后(在可能的范围内)将原对象中找到的 引用插入其 中。 

* 一个 深层复制会构造一个新的复合对象，然后递归地将原始对象中所找到的对象的 副本插入。 

**深度复制操作通常存在两个问题,而浅层复制操作并不存在这些问题 **

* 递归对象 (直接或间接包含对自身引用的复合对象) 可能会导致递归循环 
* 由于深层复制会复制所有内容，因此可能会过多复制(例如本应该在副本之间共享的数据



##   \_\_new\_\_() 与 \_\_init\_\_()

* \_\_new\_\_是在实例创建之前被调用的，因为它的任务就是创建实例然后返回该实例，是个静态方法。

  * `__new__`至少要有一个参数cls，代表要实例化的类，此参数在实例化时由Python解释器自动提供

  * `__new__`必须要有返回值，返回实例化出来的实例，这点在自己实现`__new__`时要特别注意，可以return父类`__new__`出来的实例，或者直接是object的`__new__`出来的实例

* \_\_init\_\_是当实例对象创建完成后被调用的，然后设置对象属性的一些初始值。

  * `__init__`有一个参数self，就是这个`__new__`返回的实例，`__init__`在`__new__`的基础上可以完成一些其它初始化的动作，`__init__`不需要返回值



## 设计模式

1. 单例模式

   1. \_\_new\_\_方式

      ```python
      class SingleTon(object):
          
          def __new__(cls, *args, **kwargs):
              if not hasattr(cls, "_instance")
                  cls._instance = super().__new__(cls, *args, **kwargs)
              return cls._instance
            
          def __init__(self):
            pass
          
      ```

   2. \_\_new\_\_方式(2)

      ```python
      class SingleTon(object):
      		_states = {}
        
        	def __new__(cls, *args, **kwargs):
            obj = super().__new__(cls, *args, **kwargs)
            obj.__dict__ = cls._states
            return obj
          
          def __init__(self):
            pass
          # 共享属性, 所有实例指向同一个__dict__
      ```

      

   3. 装饰器方式

      ```python
      def singleton(cls, *args, **kwargs):
        	_instance = {}
        	def _singleton(*args, **kwargs):
          		if cls not in _instance:
            			_instance[cls] = cls(*args, **kwargs)
              return _instance[cls]
          return _singleton
      ```

## 编码和解码

> 参考[知乎](https://zhuanlan.zhihu.com/p/38293267)实验楼在线教育

**1、一些基本的概念**

- 比特 / `bit`：计算机中最小的数据单位，是单个的二进制数值 0 或 1
- 字节 / `byte`：计算机存储数据的单元，1 个字节由 8 个比特组成
- 字符：人类能够识别的符号
- 编码：将人类可识别的字符转换为机器可识别的字节码 / 字节序列
- 解码：编码的反向过程叫解码
- 概述：`Unicode` 是人类可识别的字符格式；`ASCII` 、`UTF-8` 、`GBK` 等都是机器可识别的字节码格式。我们写在文件中的 py3 代码，是由字符组成的，它们的格式，就是 `Unicode`，而字符是以字节为存储单位保存在文件中，文件保存在内存 / 物理磁盘中

**2、编码格式**

* Python2 的默认编码是 `ASCII`，不能识别中文字符，需要显式指定字符编码

* Python3 的默认编码为 `Unicode`，可以识别中文字符

* 计算机内存中的数据，统一使用 `Unicode` 编码

* 数据传输或保存到硬盘上，使用 `UTF-8` 编码

**3、编码和解码**

- 编码 / `encode`：将 `Unicode` 字符串转换为特定编码格式对应的字节码的过程
- 解码 / `decode`：将特定编码格式的字节码转换为对应的 `Unicode` 字符串的过程

**4、Python3 的默认编码**

- 例如我们用编辑器打开一个文件，输入 `a`，这个 `a` 就是 `Uincode` 字符。当我们保存文件，这个字符就会根据编辑器的设置被转换为对应的编码格式的字节码保存到系统硬盘。这是一个 `encode` 过程
- Python 解释器执行文件时，先将文件字节码按照指定的编码格式解码为字符，然后运行程序。这是一个 `decode` 过程
- 指定编码，在文件开头，例如：Python 文件通常这样写：`# -*- coding:utf-8 -*-`；HTML 文件通常这样写：``，意思就是该文件是 `UTF-8` 编码格式，按这个格式解码
- 上文提到 Python3 的默认编码是 `UTF-8`。即如果文件未标明编码格式，解释器会自动按照 `UTF-8`来解码。当解释器无法通过指定的或默认的编码格式对文件进行转换 / 解码时，就会出现解码错误：`UnicodeEncodeError`

## 装饰器



## 垃圾回收



## 多进程与多线程



## 进程通信



## 协程



## 算法排序



## 网络基础



## 数据库



## Linux



## django



## uWSGI



## 