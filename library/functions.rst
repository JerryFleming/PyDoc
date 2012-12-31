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

   * 如果它是个和 *buffer* 界面兼容的对象，则使用这个对象的只读缓冲区来初始化字节数组。

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

   这里的 ``@classmethod`` 形式是个函数\ :term:`修饰符` --- 详情参见\ :ref:`function`\ 中对函数定义的描述。

   它既可以用类(例如 ``C.f()``)也可以用实例(例如 ``C().f()``)来调用。对于实例，仅使用其类而忽略其它。如果在派生类中调用类方法，则把派生类对象作为隐含是第一个参数。

   类方法和 C++ 或 Java 中的静态方法是不同的。如是你需要静态方法，参见本节的 :func:`staticmethod` 。

   关于类方法的更多信息，请查阅\ :ref:`types`\ 中标准类型体系的文档。


.. function:: compile(source, filename, mode, flags=0, dont_inherit=False, optimize=-1)

   把 *source* 编译成 AST 代码对象。代码对象可以用 :func:`exec` 或者 :func:`eval` 来执行。\ *source* 可以是字符串或者 AST 对象。关于如何使用 AST 对象参见 :mod:`ast` 模块的文档。

   *filename* 参数应该指定从中读取代码的文件，如果不是从文件读取则可以传入一个易于识别的标识(通常用 ``'<string>'``)。

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

   它和 :func:`setattr` 相关联，其参数是一个对象 object 和字符串 name 。name 必须是 object 一个属性的名字。如果允许，函数会删除对象的指定的属性。例如，\ ``delattr(x, 'foobar')`` 相当于 ``del x.foobar`` 。


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

   接受两个(不是复数的)数值参数，返回一对数值，其中分别是其整除时的商和余数。如果操作数类型不同，则会应用二元运算符的精度转换规则。对于整数，结果和 ``(a // b, a % b)`` 是一样的。对于浮点数，结果是 ``(q, a % b)`` ，其中的 *q* 通常是 ``math.floor(a /
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

   返回一个迭代器，其元素是 *iterable* 中的元素经过 *function* 过滤返回为真的那些。\ *iterable* 可以是个序列、支持迭代的容器、或者迭代器。如果 *function* 是 ``None`` ，则内定使用布尔识别函数，即去除 *iterable* 中所有为假的元素。

   注意，如果 function 不是 ``None`` ，则 ``filter(function, iterable)`` 相当于生成函数表达式 ``(item for item in iterable if function(item))`` ；而如果 function 是 ``None`` ，则相当于 ``(item for item in iterable if item)`` 。

   参见See :func:`itertools.filterfalse` for the complementary function that returns
   elements of *iterable* for which *function* returns false.


.. function:: float([x])

   .. index::
      single: NaN
      single: Infinity

   把字符串或整数转化为浮点数。

   如果参数是字符串，则其中应该包含一个十进制整数，前面有可选的符号，然后整体可能在空格之中。可选的符号可以是 ``'+'`` 或者 ``'-'`` ，使用 ``'+'`` 号对产生的值没有影响。参数也可以是个表示 NaN (not-a-number)的字符串，或者正无穷或负无穷。更准确的说，输入的东西在去除前后的空格之后必须符合下面的语法：

   .. productionlist::
      sign: "+" | "-"
      infinity: "Infinity" | "inf"
      nan: "nan"
      numeric_value: `floatnumber` | `infinity` | `nan`
      numeric_string: [`sign`] `numeric_value`

   这里的 ``floatnumber`` 是 Python 中浮点数源常量的形式，在\ :ref:`floating`\ 中介绍。大小写是不重要的，例如，"inf"、"Inf"、"INFINITY"、"iNfINity" 都是正无穷的正确拼写。

   否则，如果参数是个整数或浮点数，则返回一个(在 Python 的浮点数精度之内)值相等的浮点数。如果参数在 Python 浮点数范围之外，则抛出 :exc:`OverflowError` 。

   对于一般的 Python 对象 ``x`` ，\ ``float(x)`` 会调用 ``x.__float__()`` 。

   如果没有参数，则返回 ``0.0`` 。

   例如::

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

   浮点数类型在\ :ref:`typesnumeric`\ 介绍。

   .. index::
      single: __format__
      single: string; format() (内置函数)


.. function:: format(value[, format_spec])

   按照 *format_spec* 的规定把 *value* 转化成"格式化"形式。怎么解释 *format_spec* 依赖于 *value* 参数的类型，但有个标准的格式化语法可以用于大部分内置类型：\ :ref:`formatspec`\ 。

   默认的 *format_spec* 是个空字符串，它的效果通常和调用 :func:`str(value) <str>` 是一样的。

   对 ``format(value, format_spec)`` 的调用会被翻译成 ``type(value).__format__(format_spec)`` ，这样就在查找 value 的 :meth:`__format__` 方法时避开了实例的字典。如果这个方法没有找到，或者 *format_spec* 或返回值不是字符串，则抛出 :exc:`TypeError` 异常。


.. _func-frozenset:
.. function:: frozenset([iterable])
   :noindex:

   返回一个新的 :class:`frozenset` 对象，其中的元素是可选的并且来自 *iterable* 。\ ``frozenset`` 是一个内置的类，其文档参见 :class:`frozenset` 和\ :ref:`types-set`\ 。

   关于其它容器，参见内置的 :class:`set` 、\ :class:`list` 、\ :class:`tuple` 、\ :class:`dict` 类，以及 :mod:`collections` 模块。


.. function:: getattr(object, name[, default])

   返回 *object* 指定属性的值。\ *name* 必须是个字符串；如果它是 object 某个属性的名字，则结果就是这个属性的值。例如，\ ``getattr(x, 'foobar')`` 相当于 ``x.foobar`` 。如果指定的属性不存在，而 *default* 存在，则返回 default 的值，否则抛出 :exc:`AttributeError` 。


.. function:: globals()

   返回一个字典，代表当前的全局符号表。它总是当前模块的字典(在函数和方法内部，它是定义函数或方法的模块，而不是调用它们的模块).


.. function:: hasattr(object, name)

   参数是一个对象 object 和一个字符串 name 。如果 name 是 object 的某个属性的名字则返回 ``True`` ，否则返回 ``False`` (它的实现方法是，调用 ``getattr(object, name)`` 看看是否会抛出 :exc:`AttributeError`)。


.. function:: hash(object)

   返回 object 的散列值(如果有)。散列值是个整数，用来在字典查找时快速比较字典的键。相等的数值有相同的散列值(即使它们类型不同，例如 1 和 1.0 的情形)。


.. function:: help([object])

   启动内部的帮助系统。(这个函数旨在用于交互式界面)如果没有提供参数，则交互式的帮助系统会在解释器终端上启动。如果参数是个字符串，则把这个字符串当作模块、函数、类、方法、关键字的名称，或者文档的主题，并在终端上打印其帮助页面。如果参数是任何其它类型的对象，则生成该对象的帮助页面。

   这个函数通过 :mod:`site` 模块加入到内置的命名空间。


.. function:: hex(x)

   把一个整数转化成十六进制字符串，结果是个有效的 Python 表达式。如果 *x* 不是 Python :class:`int` 对象，则必须定义 :meth:`__index__` 方法并返回一个整数。

   .. note::

      要获得一个浮点数的十六进制形式，需使用 :meth:`float.hex` 方法。


.. function:: id(object)

   返回一个对象 object 的"身份标志"。这是个整数，它能确保是唯一的，并且在 object 的生命周期中保持不变。两个对象如果生命周期没有重合，则可能有相同的 :func:`id` 值。

   .. impl-detail:: 这是 object 在内存中的地址。


.. function:: input([prompt])

   如果提供了 *prompt* 参数，则把它写入到标准输出，后面不加换行符。这个函数然后会从标准输入读取一行内容，把它转化成字符串(去掉结尾的换行符)，并返回其结果。如果读取到 EOF ，则抛出 :exc:`EOFError` 。例如::

      >>> s = input('--> ')  # doctest: +SKIP
      --> Monty Python's Flying Circus
      >>> s  # doctest: +SKIP
      "Monty Python's Flying Circus"

   如果加载了 :mod:`readline` 模块，则 :func:`input` 就会用它来提供更便利的行编辑和历史功能。


.. function:: int(x=0)
              int(x, base=10)

   把作为数字或字符串的 *x* 转化成整数，如果没有参数则返回 ``0`` 。如果 *x* 的个整数，则返回 :meth:`x.__int__() <object.__int__>` 。对于浮点数，则在去掉尾数时向零靠近。

   如果 *x* 不是整数，或者指定了 *base* ，则 *x* 必须是表示 *base* 进制的\ :ref:`整数源常量 <integers>`\ 字符串、\ :class:`bytes` 、或者 :class:`bytearray` 实例。这个源常量前面有可选的 ``+`` 或 ``-`` (之间没有空格)，周围可以有空格。n 进制源常量由数字 0 到 n-1 组成，并且用 ``a`` 到 ``z`` (或者 ``A`` 到 ``Z``)表示 10 到 35。\ *base* 的默认值是 10 ，其允许的范围是 0 和 2-36 。2/8/16 进制源常量可以分别带有可选的 ``0b``/``0B`` 、``0o``/``0O`` 或 ``0x``/``0X`` 前缀，和代码中的整形源常量一样。base 为 0 表示把 x 当作源常量本身而不转化，所以其基数实际是 2 、8 、10 、或 16，这样 ``int('010', 0)`` 就是非法的，而 ``int('010')`` 以及 ``int('010', 8)`` 却是合法的。

   整数类型在\ :ref:`typesnumeric`\ 中介绍。


.. function:: isinstance(object, classinfo)

   如果 *object* 参数是 *classinfo* 参数或其(直接的、间接的、或者\ :term:`虚的 <虚基类>`)子类的一个实例则返回真。如果 *object* 不是指定的类型则总是返回假。如果 *classinfo* 不是一个类(type 对象)，则可以是 type 对象的元组，或者递归的包含这样的元组(不可以是其它序列类型)。如果 *classinfo* 不是一个类型，也不是类型元组或由类型组成的元组，则抛出 :exc:`TypeError` 异常。


.. function:: issubclass(class, classinfo)

   如果 *class* 是 *classinfo* 的(直接的、间接的、或者\ :term:`虚的 <虚基类>`)子类则返回真。一个类也是其自身的子类。\ *classinfo* 可以是类对象的元组，这时会检测 *classinfo* 中的每个元素。在任何其它情况下都会抛出 :exc:`TypeError` 异常。


.. function:: iter(object[, sentinel])

   返回一个\ :term:`迭代器`\ 对象。对第一个参数的解释会根据是否有第二个参数而区别很大。如果没有第二个参数，则 *object* 必须是个支持迭代协议(:meth:`__iter__` 方法)的集合对象，或者支持序列协议(:meth:`__getitem__` 方法，其参数为整数，从 ``0`` 开始)。如果它不支持这两种协议，则抛出 :exc:`TypeError` 。如果指定了第二个参数 *sentinel* 则 *object* 必须可调用。这种情况下创建的迭代器，每当调用其 :meth:`~iterator.__next__` 方法时都会不带参数的调用 *object* ；如果返回的值和 *sentinel* 相等，则抛出 :exc:`StopIteration` ，否则才正常返回这个值。

   另见\ :ref:`typeiter`\ 。

   第二种 :func:`iter` 形式的一个用处就是读取一个文件中和行，直到遇到特定的一行。下面的例子读取一个文件，直到 :meth:`readline` 方法遇到一个空字符串::

      with open('mydata.txt') as fp:
          for line in iter(fp.readline, ''):
              process_line(line)


.. function:: len(s)

   返回对象的长度(其中元素的个数)。其参数可以是序列(字符串、元组、列表)或映射(字典)。


.. _func-list:
.. function:: list([iterable])
   :noindex:

   :class:`list` 其实不仅是个函数，它更是个可变的序列类型，在\ :ref:`typesseq-list`\ 和\ :ref:`typesseq`\ 中介绍。


.. function:: locals()

   更新并返回一个字典，代表当前局部符号表。如果在函数块体调用 :func:`locals` 则会在返回中包含自由变量，而在类体中调用却不会。

   .. note::
      不应该修改这个字典的内容。如果修改，也可能不会影响解释器中局部和自由变量的值。

.. function:: map(function, iterable, ...)

   返回一个迭代器，依次都对 *iterable* 的每个元素调用 *function* ，从而产生返回结果。如果还有更多的 *iterable* ，则 *function* 必须接受这么多参数，然后从每个 iterable 中并行取出元素并调用 function 。对于多个 iterable 的情形，在最短的那个穷尽时迭代器就会停止。对于只接受元组参数的情况，参见 :func:`itertools.starmap`\ 。


.. function:: max(iterable, *[, key])
              max(arg1, arg2, *args[, key])

   返回 iterable 中最大的项，或者两个或更多参数中最大的那个。

   如果有一个位置参数，则 *iterable* 必须是非空的可迭代对象(例如非空字符串、元组、或列表)；这时返回 iterable 中最大的项。如果有两个或更多参数，则返回位置参数中最大的那个。

   可选的唯关键字参数 *key* 指定一个只接受单个参数的排序函数，就像 :meth:`list.sort` 中使用的那样。

   如果最大的项有多个，这个函数返回最先找到的那个。这和其它具有排序稳定性的工具是一致的，例如 ``sorted(iterable, key=keyfunc, reverse=True)[0]`` 和 ``heapq.nlargest(1, iterable, key=keyfunc)`` 。


.. _func-memoryview:
.. function:: memoryview(obj)
   :noindex:

   根据指定的参数创建并返回一个"内存视图"对象。详情参见\ :ref:`typememoryview`\ 。


.. function:: min(iterable, *[, key])
              min(arg1, arg2, *args[, key])

   返回 iterable 中最小的项，或者两个或更多参数中最小的那个。

   如果有一个位置参数，则 *iterable* 必须是非空的可迭代对象(例如非空字符串、元组、或列表)；这时返回 iterable 中最小的项。如果有两个或更多参数，则返回位置参数中最小的那个。

   可选的唯关键字参数 *key* 指定一个只接受单个参数的排序函数，就像 :meth:`list.sort` 中使用的那样。

   如果最小的项有多个，这个函数返回最先找到的那个。这和其它具有排序稳定性的工具是一致的，例如 ``sorted(iterable, key=keyfunc, reverse=True)[0]`` 和 ``heapq.nlargest(1, iterable, key=keyfunc)`` 。

.. function:: next(iterator[, default])

   通过调用 *iteror* 的 :meth:`~iterator.__next__` 方法返回其中的下一个项。当 iterator 穷尽时，如果指定了 *default* 就返回这个值，否则就抛出 :exc:`StopIteration` 。


.. function:: object()

   返回一个普通的对象。\ :class:`object` 是所有类的基类，它定义了所有 Python 类实例所共有的方法。这个函数不接受任何参数。

   .. note::

      :class:`object` *没有* :attr:`__dict__` ，所以你不能对 :class:`object` 类实例任意赋以属性。


.. function:: oct(x)

   把一个整数转化为八进制字符串，结果是个有效的 Python 表达式。如果 *x* 不是个 Python :class:`int` 对象，则必须定义 :meth:`__index__` 方法并返回一个整数。


   .. index::
      single: 文件对象; 内置函数 open()

.. function:: open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

   打开 *file* 并返回相应的\ :term:`文件对象`\ 。如果这个文件不能打开，则抛出 :exc:`OSError` 。

   *file* 是个字符串或者 bytes 对象，它指定要打开文件的路径(绝对路径或者基于当前工作目录的相对路径)，或者指定要封装的文件描述符。(如果指定的是文件描述符，则在返回的 I/O 对象关闭时文件描述符也会关闭，除非把 *closefd* 设为 ``False`` 。)

   *mode* 是个可选的字符串，指定打开文件所使用的模式；其默认值为 ``'r'`` ，表示以文本读取的方式打开。其它常见模式有表示写入(如果文件存在则先清空)的 ``'w'`` ，表示单单创建的 ``'x'`` ，以及表示添加内容(在\ *有的*\ Unix 系统，表示\ *所有*\ 的写入都会添加到文件的结尾，而不管文件指针的位置)的 ``'a'`` 。在文本模式下，如果没有指定 *encoding* 则使用的编码方式依赖于系统：通过调用 ``locale.getpreferredencoding(False)`` 来得到当前的系统编码方式。(如果要读写二进制字节，则要使用二进制模式，并且不要指定 *encoding* 。)可用的模式如下：

   ========= ===============================================================
   字符      含义
   --------- ---------------------------------------------------------------
   ``'r'``   以读取方式打开(默认的)
   ``'w'``   以写入方式打开，会首先清空文件
   ``'x'``   单单以创建模式打开，如果文件已经存在则打开失败
   ``'a'``   以写入模式打开，如果文件已存在则在文件末尾添加
   ``'b'``   二进制模式
   ``'t'``   文件模式(默认的)
   ``'+'``   打开并更新一个磁盘文件(读取或写入)
   ``'U'``   万能换行符模式(为了向后兼容；新代码中不应使用)
   ========= ===============================================================

   默认的模式是 ``'r'`` (以读取文本方式打开，和 ``'rt'`` 同义)。对于二进制文件的读写，模式 ``'w+b'`` 会打开文件并把它清空成 0 字节；\ ``'r+b'`` 会打开文件但不清空。

   正如\ :ref:`io-overview`\ 提到的那样，Python 的 I/O 区分二进制文件和文本文件。以二进制方式(在 *mode* 参数中含有 ``'b'``)打开的文件会以 :class:`bytes` 对象的形式返回内容，而不进行任何解码。在文本模式(这是默认的，或者 *mode* 参数包含 ``'t'`` 时)则以 :class:`str` 的形式返回文件内容，这些内容使用依赖系统的编码方式进行解码，如果指定 *encoding* 则用它来解码。

   .. note::

      Python 并不依赖底层的操作系统来决定文件是否是文本文件，整个过程都由 Python 自己完成，所有和系统是无关的。

   *buffering* 是个可选的整数，用以指定缓冲区策略。传入 0 表示要关闭缓冲区(只允许用于二进制模式)，1 表示选择基于行的缓冲区(只用于文本模式)，大于 1 的整数表示一个固定缓冲区的大小。如果没有指定 *buffering* 参数，默认的缓冲区策略如下：

   * 二进制文件有大小固定的缓冲区，其缓冲区大小根据其底层设备的“块大小”来决定，如果不行则用 :attr:`io.DEFAULT_BUFFER_SIZE` 。在很多系统中这个缓冲区大小都是 4096 或 8192 字节。

   * "交互的"文本文件(调用 :meth:`isatty` 返回真的那些文件)使用行缓冲区。其它的文件文件使用上面的二进制文件策略。

   *encoding* 是对文件编码或解码所用的编码名称，这个只用于文本模式。默认的编码和系统相关(任意由 :func:`locale.getpreferredencoding` 返回的值)，但可以使用任意 Python 支持的编码。这个支持的编码列表参见\ :mod:`codecs`\ 。

   *errors* 是个可选的字符串，指定如何处理编码或解码错误 --- 这不能用于二进制模式。传入 ``'strict'`` 会在发生编码错误时抛出 :exc:`ValueError` 异常(默认值 ``None`` 有相同的效果)，而传入 ``'ignore'`` 会忽略错误(注意，忽略编码错误可能导致数据丢失)。传入 ``'replace'`` 会在遇到格式错误的数据时插入一个替换的标识(例如 ``'?'``)，而在写入时可以使用 ``'xmlcharrefreplace'`` (替换成适当的 XML 字符引用)或者 ``'backslashreplace'`` (替换成反斜线转义序列)。任何通过 :func:`codecs.register_error` 注册的错误处理名称出都是有效的。

   .. index::
      single: 万能换行符; 内置函数 open()

   *newline* 控制\ :term:`万能换行符`\ 模式如何工作(只对文本模式有效)，其值可以是 ``None`` 、\ ``''`` 、\ ``'\n'`` 、\ ``'\r'`` 、或 ``'\r\n'`` 。它的作用方式如下：

   * 在从流中读取输入时，如果 *newline* 是 ``None`` ，则启用万能换行符模式。输入中的每一行都可以用 ``'\n'`` 、\ ``'\r'`` 、或者 ``'\r\n'`` 结束，它们会在返回到调用那里之前被转化为 ``'\n'`` 。如果是 ``''`` ，则会打开万能换行模式，但是行结束符会不加转换的返回给调用者。如果它是其它任何合法的值，则输入行只能以指定的字符串结束，该结束符会不回转换的返回给调用者。

   * 在向流中写入输出时，如果 *newline* 是 ``None`` ，任何写入的 ``'\n'`` 字符都会转换成系统默认的行分隔符 :data:`os.linesep` 。如果 *newline* 是 ``''`` 或者 ``'\n'`` ，则不进行转换。如果 *newline* 是任何其它合法的值，任何写入的 ``'\n'`` 字符都会转换成这个指定的字符串。

   如果 *closefd* 是 ``False`` ，并且指定了一个文件描述符而不是文件名，则在关闭该文件时保持打开这个文件描述符。如果指定了文件名，则 *closefd* 不起作用，且必须为 ``True`` (默认值)。

   可以通过传入一个可调用的 *opener* 来打开文件。这时可以通过加上 (*file*, *flags*) 来调用 *opener* 以得到文件对象所对应的文件描述符。\ *opener* 必须返回一个打开的文件描述符(把 :mod:`os.open` 作为 *opener* 传入，其作用和传入 ``None`` 相似)。

   下面的例子使用 :func:`os.open` 函数的 :ref:`dir_fd <dir_fd>` 参数来打开一个相对于指定目录的文件::

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
      增加 *opener* 参数。增加 ``'x'`` 模式。

   :func:`open` 函数所返回的\ :term:`文件对象`\ 的类型取决于打开的模式。如果用 :func:`open` 以文本模式打开文件(``'w'`` 、\ ``'r'`` 、\ ``'wt'`` 、\ ``'rt'`` 等等)，则返回 :class:`io.TextIOBase` 的子类(尤其是 :class:`io.TextIOWrapper`)。当以带缓冲区的二进制模式打开文件时，返回的类是 :class:`io.BufferedIOBase` 的子类。具体的类各不相同：在二进制读取模式中，返回 :class:`io.BufferedReader` ；在二进制写入和添加模式中，返回 :class:`io.BufferedWriter` ；在读写模式中，返回 :class:`io.BufferedRandom` ；如果禁用缓冲区，则返回原生流，它是 :class:`io.RawIOBase` 和 :class:`io.FileIO` 的子类。

   .. index::
      single: 行缓冲区 I/O
      single: 无缓冲区 I/O
      single: 缓冲区大小, I/O
      single: I/O 控制; 缓冲区
      single: 二进制模式
      single: 文本模式
      module: sys

   另见文件处理模式，例如 :mod:`fileinput` ，\ :mod:`io` (这是定义 :func:`open` 的地方)，\ :mod:`os` 、\ :mod:`os.path` 、\ :mod:`tempfile` 、和 :mod:`shutil` 。

   .. versionchanged:: 3.3
      以前会抛出 :exc:`IOError` ，它现在是 :exc:`OSError` 的别名。
      现在，如果文件单单以创建模式(``'x'``)打开并且已经存在，则会抛出 :exc:`FileExistsError` 。


.. XXX works for bytes too, but should it?
.. function:: ord(c)

   对于一个字符串表示的 Unicode 字符，返回该 Unicode 的整数编号。例如，\ ``ord('a')`` 返回整数 ``97`` 而 ``ord('\u2020')`` 返回 ``8224`` 。它和 :func:`chr` 不好相反。


.. function:: pow(x, y[, z])

   返回 *x* 的 *y* 次方。如果有 *z* ，则返回 *x* 的 *y* 次方与 *z* 整除的结果(计算时比 ``pow(x, y) % z`` 效率更高)。两个参数的形式 ``pow(x, y)`` 和使用乘方运算符 ``x**y`` 是等价的。

   参数必须是数值类型。如果操作数类型不同，则会应用二元运算符的精度转换规则。对于 :class:`int` 操作数，结果(精度提升后)和操作数的类型相同，除非第二个参数是负数；如果第二个参数是负数，则所有参数都转化为浮点数，并返回浮点数结果。例如，\ ``10**2`` 返回 ``100`` 而 ``10**-2`` 返回 ``0.01`` 。如果第二个参数是负数，则必须省略第三个参数。如果有 *z* ，则 *x* 和 *y* 必须是整数类型，并且 *y* 必须不能为负数。


.. function:: print(*objects, sep=' ', end='\\n', file=sys.stdout, flush=False)

   把 *objects* 输出到流 *file* ，中间用 *sep* 分隔，后面加上 *end* 。如果有 *sep* 、\ *end* 、\ *file* ，则必须是关键字参数。

   所有非关键字参数都会转化成字符串，像 :func:`str` 那样，并且输出到流中，中间用 *sep* 分隔，后面加上 *end* 。\ *sep* 和 *end* 必须都是字符串，或者 ``None`` ，表示使用默认值。如果没有指定 *objects* ，则 :func:`print` 会只输出 *end* 。

   *file* 参数必须是个支持 ``write(string)`` 方法的对象。如果没有指定或者为 ``None`` ，则使用 :data:`sys.stdout` 。输出是否有缓冲区通常是由 *file* 决定，但如果关键字参数 *flush* 为真，则流会强制刷新。

   .. versionchanged:: 3.3
      增加关键字参数 *flush* 。


.. function:: property(fget=None, fset=None, fdel=None, doc=None)

   返回一个 property 属性。

   *fget* 是个可以获取某个属性的函数。类似的 *fset* 是个设置函数，而 *fdel* 是个使用 del 删除属性的函数。它的典型的应用是定义一个可控的属性 ``x``::

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

   如果这时 *c* 是 *C* 的一个实例，则 ``c.x`` 会调用 getter ，\ ``c.x = value`` 会调用 setter ，而 ``del c.x`` 会调用 deleter 。

   如果指定 *doc* ，它就会用作该 property 属性的文档字符串。否则，这个 property 将会复制 *fget* 的文档字符串(如果有的话)。这就让创建只读的 property 容易通过使用 :func:`property` 作为\ :term:`修饰符`\ 来实现::

      class Parrot:
          def __init__(self):
              self._voltage = 100000

          @property
          def voltage(self):
              """Get the current voltage."""
              return self._voltage

   这就把 :meth:`voltage` 方法变成一个与之同名的只读属性的"getter"。

   一个 property 对象有 :attr:`getter` 、\ :attr:`setter` 、\ :attr:`deleter` 方法，可以用作修饰符；它把相应的访问函数设置成被修饰的函数，从而返回该 property 的副本。这点最好用例子来说明::

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

   这个例子和第一个例子是完全等价的。注意，增加的函数要和原来的 property 有相同的名字(这里是 ``x``)。

   返回的 property 也有 ``fget`` 、\ ``fset`` 、和 ``fdel`` 属性，和其构造函数的参数对应。


.. _func-range:
.. function:: range(stop)
              range(start, stop[, step])
   :noindex:

   :class:`range` 不仅是个函数，还是一个不可变的序列类型，在\ :ref:`typesseq-range`\ 和\ :ref:`typesseq`\ 中介绍。


.. function:: repr(object)

   返回一个字符串，其中包含 object 的可打印形式。对很多类型，这个函数会试图返回一个与对它进行 :func:`eval` 结果一样的字符串；不然这就是一个由尖括号包围的字符串，中间是 object 的类型名称以及额外的信息，通常这这些信息是 object 的名称和地址。一个类可以定义 :meth:`__repr__` 方法来控制这个函数的返回值。


.. function:: reversed(seq)

   返回一个置反的\ :term:`迭代器`\ 。\ *seq* 必须是带有 :meth:`__reversed__` 方法的对象，或者支持序列协议(即 :meth:`__len__` 方法和 :meth:`__getitem__` 方法，其参数为整数且从 ``0`` 开始)。


.. function:: round(number[, ndigits])

   返回浮点数，它是 *number* 保留小数点后 *ndigits* 个数字的进位后得到的。如果省略 *ndigits* ，则默认其为零。实际调用 ``number.__round__(ndigits)``\ 。

   对于支持 :func:`round` 的内置类型，这个值是最接近于 10 的负 *ndigits* 次方的整数倍；如果有两个倍数一栏相近，则进位时取偶数(所以 ``round(0.5)`` 和 ``round(-0.5)`` 都是 ``0``\ ，而 ``round(1.5)`` 是 ``2``)。如果调用时只有一个参数，则返回整数，否则返回和 *number* 类型相同的值。

   .. note::

      对浮点数，\ :func:`round` 的行为可能让人惊讶。例如，\ ``round(2.675, 2)`` 是结果是 ``2.67`` 而不是期待的 ``2.68``\ 。这不是 bug ，而是由于大部分十进制小数都不能准确的用浮点数来表示。更多信息参见\ :ref:`tut-fp-issues`\ 。


.. _func-set:
.. function:: set([iterable])
   :noindex:

   返回一个新的 :class:`set` 对象，其中的元素是可选的并且来自 *iterable*\ 。\ ``set`` 是个内置的类。其文档参见 :class:`set` 和\ :ref:`types-set`\ 。

   关于其它容器参见内置的 :class:`frozenset` 、\ :class:`list` 、\ :class:`tuple` 、和 :class:`dict` 类，以及 :mod:`collections` 模块。


.. function:: setattr(object, name, value)

   它和 :func:`getattr` 对应。其参数是一个对象 object ，一个字符串 name 和一个任意值 value 。这个字符串可以指定一个已有的或者新的属性名称。如果允许，整个函数会把 value 给 name 赋值。例如，\ ``setattr(x, 'foobar', 123)`` 相当于 ``x.foobar = 123`` 。


.. function:: slice(stop)
              slice(start, stop[, step])

   .. index:: single: Numerical Python

   返回一个\ :term:`切片`\ 对象，表示由 ``range(start, stop, step)`` 下标指定的元素。\ *start* 和 *step* 参数默认为 ``None`` 。切片对象有只读的属性 :attr:`start` 、\ :attr:`stop` 、和 :attr:`step` ，它们只返回其参数值(或者默认值)。这个函数没有其它明确的功能，但却用于 Numerical Python 和其它第三方扩展中。如果使用扩展的下标语法也会生成切片对象，例如：``a[start:stop:step]`` 或者 ``a[start:stop, i]`` 。参见其另外一个实现 :func:`itertools.islice` ，它返回一个迭代器。
   for an alternate version that returns an iterator.


.. function:: sorted(iterable[, key][, reverse])

   从 *iterable* 的元素中返回一个新的已排序的列表。它有两个可选的参数，必须以关键字参数的形式指定。

   *key* 指定只有单个参数的函数作为参数，这个函数用来从每个列表元素中取出用于比较的键，例如 ``key=str.lower`` ，它的默认值是 ``None`` (这时直接比较元素)。

   *reverse* 是个布尔值。如果为 ``True`` ，则每个元素在排序时好像其比较结果都置反了。

   可以用 :func:`functools.cmp_to_key` 把旧式的 *cmp* 函数转化为 *key* 函数。

   关于排序的例子及其简单介绍，参见\ `怎么排序 <http://wiki.python.org/moin/HowTo/Sorting/>`_\ 。

.. function:: staticmethod(function)

   为 *function* 返回一个静态方法。

   静态方法没有隐含的第一个参数。要声明静态方法，可以使用这样的成规::

      class C:
          @staticmethod
          def f(arg1, arg2, ...): ...

   ``@staticmethod`` 形式是个函数\ :term:`修饰符` --- 详情参见\ :ref:`function`\ 中的函数定义。

   静态方法可以用类调用(例如 ``C.f()``)，也可以用实例调用(例如 ``C().f()``)。调用时忽略实例，只用其类信息。

   Python 中的静态方法和 Java 或 C++ 中的类似。另见 :func:`classmethod` ，这个方法用于创建另外一种类构造函数。

   关于静态方法的更多信息，参阅标准类型体系\ :ref:`types`\ 的文档。

   .. index::
      single: string; str() (内置函数)


.. _func-str:
.. function:: str(object='')
              str(object=b'', encoding='utf-8', errors='strict')
   :noindex:

   返回 *object* 的 :class:`str` 形式。详情参见 :func:`str` 。

   ``str`` 是内置的字符串\ :term:`类`\ 。关于字符串的一般信息，参见\ :ref:`textseq`\ 。


.. function:: sum(iterable[, start])

   把 *iterable* 中的元素从 *start* 开始从左向右加起来，然后返回其和。\ *start* 默认为 ``0`` 。\ *iterable* 中的项通常都是数值，而 start 的值不能是字符串。

   有些情况下，还有比 :func:`sum` 更好的方法。连接字符串更好也更快的方法是调用 ``''.join(sequence)`` 。把浮点数相加并保留扩展的精度，可参见 :func:`math.fsum`\ 。如果要连接一系列可迭代对象，可以考虑使用 :func:`itertools.chain` 。

.. function:: super([type[, object-or-type]])

   返回一个代理对象，能把方法调用转化为对 *type* 的父类或兄弟类的调用。这对于访问类中那些被继承并重载的方法来说很有用。它的搜索顺序和 :func:`getattr` 使用的顺序一样，只不过会忽略 *type* 本身。

   *type* 的 :attr:`__mro__` 属性列出 :func:`getattr` 和 :func:`super` 所使用的方法解析顺序。这个属性是动态的，每当继承体系更新时这个属性也随之更新。

   如果忽略第二个参数，则返回的 super 对象就是没有绑定的。如果第二个参数是个对象，则 ``isinstance(obj, type)`` 必须为真。如果第二个参数是个类型，则 ``issubclass(type2, type)`` 必须为真(这对类方法来说很有用)。

   *super* 有两个典型的应用场景。在单继承的类体系中，\ *super* 可以用来指代父类而不需要明确指定名称，从而让代码更易于维护。这个用法和其它编程语言中的 *super* 最接近。

   第二种应用场景是在动态执行环境中支持多继承协作。这个用法是 Python 特有的，而静态编译地语言和只支持单继承的语言中是没有的。这就让多个基类实现同一方法的"棱形模式"易于实现。良好的设计保证这个方法在每次调用时都使用同样的调用界面(因为调用的顺序在运行时决定，也因为这个顺序会随着类体系一起更新，还因为这个顺序中可能包含在运行之前无法得知的兄弟类)。

   对于这两种应用场景，对 super 类的调用都像下面的典型例子::

      class C(B):
          def method(self, arg):
              super().method(arg)    # 它的作用和下面一样：
                                     # super(C, self).method(arg)

   注意，\ :func:`super` 的实现是绑定过程的一部分，这样才便于带点属性的查找，例如 ``super().__getitem__(name)`` 。它的做法是实现自己的 :meth:`__getattribute__` 方法用来按照可预见的顺序查找类，这个查找方法支持多继承协作。相应的，对于使用语句或运算符的隐式查找，例如 ``super()[name]`` ，\ :func:`super`\ 是末定义的。

   还要注意，除了零参数的形式，\ :func:`super` 在方法内部的使用是不受限制的。两个参数的形式明确指定了参数，并进行适当的引用。零参数的形式只能在类定义中使用，因为编译器会提供必须的细节来得到当前定义的类，以及访问当前实例的普通方法。

   对于如何用 :func:`super` 设计多类协作的实用建议，参见 super() 使用指南 <http://rhettinger.wordpress.com/2011/05/26/super-considered-super/>`_ 。


.. _func-tuple:
.. function:: tuple([iterable])
   :noindex:

   它不仅是函数，还是 :class:`tuple` 一个不可变的序列类型，在\ :ref:`typesseq-tuple`\ 和\ :ref:`typesseq`\ 介绍。


.. function:: type(object)
              type(name, bases, dict)

   .. index:: object: type


   对于单参数形式，则返回 *object* 的类型。返回的值是个类型对象，它通常和 ``object.__class__`` 的返回值是同样的对象。

   推荐使用内置函数 :func:`isinstance` 来检测一个对象的类型，因为它还会考虑子类。

   对于三个参数的形式，则返回一个新的 type 对象。这在本质上说是 :keyword:`class` 语句的动态形式。\ *name* 字符串是类的名字，并成为该类的 :attr:`__name__` 属性；\ *bases* 元组列出基类，并成为 :attr:`__bases__` 属性；而 *dict* 字典指定定义类体的命名空间并成为 :attr:`__dict__` 属性。例如，下面两个语句创建了完全一样的 :class:`type` 对象::

      >>> class X:
      ...     a = 1
      ...
      >>> X = type('X', (object,), dict(a=1))

   另见\ :ref:`bltin-type-objects`\ 。


.. function:: vars([object])

   没有参数时作用和 :func:`locals` 相似。

   如果带有模块、类、类实例对象作为参数(或者任何其它有 :attr:`__dict__` 属性的对象)，则返回其 :attr:`__dict__` 属性。

   .. note::
      不应该修改返回的字典，因为修改对这个符号个的作用是未定义的。 [#]_

.. function:: zip(*iterables)

   创建一个迭代器，它能够聚合每个 iterable 的元素。

   返回一个元组迭代器，其中的第 *i* 个元素是由每个序列或可迭代参数的第 *i* 个元素组成的元组。当最短的输入迭代器穷尽时，返回的迭代器也会结束。如果只有一个可迭代参数，则返回一个单元素元组的迭代器。如果没有参数，则返回一个空的迭代器。它相当于::

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

   它保证从左到右到每个 iterable 进行求值。这就可以把一系列数据转化为长度为 n 的分组：\ ``zip(*[iter(s)]*n)`` ；这种用法也是 Python 的习惯。

   如果你不关心结尾部分长的 iterable 没有匹配的内容，这时才应该使用 :func:`zip` 来处理长度不等的参数。如果那些多余的值很重要，则应该使用 :func:`itertools.zip_longest` 。

   :func:`zip` 配合 ``*`` 运算符可以对列表进行分解::

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

      这是一个高级的函数，它不像 :func:`importlib.import_module` 那样需要在日常的 Python 编程中使用。

   这个函数会由 :keyword:`import` 语句启用。可以(通过导入 :mod:`builtins` 模块并给 ``builtins.__import__`` 赋值)改变这个函数从而改变 :keyword:`import` 语句的主义，但现在使用导入钩子通常会更容易(参见 :pep:`302`)完成同样的功能。直接使用 :func:`__import__` 是绝对不推荐的，而应该使用 :func:`importlib.import_module` 。

   这个函数导入模块 *name* ，这时可能使用指定的 *globals* 和 *locals* 来决定怎么在包上下文中解析名字。\ *fromlist* 指定需要从 *name* 中导入的对象或模块的名称。标准的实现根本不使用 *locals* 参数，而只用 *globals* 来决定 :keyword:`import` 语句的包上下文。

   *level* 指定是否用相对或绝对导入。\ ``0`` (默认值)表示进行绝对导入。\ *level* 为正数值表示在相对于调用 :func:`__import__` 的模块，要上溯的父目录级数(详情参见 :pep:`328`)。

   如果 *name* 变量的格式形如 ``package.module`` ，则通常只返回顶级包(第一个点号之前的名字)，而\ *不是*\ 由 *name* 指定的完整名字。但是如果指定了一个非空的 *fromlist* 参数，则返回 *name* 指定的模块。

   例如，语句 ``import spam`` 产生的字节友和下面的代码类似::

      spam = __import__('spam', globals(), locals(), [], 0)

   而语句 ``import spam.ham`` 产生下面的调用::

      spam = __import__('spam.ham', globals(), locals(), [], 0)

   注意 :func:`__import__` 是如何返回顶级模块的，因为 :keyword:`import` 语句把顶级模块名绑定到了一个名字。

   另一方面，语句 ``from spam.ham import eggs, sausage as saus`` 产生::

      _temp = __import__('spam.ham', globals(), locals(), ['eggs', 'sausage'], 0)
      eggs = _temp.eggs
      saus = _temp.sausage

   这里，\ :func:`__import__` 返回 ``spam.ham`` 模块。从这个返回的对象中，取出要导入的名字并分别赋值到各自的名下。

   如果你只想通过名字导入一个(可能在包中)模块，则应使用 :func:`importlib.import_module` 。

   .. versionchanged:: 3.3
      负数的 *level* 不在支持(并把默认值改为 0)。


.. rubric:: 脚注

.. [#] 注意，解析器只接受 Unix 风格的行结束符。如果从文件中读取代码，要确保使用换行符转换模式来处理 Windows 或 Mac 风格的换行符。

.. [#] 在当前实现中，这样做通常不会影响本地变量的绑定，但从其它作用域(例如模块)获得的变量可能受影响。这点可能会改变。
