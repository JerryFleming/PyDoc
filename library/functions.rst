.. XXX document all delegations to __special__ methods
.. _built-in-funcs:

内置函数
==================

Python 解释器中内置了一些函数和类型，可以随时使用。下面按字母顺序列出。

===================  =================  ==================  ================  ====================
 ..                   ..                 内置函数            ..                ..
===================  =================  ==================  ================  ====================
:func:`abs`          |func-dict|_       :func:`help`        :func:`min`       :func:`setattr`
:func:`all`          :func:`dir`        :func:`hex`         :func:`next`      :func:`slice`
:func:`any`          :func:`divmod`     :func:`id`          :func:`object`    :func:`sorted`
:func:`ascii`        :func:`enumerate`  :func:`input`       :func:`oct`       :func:`staticmethod`
:func:`bin`          :func:`eval`       :func:`int`         :func:`open`      |func-str|_
:func:`bool`         :func:`exec`       :func:`isinstance`  :func:`ord`       :func:`sum`
:func:`bytearray`    :func:`filter`     :func:`issubclass`  :func:`pow`       :func:`super`
:func:`bytes`        :func:`float`      :func:`iter`        :func:`print`     |func-tuple|_
:func:`callable`     :func:`format`     :func:`len`         :func:`property`  :func:`type`
:func:`chr`          |func-frozenset|_  |func-list|_        |func-range|_     :func:`vars`
:func:`classmethod`  :func:`getattr`    :func:`locals`      :func:`repr`      :func:`zip`
:func:`compile`      :func:`globals`    :func:`map`         :func:`reversed`  :func:`__import__`
:func:`complex`      :func:`hasattr`    :func:`max`         :func:`round`
:func:`delattr`      :func:`hash`       |func-memoryview|_  |func-set|_
===================  =================  ==================  ================  ====================

.. using :func:`dict` would create a link to another page, so local targets are
   used, with replacement texts to make the output in the table consistent

.. |func-dict| replace:: ``dict()``
.. |func-frozenset| replace:: ``frozenset()``
.. |func-memoryview| replace:: ``memoryview()``
.. |func-set| replace:: ``set()``
.. |func-list| replace:: ``list()``
.. |func-str| replace:: ``str()``
.. |func-tuple| replace:: ``tuple()``
.. |func-range| replace:: ``range()``


.. function:: abs(x)

   返回一个数的绝对值，参数可以是整数或者浮点数。如果参数是复数，则返回其模。


.. function:: all(iterable)

   如果 *iterable* 中的所有元素都为真(或者 iterable 为空)则返回 True 。相当于::

      def all(iterable):
          for element in iterable:
              if not element:
                  return False
          return True


.. function:: any(iterable)

   如果 *iterable* 中的任意元素为真则返回 True 。如果 iterable 为空则返回 False 。相当于::

      def any(iterable):
          for element in iterable:
              if element:
                  return True
          return False


.. function:: ascii(object)

   和 :func:`repr` 一样，返回一个字符串，表示一个对象的可打印形式，但是把 :func:`repr` 返回的字符串中非 ASCII 字符用 ``\x`` 、\ ``\u`` 、\ ``\U`` 转义。生成的字符串和 Python 2 中的 :func:`repr` 返回值很相似。


.. function:: bin(x)

   把整数转化为二进制字符串，其结果是个有效的 Python 表达式。如果 *x* 不是 Python 的 :class:`int` 对象，则需要定义 :meth:`__index__` 方法并返回一个整数。


.. function:: bool([x])

   使用标准的\ :ref:`真值检测过程 <truth>`\ 把一个值转化为布尔值。如果 *x* 为假或者省略，则返回 ``False`` ，否则返回 ``True`` 。\ :class:`bool` 还是一个类，它是 :class:`int` 的子类(参见 :ref:`typesnumeric`)。类 :class:`bool` 不能继续派生，它只有两个实例，即 ``False`` 和 ``True`` (参见 :ref:`bltin-boolean-values`)。

   .. index:: pair: Boolean; type


.. _func-bytearray:
.. function:: bytearray([source[, encoding[, errors]]])

   返回一个字节数组。\ :class:`bytearray` 类型是个可变的整数序列，其中的数都在 0 <= x < 256 范围内。它有大部分可变序列的常规方法，如\ :ref:`typesseq-mutable`\ 所述，以及 :class:`bytes` 类型的大部分方法，参见\ :ref:`bytes-methods` 。

   可以用可选参数 *source* 来及以下方法来初始化数组：

   * 如果它是\ *字符串*\ ，则必须同时指定 *encoding* (以及可选的 *errors*)参数。这时 :func:`bytearray` 会把这个字符串用 :meth:`str.encode` 转化为字节。

   * 如果它是\ *整数*\ ，则表示数组的长度，数组用空字节初始化。

   * 如果它是个和 *buffer* 界面兼容的对象，则使用这个对象的只读缓存来初始化字节数组。

   * 如果它是个\ *可迭代对象*\ ，则这个对象必须是 ``0 <= x < 256`` 范围内的整数可迭代对象，其元素用来初始化数组内容。

   如果没有参数，则创建长度为 0 的数组。

   参见\ :ref:`binaryseq`\ 和\ :ref:`typebytearray` 。


.. _func-bytes:
.. function:: bytes([source[, encoding[, errors]]])

   返回一个新的"bytes"对象，它是一个 ``0 <= x < 256`` 范围内整数的不可变序列。\ :class:`bytes` 是 :class:`bytearray` 的不可变版本 --- 它用同样的不可变方法，以及索引和切片行为。

   相应的，其构造函数的参数也如 :func:`bytearray` 中描述的那样。

   bytes 对象还可以通过源常量创建，参见\ :ref:`strings`\ 。

   另见\ :ref:`binaryseq`\ 、\ :ref:`typebytes`\ 、和 :ref:`bytes-methods`\ 。


.. function:: callable(object)

   如果参数 *object* 看起来可调用，则返回 :const:`True` ，否则返回 :const:`False` 。如果返回真，调用时仍然可能失败；但如果返回假，则调用 *object* 肯定不会成功。注意，类是可调用的(调用类会返回一个新的实例)；而如果类有 :meth:`__call__` 方法，则其实例也是可调用的。

   .. versionadded:: 3.2
      这个函数先从 Python 3.0 删除，然后在 Python 3.2 中又恢复了。


.. function:: chr(i)

   返回一个字符串，用来表示整数 *i* 所代表的 Unicode 字符。例如，\ ``chr(97)`` 返回字符 ``'a'`` 。这和 :func:`ord` 正好相反。参数的有效范围是从 0 到 1,114,111 (16 进制为 0x10FFFF)。如果 *i* 不在此范围则抛出 :exc:`ValueError` 。


