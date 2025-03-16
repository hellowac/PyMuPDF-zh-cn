.. include:: header.rst

.. _Tutorial:


=========
教程
=========

Tutorial

.. highlight:: python

.. tab:: 中文

    本教程将逐步向您展示如何在Python中使用|PyMuPDF|，即:title:`MuPDF`。

    由于:title:`MuPDF`不仅支持PDF格式，还支持XPS、OpenXPS、CBZ、CBR、FB2和EPUB格式，|PyMuPDF|同样支持这些格式 [#f4]_ 。然而，为了简洁起见，我们将仅讨论PDF文件。在仅支持PDF文件的地方，会明确指出这一点。


.. tab:: 英文

    This tutorial will show you the use of |PyMuPDF|, :title:`MuPDF` in :title:`Python`, step by step.

    Because :title:`MuPDF` supports not only PDF, but also XPS, OpenXPS, CBZ, CBR, FB2 and EPUB formats, so does PyMuPDF [#f1]_. Nevertheless, for the sake of brevity we will only talk about PDF files. At places where indeed only PDF files are supported, this will be mentioned explicitly.

导入绑定
==========================

Importing the Bindings

.. tab:: 中文

    MuPDF的Python绑定可以通过以下导入语句使用。我们还展示了如何检查您的版本：

        >>> import pymupdf
        >>> print(pymupdf.__doc__)
        PyMuPDF 1.16.0: MuPDF 1.16.0库的Python绑定。
        版本日期：2019-07-28 07:30:14。
        为Python 3.7在win32 (64位) 平台上构建。


.. tab:: 英文

    The Python bindings to MuPDF are made available by this import statement. We also show here how your version can be checked::

        >>> import pymupdf
        >>> print(pymupdf.__doc__)
        PyMuPDF 1.16.0: Python bindings for the MuPDF 1.16.0 library.
        Version date: 2019-07-28 07:30:14.
        Built for Python 3.7 on win32 (64-bit).


注意名称 *fitz*
--------------------------

Note on the Name *fitz*

.. tab:: 中文

    旧版本的 |PyMuPDF| 使用的 **Python** 导入名称为 `fitz`。新版本改用了 `pymupdf`，并提供了 `fitz` 作为后备，以便旧代码仍然能正常工作。

    之所以使用 `fitz` 这个名称，有一个历史原因：

    MuPDF的原始渲染库叫做 *Libart*。

    *"在Artifex软件公司收购MuPDF项目后，开发重点转向编写一个新的现代图形库，名为"Fitz"。Fitz最初是作为一个研发项目，打算替代老旧的Ghostscript图形库，但它最终成为了MuPDF的渲染引擎。"* (引用自 `Wikipedia <https://en.wikipedia.org/wiki/MuPDF>`_) 。

    .. note::

        如果安装了已经废弃的pypi.org包 `fitz`，则使用遗留名称 `fitz` 可能会失败；请参阅 :ref:`安装后问题`。


.. tab:: 英文

    Old versions of |PyMuPDF| had their **Python** import name as `fitz`. Newer versions use `pymupdf` instead, and offer `fitz` as a fallback so that old code will still work.

    The reason for the name `fitz` is a historical curiosity:

    The original rendering library for MuPDF was called *Libart*.

    *"After Artifex Software acquired the MuPDF project, the development focus shifted on writing a new modern graphics library called "Fitz". Fitz was originally intended as an R&D project to replace the aging Ghostscript graphics library, but has instead become the rendering engine powering MuPDF."* (Quoted from `Wikipedia <https://en.wikipedia.org/wiki/MuPDF>`_).

    .. note::

        Use of legacy name `fitz` can fail if defunct pypi.org package `fitz` is installed; see :ref:`problems-after-installation`.


.. _Tutorial_Opening_a_Document:

打开文档
======================

Opening a Document

.. tab:: 中文

    要访问一个 :ref:`支持的文档<Supported_File_Types>`，必须使用以下语句打开：

        doc = pymupdf.open(filename)  # 或 pymupdf.Document(filename)

    这会创建一个 :ref:`Document` 对象 *doc*。*filename* 必须是一个 Python 字符串 (或 `pathlib.Path`) ，指定一个现有文件的名称。

    还可以从内存数据打开文档，或者创建一个新的空 PDF。有关详细信息，请参阅 :ref:`Document`。你还可以将 :ref:`Document` 作为 *上下文管理器* 使用。

    一个文档包含许多属性和函数。其中包括元信息 (如 "作者" 或 "主题") 、总页数、目录和加密信息。


.. tab:: 英文

    To access a :ref:`supported document<Supported_File_Types>`, it must be opened with the following statement::

        doc = pymupdf.open(filename)  # or pymupdf.Document(filename)

    This creates the :ref:`Document` object *doc*. *filename* must be a Python string (or a `pathlib.Path`) specifying the name of an existing file.

    It is also possible to open a document from memory data, or to create a new, empty PDF. See :ref:`Document` for details. You can also use :ref:`Document` as a *context manager*.

    A document contains many attributes and functions. Among them are meta information (like "author" or "subject"), number of total pages, outline and encryption information.

一些 :ref:`Document` 方法和属性
=============================================

Some :ref:`Document` Methods and Attributes

.. tab:: 中文

    =========================== ==========================================
    **Method / Attribute**      **Description**
    =========================== ==========================================
    :attr:`Document.page_count`  页数 (*int*)                        
    :attr:`Document.metadata`   元数据 (*dict*)
    :meth:`Document.get_toc`    获取目录 (*list*)
    :meth:`Document.load_page`   加载一个 :ref:`Page`
    =========================== ==========================================

.. tab:: 英文

    =========================== ==========================================
    **Method / Attribute**      **Description**
    =========================== ==========================================
    :attr:`Document.page_count`  the number of pages (*int*)
    :attr:`Document.metadata`   the metadata (*dict*)
    :meth:`Document.get_toc`    get the table of contents (*list*)
    :meth:`Document.load_page`   read a :ref:`Page`
    =========================== ==========================================

访问元数据
========================

Accessing Meta Data

.. tab:: 中文

    PyMuPDF 完全支持标准元数据。 :attr:`Document.metadata` 是一个 Python 字典，包含以下键。它适用于 **所有文档类型**，尽管并非所有条目总是包含数据。有关它们的含义和格式的详细信息，请参考相应的手册，例如 PDF 的 :ref:`AdobeManual`。进一步的信息也可以在 :ref:`Document` 章节中找到。元数据字段是字符串类型，如果没有特别说明，则为 *None* 。还请注意，并非所有字段总是包含有意义的数据——即使它们不是 *None*。

    ============== =================================
    键               值
    ============== =================================
    producer         生产者 (生产软件) 
    format           格式：'PDF-1.4', 'EPUB' 等
    encryption       如果有的话，使用的加密方法
    author           作者
    modDate          最后修改日期
    keywords         关键词
    title            标题
    creationDate     创建日期
    creator          创建应用程序
    subject          主题
    ============== =================================

    .. note:: 
        
        除了这些标准元数据之外， **PDF 文档** 从 PDF 版本 1.4 开始，还可能包含所谓的  *"元数据流"*  (参见 :data:`stream`) 。这些流中的信息以 XML 编码。PyMuPDF 特意不包含用于此目的的 XML 组件 (:ref:`PyMuPDF Xml class<Xml>` 是一个用于访问 :ref:`Story` 对象 DOM 内容的帮助类) ，因此我们不直接支持访问其中包含的信息。但你可以提取整个流，使用像 `lxml`_ 这样的包来检查或修改它，然后将结果存回 PDF。如果需要，你还可以完全删除这些数据。

    .. note:: 
        
        在代码库中有两个实用脚本，分别是 `metadata import (PDF only)`_ 和 `metadata export`_ ，它们分别从 CSV 文件导入或导出元数据。


.. tab:: 英文

    PyMuPDF fully supports standard metadata. :attr:`Document.metadata` is a Python dictionary with the following keys. It is available for **all document types**, though not all entries may always contain data. For details of their meanings and formats consult the respective manuals, e.g. :ref:`AdobeManual` for PDF. Further information can also be found in chapter :ref:`Document`. The meta data fields are strings or *None* if not otherwise indicated. Also be aware that not all of them always contain meaningful data -- even if they are not *None*.

    ============== =================================
    Key            Value
    ============== =================================
    producer       producer (producing software)
    format         format: 'PDF-1.4', 'EPUB', etc.
    encryption     encryption method used if any
    author         author
    modDate        date of last modification
    keywords       keywords
    title          title
    creationDate   date of creation
    creator        creating application
    subject        subject
    ============== =================================

    .. note:: Apart from these standard metadata, **PDF documents** starting from PDF version 1.4 may also contain so-called *"metadata streams"* (see also :data:`stream`). Information in such streams is coded in XML. PyMuPDF deliberately contains no XML components for this purpose (the :ref:`PyMuPDF Xml class<Xml>` is a helper class intended to access the DOM content of a :ref:`Story` object), so we do not directly support access to information contained therein. But you can extract the stream as a whole, inspect or modify it using a package like `lxml`_ and then store the result back into the PDF. If you want, you can also delete this data altogether.

    .. note:: There are two utility scripts in the repository that `metadata import (PDF only)`_ resp. `metadata export`_ metadata from resp. to CSV files.

使用大纲
=========================

Working with Outlines

.. tab:: 中文

    获取文档所有大纲 (也叫“书签”) 的最简单方法是加载其 *目录*::

        toc = doc.get_toc()

    这将返回一个 Python 列表 *[[lvl, title, page, ...], ...]*，其结构类似于书籍中常见的目录。

    *lvl* 是条目的层级 (从 1 开始) ，*title* 是条目的标题，*page* 是页码 (以 1 为基准！) 。其他参数描述书签目标的详细信息。

    .. note:: 

        在代码库中有两个实用脚本，分别是 `toc import (仅限 PDF)`_ 和 `toc export`_，它们分别从 CSV 文件导入或导出目录。


.. tab:: 英文

    The easiest way to get all outlines (also called "bookmarks") of a document, is by loading its *table of contents*::

        toc = doc.get_toc()

    This will return a Python list of lists *[[lvl, title, page, ...], ...]* which looks much like a conventional table of contents found in books.

    *lvl* is the hierarchy level of the entry (starting from 1), *title* is the entry's title, and *page* the page number (1-based!). Other parameters describe details of the bookmark target.

    .. note:: 
        
        There are two utility scripts in the repository that `toc import (PDF only)`_ resp. `toc export`_ table of contents from resp. to CSV files.

使用页面
======================

Working with Pages

.. tab:: 中文

    :ref:`Page` 处理是 MuPDF 功能的核心。

    * 您可以将页面渲染为栅格图像或矢量图像 (SVG) ，并可选择性地进行缩放、旋转、平移或剪切。
    * 您可以提取页面的文本和图像，支持多种格式并且可以搜索文本字符串。
    * 对于 PDF 文档，还可以使用更多方法向页面添加文本或图像。

    首先，必须创建一个 :ref:`Page`。这是 :ref:`Document` 的一个方法::

        page = doc.load_page(pno)  # 加载文档的页面 'pno' (从 0 开始) 
        page = doc[pno]  # 简写形式

    这里的任何整数 `-∞ < pno < page_count` 都是有效的。负数表示从文档末尾向回计数，因此 *doc[-1]* 是最后一页，类似于 Python 序列。

    更高级的方式是将文档作为其页面的 **迭代器** 使用::

        for page in doc:
            # 对 'page' 做一些操作

        # ... 或者反向读取
        for page in reversed(doc):
            # 对 'page' 做一些操作

        # ... 甚至可以使用切片
        for page in doc.pages(start, stop, step):
            # 对 'page' 做一些操作

    一旦获取到页面，以下是您通常会对页面执行的操作：


.. tab:: 英文

    :ref:`Page` handling is at the core of MuPDF's functionality.

    * You can render a page into a raster or vector (SVG) image, optionally zooming, rotating, shifting or shearing it.
    * You can extract a page's text and images in many formats and search for text strings.
    * For PDF documents many more methods are available to add text or images to pages.

    First, a :ref:`Page` must be created. This is a method of :ref:`Document`::

        page = doc.load_page(pno)  # loads page number 'pno' of the document (0-based)
        page = doc[pno]  # the short form

    Any integer `-∞ < pno < page_count` is possible here. Negative numbers count backwards from the end, so *doc[-1]* is the last page, like with Python sequences.

    Some more advanced way would be using the document as an **iterator** over its pages::

        for page in doc:
            # do something with 'page'

        # ... or read backwards
        for page in reversed(doc):
            # do something with 'page'

        # ... or even use 'slicing'
        for page in doc.pages(start, stop, step):
            # do something with 'page'


    Once you have your page, here is what you would typically do with it:

检查页面的链接、注释或表单字段
-----------------------------------------------------------

Inspecting the Links, Annotations or Form Fields of a Page

.. tab:: 中文

    链接在文档中显示为“热区”，当文档在某些查看软件中显示时，点击这些区域时，通常会跳转到该热区所编码的目标。当光标显示为手形时，通常会跳转到该目标。以下是获取所有链接的方法::

        # 获取页面上的所有链接
        links = page.get_links()

    *links* 是一个 Python 字典列表。详细信息请参见 :meth:`Page.get_links`。

    您还可以使用迭代器逐个获取链接::

        for link in page.links():
            # 对 'link' 做一些操作

    如果处理的是 PDF 文档页面，页面上可能还存在注释 (:ref:`Annot`) 或表单字段 (:ref:`Widget`)，每种都有自己的迭代器::

        for annot in page.annots():
            # 对 'annot' 做一些操作

        for field in page.widgets():
            # 对 'field' 做一些操作


.. tab:: 英文

    Links are shown as "hot areas" when a document is displayed with some viewer software. If you click while your cursor shows a hand symbol, you will usually be taken to the target that is encoded in that hot area. Here is how to get all links::

        # get all links on a page
        links = page.get_links()

    *links* is a Python list of dictionaries. For details see :meth:`Page.get_links`.

    You can also use an iterator which emits one link at a time::

        for link in page.links():
            # do something with 'link'

    If dealing with a PDF document page, there may also exist annotations (:ref:`Annot`) or form fields (:ref:`Widget`), each of which have their own iterators::

        for annot in page.annots():
            # do something with 'annot'

        for field in page.widgets():
            # do something with 'field'


渲染页面
-----------------------

Rendering a Page

.. tab:: 中文

    此示例创建页面内容的 **光栅** 图像::

        pix = page.get_pixmap()

    *pix* 是一个 :ref:`Pixmap` 对象，其中 (在这种情况下) 包含页面的 **RGB** 图像，准备好用于多种用途。方法 :meth:`Page.get_pixmap` 提供了许多控制图像的变体：分辨率 / DPI、色彩空间 (例如生成灰度图像或具有减色方案的图像) 、透明度、旋转、镜像、平移、剪切等。例如：要创建一个 **RGBA** 图像 (即包含 alpha 通道) ，请指定 *pix = page.get_pixmap(alpha=True)*。

    一个 :ref:`Pixmap` 包含许多方法和属性，下面会提到其中的一些。包括 *width*、*height* (每个以像素为单位) 和 *stride* (每一水平图像行的字节数) 。属性 *samples* 表示表示图像数据的矩形字节区域 (一个 Python *bytes* 对象) 。

    .. note::

        你还可以通过使用 :meth:`Page.get_svg_image` 创建页面的 **矢量** 图像。详细信息请参阅 `Vector Image Support page`_。


.. tab:: 英文

    This example creates a **raster** image of a page's content::

        pix = page.get_pixmap()

    *pix* is a :ref:`Pixmap` object which (in this case) contains an **RGB** image of the page, ready to be used for many purposes. Method :meth:`Page.get_pixmap` offers lots of variations for controlling the image: resolution / DPI, colorspace (e.g. to produce a grayscale image or an image with a subtractive color scheme), transparency, rotation, mirroring, shifting, shearing, etc. For example: to create an **RGBA** image (i.e. containing an alpha channel), specify *pix = page.get_pixmap(alpha=True)*.

    A :ref:`Pixmap` contains a number of methods and attributes which are referenced below. Among them are the integers *width*, *height* (each in pixels) and *stride* (number of bytes of one horizontal image line). Attribute *samples* represents a rectangular area of bytes representing the image data (a Python *bytes* object).

    .. note:: 
        
        You can also create a **vector** image of a page by using :meth:`Page.get_svg_image`. Refer to this `Vector Image Support page`_ for details.

将页面图像保存在文件中
-----------------------------------

Saving the Page Image in a File

.. tab:: 中文

    我们可以简单地将图像存储在 PNG 文件中::

        pix.save("page-%i.png" % page.number)

.. tab:: 英文

    We can simply store the image in a PNG file::

        pix.save("page-%i.png" % page.number)

Displaying the Image in GUIs
-------------------------------------------

.. tab:: 中文

    我们还可以在 GUI 对话框管理器中使用它。:attr:`Pixmap.samples` 将所有像素的字节区域表示为 Python 字节对象。以下是一些示例，更多内容请参阅 `examples`_ 目录。

.. tab:: 英文
    
    We can also use it in GUI dialog managers. :attr:`Pixmap.samples` represents an area of bytes of all the pixels as a Python bytes object. Here are some examples, find more in the `examples`_ directory.

wxPython
~~~~~~~~~~~~~

wxPython

.. tab:: 中文

    请参阅其文档以了解对 RGB(A) 像素图的调整，以及可能针对您的 wxPython 版本的具体信息::

        if pix.alpha:
            bitmap = wx.Bitmap.FromBufferRGBA(pix.width, pix.height, pix.samples)
        else:
            bitmap = wx.Bitmap.FromBuffer(pix.width, pix.height, pix.samples)

.. tab:: 英文

    Consult their documentation for adjustments to RGB(A) pixmaps and, potentially, specifics for your wxPython release::

        if pix.alpha:
            bitmap = wx.Bitmap.FromBufferRGBA(pix.width, pix.height, pix.samples)
        else:
            bitmap = wx.Bitmap.FromBuffer(pix.width, pix.height, pix.samples)

Tkinter
~~~~~~~~~~

Tkinter

.. tab:: 中文

    请参阅 `Pillow 文档`_ 的第 3.19 节::

        from PIL import Image, ImageTk

        # 根据 alpha 设置模式
        mode = "RGBA" if pix.alpha else "RGB"
        img = Image.frombytes(mode, [pix.width, pix.height], pix.samples)
        tkimg = ImageTk.PhotoImage(img)

    以下代码 **避免使用 Pillow**::

        # 如果存在 alpha，则移除它
        pix1 = pymupdf.Pixmap(pix, 0) if pix.alpha else pix  # PPM 不支持透明度
        imgdata = pix1.tobytes("ppm")  # 非常快速！
        tkimg = tkinter.PhotoImage(data = imgdata)

    如果你在寻找一个完整的 Tkinter 脚本，用于分页浏览 **任何支持的** 文档，`这里是代码!`_ 它还可以对页面进行缩放，并且可以在 Python 2 或 3 下运行。它需要非常方便的 `PySimpleGUI`_ 纯 Python 包。


.. tab:: 英文

    Please also see section 3.19 of the `Pillow documentation`_::

        from PIL import Image, ImageTk

        # set the mode depending on alpha
        mode = "RGBA" if pix.alpha else "RGB"
        img = Image.frombytes(mode, [pix.width, pix.height], pix.samples)
        tkimg = ImageTk.PhotoImage(img)

    The following **avoids using Pillow**::

        # remove alpha if present
        pix1 = pymupdf.Pixmap(pix, 0) if pix.alpha else pix  # PPM does not support transparency
        imgdata = pix1.tobytes("ppm")  # extremely fast!
        tkimg = tkinter.PhotoImage(data = imgdata)

    If you are looking for a complete Tkinter script paging through **any supported** document, `here it is!`_. It can also zoom into pages, and it runs under Python 2 or 3. It requires the extremely handy `PySimpleGUI`_ pure Python package.

PyQt4、PyQt5、PySide
~~~~~~~~~~~~~~~~~~~~~

PyQt4, PyQt5, PySide

.. tab:: 中文

    请参阅 `Pillow 文档`_ 的第 3.16 节::

        from PIL import Image, ImageQt

        # 根据 alpha 设置模式
        mode = "RGBA" if pix.alpha else "RGB"
        img = Image.frombytes(mode, [pix.width, pix.height], pix.samples)
        qtimg = ImageQt.ImageQt(img)

    同样，你也可以 **避免使用 Pillow**。幸运的是，Qt 的 `QImage` 支持原生 Python 指针，所以以下代码是创建 Qt 图像的推荐方式::

        from PyQt5.QtGui import QImage

        # 根据 alpha 设置正确的 QImage 格式
        fmt = QImage.Format_RGBA8888 if pix.alpha else QImage.Format_RGB888
        qtimg = QImage(pix.samples_ptr, pix.width, pix.height, fmt)


.. tab:: 英文

    Please also see section 3.16 of the `Pillow documentation`_::

        from PIL import Image, ImageQt

        # set the mode depending on alpha
        mode = "RGBA" if pix.alpha else "RGB"
        img = Image.frombytes(mode, [pix.width, pix.height], pix.samples)
        qtimg = ImageQt.ImageQt(img)

    Again, you also can get along **without using Pillow.** Qt's `QImage` luckily supports native Python pointers, so the following is the recommended way to create Qt images::

        from PyQt5.QtGui import QImage

        # set the correct QImage format depending on alpha
        fmt = QImage.Format_RGBA8888 if pix.alpha else QImage.Format_RGB888
        qtimg = QImage(pix.samples_ptr, pix.width, pix.height, fmt)


提取文本和图像
---------------------------

Extracting Text and Images

.. tab:: 中文

    我们还可以以多种不同的形式和详细级别提取页面上的所有文本、图像和其他信息::

        text = page.get_text(opt)

    使用以下字符串之一作为 *opt* 来获取不同的格式 [#f5]_：

    * **"text"**:  (默认) 纯文本，带有换行符。没有格式，没有文本位置详情，也没有图像。

    * **"blocks"**: 生成文本块 (即段落) 列表。

    * **"words"**: 生成单词列表 (不包含空格的字符串) 。

    * **"html"**: 创建页面的完整视觉版本，包括任何图像。可以使用浏览器显示。

    * **"dict"** / **"json"**: 与 HTML 相同的信息级别，但以 Python 字典或 JSON 字符串的形式提供。有关其结构的详细信息，请参见 :meth:`TextPage.extractDICT`。

    * **"rawdict"** / **"rawjson"**: **"dict"** / **"json"** 的超集。它还提供字符详细信息，如 XML。有关其结构的详细信息，请参见 :meth:`TextPage.extractRAWDICT`。

    * **"xhtml"**: 作为文本版本的文本信息级别，但包括图像。也可以由浏览器显示。

    * **"xml"**: 不包含图像，但包含每个文本字符的完整位置和字体信息。使用 XML 模块进行解析。

    为了给您一个关于这些替代方案输出的概念，我们提供了文本示例提取。请参见 :ref:`附录1`。


.. tab:: 英文

    We can also extract all text, images and other information of a page in many different forms, and levels of detail::

        text = page.get_text(opt)

    Use one of the following strings for *opt* to obtain different formats [#f2]_:

    * **"text"**: (default) plain text with line breaks. No formatting, no text position details, no images.

    * **"blocks"**: generate a list of text blocks (= paragraphs).

    * **"words"**: generate a list of words (strings not containing spaces).

    * **"html"**: creates a full visual version of the page including any images. This can be displayed with your internet browser.

    * **"dict"** / **"json"**: same information level as HTML, but provided as a Python dictionary or resp. JSON string. See :meth:`TextPage.extractDICT` for details of its structure.

    * **"rawdict"** / **"rawjson"**: a super-set of **"dict"** / **"json"**. It additionally provides character detail information like XML. See :meth:`TextPage.extractRAWDICT` for details of its structure.

    * **"xhtml"**: text information level as the TEXT version but includes images. Can also be displayed by internet browsers.

    * **"xml"**: contains no images, but full position and font information down to each single text character. Use an XML module to interpret.

    To give you an idea about the output of these alternatives, we did text example extracts. See :ref:`Appendix1`.

搜索文本
-------------------

Searching for Text

.. tab:: 中文

    您可以确切地找到页面上某个文本字符串出现的位置::

        areas = page.search_for("mupdf")

    这将返回一个矩形列表 (参见 :ref:`Rect`) ，每个矩形包围一个 "mupdf" 字符串的出现位置 (不区分大小写) 。您可以使用这些信息，例如在 PDF 中高亮显示这些区域，或者创建文档的交叉引用。

    请务必查看 :ref:`合作` 章节，并查看示例程序 `demo.py`_ 和 `demo-lowlevel.py`_。其中包含了如何使用 :ref:`TextPage`、:ref:`Device` 和 :ref:`DisplayList` 类进行更直接的控制的详细信息，例如在性能考虑因素提示时使用。


.. tab:: 英文

    You can find out, exactly where on a page a certain text string appears::

        areas = page.search_for("mupdf")

    This delivers a list of rectangles (see :ref:`Rect`), each of which surrounds one occurrence of the string "mupdf" (case insensitive). You could use this information to e.g. highlight those areas (PDF only) or create a cross reference of the document.

    Please also do have a look at chapter :ref:`cooperation` and at demo programs `demo.py`_ and `demo-lowlevel.py`_. Among other things they contain details on how the :ref:`TextPage`, :ref:`Device` and :ref:`DisplayList` classes can be used for a more direct control, e.g. when performance considerations suggest it.



.. _WorkingWithStories:

:ref:`Story`：从 HTML 源生成 PDF
=========================================

Stories: Generating PDF from HTML Source

.. tab:: 中文

    :ref:`Story` 类是 PyMuPDF 版本 1.21.0 引入的新特性。它代表了对 MuPDF **"story"** 接口的支持。

    以下是来自 `Artifex`_ 的 Robin Watts 编写的书籍 `"MuPDF Explored"`_ 中的摘录：

    -----

    *故事提供了一种简单的方法来布局带有样式的内容，用于设备，比如文档写入器提供的设备 (...) 。故事的概念源自桌面出版，进而 (...) 来自报纸。如果你考虑传统的报纸布局，它将包含各种新闻文章 (故事) ，这些文章被布局到多个栏目中，可能跨多个页面。*

    *因此，MuPDF 使用故事来表示带有样式信息的文本流。故事的使用者随后可以提供一系列矩形，将故事布局到这些矩形中，定位后的文本可以被绘制到输出设备上。这将文本本身 (故事) 与文本应流入的区域 (布局) 分开。*

    -----

    .. note:: Story 的工作方式有点类似于互联网浏览器：它忠实地解析和渲染 HTML 超文本以及可选的样式表 (CSS) 。但它的 **输出是 PDF** — 而不是网页。

    创建 :ref:`Story` 时，会考虑最多来自三种不同信息来源的输入。所有这些项都是可选的。

    1. HTML 源代码，可以是 Python 字符串，或者 **由脚本创建**，使用 :ref:`Xml` 的方法。

    2. CSS (层叠样式表) 源代码，以 Python 字符串提供。CSS 可用于提供样式信息 (如文本字体大小、颜色等) ，就像网页一样。显然，这个字符串也可以从文件中读取。

    3. 当 DOM 引用图片或使用除标准 :ref:`Base-14-Fonts`、CJK 字体和 PyMuPDF 二进制生成的 NOTO 字体之外的文本字体时，必须使用 :ref:`Archive`。

    :ref:`API<Xml>` 允许完全从头创建 DOM，包括所需的样式信息。它也可以用来修改或扩展 **提供的** HTML：文本可以被删除或替换，或其样式可以被更改。还可以添加从数据库提取的文本，并填充类似模板的 HTML 文档。

    **不要求** 提供语法上完整的 HTML 文档：像 ``<b>Hello <i>World!</i></b>`` 这样的片段完全可以接受，并且许多/大多数语法错误会被自动修正。

    当 HTML 被认为完整后，它可以用于创建 PDF 文档。这是通过新的 :ref:`DocumentWriter` 类完成的。程序员调用其方法来创建一个新的空白页面，并将矩形传递给 :ref:`Story` 来填充它们。

    :ref:`Story` 会返回完成代码，指示是否还有内容待写。内容的哪一部分将落到哪个矩形或页面上，自动由 :ref:`Story` 本身决定 — 只能通过提供矩形来影响它。

    请参阅 :ref:`Stories recipes<RecipesStories>` 了解一些典型的用例。


.. tab:: 英文

    The :ref:`Story` class is a new feature of PyMuPDF version 1.21.0. It represents support for MuPDF's **"story"** interface.

    The following is a quote from the book `"MuPDF Explored"`_ by Robin Watts from `Artifex`_:

    -----

    *Stories provide a way to easily layout styled content for use with devices, such as those offered by Document Writers (...). The concept of a story comes from desktop publishing, which in turn (...) gets it from newspapers. If you consider a traditional newspaper layout, it will consist of various news articles (stories) that are laid out into multiple columns, possibly across multiple pages.*

    *Accordingly, MuPDF uses a story to represent a flow of text with styling information. The user of the story can then supply a sequence of rectangles into which the story will be laid out, and the positioned text can then be drawn to an output device. This keeps the concept of the text itself (the story) to be separated from the areas into which the text should be flowed (the layout).*

    -----

    .. note:: A Story works somewhat similar to an internet browser: It faithfully parses and renders HTML hypertext and also optional stylesheets (CSS). But its **output is a PDF** -- not web pages.


    When creating a :ref:`Story`, the input from up to three different information sources is taken into account. All these items are optional.

    1. HTML source code, either a Python string or **created by the script** using methods of :ref:`Xml`.

    2. CSS (Cascaded Style Sheet) source code, provided as a Python string. CSS can be used to provide styling information (text font size, color, etc.) like it would happen for web pages. Obviously, this string may also be read from a file.

    3. An :ref:`Archive` **must be used** whenever the DOM references images, or uses text fonts except the standard :ref:`Base-14-Fonts`, CJK fonts and the NOTO fonts generated into the PyMuPDF binary.


    The :ref:`API<Xml>` allows creating DOMs completely from scratch, including desired styling information. It can also be used to modify or extend **provided** HTML: text can be deleted or replaced, or its styling can be changed. Text -- for example extracted from databases -- can also be added and fill template-like HTML documents.

    It is **not required** to provide syntactically complete HTML documents: snippets like ``<b>Hello <i>World!</i></b>`` are fully accepted, and many / most syntax errors are automatically corrected.

    After the HTML is considered complete, it can be used to create a PDF document. This happens via the new :ref:`DocumentWriter` class. The programmer calls its methods to create a new empty page, and passes rectangles to the Story to fill them.

    The story in turn will return completion codes indicating whether or not more content is waiting to be written. Which part of the content will land in which rectangle or on which page is automatically determined by the story itself -- it cannot be influenced other than by providing the rectangles.

    Please see the :ref:`Stories recipes<RecipesStories>` for a number of typical use cases.


PDF 维护
==================

PDF Maintenance

.. tab:: 中文

    PDF 是唯一可以使用 PyMuPDF **修改** 的文档类型。其他文件类型都是只读的。

    然而，您可以将 **任何文档**  (包括图像) 转换为 PDF，然后对转换结果应用所有 PyMuPDF 功能。有关更多信息，请参阅 :meth:`Document.convert_to_pdf`，并查看演示脚本 `pdf-converter.py`_，该脚本可以将任何 :ref:`supported document<Supported_File_Types>` 转换为 PDF。

    :meth:`Document.save()` 始终将 PDF 以当前 (可能已修改) 状态保存到磁盘。

    您通常可以选择将文件保存到新文件，或者将修改追加到现有文件 (“增量保存”) ，后者通常非常快。

    以下描述了您可以如何操作 PDF 文档。此描述并非完整：更多内容可以在接下来的章节中找到。


.. tab:: 英文

    PDFs are the only document type that can be **modified** using PyMuPDF. Other file types are read-only.

    However, you can convert **any document** (including images) to a PDF and then apply all PyMuPDF features to the conversion result. Find out more here :meth:`Document.convert_to_pdf`, and also look at the demo script `pdf-converter.py`_ which can convert any :ref:`supported document<Supported_File_Types>` to PDF.

    :meth:`Document.save()` always stores a PDF in its current (potentially modified) state on disk.

    You normally can choose whether to save to a new file, or just append your modifications to the existing one ("incremental save"), which often is very much faster.

    The following describes ways how you can manipulate PDF documents. This description is by no means complete: much more can be found in the following chapters.

修改、创建、重新排列和删除页面
-------------------------------------------------------

Modifying, Creating, Re-arranging and Deleting Pages

.. tab:: 中文

    PDF 的 **页面树** (描述所有页面的结构) 可以通过多种方式进行操作：

    :meth:`Document.delete_page` 和 :meth:`Document.delete_pages` 用于删除页面。

    :meth:`Document.copy_page`、:meth:`Document.fullcopy_page` 和 :meth:`Document.move_page` 用于在同一文档内复制或移动页面。

    :meth:`Document.select` 允许根据指定的页面序列缩小 PDF。参数是一个包含要保留的页面编号的序列 [#f6]_。这些整数必须满足 *0 <= i < page_count* 。执行后，未包含在此列表中的所有页面都会被删除。剩余的页面将按照指定顺序出现，并且可以重复出现多次。

    因此，您可以轻松创建新的 PDF 文件，例如：

    * 仅包含前 10 页或后 10 页，
    * 仅包含奇数页或偶数页 (用于双面打印) ，
    * 仅包含 (或不包含) 特定文本的页面，
    * 反转页面顺序，……

    ... 以及任何您能想到的操作。

    保存的新文档仍然会包含有效的链接、注释和书签 (即指向选定页面或外部资源) 。

    :meth:`Document.insert_page` 和 :meth:`Document.new_page` 可用于插入新页面。

    此外，页面本身还可以通过多种方法进行修改，例如页面旋转、注释和链接管理、文本和图像插入等。


.. tab:: 英文

    There are several ways to manipulate the so-called **page tree** (a structure describing all the pages) of a PDF:

    :meth:`Document.delete_page` and :meth:`Document.delete_pages` delete pages.

    :meth:`Document.copy_page`, :meth:`Document.fullcopy_page` and :meth:`Document.move_page` copy or move a page to other locations within the same document.

    :meth:`Document.select` shrinks a PDF down to selected pages. Parameter is a sequence [#f3]_ of the page numbers that you want to keep. These integers must all be in range *0 <= i < page_count*. When executed, all pages **missing** in this list will be deleted. Remaining pages will occur **in the sequence and as many times (!) as you specify them**.

    So you can easily create new PDFs with

    * the first or last 10 pages,
    * only the odd or only the even pages (for doing double-sided printing),
    * pages that **do** or **don't** contain a given text,
    * reverse the page sequence, ...

    ... whatever you can think of.

    The saved new document will contain links, annotations and bookmarks that are still valid (i.a.w. either pointing to a selected page or to some external resource).

    :meth:`Document.insert_page` and :meth:`Document.new_page` insert new pages.

    Pages themselves can moreover be modified by a range of methods (e.g. page rotation, annotation and link maintenance, text and image insertion).

合并和拆分 PDF 文档
------------------------------------

Joining and Splitting PDF Documents

.. tab:: 中文

    :meth:`Document.insert_pdf` 方法用于 **在不同** PDF 文档之间复制页面。以下是一个简单的 **合并** 示例 (假设 *doc1* 和 *doc2* 是已打开的 PDF 文档) ::

        # 将 doc2 的所有页面追加到 doc1 末尾
        doc1.insert_pdf(doc2)

    下面是一个 **拆分** *doc1* 的示例。它创建一个新文档，其中包含 *doc1* 的前 10 页和后 10 页::

        doc2 = pymupdf.open()                 # 创建新的空 PDF
        doc2.insert_pdf(doc1, to_page=9)  # 复制前 10 页
        doc2.insert_pdf(doc1, from_page=len(doc1) - 10)  # 复制后 10 页
        doc2.save("first-and-last-10.pdf")

    更多信息请参考 :ref:`Document` 章节，同时也可以查看 `PDFjoiner.py`_ 示例。


.. tab:: 英文

    Method :meth:`Document.insert_pdf` copies pages **between different** PDF documents. Here is a simple **joiner** example (*doc1* and *doc2* being opened PDFs)::

        # append complete doc2 to the end of doc1
        doc1.insert_pdf(doc2)

    Here is a snippet that **splits** *doc1*. It creates a new document of its first and its last 10 pages::

        doc2 = pymupdf.open()                 # new empty PDF
        doc2.insert_pdf(doc1, to_page = 9)  # first 10 pages
        doc2.insert_pdf(doc1, from_page = len(doc1) - 10) # last 10 pages
        doc2.save("first-and-last-10.pdf")

    More can be found in the :ref:`Document` chapter. Also have a look at `PDFjoiner.py`_.

嵌入数据
---------------

Embedding Data

.. tab:: 中文

    PDF 文档可以作为容器存储任意数据 (如可执行文件、其他 PDF、文本或二进制文件等) ，类似于 ZIP 压缩包。

    PyMuPDF 通过 :ref:`Document` 的 *embfile_* 方法和属性完全支持此功能。有关详细信息，请参阅 :ref:`附录 3<Appendix 3>`，或者访问 Wiki 页面 `dealing with embedding files`_，以及示例脚本 `embedded-copy.py`_、 `embedded-export.py`_、 `embedded-import.py`_ 和 `embedded-list.py`_。


.. tab:: 英文

    PDFs can be used as containers for arbitrary data (executables, other PDFs, text or binary files, etc.) much like ZIP archives.

    PyMuPDF fully supports this feature via :ref:`Document` *embfile_** methods and attributes. For some detail read :ref:`Appendix 3`, consult the Wiki on `dealing with embedding files`_, or the example scripts `embedded-copy.py`_, `embedded-export.py`_, `embedded-import.py`_, and `embedded-list.py`_.


保存
-------

Saving

.. tab:: 中文

    如上所述，:meth:`Document.save` 方法 **始终** 以当前状态保存文档。

    如果指定选项 *incremental=True*，可以将更改写回 **原始 PDF**。此过程通常 **极快**，因为更改只是 **追加** 到原文件，而不会完全重写它。

    :meth:`Document.save` 选项与 MuPDF 命令行工具 *mutool clean* 的选项对应，具体如下表所示。

    =================== =========== ==================================================
    **保存选项**       **mutool**  **效果**
    =================== =========== ==================================================
    garbage=1           g           垃圾回收未使用对象
    garbage=2           gg          除 1 之外，还会压缩 :data:`xref` 表
    garbage=3           ggg         除 2 之外，还会合并重复对象
    garbage=4           gggg        除 3 之外，还会合并重复的流内容
    clean=True          cs          清理并规范化内容流
    deflate=True        z           压缩未压缩的流
    deflate_images=True i           压缩图像流
    deflate_fonts=True  f           压缩字体文件流
    ascii=True          a           将二进制数据转换为 ASCII 格式
    linear=True         l           创建线性化版本
    expand=True         d           解压所有流
    =================== =========== ==================================================

    .. note:: 
        
        关于 *object, stream, xref* 等术语的解释，请参阅 :ref:`Glossary` 章节。

    例如，执行 *mutool clean -ggggz file.pdf* 可以获得极佳的压缩效果，其等效的 PyMuPDF 代码为 *doc.save(filename, garbage=4, deflate=True)*。


.. tab:: 英文

    As mentioned above, :meth:`Document.save` will **always** save the document in its current state.

    You can write changes back to the **original PDF** by specifying option *incremental=True*. This process is (usually) **extremely fast**, since changes are **appended to the original file** without completely rewriting it.

    :meth:`Document.save` options correspond to options of MuPDF's command line utility *mutool clean*, see the following table.

    =================== =========== ==================================================
    **Save Option**     **mutool**  **Effect**
    =================== =========== ==================================================
    garbage=1           g           garbage collect unused objects
    garbage=2           gg          in addition to 1, compact :data:`xref` tables
    garbage=3           ggg         in addition to 2, merge duplicate objects
    garbage=4           gggg        in addition to 3, merge duplicate stream content
    clean=True          cs          clean and sanitize content streams
    deflate=True        z           deflate uncompressed streams
    deflate_images=True i           deflate image streams
    deflate_fonts=True  f           deflate fontfile streams
    ascii=True          a           convert binary data to ASCII format
    linear=True         l           create a linearized version
    expand=True         d           decompress all streams
    =================== =========== ==================================================

    .. note:: 
        
        For an explanation of terms like *object, stream, xref* consult the :ref:`Glossary` chapter.

    For example, *mutool clean -ggggz file.pdf* yields excellent compression results. It corresponds to *doc.save(filename, garbage=4, deflate=True)*.

关闭
=========

Closing

.. tab:: 中文

    通常，在程序运行时，"close" 文档以释放对底层文件的控制权，使操作系统能够管理它，是一种理想的做法。

    可以使用 :meth:`Document.close` 方法实现这一点。除了关闭底层文件外，该方法还会释放与文档相关的缓冲区。


.. tab:: 英文

    It is often desirable to "close" a document to relinquish control of the underlying file to the OS, while your program continues.

    This can be achieved by the :meth:`Document.close` method. Apart from closing the underlying file, buffer areas associated with the document will be freed.

进一步阅读
================

Further Reading

.. tab:: 中文

    另外，还可以查看 PyMuPDF 的 `Wiki`_ 页面。特别是侧边栏标题 **"Recipes"** 下列出的内容，涵盖了 15 多个主题，并采用 "How-To" 风格编写。

    本文档还包含一个 :ref:`FAQ` 章节，该部分与上述 Recipes 紧密相关，并且会随着时间的推移不断扩展内容。


.. tab:: 英文

    Also have a look at PyMuPDF's `Wiki`_ pages. Especially those named in the sidebar under title **"Recipes"** cover over 15 topics written in "How-To" style.

    This document also contains a :ref:`FAQ`. This chapter has close connection to the aforementioned recipes, and it will be extended with more content over time.


-----


.. rubric:: Footnotes

.. [#f1] PyMuPDF lets you also open several image file types just like normal documents. See section :ref:`ImageFiles` in chapter :ref:`Pixmap` for more comments.

.. [#f2] :meth:`Page.get_text` is a convenience wrapper for several methods of another PyMuPDF class, :ref:`TextPage`. The names of these methods correspond to the argument string passed to :meth:`Page.get_text` \:  *Page.get_text("dict")* is equivalent to *TextPage.extractDICT()* \.

.. [#f3] "Sequences" are Python objects conforming to the sequence protocol. These objects implement a method named *__getitem__()*. Best known examples are Python tuples and lists. But *array.array*, *numpy.array* and PyMuPDF's "geometry" objects (:ref:`Algebra`) are sequences, too. Refer to :ref:`SequenceTypes` for details.

.. [#f4] PyMuPDF 还允许您像打开普通文档一样打开多种图像文件类型。请参阅 :ref:`Pixmap` 章节中的 :ref:`ImageFiles` 部分，以获取更多相关信息。

.. [#f5] :meth:`Page.get_text` 是对 PyMuPDF 另一个类 :ref:`TextPage` 多种方法的封装。传递给 :meth:`Page.get_text` 的参数字符串与这些方法的名称相对应。例如，*Page.get_text("dict")* 等价于 *TextPage.extractDICT()* \。

.. [#f6] "序列" (Sequences) 是符合 Python 序列协议的对象。这些对象实现了一个名为 *__getitem__()* 的方法。最常见的例子是 Python 的元组 (tuple) 和列表 (list) 。此外，*array.array*、*numpy.array* 以及 PyMuPDF 的 "几何" 对象 (:ref:`Algebra`) 也是序列。更多详情请参考 :ref:`SequenceTypes` 章节。



.. include:: footer.rst

.. External links:


.. _lxml: https://pypi.org/project/lxml/
.. _metadata import (PDF only): https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/import-metadata/import.py
.. _metadata export: https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/export-metadata/export.py
.. _toc import (PDF only): https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/import-toc/import.py
.. _toc export: https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/export-toc/export.py
.. _Vector Image Support page: https://github.com/pymupdf/PyMuPDF/wiki/Vector-Image-Support
.. _examples: https://github.com/pymupdf/PyMuPDF/tree/master/examples
.. _Pillow documentation: https://Pillow.readthedocs.io
.. _here it is!: https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/browse-document/browse.py
.. _PySimpleGUI: https://pypi.org/project/PySimpleGUI/
.. _demo.py: https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/demo/demo.py
.. _demo-lowlevel.py: https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/demo/demo-lowlevel.py
.. _"MuPDF Explored": https://mupdf.com/docs/mupdf-explored.html
.. _Artifex: https://www.artifex.com
.. _pdf-converter.py: https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/convert-document/convert.py
.. _PDFjoiner.py: https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/join-documents/join.py
.. _dealing with embedding files: https://github.com/pymupdf/PyMuPDF/wiki/Dealing-with-Embedded-Files
.. _embedded-copy.py: https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/copy-embedded/copy.py
.. _embedded-export.py: https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/export-embedded/export.py
.. _embedded-import.py: https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/import-embedded/import.py
.. _embedded-list.py: https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/list-embedded/list.py
.. _Wiki: https://github.com/pymupdf/PyMuPDF/wiki


