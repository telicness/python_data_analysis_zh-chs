
[*第7章：使用matplotlib进行数据可视化*](./README.md)


# 7.1. matplotlib库


matplotlib是一个Python库，专门开发二维图表(包括3D图表)。近年来，它在科学界和工程界得到了广泛的应用(http://matplolib.org).)。

在所有使它成为图形化数据表示中最常用的工具的特性中，有一些特别突出:

* 使用极其简单
* 渐进式开发与交互式数据可视化
* 支持LaTeX中的表达式和文本
* 强大的图形元素的控制能力
* 可导出到许多格式，例如PNG、PDF、SVG和EPS

matplotlib旨在尽可能多地从图形视图和语法形式方面重现与MATLAB类似的环境。这种方法已经被证明是成功的,因为它已经能够利用软件的经验(MATLAB)已经在市场上了好几年,现在普遍在所有专业技术——­科学界。matplotlib不仅是基于一个已知方案和大多数专家在该领域非常熟悉,而且还利用那些多年来已导致的优化可推断性和简单的使用,使得这个库也是一个很好的选择对于那些首次接近数据可视化,特别是那些没有任何经验如MATLAB或类似的应用程序。

除了简单性和可推断性之外，matplotlib库还继承了MATLAB的交互性。也就是说，分析人员可以逐一插入命令来控制数据图形表示的逐步开发。这种模式非常适合Python的交互式方法，如IPython QtConsole和IPython NoteBook(请参阅第2章)，从而提供了一个可媲美Mathematica、IDL或MATLAB等其他工具的数据分析环境。

这个漂亮库的开发者天才之处在于，他们利用并整合了目前可用的和在科学中使用的好东西。正如我们所看到的，这不仅局限于MATLAB和类似的操作模式，而且还局限于科学表达式的文本格式模型和LaTeX所代表的符号。由于LaTeX具有强大的显示和表示科学表达式的能力，因此在任何科学出版物或文档中都是不可替代的元素，在这些文档中，必须以可视化的方式表示表达式，如积分、求和还有导数。因此，matplotlib集成了这一出色的工具，以提高图表的表现能力。

另外，不要忘记matplotlib不是一个独立的应用程序，而是Python之类的编程语言的库。因此，它还充分利用了编程语言提供的潜力。matplotlib看起来像一个图形库，它允许您以编程方式管理组成图表的图形元素，以便完整地控制图形显示。可编程图形表示的能力允许管理跨多个环境的数据表示的可再现性，特别是在进行更改或数据更新时。

此外，由于matplotlib是一个Python库，它允许您充分利用其他库的潜力，任何使用这种语言实现的开发人员都可以使用这些库。事实上，在数据分析方面，matplotlib通常会与其他一些库(如NumPy和pandas合作，但许多其他库可以毫无问题地集成。

最后，通过使用这个库进行编码获得的图形表示可以以最常见的图形格式(如PNG和SVG)导出，然后在其他应用程序、文档、web页面等中使用。
