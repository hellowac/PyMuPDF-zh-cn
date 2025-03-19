.. include:: header.rst

.. _Appendix3:

================================================
附录 3：各类技术信息
================================================

Appendix 3: Assorted Technical Information

.. tab:: 中文

    本节涉及各种技术主题，这些主题不一定彼此相关。

.. tab:: 英文

    This section deals with various technical topics, that are not necessarily related to each other.

------------

.. _ImageTransformation:

图像转换矩阵
----------------------------

Image Transformation Matrix

.. tab:: 中文

    从版本 1.18.11 开始，一些文本和图像提取方法会返回图像变换矩阵： :meth:`Page.get_text` 和： :meth:`Page.get_image_bbox`。

    变换矩阵包含有关图像如何转换以适应某个文档页面上的矩形（其“边界框” = “bbox”）的信息。通过检查图像在页面上的 bbox 和此矩阵，可以确定图像是否以及如何在页面上进行缩放或旋转。

    图像尺寸与页面上 bbox 之间的关系如下：

    1. 使用原始图像的宽度和高度，
        - 定义图像矩形 `imgrect = pymupdf.Rect(0, 0, width, height)`
        - 定义“缩放矩阵” `shrink = pymupdf.Matrix(1/width, 0, 0, 1/height, 0, 0)`。

    2. 使用缩放矩阵转换图像矩形，将得到单位矩形： `imgrect * shrink = pymupdf.Rect(0, 0, 1, 1)` 。

    3. 使用图像 **变换矩阵** “transform”，以下步骤将计算 bbox::

        imgrect = pymupdf.Rect(0, 0, width, height)
        shrink = pymupdf.Matrix(1/width, 0, 0, 1/height, 0, 0)
        bbox = imgrect * shrink * transform

    4. 检查矩阵乘积 `shrink * transform` 将揭示所有有关图像矩形发生了什么变化的信息，以使其适应页面上的 bbox：旋转、缩放其边和原点的平移。让我们来看一个例子：

        >>> imginfo = page.get_images()[0]  # 获取页面上的图像项
        >>> imginfo
        (5, 0, 439, 501, 8, 'DeviceRGB', '', 'fzImg0', 'DCTDecode')
        >>> #------------------------------------------------
        >>> # 定义图像缩放矩阵和矩形
        >>> #------------------------------------------------
        >>> shrink = pymupdf.Matrix(1 / 439, 0, 0, 1 / 501, 0, 0)
        >>> imgrect = pymupdf.Rect(0, 0, 439, 501)
        >>> #------------------------------------------------
        >>> # 确定图像的 bbox 和变换矩阵：
        >>> #------------------------------------------------
        >>> bbox, transform = page.get_image_bbox("fzImg0", transform=True)
        >>> #------------------------------------------------
        >>> # 确认相等性 - 允许舍入误差
        >>> #------------------------------------------------
        >>> bbox
        Rect(100.0, 112.37525939941406, 300.0, 287.624755859375)
        >>> imgrect * shrink * transform
        Rect(100.0, 112.375244140625, 300.0, 287.6247253417969)
        >>> #------------------------------------------------
        >>> # 缩放矩阵与变换矩阵的乘积
        >>> #------------------------------------------------
        >>> shrink * transform
        Matrix(0.0, -0.39920157194137573, 0.3992016017436981, 0.0, 100.0, 287.6247253417969)
        >>> #------------------------------------------------
        >>> # 上述结果说明：
        >>> # 图像的边是按约 0.4 的因子缩放的，
        >>> # 并且图像顺时针旋转了 90 度
        >>> # 比较这与 pymupdf.Matrix(-90) * 0.4
        >>> #------------------------------------------------


.. tab:: 英文

    Starting with version 1.18.11, the image transformation matrix is returned by some methods for text and image extraction: :meth:`Page.get_text` and :meth:`Page.get_image_bbox`.

    The transformation matrix contains information about how an image was transformed to fit into the rectangle (its "boundary box" = "bbox") on some document page. By inspecting the image's bbox on the page and this matrix, one can determine for example, whether and how the image is displayed scaled or rotated on a page.

    The relationship between image dimension and its bbox on a page is the following:

    1. Using the original image's width and height,
        - define the image rectangle `imgrect = pymupdf.Rect(0, 0, width, height)`
        - define the "shrink matrix" `shrink = pymupdf.Matrix(1/width, 0, 0, 1/height, 0, 0)`.

    2. Transforming the image rectangle with its shrink matrix, will result in the unit rectangle: `imgrect * shrink = pymupdf.Rect(0, 0, 1, 1)`.

    3. Using the image **transformation matrix** "transform", the following steps will compute the bbox::

        imgrect = pymupdf.Rect(0, 0, width, height)
        shrink = pymupdf.Matrix(1/width, 0, 0, 1/height, 0, 0)
        bbox = imgrect * shrink * transform

    4. Inspecting the matrix product `shrink * transform` will reveal all information about what happened to the image rectangle to make it fit into the bbox on the page: rotation, scaling of its sides and translation of its origin. Let us look at an example:

        >>> imginfo = page.get_images()[0]  # get an image item on a page
        >>> imginfo
        (5, 0, 439, 501, 8, 'DeviceRGB', '', 'fzImg0', 'DCTDecode')
        >>> #------------------------------------------------
        >>> # define image shrink matrix and rectangle
        >>> #------------------------------------------------
        >>> shrink = pymupdf.Matrix(1 / 439, 0, 0, 1 / 501, 0, 0)
        >>> imgrect = pymupdf.Rect(0, 0, 439, 501)
        >>> #------------------------------------------------
        >>> # determine image bbox and transformation matrix:
        >>> #------------------------------------------------
        >>> bbox, transform = page.get_image_bbox("fzImg0", transform=True)
        >>> #------------------------------------------------
        >>> # confirm equality - permitting rounding errors
        >>> #------------------------------------------------
        >>> bbox
        Rect(100.0, 112.37525939941406, 300.0, 287.624755859375)
        >>> imgrect * shrink * transform
        Rect(100.0, 112.375244140625, 300.0, 287.6247253417969)
        >>> #------------------------------------------------
        >>> shrink * transform
        Matrix(0.0, -0.39920157194137573, 0.3992016017436981, 0.0, 100.0, 287.6247253417969)
        >>> #------------------------------------------------
        >>> # the above shows:
        >>> # image sides are scaled by same factor ~0.4,
        >>> # and the image is rotated by 90 degrees clockwise
        >>> # compare this with pymupdf.Matrix(-90) * 0.4
        >>> #------------------------------------------------