.. function:: classmethod(function)

   为 *function* 返回一个类方法。

   类方法隐含的把其类作为第一个参数，就像实例方法接受实例一样。要声明类方法，使用下面的约定::

      class C:
          @classmethod
          def f(cls, arg1, arg2, ...): ...

   这里的 ``@classmethod`` 形式是个函数\ :term:`描述符` --- 详情参见\ :ref:`function`\ 中对函数定义的描述。

   它既可以用类(例如 ``C.f()``)也可以用实例(例如 ``C().f()``)来调用。对于实例，仅使用其类而忽略其它。如果在派生类中调用类方法，则把派生类对象作为隐含是第一个参数。

   类方法和 C++ 或 Java 中的静态方法是不同的。如是你需要静态方法，参见本节的 :func:`staticmethod` 。

   关于类方法的更多信息，请查阅\ :ref:`types`\ 中标准类型体系的文档。


.. function:: compile(source, filename, mode, flags=0, dont_inherit=False, optimize=-1)

   把 *source* 编译成 AST 代码对象。代码对象可以用 :func:`exec` 或者 :func:`eval` 来执行。\ *source* 可以是字符串或者 AST 对象。关于如何使用 AST 对象参见 :mod:`ast` 模块的文档。

   *filename* 参数应该指定从中读取代码的文件，如果不是从文件读取则可以转入一个易于识别的标识(通常用 ``'<string>'``)。

   *mode* 参数指定编译什么类型的代码。如果 *source* 含有一系列语句，则这个参数可以是 ``'exec'`` ；如果只包含一个表达式，则它可以是 ``'eval'`` ；而如果只包含一个交互式语句，则是 ``'single'`` (在最后这种情况下，如果表达式语句的值不是 ``None`` 则会被打印出来)。

   可选的参数 *flags* 和 *dont_inherit* 控制哪些 future 语句(参见 :pep:`236`)可以影响 *source* 的编译。如果一个也没有给出(或者都是零)，则调用 compile 的代码中起作用的 future 语句将会被使用。如果指定了 *flags* 参数而没有指定 *dont_inherit* (或其值为零)，则除了正常要使用的 future 语句，还会使用 *flags* 参数中指定的那些。如果 *dont_inherit* 是个非零整数，则仅使用 *flags* 参数指定的 future 语句 --- 调用 compile 时起作用的那些将会被忽略。

   future 语句通过二进制位来指定，可以通过\ *按位与*\ 运算来指定多个。指定特定功能的二进制值可见于 :mod:`__future__` 模块中 :class:`_Feature` 类实例的 :attr:`compiler_flag` 属性。

   参数 *optimize* 指定编译器的优化级别，默认值 ``-1`` 选择的级别和解释器 :option:`-O` 选项给出的一样。可以明确指定的级别有 ``0`` (不优化，\ ``__debug__`` 为真)，\ ``1`` (去除断言语句，\ ``__debug__`` 为假)，或者 ``2`` (还要去掉文档字符串)。

   如果编译后的代码无效则抛出 :exc:`SyntaxError` ，如果代码中含有空字节则抛出 :exc:`TypeError` 。

   .. note::

      如果使用 ``'single'`` 或者 ``'eval'`` 模式编译含有多行代码的字符串，则输入代码必须由至少一个换行符结束。这是为了帮助检测 :mod:`code` 模块中的完整和不完整的语句。

   .. versionchanged:: 3.2
      允许使用 Windows 和 Mac 换行符。并且 ``'exec'`` 模式下的输入不一定要换行符结束。还加入了 *optimize* 参数。


.. function:: complex([real[, imag]])

   创建一个值为 *real* + *imag*\*j 的复数，或者把一个字符串或数值转化为复数。如果第一个参数是字符串，则把它解析成复数；这时不得有第二个参数。第二个参数不可能是字符串。每个参数都可以是任意的数值类型(包括复数)。如果 *imag* 省略，则其默认为零，这时函数就相当于一个数值转换函数，像 :func:`int` 和 :func:`float` 一样。如果两个参数都省略，则返回 ``0j`` 。

   .. note::

      转换字符串时，字符串中间的 ``+`` 或 ``-`` 运算符旁边不得包含空格。例如，\ ``complex('1+2j')`` 可以接受，而 ``complex('1 + 2j')`` 会抛出 :exc:`ValueError` 。

   复数类型在\ :ref:`typesnumeric`\ 中介绍。


.. function:: delattr(object, name)

   它和 :func:`setattr` 相关联，其参数是一个对象 object 和字符串 name。name 必须是 object 一个属性的名字。如果允许，函数会删除对象的指定的属性。例如，\ ``delattr(x, 'foobar')`` 相当于 ``del x.foobar`` 。


.. _func-dict:
.. function:: dict(**kwarg)
              dict(mapping, **kwarg)
              dict(iterable, **kwarg)
   :noindex:

   创建一个新的字典。\ :class:`dict` 对象是字典类，其文档参见 :class:`dict` 和\ :ref:`typesmapping`\ 。

   关于其它容器，参见内置的 :class:`list` 、\ :class:`set` 、和 :class:`tuple` 类，以及 :mod:`collections` 模块。


.. function:: dir([object])

   如果没有参数，则返回当前本地作用域的名字列表。如果有参数，将试图返回该对象有效属性的列表。

   如果 object 有个叫 :meth:`__dir__` 的方法，则调用这个方法；方法必须返回属性列表。这样对象就可以通过实现 :func:`__getattr__` 或者 :func:`__getattribute__` 函数来自定义 :func:`dir` 如何列出其属性。

   如果 object 没有提供 :meth:`__dir__` ，则该函数会尽量从 object 的 :attr:`__dict__` 属性(如果存在)和其 type 对象中搜集信息。结果列表不一定要详尽，如果 object 有自定义的 :func:`__getattr__` 则结果可能不准。

   默认的 :func:`dir` 机制对不同类型的对象其行为也不一样，因为它会尽量提供最相关的而不是最准确的信息：

   * 如果 object 是个模块级对象，则结果包含该模块属性的名称。

   * 如果 object 是一种类型或类对象，则结果包含其属性名称，并递归的包含基类的属性名称。

   * 否则，结果包含 object 的属性名称、其类的属性名称、以及递归包含其类之基类的属性名称。

   结果列表会按字母排序。例如：

      >>> import struct
      >>> dir()   # 显示模块命名空间中的名称
      ['__builtins__', '__name__', 'struct']
      >>> dir(struct)   # 显示 struct 模块中的名称 # doctest: +SKIP
      ['Struct', '__all__', '__builtins__', '__cached__', '__doc__', '__file__',
       '__initializing__', '__loader__', '__name__', '__package__',
       '_clearcache', 'calcsize', 'error', 'pack', 'pack_into',
       'unpack', 'unpack_from']
      >>> class Shape(object):
      ...     def __dir__(self):
      ...         return ['area', 'perimeter', 'location']
      >>> s = Shape()
      >>> dir(s)
      ['area', 'location', 'perimeter']

   .. note::

      因为 :func:`dir` 主要作用是便于在交互式提示符下使用，它会尽量提供有用的名称列表，而不是生硬的完备的名称列表，并且其具体行为可能在不同版本中有改变。例如，当参数是一个类时，元类的属性并不包含在结果中。


