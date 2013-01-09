.. _built-in-consts:

内置常量
==================

内置命名空间中有少些常量，它们是：

.. data:: False

   :class:`bool` 类型的假值。对 ``False`` 进行赋值是非法的，会抛出 :exc:`SyntaxError` 异常。


.. data:: True

   :class:`bool` 类型的真值。对 ``True`` 进行赋值是非法的，会抛出 :exc:`SyntaxError` 异常。


.. data:: None

   类型 ``NoneType`` 唯一的值。\ ``None`` 经常用来表示没有值，例如不把默认的参数传给函数时。对 ``None`` 进行赋值是非法的，会抛出 :exc:`SyntaxError` 异常。


.. data:: NotImplemented

   一个特殊的值，会在特殊方法的"富比较"(:meth:`__eq__` 、\ :meth:`__lt__` 之类)中返回，以表示与其它类型的比较尚未实现。


.. data:: Ellipsis

   与 ``...`` 相同。这是一个特殊的值，主要用于为用户自定义的容器类型扩展切片语法。


.. data:: __debug__

   如果 Python 启动时没有带 :option:`-O` 选项，它就为真。参见 :keyword:`assert` 语句。


.. note::

   不能对 :data:`None` 、\ :data:`False` 、\ :data:`True` 和 :data:`__debug__` 重新赋值(对它们赋值，即使是作为属性名字，也会抛出 :exc:`SyntaxError` 异常)，所以可以认为它们是"真正的"常量。


:mod:`site` 模块添加的常量
-----------------------------------------

:mod:`site` 模块(启动时会自动导入，除非指定了 :option:`-S` 命令行选项)在内置命名空间中加入了几个常量。它们对交互式解释器有用，但不应该在程序中使用。

.. data:: quit(code=None)
          exit(code=None)

   一个对象，如果打印它就会输出一条信息"Use quit() or Ctrl-D (i.e. EOF) to exit"，如果调用它就会抛出 :exc:`SystemExit` 异常并以指定返回码 code 返回。

.. data:: copyright
          license
          credits

   一个对象，如果打印它就会输出一条信息"Type license() to see the full license text"，如果调用它就会显示相关的文本，带分页的功能(每次一屏)。