------------

.. _Base-14-Fonts:

PDF Base 14 字体
---------------------

PDF Base 14 Fonts

.. tab:: 中文

    以下 14 种内建字体名称 **必须** 被每个 PDF 查看器应用程序支持。它们作为字典提供，该字典将它们的全名和小写的缩写映射到完整的字体基本名称。每当在 PyMuPDF 中需要提供 **fontname** 时，可以使用字典中的任何 **键或值**：

        In [2]: pymupdf.Base14_fontdict
        Out[2]:
        {'courier': 'Courier',
        'courier-oblique': 'Courier-Oblique',
        'courier-bold': 'Courier-Bold',
        'courier-boldoblique': 'Courier-BoldOblique',
        'helvetica': 'Helvetica',
        'helvetica-oblique': 'Helvetica-Oblique',
        'helvetica-bold': 'Helvetica-Bold',
        'helvetica-boldoblique': 'Helvetica-BoldOblique',
        'times-roman': 'Times-Roman',
        'times-italic': 'Times-Italic',
        'times-bold': 'Times-Bold',
        'times-bolditalic': 'Times-BoldItalic',
        'symbol': 'Symbol',
        'zapfdingbats': 'ZapfDingbats',
        'helv': 'Helvetica',
        'heit': 'Helvetica-Oblique',
        'hebo': 'Helvetica-Bold',
        'hebi': 'Helvetica-BoldOblique',
        'cour': 'Courier',
        'coit': 'Courier-Oblique',
        'cobo': 'Courier-Bold',
        'cobi': 'Courier-BoldOblique',
        'tiro': 'Times-Roman',
        'tibo': 'Times-Bold',
        'tiit': 'Times-Italic',
        'tibi': 'Times-BoldItalic',
        'symb': 'Symbol',
        'zadb': 'ZapfDingbats'}

    与其强制要求不同，并非所有 PDF 查看器都能正确和完全地支持这些字体——尤其是 **Symbol** 和 **ZapfDingbats**。此外，字形（视觉效果）图像将根据每个查看器的不同而有所不同。

    要查看如何使用这些字体——包括 **CJK 内建** 字体——请参见： :meth:`Page.insert_font` 中的表格。


.. tab:: 英文

    The following 14 builtin font names **must be supported by every PDF viewer** application. They are available as a dictionary, which maps their full names amd their abbreviations in lower case to the full font basename. Wherever a **fontname** must be provided in PyMuPDF, any **key or value** from the dictionary may be used::

        In [2]: pymupdf.Base14_fontdict
        Out[2]:
        {'courier': 'Courier',
        'courier-oblique': 'Courier-Oblique',
        'courier-bold': 'Courier-Bold',
        'courier-boldoblique': 'Courier-BoldOblique',
        'helvetica': 'Helvetica',
        'helvetica-oblique': 'Helvetica-Oblique',
        'helvetica-bold': 'Helvetica-Bold',
        'helvetica-boldoblique': 'Helvetica-BoldOblique',
        'times-roman': 'Times-Roman',
        'times-italic': 'Times-Italic',
        'times-bold': 'Times-Bold',
        'times-bolditalic': 'Times-BoldItalic',
        'symbol': 'Symbol',
        'zapfdingbats': 'ZapfDingbats',
        'helv': 'Helvetica',
        'heit': 'Helvetica-Oblique',
        'hebo': 'Helvetica-Bold',
        'hebi': 'Helvetica-BoldOblique',
        'cour': 'Courier',
        'coit': 'Courier-Oblique',
        'cobo': 'Courier-Bold',
        'cobi': 'Courier-BoldOblique',
        'tiro': 'Times-Roman',
        'tibo': 'Times-Bold',
        'tiit': 'Times-Italic',
        'tibi': 'Times-BoldItalic',
        'symb': 'Symbol',
        'zadb': 'ZapfDingbats'}

    In contrast to their obligation, not all PDF viewers support these fonts correctly and completely -- this is especially true for Symbol and ZapfDingbats. Also, the glyph (visual) images will be specific to every reader.

    To see how these fonts can be used -- including the **CJK built-in** fonts -- look at the table in :meth:`Page.insert_font`.

------------

.. _AdobeManual:

Adobe PDF 参考
---------------------------

Adobe PDF References

.. tab:: 中文

    本 PDF 参考手册由 Adobe 发布，在本文档中经常被引用。它可以通过以下链接查看和下载： `here <https://opensource.adobe.com/dc-acrobat-sdk-docs/standards/pdfstandards/pdf/PDF32000_2008.pdf>`_ 。

    .. note:: 

        长时间以来，较旧版本的 PDF 参考手册也可以通过 `this <http://www.adobe.com/content/dam/Adobe/en/devnet/acrobat/pdfs/pdf_reference_1-7.pdf>`_ 链接访问。该链接似乎在 2021 年 10 月被从网站上移除。在 PyMuPDF 文档的早期版本（1.19.* 之前）中，曾引用过此文档。我们已经着手替换这些引用，指向当前的规范链接。


.. tab:: 英文

    This PDF Reference manual published by Adobe is frequently quoted throughout this documentation. It can be viewed and downloaded from `here <https://opensource.adobe.com/dc-acrobat-sdk-docs/standards/pdfstandards/pdf/PDF32000_2008.pdf>`_.

    .. note:: 
      
        For a long time, an older version was also available under `this <http://www.adobe.com/content/dam/Adobe/en/devnet/acrobat/pdfs/pdf_reference_1-7.pdf>`_ link. It seems to be taken off of the web site in October 2021. Earlier (pre 1.19.*) versions of the PyMuPDF documentation used to refer to this document. We have undertaken an effort to replace referrals to the current specification above.

------------

