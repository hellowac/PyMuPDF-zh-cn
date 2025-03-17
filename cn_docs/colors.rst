.. include:: header.rst

.. _ColorDatabase:

================
颜色数据库
================

Color Database

.. tab:: 中文

    自从引入涉及颜色的方法（如 :meth:`Page.draw_circle` ）以来，可能需要访问预定义的颜色。

    精彩的 GUI 包 `wxPython <https://wxpython.org/>`_ 拥有一个包含超过 540 种预定义 RGB 颜色的数据库，并为这些颜色提供了更多或更易记的名称。除了“绿色”或“蓝色”等标准名称之外，还有“绿松石”、“天蓝色”和 100 种不同深浅的“灰色”等等。

    我们已经采取了将这个数据库（一个元组列表）复制并修改为 PyMuPDF 格式的措施，并使其颜色可作为 PDF 兼容的浮动三元组进行使用：对于 wxPython 中的 *("WHITE", 255, 255, 255)*，我们返回 *(1, 1, 1)*，可以直接在 *color* 和 *fill* 参数中使用。我们还接受“wHiTe”的任何大小写形式来查找颜色。

.. tab:: 英文

    Since the introduction of methods involving colors (like :meth:`Page.draw_circle`), a requirement may be to have access to predefined colors.

    The fabulous GUI package `wxPython <https://wxpython.org/>`_ has a database of over 540 predefined RGB colors, which are given more or less memorizable names. Among them are not only standard names like "green" or "blue", but also "turquoise", "skyblue", and 100 (not only 50 ...) shades of "gray", etc.

    We have taken the liberty to copy this database (a list of tuples) modified into PyMuPDF and make its colors available as PDF compatible float triples: for wxPython's *("WHITE", 255, 255, 255)* we return *(1, 1, 1)*, which can be directly used in *color* and *fill* parameters. We also accept any mixed case of "wHiTe" to find a color.

函数 *getColor()*
------------------------

Function *getColor()*

.. tab:: 中文

    由于颜色数据库可能不经常需要，因此引入额外的导入语句来访问它似乎是可以接受的::

        >>> # "getColor" 是你真正需要的唯一方法
        >>> from pymupdf.utils import getColor
        >>> getColor("aliceblue")
        (0.9411764705882353, 0.9725490196078431, 1.0)
        >>> #
        >>> # 获取所有现有名称的列表
        >>> from pymupdf.utils import getColorList
        >>> cl = getColorList()
        >>> cl
        ['ALICEBLUE', 'ANTIQUEWHITE', 'ANTIQUEWHITE1', 'ANTIQUEWHITE2', 'ANTIQUEWHITE3',
        'ANTIQUEWHITE4', 'AQUAMARINE', 'AQUAMARINE1'] ...
        >>> #
        >>> # 查看完整的整数颜色编码
        >>> from pymupdf.utils import getColorInfoList
        >>> il = getColorInfoList()
        >>> il
        [('ALICEBLUE', 240, 248, 255), ('ANTIQUEWHITE', 250, 235, 215),
        ('ANTIQUEWHITE1', 255, 239, 219), ('ANTIQUEWHITE2', 238, 223, 204),
        ('ANTIQUEWHITE3', 205, 192, 176), ('ANTIQUEWHITE4', 139, 131, 120),
        ('AQUAMARINE', 127, 255, 212), ('AQUAMARINE1', 127, 255, 212)] ...


.. tab:: 英文

    As the color database may not be needed very often, one additional import statement seems acceptable to get access to it::

        >>> # "getColor" is the only method you really need
        >>> from pymupdf.utils import getColor
        >>> getColor("aliceblue")
        (0.9411764705882353, 0.9725490196078431, 1.0)
        >>> #
        >>> # to get a list of all existing names
        >>> from pymupdf.utils import getColorList
        >>> cl = getColorList()
        >>> cl
        ['ALICEBLUE', 'ANTIQUEWHITE', 'ANTIQUEWHITE1', 'ANTIQUEWHITE2', 'ANTIQUEWHITE3',
        'ANTIQUEWHITE4', 'AQUAMARINE', 'AQUAMARINE1'] ...
        >>> #
        >>> # to see the full integer color coding
        >>> from pymupdf.utils import getColorInfoList
        >>> il = getColorInfoList()
        >>> il
        [('ALICEBLUE', 240, 248, 255), ('ANTIQUEWHITE', 250, 235, 215),
        ('ANTIQUEWHITE1', 255, 239, 219), ('ANTIQUEWHITE2', 238, 223, 204),
        ('ANTIQUEWHITE3', 205, 192, 176), ('ANTIQUEWHITE4', 139, 131, 120),
        ('AQUAMARINE', 127, 255, 212), ('AQUAMARINE1', 127, 255, 212)] ...


打印颜色数据库
----------------------------

Printing the Color Database

.. tab:: 中文

    如果你想实际查看这些可用的颜色，可以使用脚本 `print by RGB <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/print-rgb/print.py>`_ 或 `print by HSV <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/print-hsv/print.py>`_，它们位于示例目录中。这些脚本会创建 PDF（已经存在于同一目录中），展示所有这些颜色。它们唯一的不同之处在于排序顺序：一个按照 RGB 值排序，另一个则根据色调-饱和度-明度（HSV）值作为排序标准。

    以下是这些文件的屏幕截图展示：


.. tab:: 英文

    If you want to actually see how the many available colors look like, use scripts `print by RGB <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/print-rgb/print.py>`_ or `print by HSV <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/print-hsv/print.py>`_ in the examples directory. They create PDFs (already existing in the same directory) with all these colors. Their only difference is sorting order: one takes the RGB values, the other one the Hue-Saturation-Values as sort criteria.
    This is a screen print of what these files look like.

.. image:: images/img-colordb.*

.. include:: footer.rst