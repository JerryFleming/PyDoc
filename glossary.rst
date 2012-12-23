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
      在调用\ :term:`函数`(或者\ :term:`方法`)时所传递的值。实参有两种类型：

      * :dfn:`命名实参`\ ：在函数调用中带有标志符(例如 ``name=``)的参数，或者字典中键值前带有 ``**`` 的传入参数。例如下面调用 :func:`complex` 时 ``3`` 和 ``5`` 都是命名实参::

           complex(real=3, imag=5)
           complex(**{'real': 3, 'imag': 5})

      * :dfn:`位置实参`\ ：除了命名实参以外的实参。位置实参应该在实参列表的开始；或者作为\ :term:`迭代`\ 的一个元素，这时它前面要带有 ``*``\ 。例如下面调用 :func:`complex` 时 ``3`` 和 ``5`` 都是位置实参::

           complex(3, 5)
           complex(*(3, 5))

      在函数体内，实参会赋值给本地定义的变量，关于赋值的具体规则，参见\ :ref:`调用`\ 一节。从语法上来说，任何表达式都可以用作实参，它的结果值将会传给本地变量。

      另请参见术语词条\ :term:`形参`\ ，或者\ :ref:`实参和形参的区别<faq-argument-vs-parameter>`\ 处的常见问题，以及 :pep:`362`\ 。

   属性
      和某个对象相关联的一个值，可以用点表达式来引用。例如，如果有个对象 *o* 具有属性 *a*，可以用 *o.a* 来引用。

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
      这是您熟悉的实数系统的扩展，其中所有的数都表示为实部和虚部之和。虚数是虚数单位(``-1`` 的平方根，在数学中通常记为 ``i``，而在工程领域则记为 ``j``)与实数的积。Python 内置了对算数的支持，采用了第二种书写形式，即虚部带有 ``j`` 后缀，例如 ``3+1j``\ 。和 :mod:`math` 模块相对应的算数模块是 :mod:`cmath`\ 。复数的使用是相当高等的数学中才会有的。如果你不知道哪里要用到它，就可以放心的忽略这些。

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
      一段可以求值的语法结构。换句话说，一个表达式是由一系列表达式元素组成的，例如源常量，名称，属性访问，运算符，或者有返回值的函数调用。和其它语言不同的是，并非所有的语法结构都是表达式。还有些\ :term:`语句`\ 不能作为表达式使用，例如 :keyword:`if`\ 。赋值语法也是语句，而不是表达式。

   扩展模块
      使用 C 或者 C++ 编写的模块，它们通过 Python 的 C API 来和核心及用户代码交互。

   文件对象
      对底层资源提供面向文件 API (诸如 :meth:`read()` 或者 :meth:`write()` 的方法) 的对象。根据创建方式的不同，文件对象可以间接访问真实的磁盘文件，或者存储或通讯设备(例如标准输入/输出，内存中的缓存区，套接口，管道等)。文件对象又称作\ :dfn:`类文件对象`\ 或者\ :dfn:`流`\ 。

      文件对象事实上有三种类型：原始二进制文件，缓存二进制文件，以及文本文件。它们的接口都定义在 :mod:`io` 模块。创建文件的标准版方法是使用 :func:`open` 函数。

   类文件对象
      :term:`文件对象`\ 的同义词。

   查找器
      能够尽可能搜索一个模块 :term:`loader` 的对象。它必须实现一个叫 :meth:`find_loader` 或者 :meth:`find_module` 的方法。详情参见 :pep:`302` 以及 :pep:`420`\ ，或者 :class:`importlib.abc.Finder` 中的 :term:`abstract base class`\ 。

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
      :term:`CPython` 解释器所使用机制，用来确保同一时刻只有一个线程执行 Python :term:`字节码`\ 。这就简化了 CPython 的实现，因为它使得对象模型(包括重要的内置类型，例如 :class:`dict`)默认对并发访问就是安全的。Locking the entire interpreter
      makes it easier for the interpreter to be multi-threaded, at the
      expense of much of the parallelism afforded by multi-processor
      machines.

      However, some extension modules, either standard or third-party,
      are designed so as to release the GIL when doing computationally-intensive
      tasks such as compression or hashing.  Also, the GIL is always released
      when doing I/O.

      Past efforts to create a "free-threaded" interpreter (one which locks
      shared data at a much finer granularity) have not been successful
      because performance suffered in the common single-processor case. It
      is believed that overcoming this performance issue would make the
      implementation much more complicated and therefore costlier to maintain.

   hashable
      An object is *hashable* if it has a hash value which never changes during
      its lifetime (it needs a :meth:`__hash__` method), and can be compared to
      other objects (it needs an :meth:`__eq__` method).  Hashable objects which
      compare equal must have the same hash value.

      Hashability makes an object usable as a dictionary key and a set member,
      because these data structures use the hash value internally.

      All of Python's immutable built-in objects are hashable, while no mutable
      containers (such as lists or dictionaries) are.  Objects which are
      instances of user-defined classes are hashable by default; they all
      compare unequal, and their hash value is their :func:`id`.

   IDLE
      An Integrated Development Environment for Python.  IDLE is a basic editor
      and interpreter environment which ships with the standard distribution of
      Python.

   immutable
      An object with a fixed value.  Immutable objects include numbers, strings and
      tuples.  Such an object cannot be altered.  A new object has to
      be created if a different value has to be stored.  They play an important
      role in places where a constant hash value is needed, for example as a key
      in a dictionary.

   import path
      A list of locations (or :term:`path entries <path entry>`) that are
      searched by the :term:`path based finder` for modules to import. During
      import, this list of locations usually comes from :data:`sys.path`, but
      for subpackages it may also come from the parent package's ``__path__``
      attribute.

   importing
      The process by which Python code in one module is made available to
      Python code in another module.

   importer
      An object that both finds and loads a module; both a
      :term:`finder` and :term:`loader` object.

   interactive
      Python has an interactive interpreter which means you can enter
      statements and expressions at the interpreter prompt, immediately
      execute them and see their results.  Just launch ``python`` with no
      arguments (possibly by selecting it from your computer's main
      menu). It is a very powerful way to test out new ideas or inspect
      modules and packages (remember ``help(x)``).

   interpreted
      Python is an interpreted language, as opposed to a compiled one,
      though the distinction can be blurry because of the presence of the
      bytecode compiler.  This means that source files can be run directly
      without explicitly creating an executable which is then run.
      Interpreted languages typically have a shorter development/debug cycle
      than compiled ones, though their programs generally also run more
      slowly.  See also :term:`interactive`.

   iterable
      An object capable of returning its members one at a
      time. Examples of iterables include all sequence types (such as
      :class:`list`, :class:`str`, and :class:`tuple`) and some non-sequence
      types like :class:`dict` and :class:`file` and objects of any classes you
      define with an :meth:`__iter__` or :meth:`__getitem__` method.  Iterables
      can be used in a :keyword:`for` loop and in many other places where a
      sequence is needed (:func:`zip`, :func:`map`, ...).  When an iterable
      object is passed as an argument to the built-in function :func:`iter`, it
      returns an iterator for the object.  This iterator is good for one pass
      over the set of values.  When using iterables, it is usually not necessary
      to call :func:`iter` or deal with iterator objects yourself.  The ``for``
      statement does that automatically for you, creating a temporary unnamed
      variable to hold the iterator for the duration of the loop.  See also
      :term:`iterator`, :term:`sequence`, and :term:`generator`.

   iterator
      An object representing a stream of data.  Repeated calls to the iterator's
      :meth:`~iterator.__next__` method (or passing it to the built-in function
      :func:`next`) return successive items in the stream.  When no more data
      are available a :exc:`StopIteration` exception is raised instead.  At this
      point, the iterator object is exhausted and any further calls to its
      :meth:`__next__` method just raise :exc:`StopIteration` again.  Iterators
      are required to have an :meth:`__iter__` method that returns the iterator
      object itself so every iterator is also iterable and may be used in most
      places where other iterables are accepted.  One notable exception is code
      which attempts multiple iteration passes.  A container object (such as a
      :class:`list`) produces a fresh new iterator each time you pass it to the
      :func:`iter` function or use it in a :keyword:`for` loop.  Attempting this
      with an iterator will just return the same exhausted iterator object used
      in the previous iteration pass, making it appear like an empty container.

      More information can be found in :ref:`typeiter`.

   key function
      A key function or collation function is a callable that returns a value
      used for sorting or ordering.  For example, :func:`locale.strxfrm` is
      used to produce a sort key that is aware of locale specific sort
      conventions.

      A number of tools in Python accept key functions to control how elements
      are ordered or grouped.  They include :func:`min`, :func:`max`,
      :func:`sorted`, :meth:`list.sort`, :func:`heapq.nsmallest`,
      :func:`heapq.nlargest`, and :func:`itertools.groupby`.

      There are several ways to create a key function.  For example. the
      :meth:`str.lower` method can serve as a key function for case insensitive
      sorts.  Alternatively, an ad-hoc key function can be built from a
      :keyword:`lambda` expression such as ``lambda r: (r[0], r[2])``.  Also,
      the :mod:`operator` module provides three key function constructors:
      :func:`~operator.attrgetter`, :func:`~operator.itemgetter`, and
      :func:`~operator.methodcaller`.  See the :ref:`Sorting HOW TO
      <sortinghowto>` for examples of how to create and use key functions.

   keyword argument
      See :term:`argument`.

   lambda
      An anonymous inline function consisting of a single :term:`expression`
      which is evaluated when the function is called.  The syntax to create
      a lambda function is ``lambda [arguments]: expression``

   LBYL
      看看周围环境再跳跃。这种编程风格在调用方法或者查找属性前明确检测前提条件。它和 :term:`EAFP` 方法相对，其特征是有很多 :keyword:`if` 语句。

      在多线程环境中，LBYL 方法可能引发"查看者"和"跳跃者"之间的竞争状态。例如，在代码 ``if key in mapping: return mapping[key]`` 中，如果另一个线程在检测过后查找之前把 *key* 从 *mapping* 删除掉。这种情况可以通过锁或者 EAFP 方法解决。

   list
      A built-in Python :term:`sequence`.  Despite its name it is more akin
      to an array in other languages than to a linked list since access to
      elements are O(1).

   list comprehension
      A compact way to process all or part of the elements in a sequence and
      return a list with the results.  ``result = ['{:#04x}'.format(x) for x in
      range(256) if x % 2 == 0]`` generates a list of strings containing
      even hex numbers (0x..) in the range from 0 to 255. The :keyword:`if`
      clause is optional.  If omitted, all elements in ``range(256)`` are
      processed.

   loader
      An object that loads a module. It must define a method named
      :meth:`load_module`. A loader is typically returned by a
      :term:`finder`. See :pep:`302` for details and
      :class:`importlib.abc.Loader` for an :term:`abstract base class`.

   mapping
      A container object that supports arbitrary key lookups and implements the
      methods specified in the :class:`~collections.abc.Mapping` or
      :class:`~collections.abc.MutableMapping`
      :ref:`abstract base classes <collections-abstract-base-classes>`.  Examples
      include :class:`dict`, :class:`collections.defaultdict`,
      :class:`collections.OrderedDict` and :class:`collections.Counter`.

   meta path finder
      A finder returned by a search of :data:`sys.meta_path`.  Meta path
      finders are related to, but different from :term:`path entry finders
      <path entry finder>`.

   metaclass
      The class of a class.  Class definitions create a class name, a class
      dictionary, and a list of base classes.  The metaclass is responsible for
      taking those three arguments and creating the class.  Most object oriented
      programming languages provide a default implementation.  What makes Python
      special is that it is possible to create custom metaclasses.  Most users
      never need this tool, but when the need arises, metaclasses can provide
      powerful, elegant solutions.  They have been used for logging attribute
      access, adding thread-safety, tracking object creation, implementing
      singletons, and many other tasks.

      More information can be found in :ref:`metaclasses`.

   method
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

   path entry
      A single location on the :term:`import path` which the :term:`path
      based finder` consults to find modules for importing.

   path entry finder
      A :term:`finder` returned by a callable on :data:`sys.path_hooks`
      (i.e. a :term:`path entry hook`) which knows how to locate modules given
      a :term:`path entry`.

   path entry hook
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