.. function:: divmod(a, b)

   接受两个(不是复数的)数值参数，返回一对数值，其中分别是其整除时的商和余数。对于混合的操作数类型，还会应用二进制算术运算规则。对于整数，结果和 ``(a // b, a % b)`` 是一样的。对于浮点数，结果是 ``(q, a % b)`` ，其中的 *q* 通常是 ``math.floor(a /
   b)`` ，但也有可能比之小 1 。在任何情况下，\ ``q * b + a % b`` 都和 *a* 相当接近，如果 ``a % b`` 不为零就会和 *b* 正负符号相同，还有 ``0 <= abs(a % b) < abs(b)`` 。


.. function:: enumerate(iterable, start=0)

   返回一个枚举对象。\ *iterable* 必须是个序列、\ :term:`迭代器`\ 、或者支迭代的其它对象。\ :func:`enumerate` 返回一个迭代器，这个迭代器的 :meth:`~iterator.__next__` 方法返回一个元组，其中包含一个计数(从 *start* 开始，默认为 0)和通过迭代 *iterable* 所得到的值。

      >>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
      >>> list(enumerate(seasons))
      [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
      >>> list(enumerate(seasons, start=1))
      [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]

   相当于::

      def enumerate(sequence, start=0):
          n = start
          for elem in sequence:
              yield n, elem
              n += 1


.. function:: eval(expression, globals=None, locals=None)

   参数是个字符串以及可选的全局和局部变量。如果指定 *globals* ，则它必须是字典。如果指定 *locals* ，则它可以是任何映射对象。

   *expression* 参数作为 Python 表达式(从技术上讲其实是条件列表)解析和运算，并把 *globals* 和 *locals* 字典作为全局和局部的命名空间。如果有 *globals* 字典并且其中没有 '__builtins__' ，则在解析 *expression* 之前会把当前的全局变量复制到 *globals* 中去。这意味着 *expression* 通常总是可以完全访问标准的 :mod:`builtins` 模块，即使是在受限制的环境中。如果省略了 *locals* 字典，则它默认使用 *globals* 字典。如果两个字典都省略，则使用调用 :func:`eval` 的环境来执行表达式。其返回值是表达式的计算结果。如果有语法错误，则抛出异常。例如：

      >>> x = 1
      >>> eval('x+1')
      2

   这个函数还可以用来执行任意的代码对象(例如用 :func:`compile` 创建的)。这时，传进的参数是个代码对象而不是字符串。如果这个代码对象编译时已经用 ``'exec'`` 作为 *mode* 参数，则 :func:`eval` 的返回值为 ``None`` 。

   提示：\ :func:`exec` 函数支持动态执行语句。\ :func:`globals` 和 :func:`locals` 函数分别返回当前的全局和局部字典，它们可以用来传给 :func:`eval` 或者 :func:`exec` 。

   参见 :func:`ast.literal_eval` 函数；它能安全的对字符串中仅含源常量的表达式进行求值。


.. function:: exec(object[, globals[, locals]])

   这个函数支持动态执行 Python 代码。\ *object* 必须是字符串或者代码对象。如果是字符串，则把它当成包含 Python 语句的代码块来执行(除非发生语法错误)。\ [#]_\ 如果它是个代码块，则直接执行。不管怎样，要执行的代码都要是有效的文件输入(参见参考手册中的"File input"一节)。注意，\ :keyword:`return` 和 :keyword:`yield` 语句不可以在函数定义以外的地方使用，即使是对传给 :func:`exec` 函数的代码也是一样。其返回值是 ``None`` 。

   在任何情况下，如果省略了可选参数，则在当前作用域中执行代码。如果只提供了 *globals* ，则它必须是个字典，它会同时用作全局和局部变量。如果给出 *globals* 和 *locals* ，则分别用它们作为全局和局部变量。如果提供了 *locals* ，则它可以是任意映射对象。记住，在模块级别，全局和局部变量是相同的字典。如果 exec 得到两个不同的对象作为 *globals* 和 *locals* ，则会像在类定义中一样执行代码。

   如果 *globals* 中不包含其值为 ``__builtins__`` 的键名，则会在其中插入这个缺少的键名，其值是对内部模块 :mod:`builtins` 字典的引用。这样你就可以在 *globals* 中插入自己的 ``__builtins__`` 字典，然后传给 :func:`exec` ，从而能控制代码执行时哪些内部成员可用。

   .. note::

      内部函数 :func:`globals` 和 :func:`locals` 分别返回当前的全局和局部字典，它们可以用作第二和第三个参数传给 :func:`exec` 。

   .. note::

      对于 :func:`locals` ，默认的 *locals* 行为如下：不应该修改默认的 *locals* 字典。如果你想看到 :func:`exec` 返回后代码的执行对 *locals* 有何效果，就需要显式的传递一个 *locals* 字典。


.. function:: filter(function, iterable)

   Construct an iterator from those elements of *iterable* for which *function*
   returns true.  *iterable* may be either a sequence, a container which
   supports iteration, or an iterator.  If *function* is ``None``, the identity
   function is assumed, that is, all elements of *iterable* that are false are
   removed.

   Note that ``filter(function, iterable)`` is equivalent to the generator
   expression ``(item for item in iterable if function(item))`` if function is
   not ``None`` and ``(item for item in iterable if item)`` if function is
   ``None``.

   See :func:`itertools.filterfalse` for the complementary function that returns
   elements of *iterable* for which *function* returns false.


.. function:: float([x])

   .. index::
      single: NaN
      single: Infinity

   Convert a string or a number to floating point.

   If the argument is a string, it should contain a decimal number, optionally
   preceded by a sign, and optionally embedded in whitespace.  The optional
   sign may be ``'+'`` or ``'-'``; a ``'+'`` sign has no effect on the value
   produced.  The argument may also be a string representing a NaN
   (not-a-number), or a positive or negative infinity.  More precisely, the
   input must conform to the following grammar after leading and trailing
   whitespace characters are removed:

   .. productionlist::
      sign: "+" | "-"
      infinity: "Infinity" | "inf"
      nan: "nan"
      numeric_value: `floatnumber` | `infinity` | `nan`
      numeric_string: [`sign`] `numeric_value`

   Here ``floatnumber`` is the form of a Python floating-point literal,
   described in :ref:`floating`.  Case is not significant, so, for example,
   "inf", "Inf", "INFINITY" and "iNfINity" are all acceptable spellings for
   positive infinity.

   Otherwise, if the argument is an integer or a floating point number, a
   floating point number with the same value (within Python's floating point
   precision) is returned.  If the argument is outside the range of a Python
   float, an :exc:`OverflowError` will be raised.

   For a general Python object ``x``, ``float(x)`` delegates to
   ``x.__float__()``.

   If no argument is given, ``0.0`` is returned.

   Examples::

      >>> float('+1.23')
      1.23
      >>> float('   -12345\n')
      -12345.0
      >>> float('1e-003')
      0.001
      >>> float('+1E6')
      1000000.0
      >>> float('-Infinity')
      -inf

   The float type is described in :ref:`typesnumeric`.

   .. index::
      single: __format__
      single: string; format() (built-in function)


.. function:: format(value[, format_spec])

   Convert a *value* to a "formatted" representation, as controlled by
   *format_spec*.  The interpretation of *format_spec* will depend on the type
   of the *value* argument, however there is a standard formatting syntax that
   is used by most built-in types: :ref:`formatspec`.

   The default *format_spec* is an empty string which usually gives the same
   effect as calling :func:`str(value) <str>`.

   A call to ``format(value, format_spec)`` is translated to
   ``type(value).__format__(format_spec)`` which bypasses the instance
   dictionary when searching for the value's :meth:`__format__` method.  A
   :exc:`TypeError` exception is raised if the method is not found or if either
   the *format_spec* or the return value are not strings.


.. _func-frozenset:
.. function:: frozenset([iterable])
   :noindex:

   Return a new :class:`frozenset` object, optionally with elements taken from
   *iterable*.  ``frozenset`` is a built-in class.  See :class:`frozenset` and
   :ref:`types-set` for documentation about this class.

   For other containers see the built-in :class:`set`, :class:`list`,
   :class:`tuple`, and :class:`dict` classes, as well as the :mod:`collections`
   module.


.. function:: getattr(object, name[, default])

   Return the value of the named attribute of *object*.  *name* must be a string.
   If the string is the name of one of the object's attributes, the result is the
   value of that attribute.  For example, ``getattr(x, 'foobar')`` is equivalent to
   ``x.foobar``.  If the named attribute does not exist, *default* is returned if
   provided, otherwise :exc:`AttributeError` is raised.


.. function:: globals()

   Return a dictionary representing the current global symbol table. This is always
   the dictionary of the current module (inside a function or method, this is the
   module where it is defined, not the module from which it is called).


.. function:: hasattr(object, name)

   The arguments are an object and a string.  The result is ``True`` if the
   string is the name of one of the object's attributes, ``False`` if not. (This
   is implemented by calling ``getattr(object, name)`` and seeing whether it
   raises an :exc:`AttributeError` or not.)


.. function:: hash(object)

   Return the hash value of the object (if it has one).  Hash values are integers.
   They are used to quickly compare dictionary keys during a dictionary lookup.
   Numeric values that compare equal have the same hash value (even if they are of
   different types, as is the case for 1 and 1.0).


.. function:: help([object])

   Invoke the built-in help system.  (This function is intended for interactive
   use.)  If no argument is given, the interactive help system starts on the
   interpreter console.  If the argument is a string, then the string is looked up
   as the name of a module, function, class, method, keyword, or documentation
   topic, and a help page is printed on the console.  If the argument is any other
   kind of object, a help page on the object is generated.

   This function is added to the built-in namespace by the :mod:`site` module.


.. function:: hex(x)

   Convert an integer number to a hexadecimal string. The result is a valid Python
   expression.  If *x* is not a Python :class:`int` object, it has to define an
   :meth:`__index__` method that returns an integer.

   .. note::

      To obtain a hexadecimal string representation for a float, use the
      :meth:`float.hex` method.


.. function:: id(object)

   Return the "identity" of an object.  This is an integer which
   is guaranteed to be unique and constant for this object during its lifetime.
   Two objects with non-overlapping lifetimes may have the same :func:`id`
   value.

   .. impl-detail:: This is the address of the object in memory.


.. function:: input([prompt])

   If the *prompt* argument is present, it is written to standard output without
   a trailing newline.  The function then reads a line from input, converts it
   to a string (stripping a trailing newline), and returns that.  When EOF is
   read, :exc:`EOFError` is raised.  Example::

      >>> s = input('--> ')  # doctest: +SKIP
      --> Monty Python's Flying Circus
      >>> s  # doctest: +SKIP
      "Monty Python's Flying Circus"

   If the :mod:`readline` module was loaded, then :func:`input` will use it
   to provide elaborate line editing and history features.


.. function:: int(x=0)
              int(x, base=10)

   Convert a number or string *x* to an integer, or return ``0`` if no
   arguments are given.  If *x* is a number, return :meth:`x.__int__()
   <object.__int__>`.  For floating point numbers, this truncates towards zero.

   If *x* is not a number or if *base* is given, then *x* must be a string,
   :class:`bytes`, or :class:`bytearray` instance representing an :ref:`integer
   literal <integers>` in radix *base*.  Optionally, the literal can be
   preceded by ``+`` or ``-`` (with no space in between) and surrounded by
   whitespace.  A base-n literal consists of the digits 0 to n-1, with ``a``
   to ``z`` (or ``A`` to ``Z``) having
   values 10 to 35.  The default *base* is 10. The allowed values are 0 and 2-36.
   Base-2, -8, and -16 literals can be optionally prefixed with ``0b``/``0B``,
   ``0o``/``0O``, or ``0x``/``0X``, as with integer literals in code.  Base 0
   means to interpret exactly as a code literal, so that the actual base is 2,
   8, 10, or 16, and so that ``int('010', 0)`` is not legal, while
   ``int('010')`` is, as well as ``int('010', 8)``.

   The integer type is described in :ref:`typesnumeric`.


.. function:: isinstance(object, classinfo)

   Return true if the *object* argument is an instance of the *classinfo*
   argument, or of a (direct, indirect or :term:`virtual <虚基类>`) subclass thereof.  If *object* is not
   an object of the given type, the function always returns false.  If
   *classinfo* is not a class (type object), it may be a tuple of type objects,
   or may recursively contain other such tuples (other sequence types are not
   accepted).  If *classinfo* is not a type or tuple of types and such tuples,
   a :exc:`TypeError` exception is raised.


.. function:: issubclass(class, classinfo)

   Return true if *class* is a subclass (direct, indirect or :term:`virtual <虚基类>`) of *classinfo*.  A
   class is considered a subclass of itself. *classinfo* may be a tuple of class
   objects, in which case every entry in *classinfo* will be checked. In any other
   case, a :exc:`TypeError` exception is raised.


.. function:: iter(object[, sentinel])

   Return an :term:`迭代器` object.  The first argument is interpreted very
   differently depending on the presence of the second argument. Without a
   second argument, *object* must be a collection object which supports the
   iteration protocol (the :meth:`__iter__` method), or it must support the
   sequence protocol (the :meth:`__getitem__` method with integer arguments
   starting at ``0``).  If it does not support either of those protocols,
   :exc:`TypeError` is raised. If the second argument, *sentinel*, is given,
   then *object* must be a callable object.  The iterator created in this case
   will call *object* with no arguments for each call to its
   :meth:`~iterator.__next__` method; if the value returned is equal to
   *sentinel*, :exc:`StopIteration` will be raised, otherwise the value will
   be returned.

   See also :ref:`typeiter`.

   One useful application of the second form of :func:`iter` is to read lines of
   a file until a certain line is reached.  The following example reads a file
   until the :meth:`readline` method returns an empty string::

      with open('mydata.txt') as fp:
          for line in iter(fp.readline, ''):
              process_line(line)


.. function:: len(s)

   Return the length (the number of items) of an object.  The argument may be a
   sequence (string, tuple or list) or a mapping (dictionary).


.. _func-list:
.. function:: list([iterable])
   :noindex:

   Rather than being a function, :class:`list` is actually a mutable
   sequence type, as documented in :ref:`typesseq-list` and :ref:`typesseq`.


.. function:: locals()

   Update and return a dictionary representing the current local symbol table.
   Free variables are returned by :func:`locals` when it is called in function
   blocks, but not in class blocks.

   .. note::
      The contents of this dictionary should not be modified; changes may not
      affect the values of local and free variables used by the interpreter.

.. function:: map(function, iterable, ...)

   Return an iterator that applies *function* to every item of *iterable*,
   yielding the results.  If additional *iterable* arguments are passed,
   *function* must take that many arguments and is applied to the items from all
   iterables in parallel.  With multiple iterables, the iterator stops when the
   shortest iterable is exhausted.  For cases where the function inputs are
   already arranged into argument tuples, see :func:`itertools.starmap`\.


.. function:: max(iterable, *[, key])
              max(arg1, arg2, *args[, key])

   Return the largest item in an iterable or the largest of two or more
   arguments.

   If one positional argument is provided, *iterable* must be a non-empty
   iterable (such as a non-empty string, tuple or list).  The largest item
   in the iterable is returned.  If two or more positional arguments are
   provided, the largest of the positional arguments is returned.

   The optional keyword-only *key* argument specifies a one-argument ordering
   function like that used for :meth:`list.sort`.

   If multiple items are maximal, the function returns the first one
   encountered.  This is consistent with other sort-stability preserving tools
   such as ``sorted(iterable, key=keyfunc, reverse=True)[0]`` and
   ``heapq.nlargest(1, iterable, key=keyfunc)``.


.. _func-memoryview:
.. function:: memoryview(obj)
   :noindex:

   Return a "memory view" object created from the given argument.  See
   :ref:`typememoryview` for more information.


.. function:: min(iterable, *[, key])
              min(arg1, arg2, *args[, key])

   Return the smallest item in an iterable or the smallest of two or more
   arguments.

   If one positional argument is provided, *iterable* must be a non-empty
   iterable (such as a non-empty string, tuple or list).  The smallest item
   in the iterable is returned.  If two or more positional arguments are
   provided, the smallest of the positional arguments is returned.

   The optional keyword-only *key* argument specifies a one-argument ordering
   function like that used for :meth:`list.sort`.

   If multiple items are minimal, the function returns the first one
   encountered.  This is consistent with other sort-stability preserving tools
   such as ``sorted(iterable, key=keyfunc)[0]`` and ``heapq.nsmallest(1,
   iterable, key=keyfunc)``.

.. function:: next(iterator[, default])

   Retrieve the next item from the *iterator* by calling its
   :meth:`~iterator.__next__` method.  If *default* is given, it is returned
   if the iterator is exhausted, otherwise :exc:`StopIteration` is raised.


.. function:: object()

   Return a new featureless object.  :class:`object` is a base for all classes.
   It has the methods that are common to all instances of Python classes.  This
   function does not accept any arguments.

   .. note::

      :class:`object` does *not* have a :attr:`__dict__`, so you can't assign
      arbitrary attributes to an instance of the :class:`object` class.


.. function:: oct(x)

   Convert an integer number to an octal string.  The result is a valid Python
   expression.  If *x* is not a Python :class:`int` object, it has to define an
   :meth:`__index__` method that returns an integer.


   .. index::
      single: file object; open() built-in function

.. function:: open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

   Open *file* and return a corresponding :term:`文件对象`.  If the file
   cannot be opened, an :exc:`OSError` is raised.

   *file* is either a string or bytes object giving the pathname (absolute or
   relative to the current working directory) of the file to be opened or
   an integer file descriptor of the file to be wrapped.  (If a file descriptor
   is given, it is closed when the returned I/O object is closed, unless
   *closefd* is set to ``False``.)

   *mode* is an optional string that specifies the mode in which the file is
   opened.  It defaults to ``'r'`` which means open for reading in text mode.
   Other common values are ``'w'`` for writing (truncating the file if it
   already exists), ``'x'`` for exclusive creation and ``'a'`` for appending
   (which on *some* Unix systems, means that *all* writes append to the end of
   the file regardless of the current seek position).  In text mode, if
   *encoding* is not specified the encoding used is platform dependent:
   ``locale.getpreferredencoding(False)`` is called to get the current locale
   encoding. (For reading and writing raw bytes use binary mode and leave
   *encoding* unspecified.)  The available modes are:

   ========= ===============================================================
   Character Meaning
   --------- ---------------------------------------------------------------
   ``'r'``   open for reading (default)
   ``'w'``   open for writing, truncating the file first
   ``'x'``   open for exclusive creation, failing if the file already exists
   ``'a'``   open for writing, appending to the end of the file if it exists
   ``'b'``   binary mode
   ``'t'``   text mode (default)
   ``'+'``   open a disk file for updating (reading and writing)
   ``'U'``   universal newlines mode (for backwards compatibility; should
             not be used in new code)
   ========= ===============================================================

   The default mode is ``'r'`` (open for reading text, synonym of ``'rt'``).
   For binary read-write access, the mode ``'w+b'`` opens and truncates the file
   to 0 bytes.  ``'r+b'`` opens the file without truncation.

   As mentioned in the :ref:`io-overview`, Python distinguishes between binary
   and text I/O.  Files opened in binary mode (including ``'b'`` in the *mode*
   argument) return contents as :class:`bytes` objects without any decoding.  In
   text mode (the default, or when ``'t'`` is included in the *mode* argument),
   the contents of the file are returned as :class:`str`, the bytes having been
   first decoded using a platform-dependent encoding or using the specified
   *encoding* if given.

   .. note::

      Python doesn't depend on the underlying operating system's notion of text
      files; all the processing is done by Python itself, and is therefore
      platform-independent.

   *buffering* is an optional integer used to set the buffering policy.  Pass 0
   to switch buffering off (only allowed in binary mode), 1 to select line
   buffering (only usable in text mode), and an integer > 1 to indicate the size
   of a fixed-size chunk buffer.  When no *buffering* argument is given, the
   default buffering policy works as follows:

   * Binary files are buffered in fixed-size chunks; the size of the buffer is
     chosen using a heuristic trying to determine the underlying device's "block
     size" and falling back on :attr:`io.DEFAULT_BUFFER_SIZE`.  On many systems,
     the buffer will typically be 4096 or 8192 bytes long.

   * "Interactive" text files (files for which :meth:`isatty` returns True) use
     line buffering.  Other text files use the policy described above for binary
     files.

   *encoding* is the name of the encoding used to decode or encode the file.
   This should only be used in text mode.  The default encoding is platform
   dependent (whatever :func:`locale.getpreferredencoding` returns), but any
   encoding supported by Python can be used.  See the :mod:`codecs` module for
   the list of supported encodings.

   *errors* is an optional string that specifies how encoding and decoding
   errors are to be handled--this cannot be used in binary mode.  Pass
   ``'strict'`` to raise a :exc:`ValueError` exception if there is an encoding
   error (the default of ``None`` has the same effect), or pass ``'ignore'`` to
   ignore errors.  (Note that ignoring encoding errors can lead to data loss.)
   ``'replace'`` causes a replacement marker (such as ``'?'``) to be inserted
   where there is malformed data.  When writing, ``'xmlcharrefreplace'``
   (replace with the appropriate XML character reference) or
   ``'backslashreplace'`` (replace with backslashed escape sequences) can be
   used.  Any other error handling name that has been registered with
   :func:`codecs.register_error` is also valid.

   .. index::
      single: universal newlines; open() built-in function

   *newline* controls how :term:`万能换行符` mode works (it only
   applies to text mode).  It can be ``None``, ``''``, ``'\n'``, ``'\r'``, and
   ``'\r\n'``.  It works as follows:

   * When reading input from the stream, if *newline* is ``None``, universal
     newlines mode is enabled.  Lines in the input can end in ``'\n'``,
     ``'\r'``, or ``'\r\n'``, and these are translated into ``'\n'`` before
     being returned to the caller.  If it is ``''``, universal newlines mode is
     enabled, but line endings are returned to the caller untranslated.  If it
     has any of the other legal values, input lines are only terminated by the
     given string, and the line ending is returned to the caller untranslated.

   * When writing output to the stream, if *newline* is ``None``, any ``'\n'``
     characters written are translated to the system default line separator,
     :data:`os.linesep`.  If *newline* is ``''`` or ``'\n'``, no translation
     takes place.  If *newline* is any of the other legal values, any ``'\n'``
     characters written are translated to the given string.

   If *closefd* is ``False`` and a file descriptor rather than a filename was
   given, the underlying file descriptor will be kept open when the file is
   closed.  If a filename is given *closefd* has no effect and must be ``True``
   (the default).

   A custom opener can be used by passing a callable as *opener*. The underlying
   file descriptor for the file object is then obtained by calling *opener* with
   (*file*, *flags*). *opener* must return an open file descriptor (passing
   :mod:`os.open` as *opener* results in functionality similar to passing
   ``None``).

   The following example uses the :ref:`dir_fd <dir_fd>` parameter of the
   :func:`os.open` function to open a file relative to a given directory::

      >>> import os
      >>> dir_fd = os.open('somedir', os.O_RDONLY)
      >>> def opener(path, flags):
      ...     return os.open(path, flags, dir_fd=dir_fd)
      ...
      >>> with open('spamspam.txt', 'w', opener=opener) as f:
      ...     print('This will be written to somedir/spamspam.txt', file=f)
      ...
      >>> os.close(dir_fd)  # don't leak a file descriptor

   .. versionchanged:: 3.3
      The *opener* parameter was added.
      The ``'x'`` mode was added.

   The type of :term:`文件对象` returned by the :func:`open` function
   depends on the mode.  When :func:`open` is used to open a file in a text
   mode (``'w'``, ``'r'``, ``'wt'``, ``'rt'``, etc.), it returns a subclass of
   :class:`io.TextIOBase` (specifically :class:`io.TextIOWrapper`).  When used
   to open a file in a binary mode with buffering, the returned class is a
   subclass of :class:`io.BufferedIOBase`.  The exact class varies: in read
   binary mode, it returns a :class:`io.BufferedReader`; in write binary and
   append binary modes, it returns a :class:`io.BufferedWriter`, and in
   read/write mode, it returns a :class:`io.BufferedRandom`.  When buffering is
   disabled, the raw stream, a subclass of :class:`io.RawIOBase`,
   :class:`io.FileIO`, is returned.

   .. index::
      single: line-buffered I/O
      single: unbuffered I/O
      single: buffer size, I/O
      single: I/O control; buffering
      single: binary mode
      single: text mode
      module: sys

   See also the file handling modules, such as, :mod:`fileinput`, :mod:`io`
   (where :func:`open` is declared), :mod:`os`, :mod:`os.path`, :mod:`tempfile`,
   and :mod:`shutil`.

   .. versionchanged:: 3.3
      :exc:`IOError` used to be raised, it is now an alias of :exc:`OSError`.
      :exc:`FileExistsError` is now raised if the file opened in exclusive
      creation mode (``'x'``) already exists.


.. XXX works for bytes too, but should it?
.. function:: ord(c)

   Given a string representing one Unicode character, return an integer
   representing the Unicode code
   point of that character.  For example, ``ord('a')`` returns the integer ``97``
   and ``ord('\u2020')`` returns ``8224``.  This is the inverse of :func:`chr`.


.. function:: pow(x, y[, z])

   Return *x* to the power *y*; if *z* is present, return *x* to the power *y*,
   modulo *z* (computed more efficiently than ``pow(x, y) % z``). The two-argument
   form ``pow(x, y)`` is equivalent to using the power operator: ``x**y``.

   The arguments must have numeric types.  With mixed operand types, the
   coercion rules for binary arithmetic operators apply.  For :class:`int`
   operands, the result has the same type as the operands (after coercion)
   unless the second argument is negative; in that case, all arguments are
   converted to float and a float result is delivered.  For example, ``10**2``
   returns ``100``, but ``10**-2`` returns ``0.01``.  If the second argument is
   negative, the third argument must be omitted.  If *z* is present, *x* and *y*
   must be of integer types, and *y* must be non-negative.


.. function:: print(*objects, sep=' ', end='\\n', file=sys.stdout, flush=False)

   Print *objects* to the stream *file*, separated by *sep* and followed by
   *end*.  *sep*, *end* and *file*, if present, must be given as keyword
   arguments.

   All non-keyword arguments are converted to strings like :func:`str` does and
   written to the stream, separated by *sep* and followed by *end*.  Both *sep*
   and *end* must be strings; they can also be ``None``, which means to use the
   default values.  If no *objects* are given, :func:`print` will just write
   *end*.

   The *file* argument must be an object with a ``write(string)`` method; if it
   is not present or ``None``, :data:`sys.stdout` will be used.  Whether output
   is buffered is usually determined by *file*, but if the  *flush* keyword
   argument is true, the stream is forcibly flushed.

   .. versionchanged:: 3.3
      Added the *flush* keyword argument.


.. function:: property(fget=None, fset=None, fdel=None, doc=None)

   Return a property attribute.

   *fget* is a function for getting an attribute value, likewise *fset* is a
   function for setting, and *fdel* a function for del'ing, an attribute.  Typical
   use is to define a managed attribute ``x``::

      class C:
          def __init__(self):
              self._x = None

          def getx(self):
              return self._x
          def setx(self, value):
              self._x = value
          def delx(self):
              del self._x
          x = property(getx, setx, delx, "I'm the 'x' property.")

   If then *c* is an instance of *C*, ``c.x`` will invoke the getter,
   ``c.x = value`` will invoke the setter and ``del c.x`` the deleter.

   If given, *doc* will be the docstring of the property attribute. Otherwise, the
   property will copy *fget*'s docstring (if it exists).  This makes it possible to
   create read-only properties easily using :func:`property` as a :term:`迭代器`::

      class Parrot:
          def __init__(self):
              self._voltage = 100000

          @property
          def voltage(self):
              """Get the current voltage."""
              return self._voltage

   turns the :meth:`voltage` method into a "getter" for a read-only attribute
   with the same name.

   A property object has :attr:`getter`, :attr:`setter`, and :attr:`deleter`
   methods usable as decorators that create a copy of the property with the
   corresponding accessor function set to the decorated function.  This is
   best explained with an example::

      class C:
          def __init__(self):
              self._x = None

          @property
          def x(self):
              """I'm the 'x' property."""
              return self._x

          @x.setter
          def x(self, value):
              self._x = value

          @x.deleter
          def x(self):
              del self._x

   This code is exactly equivalent to the first example.  Be sure to give the
   additional functions the same name as the original property (``x`` in this
   case.)

   The returned property also has the attributes ``fget``, ``fset``, and
   ``fdel`` corresponding to the constructor arguments.


.. _func-range:
.. function:: range(stop)
              range(start, stop[, step])
   :noindex:

   Rather than being a function, :class:`range` is actually an immutable
   sequence type, as documented in :ref:`typesseq-range` and :ref:`typesseq`.


.. function:: repr(object)

   Return a string containing a printable representation of an object.  For many
   types, this function makes an attempt to return a string that would yield an
   object with the same value when passed to :func:`eval`, otherwise the
   representation is a string enclosed in angle brackets that contains the name
   of the type of the object together with additional information often
   including the name and address of the object.  A class can control what this
   function returns for its instances by defining a :meth:`__repr__` method.


.. function:: reversed(seq)

   Return a reverse :term:`迭代器`.  *seq* must be an object which has
   a :meth:`__reversed__` method or supports the sequence protocol (the
   :meth:`__len__` method and the :meth:`__getitem__` method with integer
   arguments starting at ``0``).


.. function:: round(number[, ndigits])

   Return the floating point value *number* rounded to *ndigits* digits after
   the decimal point.  If *ndigits* is omitted, it defaults to zero. Delegates
   to ``number.__round__(ndigits)``.

   For the built-in types supporting :func:`round`, values are rounded to the
   closest multiple of 10 to the power minus *ndigits*; if two multiples are
   equally close, rounding is done toward the even choice (so, for example,
   both ``round(0.5)`` and ``round(-0.5)`` are ``0``, and ``round(1.5)`` is
   ``2``).  The return value is an integer if called with one argument,
   otherwise of the same type as *number*.

   .. note::

      The behavior of :func:`round` for floats can be surprising: for example,
      ``round(2.675, 2)`` gives ``2.67`` instead of the expected ``2.68``.
      This is not a bug: it's a result of the fact that most decimal fractions
      can't be represented exactly as a float.  See :ref:`tut-fp-issues` for
      more information.


.. _func-set:
.. function:: set([iterable])
   :noindex:

   Return a new :class:`set` object, optionally with elements taken from
   *iterable*.  ``set`` is a built-in class.  See :class:`set` and
   :ref:`types-set` for documentation about this class.

   For other containers see the built-in :class:`frozenset`, :class:`list`,
   :class:`tuple`, and :class:`dict` classes, as well as the :mod:`collections`
   module.


.. function:: setattr(object, name, value)

   This is the counterpart of :func:`getattr`.  The arguments are an object, a
   string and an arbitrary value.  The string may name an existing attribute or a
   new attribute.  The function assigns the value to the attribute, provided the
   object allows it.  For example, ``setattr(x, 'foobar', 123)`` is equivalent to
   ``x.foobar = 123``.


.. function:: slice(stop)
              slice(start, stop[, step])

   .. index:: single: Numerical Python

   Return a :term:`切片` object representing the set of indices specified by
   ``range(start, stop, step)``.  The *start* and *step* arguments default to
   ``None``.  Slice objects have read-only data attributes :attr:`start`,
   :attr:`stop` and :attr:`step` which merely return the argument values (or their
   default).  They have no other explicit functionality; however they are used by
   Numerical Python and other third party extensions.  Slice objects are also
   generated when extended indexing syntax is used.  For example:
   ``a[start:stop:step]`` or ``a[start:stop, i]``.  See :func:`itertools.islice`
   for an alternate version that returns an iterator.


.. function:: sorted(iterable[, key][, reverse])

   Return a new sorted list from the items in *iterable*.

   Has two optional arguments which must be specified as keyword arguments.

   *key* specifies a function of one argument that is used to extract a comparison
   key from each list element: ``key=str.lower``.  The default value is ``None``
   (compare the elements directly).

   *reverse* is a boolean value.  If set to ``True``, then the list elements are
   sorted as if each comparison were reversed.

   Use :func:`functools.cmp_to_key` to convert an old-style *cmp* function to a
   *key* function.

   For sorting examples and a brief sorting tutorial, see `Sorting HowTo
   <http://wiki.python.org/moin/HowTo/Sorting/>`_\.

.. function:: staticmethod(function)

   Return a static method for *function*.

   A static method does not receive an implicit first argument. To declare a static
   method, use this idiom::

      class C:
          @staticmethod
          def f(arg1, arg2, ...): ...

   The ``@staticmethod`` form is a function :term:`迭代器` -- see the
   description of function definitions in :ref:`function` for details.

   It can be called either on the class (such as ``C.f()``) or on an instance (such
   as ``C().f()``).  The instance is ignored except for its class.

   Static methods in Python are similar to those found in Java or C++. Also see
   :func:`classmethod` for a variant that is useful for creating alternate class
   constructors.

   For more information on static methods, consult the documentation on the
   standard type hierarchy in :ref:`types`.

   .. index::
      single: string; str() (built-in function)


.. _func-str:
.. function:: str(object='')
              str(object=b'', encoding='utf-8', errors='strict')
   :noindex:

   Return a :class:`str` version of *object*.  See :func:`str` for details.

   ``str`` is the built-in string :term:`类`.  For general information
   about strings, see :ref:`textseq`.


.. function:: sum(iterable[, start])

   Sums *start* and the items of an *iterable* from left to right and returns the
   total.  *start* defaults to ``0``. The *iterable*'s items are normally numbers,
   and the start value is not allowed to be a string.

   For some use cases, there are good alternatives to :func:`sum`.
   The preferred, fast way to concatenate a sequence of strings is by calling
   ``''.join(sequence)``.  To add floating point values with extended precision,
   see :func:`math.fsum`\.  To concatenate a series of iterables, consider using
   :func:`itertools.chain`.

.. function:: super([type[, object-or-type]])

   Return a proxy object that delegates method calls to a parent or sibling
   class of *type*.  This is useful for accessing inherited methods that have
   been overridden in a class. The search order is same as that used by
   :func:`getattr` except that the *type* itself is skipped.

   The :attr:`__mro__` attribute of the *type* lists the method resolution
   search order used by both :func:`getattr` and :func:`super`.  The attribute
   is dynamic and can change whenever the inheritance hierarchy is updated.

   If the second argument is omitted, the super object returned is unbound.  If
   the second argument is an object, ``isinstance(obj, type)`` must be true.  If
   the second argument is a type, ``issubclass(type2, type)`` must be true (this
   is useful for classmethods).

   There are two typical use cases for *super*.  In a class hierarchy with
   single inheritance, *super* can be used to refer to parent classes without
   naming them explicitly, thus making the code more maintainable.  This use
   closely parallels the use of *super* in other programming languages.

   The second use case is to support cooperative multiple inheritance in a
   dynamic execution environment.  This use case is unique to Python and is
   not found in statically compiled languages or languages that only support
   single inheritance.  This makes it possible to implement "diamond diagrams"
   where multiple base classes implement the same method.  Good design dictates
   that this method have the same calling signature in every case (because the
   order of calls is determined at runtime, because that order adapts
   to changes in the class hierarchy, and because that order can include
   sibling classes that are unknown prior to runtime).

   For both use cases, a typical superclass call looks like this::

      class C(B):
          def method(self, arg):
              super().method(arg)    # This does the same thing as:
                                     # super(C, self).method(arg)

   Note that :func:`super` is implemented as part of the binding process for
   explicit dotted attribute lookups such as ``super().__getitem__(name)``.
   It does so by implementing its own :meth:`__getattribute__` method for searching
   classes in a predictable order that supports cooperative multiple inheritance.
   Accordingly, :func:`super` is undefined for implicit lookups using statements or
   operators such as ``super()[name]``.

   Also note that, aside from the zero argument form, :func:`super` is not
   limited to use inside methods.  The two argument form specifies the
   arguments exactly and makes the appropriate references.  The zero
   argument form only works inside a class definition, as the compiler fills
   in the necessary details to correctly retrieve the class being defined,
   as well as accessing the current instance for ordinary methods.

   For practical suggestions on how to design cooperative classes using
   :func:`super`, see `guide to using super()
   <http://rhettinger.wordpress.com/2011/05/26/super-considered-super/>`_.


.. _func-tuple:
.. function:: tuple([iterable])
   :noindex:

   Rather than being a function, :class:`tuple` is actually an immutable
   sequence type, as documented in :ref:`typesseq-tuple` and :ref:`typesseq`.


.. function:: type(object)
              type(name, bases, dict)

   .. index:: object: type


   With one argument, return the type of an *object*.  The return value is a
   type object and generally the same object as returned by ``object.__class__``.

   The :func:`isinstance` built-in function is recommended for testing the type
   of an object, because it takes subclasses into account.


   With three arguments, return a new type object.  This is essentially a
   dynamic form of the :keyword:`class` statement. The *name* string is the
   class name and becomes the :attr:`__name__` attribute; the *bases* tuple
   itemizes the base classes and becomes the :attr:`__bases__` attribute;
   and the *dict* dictionary is the namespace containing definitions for class
   body and becomes the :attr:`__dict__` attribute.  For example, the
   following two statements create identical :class:`type` objects:

      >>> class X:
      ...     a = 1
      ...
      >>> X = type('X', (object,), dict(a=1))

   See also :ref:`bltin-type-objects`.


.. function:: vars([object])

   Without an argument, act like :func:`locals`.

   With a module, class or class instance object as argument (or anything else that
   has a :attr:`__dict__` attribute), return that attribute.

   .. note::
      The returned dictionary should not be modified:
      the effects on the corresponding symbol table are undefined. [#]_

.. function:: zip(*iterables)

   Make an iterator that aggregates elements from each of the iterables.

   Returns an iterator of tuples, where the *i*-th tuple contains
   the *i*-th element from each of the argument sequences or iterables.  The
   iterator stops when the shortest input iterable is exhausted. With a single
   iterable argument, it returns an iterator of 1-tuples.  With no arguments,
   it returns an empty iterator.  Equivalent to::

        def zip(*iterables):
            # zip('ABCD', 'xy') --> Ax By
            sentinel = object()
            iterators = [iter(it) for it in iterables]
            while iterators:
                result = []
                for it in iterators:
                    elem = next(it, sentinel)
                    if elem is sentinel:
                        return
                    result.append(elem)
                yield tuple(result)

   The left-to-right evaluation order of the iterables is guaranteed. This
   makes possible an idiom for clustering a data series into n-length groups
   using ``zip(*[iter(s)]*n)``.

   :func:`zip` should only be used with unequal length inputs when you don't
   care about trailing, unmatched values from the longer iterables.  If those
   values are important, use :func:`itertools.zip_longest` instead.

   :func:`zip` in conjunction with the ``*`` operator can be used to unzip a
   list::

      >>> x = [1, 2, 3]
      >>> y = [4, 5, 6]
      >>> zipped = zip(x, y)
      >>> list(zipped)
      [(1, 4), (2, 5), (3, 6)]
      >>> x2, y2 = zip(*zip(x, y))
      >>> x == list(x2) and y == list(y2)
      True


.. function:: __import__(name, globals=None, locals=None, fromlist=(), level=0)

   .. index::
      statement: import
      module: imp

   .. note::

      This is an advanced function that is not needed in everyday Python
      programming, unlike :func:`importlib.import_module`.

   This function is invoked by the :keyword:`import` statement.  It can be
   replaced (by importing the :mod:`builtins` module and assigning to
   ``builtins.__import__``) in order to change semantics of the
   :keyword:`import` statement, but nowadays it is usually simpler to use import
   hooks (see :pep:`302`) to attain the same goals.  Direct use of
   :func:`__import__` is entirely discouraged in favor of
   :func:`importlib.import_module`.

   The function imports the module *name*, potentially using the given *globals*
   and *locals* to determine how to interpret the name in a package context.
   The *fromlist* gives the names of objects or submodules that should be
   imported from the module given by *name*.  The standard implementation does
   not use its *locals* argument at all, and uses its *globals* only to
   determine the package context of the :keyword:`import` statement.

   *level* specifies whether to use absolute or relative imports. ``0`` (the
   default) means only perform absolute imports.  Positive values for
   *level* indicate the number of parent directories to search relative to the
   directory of the module calling :func:`__import__` (see :pep:`328` for the
   details).

   When the *name* variable is of the form ``package.module``, normally, the
   top-level package (the name up till the first dot) is returned, *not* the
   module named by *name*.  However, when a non-empty *fromlist* argument is
   given, the module named by *name* is returned.

   For example, the statement ``import spam`` results in bytecode resembling the
   following code::

      spam = __import__('spam', globals(), locals(), [], 0)

   The statement ``import spam.ham`` results in this call::

      spam = __import__('spam.ham', globals(), locals(), [], 0)

   Note how :func:`__import__` returns the toplevel module here because this is
   the object that is bound to a name by the :keyword:`import` statement.

   On the other hand, the statement ``from spam.ham import eggs, sausage as
   saus`` results in ::

      _temp = __import__('spam.ham', globals(), locals(), ['eggs', 'sausage'], 0)
      eggs = _temp.eggs
      saus = _temp.sausage

   Here, the ``spam.ham`` module is returned from :func:`__import__`.  From this
   object, the names to import are retrieved and assigned to their respective
   names.

   If you simply want to import a module (potentially within a package) by name,
   use :func:`importlib.import_module`.

   .. versionchanged:: 3.3
      Negative values for *level* are no longer supported (which also changes
      the default value to 0).


.. rubric:: 脚注

.. [#] 注意，解析器只接受 Unix 风格的行结束符。如果从文件中读取代码，要确保使用换行符转换模式来处理 Windows 或 Mac 风格的换行符。

.. [#] 在当前实现中，这样做通常不会影响本地变量的绑定，但从其它作用域(例如模块)获得的变量可能受影响。这点可能会改变。
