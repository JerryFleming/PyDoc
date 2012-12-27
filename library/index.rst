.. _library-index:

###############################
Python 标准库
###############################

:ref:`reference-index`\ 准确的描述了 Python 语言的语法和主义，而这里的标准库参考手册介绍了随着 Python 一起发行的标准库。它还介绍一些 Python 发行版中常见的模块。

Python 标准库内容非常广泛，包括下面冗长列表中所展示的各种组件。库中有内置模块(用 C 实现）用来访问系统功能，例如文件读写，没有这些 Python 程序员就不能访问系统了；还有些模块是用 Python 写的，为很多日常编程工作中的问题提供了标准化的解决方案。有的模块在设计时明确把系统相关的需求抽象成与系统无关的 API，这就有助于增强 Python 程序的可移植性。

Windows 系统上的 Python 安装程序通常会包含完整的标准库，以及很多额外的组件。对于类 Unix 系统，Python 通常作为一系列包提供，所以可能有必要使用操作系统的包管理工具来安装全部或部分组件。

除了标准库以外，还有数以千计并且不断增加的组件(从个别程序和模块到包甚至整个应用开发框架)，它们位于 `Python 包索引 <http://pypi.python.org/pypi>`_\ 上。


.. toctree::
   :maxdepth: 2
   :numbered:

   intro.rst
   functions.rst
   constants.rst
   stdtypes.rst
   exceptions.rst

   text.rst
   binary.rst
   datatypes.rst
   numeric.rst
   functional.rst
   filesys.rst
   persistence.rst
   archiving.rst
   fileformats.rst
   crypto.rst
   allos.rst
   concurrency.rst
   ipc.rst
   netdata.rst
   markup.rst
   internet.rst
   mm.rst
   i18n.rst
   frameworks.rst
   tk.rst
   development.rst
   debug.rst
   python.rst
   custominterp.rst
   modules.rst
   language.rst
   misc.rst
   windows.rst
   unix.rst
   undoc.rst