.. _SequenceTypes:

在 PyMuPDF 中使用 Python 序列作为参数
------------------------------------------------

Using Python Sequences as Arguments in PyMuPDF

.. tab:: 中文

    当 PyMuPDF 对象和方法要求使用 Python **列表** 形式的数值时，其他 Python **序列类型** 也可以使用。Python 类被认为实现了 **序列协议**，如果它们具有 `__getitem__()` 方法。

    这基本上意味着，您可以在这些情况下互换使用 Python 的 *list*、 *tuple* ，甚至是 *array.array* 、 *numpy.array* 和 *bytearray* 类型。

    例如，以以下任何方式指定一个序列 `"s"`：

    * `s = [1, 2]` -- 一个列表
    * `s = (1, 2)` -- 一个元组
    * `s = array.array("i", (1, 2))` -- 一个 array.array
    * `s = numpy.array((1, 2))` -- 一个 numpy 数组
    * `s = bytearray((1, 2))` -- 一个字节数组

    都可以在以下表达式中使用：

    * `pymupdf.Point(s)`
    * `pymupdf.Point(x, y) + s`
    * `doc.select(s)`

    同样，所有几何对象如 :ref:`Rect`、:ref:`IRect`、:ref:`Matrix` 和 :ref:`Point` 也可以如此使用。

    因为所有 PyMuPDF 几何类本身是序列的特例，所以它们（除了 :ref:`Quad` -- 见下文）可以自由地在需要数值序列的地方使用，例如作为 *list()*、*tuple()*、*array.array()* 或 *numpy.array()* 等函数的参数。请查看以下代码片段了解其工作原理。

    >>> import pymupdf, array, numpy as np
    >>> m = pymupdf.Matrix(1, 2, 3, 4, 5, 6)
    >>>
    >>> list(m)
    [1.0, 2.0, 3.0, 4.0, 5.0, 6.0]
    >>>
    >>> tuple(m)
    (1.0, 2.0, 3.0, 4.0, 5.0, 6.0)
    >>>
    >>> array.array("f", m)
    array('f', [1.0, 2.0, 3.0, 4.0, 5.0, 6.0])
    >>>
    >>> np.array(m)
    array([1., 2., 3., 4., 5., 6.])

    .. note::

      :ref:`Quad` 也是一个 Python 序列对象，长度为 4。它的项是 :data:`point_like` -- 而非数字。因此，以上内容不适用于 :ref:`Quad`。


.. tab:: 英文

    When PyMuPDF objects and methods require a Python **list** of numerical values, other Python **sequence types** are also allowed. Python classes are said to implement the **sequence protocol**, if they have a `__getitem__()` method.

    This basically means, you can interchangeably use Python *list* or *tuple* or even *array.array*, *numpy.array* and *bytearray* types in these cases.

    For example, specifying a sequence `"s"` in any of the following ways

    * `s = [1, 2]` -- a list
    * `s = (1, 2)` -- a tuple
    * `s = array.array("i", (1, 2))` -- an array.array
    * `s = numpy.array((1, 2))` -- a numpy array
    * `s = bytearray((1, 2))` -- a bytearray

    will make it usable in the following example expressions:

    * `pymupdf.Point(s)`
    * `pymupdf.Point(x, y) + s`
    * `doc.select(s)`

    Similarly with all geometry objects :ref:`Rect`, :ref:`IRect`, :ref:`Matrix` and :ref:`Point`.

    Because all PyMuPDF geometry classes themselves are special cases of sequences, they (with the exception of :ref:`Quad` -- see below) can be freely used where numerical sequences can be used, e.g. as arguments for functions like *list()*, *tuple()*, *array.array()* or *numpy.array()*. Look at the following snippet to see this work.

    >>> import pymupdf, array, numpy as np
    >>> m = pymupdf.Matrix(1, 2, 3, 4, 5, 6)
    >>>
    >>> list(m)
    [1.0, 2.0, 3.0, 4.0, 5.0, 6.0]
    >>>
    >>> tuple(m)
    (1.0, 2.0, 3.0, 4.0, 5.0, 6.0)
    >>>
    >>> array.array("f", m)
    array('f', [1.0, 2.0, 3.0, 4.0, 5.0, 6.0])
    >>>
    >>> np.array(m)
    array([1., 2., 3., 4., 5., 6.])

    .. note:: 
      
      :ref:`Quad` is a Python sequence object as well and has a length of 4. Its items however are :data:`point_like` -- not numbers. Therefore, the above remarks do not apply.

------------

.. _ReferenialIntegrity:

确保 PyMuPDF 中重要对象的一致性
------------------------------------------------------------

Ensuring Consistency of Important Objects in PyMuPDF

