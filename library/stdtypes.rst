.. XXX: reference/datamodel and this have quite a few overlaps!


.. _bltin-types:

**************
内置类型
**************

下面的章节介绍内置于解释器中的标准类型。

.. index:: pair: 内置; 类型

内置类型中最主要的是数值、序列、映射、类、实例、以及异常。

有些集合类是可变的。它们的添加、去除、重排序方法加在原地操纵其元素，而并不返回某个项，也从不返回整个集合实例自身，只返回 ``None`` 。

有些操作被好几种对象类型支持。尤其是几乎所有的对象都可以时行比较、检测真值、转化为字符串(使用 :func:`repr` 函数或者稍稍不同的 :func:`str` 函数)。转化为字符串的函数在使用 :func:`print` 函数打印对象时会明确调用。


.. _truth:

检测真值
===================

.. index::
   statement: if
   statement: while
   pair: 真值; 值
   pair: 布尔; 运算
   single: 假

任何对象都可以进行真值检测，以用于 :keyword:`if` 或者 :keyword:`while` 条件中，或者作为下述布尔运算的操作数。下面的值被认为是假：

  .. index:: single: None (内置对象)

* ``None``

  .. index:: single: False (内置对象)

* ``False``

* 任何数值类型中的零，例如 ``0`` 、\ ``0.0`` 、\ ``0j`` 。

* 任何空的序列，例如 ``''`` 、\ ``()`` 、\ ``[]`` 。

* 任何空的映射，例如 ``{}`` 。

* 对于用户自定义类的实例，如果这个类定义了 :meth:`__bool__` 或者 :meth:`__len__` 方法，当这些方法返回整数零或者 :class:`bool` 值 ``False`` 时。 [1]_

.. index:: single: 真

所有其它的值都被当作真 --- 所以很多类型的对象总是真。

.. index::
   operator: or
   operator: and
   single: False
   single: True

结果为布尔值的运算和内置函数，当为假的时候总是返回 ``0`` 或者 ``False`` ，为真时总是返回 ``1`` 或者 ``True`` ，除非特别说明。(重要反例：布尔运算符 ``or`` 和 ``and`` 总是返回其中一个操作数。)


.. _boolean:

布尔运算 --- :keyword:`and` 、\ :keyword:`or` 、\ :keyword:`not`
====================================================================

.. index:: pair: 布尔; 运算符

下面是布尔运算，按优先升序排列：

+-------------+---------------------------------+-------+
| 运算        | 结果                            | 备注  |
+=============+=================================+=======+
| ``x or y``  | 如果 *x* 为假则为 *y* ，否则为  | \(1)  |
|             | *x*                             |       |
+-------------+---------------------------------+-------+
| ``x and y`` | 如果 *x* 为假则为 *x* ，否则为  | \(2)  |
|             | *y*                             |       |
+-------------+---------------------------------+-------+
| ``not x``   | 如果 *x* 为假则为 ``True`` ，否 | \(3)  |
|             | 则为 ``False``                  |       |
+-------------+---------------------------------+-------+

.. index::
   operator: and
   operator: or
   operator: not

备注：

(1)
   这是一个短路运算符，只有在第一个参数为 :const:`False` 时它才会对第二个参数时行求值。

(2)
   这是一个短路运算符，只有在第一个参数为 :const:`True` 时它才会对第二个参数时行求值。

(3)
   ``not`` 的优先级比非布尔运算符低，所以 ``not a == b`` 会解释为 ``not (a == b)`` ，而 ``a == not b`` 是语法错误。


.. _stdcomparisons:

Comparisons
===========

.. index::
   pair: 级联; 比较
   pair: 运算符; 比较
   operator: ==
   operator: <
   operator: <=
   operator: >
   operator: >=
   operator: !=
   operator: is
   operator: is not

Python 中有八种比较运算，它们的优先级相同(都比布尔运算优先级高)。比较运算可以任意级联，例如 ``x < y <= z`` 相当于 ``x < y and
y <= z`` ，区别在于 *y* 仅求值一次(如果发现 ``x < y`` ，则两者都不会对 *z* 求值)。

下表总结了比较运算：

+------------+-------------------------+
| 运算       | 含义                    |
+============+=========================+
| ``<``      | 仅小于                  |
+------------+-------------------------+
| ``<=``     | 小于或等于              |
+------------+-------------------------+
| ``>``      | 仅大于                  |
+------------+-------------------------+
| ``>=``     | 大于或等于              |
+------------+-------------------------+
| ``==``     | 等于                    |
+------------+-------------------------+
| ``!=``     | 不等于                  |
+------------+-------------------------+
| ``is``     | 对象身份识别            |
+------------+-------------------------+
| ``is not`` | 否定的对象身份识别      |
+------------+-------------------------+

.. index::
   pair: 对象; 数值
   pair: 对象; 比较

不同类型的对象，除非是不同的数值类型，比较时都不相等。此外，有些类型(例如函数对象)仅支持弱化的比较，这时这种类型的任意两个对象都不相等。当把一个复数和其它内置的数值类型进行比较时，或不同类型且不能比较时，或者其它没有定义顺序的情况下，\ ``<`` 、\ ``<=`` 、\ ``>`` 和 ``>=`` 运算符会抛出 :exc:`TypeError` 异常。

.. index::
   single: __eq__() (实例方法)
   single: __ne__() (实例方法)
   single: __lt__() (实例方法)
   single: __le__() (实例方法)
   single: __gt__() (实例方法)
   single: __ge__() (实例方法)

类中不是同一实例的比较时一般都不相等，除非该类定义了 :meth:`__eq__` 方法。

类的实例不能和该类的其它实例，或者其它类型的对象，进行排序，除非该类定义了足够全面的下述方法： :meth:`__lt__` 、\ :meth:`__le__` 、\ :meth:`__gt__` 、和 :meth:`__ge__` (通常 :meth:`__lt__` 和 :meth:`__eq__` 就已经足够了，如果你只需要传统意义上的比较运行符)。

:keyword:`is` 和 :keyword:`is not` 运算符的行为不能自定义；它们可以应用于任何两个对象而不会抛出异常。

.. index::
   operator: in
   operator: not in

:keyword:`in` 和 :keyword:`not in` 是优先级相同的另外两种运算，它们只支持序列类型(见下面)。


.. _typesnumeric:

数值类型 --- :class:`int` 、\ :class:`float` 、和 :class:`complex`
=======================================================================

.. index::
   object: 数值
   object: 布尔
   object: 整数
   object: 浮点数
   object: 复数
   pair: C; 语言

有三种不同的数值类型：\ :dfn:`integers` (整数形) 、\ :dfn:`floating point numbers` (浮点数型) 、和 :dfn:`complex numbers` (复数型)。此外，布尔型是整数型的子类型。整数型精度不受限制。浮点数通常是用 C 的 :c:type:`double` 实现，有关你运行程序的机器对浮点数的精度和内部表示形式的信息，参见 :data:`sys.float_info` 。复数有实部和虚部，它们都是浮点数；要得到一个浮点数 *z* 的实部和虚部，用 ``z.real`` 和 ``z.imag`` 。(标准库还包括其它数值类型：\ :mod:`fractions` 覆盖有理数，\ :mod:`decimal` 覆盖浮点数能让用户定义其精度。)

.. index::
   pair: 数值; 源常量
   pair: 整数; 源常量
   pair: 浮点; 源常量
   pair: 复数; 源常量
   pair: 十六进制数; 源常量
   pair: 八进制数; 源常量
   pair: 二进制数; 源常量

数值通过数值源常量创建，也可以是通过内置函数和运算符的返回值获得。不回修饰的整数型源常量(包括十六进制、八进制、二进制数)生成整数。带小数点和指数符号的数值源常量生成浮点数。在数值源常量后面加上 ``'j'`` 或者 ``'J'`` 生成虚数(实部为零的复数)，它可以和整数或浮点数相加得到同时有实部和虚部的复数。

.. index::
   single: 算术运算
   builtin: int
   builtin: float
   builtin: complex
   operator: +
   operator: -
   operator: *
   operator: /
   operator: //
   operator: %
   operator: **

Python 完全支持混合算术运算：如果一个二目算术运算符的两个操作数有不同的类型，则类型较"窄"的操作数就会扩展为另外一个操作数的类型；其中，整数型比浮点数型窄，浮点数型比复数型窄。混合类型的数值进行比较时也适用同样的规则。\ [2]_\ 构造函数 :func:`int` 、\ :func:`float` 、和 :func:`complex` 可以用来生成指定类型的数值。

所有的数值类型(复数除外)都支持下列运算，按优先级升序排列(同一单元格中的运算有相同的优先级；所有数值运算的优先级都比比较运算高)：

