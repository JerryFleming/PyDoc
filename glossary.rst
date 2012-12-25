.. _glossary:

********
术语
********

.. 如果你添加了新词条，请保持按字母顺序排列！

.. glossary::

   ``>>>``
      Python 中交互式 shell 的默认提示符。常见于代码示例中，表示代码能在解释器中交互执行。

   ``...``
      在交互式 shell 中输入缩进的代码，或者在输入成对的左右分隔符之间时，Python 默认使用的提示符。

   2to3
      一个能尽可能把 Python 2.x 的代码转换成 3.x 格式的脚本，能够通过分析源代码及其语法树来检测并处理大部分不兼容的地方。

      2to3 在标准库中的模块名是 :mod:`lib2to3`\ ，它在 :file:`Tools/scripts/2to3` 中还有个独立的入口。参见 :ref:`2to3-reference`\ 。

   虚基类
      虚基类(ABC)是\ :term:`鸭子类型`\ 的典型例子，它提供了一种定义接口的方法；而这时其它的技巧，例如 :func:`hasattr` 等(如用\ :ref:`魔术方法<special-lookup>`)，都显得笨拙或有点错误。虚基类引入虚子类的概念，即一个类并没有继承另一个类，但仍可以通过 :func:`isinstance` 和 :func:`issubclass` 来识别，详见 :mod:`abc` 模块的文档。Python 内置了很多虚基类以用于数据类型(在 :mod:`collections.abc` 模块中)、数值(在 :mod:`numbers` 模块中)、数据流(在 :mod:`io` 模块中)、导入时搜索和加载(在 :mod:`importlib.abc` 模块中)。你也可以用 :mod:`abc` 模块创建自己的虚基类。

   实参
      在调用\ :term:`函数`\ (或者\ :term:`方法`)时所传递的值。实参有两种类型：

      * :dfn:`命名实参`\ ：在函数调用中带有标志符(例如 ``name=``)的参数，或者字典中键值前带有 ``**`` 的传入参数。例如下面调用 :func:`complex` 时 ``3`` 和 ``5`` 都是命名实参::

           complex(real=3, imag=5)
           complex(**{'real': 3, 'imag': 5})

      * :dfn:`位置实参`\ ：除了命名实参以外的实参。位置实参应该在实参列表的开始；或者作为\ :term:`迭代`\ 的一个元素，这时它前面要带有 ``*``\ 。例如下面调用 :func:`complex` 时 ``3`` 和 ``5`` 都是位置实参::

           complex(3, 5)
           complex(*(3, 5))

      在函数体内，实参会赋值给本地定义的变量，关于赋值的具体规则，参见\ :ref:`调用`\ 一节。从语法上来说，任何表达式都可以用作实参，它的结果值将会传给本地变量。

      另请参见术语词条\ :term:`形参`\ ，或者\ :ref:`实参和形参的区别<faq-argument-vs-parameter>`\ 处的常见问题，以及 :pep:`362`\ 。

   属性
      和某个对象相关联的一个值，可以用点表达式来引用。例如，如果有个对象 *o* 具有属性 *a*\ ，可以用 *o.a* 来引用。

   BDFL
      生命中仁慈的主(Benevolent Dictator For Life)，又称作 `Guido van Rossum <http://www.python.org/~guido/>`_\ ，即 Python 的创始人。

   字节码
      Python 会被编译成字节码，这是 CPython 解释器对 Python 程序的内部表示形式。字节码还会缓存在 ``.pyc`` 和 ``.pyo`` 文件中，这样再次执行同样的文件就快多了(因为可以省略从源文件到字节码的编译过程)。这种"内部语言"在\ :term:`虚拟机`\ 上运行，并执行与字节码对应的机器码。要注意的是，把字节码移到其它 Python 虚拟机上，或者其它 Python 版本上面，不要指望它还能正常工作。

      关于字节码指令集，请参见 :ref:`dis 模块<bytecodes>`\ 的文档。

   类
      用来创建用户自定义对象的模板。类定义中通常包含方法的定义，用以操作类实例。

   隐式转换
      在要求两个运算数类型相同的运算中，一种类的实例转化为另外一种类的实例。例如，\ ``int(3.15)`` 把浮点数转化为整数 ``3``\ ，但是在 ``3+4.5`` 中，运算数的类型各不相同(一个 int，一个 float)，所以在它们相加之前都要转化为相同的类型，否则就会抛出 ``TypeError`` 异常来。如果没有隐匿转换，即使是类型兼容的运算数也必须由程序员来转换。例如要写成 ``float(3)+4.5`` 而不是简单的 ``3+4.5``\ 。

   复数
      这是您熟悉的实数系统的扩展，其中所有的数都表示为实部和虚部之和。虚数是虚数单位(``-1`` 的平方根，在数学中通常记为 ``i``\ ，而在工程领域则记为 ``j``)与实数的积。Python 内置了对算数的支持，采用了第二种书写形式，即虚部带有 ``j`` 后缀，例如 ``3+1j``\ 。和 :mod:`math` 模块相对应的算数模块是 :mod:`cmath`\ 。复数的使用是相当高等的数学中才会有的。如果你不知道哪里要用到它，就可以放心的忽略这些。

   上下文管理器
      在 :keyword:`with` 语句中用于管理环境的一个对象，它定义了 :meth:`__enter__` 和 :meth:`__exit__` 方法。参见 :pep:`343`\ 。

   CPython
      Python 编程语言的标准实现，发布在 `python.org <http://python.org>`_ 新闻组。"CPython" 这个词用来在必要的时候区别标准实现与其它实现，例如 Jython 或者 IronPython。

   修饰函数
      能够返回另外一个函数的函数，通常使用 ``@wrapper`` 的语法形式作为函数的转化器。常见的修饰函数有 :func:`classmethod` 和 :func:`staticmethod`\ 。

      修饰函数的语法只是一种语法简记形式。下面两种函数定义在语法上是等价的::

         def f(...):
             ...
         f = staticmethod(f)

         @staticmethod
         def f(...):
             ...

      同样的概念还见于类中，但是用得较少。详情参见文档\ :ref:`定义函数<function>`\ 和\ :ref:`定义类<class>`\ 。

   描述符
      一个定义了 :meth:`__get__`\ 、\ :meth:`__set__` 或者 :meth:`__delete__` 方法的对象。如果类属性是描述符，查找属性时就会触发特殊的绑定行为。通常，使用 *a.b* 的形式来获取、设置或删除一个属性时会在 *a* 的类字典中查找名字为 *b* 的对象，但如果 *b* 是个描述符，则会调用相应的描述符方法。理解描述符是深入理解 Python 的关键，因为它是很多功能的基础，例如函数、方法、属性、类方法、静态方法、以及对父类的引用。

      关于描述符的更多信息，参见\ :ref:`描述符`\ 。

   字典
      一种关联数组，其中任意的键映射到值。键可以是带有 :meth:`__hash__` 和 :meth:`__eq__` 方法的对象。在 Perl 中称为哈希表。

   文档字符串
      作为类、方法、或者模块中第一个表达式出现的源字符串。跃然在代码块执行时会被忽略，编译器却会利用它并把它放在其所对应的类、方法、或模块的 :attr:`__doc__` 属性中。因为它能够通过自省来访问，所以就成了保存该对象文档的标准地方。

   鸭子类型
      一种编程风格，即不是根据一个对象的类型来决定其是否拥有正确的接口，而是直接调用其方法或访问其属性（"如果它看起来像鸭子，并且也像鸭子那样嘎嘎叫，那么它肯定是一只鸭子。")通过强调接口而不是具体的类型，设计良好的代码会通过多态性替换而变得灵活。鸭子类型避免了使用 :func:`type` 或者 :func:`isinstance` 来检测类型，(不过要注意的是，鸭子类型可以通过\ :term:`虚基类<abstract base class>`)，而是通常使用 :func:`hasattr` 来检测，或者使用 :term:`EAFP` 编程方法。

   EAFP
      请求谅解比获得权限更容易。这是 Python 的编程风格，它假定某个键名或属性是有效的，如果假设错误则通过捕获异常来处理。这种简捷快速的风格的特征是带有很多 :keyword:`try` 和 :keyword:`except` 语句。这和很多其它语言中使用的 `LBYL` 风格相对，例如 C 中的那样。

   表达式
      一段可以求值的语法结构。换句话说，一个表达式是由一系列表达式元素组成的，例如源常量、名称、属性访问、运算符、或者有返回值的函数调用。和其它语言不同的是，并非所有的语法结构都是表达式。还有些\ :term:`语句`\ 不能作为表达式使用，例如 :keyword:`if`\ 。赋值语法也是语句，而不是表达式。

   扩展模块
      使用 C 或者 C++ 编写的模块，它们通过 Python 的 C API 来和核心及用户代码交互。

   文件对象
      对底层资源提供面向文件 API (诸如 :meth:`read()` 或者 :meth:`write()` 的方法) 的对象。根据创建方式的不同，文件对象可以间接访问真实的磁盘文件，或者存储或通讯设备(例如标准输入/输出，内存中的缓存区，套接口，管道等)。文件对象又称作\ :dfn:`类文件对象`\ 或者\ :dfn:`流`\ 。

      文件对象事实上有三种类型：原始二进制文件，缓存二进制文件，以及文本文件。它们的接口都定义在 :mod:`io` 模块。创建文件的标准版方法是使用 :func:`open` 函数。

   类文件对象
      :term:`文件对象`\ 的同义词。

   查找器
      能够尽可能搜索一个模块中 :term:`loader` 的对象。它必须实现一个叫 :meth:`find_loader` 或者 :meth:`find_module` 的方法。详情参见 :pep:`302` 以及 :pep:`420`\ ，或者 :class:`importlib.abc.Finder` 中的 :term:`abstract base class`\ 。

   下进位除法(整除)
      数学中的商向下进位到最近的整数。下进位除法的运算符是 ``//``\ 。例如，表达式 ``11 // 4`` 值为 ``2`` 而真正的浮点数除法的值是 ``2.75``\ 。注意 ``(-11) // 4`` 是 ``-3``\ ，因为要把 ``-2.75`` *向下*\ 进位。参见 :pep:`238`\ 。

   函数
      一系列语句的集合，并向调用者返回某个值。在执行函数时可以向其传递零个或多个参数。另参见\ :term:`实参`\ 和\ :term:`方法`\ 。

   __future__
      一个伪模块，编程者可以用它来启用新的语法功能，而这些功能和当前的解释器是不兼容的。

      通过导入 :mod:`__future__` 模块并使用其中的变量，就可以看到一个新的功能是什么时候加入，又是什么时候成为语言中的默认功能的::

         >>> import __future__
         >>> __future__.division
         _Feature((2, 2, 0, 'alpha', 2), (3, 0, 0, 'alpha', 0), 8192)

   垃圾清理
      在内存不在使用时就把它释放的过程。Python 通过引用记数以及一个周期性运行的垃圾清理程序来进行垃圾清理，这个清理程序能够检测和打破引用循环。

      .. index:: single: 生成函数

   生成函数
      返回迭代器的函数。这个函数看起来像普通函数，只不过它含有 :keyword:`yield` 语句，能够产生一系列值，可以在 for 循环中通过 :func:`next` 函数每次获取一个。每个 :keyword:`yield` 都会暂停执行，并记住上次执行时的位置和状态(包括本地变量和暂停的 try 语句)。当生成器恢复执行时，它会从上次停止的地方开始执行(而不是像普通函数那样每次调用时都会从头开始执行)。

      .. index:: single: 生成函数表达式

   生成函数表达式
      返回值为迭代器的表达式。它看起来像普通表达式，但是后面带有一个 :keyword:`for` 表达式用来定义循环变量和范围，还有一个可选的 :keyword:`if` 表达式。这种表达式联合起来会在被包含的函数中生成一系列值::

         >>> sum(i*i for i in range(10))  # 平方和 0, 1, 4, ... 81
         285

   GIL
      参见\ :term:`解释器全局锁`\ 。

   解释器全局锁
      :term:`CPython` 解释器所使用机制，用来确保同一时刻只有一个线程执行 Python :term:`字节码`\ 。这就简化了 CPython 的实现，因为它使得对象模型(包括重要的内置类型，例如 :class:`dict`)默认对并发访问就是安全的。把整个解释器锁定可以让它更容易的进行多线程工作，这样做的代价是多核处理器要应付更多的并行处理。

      但是有一些模块的设计，有核心的也有第三方的，在处理计算密集型的任务时，例如压缩或者哈希运算，是要释放 GIL 。并且，在进行 I/O 操作时也总是释放 GIL 。

      以前曾试图创建一个"自由的多线程"解释器(能够在更细的粒度锁定共享数据)，但并没有取得多大成功，因为在常见的单处理器上性能非常差。大家认为，要解决这个性能问题需要把实现做得更复杂，所以维护成本也更高。

   可散列对象
      在一个对象的生命周期中，如果它的散列值从来不会改变(要有一个 :meth:`__hash__` 方法)，还可以和其它对象比较(需要有 :meth:`__eq__` 方法)，则说这个对象是\ *可散列的*\ 。比较结果相等的可散列对象必须有相同的散列值。

      可散列的特性使一个对象能够用作字典的键名，以及集合的元素，因为这些数据结构内部使用散列值。

      Python 中所有不可改变的内部对象都是可散列的，而任何可改变的容器(例如列表或字典)都不是。用户自定义类的对象实例默认都是可散列的，它们比较结果都是不相等的；它们的散列值是其 :func:`id`\ 。

   IDLE
      Python 集成开发环境。IDLE 是 Python 发行版中自带的基本编辑器及解释执行环境。

   不可变对象
      其值固定不变的对象。不可变对象包括数值，字符串和元组，它们都不能更改。如果要存储不同的值，必须创建新的对象。它们在需要散列常量值的地方起着重要作用，例如作为字典的键名。

   导入路径
      :term:`基于路径的搜索器`\ 在导入模块时所搜索的位置列表(或者叫\ :term:`路径条目<path entry>`)。进行导入时，这个位置列表通常来自 :data:`sys.path`\ ，但对于子包也可以来自父包的 ``__path__`` 属性。

   导入
      Python 一个模块中的代码借此在另一个模块中使用的过程。

   导入器
      一个既能搜索又能加载模块的对象；它既是\ :term:`查找器`\ 又是\ :term:`加载器`\ 对象。

   交互式
      Python 有个交互式的解析器，这意味着你可以在解释器提示符下输入语句和表达式，让它们立即执行并看到运行结果。这只要运行 ``python`` 命令而不加参数(也有可能从你电脑的主菜单中选择)就可以了。这对于测试新的思路或检阅模块及包(记住要用 ``help(x)``)来说是非常强大的。

   解释型
      Python 是解释型语言而不是编译型的，尽管字节码编译器的存在，其区别已经很模糊。这意味着源文件可以直接运行而不需要先明确创建一个可执行程序然后再运行。解释型语言的开发/调试周期通常比编译型的短，尽管这些程序一般也运行得慢。参见\ :term:`交互式`\ 。

   可迭代对象
      可以每次返回其中一个元素的对象。可迭代对象的例子包括所有的序列类型(例如 :class:`list`\ 、\ :class:`str`\ 、\ :class:`tuple`)和一些非序列类型，如 :class:`dict` 和 :class:`file`\ ，还有你自己定义的类对象，只要它们有 :meth:`__iter__` 或者 :meth:`__getitem__` 方法。可迭代对象可以在 :keyword:`for` 循环语句和很多其它需要序列类型(:func:`zip`\ 、\ :func:`map`\ 、 ...)的地方使用。如果把一个可迭代对象作为形参传递给内部函数 :func:`iter`\ ，就会返回该对象的一个迭代器。这对于单次遍历一系列值而言是很好的。在使用可迭代对象时，通常都没有必要亲自调用 :func:`iter` 或者处理迭代器对象，\ ``for`` 语句会自动帮你完成，并在循环过程中创建一个临时变量来保存迭代器。参见 :term:`迭代器`\ 、\ :term:`序列` 和 :term:`生成函数`\ 。

   迭代器
      代表一个数据流的对象。连续调用迭代器的 :meth:`~iterator.__next__` 方法(或者连续把它传给内部函数 :func:`next`)会相继返回流中的数据项。如果没有数据可用，则抛出 :exc:`StopIteration` 异常。这时，迭代器对象已经穷尽，如果继续调用其 :meth:`__next__` 方法就会再次抛出 :exc:`StopIteration` 异常。迭代器必须要有 :meth:`__iter__` 方法，这个方法要返回迭代器本身，所以每个迭代器都是可迭代对象，在可以使用其它可迭代对象的大部分对方也可以用迭代器。这里有个异常值得关注，就是试图多次遍历数据的代码。每当把容器对象(例如 :class:`list`)传给 :func:`iter` 函数或者在 :keyword:`for` 循环中使用时都会产生一个新的迭代器。如果这样使用迭代器，则会返回同样已经上次遍历中穷尽的迭代器对象，这使得它像一个空的容器。

      更多信息参见\ :ref:`typeiter`\ 。

   关键字函数
      关键字函数，又叫整理函数，是一个返回值可以用来排序的函数。例如， :func:`locale.strxfrm` 被用来生成一个排序键值，这个值知道语言区域相关的惯例。

      Python 中有许多函数接受关键字函数来控制元素的排序或者分组，包括 :func:`min`\ ，\ :func:`max`\ ，\ :func:`sorted`\ ，\ :meth:`list.sort`\ ，\ :func:`heapq.nsmallest`\ ，\ :func:`heapq.nlargest`\ ，以及\ :func:`itertools.groupby`\ 。

      创建关键字函数的方法有好几种。例如，\ :meth:`str.lower` 方法可以作为不区分大小写排序的关键字函数。还可以用 :keyword:`lambda` 表达式临时创建一个关键字函数，例如 ``lambda r: (r[0], r[2])``\ 。此外，\ :mod:`operator` 模块提供了三个关键字函数构造方法：\ :func:`~operator.attrgetter`\ ，\ :func:`~operator.itemgetter` 和 :func:`~operator.methodcaller`\ 。关于如何创建和使用关键字函数，参见\ :ref:`怎么排序<sortinghowto>`\ 。

   命名实参
      参见\ :term:`实参`\ 。

   lambda
      匿名的内联函数，只包含一个\ :term:`表达式`\ ，在调用该函数时会对这个表达式进行求值。创建一个 lambda 函数的语法是 ``lambda [arguments]: expression``\ 。

   LBYL
      看看周围环境再跳跃。这种编程风格在调用方法或者查找属性前明确检测前提条件。它和 :term:`EAFP` 方法相对，其特征是有很多 :keyword:`if` 语句。

      在多线程环境中，LBYL 方法可能引发"查看者"和"跳跃者"之间的竞争状态。例如，在代码 ``if key in mapping: return mapping[key]`` 中，如果另一个线程在检测过后查找之前把 *key* 从 *mapping* 删除掉。这种情况可以通过锁或者 EAFP 方法解决。

   列表
      Python 内置的一种\ :term:`序列`\ 类型。尽管这个名字有其它含义，它和其它语言中的数组更相近，而不是链表，因为它访问元素的复杂度是 O(1)。

   列表解析
      处理序列中所有或者部分元素的一种紧凑形式，结果返回一个列表。\ ``result = ['{:#04x}'.format(x) for x in range(256) if x % 2 == 0]`` 生成一个字符串列表，包括了从 0 到 255 之间的十六进制偶数(0x..)。这里的 :keyword:`if` 子句是可选的；如果省略，则会处理 ``range(256)`` 中的所有元素。

   加载器
      能加载模块的对象，它必须定义 :meth:`load_module` 方法。加载器通常是由\ :term:`查找器`\ 返回的。详情参见 :pep:`302` 或者 :class:`importlib.abc.Loader` 中的\ :term:`虚基类`\ 。

   映射
      一个容器对象，支持用任意键名访问，并实现了 :class:`~collections.abc.Mapping` 或者 :class:`~collections.abc.MutableMapping` :ref:`虚基类<collections-abstract-base-classes>`\ 中指定的方法。例如 :class:`dict`\ ，\ :class:`collections.defaultdict`\ ，\ :class:`collections.OrderedDict` 和 :class:`collections.Counter`\ 。

   元路径查找器
      在 :data:`sys.meta_path` 找到的查找器，它和\ :term:`路径条目查找器<path entry finder>`\ 有关系，但又不一样。.

   元类
      类之类，即定义了类名、类字典、一系列基类的类。元类负责利用这三个参数创建一个类。许多面向对象的编程语言提供了默认的元类实现，而 Python 的特别之处在于它允许创建自定义元类。大部分用户都不需要这个东西，但在需要的时候它就能提供很强大和优雅的方案。它们已经被用来记录属性访问、增加线程安全性、跟踪对象创建、实现单例模式以及很多其它的任务。

      更多信息参见\ :ref:`metaclasses`\ 。

   方法
      A function which is defined inside a class body.  If called as an attribute
      of an instance of that class, the method will get the instance object as
      its first :term:`argument` (which is usually called ``self``).
      See :term:`function` and :term:`nested scope`.

   method resolution order
      Method Resolution Order is the order in which base classes are searched
      for a member during lookup. See `The Python 2.3 Method Resolution Order
      <http://www.python.org/download/releases/2.3/mro/>`_.

   module
      An object that serves as an organizational unit of Python code.  Modules
      have a namespace containing arbitrary Python objects.  Modules are loaded
      into Python by the process of :term:`importing`.

   MRO
      See :term:`method resolution order`.

   mutable
      Mutable objects can change their value but keep their :func:`id`.  See
      also :term:`immutable`.

   named tuple
      Any tuple-like class whose indexable elements are also accessible using
      named attributes (for example, :func:`time.localtime` returns a
      tuple-like object where the *year* is accessible either with an
      index such as ``t[0]`` or with a named attribute like ``t.tm_year``).

      A named tuple can be a built-in type such as :class:`time.struct_time`,
      or it can be created with a regular class definition.  A full featured
      named tuple can also be created with the factory function
      :func:`collections.namedtuple`.  The latter approach automatically
      provides extra features such as a self-documenting representation like
      ``Employee(name='jones', title='programmer')``.

   namespace
      The place where a variable is stored.  Namespaces are implemented as
      dictionaries.  There are the local, global and built-in namespaces as well
      as nested namespaces in objects (in methods).  Namespaces support
      modularity by preventing naming conflicts.  For instance, the functions
      :func:`builtins.open` and :func:`os.open` are distinguished by their
      namespaces.  Namespaces also aid readability and maintainability by making
      it clear which module implements a function.  For instance, writing
      :func:`random.seed` or :func:`itertools.islice` makes it clear that those
      functions are implemented by the :mod:`random` and :mod:`itertools`
      modules, respectively.

   namespace package
      A :pep:`420` :term:`package` which serves only as a container for
      subpackages.  Namespace packages may have no physical representation,
      and specifically are not like a :term:`regular package` because they
      have no ``__init__.py`` file.

   nested scope
      The ability to refer to a variable in an enclosing definition.  For
      instance, a function defined inside another function can refer to
      variables in the outer function.  Note that nested scopes by default work
      only for reference and not for assignment.  Local variables both read and
      write in the innermost scope.  Likewise, global variables read and write
      to the global namespace.  The :keyword:`nonlocal` allows writing to outer
      scopes.

   new-style class
      Old name for the flavor of classes now used for all class objects.  In
      earlier Python versions, only new-style classes could use Python's newer,
      versatile features like :attr:`__slots__`, descriptors, properties,
      :meth:`__getattribute__`, class methods, and static methods.

   object
      Any data with state (attributes or value) and defined behavior
      (methods).  Also the ultimate base class of any :term:`new-style
      class`.

   package
      A Python module which can contain submodules or recursively,
      subpackages.  Technically, a package is a Python module with an
      ``__path__`` attribute.

   parameter
      A named entity in a :term:`function` (or method) definition that
      specifies an :term:`argument` (or in some cases, arguments) that the
      function can accept.  There are five types of parameters:

      * :dfn:`positional-or-keyword`: specifies an argument that can be passed
        either :term:`positionally <argument>` or as a :term:`keyword argument
        <argument>`.  This is the default kind of parameter, for example *foo*
        and *bar* in the following::

           def func(foo, bar=None): ...

      * :dfn:`positional-only`: specifies an argument that can be supplied only
        by position.  Python has no syntax for defining positional-only
        parameters.  However, some built-in functions have positional-only
        parameters (e.g. :func:`abs`).

      * :dfn:`keyword-only`: specifies an argument that can be supplied only
        by keyword.  Keyword-only parameters can be defined by including a
        single var-positional parameter or bare ``*`` in the parameter list
        of the function definition before them, for example *kw_only1* and
        *kw_only2* in the following::

           def func(arg, *, kw_only1, kw_only2): ...

      * :dfn:`var-positional`: specifies that an arbitrary sequence of
        positional arguments can be provided (in addition to any positional
        arguments already accepted by other parameters).  Such a parameter can
        be defined by prepending the parameter name with ``*``, for example
        *args* in the following::

           def func(*args, **kwargs): ...

      * :dfn:`var-keyword`: specifies that arbitrarily many keyword arguments
        can be provided (in addition to any keyword arguments already accepted
        by other parameters).  Such a parameter can be defined by prepending
        the parameter name with ``**``, for example *kwargs* in the example
        above.

      Parameters can specify both optional and required arguments, as well as
      default values for some optional arguments.

      See also the :term:`argument` glossary entry, the FAQ question on
      :ref:`the difference between arguments and parameters
      <faq-argument-vs-parameter>`, the :class:`inspect.Parameter` class, the
      :ref:`function` section, and :pep:`362`.

   路径条目
      A single location on the :term:`import path` which the :term:`path
      based finder` consults to find modules for importing.

   路径条目查找器
      A :term:`finder` returned by a callable on :data:`sys.path_hooks`
      (i.e. a :term:`path entry hook`) which knows how to locate modules given
      a :term:`path entry`.

   路径条目 hook
      A callable on the :data:`sys.path_hook` list which returns a :term:`path
      entry finder` if it knows how to find modules on a specific :term:`path
      entry`.

   path based finder
      One of the default :term:`meta path finders <meta path finder>` which
      searches an :term:`import path` for modules.

   portion
      A set of files in a single directory (possibly stored in a zip file)
      that contribute to a namespace package, as defined in :pep:`420`.

   positional argument
      See :term:`argument`.

   provisional package
      A provisional package is one which has been deliberately excluded from
      the standard library's backwards compatibility guarantees.  While major
      changes to such packages are not expected, as long as they are marked
      provisional, backwards incompatible changes (up to and including removal
      of the package) may occur if deemed necessary by core developers.  Such
      changes will not be made gratuitously -- they will occur only if serious
      flaws are uncovered that were missed prior to the inclusion of the
      package.

      This process allows the standard library to continue to evolve over
      time, without locking in problematic design errors for extended periods
      of time.  See :pep:`411` for more details.

   Python 3000
      Nickname for the Python 3.x release line (coined long ago when the
      release of version 3 was something in the distant future.)  This is also
      abbreviated "Py3k".

   Pythonic
      An idea or piece of code which closely follows the most common idioms
      of the Python language, rather than implementing code using concepts
      common to other languages.  For example, a common idiom in Python is
      to loop over all elements of an iterable using a :keyword:`for`
      statement.  Many other languages don't have this type of construct, so
      people unfamiliar with Python sometimes use a numerical counter instead::

          for i in range(len(food)):
              print(food[i])

      As opposed to the cleaner, Pythonic method::

         for piece in food:
             print(piece)

   qualified name
      A dotted name showing the "path" from a module's global scope to a
      class, function or method defined in that module, as defined in
      :pep:`3155`.  For top-level functions and classes, the qualified name
      is the same as the object's name::

         >>> class C:
         ...     class D:
         ...         def meth(self):
         ...             pass
         ...
         >>> C.__qualname__
         'C'
         >>> C.D.__qualname__
         'C.D'
         >>> C.D.meth.__qualname__
         'C.D.meth'

      When used to refer to modules, the *fully qualified name* means the
      entire dotted path to the module, including any parent packages,
      e.g. ``email.mime.text``::

         >>> import email.mime.text
         >>> email.mime.text.__name__
         'email.mime.text'

   reference count
      The number of references to an object.  When the reference count of an
      object drops to zero, it is deallocated.  Reference counting is
      generally not visible to Python code, but it is a key element of the
      :term:`CPython` implementation.  The :mod:`sys` module defines a
      :func:`~sys.getrefcount` function that programmers can call to return the
      reference count for a particular object.

   regular package
      A traditional :term:`package`, such as a directory containing an
      ``__init__.py`` file.

   __slots__
      A declaration inside a class that saves memory by pre-declaring space for
      instance attributes and eliminating instance dictionaries.  Though
      popular, the technique is somewhat tricky to get right and is best
      reserved for rare cases where there are large numbers of instances in a
      memory-critical application.

   sequence
      An :term:`iterable` which supports efficient element access using integer
      indices via the :meth:`__getitem__` special method and defines a
      :meth:`__len__` method that returns the length of the sequence.
      Some built-in sequence types are :class:`list`, :class:`str`,
      :class:`tuple`, and :class:`bytes`. Note that :class:`dict` also
      supports :meth:`__getitem__` and :meth:`__len__`, but is considered a
      mapping rather than a sequence because the lookups use arbitrary
      :term:`immutable` keys rather than integers.

   slice
      An object usually containing a portion of a :term:`sequence`.  A slice is
      created using the subscript notation, ``[]`` with colons between numbers
      when several are given, such as in ``variable_name[1:3:5]``.  The bracket
      (subscript) notation uses :class:`slice` objects internally.

   special method
      A method that is called implicitly by Python to execute a certain
      operation on a type, such as addition.  Such methods have names starting
      and ending with double underscores.  Special methods are documented in
      :ref:`specialnames`.

   statement
      A statement is part of a suite (a "block" of code).  A statement is either
      an :term:`expression` or a one of several constructs with a keyword, such
      as :keyword:`if`, :keyword:`while` or :keyword:`for`.

   struct sequence
      A tuple with named elements. Struct sequences expose an interface similar
      to :term:`named tuple` in that elements can either be accessed either by
      index or as an attribute. However, they do not have any of the named tuple
      methods like :meth:`~collections.somenamedtuple._make` or
      :meth:`~collections.somenamedtuple._asdict`. Examples of struct sequences
      include :data:`sys.float_info` and the return value of :func:`os.stat`.

   triple-quoted string
      A string which is bound by three instances of either a quotation mark
      (") or an apostrophe (').  While they don't provide any functionality
      not available with single-quoted strings, they are useful for a number
      of reasons.  They allow you to include unescaped single and double
      quotes within a string and they can span multiple lines without the
      use of the continuation character, making them especially useful when
      writing docstrings.

   type
      The type of a Python object determines what kind of object it is; every
      object has a type.  An object's type is accessible as its
      :attr:`__class__` attribute or can be retrieved with ``type(obj)``.

   universal newlines
      A manner of interpreting text streams in which all of the following are
      recognized as ending a line: the Unix end-of-line convention ``'\n'``,
      the Windows convention ``'\r\n'``, and the old Macintosh convention
      ``'\r'``.  See :pep:`278` and :pep:`3116`, as well as
      :func:`str.splitlines` for an additional use.

   view
      The objects returned from :meth:`dict.keys`, :meth:`dict.values`, and
      :meth:`dict.items` are called dictionary views.  They are lazy sequences
      that will see changes in the underlying dictionary.  To force the
      dictionary view to become a full list use ``list(dictview)``.  See
      :ref:`dict-views`.

   virtual machine
      A computer defined entirely in software.  Python's virtual machine
      executes the :term:`bytecode` emitted by the bytecode compiler.

   Zen of Python
      Listing of Python design principles and philosophies that are helpful in
      understanding and using the language.  The listing can be found by typing
      "``import this``" at the interactive prompt.