.. tab:: 中文

    PyMuPDF 是 C 库 MuPDF 的 Python 绑定。虽然 MuPDF 的创建者投入了大量的努力来逼近某种面向对象的行为，但他们肯定无法克服 C 语言在这方面的基本缺陷。

    另一方面，Python 在实现面向对象模型时非常干净。PyMuPDF 和 MuPDF 之间的接口代码由两个基本文件组成：*pymupdf.py* 和 *fitz_wrap.c*。这些文件由出色的 SWIG 工具为每个新版本生成。

    当你使用 PyMuPDF 的对象或方法时，会执行 *pymupdf.py* 中的一些代码，进而调用用 *fitz_wrap.c* 编译的 C 代码。

    由于 SWIG 会尽量保持 Python 和 C 级别的同步，因此，只要严格遵循一定的规则，一切就会正常工作。例如：**永远不要访问** 已关闭（或删除或设置为 *None*）的文档的 :ref:`Page` 对象。或者，更不明显的规则：**永远不要访问** 页面或其子对象（链接或注释），如果你执行了文档方法如 *select()*、*delete_page()*、*insert_page()* 等等。

    但仅仅不再访问失效的对象是不够的：它们应该被主动删除，以完全释放 C 级资源（即分配的内存）。

    这些规则的原因在于，文档与其页面之间，以及页面与其链接/注释之间存在着一种层次化的 2 级一对多关系。为了维持一致的状态，上述任何操作必须导致 **Python 和 C** 中的完全重置。

    SWIG 并不知道这一点，因此不会自动执行此操作。

    因此，所需的逻辑已经内建于 PyMuPDF 中，具体方式如下。

    1. 如果页面“失去”了其拥有的文档或被删除，那么它当前存在的所有注释和链接将在 Python 中变得不可用，并且它们在 C 级的对应项将被删除并释放。

    2. 如果文档被关闭（或删除或设置为 *None*），或者其结构发生变化，那么同样地，所有当前存在的页面及其子项将变得不可用，并将进行相应的 C 级删除。“结构变化”包括像 *select()*、*delete_page()*、*insert_page()*、*insert_pdf()* 等方法：所有这些都会导致对象删除的级联效应。

    程序员通常不会意识到这一点。但如果他尝试访问失效的对象，将会引发异常。

    失效的对象不能像使用 Python 语句 *del page* 或 *page = None* 等直接删除。相反，必须调用它们的 *__del__* 方法。

    所有页面、链接和注释都有 *parent* 属性，指向拥有它们的对象。这是可以在应用层检查的属性：如果 *obj.parent == None*，则该对象的父对象已不存在，任何对其属性或方法的引用将引发异常，提示“孤立”状态。

    示例会话：

    >>> page = doc[n]
    >>> annot = page.first_annot
    >>> annot.type                    # 一切正常
    [5, 'Circle']
    >>> page = None                   # 这会将 'annot' 变成孤立对象
    >>> annot.type
    <...省略行...>
    RuntimeError: orphaned object: parent is None
    >>>
    >>> # 如果你这样做，也会发生相同的情况：
    >>> annot = doc[n].first_annot     # 立即删除该页面！
    >>> annot.type                    # 因此，'annot' 是“孤立出生的”
    <...省略行...>
    RuntimeError: orphaned object: parent is None

    这展示了级联效应：

    >>> doc = pymupdf.open("some.pdf")
    >>> page = doc[n]
    >>> annot = page.first_annot
    >>> page.rect
    pymupdf.Rect(0.0, 0.0, 595.0, 842.0)
    >>> annot.type
    [5, 'Circle']
    >>> del doc                       # 或 doc = None 或 doc.close()
    >>> page.rect
    <...省略行...>
    RuntimeError: orphaned object: parent is None
    >>> annot.type
    <...省略行...>
    RuntimeError: orphaned object: parent is None

    .. note::

        上述关系之外的对象不包含在此机制中。如果你例如通过 *toc = doc.get_toc()* 创建了目录，然后关闭或更改文档，那么这不会以任何方式更改变量 *toc*。刷新这些变量的责任由你自己承担。


.. tab:: 英文

    PyMuPDF is a Python binding for the C library MuPDF. While a lot of effort has been invested by MuPDF's creators to approximate some sort of an object-oriented behavior, they certainly could not overcome basic shortcomings of the C language in that respect.

    Python on the other hand implements the OO-model in a very clean way. The interface code between PyMuPDF and MuPDF consists of two basic files: *pymupdf.py* and *fitz_wrap.c*. They are created by the excellent SWIG tool for each new version.

    When you use one of PyMuPDF's objects or methods, this will result in execution of some code in *pymupdf.py*, which in turn will call some C code compiled with *fitz_wrap.c*.

    Because SWIG goes a long way to keep the Python and the C level in sync, everything works fine, if a certain set of rules is being strictly followed. For example: **never access** a :ref:`Page` object, after you have closed (or deleted or set to *None*) the owning :ref:`Document`. Or, less obvious: **never access** a page or any of its children (links or annotations) after you have executed one of the document methods *select()*, *delete_page()*, *insert_page()* ... and more.

    But just no longer accessing invalidated objects is actually not enough: They should rather be actively deleted entirely, to also free C-level resources (meaning allocated memory).

    The reason for these rules lies in the fact that there is a hierarchical 2-level one-to-many relationship between a document and its pages and also between a page and its links / annotations. To maintain a consistent situation, any of the above actions must lead to a complete reset -- in **Python and, synchronously, in C**.

    SWIG cannot know about this and consequently does not do it.

    The required logic has therefore been built into PyMuPDF itself in the following way.

    1. If a page "loses" its owning document or is being deleted itself, all of its currently existing annotations and links will be made unusable in Python, and their C-level counterparts will be deleted and deallocated.

    2. If a document is closed (or deleted or set to *None*) or if its structure has changed, then similarly all currently existing pages and their children will be made unusable, and corresponding C-level deletions will take place. "Structure changes" include methods like *select()*, *delePage()*, *insert_page()*, *insert_pdf()* and so on: all of these will result in a cascade of object deletions.

    The programmer will normally not realize any of this. If he, however, tries to access invalidated objects, exceptions will be raised.

    Invalidated objects cannot be directly deleted as with Python statements like *del page* or *page = None*, etc. Instead, their *__del__* method must be invoked.

    All pages, links and annotations have the property *parent*, which points to the owning object. This is the property that can be checked on the application level: if *obj.parent == None* then the object's parent is gone, and any reference to its properties or methods will raise an exception informing about this "orphaned" state.

    A sample session:

    >>> page = doc[n]
    >>> annot = page.first_annot
    >>> annot.type                    # everything works fine
    [5, 'Circle']
    >>> page = None                   # this turns 'annot' into an orphan
    >>> annot.type
    <... omitted lines ...>
    RuntimeError: orphaned object: parent is None
    >>>
    >>> # same happens, if you do this:
    >>> annot = doc[n].first_annot     # deletes the page again immediately!
    >>> annot.type                    # so, 'annot' is 'born' orphaned
    <... omitted lines ...>
    RuntimeError: orphaned object: parent is None

    This shows the cascading effect:

    >>> doc = pymupdf.open("some.pdf")
    >>> page = doc[n]
    >>> annot = page.first_annot
    >>> page.rect
    pymupdf.Rect(0.0, 0.0, 595.0, 842.0)
    >>> annot.type
    [5, 'Circle']
    >>> del doc                       # or doc = None or doc.close()
    >>> page.rect
    <... omitted lines ...>
    RuntimeError: orphaned object: parent is None
    >>> annot.type
    <... omitted lines ...>
    RuntimeError: orphaned object: parent is None

    .. note:: 
      
        Objects outside the above relationship are not included in this mechanism. If you e.g. created a table of contents by *toc = doc.get_toc()*, and later close or change the document, then this cannot and does not change variable *toc* in any way. It is your responsibility to refresh such variables as required.