+---------------------+---------------------------------+---------+--------------------+
| 运算                | 结果                            | 备注    | 完整文档           |
+=====================+=================================+=========+====================+
| ``x + y``           | *x* 和 *y* 的和                 |         |                    |
+---------------------+---------------------------------+---------+--------------------+
| ``x - y``           | *x* 和 *y* 的差                 |         |                    |
+---------------------+---------------------------------+---------+--------------------+
| ``x * y``           | *x* 和 *y* 的积                 |         |                    |
+---------------------+---------------------------------+---------+--------------------+
| ``x / y``           | *x* 和 *y* 的商                 |         |                    |
+---------------------+---------------------------------+---------+--------------------+
| ``x // y``          | *x* 和 *y* 的商向下取整         | \(1)    |                    |
+---------------------+---------------------------------+---------+--------------------+
| ``x % y``           | ``x / y`` 的余数                | \(2)    |                    |
+---------------------+---------------------------------+---------+--------------------+
| ``-x``              | *x* 的相反数                    |         |                    |
+---------------------+---------------------------------+---------+--------------------+
| ``+x``              | *x* 自身                        |         |                    |
+---------------------+---------------------------------+---------+--------------------+
| ``abs(x)``          | *x* 的绝对值或者模              |         | :func:`abs`        |
+---------------------+---------------------------------+---------+--------------------+
| ``int(x)``          | *x* 转化为整数                  | \(3)\(6)| :func:`int`        |
+---------------------+---------------------------------+---------+--------------------+
| ``float(x)``        | *x* 转化为浮点数                | \(4)\(6)| :func:`float`      |
+---------------------+---------------------------------+---------+--------------------+
| ``complex(re, im)`` | 实部为 *re* ，虚部为 *im* 的复  | \(6)    | :func:`complex`    |
|                     | 数。\ *im* 默认为零。           |         |                    |
+---------------------+---------------------------------+---------+--------------------+
|  ``c.conjugate()``  | 复数 *c* 的共扼复数             |         |                    |
+---------------------+---------------------------------+---------+--------------------+
| ``divmod(x, y)``    | 元组 ``(x // y, x % y)``        | \(2)    | :func:`divmod`     |
+---------------------+---------------------------------+---------+--------------------+
| ``pow(x, y)``       | *x* 的 *y* 次方                 | \(5)    | :func:`pow`        |
+---------------------+---------------------------------+---------+--------------------+
| ``x ** y``          | *x* 的 *y* 次方                 | \(5)    |                    |
+---------------------+---------------------------------+---------+--------------------+

.. index::
   triple: 运算; 数值; 类型
   single: conjugate() (复数方法)

备注：

(1)
   又叫整除，返回整数，尽量其类型不一定是整数。结果总是向负无穷进位：\ ``1//2`` 是 ``0`` 、\ ``(-1)//2`` 是 ``-1`` 、\ ``1//(-2)`` 是 ``-1`` 、则 ``(-1)//(-2)`` 是 ``0`` 。

(2)
   不适用于复数，而应该在适当的时候用 :func:`abs` 把它转化为浮点数。

(3)
   .. index::
      module: math
      single: floor() (模块 math 中)
      single: ceil() (模块 math 中)
      single: trunc() (模块 math 中)
      pair: 数值; 转换
      pair: C; 语言

   浮点数向整数的转换可能像 C 那样进位或者舍尾，具体规则参见模块 :mod:`math` 中的函数 :func:`floor` 和 :func:`ceil` 。

(4)
   float 的参数还可以是字符串 "nan" 和 "inf" ，它们前面可以有前缀 "+" 或 "-" ，表示非数值(NaN)和正负无穷。

(5)
   Python 定义 ``pow(0, 0)`` 和 ``0 ** 0`` 为 ``1`` ，和其它编程语言一样。

(6)
   可以接受的数值源常量包括数字 ``0`` 到 ``9`` 或任何对等的 Unicode 字符(带 ``Nd`` 属性的字码)。

   所有带 ``Nd`` 属性的字码，参见 http://www.unicode.org/Public/6.0.0/ucd/extracted/DerivedNumericType.txt 。


所有的 :class:`numbers.Real` 类型(:class:`int` 和 :class:`float`)还支持下列运算：

+--------------------+------------------------------------+--------+
| 运算               | 结果                               | 备注   |
+====================+====================================+========+
| ``math.trunc(x)``  | *x* 去尾成 Integral 类型           |        |
+--------------------+------------------------------------+--------+
| ``round(x[, n])``  | *x* 向小数点后 n 位进位，进位成偶  |        |
|                    | 数。n 如果省略则默认为 0           |        |
+--------------------+------------------------------------+--------+
| ``math.floor(x)``  | 小于等于 *x* 的最大整数            |        |
+--------------------+------------------------------------+--------+
| ``math.ceil(x)``   | 大于等于 *x* 的最小整数            |        |
+--------------------+------------------------------------+--------+

更多数值运算参见 :mod:`math` 和 :mod:`cmath` 模块。

.. XXXJH exceptions: overflow (when? what operations?) zerodivision


.. _bitstring-ops:

整数类型的位运算
--------------------------------------

.. index::
   triple: 运算; 整数; 类型
   pair: 按位; 运算
   pair: 移位; 运算
   pair: 遮位; 运算
   operator: ^
   operator: &
   operator: <<
   operator: >>

按位运算只对整数有意义。负数在处理时使用其 2 的补码(这里假定数位足够多，在运算时不会发生溢出)。

二目按位运算的优先级都比数值运算的低，比比较运算高；单目运算 ``~`` 的优先级和其它单目运算(``+`` 和 ``-``)一样。

下表列出按位运算，并按优先级升序排列(同一单元格中的运算有相同的优先级)：

+------------+--------------------------------+----------+
| 运算       | 结果                           | 备注     |
+============+================================+==========+
| ``x | y``  | *x* 和 *y* 的按位 :dfn:`or`    |          |
+------------+--------------------------------+----------+
| ``x ^ y``  | *x* 和 *y* 的按位              |          |
|            |  :dfn:`exclusive or`           |          |
+------------+--------------------------------+----------+
| ``x & y``  | *x* 和 *y* 按位 :dfn:`and`     |          |
+------------+--------------------------------+----------+
| ``x << n`` | *x* 向左移动 *n* 位            | (1)(2)   |
+------------+--------------------------------+----------+
| ``x >> n`` | *x* 向右移动 *n* 位            | (1)(3)   |
+------------+--------------------------------+----------+
| ``~x``     | *x* 的各数位按位取反           |          |
+------------+--------------------------------+----------+

备注：

(1)
   负的移动位数是非法的，会导致抛出 :exc:`ValueError` 异常。

(2)
   向左移动 *n* 位相当于乘以 ``pow(2, n)`` 而不进行溢出检查。

(3)
  向右移动 *n* 位相当于除以 ``pow(2, n)`` 而不进行溢出检查。


整数类型的其它方法
-----------------------------------

int 类型实现了 :class:`numbers.Integral` :term:`虚基类`\ 。此外，它还提供一个额外的方法：

.. method:: int.bit_length()

    返回表示一个数所需要二进制的位数，不包括正负符号和前导的零::

        >>> n = -37
        >>> bin(n)
        '-0b100101'
        >>> n.bit_length()
        6

    准确的说，如果 ``x`` 非零，则 ``x.bit_length()`` 是唯一的正整数 ``k`` ，它满足 ``2**(k-1) <= abs(x) < 2**k`` 。等价的，当 ``abs(x)`` 足够小而有个进位正确的对数，则 ``k = 1 + int(log(abs(x), 2))`` 。如果 ``x`` 是零则 ``x.bit_length()`` 返回 ``0`` 。

    相当于::

        def bit_length(self):
            s = bin(self)       # 二进制形式： bin(-37) --> '-0b100101'
            s = s.lstrip('-0b') # 去掉前导的零和负号
            return len(s)       # len('100101') --> 6

    .. versionadded:: 3.1

.. method:: int.to_bytes(length, byteorder, \*, signed=False)

    返回一个表示整数的各字节的数组。

        >>> (1024).to_bytes(2, byteorder='big')
        b'\x04\x00'
        >>> (1024).to_bytes(10, byteorder='big')
        b'\x00\x00\x00\x00\x00\x00\x00\x00\x04\x00'
        >>> (-1024).to_bytes(10, byteorder='big', signed=True)
        b'\xff\xff\xff\xff\xff\xff\xff\xff\xfc\x00'
        >>> x = 1000
        >>> x.to_bytes((x.bit_length() // 8) + 1, byteorder='little')
        b'\xe8\x03'

    该整数用 *length* 个字节表示。如果指定的字节不能表示这个整数，则抛出 :exc:`OverflowError` 异常。

    *byteorder* 参数决定表示该整数的字节先后顺序。如果 *byteorder* 是 ``"big"`` ，则高位字节放在数组的前面。如果 *byteorder* 是 ``"little"`` ，则高位字节放在数组的后面。如果想使用系统自身的字节顺序，可以用 :data:`sys.byteorder` 。

    *signed* 参数决定是否使用二的补码来表示这个整数。如果 *signed* 是 ``False`` ，而给出的整数又是负的，则抛出 :exc:`OverflowError` 异常。默认的 *signed* 值是 ``False`` 。

    .. versionadded:: 3.2

.. classmethod:: int.from_bytes(bytes, byteorder, \*, signed=False)

    返回由指定数组所表示的整数。

        >>> int.from_bytes(b'\x00\x10', byteorder='big')
        16
        >>> int.from_bytes(b'\x00\x10', byteorder='little')
        4096
        >>> int.from_bytes(b'\xfc\x00', byteorder='big', signed=True)
        -1024
        >>> int.from_bytes(b'\xfc\x00', byteorder='big', signed=False)
        64512
        >>> int.from_bytes([255, 0, 0], byteorder='big')
        16711680

    参数 *bytes* 必须支持 buffer 协议或者是一个能够产生字节的可迭代对象。\ :class:`bytes` 和 :class:`bytearray` 是支持 buffer 协议的内置类型。

    *byteorder* 参数表示整数时所使用的字节顺序。如果 *byteorder* 是 ``"big"`` ，则高位字节位于数组前端；如果 *byteorder* 是 ``"little"`` ，则高位字节位于数组后端。如果想使用系统自身的字节顺序，可以用 :data:`sys.byteorder` 。

    *signed* 参数决定是否使用二的补码来表示这个整数。

    .. versionadded:: 3.2


浮点数类型的其它方法
---------------------------

float 类型实现了 :class:`numbers.Real` :term:`虚基类`\ 。它还有下面的额外方法：

.. method:: float.as_integer_ratio()

   返回一对整数，其比值正好等于原来的浮点数，并且分母是正数。对正负无穷会抛出 :exc:`OverflowError` 异常，而对 NaN 会抛出 :exc:`ValueError` 异常。

.. method:: float.is_integer()

   如果这个浮点数实例的整数部分是有限的则返回 ``True`` ，否则返回 ``False``::

      >>> (-2.0).is_integer()
      True
      >>> (3.2).is_integer()
      False

有两个方法支持与十六进制的互相转化。因为 Python 中的浮点数是使用二进制来存储的，它与\ *十进制*\ 字符串的来回转化通常都会产生细微的进位误差。相对而言，十六进制字符串却能精确表示一个浮点数。这在调试或数值计算中很有用。


.. method:: float.hex()

   把一个浮点数用十六进制客串表示。对于有限的浮点数，这种表示形式总是包含一个前导的 ``0x`` 和结尾的 ``p`` 及指数。


.. classmethod:: float.fromhex(s)

   类方法，返回由十六进制字符串 *s* 表示的浮点数。字符串 *s* 前后都可以有空白字符。


注意，\ :meth:`float.hex` 是一个实例方法，而 :meth:`float.fromhex` 是类方法。

十六进制字符串有下面的形式::

   [sign] ['0x'] integer ['.' fraction] ['p' exponent]

可选的 ``sign`` 可以是 ``+`` 或者 ``-`` ，\ ``integer`` 和 ``fraction`` 是十六进制数字构成的字符串，而 ``exponent`` 是一个前面可以带有符号的十进制整数。大小写是不重要的，并且 integer 或者 fraction 的部分必须至少有一个十六进制数字。这种语法和 C99 标准第 6.4.4.2 节规定的语法相似，也和 Java 1.5 及以后版本的语法相似。特别的，\ :meth:`float.hex` 是输出可以在 C 和 Java 代码中用作十六进制浮点数，而 C 的 ``%a`` 格式或 Java 的 ``Double.toHexString`` 产生的结果都可以作为 :meth:`float.fromhex` 的输入。

注意，指数是用十进制表示的，而不是十六进制，并且它表示系数要乘以 2 的幂。例如，十六进制字符串 ``0x3.a7p10`` 表示浮点数 ``(3 + 10./16 + 7./16**2) * 2.0**10`` ，即 ``3740.0``::

   >>> float.fromhex('0x3.a7p10')
   3740.0


对 ``3740.0`` 进行反向转换会得到一个不同的十六进制字符串，但表示的是同一个数::

   >>> float.hex(3740.0)
   '0x1.d380000000000p+11'


.. _numeric-hash:

数值类型的散列值
------------------------

对于数值 ``x`` 和 ``y`` ，可能是不同类型的，要求只要 ``x == y`` ，就应该有 ``hash(x) == hash(y)`` (详情参见 :meth:`__hash__` 方法的文档)。为了实现的方便，以及兼顾各种不同数值类型的效率(包括 :class:`int` 、\ :class:`float` 、\ :class:`decimal.Decimal` 和 :class:`fractions.Fraction`)，Python 中数值类型的散列值是基于同一个数学函数，这个函数定义了任意有理数，所以它也适合所有 :class:`int` 和 :class:`fraction.Fraction` 的实例，以及有限的 :class:`float` 和 :class:`decimal.Decimal` 实例。这个函数的核心是用一个固定的素数 ``P`` ，计算出它的归约模 ``P`` 。\ ``P`` 的值在 Python 中可以通过 :data:`sys.hash_info` 的 :attr:`modulus` 属性来访问。

.. impl-detail::

   目前使用的素数，在 C 长整型为 32 位的机器上是 ``P = 2**31 - 1`` ，在 C 长整型为 64 位的机器上是 ``P = 2**61 - 1`` 。

下面是具体规则:

- 如果 ``x = m / n`` 是一个非负有理数并且 ``n`` 不可被 ``P`` 整除，则 ``hash(x)`` 定义为 ``m * invmod(n, P) % P`` ，其中 ``invmod(n,  P)`` 是 ``n`` 模 ``P`` 的倒数。

- 如果 ``x = m / n`` 是一个非负有理数并且 ``n`` 可以被 ``P`` 整除(但 ``m`` 不可以)，则 ``n`` 不是模 ``P`` 的倒数，上面的规则不适用。这种情况下，定义 ``hash(x)`` 为常量值 ``sys.hash_info.inf`` 。

- 如果 ``x = m / n`` 是负的有理数，定义 ``hash(x)`` 为 ``-hash(-x)`` 。如果这个结果是 ``-1`` ，则使用 ``-2`` 为散列值。

- 特殊值 ``sys.hash_info.inf`` 、\ ``-sys.hash_info.inf`` 和 ``sys.hash_info.nan`` 分别作为正无穷、负无穷、NaN 的散列值(所有可散列的 NaN 都有相同的散列值)。

- 对于 :class:`complex` 数值 ``z`` ，通过计算 ``hash(z.real) + sys.hash_info.imag * hash(z.imag)`` ，把实部和虚部的散列值结合起来，然后进行模归约 ``2**sys.hash_info.width`` ，使得结果位于 ``range(-2**(sys.hash_info.width - 1), 2**(sys.hash_info.width - 1))`` 。同样的，如果结果是 ``-1`` ，则用 ``-2`` 代替。


为了阐明上述规则，下面是一些 Python 代码例子，相当于内置的 hash 函数，它们用来计算一个有理数、\ :class:`float` 或 :class:`complex` 的散列值::


   import sys, math

   def hash_fraction(m, n):
       """计算有理数 m / n 的散列值。

       假定 m 和 n 都是整数，其中 n 是正数。相当于 hash(fractions.Fraction(m, n)) 。

       """
       P = sys.hash_info.modulus
       # 去掉公分母 P(如果 m 和 n 已经是互质数则不需要)。
       while m % P == n % P == 0:
           m, n = m // P, n // P

       if n % P == 0:
           hash_ = sys.hash_info.inf
       else:
           # 费马小定理： pow(n, P-1, P) 是 1 ，所以 pow(n, P-2, P) 是 n 模 P 的倒数。
           hash_ = (abs(m) % P) * pow(n, P - 2, P) % P
       if m < 0:
           hash_ = -hash_
       if hash_ == -1:
           hash_ = -2
       return hash_

   def hash_float(x):
       """计算浮点数 x 的散列值。"""

       if math.isnan(x):
           return sys.hash_info.nan
       elif math.isinf(x):
           return sys.hash_info.inf if x > 0 else -sys.hash_info.inf
       else:
           return hash_fraction(*x.as_integer_ratio())

   def hash_complex(z):
       """计算复数 z 的散列值。"""

       hash_ = hash_float(z.real) + sys.hash_info.imag * hash_float(z.imag)
       # 进行带符号的模归约 2**sys.hash_info.width
       M = 2**(sys.hash_info.width - 1)
       hash_ = (hash_ & (M - 1)) - (hash & M)
       if hash_ == -1:
           hash_ == -2
       return hash_

.. _typeiter:

迭代器类型
==============

.. index::
   single: 迭代器协议
   single: 协议; 迭代器
   single: 序列; 迭代
   single: 容器; 迭代

Python 支持对容器的迭代概念。它是通过两个不同的方法实现的，这就允许用户自定义的类支持迭代。序列(下面将详述)总是支持迭代方法。

容器要支持迭代，需要定义一个方法：

.. XXX duplicated in reference/datamodel!

.. method:: container.__iter__()

   返回一个迭代器对象。这个对象对于支持下述迭代器协议是必须的。如果一个容器支持不同类型的迭代，则可以提供额外的方法以明确指定需要的迭代类型。(支持多种迭代形式的例子如树型结构，它同时支持广度优先和深度优先的搜索。)这个方法对应 Python/C API 中 Python 对象类型结构的 :attr:`tp_iter` 接槽。

迭代器对象本身要求支持下面两个方法，它们共同构成\ :dfn:`迭代器协议`\ ：

.. method:: iterator.__iter__()

   返回迭代器对象本身。这对于容器和迭代器都可以用于 :keyword:`for` 和 :keyword:`in` 语句来说，是必须的。这个方法对应 Python/C API 中 Python 对象类型结构的 :attr:`tp_iter` 接槽。


.. method:: iterator.__next__()

   返回容器中的下一个项。如果没有其它项了，则抛出 :exc:`StopIteration` 异常。这个方法对应 Python/C API 中 Python 对象类型结构的 :attr:`tp_iternext` 接槽。

Python 定义了几个迭代器对象用以支持对一般和特定的序列类型、字典、以及其它更广泛形式的迭代。这些特定的类型除了实现迭代器协议以外，没有其它重要作用。

一旦迭代器的 :meth:`~iterator.__next__` 方法抛出 :exc:`StopIteration` 异常，则后续调用也必须抛出同样的异常。不这么做的实现是不兼容的。


.. _generator-types:

生成函数类型
---------------

Python 的\ :term:`生成函数`\ 为实现迭代器协议提供了便捷方法。如果一个容器对象的 :meth:`__iter__` 方法是用生成函数实现的，就会自动返回一个迭代器对象(从技术上说是一个生成函数对象)，它支持 :meth:`__iter__` 和 :meth:`~generator.__next__` 方法。关于生成函数的更多信息参见\ :ref:`yield 表达式的文档 <yieldexpr>`\ 。


.. _typesseq:

序列类型 --- :class:`list` 、\ :class:`tuple` 、\ :class:`range`
================================================================

有三种基本的序列类型: 列表、元组、区间对象。为处理\ :ref:`二进制数据 <binaryseq>`\ 和\ :ref:`文本字符串 <textseq>`\ 而定制的其它序列类型会在专门的章节中介绍。


.. _typesseq-common:

共同的序列运算
--------------------------

.. index:: 对象: 序列

大部分序列类型，不管是可变的还是不可变的，都支持下表中的运算。\ :class:`collections.abc.Sequence` 虚基类可以用来让自定义序列类型正确且容易的实现这些运算。

表中列出的序列运算按照优先级升序排列(同一个单元格中的运算具有相同的优先级)。其中，\ *s* 和 *t* 是类型相同的序列，\ *n* 、\ *i* 、\ *j* 和 *k* 是整数，而 *x* 是个任意对象，它满足 *s* 对类型和值的限定。

``in`` 和 ``not in`` 运算与比较运算有相同的优先级。\ ``+`` (拼接)和 ``*`` (重复)运算与相应的算术运算有相同的优先级。

.. index::
   triple: 运算; 序列; 类型
   builtin: len
   builtin: min
   builtin: max
   pair: 拼接; 运算
   pair: 重复; 运算
   pair: 下标; 运算
   pair: 切片; 运算
   operator: in
   operator: not in
   single: count() (序列方法)
   single: index() (序列方法)

+--------------------------+-------------------------------------+----------+
| 运算                     | 结果                                | 备注     |
+==========================+=====================================+==========+
| ``x in s``               | 如果 *s* 中的某个元素与 *x* 相      | \(1)     |
|                          | 等则为 ``True`` ，否则为 ``False``  |          |
+--------------------------+-------------------------------------+----------+
| ``x not in s``           | 如果 *s* 中的某个元素与 *x* 相      | \(1)     |
|                          | 等则为 ``False`` ，否则为 ``True``  |          |
+--------------------------+-------------------------------------+----------+
| ``s + t``                | *s* 和 *t* 的拼接                   | (6)(7)   |
+--------------------------+-------------------------------------+----------+
| ``s * n`` 或者           | 对 *s* 进行 *n* 次浅复制并拼接      | (2)(7)   |
| ``n * s``                |                                     |          |
+--------------------------+-------------------------------------+----------+
| ``s[i]``                 | *s* 中第 *i* 个元素，从 0 算起      | \(3)     |
+--------------------------+-------------------------------------+----------+
| ``s[i:j]``               | *s* 中从 *i* 支 *j* 的切片          | (3)(4)   |
+--------------------------+-------------------------------------+----------+
| ``s[i:j:k]``             | *s* 中从 *i* 支 *j* 的切片，间隔    | (3)(5)   |
|                          | *k*                                 |          |
+--------------------------+-------------------------------------+----------+
| ``len(s)``               | *s* 的长度                          |          |
+--------------------------+-------------------------------------+----------+
| ``min(s)``               | *s* 中最小的元素                    |          |
+--------------------------+-------------------------------------+----------+
| ``max(s)``               | *s* 中最大的元素                    |          |
+--------------------------+-------------------------------------+----------+
| ``s.index(x[, i[, j]])`` | *s* 中第一个 *x* 出现的下标(在 *i*  | \(8)     |
|                          | 下标处或之后，而在 *j* 下标前)      |          |
+--------------------------+-------------------------------------+----------+
| ``s.count(x)``           | *s* 中 *x* 出现的总次数             |          |
+--------------------------+-------------------------------------+----------+

同种类型的序列还支持比较运算。特别的，元组和列表在比较时会按照字母顺序比较其对应的元素。这意味着，如果比较结果相等，则每个元素必须相等，并且两个序列必须类型相同且长度一样。(详情参见语言参考中的\ :ref:`比较`\ 。)

备注：

(1)
   尽管一般情况下 ``in`` 和 ``not in`` 运算只用于简单的包含检测，一些专门的序列(例如 :class:`str` 、\ :class:`bytes` 和 :class:`bytearray`)还用它们进行子序列检测::

      >>> "gg" in "eggs"
      True

(2)
   小于 ``0`` 的 *n* 值会当成 ``0`` 处理(这时会生成一个空的列表，其类型与 *s* 相同)。还要注意这里使用浅复制，不会复制嵌套的结构。这常常会让 Python 新手困惑，例如::

      >>> lists = [[]] * 3
      >>> lists
      [[], [], []]
      >>> lists[0].append(3)
      >>> lists
      [[3], [3], [3]]

   事实上，\ ``[[]]`` 是个单元素列表，它只含有一个空列表，所以 ``[[]] * 3`` 的三个元素都是(指向)这个唯一的空列表。修改 ``lists`` 中的任何元素都会改变这个唯一的列表。你可以像这样来创建一个包含不同列表的列表::

      >>> lists = [[] for i in range(3)]
      >>> lists[0].append(3)
      >>> lists[1].append(5)
      >>> lists[2].append(7)
      >>> lists
      [[3], [5], [7]]

(3)
   如果 *i* 或 *j* 是负的，则下标是相对于字符串结尾，即代以 ``len(s) + i`` 或 ``len(s) + j``\ 。但是要注意，\ ``-0`` 还是 ``0`` 。

(4)
   *s* 中从 *i* 到 *j* 的切片定义为下标为 *k* 的元素组成序列，使得 ``i <= k < j`` 。如果 *i* 或 *j* 大于 ``len(s)`` ，则使用 ``len(s)`` 。如果 *i* 省略或者是 ``None`` ，则用 ``0`` 。如果 *j* 省略或者为 ``None`` ，则用 ``len(s)`` 。如果 *i* 大于或等于 *j* ，则生成的序列为空。

(5)
   *s* 中从 *i* 到 *j* 且步长为 *k* 的切片定义为下标为 *k* 的元素组成序列，使得 ``x = i + n*k`` ，其中 ``0 <= n < (j-i)/k`` 。换句话说，下标是 ``i`` 、\ ``i+k`` 、\ ``i+2*k`` 、\ ``i+3*k`` 等等，到达 *j* 时停止(但从来不会包含 *j*)。如果 *i* 或 *j* 大于 ``len(s)`` 则使用 ``len(s)`` 。如果 *i* 或 *j* 省略或者为 ``None`` ，就会使用"未尾"值(使用哪一头未尾取决于 *k* 的正负符号)。注意，\ *k* 不能为零。
   如果 *k* 是 ``None`` ，则当它为 ``1`` 。

(6)
   拼接不可变序列总是生成一个新的对象。这意味着如果通过连续的拼接来构建一个序列，其时间复杂度为序列总长度的二次方程。为了取得线性的时间开销，你必须转而使用下面的方法之一：

   * 如果要拼接 :class:`str` 对象，你可以构建一个列表，最后使用 :meth:`str.join` 或者写到一个 :class:`io.StringIO` 实例并在完成后取回它的值。

   * 如果要拼接 :class:`bytes` 对象，你可以使用类似的 :meth:`bytes.join` 或者 :class:`io.BytesIO` ，或者使用 :class:`bytearray` 对象在原地拼接。\ :class:`bytearray` 对象是可变的，它有高效的重复分配内存机制。

   * 如果要拼接 :class:`tuple` 对象，应该转而扩展 :class:`list` 。

   * 对于其它类型，需要研读相关的类文档。


(7)
  有些序列类型(例如 :class:`range`)只支持拥有特定模式的元素序列，所以并不支持序列拼接或者重复。

(8)
   如果 *s* 中找不到 *x* ，\ ``index`` 就会抛出 :exc:`ValueError` 异常。如果支持的话，index 方法的额外参数会让序列中的片断搜索变得高效。传入多余的参数大致相当于使用 ``s[i:j].index(x)`` ，只不过没有复制任何数据，并且返回的下标是相对于整个序列的开头，而不是切片的开头。


.. _typesseq-immutable:

不可变序列类型
------------------------

.. index::
   triple: 不可变; 序列; 类型
   object: 元组
   builtin: hash

通常由不可变序列类型实现的，而可变序列却缺少的唯一运算就是对 :func:`hash` 内置函数的支持。

这种支持让不可变序列，例如 :class:`tuple` 实例，可以用作 :class:`dict` 的键，或者存储在 :class:`set` 和 :class:`frozenset` 实例中。

如果试图对含有不可散列值的不可变序列进行散列，就会导致 :exc:`TypeError` 。


.. _typesseq-mutable:

可变序列类型
----------------------

.. index::
   triple: 可变; 序列; 类型
   object: list
   object: bytearray

下表列出的运算是针对可变序列类型的。\ :class:`collections.abc.MutableSequence` 虚基类使得自定义序列类型正确实现这些运算变得容易。

表中的 *s* 是一个可变序列类型的实例；\ *t* 是任意的可迭代对象；\ *x* 是个任意的对象，它能满足 *s* 对类型和值的限制(例如，\ :class:`bytearray` 只接受那些值为 ``0 <= x <= 255`` 的整数。


.. index::
   triple: 运算; 序列; 类型
   triple: 运算; 列表; 类型
   pair: 下标; 赋值
   pair: 切片; 赋值
   statement: del
   single: append() (序列方法)
   single: clear() (序列方法)
   single: copy() (序列方法)
   single: extend() (序列方法)
   single: insert() (序列方法)
   single: pop() (序列方法)
   single: remove() (序列方法)
   single: reverse() (序列方法)

+--------------------+----------------------------------+----------+
| 运算               | 结果                             | 备注     |
+====================+==================================+==========+
| ``s[i] = x``       | 把 *s* 的元素 *i* 替换为 *x*     |          |
+--------------------+----------------------------------+----------+
| ``s[i:j] = t``     | 把 *s* 中从 *i* 到 *j* 的切片    |          |
|                    | 替换为可迭代对象 *t* 的内容      |          |
+--------------------+----------------------------------+----------+
| ``del s[i:j]``     | 和 ``s[i:j] = []`` 相同          |          |
+--------------------+----------------------------------+----------+
| ``s[i:j:k] = t``   | 把 ``s[i:j:k]`` 中的元素用       | \(1)     |
|                    | *t* 中的替换                     |          |
+--------------------+----------------------------------+----------+
| ``del s[i:j:k]``   | 从列表中移除 ``s[i:j:k]`` 中     |          |
|                    | 的元素                           |          |
+--------------------+----------------------------------+----------+
| ``s.append(x)``    | 把 *x* 添加到序列的结尾(和       |          |
|                    | ``s[len(s):len(s)] = [x]`` 相同) |          |
+--------------------+----------------------------------+----------+
| ``s.clear()``      | 移除 ``s`` 中的所有元素          | \(5)     |
|                    | (和 ``del s[:]`` 相同)           |          |
+--------------------+----------------------------------+----------+
| ``s.copy()``       | 对 ``s`` 进行浅复制              | \(5)     |
|                    | (和 ``s[:]`` 相同)               |          |
+--------------------+----------------------------------+----------+
| ``s.extend(t)``    | 把 *t* 的内容扩展到 *s* 中(和    |          |
|                    | ``s[len(s):len(s)] = t`` 相同)   |          |
+--------------------+----------------------------------+----------+
| ``s.insert(i, x)`` | 把 *x* 插入到 *s* 中下标为 *i*   |          |
|                    | 的地方(和 ``s[i:i] = [x]`` 相同) |          |
+--------------------+----------------------------------+----------+
| ``s.pop([i])``     | 取得 *i* 处的元素并把它从 *t* 中 | \(2)     |
|                    | 移除                             |          |
+--------------------+----------------------------------+----------+
| ``s.remove(x)``    | 移除 *s* 中第一个满足            | \(3)     |
|                    |  ``s[i] == x`` 的元素            |          |
+--------------------+----------------------------------+----------+
| ``s.reverse()``    | 在原地前后对调 *s* 中的元素      | \(4)     |
+--------------------+----------------------------------+----------+


备注：

(1)
   *t* 必须和它要替换的切片有相同的长度。

(2)
   可选参数 *i* 默认为 ``-1`` ，所以默认会删除并返回最后一个元素。

(3)
   如果 *x* 不在 *s* 中，\ ``remove`` 会抛出 :exc:`ValueError` 异常。

(4)
   :meth:`reverse` 方法会在原地修改序列，以便在处理大容量序列时节省空间。为了让用户知道这个运算通过副作用操作，它并不返回对调好的序列。

(5)
   加入 :meth:`clear` 和 :meth:`!copy` 是为了与不支持切片运算的可变容器(例如 :class:`dict` 和 :class:`set`)在界面上保持一致。

   .. versionadded:: 3.3
      :meth:`clear` 和 :meth:`!copy` 方法。


.. _typesseq-list:

列表
-----

.. index:: 对象: 列表

列表是可变序列，常规用于存放性质相同(相似的程序会根据应用而有不同)的一系列项。

.. class:: list([iterable])

   有好几种方法构建一个列表：

   * 使用一对方括号来表示空列表： ``[]``
   * 使用方括号，中间是逗号分隔的各项： ``[a]`` 、\ ``[a, b, c]``
   * 使用列表解析式： ``[x for x in iterable]``
   * 使用类型构造函数： ``list()`` or ``list(iterable)``

   这个构造函数会创建一个列表，其中的元素及其顺序与 *iterable* 的一样。\ *iterable* 可以是序列、支持迭代的容器、或者迭代器对象。如果 *iterable* 已经是个列表，则把它复制一份并返回这个副本，这类似于 ``iterable[:]`` 。例如，\ ``list('abc')`` 返回 ``['a', 'b', 'c']`` ，而 ``list( (1, 2, 3) )`` 返回 ``[1, 2, 3]`` 。如果没有指定参数，构造函数会创建一个空列表 ``[]`` 。

   还有其它很多运算都会产生列表，包括 :func:`sorted` 内置函数。

   列表实现了序列的所有\ :ref:`共同 <typesseq-common>`\ 和\ :ref:`可变 <typesseq-mutable>`\ 运算，还提供下面额外的方法：

   .. method:: list.sort(*, key=None, reverse=None)

      这个方法会在原地对列表排序，排序时只对各项使用 ``<`` 比较。它不会处理异常---如果任何比较运算失败，则整个排序操作就失败(这时该列表可能处于部分被修改的状态)。

      *key* 指定一个单参数的函数，用来从每个列表元素中抽取用于比较的键(例如 ``key=str.lower``)。每个项所对应的键只计算一次，然后用于整个排序过程。默认值 ``None`` 表示直接对列表的各项排序，而不需要另外计算键值。

      实用程序 :func:`functools.cmp_to_key` 用来把 2.x 风格的 *cmp* 函数转化为 *key* 函数。

      *reverse* 是个布尔值。如果设为 ``True`` ，则对列表元素排序时对每次比较结果取反。

      这个方法会在原地修改序列，这样可以在对大型序列排序时节省空间。为了让用户知道它是通过副作用操作的，它并不返回排序好的序列(如果明确要返回排序好的列表实例，可以使用 :func:`sorted`)。

      :meth:`sort` 方法的排序是稳定的。稳定的排序能保证在元素比较结果相等时，不改变它们的相对顺序---这对于多次排序来说很有用(例如先根据部门排序，然后再按照工资等级排序)。

      .. impl-detail::

         在对一个列表进行排序时，试图对它进行改变，甚至只是查看，所带来的影响，是没有定义的。Python 的 C 实现在这段时间会让列表看起来为空，如果它检测到列表在排序时被改变就会抛出 :exc:`ValueError` 异常。


.. _typesseq-tuple:

元组
------

.. index:: 对象: 元组

元组是不可变的序列，通常用于存储性质相同的一系列数据(例如 :func:`enumerate` 内置函数产生的二元组)。它还用于需要性质相同的不可变序列的地方(例如需要存放在 :class:`set` 或者 :class:`dict` 实例中)。

.. class:: tuple([iterable])

   有好几种方法构建元组：

   * 使用一对圆括号表示空元组： ``()``
   * 用结尾的逗号表示一个单元素的元组： ``a,`` 或 ``(a,)``
   * 用逗号分隔各个项： ``a, b, c`` 或 ``(a, b, c)``
   * 使用 :func:`tuple` 内置函数： ``tuple()`` 或 ``tuple(iterable)``

   这个构造函数会构建一个元组，它的元组及其顺序与 *iterable* 的相同。\ *iterable* 可以是序列、支持迭代协议的容器、或者一个迭代器对象。如果 *iterable* 已经是 个元组，则原封不动的返回它。例如，\ ``tuple('abc')`` 返回 ``('a', 'b', 'c')`` ，而 ``tuple( [1, 2, 3] )`` 返回 ``(1, 2, 3)`` 。如果没有指定参数，则构造函数会创建一个空元组 ``()`` 。

   注意，创建了元组的实际上是逗号，而不是括号。括号的可选的，除非是对于空元组，或者为了避免语法岐义。例如，\ ``f(a, b, c)`` 是个带三个参数的函数调用，而 ``f((a, b, c))`` 是个带有三元组参数的函数调用。

   元组实现了序列的所有\ :ref:`共同 <typesseq-common>`\ 运算。

对于性质相同的系列数据，如果按名称访问比按下标位置访问更清楚，则 :func:`collections.namedtuple` 相对于简单的元组对象来说可能是更好的选择。


.. _typesseq-range:

区间
------

.. index:: 对象: 区间

:class:`range` 类型表示不可变的一系列数，通常用于 :keyword:`for` 语句中循环指定的次数。

.. class:: range(stop)
           range(start, stop[, step])

   range 构造函数的参数必须是整数(内置的 :class:`int` 或任何实现了 ``__index__`` 特殊方法的对象)。如果 *step* 参数省略，则默认为 ``1`` 。如果 *start* 参数省略，则默认为 ``0`` 。如果 *step* 是零，则抛出 :exc:`ValueError` 异常。

   对于为正数的 *step* ，则区间 ``r`` 的内容由公式 ``r[i] = start + step*i`` 决定，其中 ``i >= 0`` 且 ``r[i] < stop`` 。

   对于负数的 *step* ，区间的内容仍然由公式 ``r[i] = start + step*i`` 决定，但是约束条件是 ``i >= 0`` 且 ``r[i] > stop`` 。

   如果 ``r[0]`` 不符合约束条件，则区间对象为空。尽管区间也支持负数的下标，但它是从基于正数下标的序列结尾开始算起的。

   包含绝对值比 :data:`sys.maxsize` 还要大的区间是允许的，但是有些功能(例如 :func:`len`)可能会抛出 :exc:`OverflowError` 异常。

   区间的例子::

      >>> list(range(10))
      [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
      >>> list(range(1, 11))
      [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
      >>> list(range(0, 30, 5))
      [0, 5, 10, 15, 20, 25]
      >>> list(range(0, 10, 3))
      [0, 3, 6, 9]
      >>> list(range(0, -10, -1))
      [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
      >>> list(range(0))
      []
      >>> list(range(1, 0))
      []

   区间实现了序列的所有\ :ref:`共同 <typesseq-common>`\ 运算，除了拼接和重复(因为区间对象只能接受严格遵循规则的序列，而重复和拼接则通常违反这个规则)。

.. XXX This secion does not RENDERRRRR!

   .. data: start

      *start* 参数的值(如果没有指定这个参数则为 ``0``)

   .. data: stop

      *stop* 参数的值

   .. data: step

      *step* 参数的值(如果没有指定这个参数则为 ``1``)

:class:`range` 类型比普通的 :class:`list` 或 :class:`tuple` 优越之处在于，\ :class:`range` 对象总是占用相同(少量)的内存，不管它所表示的区间有多大(因为它只存储 ``start`` 、\ ``stop`` 和 ``step`` 值，在需要时才计算具体的项以及子区间)。

区间对象实现了 :class:`collections.Sequence` 虚基类，提供了包容检测、元素按下标查找、切片、以及负数的下标等功能(参见\ :ref:`typesseq`)：

   >>> r = range(0, 20, 2)
   >>> r
   range(0, 20, 2)
   >>> 11 in r
   False
   >>> 10 in r
   True
   >>> r.index(10)
   5
   >>> r[5]
   10
   >>> r[:5]
   range(0, 10, 2)
   >>> r[-1]
   18

如果对区间对象用 ``==`` 和 ``!=`` 进行比较判断，就会把它们当作序列来操作。也就是说，如果两个区间对象表示相同序列的值，则认为它们相等。(注意，两个比较结果相等的区间可能有不同的 :attr:`start` 、\ :attr:`stop` 和 :attr:`step` 属性，例如 ``range(0) == range(2, 1, 3)`` 或 ``range(0, 3, 2) == range(0, 4, 2)`` 。)

.. versionchanged:: 3.2
   实现了序列虚基类。支持切片和负数下标。在固定时间内检测 :class:`int` 对象的是否是其成员，而不是对所有项进行迭代。

.. versionchanged:: 3.3
   定义了 '==' 和 '!=' 用来根据其定义的序列比较区间对象(而不是根据对象身份比较)。

.. versionadded:: 3.3
   :attr:`start` 、\ :attr:`stop` 和 :attr:`step` 属性。


.. index::
   single: 字符串; 文本序列类型
   single: str (内置类); (另见 string)
   object: string

.. _textseq:

Text Sequence Type --- :class:`str`
===================================

Textual data in Python is handled with :class:`str` objects, or :dfn:`strings`.
Strings are immutable
:ref:`sequences <typesseq>` of Unicode code points.  String literals are
written in a variety of ways:

* Single quotes: ``'allows embedded "double" quotes'``
* Double quotes: ``"allows embedded 'single' quotes"``.
* Triple quoted: ``'''Three single quotes'''``, ``"""Three double quotes"""``

Triple quoted strings may span multiple lines - all associated whitespace will
be included in the string literal.

String literals that are part of a single expression and have only whitespace
between them will be implicitly converted to a single string literal. That
is, ``("spam " "eggs") == "spam eggs"``.

See :ref:`strings` for more about the various forms of string literal,
including supported escape sequences, and the ``r`` ("raw") prefix that
disables most escape sequence processing.

Strings may also be created from other objects using the :class:`str`
constructor.

Since there is no separate "character" type, indexing a string produces
strings of length 1. That is, for a non-empty string *s*, ``s[0] == s[0:1]``.

.. index::
   object: io.StringIO

There is also no mutable string type, but :meth:`str.join` or
:class:`io.StringIO` can be used to efficiently construct strings from
multiple fragments.

.. versionchanged:: 3.3
   For backwards compatibility with the Python 2 series, the ``u`` prefix is
   once again permitted on string literals. It has no effect on the meaning
   of string literals and cannot be combined with the ``r`` prefix.


.. index::
   single: string; str (built-in class)

.. class:: str(object='')
           str(object=b'', encoding='utf-8', errors='strict')

   Return a :ref:`string <textseq>` version of *object*.  If *object* is not
   provided, returns the empty string.  Otherwise, the behavior of ``str()``
   depends on whether *encoding* or *errors* is given, as follows.

   If neither *encoding* nor *errors* is given, ``str(object)`` returns
   :meth:`object.__str__() <object.__str__>`, which is the "informal" or nicely
   printable string representation of *object*.  For string objects, this is
   the string itself.  If *object* does not have a :meth:`~object.__str__`
   method, then :func:`str` falls back to returning
   :meth:`repr(object) <repr>`.

   .. index::
      single: buffer protocol; str (built-in class)
      single: bytes; str (built-in class)

   If at least one of *encoding* or *errors* is given, *object* should be a
   :class:`bytes` or :class:`bytearray` object, or more generally any object
   that supports the :ref:`buffer protocol <bufferobjects>`.  In this case, if
   *object* is a :class:`bytes` (or :class:`bytearray`) object, then
   ``str(bytes, encoding, errors)`` is equivalent to
   :meth:`bytes.decode(encoding, errors) <bytes.decode>`.  Otherwise, the bytes
   object underlying the buffer object is obtained before calling
   :meth:`bytes.decode`.  See :ref:`binaryseq` and
   :ref:`bufferobjects` for information on buffer objects.

   Passing a :class:`bytes` object to :func:`str` without the *encoding*
   or *errors* arguments falls under the first case of returning the informal
   string representation (see also the :option:`-b` command-line option to
   Python).  For example::

      >>> str(b'Zoot!')
      "b'Zoot!'"

   For more information on the ``str`` class and its methods, see
   :ref:`textseq` and the :ref:`string-methods` section below.  To output
   formatted strings, see the :ref:`string-formatting` section.  In addition,
   see the :ref:`stringservices` section.


.. index::
   pair: string; methods

.. _string-methods:

String Methods
--------------

.. index::
   module: re

Strings implement all of the :ref:`common <typesseq-common>` sequence
operations, along with the additional methods described below.

Strings also support two styles of string formatting, one providing a large
degree of flexibility and customization (see :meth:`str.format`,
:ref:`formatstrings` and :ref:`string-formatting`) and the other based on C
``printf`` style formatting that handles a narrower range of types and is
slightly harder to use correctly, but is often faster for the cases it can
handle (:ref:`old-string-formatting`).

The :ref:`textservices` section of the standard library covers a number of
other modules that provide various text related utilities (including regular
expression support in the :mod:`re` module).

.. method:: str.capitalize()

   Return a copy of the string with its first character capitalized and the
   rest lowercased.


.. method:: str.casefold()

   Return a casefolded copy of the string. Casefolded strings may be used for
   caseless matching.

   Casefolding is similar to lowercasing but more aggressive because it is
   intended to remove all case distinctions in a string. For example, the German
   lowercase letter ``'ß'`` is equivalent to ``"ss"``. Since it is already
   lowercase, :meth:`lower` would do nothing to ``'ß'``; :meth:`casefold`
   converts it to ``"ss"``.

   The casefolding algorithm is described in section 3.13 of the Unicode
   Standard.

   .. versionadded:: 3.3


.. method:: str.center(width[, fillchar])

   Return centered in a string of length *width*. Padding is done using the
   specified *fillchar* (default is a space).


.. method:: str.count(sub[, start[, end]])

   Return the number of non-overlapping occurrences of substring *sub* in the
   range [*start*, *end*].  Optional arguments *start* and *end* are
   interpreted as in slice notation.


.. method:: str.encode(encoding="utf-8", errors="strict")

   Return an encoded version of the string as a bytes object. Default encoding
   is ``'utf-8'``. *errors* may be given to set a different error handling scheme.
   The default for *errors* is ``'strict'``, meaning that encoding errors raise
   a :exc:`UnicodeError`. Other possible
   values are ``'ignore'``, ``'replace'``, ``'xmlcharrefreplace'``,
   ``'backslashreplace'`` and any other name registered via
   :func:`codecs.register_error`, see section :ref:`codec-base-classes`. For a
   list of possible encodings, see section :ref:`standard-encodings`.

   .. versionchanged:: 3.1
      Support for keyword arguments added.


.. method:: str.endswith(suffix[, start[, end]])

   Return ``True`` if the string ends with the specified *suffix*, otherwise return
   ``False``.  *suffix* can also be a tuple of suffixes to look for.  With optional
   *start*, test beginning at that position.  With optional *end*, stop comparing
   at that position.


.. method:: str.expandtabs([tabsize])

   Return a copy of the string where all tab characters are replaced by zero or
   more spaces, depending on the current column and the given tab size.  The
   column number is reset to zero after each newline occurring in the string.
   If *tabsize* is not given, a tab size of ``8`` characters is assumed.  This
   doesn't understand other non-printing characters or escape sequences.


.. method:: str.find(sub[, start[, end]])

   Return the lowest index in the string where substring *sub* is found, such
   that *sub* is contained in the slice ``s[start:end]``.  Optional arguments
   *start* and *end* are interpreted as in slice notation.  Return ``-1`` if
   *sub* is not found.

   .. note::

      The :meth:`~str.find` method should be used only if you need to know the
      position of *sub*.  To check if *sub* is a substring or not, use the
      :keyword:`in` operator::

         >>> 'Py' in 'Python'
         True


.. method:: str.format(*args, **kwargs)

   Perform a string formatting operation.  The string on which this method is
   called can contain literal text or replacement fields delimited by braces
   ``{}``.  Each replacement field contains either the numeric index of a
   positional argument, or the name of a keyword argument.  Returns a copy of
   the string where each replacement field is replaced with the string value of
   the corresponding argument.

      >>> "The sum of 1 + 2 is {0}".format(1+2)
      'The sum of 1 + 2 is 3'

   See :ref:`formatstrings` for a description of the various formatting options
   that can be specified in format strings.


.. method:: str.format_map(mapping)

   Similar to ``str.format(**mapping)``, except that ``mapping`` is
   used directly and not copied to a :class:`dict` .  This is useful
   if for example ``mapping`` is a dict subclass:

   >>> class Default(dict):
   ...     def __missing__(self, key):
   ...         return key
   ...
   >>> '{name} was born in {country}'.format_map(Default(name='Guido'))
   'Guido was born in country'

   .. versionadded:: 3.2


.. method:: str.index(sub[, start[, end]])

   Like :meth:`find`, but raise :exc:`ValueError` when the substring is not found.


.. method:: str.isalnum()

   Return true if all characters in the string are alphanumeric and there is at
   least one character, false otherwise.  A character ``c`` is alphanumeric if one
   of the following returns ``True``: ``c.isalpha()``, ``c.isdecimal()``,
   ``c.isdigit()``, or ``c.isnumeric()``.


.. method:: str.isalpha()

   Return true if all characters in the string are alphabetic and there is at least
   one character, false otherwise.  Alphabetic characters are those characters defined
   in the Unicode character database as "Letter", i.e., those with general category
   property being one of "Lm", "Lt", "Lu", "Ll", or "Lo".  Note that this is different
   from the "Alphabetic" property defined in the Unicode Standard.


.. method:: str.isdecimal()

   Return true if all characters in the string are decimal
   characters and there is at least one character, false
   otherwise. Decimal characters are those from general category "Nd". This category
   includes digit characters, and all characters
   that can be used to form decimal-radix numbers, e.g. U+0660,
   ARABIC-INDIC DIGIT ZERO.


.. method:: str.isdigit()

   Return true if all characters in the string are digits and there is at least one
   character, false otherwise.  Digits include decimal characters and digits that need
   special handling, such as the compatibility superscript digits.  Formally, a digit
   is a character that has the property value Numeric_Type=Digit or Numeric_Type=Decimal.


.. method:: str.isidentifier()

   Return true if the string is a valid identifier according to the language
   definition, section :ref:`identifiers`.


.. method:: str.islower()

   Return true if all cased characters [4]_ in the string are lowercase and
   there is at least one cased character, false otherwise.


.. method:: str.isnumeric()

   Return true if all characters in the string are numeric
   characters, and there is at least one character, false
   otherwise. Numeric characters include digit characters, and all characters
   that have the Unicode numeric value property, e.g. U+2155,
   VULGAR FRACTION ONE FIFTH.  Formally, numeric characters are those with the property
   value Numeric_Type=Digit, Numeric_Type=Decimal or Numeric_Type=Numeric.


.. method:: str.isprintable()

   Return true if all characters in the string are printable or the string is
   empty, false otherwise.  Nonprintable characters are those characters defined
   in the Unicode character database as "Other" or "Separator", excepting the
   ASCII space (0x20) which is considered printable.  (Note that printable
   characters in this context are those which should not be escaped when
   :func:`repr` is invoked on a string.  It has no bearing on the handling of
   strings written to :data:`sys.stdout` or :data:`sys.stderr`.)


.. method:: str.isspace()

   Return true if there are only whitespace characters in the string and there is
   at least one character, false otherwise.  Whitespace characters  are those
   characters defined in the Unicode character database as "Other" or "Separator"
   and those with bidirectional property being one of "WS", "B", or "S".

.. method:: str.istitle()

   Return true if the string is a titlecased string and there is at least one
   character, for example uppercase characters may only follow uncased characters
   and lowercase characters only cased ones.  Return false otherwise.


.. method:: str.isupper()

   Return true if all cased characters [4]_ in the string are uppercase and
   there is at least one cased character, false otherwise.


.. method:: str.join(iterable)

   Return a string which is the concatenation of the strings in the
   :term:`可迭代对象` *iterable*.  A :exc:`TypeError` will be raised if there are
   any non-string values in *iterable*, including :class:`bytes` objects.  The
   separator between elements is the string providing this method.


.. method:: str.ljust(width[, fillchar])

   Return the string left justified in a string of length *width*. Padding is done
   using the specified *fillchar* (default is a space).  The original string is
   returned if *width* is less than or equal to ``len(s)``.


.. method:: str.lower()

   Return a copy of the string with all the cased characters [4]_ converted to
   lowercase.

   The lowercasing algorithm used is described in section 3.13 of the Unicode
   Standard.


.. method:: str.lstrip([chars])

   Return a copy of the string with leading characters removed.  The *chars*
   argument is a string specifying the set of characters to be removed.  If omitted
   or ``None``, the *chars* argument defaults to removing whitespace.  The *chars*
   argument is not a prefix; rather, all combinations of its values are stripped:

      >>> '   spacious   '.lstrip()
      'spacious   '
      >>> 'www.example.com'.lstrip('cmowz.')
      'example.com'


.. staticmethod:: str.maketrans(x[, y[, z]])

   This static method returns a translation table usable for :meth:`str.translate`.

   If there is only one argument, it must be a dictionary mapping Unicode
   ordinals (integers) or characters (strings of length 1) to Unicode ordinals,
   strings (of arbitrary lengths) or None.  Character keys will then be
   converted to ordinals.

   If there are two arguments, they must be strings of equal length, and in the
   resulting dictionary, each character in x will be mapped to the character at
   the same position in y.  If there is a third argument, it must be a string,
   whose characters will be mapped to None in the result.


.. method:: str.partition(sep)

   Split the string at the first occurrence of *sep*, and return a 3-tuple
   containing the part before the separator, the separator itself, and the part
   after the separator.  If the separator is not found, return a 3-tuple containing
   the string itself, followed by two empty strings.


.. method:: str.replace(old, new[, count])

   Return a copy of the string with all occurrences of substring *old* replaced by
   *new*.  If the optional argument *count* is given, only the first *count*
   occurrences are replaced.


.. method:: str.rfind(sub[, start[, end]])

   Return the highest index in the string where substring *sub* is found, such
   that *sub* is contained within ``s[start:end]``.  Optional arguments *start*
   and *end* are interpreted as in slice notation.  Return ``-1`` on failure.


.. method:: str.rindex(sub[, start[, end]])

   Like :meth:`rfind` but raises :exc:`ValueError` when the substring *sub* is not
   found.


.. method:: str.rjust(width[, fillchar])

   Return the string right justified in a string of length *width*. Padding is done
   using the specified *fillchar* (default is a space). The original string is
   returned if *width* is less than or equal to ``len(s)``.


.. method:: str.rpartition(sep)

   Split the string at the last occurrence of *sep*, and return a 3-tuple
   containing the part before the separator, the separator itself, and the part
   after the separator.  If the separator is not found, return a 3-tuple containing
   two empty strings, followed by the string itself.


.. method:: str.rsplit(sep=None, maxsplit=-1)

   Return a list of the words in the string, using *sep* as the delimiter string.
   If *maxsplit* is given, at most *maxsplit* splits are done, the *rightmost*
   ones.  If *sep* is not specified or ``None``, any whitespace string is a
   separator.  Except for splitting from the right, :meth:`rsplit` behaves like
   :meth:`split` which is described in detail below.


.. method:: str.rstrip([chars])

   Return a copy of the string with trailing characters removed.  The *chars*
   argument is a string specifying the set of characters to be removed.  If omitted
   or ``None``, the *chars* argument defaults to removing whitespace.  The *chars*
   argument is not a suffix; rather, all combinations of its values are stripped:

      >>> '   spacious   '.rstrip()
      '   spacious'
      >>> 'mississippi'.rstrip('ipz')
      'mississ'


.. method:: str.split(sep=None, maxsplit=-1)

   Return a list of the words in the string, using *sep* as the delimiter
   string.  If *maxsplit* is given, at most *maxsplit* splits are done (thus,
   the list will have at most ``maxsplit+1`` elements).  If *maxsplit* is not
   specified or ``-1``, then there is no limit on the number of splits
   (all possible splits are made).

   If *sep* is given, consecutive delimiters are not grouped together and are
   deemed to delimit empty strings (for example, ``'1,,2'.split(',')`` returns
   ``['1', '', '2']``).  The *sep* argument may consist of multiple characters
   (for example, ``'1<>2<>3'.split('<>')`` returns ``['1', '2', '3']``).
   Splitting an empty string with a specified separator returns ``['']``.

   If *sep* is not specified or is ``None``, a different splitting algorithm is
   applied: runs of consecutive whitespace are regarded as a single separator,
   and the result will contain no empty strings at the start or end if the
   string has leading or trailing whitespace.  Consequently, splitting an empty
   string or a string consisting of just whitespace with a ``None`` separator
   returns ``[]``.

   For example, ``' 1  2   3  '.split()`` returns ``['1', '2', '3']``, and
   ``'  1  2   3  '.split(None, 1)`` returns ``['1', '2   3  ']``.


.. index::
   single: universal newlines; str.splitlines method

.. method:: str.splitlines([keepends])

   Return a list of the lines in the string, breaking at line boundaries.
   This method uses the :term:`万能换行符` approach to splitting lines.
   Line breaks are not included in the resulting list unless *keepends* is
   given and true.

   For example, ``'ab c\n\nde fg\rkl\r\n'.splitlines()`` returns
   ``['ab c', '', 'de fg', 'kl']``, while the same call with ``splitlines(True)``
   returns ``['ab c\n', '\n', 'de fg\r', 'kl\r\n']``.

   Unlike :meth:`~str.split` when a delimiter string *sep* is given, this
   method returns an empty list for the empty string, and a terminal line
   break does not result in an extra line.


.. method:: str.startswith(prefix[, start[, end]])

   Return ``True`` if string starts with the *prefix*, otherwise return ``False``.
   *prefix* can also be a tuple of prefixes to look for.  With optional *start*,
   test string beginning at that position.  With optional *end*, stop comparing
   string at that position.


.. method:: str.strip([chars])

   Return a copy of the string with the leading and trailing characters removed.
   The *chars* argument is a string specifying the set of characters to be removed.
   If omitted or ``None``, the *chars* argument defaults to removing whitespace.
   The *chars* argument is not a prefix or suffix; rather, all combinations of its
   values are stripped:

      >>> '   spacious   '.strip()
      'spacious'
      >>> 'www.example.com'.strip('cmowz.')
      'example'


.. method:: str.swapcase()

   Return a copy of the string with uppercase characters converted to lowercase and
   vice versa. Note that it is not necessarily true that
   ``s.swapcase().swapcase() == s``.


.. method:: str.title()

   Return a titlecased version of the string where words start with an uppercase
   character and the remaining characters are lowercase.

   The algorithm uses a simple language-independent definition of a word as
   groups of consecutive letters.  The definition works in many contexts but
   it means that apostrophes in contractions and possessives form word
   boundaries, which may not be the desired result::

        >>> "they're bill's friends from the UK".title()
        "They'Re Bill'S Friends From The Uk"

   A workaround for apostrophes can be constructed using regular expressions::

        >>> import re
        >>> def titlecase(s):
        ...     return re.sub(r"[A-Za-z]+('[A-Za-z]+)?",
        ...                   lambda mo: mo.group(0)[0].upper() +
        ...                              mo.group(0)[1:].lower(),
        ...                   s)
        ...
        >>> titlecase("they're bill's friends.")
        "They're Bill's Friends."


.. method:: str.translate(map)

   Return a copy of the *s* where all characters have been mapped through the
   *map* which must be a dictionary of Unicode ordinals (integers) to Unicode
   ordinals, strings or ``None``.  Unmapped characters are left untouched.
   Characters mapped to ``None`` are deleted.

   You can use :meth:`str.maketrans` to create a translation map from
   character-to-character mappings in different formats.

   .. note::

      An even more flexible approach is to create a custom character mapping
      codec using the :mod:`codecs` module (see :mod:`encodings.cp1251` for an
      example).


.. method:: str.upper()

   Return a copy of the string with all the cased characters [4]_ converted to
   uppercase.  Note that ``str.upper().isupper()`` might be ``False`` if ``s``
   contains uncased characters or if the Unicode category of the resulting
   character(s) is not "Lu" (Letter, uppercase), but e.g. "Lt" (Letter,
   titlecase).

   The uppercasing algorithm used is described in section 3.13 of the Unicode
   Standard.


.. method:: str.zfill(width)

   Return the numeric string left filled with zeros in a string of length
   *width*.  A sign prefix is handled correctly.  The original string is
   returned if *width* is less than or equal to ``len(s)``.



.. _old-string-formatting:

``printf``-style String Formatting
----------------------------------

.. index::
   single: formatting, string (%)
   single: interpolation, string (%)
   single: string; formatting
   single: string; interpolation
   single: printf-style formatting
   single: sprintf-style formatting
   single: % formatting
   single: % interpolation

.. note::

   The formatting operations described here exhibit a variety of quirks that
   lead to a number of common errors (such as failing to display tuples and
   dictionaries correctly).  Using the newer :meth:`str.format` interface
   helps avoid these errors, and also provides a generally more powerful,
   flexible and extensible approach to formatting text.

String objects have one unique built-in operation: the ``%`` operator (modulo).
This is also known as the string *formatting* or *interpolation* operator.
Given ``format % values`` (where *format* is a string), ``%`` conversion
specifications in *format* are replaced with zero or more elements of *values*.
The effect is similar to using the :c:func:`sprintf` in the C language.

If *format* requires a single argument, *values* may be a single non-tuple
object. [5]_  Otherwise, *values* must be a tuple with exactly the number of
items specified by the format string, or a single mapping object (for example, a
dictionary).

A conversion specifier contains two or more characters and has the following
components, which must occur in this order:

#. The ``'%'`` character, which marks the start of the specifier.

#. Mapping key (optional), consisting of a parenthesised sequence of characters
   (for example, ``(somename)``).

#. Conversion flags (optional), which affect the result of some conversion
   types.

#. Minimum field width (optional).  If specified as an ``'*'`` (asterisk), the
   actual width is read from the next element of the tuple in *values*, and the
   object to convert comes after the minimum field width and optional precision.

#. Precision (optional), given as a ``'.'`` (dot) followed by the precision.  If
   specified as ``'*'`` (an asterisk), the actual precision is read from the next
   element of the tuple in *values*, and the value to convert comes after the
   precision.

#. Length modifier (optional).

#. Conversion type.

When the right argument is a dictionary (or other mapping type), then the
formats in the string *must* include a parenthesised mapping key into that
dictionary inserted immediately after the ``'%'`` character. The mapping key
selects the value to be formatted from the mapping.  For example:

   >>> print('%(language)s has %(number)03d quote types.' %
   ...       {'language': "Python", "number": 2})
   Python has 002 quote types.

In this case no ``*`` specifiers may occur in a format (since they require a
sequential parameter list).

The conversion flag characters are:

+---------+---------------------------------------------------------------------+
| Flag    | Meaning                                                             |
+=========+=====================================================================+
| ``'#'`` | The value conversion will use the "alternate form" (where defined   |
|         | below).                                                             |
+---------+---------------------------------------------------------------------+
| ``'0'`` | The conversion will be zero padded for numeric values.              |
+---------+---------------------------------------------------------------------+
| ``'-'`` | The converted value is left adjusted (overrides the ``'0'``         |
|         | conversion if both are given).                                      |
+---------+---------------------------------------------------------------------+
| ``' '`` | (a space) A blank should be left before a positive number (or empty |
|         | string) produced by a signed conversion.                            |
+---------+---------------------------------------------------------------------+
| ``'+'`` | A sign character (``'+'`` or ``'-'``) will precede the conversion   |
|         | (overrides a "space" flag).                                         |
+---------+---------------------------------------------------------------------+

A length modifier (``h``, ``l``, or ``L``) may be present, but is ignored as it
is not necessary for Python -- so e.g. ``%ld`` is identical to ``%d``.

The conversion types are:

+------------+-----------------------------------------------------+-------+
| Conversion | Meaning                                             | Notes |
+============+=====================================================+=======+
| ``'d'``    | Signed integer decimal.                             |       |
+------------+-----------------------------------------------------+-------+
| ``'i'``    | Signed integer decimal.                             |       |
+------------+-----------------------------------------------------+-------+
| ``'o'``    | Signed octal value.                                 | \(1)  |
+------------+-----------------------------------------------------+-------+
| ``'u'``    | Obsolete type -- it is identical to ``'d'``.        | \(7)  |
+------------+-----------------------------------------------------+-------+
| ``'x'``    | Signed hexadecimal (lowercase).                     | \(2)  |
+------------+-----------------------------------------------------+-------+
| ``'X'``    | Signed hexadecimal (uppercase).                     | \(2)  |
+------------+-----------------------------------------------------+-------+
| ``'e'``    | Floating point exponential format (lowercase).      | \(3)  |
+------------+-----------------------------------------------------+-------+
| ``'E'``    | Floating point exponential format (uppercase).      | \(3)  |
+------------+-----------------------------------------------------+-------+
| ``'f'``    | Floating point decimal format.                      | \(3)  |
+------------+-----------------------------------------------------+-------+
| ``'F'``    | Floating point decimal format.                      | \(3)  |
+------------+-----------------------------------------------------+-------+
| ``'g'``    | Floating point format. Uses lowercase exponential   | \(4)  |
|            | format if exponent is less than -4 or not less than |       |
|            | precision, decimal format otherwise.                |       |
+------------+-----------------------------------------------------+-------+
| ``'G'``    | Floating point format. Uses uppercase exponential   | \(4)  |
|            | format if exponent is less than -4 or not less than |       |
|            | precision, decimal format otherwise.                |       |
+------------+-----------------------------------------------------+-------+
| ``'c'``    | Single character (accepts integer or single         |       |
|            | character string).                                  |       |
+------------+-----------------------------------------------------+-------+
| ``'r'``    | String (converts any Python object using            | \(5)  |
|            | :func:`repr`).                                      |       |
+------------+-----------------------------------------------------+-------+
| ``'s'``    | String (converts any Python object using            | \(5)  |
|            | :func:`str`).                                       |       |
+------------+-----------------------------------------------------+-------+
| ``'a'``    | String (converts any Python object using            | \(5)  |
|            | :func:`ascii`).                                     |       |
+------------+-----------------------------------------------------+-------+
| ``'%'``    | No argument is converted, results in a ``'%'``      |       |
|            | character in the result.                            |       |
+------------+-----------------------------------------------------+-------+

Notes:

(1)
   The alternate form causes a leading zero (``'0'``) to be inserted between
   left-hand padding and the formatting of the number if the leading character
   of the result is not already a zero.

(2)
   The alternate form causes a leading ``'0x'`` or ``'0X'`` (depending on whether
   the ``'x'`` or ``'X'`` format was used) to be inserted between left-hand padding
   and the formatting of the number if the leading character of the result is not
   already a zero.

(3)
   The alternate form causes the result to always contain a decimal point, even if
   no digits follow it.

   The precision determines the number of digits after the decimal point and
   defaults to 6.

(4)
   The alternate form causes the result to always contain a decimal point, and
   trailing zeroes are not removed as they would otherwise be.

   The precision determines the number of significant digits before and after the
   decimal point and defaults to 6.

(5)
   If precision is ``N``, the output is truncated to ``N`` characters.


(7)
   See :pep:`237`.

Since Python strings have an explicit length, ``%s`` conversions do not assume
that ``'\0'`` is the end of the string.

.. XXX Examples?

.. versionchanged:: 3.1
   ``%f`` conversions for numbers whose absolute value is over 1e50 are no
   longer replaced by ``%g`` conversions.


.. index::
   single: buffer protocol; binary sequence types

.. _binaryseq:

Binary Sequence Types --- :class:`bytes`, :class:`bytearray`, :class:`memoryview`
=================================================================================

.. index::
   object: bytes
   object: bytearray
   object: memoryview
   module: array

The core built-in types for manipulating binary data are :class:`bytes` and
:class:`bytearray`. They are supported by :class:`memoryview` which uses
the :ref:`buffer protocol <bufferobjects>` to access the memory of other
binary objects without needing to make a copy.

The :mod:`array` module supports efficient storage of basic data types like
32-bit integers and IEEE754 double-precision floating values.

.. _typebytes:

Bytes
-----

.. index:: object: bytes

Bytes objects are immutable sequences of single bytes. Since many major
binary protocols are based on the ASCII text encoding, bytes objects offer
several methods that are only valid when working with ASCII compatible
data and are closely related to string objects in a variety of other ways.

Firstly, the syntax for bytes literals is largely the same as that for string
literals, except that a ``b`` prefix is added:

* Single quotes: ``b'still allows embedded "double" quotes'``
* Double quotes: ``b"still allows embedded 'single' quotes"``.
* Triple quoted: ``b'''3 single quotes'''``, ``b"""3 double quotes"""``

Only ASCII characters are permitted in bytes literals (regardless of the
declared source code encoding). Any binary values over 127 must be entered
into bytes literals using the appropriate escape sequence.

As with string literals, bytes literals may also use a ``r`` prefix to disable
processing of escape sequences. See :ref:`strings` for more about the various
forms of bytes literal, including supported escape sequences.

While bytes literals and representations are based on ASCII text, bytes
objects actually behave like immutable sequences of integers, with each
value in the sequence restricted such that ``0 <= x < 256`` (attempts to
violate this restriction will trigger :exc:`ValueError`. This is done
deliberately to emphasise that while many binary formats include ASCII based
elements and can be usefully manipulated with some text-oriented algorithms,
this is not generally the case for arbitrary binary data (blindly applying
text processing algorithms to binary data formats that are not ASCII
compatible will usually lead to data corruption).

In addition to the literal forms, bytes objects can be created in a number of
other ways:

* A zero-filled bytes object of a specified length: ``bytes(10)``
* From an iterable of integers: ``bytes(range(20))``
* Copying existing binary data via the buffer protocol:  ``bytes(obj)``

Also see the :ref:`bytes <func-bytes>` built-in.

Since bytes objects are sequences of integers, for a bytes object *b*,
``b[0]`` will be an integer, while ``b[0:1]`` will be a bytes object of
length 1.  (This contrasts with text strings, where both indexing and
slicing will produce a string of length 1)

The representation of bytes objects uses the literal format (``b'...'``)
since it is often more useful than e.g. ``bytes([46, 46, 46])``.  You can
always convert a bytes object into a list of integers using ``list(b)``.


.. note::
   For Python 2.x users: In the Python 2.x series, a variety of implicit
   conversions between 8-bit strings (the closest thing 2.x offers to a
   built-in binary data type) and Unicode strings were permitted. This was a
   backwards compatibility workaround to account for the fact that Python
   originally only supported 8-bit text, and Unicode text was a later
   addition. In Python 3.x, those implicit conversions are gone - conversions
   between 8-bit binary data and Unicode text must be explicit, and bytes and
   string objects will always compare unequal.


.. _typebytearray:

Bytearray Objects
-----------------

.. index:: object: bytearray

:class:`bytearray` objects are a mutable counterpart to :class:`bytes`
objects. There is no dedicated literal syntax for bytearray objects, instead
they are always created by calling the constructor:

* Creating an empty instance: ``bytearray()``
* Creating a zero-filled instance with a given length: ``bytearray(10)``
* From an iterable of integers: ``bytearray(range(20))``
* Copying existing binary data via the buffer protocol:  ``bytearray(b'Hi!')``

As bytearray objects are mutable, they support the
:ref:`mutable <typesseq-mutable>` sequence operations in addition to the
common bytes and bytearray operations described in :ref:`bytes-methods`.

Also see the :ref:`bytearray <func-bytearray>` built-in.


.. _bytes-methods:

Bytes and Bytearray Operations
------------------------------

.. index:: pair: bytes; methods
           pair: bytearray; methods

Both bytes and bytearray objects support the :ref:`common <typesseq-common>`
sequence operations. They interoperate not just with operands of the same
type, but with any object that supports the
:ref:`buffer protocol <bufferobjects>`. Due to this flexibility, they can be
freely mixed in operations without causing errors. However, the return type
of the result may depend on the order of operands.

Due to the common use of ASCII text as the basis for binary protocols, bytes
and bytearray objects provide almost all methods found on text strings, with
the exceptions of:

* :meth:`str.encode` (which converts text strings to bytes objects)
* :meth:`str.format` and :meth:`str.format_map` (which are used to format
  text for display to users)
* :meth:`str.isidentifier`, :meth:`str.isnumeric`, :meth:`str.isdecimal`,
  :meth:`str.isprintable` (which are used to check various properties of
  text strings which are not typically applicable to binary protocols).

All other string methods are supported, although sometimes with slight
differences in functionality and semantics (as described below).

.. note::

   The methods on bytes and bytearray objects don't accept strings as their
   arguments, just as the methods on strings don't accept bytes as their
   arguments.  For example, you have to write::

      a = "abc"
      b = a.replace("a", "f")

   and::

      a = b"abc"
      b = a.replace(b"a", b"f")

Whenever a bytes or bytearray method needs to interpret the bytes as
characters (e.g. the :meth:`is...` methods, :meth:`split`, :meth:`strip`),
the ASCII character set is assumed (text strings use Unicode semantics).

.. note::
   Using these ASCII based methods to manipulate binary data that is not
   stored in an ASCII based format may lead to data corruption.

The search operations (:keyword:`in`, :meth:`count`, :meth:`find`,
:meth:`index`, :meth:`rfind` and :meth:`rindex`) all accept both integers
in the range 0 to 255 (inclusive) as well as bytes and byte array sequences.

.. versionchanged:: 3.3
   All of the search methods also accept an integer in the range 0 to 255
   (inclusive) as their first argument.


Each bytes and bytearray instance provides a :meth:`decode` convenience
method that is the inverse of :meth:`str.encode`:

.. method:: bytes.decode(encoding="utf-8", errors="strict")
            bytearray.decode(encoding="utf-8", errors="strict")

   Return a string decoded from the given bytes.  Default encoding is
   ``'utf-8'``. *errors* may be given to set a different
   error handling scheme.  The default for *errors* is ``'strict'``, meaning
   that encoding errors raise a :exc:`UnicodeError`.  Other possible values are
   ``'ignore'``, ``'replace'`` and any other name registered via
   :func:`codecs.register_error`, see section :ref:`codec-base-classes`. For a
   list of possible encodings, see section :ref:`standard-encodings`.

   .. versionchanged:: 3.1
      Added support for keyword arguments.

Since 2 hexadecimal digits correspond precisely to a single byte, hexadecimal
numbers are a commonly used format for describing binary data. Accordingly,
the bytes and bytearray types have an additional class method to read data in
that format:

.. classmethod:: bytes.fromhex(string)
                 bytearray.fromhex(string)

   This :class:`bytes` class method returns a bytes or bytearray object,
   decoding the given string object.  The string must contain two hexadecimal
   digits per byte, spaces are ignored.

   >>> bytes.fromhex('2Ef0 F1f2  ')
   b'.\xf0\xf1\xf2'


The maketrans and translate methods differ in semantics from the versions
available on strings:

.. method:: bytes.translate(table[, delete])
            bytearray.translate(table[, delete])

   Return a copy of the bytes or bytearray object where all bytes occurring in
   the optional argument *delete* are removed, and the remaining bytes have been
   mapped through the given translation table, which must be a bytes object of
   length 256.

   You can use the :func:`bytes.maketrans` method to create a translation table.

   Set the *table* argument to ``None`` for translations that only delete
   characters::

      >>> b'read this short text'.translate(None, b'aeiou')
      b'rd ths shrt txt'


.. staticmethod:: bytes.maketrans(from, to)
                  bytearray.maketrans(from, to)

   This static method returns a translation table usable for
   :meth:`bytes.translate` that will map each character in *from* into the
   character at the same position in *to*; *from* and *to* must be bytes objects
   and have the same length.

   .. versionadded:: 3.1


.. _typememoryview:

Memory Views
------------

:class:`memoryview` objects allow Python code to access the internal data
of an object that supports the :ref:`buffer protocol <bufferobjects>` without
copying.

.. class:: memoryview(obj)

   Create a :class:`memoryview` that references *obj*.  *obj* must support the
   buffer protocol.  Built-in objects that support the buffer protocol include
   :class:`bytes` and :class:`bytearray`.

   A :class:`memoryview` has the notion of an *element*, which is the
   atomic memory unit handled by the originating object *obj*.  For many
   simple types such as :class:`bytes` and :class:`bytearray`, an element
   is a single byte, but other types such as :class:`array.array` may have
   bigger elements.

   ``len(view)`` is equal to the length of :class:`~memoryview.tolist`.
   If ``view.ndim = 0``, the length is 1. If ``view.ndim = 1``, the length
   is equal to the number of elements in the view. For higher dimensions,
   the length is equal to the length of the nested list representation of
   the view. The :class:`~memoryview.itemsize` attribute will give you the
   number of bytes in a single element.

   A :class:`memoryview` supports slicing to expose its data. If
   :class:`~memoryview.format` is one of the native format specifiers
   from the :mod:`struct` module, indexing will return a single element
   with the correct type. Full slicing will result in a subview::

    >>> v = memoryview(b'abcefg')
    >>> v[1]
    98
    >>> v[-1]
    103
    >>> v[1:4]
    <memory at 0x7f3ddc9f4350>
    >>> bytes(v[1:4])
    b'bce'

   Other native formats::

      >>> import array
      >>> a = array.array('l', [-11111111, 22222222, -33333333, 44444444])
      >>> a[0]
      -11111111
      >>> a[-1]
      44444444
      >>> a[2:3].tolist()
      [-33333333]
      >>> a[::2].tolist()
      [-11111111, -33333333]
      >>> a[::-1].tolist()
      [44444444, -33333333, 22222222, -11111111]

   .. versionadded:: 3.3

   If the underlying object is writable, the memoryview supports slice
   assignment. Resizing is not allowed::

      >>> data = bytearray(b'abcefg')
      >>> v = memoryview(data)
      >>> v.readonly
      False
      >>> v[0] = ord(b'z')
      >>> data
      bytearray(b'zbcefg')
      >>> v[1:4] = b'123'
      >>> data
      bytearray(b'z123fg')
      >>> v[2:3] = b'spam'
      Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
      ValueError: memoryview assignment: lvalue and rvalue have different structures
      >>> v[2:6] = b'spam'
      >>> data
      bytearray(b'z1spam')

   One-dimensional memoryviews of hashable (read-only) types with formats
   'B', 'b' or 'c' are also hashable. The hash is defined as
   ``hash(m) == hash(m.tobytes())``::

      >>> v = memoryview(b'abcefg')
      >>> hash(v) == hash(b'abcefg')
      True
      >>> hash(v[2:4]) == hash(b'ce')
      True
      >>> hash(v[::-2]) == hash(b'abcefg'[::-2])
      True

   .. versionchanged:: 3.3
      One-dimensional memoryviews with formats 'B', 'b' or 'c' are now hashable.

   :class:`memoryview` has several methods:

   .. method:: __eq__(exporter)

      A memoryview and a :pep:`3118` exporter are equal if their shapes are
      equivalent and if all corresponding values are equal when the operands'
      respective format codes are interpreted using :mod:`struct` syntax.

      For the subset of :mod:`struct` format strings currently supported by
      :meth:`tolist`, ``v`` and ``w`` are equal if ``v.tolist() == w.tolist()``::

         >>> import array
         >>> a = array.array('I', [1, 2, 3, 4, 5])
         >>> b = array.array('d', [1.0, 2.0, 3.0, 4.0, 5.0])
         >>> c = array.array('b', [5, 3, 1])
         >>> x = memoryview(a)
         >>> y = memoryview(b)
         >>> x == a == y == b
         True
         >>> x.tolist() == a.tolist() == y.tolist() == b.tolist()
         True
         >>> z = y[::-2]
         >>> z == c
         True
         >>> z.tolist() == c.tolist()
         True

      If either format string is not supported by the :mod:`struct` module,
      then the objects will always compare as unequal (even if the format
      strings and buffer contents are identical)::

         >>> from ctypes import BigEndianStructure, c_long
         >>> class BEPoint(BigEndianStructure):
         ...     _fields_ = [("x", c_long), ("y", c_long)]
         ...
         >>> point = BEPoint(100, 200)
         >>> a = memoryview(point)
         >>> b = memoryview(point)
         >>> a == point
         False
         >>> a == b
         False

      Note that, as with floating point numbers, ``v is w`` does *not* imply
      ``v == w`` for memoryview objects.

      .. versionchanged:: 3.3
         Previous versions compared the raw memory disregarding the item format
         and the logical array structure.

   .. method:: tobytes()

      Return the data in the buffer as a bytestring.  This is equivalent to
      calling the :class:`bytes` constructor on the memoryview. ::

         >>> m = memoryview(b"abc")
         >>> m.tobytes()
         b'abc'
         >>> bytes(m)
         b'abc'

      For non-contiguous arrays the result is equal to the flattened list
      representation with all elements converted to bytes. :meth:`tobytes`
      supports all format strings, including those that are not in
      :mod:`struct` module syntax.

   .. method:: tolist()

      Return the data in the buffer as a list of elements. ::

         >>> memoryview(b'abc').tolist()
         [97, 98, 99]
         >>> import array
         >>> a = array.array('d', [1.1, 2.2, 3.3])
         >>> m = memoryview(a)
         >>> m.tolist()
         [1.1, 2.2, 3.3]

      .. versionchanged:: 3.3
         :meth:`tolist` now supports all single character native formats in
         :mod:`struct` module syntax as well as multi-dimensional
         representations.

   .. method:: release()

      Release the underlying buffer exposed by the memoryview object.  Many
      objects take special actions when a view is held on them (for example,
      a :class:`bytearray` would temporarily forbid resizing); therefore,
      calling release() is handy to remove these restrictions (and free any
      dangling resources) as soon as possible.

      After this method has been called, any further operation on the view
      raises a :class:`ValueError` (except :meth:`release()` itself which can
      be called multiple times)::

         >>> m = memoryview(b'abc')
         >>> m.release()
         >>> m[0]
         Traceback (most recent call last):
           File "<stdin>", line 1, in <module>
         ValueError: operation forbidden on released memoryview object

      The context management protocol can be used for a similar effect,
      using the ``with`` statement::

         >>> with memoryview(b'abc') as m:
         ...     m[0]
         ...
         97
         >>> m[0]
         Traceback (most recent call last):
           File "<stdin>", line 1, in <module>
         ValueError: operation forbidden on released memoryview object

      .. versionadded:: 3.2

   .. method:: cast(format[, shape])

      Cast a memoryview to a new format or shape. *shape* defaults to
      ``[byte_length//new_itemsize]``, which means that the result view
      will be one-dimensional. The return value is a new memoryview, but
      the buffer itself is not copied. Supported casts are 1D -> C-contiguous
      and C-contiguous -> 1D.

      Both formats are restricted to single element native formats in
      :mod:`struct` syntax. One of the formats must be a byte format
      ('B', 'b' or 'c'). The byte length of the result must be the same
      as the original length.

      Cast 1D/long to 1D/unsigned bytes::

         >>> import array
         >>> a = array.array('l', [1,2,3])
         >>> x = memoryview(a)
         >>> x.format
         'l'
         >>> x.itemsize
         8
         >>> len(x)
         3
         >>> x.nbytes
         24
         >>> y = x.cast('B')
         >>> y.format
         'B'
         >>> y.itemsize
         1
         >>> len(y)
         24
         >>> y.nbytes
         24

      Cast 1D/unsigned bytes to 1D/char::

         >>> b = bytearray(b'zyz')
         >>> x = memoryview(b)
         >>> x[0] = b'a'
         Traceback (most recent call last):
           File "<stdin>", line 1, in <module>
         ValueError: memoryview: invalid value for format "B"
         >>> y = x.cast('c')
         >>> y[0] = b'a'
         >>> b
         bytearray(b'ayz')

      Cast 1D/bytes to 3D/ints to 1D/signed char::

         >>> import struct
         >>> buf = struct.pack("i"*12, *list(range(12)))
         >>> x = memoryview(buf)
         >>> y = x.cast('i', shape=[2,2,3])
         >>> y.tolist()
         [[[0, 1, 2], [3, 4, 5]], [[6, 7, 8], [9, 10, 11]]]
         >>> y.format
         'i'
         >>> y.itemsize
         4
         >>> len(y)
         2
         >>> y.nbytes
         48
         >>> z = y.cast('b')
         >>> z.format
         'b'
         >>> z.itemsize
         1
         >>> len(z)
         48
         >>> z.nbytes
         48

      Cast 1D/unsigned char to to 2D/unsigned long::

         >>> buf = struct.pack("L"*6, *list(range(6)))
         >>> x = memoryview(buf)
         >>> y = x.cast('L', shape=[2,3])
         >>> len(y)
         2
         >>> y.nbytes
         48
         >>> y.tolist()
         [[0, 1, 2], [3, 4, 5]]

      .. versionadded:: 3.3

   There are also several readonly attributes available:

   .. attribute:: obj

      The underlying object of the memoryview::

         >>> b  = bytearray(b'xyz')
         >>> m = memoryview(b)
         >>> m.obj is b
         True

      .. versionadded:: 3.3

   .. attribute:: nbytes

      ``nbytes == product(shape) * itemsize == len(m.tobytes())``. This is
      the amount of space in bytes that the array would use in a contiguous
      representation. It is not necessarily equal to len(m)::

         >>> import array
         >>> a = array.array('i', [1,2,3,4,5])
         >>> m = memoryview(a)
         >>> len(m)
         5
         >>> m.nbytes
         20
         >>> y = m[::2]
         >>> len(y)
         3
         >>> y.nbytes
         12
         >>> len(y.tobytes())
         12

      Multi-dimensional arrays::

         >>> import struct
         >>> buf = struct.pack("d"*12, *[1.5*x for x in range(12)])
         >>> x = memoryview(buf)
         >>> y = x.cast('d', shape=[3,4])
         >>> y.tolist()
         [[0.0, 1.5, 3.0, 4.5], [6.0, 7.5, 9.0, 10.5], [12.0, 13.5, 15.0, 16.5]]
         >>> len(y)
         3
         >>> y.nbytes
         96

      .. versionadded:: 3.3

   .. attribute:: readonly

      A bool indicating whether the memory is read only.

   .. attribute:: format

      A string containing the format (in :mod:`struct` module style) for each
      element in the view. A memoryview can be created from exporters with
      arbitrary format strings, but some methods (e.g. :meth:`tolist`) are
      restricted to native single element formats.

      .. versionchanged:: 3.3
         format ``'B'`` is now handled according to the struct module syntax.
         This means that ``memoryview(b'abc')[0] == b'abc'[0] == 97``.

   .. attribute:: itemsize

      The size in bytes of each element of the memoryview::

         >>> import array, struct
         >>> m = memoryview(array.array('H', [32000, 32001, 32002]))
         >>> m.itemsize
         2
         >>> m[0]
         32000
         >>> struct.calcsize('H') == m.itemsize
         True

   .. attribute:: ndim

      An integer indicating how many dimensions of a multi-dimensional array the
      memory represents.

   .. attribute:: shape

      A tuple of integers the length of :attr:`ndim` giving the shape of the
      memory as an N-dimensional array.

      .. versionchanged:: 3.3
         An empty tuple instead of None when ndim = 0.

   .. attribute:: strides

      A tuple of integers the length of :attr:`ndim` giving the size in bytes to
      access each element for each dimension of the array.

      .. versionchanged:: 3.3
         An empty tuple instead of None when ndim = 0.

   .. attribute:: suboffsets

      Used internally for PIL-style arrays. The value is informational only.

   .. attribute:: c_contiguous

      A bool indicating whether the memory is C-contiguous.

      .. versionadded:: 3.3

   .. attribute:: f_contiguous

      A bool indicating whether the memory is Fortran contiguous.

      .. versionadded:: 3.3

   .. attribute:: contiguous

      A bool indicating whether the memory is contiguous.

      .. versionadded:: 3.3


.. _types-set:

Set Types --- :class:`set`, :class:`frozenset`
==============================================

.. index:: object: set

A :dfn:`set` object is an unordered collection of distinct :term:`可散列对象` objects.
Common uses include membership testing, removing duplicates from a sequence, and
computing mathematical operations such as intersection, union, difference, and
symmetric difference.
(For other containers see the built-in :class:`dict`, :class:`list`,
and :class:`tuple` classes, and the :mod:`collections` module.)

Like other collections, sets support ``x in set``, ``len(set)``, and ``for x in
set``.  Being an unordered collection, sets do not record element position or
order of insertion.  Accordingly, sets do not support indexing, slicing, or
other sequence-like behavior.

There are currently two built-in set types, :class:`set` and :class:`frozenset`.
The :class:`set` type is mutable --- the contents can be changed using methods
like :meth:`add` and :meth:`remove`.  Since it is mutable, it has no hash value
and cannot be used as either a dictionary key or as an element of another set.
The :class:`frozenset` type is immutable and :term:`可散列对象` --- its contents cannot be
altered after it is created; it can therefore be used as a dictionary key or as
an element of another set.

Non-empty sets (not frozensets) can be created by placing a comma-separated list
of elements within braces, for example: ``{'jack', 'sjoerd'}``, in addition to the
:class:`set` constructor.

The constructors for both classes work the same:

.. class:: set([iterable])
           frozenset([iterable])

   Return a new set or frozenset object whose elements are taken from
   *iterable*.  The elements of a set must be hashable.  To represent sets of
   sets, the inner sets must be :class:`frozenset` objects.  If *iterable* is
   not specified, a new empty set is returned.

   Instances of :class:`set` and :class:`frozenset` provide the following
   operations:

   .. describe:: len(s)

      Return the cardinality of set *s*.

   .. describe:: x in s

      Test *x* for membership in *s*.

   .. describe:: x not in s

      Test *x* for non-membership in *s*.

   .. method:: isdisjoint(other)

      Return True if the set has no elements in common with *other*.  Sets are
      disjoint if and only if their intersection is the empty set.

   .. method:: issubset(other)
               set <= other

      Test whether every element in the set is in *other*.

   .. method:: set < other

      Test whether the set is a proper subset of *other*, that is,
      ``set <= other and set != other``.

   .. method:: issuperset(other)
               set >= other

      Test whether every element in *other* is in the set.

   .. method:: set > other

      Test whether the set is a proper superset of *other*, that is, ``set >=
      other and set != other``.

   .. method:: union(other, ...)
               set | other | ...

      Return a new set with elements from the set and all others.

   .. method:: intersection(other, ...)
               set & other & ...

      Return a new set with elements common to the set and all others.

   .. method:: difference(other, ...)
               set - other - ...

      Return a new set with elements in the set that are not in the others.

   .. method:: symmetric_difference(other)
               set ^ other

      Return a new set with elements in either the set or *other* but not both.

   .. method:: copy()

      Return a new set with a shallow copy of *s*.


   Note, the non-operator versions of :meth:`union`, :meth:`intersection`,
   :meth:`difference`, and :meth:`symmetric_difference`, :meth:`issubset`, and
   :meth:`issuperset` methods will accept any iterable as an argument.  In
   contrast, their operator based counterparts require their arguments to be
   sets.  This precludes error-prone constructions like ``set('abc') & 'cbs'``
   in favor of the more readable ``set('abc').intersection('cbs')``.

   Both :class:`set` and :class:`frozenset` support set to set comparisons. Two
   sets are equal if and only if every element of each set is contained in the
   other (each is a subset of the other). A set is less than another set if and
   only if the first set is a proper subset of the second set (is a subset, but
   is not equal). A set is greater than another set if and only if the first set
   is a proper superset of the second set (is a superset, but is not equal).

   Instances of :class:`set` are compared to instances of :class:`frozenset`
   based on their members.  For example, ``set('abc') == frozenset('abc')``
   returns ``True`` and so does ``set('abc') in set([frozenset('abc')])``.

   The subset and equality comparisons do not generalize to a complete ordering
   function.  For example, any two disjoint sets are not equal and are not
   subsets of each other, so *all* of the following return ``False``: ``a<b``,
   ``a==b``, or ``a>b``.

   Since sets only define partial ordering (subset relationships), the output of
   the :meth:`list.sort` method is undefined for lists of sets.

   Set elements, like dictionary keys, must be :term:`可散列对象`.

   Binary operations that mix :class:`set` instances with :class:`frozenset`
   return the type of the first operand.  For example: ``frozenset('ab') |
   set('bc')`` returns an instance of :class:`frozenset`.

   The following table lists operations available for :class:`set` that do not
   apply to immutable instances of :class:`frozenset`:

   .. method:: update(other, ...)
               set |= other | ...

      Update the set, adding elements from all others.

   .. method:: intersection_update(other, ...)
               set &= other & ...

      Update the set, keeping only elements found in it and all others.

   .. method:: difference_update(other, ...)
               set -= other | ...

      Update the set, removing elements found in others.

   .. method:: symmetric_difference_update(other)
               set ^= other

      Update the set, keeping only elements found in either set, but not in both.

   .. method:: add(elem)

      Add element *elem* to the set.

   .. method:: remove(elem)

      Remove element *elem* from the set.  Raises :exc:`KeyError` if *elem* is
      not contained in the set.

   .. method:: discard(elem)

      Remove element *elem* from the set if it is present.

   .. method:: pop()

      Remove and return an arbitrary element from the set.  Raises
      :exc:`KeyError` if the set is empty.

   .. method:: clear()

      Remove all elements from the set.


   Note, the non-operator versions of the :meth:`update`,
   :meth:`intersection_update`, :meth:`difference_update`, and
   :meth:`symmetric_difference_update` methods will accept any iterable as an
   argument.

   Note, the *elem* argument to the :meth:`__contains__`, :meth:`remove`, and
   :meth:`discard` methods may be a set.  To support searching for an equivalent
   frozenset, the *elem* set is temporarily mutated during the search and then
   restored.  During the search, the *elem* set should not be read or mutated
   since it does not have a meaningful value.


.. _typesmapping:

Mapping Types --- :class:`dict`
===============================

.. index::
   object: mapping
   object: dictionary
   triple: operations on; mapping; types
   triple: operations on; dictionary; type
   statement: del
   builtin: len

A :term:`映射` object maps :term:`可散列对象` values to arbitrary objects.
Mappings are mutable objects.  There is currently only one standard mapping
type, the :dfn:`dictionary`.  (For other containers see the built-in
:class:`list`, :class:`set`, and :class:`tuple` classes, and the
:mod:`collections` module.)

A dictionary's keys are *almost* arbitrary values.  Values that are not
:term:`可散列对象`, that is, values containing lists, dictionaries or other
mutable types (that are compared by value rather than by object identity) may
not be used as keys.  Numeric types used for keys obey the normal rules for
numeric comparison: if two numbers compare equal (such as ``1`` and ``1.0``)
then they can be used interchangeably to index the same dictionary entry.  (Note
however, that since computers store floating-point numbers as approximations it
is usually unwise to use them as dictionary keys.)

Dictionaries can be created by placing a comma-separated list of ``key: value``
pairs within braces, for example: ``{'jack': 4098, 'sjoerd': 4127}`` or ``{4098:
'jack', 4127: 'sjoerd'}``, or by the :class:`dict` constructor.

.. class:: dict(**kwarg)
           dict(mapping, **kwarg)
           dict(iterable, **kwarg)

   Return a new dictionary initialized from an optional positional argument
   and a possibly empty set of keyword arguments.

   If no positional argument is given, an empty dictionary is created.
   If a positional argument is given and it is a mapping object, a dictionary
   is created with the same key-value pairs as the mapping object.  Otherwise,
   the positional argument must be an :term:`迭代器` object.  Each item in
   the iterable must itself be an iterator with exactly two objects.  The
   first object of each item becomes a key in the new dictionary, and the
   second object the corresponding value.  If a key occurs more than once, the
   last value for that key becomes the corresponding value in the new
   dictionary.

   If keyword arguments are given, the keyword arguments and their values are
   added to the dictionary created from the positional argument.  If a key
   being added is already present, the value from the keyword argument
   replaces the value from the positional argument.

   To illustrate, the following examples all return a dictionary equal to
   ``{"one": 1, "two": 2, "three": 3}``::

      >>> a = dict(one=1, two=2, three=3)
      >>> b = {'one': 1, 'two': 2, 'three': 3}
      >>> c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
      >>> d = dict([('two', 2), ('one', 1), ('three', 3)])
      >>> e = dict({'three': 3, 'one': 1, 'two': 2})
      >>> a == b == c == d == e
      True

   Providing keyword arguments as in the first example only works for keys that
   are valid Python identifiers.  Otherwise, any valid keys can be used.


   These are the operations that dictionaries support (and therefore, custom
   mapping types should support too):

   .. describe:: len(d)

      Return the number of items in the dictionary *d*.

   .. describe:: d[key]

      Return the item of *d* with key *key*.  Raises a :exc:`KeyError` if *key* is
      not in the map.

      If a subclass of dict defines a method :meth:`__missing__`, if the key *key*
      is not present, the ``d[key]`` operation calls that method with the key *key*
      as argument.  The ``d[key]`` operation then returns or raises whatever is
      returned or raised by the ``__missing__(key)`` call if the key is not
      present. No other operations or methods invoke :meth:`__missing__`. If
      :meth:`__missing__` is not defined, :exc:`KeyError` is raised.
      :meth:`__missing__` must be a method; it cannot be an instance variable::

          >>> class Counter(dict):
          ...     def __missing__(self, key):
          ...         return 0
          >>> c = Counter()
          >>> c['red']
          0
          >>> c['red'] += 1
          >>> c['red']
          1

      See :class:`collections.Counter` for a complete implementation including
      other methods helpful for accumulating and managing tallies.

   .. describe:: d[key] = value

      Set ``d[key]`` to *value*.

   .. describe:: del d[key]

      Remove ``d[key]`` from *d*.  Raises a :exc:`KeyError` if *key* is not in the
      map.

   .. describe:: key in d

      Return ``True`` if *d* has a key *key*, else ``False``.

   .. describe:: key not in d

      Equivalent to ``not key in d``.

   .. describe:: iter(d)

      Return an iterator over the keys of the dictionary.  This is a shortcut
      for ``iter(d.keys())``.

   .. method:: clear()

      Remove all items from the dictionary.

   .. method:: copy()

      Return a shallow copy of the dictionary.

   .. classmethod:: fromkeys(seq[, value])

      Create a new dictionary with keys from *seq* and values set to *value*.

      :meth:`fromkeys` is a class method that returns a new dictionary. *value*
      defaults to ``None``.

   .. method:: get(key[, default])

      Return the value for *key* if *key* is in the dictionary, else *default*.
      If *default* is not given, it defaults to ``None``, so that this method
      never raises a :exc:`KeyError`.

   .. method:: items()

      Return a new view of the dictionary's items (``(key, value)`` pairs).
      See the :ref:`documentation of view objects <dict-views>`.

   .. method:: keys()

      Return a new view of the dictionary's keys.  See the :ref:`documentation
      of view objects <dict-views>`.

   .. method:: pop(key[, default])

      If *key* is in the dictionary, remove it and return its value, else return
      *default*.  If *default* is not given and *key* is not in the dictionary,
      a :exc:`KeyError` is raised.

   .. method:: popitem()

      Remove and return an arbitrary ``(key, value)`` pair from the dictionary.

      :meth:`popitem` is useful to destructively iterate over a dictionary, as
      often used in set algorithms.  If the dictionary is empty, calling
      :meth:`popitem` raises a :exc:`KeyError`.

   .. method:: setdefault(key[, default])

      If *key* is in the dictionary, return its value.  If not, insert *key*
      with a value of *default* and return *default*.  *default* defaults to
      ``None``.

   .. method:: update([other])

      Update the dictionary with the key/value pairs from *other*, overwriting
      existing keys.  Return ``None``.

      :meth:`update` accepts either another dictionary object or an iterable of
      key/value pairs (as tuples or other iterables of length two).  If keyword
      arguments are specified, the dictionary is then updated with those
      key/value pairs: ``d.update(red=1, blue=2)``.

   .. method:: values()

      Return a new view of the dictionary's values.  See the
      :ref:`documentation of view objects <dict-views>`.

.. seealso::
   :class:`types.MappingProxyType` can be used to create a read-only view
   of a :class:`dict`.


.. _dict-views:

Dictionary view objects
-----------------------

The objects returned by :meth:`dict.keys`, :meth:`dict.values` and
:meth:`dict.items` are *view objects*.  They provide a dynamic view on the
dictionary's entries, which means that when the dictionary changes, the view
reflects these changes.

Dictionary views can be iterated over to yield their respective data, and
support membership tests:

.. describe:: len(dictview)

   Return the number of entries in the dictionary.

.. describe:: iter(dictview)

   Return an iterator over the keys, values or items (represented as tuples of
   ``(key, value)``) in the dictionary.

   Keys and values are iterated over in an arbitrary order which is non-random,
   varies across Python implementations, and depends on the dictionary's history
   of insertions and deletions. If keys, values and items views are iterated
   over with no intervening modifications to the dictionary, the order of items
   will directly correspond.  This allows the creation of ``(value, key)`` pairs
   using :func:`zip`: ``pairs = zip(d.values(), d.keys())``.  Another way to
   create the same list is ``pairs = [(v, k) for (k, v) in d.items()]``.

   Iterating views while adding or deleting entries in the dictionary may raise
   a :exc:`RuntimeError` or fail to iterate over all entries.

.. describe:: x in dictview

   Return ``True`` if *x* is in the underlying dictionary's keys, values or
   items (in the latter case, *x* should be a ``(key, value)`` tuple).


Keys views are set-like since their entries are unique and hashable.  If all
values are hashable, so that ``(key, value)`` pairs are unique and hashable,
then the items view is also set-like.  (Values views are not treated as set-like
since the entries are generally not unique.)  For set-like views, all of the
operations defined for the abstract base class :class:`collections.abc.Set` are
available (for example, ``==``, ``<``, or ``^``).

An example of dictionary view usage::

   >>> dishes = {'eggs': 2, 'sausage': 1, 'bacon': 1, 'spam': 500}
   >>> keys = dishes.keys()
   >>> values = dishes.values()

   >>> # iteration
   >>> n = 0
   >>> for val in values:
   ...     n += val
   >>> print(n)
   504

   >>> # keys and values are iterated over in the same order
   >>> list(keys)
   ['eggs', 'bacon', 'sausage', 'spam']
   >>> list(values)
   [2, 1, 1, 500]

   >>> # view objects are dynamic and reflect dict changes
   >>> del dishes['eggs']
   >>> del dishes['sausage']
   >>> list(keys)
   ['spam', 'bacon']

   >>> # set operations
   >>> keys & {'eggs', 'bacon', 'salad'}
   {'bacon'}
   >>> keys ^ {'sausage', 'juice'}
   {'juice', 'sausage', 'bacon', 'spam'}


.. _typecontextmanager:

Context Manager Types
=====================

.. index::
   single: context manager
   single: context management protocol
   single: protocol; context management

Python's :keyword:`with` statement supports the concept of a runtime context
defined by a context manager.  This is implemented using a pair of methods
that allow user-defined classes to define a runtime context that is entered
before the statement body is executed and exited when the statement ends:


.. method:: contextmanager.__enter__()

   Enter the runtime context and return either this object or another object
   related to the runtime context. The value returned by this method is bound to
   the identifier in the :keyword:`as` clause of :keyword:`with` statements using
   this context manager.

   An example of a context manager that returns itself is a :term:`文件对象`.
   File objects return themselves from __enter__() to allow :func:`open` to be
   used as the context expression in a :keyword:`with` statement.

   An example of a context manager that returns a related object is the one
   returned by :func:`decimal.localcontext`. These managers set the active
   decimal context to a copy of the original decimal context and then return the
   copy. This allows changes to be made to the current decimal context in the body
   of the :keyword:`with` statement without affecting code outside the
   :keyword:`with` statement.


.. method:: contextmanager.__exit__(exc_type, exc_val, exc_tb)

   Exit the runtime context and return a Boolean flag indicating if any exception
   that occurred should be suppressed. If an exception occurred while executing the
   body of the :keyword:`with` statement, the arguments contain the exception type,
   value and traceback information. Otherwise, all three arguments are ``None``.

   Returning a true value from this method will cause the :keyword:`with` statement
   to suppress the exception and continue execution with the statement immediately
   following the :keyword:`with` statement. Otherwise the exception continues
   propagating after this method has finished executing. Exceptions that occur
   during execution of this method will replace any exception that occurred in the
   body of the :keyword:`with` statement.

   The exception passed in should never be reraised explicitly - instead, this
   method should return a false value to indicate that the method completed
   successfully and does not want to suppress the raised exception. This allows
   context management code (such as ``contextlib.nested``) to easily detect whether
   or not an :meth:`__exit__` method has actually failed.

Python defines several context managers to support easy thread synchronisation,
prompt closure of files or other objects, and simpler manipulation of the active
decimal arithmetic context. The specific types are not treated specially beyond
their implementation of the context management protocol. See the
:mod:`contextlib` module for some examples.

Python's :term:`生成函数`\s and the :class:`contextlib.contextmanager` decorator
provide a convenient way to implement these protocols.  If a generator function is
decorated with the :class:`contextlib.contextmanager` decorator, it will return a
context manager implementing the necessary :meth:`__enter__` and
:meth:`__exit__` methods, rather than the iterator produced by an undecorated
generator function.

Note that there is no specific slot for any of these methods in the type
structure for Python objects in the Python/C API. Extension types wanting to
define these methods must provide them as a normal Python accessible method.
Compared to the overhead of setting up the runtime context, the overhead of a
single class dictionary lookup is negligible.


.. _typesother:

Other Built-in Types
====================

The interpreter supports several other kinds of objects. Most of these support
only one or two operations.


.. _typesmodules:

Modules
-------

The only special operation on a module is attribute access: ``m.name``, where
*m* is a module and *name* accesses a name defined in *m*'s symbol table.
Module attributes can be assigned to.  (Note that the :keyword:`import`
statement is not, strictly speaking, an operation on a module object; ``import
foo`` does not require a module object named *foo* to exist, rather it requires
an (external) *definition* for a module named *foo* somewhere.)

A special attribute of every module is :attr:`__dict__`. This is the dictionary
containing the module's symbol table. Modifying this dictionary will actually
change the module's symbol table, but direct assignment to the :attr:`__dict__`
attribute is not possible (you can write ``m.__dict__['a'] = 1``, which defines
``m.a`` to be ``1``, but you can't write ``m.__dict__ = {}``).  Modifying
:attr:`__dict__` directly is not recommended.

Modules built into the interpreter are written like this: ``<module 'sys'
(built-in)>``.  If loaded from a file, they are written as ``<module 'os' from
'/usr/local/lib/pythonX.Y/os.pyc'>``.


.. _typesobjects:

Classes and Class Instances
---------------------------

See :ref:`objects` and :ref:`class` for these.


.. _typesfunctions:

Functions
---------

Function objects are created by function definitions.  The only operation on a
function object is to call it: ``func(argument-list)``.

There are really two flavors of function objects: built-in functions and
user-defined functions.  Both support the same operation (to call the function),
but the implementation is different, hence the different object types.

See :ref:`function` for more information.


.. _typesmethods:

Methods
-------

.. index:: object: method

Methods are functions that are called using the attribute notation. There are
two flavors: built-in methods (such as :meth:`append` on lists) and class
instance methods.  Built-in methods are described with the types that support
them.

If you access a method (a function defined in a class namespace) through an
instance, you get a special object: a :dfn:`bound method` (also called
:dfn:`instance method`) object. When called, it will add the ``self`` argument
to the argument list.  Bound methods have two special read-only attributes:
``m.__self__`` is the object on which the method operates, and ``m.__func__`` is
the function implementing the method.  Calling ``m(arg-1, arg-2, ..., arg-n)``
is completely equivalent to calling ``m.__func__(m.__self__, arg-1, arg-2, ...,
arg-n)``.

Like function objects, bound method objects support getting arbitrary
attributes.  However, since method attributes are actually stored on the
underlying function object (``meth.__func__``), setting method attributes on
bound methods is disallowed.  Attempting to set an attribute on a method
results in an :exc:`AttributeError` being raised.  In order to set a method
attribute, you need to explicitly set it on the underlying function object::

   >>> class C:
   ...     def method(self):
   ...         pass
   ...
   >>> c = C()
   >>> c.method.whoami = 'my name is method'  # can't set on the method
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   AttributeError: 'method' object has no attribute 'whoami'
   >>> c.method.__func__.whoami = 'my name is method'
   >>> c.method.whoami
   'my name is method'

See :ref:`types` for more information.


.. _bltin-code-objects:

Code Objects
------------

.. index:: object: code

.. index::
   builtin: compile
   single: __code__ (function object attribute)

Code objects are used by the implementation to represent "pseudo-compiled"
executable Python code such as a function body. They differ from function
objects because they don't contain a reference to their global execution
environment.  Code objects are returned by the built-in :func:`compile` function
and can be extracted from function objects through their :attr:`__code__`
attribute. See also the :mod:`code` module.

.. index::
   builtin: exec
   builtin: eval

A code object can be executed or evaluated by passing it (instead of a source
string) to the :func:`exec` or :func:`eval`  built-in functions.

See :ref:`types` for more information.


.. _bltin-type-objects:

Type Objects
------------

.. index::
   builtin: type
   module: types

Type objects represent the various object types.  An object's type is accessed
by the built-in function :func:`type`.  There are no special operations on
types.  The standard module :mod:`types` defines names for all standard built-in
types.

Types are written like this: ``<class 'int'>``.


.. _bltin-null-object:

The Null Object
---------------

This object is returned by functions that don't explicitly return a value.  It
supports no special operations.  There is exactly one null object, named
``None`` (a built-in name).  ``type(None)()`` produces the same singleton.

It is written as ``None``.


.. _bltin-ellipsis-object:

The Ellipsis Object
-------------------

This object is commonly used by slicing (see :ref:`slicings`).  It supports no
special operations.  There is exactly one ellipsis object, named
:const:`Ellipsis` (a built-in name).  ``type(Ellipsis)()`` produces the
:const:`Ellipsis` singleton.

It is written as ``Ellipsis`` or ``...``.


.. _bltin-notimplemented-object:

The NotImplemented Object
-------------------------

This object is returned from comparisons and binary operations when they are
asked to operate on types they don't support. See :ref:`comparisons` for more
information.  There is exactly one ``NotImplemented`` object.
``type(NotImplemented)()`` produces the singleton instance.

It is written as ``NotImplemented``.


.. _bltin-boolean-values:

Boolean Values
--------------

Boolean values are the two constant objects ``False`` and ``True``.  They are
used to represent truth values (although other values can also be considered
false or true).  In numeric contexts (for example when used as the argument to
an arithmetic operator), they behave like the integers 0 and 1, respectively.
The built-in function :func:`bool` can be used to convert any value to a
Boolean, if the value can be interpreted as a truth value (see section
:ref:`truth` above).

.. index::
   single: False
   single: True
   pair: Boolean; values

They are written as ``False`` and ``True``, respectively.


.. _typesinternal:

Internal Objects
----------------

See :ref:`types` for this information.  It describes stack frame objects,
traceback objects, and slice objects.


.. _specialattrs:

Special Attributes
==================

The implementation adds a few special read-only attributes to several object
types, where they are relevant.  Some of these are not reported by the
:func:`dir` built-in function.


.. attribute:: object.__dict__

   A dictionary or other mapping object used to store an object's (writable)
   attributes.


.. attribute:: instance.__class__

   The class to which a class instance belongs.


.. attribute:: class.__bases__

   The tuple of base classes of a class object.


.. attribute:: class.__name__

   The name of the class or type.


.. attribute:: class.__qualname__

   The :term:`限定名字` of the class or type.

   .. versionadded:: 3.3


.. attribute:: class.__mro__

   This attribute is a tuple of classes that are considered when looking for
   base classes during method resolution.


.. method:: class.mro()

   This method can be overridden by a metaclass to customize the method
   resolution order for its instances.  It is called at class instantiation, and
   its result is stored in :attr:`__mro__`.


.. method:: class.__subclasses__

   Each class keeps a list of weak references to its immediate subclasses.  This
   method returns a list of all those references still alive.
   Example::

      >>> int.__subclasses__()
      [<class 'bool'>]


.. rubric:: Footnotes

.. [1] Additional information on these special methods may be found in the Python
   Reference Manual (:ref:`customization`).

.. [2] As a consequence, the list ``[1, 2]`` is considered equal to ``[1.0, 2.0]``, and
   similarly for tuples.

.. [3] They must have since the parser can't tell the type of the operands.

.. [4] Cased characters are those with general category property being one of
   "Lu" (Letter, uppercase), "Ll" (Letter, lowercase), or "Lt" (Letter, titlecase).

.. [5] To format only a tuple you should therefore provide a singleton tuple whose only
   element is the tuple to be formatted.