------------

.. _FormXObject:

方法设计 :meth:`Page.show_pdf_page`
--------------------------------------------

Design of Method :meth:`Page.show_pdf_page`

目的和功能
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Purpose and Capabilities

.. tab:: 中文

    该方法在当前（“包含”或“目标”）页面的指定矩形区域内显示另一个 PDF 文档的页面（“源”页面）图像。

    * **与** :meth:`Page.insert_image` **不同** ，这种显示是基于矢量的，因此在缩放时保持精确度。
    * **与** :meth:`Page.insert_image` **一样** ，显示的大小会根据给定的矩形调整。

    当前支持以下几种显示变体：

    * 布尔参数 `"keep_proportion"` 控制是否保持宽高比（默认保持）。  
        * 矩形参数 `"clip"` 限制源页面矩形可见的部分。默认显示整个页面。
    * 浮动参数 `"rotation"` 以任意角度（度数）旋转显示。如果角度不是 90 的整数倍且 `"keep_proportion"` 为真，则只能将目标边框的 4 个角中的 2 个角定位。
    * 布尔参数 `"overlay"` 控制是否将图像放在当前页面内容的顶部（前景，默认），或不放置（背景）。

    使用场景包括（但不限于）以下几种：

    1. 在当前文档的一系列页面上“盖章”，例如公司标志或水印。
    2. 将任意输入页面组合到一页输出页面中，支持“双面”或小册子打印（称为“4-up”，“n-up”）。
    3. 将（大）输入页面拆分为多个任意部分。这也叫做“海报化”，例如你可以水平和垂直拆分 A4 页面，将 4 个拆分部分放大打印到单独的 A4 页面上，最终得到原始页面的 A2 版本。


.. tab:: 英文

    The method displays an image of a ("source") page of another PDF document within a specified rectangle of the current ("containing", "target") page.

    * **In contrast** to :meth:`Page.insert_image`, this display is vector-based and hence remains accurate across zooming levels.
    * **Just like** :meth:`Page.insert_image`, the size of the display is adjusted to the given rectangle.

    The following variations of the display are currently supported:

    * Bool parameter `"keep_proportion"` controls whether to maintain the aspect ratio (default) or not.
        * Rectangle parameter `"clip"` restricts the visible part of the source page rectangle. Default is the full page.
    * float `"rotation"` rotates the display by an arbitrary angle (degrees). If the angle is not an integer multiple of 90, only 2 of the 4 corners may be positioned on the target border if also `"keep_proportion"` is true.
    * Bool parameter `"overlay"` controls whether to put the image on top (foreground, default) of current page content or not (background).

    Use cases include (but are not limited to) the following:

    1. "Stamp" a series of pages of the current document with the same image, like a company logo or a watermark.
    2. Combine arbitrary input pages into one output page to support “booklet” or double-sided printing (known as "4-up", "n-up").
    3. Split up (large) input pages into several arbitrary pieces. This is also called “posterization”, because you e.g. can split an A4 page horizontally and vertically, print the 4 pieces enlarged to separate A4 pages, and end up with an A2 version of your original page.

技术实施
~~~~~~~~~~~~~~~~~~~~~~~~~

Technical Implementation

.. tab:: 中文

    这使用了 PDF **“Form XObjects”**，请参见 :ref:`AdobeManual` 第 217 页的第 8.10 节。在执行 :meth:`Page.show_pdf_page` 时，发生以下几件事：

    1. 将源文档中源页面的 :data:`resources` 和 :data:`contents` 对象复制到目标文档中，共同创建一个新的 **Form XObject**，该对象具有以下属性。PDF :data:`xref` 编号由方法返回。

        a. `/BBox` 等于源页面的 `/Mediabox`  
        b. `/Matrix` 等于单位矩阵。  
        c. `/Resources` 等于源页面的资源。这涉及到对其他嵌套对象（包括字体、图像等）的“深拷贝”。此处的复杂性由 MuPDF 的 grafting [#f2]_ 技术函数覆盖。  
        d. 这是一个流对象类型，其流是源页面的 :data:`contents` 对象的组合数据的精确拷贝。

        该 Form XObject 只会在显示源页面时执行一次。随后的相同源页面显示将跳过此步骤，并仅创建指向该对象的“指针” Form XObjects（在下一步中完成）。

    2. 然后创建第二个 **Form XObject**，目标页面使用它来调用显示。该对象具有以下属性：

        a. `/BBox` 等于源页面的 `/CropBox` （或 `"clip"`）。  
        b. `/Matrix` 表示将 `/BBox` 映射到目标矩形。  
        c. `/XObject` 通过固定名称 `fullpage` 引用先前的 Form XObject。  
        d. 该对象的流包含一个固定语句：`/fullpage Do`。  
        e. 如果方法的 `"oc"` 参数给出，则其值被分配给此 Form XObject 的 `/OC`。

    3. 现在，目标页面的 :data:`resources` 和 :data:`contents` 对象被修改如下：

        a. 在目标页面的 `/Resources` 中的 `/XObject` 字典中添加一个条目，名称为 `fzFrm<n>` （其中 n 选择使该条目在页面中唯一）。  
        b. 根据 `"overlay"` 的值，向页面的 `/Contents` 数组中添加一个新对象，包含语句 `q /fzFrm<n> Do Q`。

    这种设计方法确保：

    1. （可能很大的）源页面只会复制一次到目标 PDF。每个目标页面仅创建小的“指针” Form XObject 对象来显示源页面。  
    2. 每个引用的目标页面可以有自己的 `"oc"` 参数，单独控制源页面的可见性。


.. tab:: 英文

    This is done using PDF **"Form XObjects"**, see section 8.10 on page 217 of :ref:`AdobeManual`. On execution of a :meth:`Page.show_pdf_page`, the following things happen:

        1. The :data:`resources` and :data:`contents` objects of source page in source document are copied over to the target document, jointly creating a new **Form XObject** with the following properties. The PDF :data:`xref` number of this object is returned by the method.

            a. `/BBox` equals `/Mediabox` of the source page
            b. `/Matrix` equals the identity matrix.
            c. `/Resources` equals that of the source page. This involves a “deep-copy” of hierarchically nested other objects (including fonts, images, etc.). The complexity involved here is covered by MuPDF's grafting [#f1]_ technique functions.
            d. This is a stream object type, and its stream is an exact copy of the combined data of the source page's :data:`contents` objects.

            This Form XObject is only executed once per shown source page. Subsequent displays of the same source page will skip this step and only create "pointer" Form XObjects (done in next step) to this object.

        2. A second **Form XObject** is then created which the target page uses to invoke the display. This object has the following properties:

            a. `/BBox` equals the `/CropBox` of the source page (or `"clip"`).
            b. `/Matrix` represents the mapping of `/BBox` to the target rectangle.
            c. `/XObject` references the previous Form XObject via the fixed name `fullpage`.
            d. The stream of this object contains exactly one fixed statement: `/fullpage Do`.
            e. If the method's `"oc"` argument is given, its value is assigned to this Form XObject as `/OC`.

        3. The :data:`resources` and :data:`contents` objects of the target page are now modified as follows.

            a. Add an entry to the `/XObject` dictionary of `/Resources` with the name `fzFrm<n>` (with n chosen such that this entry is unique on the page).
            b. Depending on `"overlay"`, prepend or append a new object to the page's `/Contents` array, containing the statement `q /fzFrm<n> Do Q`.

    This design approach ensures that:

    1. The (potentially large) source page is only copied once to the target PDF. Only small "pointer" Form XObjects objects are created per each target page to show the source page.
    2. Each referring target page can have its own `"oc"` parameter to control the source page's visibility individually.



.. _RedirectMessages:

诊断
----------------------------

Diagnostics

.. _Messages:

|PyMuPDF| 消息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|PyMuPDF| messages

.. tab:: 中文

    |PyMuPDF| 有一个消息系统用于显示文本诊断信息。

    默认情况下，消息会写入 `sys.stdout` 。可以通过以下两种方式进行控制：

    * 在 |PyMuPDF| 导入之前设置环境变量 `PYMUPDF_MESSAGE`。

    * 调用 `set_messages()`：


.. tab:: 英文

    |PyMuPDF| has a Message system for showing text diagnostics.

    By default messages are written to `sys.stdout`. This can be controlled in
    two ways:

    * Set environment variable `PYMUPDF_MESSAGE` before |PyMuPDF| is imported.

    * Call `set_messages()`:


MuPDF 错误和警告
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

MuPDF errors and warnings

.. tab:: 中文

    MuPDF 会生成文本错误和警告信息。

    * 这些错误和警告会附加到一个内部列表中，可以通过 `Tools.mupdf_warnings()` 访问。还可以参考 `Tools.reset_mupdf_warnings()`。

    * 默认情况下，这些错误和警告也会发送到 |PyMuPDF| 的消息系统。

        * 可以通过 `mupdf_display_errors()` 和 `mupdf_display_warnings()` 控制。

        * 这些消息会分别以 `MuPDF error:` 和 `MuPDF warning:` 为前缀。

    一些 MuPDF 错误可能会导致 Python 异常。

    **可恢复错误** 的示例输出。我们正在打开一个损坏的 PDF，但 MuPDF 能够修复它并提供一些关于发生了什么的信息。接着我们演示如何查看文档是否可以稍后增量保存。此时检查 `Document.is_dirty` 属性也表明在 `pymupdf.open` 时文档需要修复：

    >>> import pymupdf
    >>> doc = pymupdf.open("damaged-file.pdf")  # 会导致 sys.stderr 输出：
    mupdf: cannot find startxref
    >>> print(pymupdf.TOOLS.mupdf_warnings())  # 检查是否有更多信息：
    cannot find startxref
    trying to repair broken xref
    repairing PDF document
    object missing 'endobj' token
    >>> doc.can_save_incrementally()  # 这是预期的：
    False
    >>> # 以下表示是否有更新
    >>> # 因为修复操作，当前文档已更改：
    >>> doc.is_dirty
    True
    >>> # 尽管如此，文档已经被创建：
    >>> doc
    pymupdf.Document('damaged-file.pdf')
    >>> # 现在知道任何保存操作都必须发生在新文件中

    **不可恢复错误** 的示例输出：

    >>> import pymupdf
    >>> doc = pymupdf.open("does-not-exist.pdf")
    mupdf: cannot open does-not-exist.pdf: No such file or directory
    Traceback (most recent call last):
      File "<pyshell#1>", line 1, in <module>
        doc = pymupdf.open("does-not-exist.pdf")
      File "C:\Users\Jorj\AppData\Local\Programs\Python\Python37\lib\site-packages\fitz\pymupdf.py", line 2200, in __init__
        _pymupdf.Document_swiginit(self, _pymupdf.new_Document(filename, stream, filetype, rect, width, height, fontsize))
    RuntimeError: cannot open does-not-exist.pdf: No such file or directory
    >>> 


.. tab:: 英文

    MuPDF generates text errors and warnings.

    * These errors and warnings are appended to an internal list, accessible with `Tools.mupdf_warnings()`. Also see `Tools.reset_mupdf_warnings()`.

    * By default these errors and warnings are also sent to the |PyMuPDF| message system.
      
      * This can be controlled with `mupdf_display_errors()` and `mupdf_display_warnings()`.
      
      * These messages are prefixed with `MuPDF error:` and `MuPDF warning:` respectively.
      
    Some MuPDF errors may lead to Python exceptions.

    Example output for a **recoverable error**. We are opening a damaged PDF, but MuPDF is able to repair it and gives us a little information on what happened. Then we illustrate how to find out whether the document can later be saved incrementally. Checking the :attr:`Document.is_dirty` attribute at this point also indicates that during `pymupdf.open` the document had to be repaired:

    >>> import pymupdf
    >>> doc = pymupdf.open("damaged-file.pdf")  # leads to a sys.stderr message:
    mupdf: cannot find startxref
    >>> print(pymupdf.TOOLS.mupdf_warnings())  # check if there is more info:
    cannot find startxref
    trying to repair broken xref
    repairing PDF document
    object missing 'endobj' token
    >>> doc.can_save_incrementally()  # this is to be expected:
    False
    >>> # the following indicates whether there are updates so far
    >>> # this is the case because of the repair actions:
    >>> doc.is_dirty
    True
    >>> # the document has nevertheless been created:
    >>> doc
    pymupdf.Document('damaged-file.pdf')
    >>> # we now know that any save must occur to a new file

    Example output for an **unrecoverable error**:

    >>> import pymupdf
    >>> doc = pymupdf.open("does-not-exist.pdf")
    mupdf: cannot open does-not-exist.pdf: No such file or directory
    Traceback (most recent call last):
      File "<pyshell#1>", line 1, in <module>
        doc = pymupdf.open("does-not-exist.pdf")
      File "C:\Users\Jorj\AppData\Local\Programs\Python\Python37\lib\site-packages\fitz\pymupdf.py", line 2200, in __init__
        _pymupdf.Document_swiginit(self, _pymupdf.new_Document(filename, stream, filetype, rect, width, height, fontsize))
    RuntimeError: cannot open does-not-exist.pdf: No such file or directory
    >>>



.. _Coordinates:

坐标
--------------------------------------------

Coordinates

.. tab:: 中文

    这是本文件中最常用的术语之一。 **坐标** 通常指一对数字 `(x, y)`，表示某个位置，比如矩形的角落 (:ref:`Rect`)、一个 :ref:`Point` 等等。这两个值通常是浮动数值，但也有一些对象（比如图像）只允许它们为整数。

    为了 *找到* 一个坐标的位置，我们还需要知道 **参考点**，即 ``x`` 和 ``y`` 的位置 —— 换句话说，我们必须知道位置 `(0, 0)` 的位置。一旦知道了 `(0, 0)` （原点）的定位，我们就可以谈论“坐标系”。

    在文档处理过程中，存在多种坐标系。例如，PDF 页面和由此创建的图像的坐标系是 **不同** 的。因此，我们需要某些方法来 *转换* 坐标系统（偶尔也需要反向转换）。这正是 :ref:`Matrix` 的任务。它是一个数学函数，类似于一个因子，可以与一个点或矩形相乘，从而得到另一个坐标系中的对应点/矩形。转换矩阵的逆矩阵可以用来恢复转换。就像乘以某个因子，比如 3，可以通过将结果除以 3（或者乘以 1/3）来恢复。

.. tab:: 英文

    This is one of the most frequently used terms in this documentation. A **coordinate** generally means a pair of numbers `(x, y)` referring to some location, like a corner of a rectangle (:ref:`Rect`), a :ref:`Point` and so forth. The two values usually are floats, but there a objects like images which only allow them to be integers.

    To actually *find* a coordinate's location, we also need to know the *reference* point for ``x`` and ``y`` - in other words, we must know where location `(0, 0)` is positioned. Once `(0, 0)` (the "origin") is known, we speak of a "coordinate system".

    Several coordinate systems exist in document processing. For instance, the coordinate systems of a PDF page and the image created from it are **different**. We therefore need ways to *transform* coordinates from one system to another (and also back occasionally). This is the task of a :ref:`Matrix`. It is a mathematical function which works much like a factor that can be "multiplied" with a point or rectangle to give us the corresponding point / rectangle in another coordinate system. The inverse of a transformation matrix can be used to revert the transformation. Much like multiplying by some factor, say 3, can be reverted by dividing the result by 3 (or multiplying it with 1/3).

坐标和图像
~~~~~~~~~~~~~~~~~~~~~~~

Coordinates and Images

.. tab:: 中文

    图像有一个整数坐标系。原点 `(0, 0)` 位于左上角。 ``x`` 值必须在 `range(width)` 范围内，``y`` 值必须在 `range(height)` 范围内。因此， ``y`` 值 *向下* 增加。对于每个图像，只有一个 **有限数量** 的坐标，即 `width * height`。图像中的位置也被称为“像素”。

    - 图像的 **实际大小** （例如打印时的厘米或英寸尺寸）取决于额外的信息：即“分辨率”。分辨率通常以 **DPI** （每英寸点数，或每英寸像素）来衡量。为了找到某个图像的打印尺寸，我们必须将其宽度和高度分别除以相应的 DPI 值（宽度和高度可能有不同的 DPI 值），从而得到相应的英寸数。


.. tab:: 英文

    Images have a coordinate system with integer coordinates. Origin `(0, 0)` is the top-left point. ``x`` values must be in `range(width)`, and ``y`` values in `range(height)`. Therefore, ``y`` values *increase* if we go *downwards*. For every image, there is only a **finite number** of coordinates, namely `width * height`. A location in an image is also called a "pixel".

    - How **large** an image will be (in centimeters or inches) when e.g. printed, depends on additional information: the "resolution". This is measured in **DPI** (dots per inch, or pixels per inch). To find the printed size of some image, we therefore must divide its width and its height by the corresponding DPI values (there may separate ones for width and for height) and will get the respective number of inches.


原点、点大小和 Y 轴
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Origin Point, Point Size and Y-Axis

.. tab:: 中文

    在 **PDF** 中，页面的原点 `(0, 0)` 位于其 **底部左侧** 。在 **MuPDF** 中，页面的原点 `(0, 0)` 位于其 **顶部左侧** 。

    .. image:: images/img-coordinate-space.png

    坐标是浮动数，单位为 **点** ，其中：

    - **1 点等于 1/72 英寸**。

    典型的文档页面尺寸包括 **ISO A4** 和 **Letter**。一个 **Letter** 页面尺寸为 **8.5 x 11 英寸**，对应 **612 x 792 点**。在 **PDF** 坐标系统中， **Letter** 页面的左上角坐标是 `(0, 792)`，因为 **y 轴向上**。因此我们知道，在 **MuPDF** 坐标系统中，右下角的坐标是 `(612, 792)` （而在 **PDF** 中，此坐标则为 `(612, 0)`）。

    - 理论上， **PDF** 页面上有 **无数个** 坐标位置。然而，实际中最多只需精确到小数点后五位即可获得合理的精度。

    - 在 **MuPDF** 中，支持多种文档格式—— **PDF** 只是其中的 **一个**。图像也被视为文档格式（通常只包含一个页面）。这也是 **MuPDF** 使用坐标系统，其中原点 `(0, 0)` 是任何文档页面的 **顶部左侧** 的原因之一。 **y 轴向下** ，这和图像一样。无论如何， **MuPDF** 中的坐标都是浮动数，与 **PDF** 中一致。

    - 例如，在 **MuPDF** （因此也是 **PyMuPDF**）中， `Rect(0, 0, 100, 100)` 是一个边长为 100 点的正方形（即 1.39 英寸或 3.53 厘米）。其左上角是原点。为了在 **PDF** 和 **MuPDF** 之间转换坐标系统，每个 :ref:`Page` 对象都有一个 :attr:`Page.transformation_matrix`。它的逆矩阵可以用来计算矩形的 PDF 坐标。通过这种方式，我们可以方便地发现， **MuPDF** 中的 `Rect(0, 0, 100, 100)` 与 **PDF** 中的 `Rect(0, 692, 100, 792)` 是相同的。以下是代码示例：

        >>> page = doc.new_page(width=612, height=792)  # 创建新的 Letter 页面
        >>> ptm = page.transformation_matrix
        >>> # ptm 的逆矩阵是 ~ptm
        >>> pymupdf.Rect(0, 0, 100, 100) * ~ptm
        Rect(0.0, 692.0, 100.0, 792.0)


.. tab:: 英文

    In **PDF**, the origin `(0, 0)` of a page is located at its **bottom-left point**. In **MuPDF**, the origin `(0, 0)` of a page is located at its **top-left point**.


    .. image:: images/img-coordinate-space.png

    Coordinates are float numbers and measured in **points**, where:

    - **one point equals 1/72 inches**.

    Typical document page sizes are **ISO A4** and **Letter**. A **Letter** page has a size of **8.5 x 11 inches**, corresponding to **612 x 792 points**. In the **PDF** coordinate system, the top-left point of a **Letter** page hence has the coordinate `(0, 792)` as **the y-axis points upwards**. Now we know our document size the **MuPDF** coordinate system for the bottom right would be coordinate `(612, 792)` (and for **PDF** this coordinate would then be `(612,0)`).

    - Theoretically, there are **infinitely many** coordinate positions on a **PDF** page. In practice however, at most the first 5 decimal places are sufficient for a reasonable precision.


    - In **MuPDF**, multiple document formats are supported - **PDF** just being one among **over a dozen others**. Images are also supported as documents in **MuPDF** (therefore having one page usually). This is one of the reasons why **MuPDF** uses a coordinate system with the origin `(0, 0)` being the **top-left** point of any document page. **The y-axis points downwards**, like with images. Coordinates in **MuPDF** in any case are floats, like in **PDF**.

    - A rectangle `Rect(0, 0, 100, 100)` for instance in **MuPDF** (and thus **PyMuPDF**) therefore is a square with edges of length 100 points (= 1.39 inches or 3.53 centimeters). Its top-left corner is the origin. To switch between the two coordinate systems **PDF** to **MuPDF**, every :ref:`Page` object has a :attr:`Page.transformation_matrix`. Its inverse can be used to compute a rectangle's PDF coordinates. In this way we can conveniently find that `Rect(0, 0, 100, 100)` in **MuPDF** is the same as `Rect(0, 692, 100, 792)` in **PDF**. See this code snippet::

        >>> page = doc.new_page(width=612, height=792)  # make new Letter page
        >>> ptm = page.transformation_matrix
        >>> # the inverse matrix of ptm is ~ptm
        >>> pymupdf.Rect(0, 0, 100, 100) * ~ptm
        Rect(0.0, 692.0, 100.0, 792.0)



.. rubric:: Footnotes

.. [#f1] MuPDF supports "deep-copying" objects between PDF documents. To avoid duplicate data in the target, it uses so-called "graftmaps", like a form of scratchpad: for each object to be copied, its :data:`xref` number is looked up in the graftmap. If found, copying is skipped. Otherwise, the new :data:`xref` is recorded and the copy takes place. PyMuPDF makes use of this technique in two places so far: :meth:`Document.insert_pdf` and :meth:`Page.show_pdf_page`. This process is fast and very efficient, because it prevents multiple copies of typically large and frequently referenced data, like images and fonts. However, you may still want to consider using garbage collection (option 4) in any of the following cases:

    1. The target PDF is not new / empty: grafting does not check for resources that already existed (e.g. images, fonts) in the target document before opening it.
    2. Using :meth:`Page.show_pdf_page` for more than one source document: each grafting occurs **within one source** PDF only, not across multiple. So if e.g. the same image exists in pages from different source PDFs, then this will not be detected until garbage collection.



.. [#f2] MuPDF 支持在 PDF 文档之间“深度复制”对象。为了避免在目标文档中重复数据，它使用所谓的“嫁接图”（graftmaps），类似于一种便签本：对于每个要复制的对象，会在嫁接图中查找其 :data:`xref` 编号。如果找到，则跳过复制。否则，会记录新的 :data:`xref` 并进行复制。PyMuPDF 到目前为止在两个地方使用了这种技术： :meth:`Document.insert_pdf` 和 :meth:`Page.show_pdf_page`。这一过程快速且高效，因为它可以防止对通常较大且经常引用的数据（如图像和字体）进行多次复制。然而，您仍然可能需要考虑在以下情况下使用垃圾回收（选项 4）：

    1. 目标 PDF 不是新建的/空的：嫁接不会检查目标文档中在打开之前已经存在的资源（如图像、字体）。
    2. 对于多个源文档使用 :meth:`Page.show_pdf_page`：每次嫁接仅在 **单个源** PDF 中进行，而不是跨多个源文档。所以如果相同的图像出现在来自不同源 PDF 的页面中，则直到垃圾回收时才会检测到这一点。


.. include:: footer.rst
