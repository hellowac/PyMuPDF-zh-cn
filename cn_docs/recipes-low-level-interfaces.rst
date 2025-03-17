.. include:: header.rst

.. _RecipesLowLevelInterfaces:

=========================================
低级接口
=========================================

Low-Level Interfaces

.. tab:: 中文

    有多种方法可用于在相当低的级别上访问和操作 PDF 文件。诚然，“低级”和“正常”功能之间的明确区分并不总是可能的，或者取决于个人喜好。

    也可能发生这种情况，以前被视为低级的功能后来被评估为正常界面的一部分。这在 v1.14.0 中发生在类 :ref:`Tools` 中 - 您现在可以在类章节中找到它作为一项。

    只有在文档的哪一章中才能找到您要查找的内容，这只是文档的问题。一切都可用，并且始终通过相同的界面。

.. tab:: 英文

    Numerous methods are available to access and manipulate PDF files on a fairly low level. Admittedly, a clear distinction between "low level" and "normal" functionality is not always possible or subject to personal taste.

    It also may happen, that functionality previously deemed low-level is later on assessed as being part of the normal interface. This has happened in v1.14.0 for the class :ref:`Tools` - you now find it as an item in the Classes chapter.

    It is a matter of documentation only in which chapter of the documentation you find what you are looking for. Everything is available and always via the same interface.

----------------------------------

如何遍历 :data:`xref` 表
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Iterate through the :data:`xref` Table

.. tab:: 中文

    PDF 的 :data:`xref` 表是文件中所有对象的列表。这个表可能包含成千上万条条目——例如，手册 :ref:`AdobeManual` 就有 127,000 个对象。表项 "0" 是保留项，不能被修改。
    以下脚本遍历 :data:`xref` 表并打印每个对象的定义::

        >>> xreflen = doc.xref_length()  # 对象表的长度
        >>> for xref in range(1, xreflen):  # 跳过第 0 项！
                print("")
                print("object %i (stream: %s)" % (xref, doc.xref_is_stream(xref)))
                print(doc.xref_object(xref, compressed=False))


    .. highlight:: text

    这将输出如下内容::

        object 1 (stream: False)
        <<
            /ModDate (D:20170314122233-04'00')
            /PXCViewerInfo (PDF-XChange Viewer;2.5.312.1;Feb  9 2015;12:00:06;D:20170314122233-04'00')
        >>

        object 2 (stream: False)
        <<
            /Type /Catalog
            /Pages 3 0 R
        >>

        object 3 (stream: False)
        <<
            /Kids [ 4 0 R 5 0 R ]
            /Type /Pages
            /Count 2
        >>

        object 4 (stream: False)
        <<
            /Type /Page
            /Annots [ 6 0 R ]
            /Parent 3 0 R
            /Contents 7 0 R
            /MediaBox [ 0 0 595 842 ]
            /Resources 8 0 R
        >>
        ...
        object 7 (stream: True)
        <<
            /Length 494
            /Filter /FlateDecode
        >>
        ...

    .. highlight:: python

    PDF 对象定义是一个普通的 ASCII 字符串。


.. tab:: 英文

    A PDF's :data:`xref` table is a list of all objects defined in the file. This table may easily contain many thousands of entries -- the manual :ref:`AdobeManual` for example has 127,000 objects. Table entry "0" is reserved and must not be touched.
    The following script loops through the :data:`xref` table and prints each object's definition::

        >>> xreflen = doc.xref_length()  # length of objects table
        >>> for xref in range(1, xreflen):  # skip item 0!
                print("")
                print("object %i (stream: %s)" % (xref, doc.xref_is_stream(xref)))
                print(doc.xref_object(xref, compressed=False))


    .. highlight:: text

    This produces the following output::

        object 1 (stream: False)
        <<
            /ModDate (D:20170314122233-04'00')
            /PXCViewerInfo (PDF-XChange Viewer;2.5.312.1;Feb  9 2015;12:00:06;D:20170314122233-04'00')
        >>

        object 2 (stream: False)
        <<
            /Type /Catalog
            /Pages 3 0 R
        >>

        object 3 (stream: False)
        <<
            /Kids [ 4 0 R 5 0 R ]
            /Type /Pages
            /Count 2
        >>

        object 4 (stream: False)
        <<
            /Type /Page
            /Annots [ 6 0 R ]
            /Parent 3 0 R
            /Contents 7 0 R
            /MediaBox [ 0 0 595 842 ]
            /Resources 8 0 R
        >>
        ...
        object 7 (stream: True)
        <<
            /Length 494
            /Filter /FlateDecode
        >>
        ...

    .. highlight:: python

    A PDF object definition is an ordinary ASCII string.

----------------------------------

如何处理对象流
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Handle Object Streams

.. tab:: 中文

    某些对象类型除了其对象定义外，还包含其他数据。例如，图像、字体、嵌入文件或描述页面外观的命令。

    这些类型的对象被称为“流对象”。PyMuPDF 允许通过方法 :meth:`Document.xref_stream` 读取对象的流，方法的参数是对象的 :data:`xref`。也可以使用 :meth:`Document.update_stream` 写回修改后的流版本。

    假设以下代码片段希望读取 PDF 中的所有流，无论出于何种原因：

        >>> xreflen = doc.xref_length() # 文件中的对象数量
        >>> for xref in range(1, xreflen): # 跳过第 0 项！
                if stream := doc.xref_stream(xref):
                    # 对其进行处理（它是一个字节对象或 None）
                    # 例如，仅写回：
                    doc.update_stream(xref, stream)

    :meth:`Document.xref_stream` 会自动返回已解压的字节对象流，而 :meth:`Document.update_stream` 会在有利的情况下自动对其进行压缩。


.. tab:: 英文

    Some object types contain additional data apart from their object definition. Examples are images, fonts, embedded files or commands describing the appearance of a page.

    Objects of these types are called "stream objects". PyMuPDF allows reading an object's stream via method :meth:`Document.xref_stream` with the object's :data:`xref` as an argument. It is also possible to write back a modified version of a stream using :meth:`Document.update_stream`.

    Assume that the following snippet wants to read all streams of a PDF for whatever reason::

        >>> xreflen = doc.xref_length() # number of objects in file
        >>> for xref in range(1, xreflen): # skip item 0!
                if stream := doc.xref_stream(xref):
                    # do something with it (it is a bytes object or None)
                    # e.g. just write it back:
                    doc.update_stream(xref, stream)

    :meth:`Document.xref_stream` automatically returns a stream decompressed as a bytes object -- and :meth:`Document.update_stream` automatically compresses it if beneficial.

----------------------------------

如何处理页面内容
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Handle Page Contents

.. tab:: 中文

    PDF 页面可以有零个或多个 :data:`contents` 对象。这些是描述页面上**什么**出现在**哪里**以及**如何**显示的流对象（如文本和图像）。它们使用一种特殊的迷你语言进行编写，例如在 :ref:`AdobeManual` 第 643 页的“附录 A - 操作符摘要”章节中描述。

    每个 PDF 阅读器应用程序必须能够解释内容语法，以重现页面的预期外观。

    如果提供了多个 :data:`contents` 对象，它们必须按指定顺序解释，完全按照将它们连接为一个整体时的方式进行处理。

    拥有多个 :data:`contents` 对象有一些良好的技术理由：

    * 添加新的 :data:`contents` 对象比维护一个大的 :data:`contents` 对象要容易得多且更快（后者涉及到读取、解压、修改、重新压缩和每次修改时的重写）。
    * 在处理增量更新时，修改过的大型 :data:`contents` 对象会使更新差异膨胀，因此可能会轻易抵消增量保存的效率。

    例如，PyMuPDF 在方法 :meth:`Page.insert_image`、:meth:`Page.show_pdf_page` 和 :ref:`Shape` 方法中添加新的小型 :data:`contents` 对象。

    然而，也有一些情况，当只有**一个** :data:`contents` 对象时更为有利：它比多个小的对象更容易解释，且更易于压缩。

    以下是两种合并页面多个内容的方法::

        >>> # 方法 1: 使用 MuPDF 清理功能
        >>> page.clean_contents()  # 清理并合并多个 Contents
        >>> xref = page.get_contents()[0]  # 现在只有一个 /Contents!
        >>> cont = doc.xref_stream(xref)
        >>> # 这也重新格式化了 PDF 命令

        >>> # 方法 2: 提取连接的内容
        >>> cont = page.read_contents()
        >>> # /Contents 源本身未修改

    清理函数 :meth:`Page.clean_contents` 做的不仅仅是将 :data:`contents` 对象拼接起来：它还会修正并优化页面的 PDF 操作符语法，删除与页面对象定义的不一致之处。


.. tab:: 英文

    A PDF page can have zero or multiple :data:`contents` objects. These are stream objects describing **what** appears **where** and **how** on a page (like text and images). They are written in a special mini-language described e.g. in chapter "APPENDIX A - Operator Summary" on page 643 of the :ref:`AdobeManual`.

    Every PDF reader application must be able to interpret the contents syntax to reproduce the intended appearance of the page.

    If multiple :data:`contents` objects are provided, they must be interpreted in the specified sequence in exactly the same way as if they were provided as a concatenation of the several.

    There are good technical arguments for having multiple :data:`contents` objects:

    * It is a lot easier and faster to just add new :data:`contents` objects than maintaining a single big one (which entails reading, decompressing, modifying, recompressing, and rewriting it for each change).
    * When working with incremental updates, a modified big :data:`contents` object will bloat the update delta and can thus easily negate the efficiency of incremental saves.

    For example, PyMuPDF adds new, small :data:`contents` objects in methods :meth:`Page.insert_image`, :meth:`Page.show_pdf_page` and the :ref:`Shape` methods.

    However, there are also situations when a **single** :data:`contents` object is beneficial: it is easier to interpret and more compressible than multiple smaller ones.

    Here are two ways of combining multiple contents of a page::

        >>> # method 1: use the MuPDF clean function
        >>> page.clean_contents()  # cleans and combines multiple Contents
        >>> xref = page.get_contents()[0]  # only one /Contents now!
        >>> cont = doc.xref_stream(xref)
        >>> # this has also reformatted the PDF commands

        >>> # method 2: extract concatenated contents
        >>> cont = page.read_contents()
        >>> # the /Contents source itself is unmodified

    The clean function :meth:`Page.clean_contents` does a lot more than just glueing :data:`contents` objects: it also corrects and optimizes the PDF operator syntax of the page and removes any inconsistencies with the page's object definition.

----------------------------------

如何访问 PDF 目录
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Access the PDF Catalog

.. tab:: 中文

    这是 PDF 的一个核心（"根"）对象。它作为访问其他重要对象的起点，同时也包含一些全局选项，如下所示::

        >>> import pymupdf
        >>> doc=pymupdf.open("PyMuPDF.pdf")
        >>> cat = doc.pdf_catalog()  # 获取 /Catalog 的 xref
        >>> print(doc.xref_object(cat))  # 打印对象定义
        <<
            /Type/Catalog                 % 对象类型
            /Pages 3593 0 R               % 指向页面树
            /OpenAction 225 0 R           % 打开时执行的动作
            /Names 3832 0 R               % 指向全局名称树
            /PageMode /UseOutlines        % 初始显示 TOC
            /PageLabels<</Nums[0<</S/D>>2<</S/r>>8<</S/D>>]>> % 页面标签
            /Outlines 3835 0 R            % 指向大纲树
        >>

    .. note:: 
      
        为了说明目的，这里插入了缩进、换行符和注释，通常不会出现。有关 PDF 目录的更多信息，请参阅 :ref:`AdobeManual` 第 71 页的第 7.7.2 节。

.. tab:: 英文

    This is a central ("root") object of a PDF. It serves as a starting point to reach important other objects and it also contains some global options for the PDF::

        >>> import pymupdf
        >>> doc=pymupdf.open("PyMuPDF.pdf")
        >>> cat = doc.pdf_catalog()  # get xref of the /Catalog
        >>> print(doc.xref_object(cat))  # print object definition
        <<
            /Type/Catalog                 % object type
            /Pages 3593 0 R               % points to page tree
            /OpenAction 225 0 R           % action to perform on open
            /Names 3832 0 R               % points to global names tree
            /PageMode /UseOutlines        % initially show the TOC
            /PageLabels<</Nums[0<</S/D>>2<</S/r>>8<</S/D>>]>> % labels given to pages
            /Outlines 3835 0 R            % points to outline tree
        >>

    .. note:: 
      
        Indentation, line breaks and comments are inserted here for clarification purposes only and will not normally appear. For more information on the PDF catalog see section 7.7.2 on page 71 of the :ref:`AdobeManual`.

----------------------------------

如何访问 PDF 文件尾部
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Access the PDF File Trailer

.. tab:: 中文

    PDF 文件的尾部包含一个 :data:`dictionary`，称为 trailer。它包含特殊对象和指向重要其他信息的指针。参见 :ref:`AdobeManual` 第 42 页。以下是概述：

    ======= =========== ===================================================================================
    **键**     **类型**    **值**
    ======= =========== ===================================================================================
    Size    int         在交叉引用表中的条目数 + 1。
    Prev    int         指向先前 :data:`xref` 部分的偏移量（表示增量更新）。
    Root    dictionary  （间接）指向目录的指针。参见前一节。
    Encrypt dictionary  指向加密对象的指针（仅限加密文件）。
    Info    dictionary  （间接）指向信息（元数据）的指针。
    ID      array       由两个字节字符串组成的文件标识符。
    XRefStm int         交叉引用流的偏移量。参见 :ref:`AdobeManual` 第 49 页。
    ======= =========== ===================================================================================

    可以通过 PyMuPDF 使用 :meth:`Document.pdf_trailer` 或者使用 :meth:`Document.xref_object`，传入 -1 代替有效的 :data:`xref` 数字来访问这些信息。

        >>> import pymupdf
        >>> doc=pymupdf.open("PyMuPDF.pdf")
        >>> print(doc.xref_object(-1))  # 或者：print(doc.pdf_trailer())
        <<
        /Type /XRef
        /Index [ 0 8263 ]
        /Size 8263
        /W [ 1 3 1 ]
        /Root 8260 0 R
        /Info 8261 0 R
        /ID [ <4339B9CEE46C2CD28A79EBDDD67CC9B3> <4339B9CEE46C2CD28A79EBDDD67CC9B3> ]
        /Length 19883
        /Filter /FlateDecode
        >>
        >>>


.. tab:: 英文

    The trailer of a PDF file is a :data:`dictionary` located towards the end of the file. It contains special objects, and pointers to important other information. See :ref:`AdobeManual` p. 42. Here is an overview:

    ======= =========== ===================================================================================
    **Key** **Type**    **Value**
    ======= =========== ===================================================================================
    Size    int         Number of entries in the cross-reference table + 1.
    Prev    int         Offset to previous :data:`xref` section (indicates incremental updates).
    Root    dictionary  (indirect) Pointer to the catalog. See previous section.
    Encrypt dictionary  Pointer to encryption object (encrypted files only).
    Info    dictionary  (indirect) Pointer to information (metadata).
    ID      array       File identifier consisting of two byte strings.
    XRefStm int         Offset of a cross-reference stream. See :ref:`AdobeManual` p. 49.
    ======= =========== ===================================================================================

    Access this information via PyMuPDF with :meth:`Document.pdf_trailer` or, equivalently, via :meth:`Document.xref_object` using -1 instead of a valid :data:`xref` number.

        >>> import pymupdf
        >>> doc=pymupdf.open("PyMuPDF.pdf")
        >>> print(doc.xref_object(-1))  # or: print(doc.pdf_trailer())
        <<
        /Type /XRef
        /Index [ 0 8263 ]
        /Size 8263
        /W [ 1 3 1 ]
        /Root 8260 0 R
        /Info 8261 0 R
        /ID [ <4339B9CEE46C2CD28A79EBDDD67CC9B3> <4339B9CEE46C2CD28A79EBDDD67CC9B3> ]
        /Length 19883
        /Filter /FlateDecode
        >>
        >>>

----------------------------------

如何访问 XML 元数据
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Access XML Metadata

.. tab:: 中文

    PDF 文件可能包含 XML 元数据，除了标准的元数据格式外。事实上，大多数 PDF 查看器或修改软件（如 Adobe、Nitro PDF、PDF-XChange 等）在保存 PDF 时会添加此类信息。

    PyMuPDF 不能直接**解释或更改**这些信息，因为它不包含 XML 特性。然而，XML 元数据存储为 :data:`stream` 对象，因此可以读取、使用适当的软件修改并写回。

        >>> xmlmetadata = doc.get_xml_metadata()
        >>> print(xmlmetadata)
        <?xpacket begin="\ufeff" id="W5M0MpCehiHzreSzNTczkc9d"?>
        <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="3.1-702">
        <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
        ...
        omitted data
        ...
        <?xpacket end="w"?>

    使用一些 XML 包，可以解释和/或修改 XML 数据，然后将其存储回来。以下操作也适用于如果 PDF 之前没有 XML 元数据的情况::

        >>> # 写回修改后的 XML 元数据：
        >>> doc.set_xml_metadata(xmlmetadata)
        >>>
        >>> # 可以像这样删除 XML 元数据：
        >>> doc.del_xml_metadata()


.. tab:: 英文

    A PDF may contain XML metadata in addition to the standard metadata format. In fact, most PDF viewer or modification software adds this type of information when saving the PDF (Adobe, Nitro PDF, PDF-XChange, etc.).

    PyMuPDF has no way to **interpret or change** this information directly, because it contains no XML features. XML metadata is however stored as a :data:`stream` object, so it can be read, modified with appropriate software and written back.

        >>> xmlmetadata = doc.get_xml_metadata()
        >>> print(xmlmetadata)
        <?xpacket begin="\ufeff" id="W5M0MpCehiHzreSzNTczkc9d"?>
        <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="3.1-702">
        <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
        ...
        omitted data
        ...
        <?xpacket end="w"?>

    Using some XML package, the XML data can be interpreted and / or modified and then stored back. The following also works, if the PDF previously had no XML metadata::

        >>> # write back modified XML metadata:
        >>> doc.set_xml_metadata(xmlmetadata)
        >>>
        >>> # XML metadata can be deleted like this:
        >>> doc.del_xml_metadata()

----------------------------------

如何扩展 PDF 元数据
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Extend PDF Metadata

.. tab:: 中文

    属性 :attr:`Document.metadata` 设计为以相同的方式适用于所有 :ref:`支持的文件类型<Supported_File_Types>`：它是一个包含 **固定键值对** 的 Python 字典。因此，:meth:`Document.set_metadata` 只接受标准键。

    然而，PDF 可能包含无法直接访问的项目。也可能有理由存储额外的信息，如版权。下面是使用 PyMuPDF 低级函数处理 **任意元数据项** 的方法。

    例如，查看某个 PDF 的标准元数据输出::

        # ---------------------
        # 标准元数据
        # ---------------------
        pprint(doc.metadata)
        {'author': 'PRINCE',
        'creationDate': "D:2010102417034406'-30'",
        'creator': 'PrimoPDF http://www.primopdf.com/',
        'encryption': None,
        'format': 'PDF 1.4',
        'keywords': '',
        'modDate': "D:20200725062431-04'00'",
        'producer': 'macOS Version 10.15.6 (Build 19G71a) Quartz PDFContext, '
                    'AppendMode 1.1',
        'subject': '',
        'title': 'Full page fax print',
        'trapped': ''}

    使用以下代码查看 **所有项** 存储在元数据对象中::

        # ----------------------------------
        # 包括私有项的元数据
        # ----------------------------------
        metadata = {}  # 创建我自己的元数据字典
        what, value = doc.xref_get_key(-1, "Info")  # 获取 /Info 键在 trailer 中的值
        if what != "xref":
            pass  # PDF 没有元数据
        else:
            xref = int(value.replace("0 R", ""))  # 提取元数据的 xref
            for key in doc.xref_get_keys(xref):
                metadata[key] = doc.xref_get_key(xref, key)[1]
        pprint(metadata)
        {'Author': 'PRINCE',
        'CreationDate': "D:2010102417034406'-30'",
        'Creator': 'PrimoPDF http://www.primopdf.com/',
        'ModDate': "D:20200725062431-04'00'",
        'PXCViewerInfo': 'PDF-XChange Viewer;2.5.312.1;Feb  9 '
                        "2015;12:00:06;D:20200725062431-04'00'",
        'Producer': 'macOS Version 10.15.6 (Build 19G71a) Quartz PDFContext, '
                    'AppendMode 1.1',
        'Title': 'Full page fax print'}
        # ---------------------------------------------------------------
        # 注意额外的 'PXCViewerInfo' 键 - 标准元数据中未包含！
        # ---------------------------------------------------------------

    *反过来*，您也可以 **存储私有元数据项** 到 PDF 中。您需要确保这些项符合 PDF 规范，特别是它们必须是（Unicode）字符串。有关详细信息和注意事项，请参阅 :ref:`AdobeManual` 第 14.3 节（第 548 页）::

        what, value = doc.xref_get_key(-1, "Info")  # 获取 /Info 键在 trailer 中的值
        if what != "xref":
            raise ValueError("PDF 没有元数据")
        xref = int(value.replace("0 R", ""))  # 提取元数据的 xref
        # 添加一些私有信息
        doc.xref_set_key(xref, "mykey", pymupdf.get_pdf_str("北京 is Beijing"))
        #
        # 执行上述代码片段后，您将看到：
        pprint(metadata)
        {'Author': 'PRINCE',
        'CreationDate': "D:2010102417034406'-30'",
        'Creator': 'PrimoPDF http://www.primopdf.com/',
        'ModDate': "D:20200725062431-04'00'",
        'PXCViewerInfo': 'PDF-XChange Viewer;2.5.312.1;Feb  9 '
                          "2015;12:00:06;D:20200725062431-04'00'",
        'Producer': 'macOS Version 10.15.6 (Build 19G71a) Quartz PDFContext, '
                    'AppendMode 1.1',
        'Title': 'Full page fax print',
        'mykey': '北京 is Beijing'}

    要删除选定的键，可以使用 `doc.xref_set_key(xref, "mykey", "null")`。如下一节所述，字符串 "null" 是 PDF 中 Python `None` 的等效物。具有该值的键将被视为未指定 -- 并在垃圾回收中物理删除。


.. tab:: 英文

    Attribute :attr:`Document.metadata` is designed so it works for all :ref:`supported document types<Supported_File_Types>` in the same way: it is a Python dictionary with a **fixed set of key-value pairs**. Correspondingly, :meth:`Document.set_metadata` only accepts standard keys.

    However, PDFs may contain items not accessible like this. Also, there may be reasons to store additional information, like copyrights. Here is a way to handle **arbitrary metadata items** by using PyMuPDF low-level functions.

    As an example, look at this standard metadata output of some PDF::

        # ---------------------
        # standard metadata
        # ---------------------
        pprint(doc.metadata)
        {'author': 'PRINCE',
        'creationDate': "D:2010102417034406'-30'",
        'creator': 'PrimoPDF http://www.primopdf.com/',
        'encryption': None,
        'format': 'PDF 1.4',
        'keywords': '',
        'modDate': "D:20200725062431-04'00'",
        'producer': 'macOS Version 10.15.6 (Build 19G71a) Quartz PDFContext, '
                    'AppendMode 1.1',
        'subject': '',
        'title': 'Full page fax print',
        'trapped': ''}

    Use the following code to see **all items** stored in the metadata object::

        # ----------------------------------
        # metadata including private items
        # ----------------------------------
        metadata = {}  # make my own metadata dict
        what, value = doc.xref_get_key(-1, "Info")  # /Info key in the trailer
        if what != "xref":
            pass  # PDF has no metadata
        else:
            xref = int(value.replace("0 R", ""))  # extract the metadata xref
            for key in doc.xref_get_keys(xref):
                metadata[key] = doc.xref_get_key(xref, key)[1]
        pprint(metadata)
        {'Author': 'PRINCE',
        'CreationDate': "D:2010102417034406'-30'",
        'Creator': 'PrimoPDF http://www.primopdf.com/',
        'ModDate': "D:20200725062431-04'00'",
        'PXCViewerInfo': 'PDF-XChange Viewer;2.5.312.1;Feb  9 '
                        "2015;12:00:06;D:20200725062431-04'00'",
        'Producer': 'macOS Version 10.15.6 (Build 19G71a) Quartz PDFContext, '
                    'AppendMode 1.1',
        'Title': 'Full page fax print'}
        # ---------------------------------------------------------------
        # note the additional 'PXCViewerInfo' key - ignored in standard!
        # ---------------------------------------------------------------


    *Vice versa*, you can also **store private metadata items** in a PDF. It is your responsibility to make sure that these items conform to PDF specifications - especially they must be (unicode) strings. Consult section 14.3 (p. 548) of the :ref:`AdobeManual` for details and caveats::

        what, value = doc.xref_get_key(-1, "Info")  # /Info key in the trailer
        if what != "xref":
            raise ValueError("PDF has no metadata")
        xref = int(value.replace("0 R", ""))  # extract the metadata xref
        # add some private information
        doc.xref_set_key(xref, "mykey", pymupdf.get_pdf_str("北京 is Beijing"))
        #
        # after executing the previous code snippet, we will see this:
        pprint(metadata)
        {'Author': 'PRINCE',
        'CreationDate': "D:2010102417034406'-30'",
        'Creator': 'PrimoPDF http://www.primopdf.com/',
        'ModDate': "D:20200725062431-04'00'",
        'PXCViewerInfo': 'PDF-XChange Viewer;2.5.312.1;Feb  9 '
                          "2015;12:00:06;D:20200725062431-04'00'",
        'Producer': 'macOS Version 10.15.6 (Build 19G71a) Quartz PDFContext, '
                    'AppendMode 1.1',
        'Title': 'Full page fax print',
        'mykey': '北京 is Beijing'}

    To delete selected keys, use `doc.xref_set_key(xref, "mykey", "null")`. As explained in the next section, string "null" is the PDF equivalent to Python's `None`. A key with that value will be treated as not being specified -- and physically removed in garbage collections.

----------------------------------

如何读取和更新 PDF 对象
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Read and Update PDF Objects

.. highlight:: python

.. tab:: 中文

    也有一些细粒度、优雅的方式来访问和操作选定的 PDF :data:`dictionary` 键。

    * :meth:`Document.xref_get_keys` 返回对象在 :data:`xref` 处的 PDF 键::

        In [1]: import pymupdf
        In [2]: doc = pymupdf.open("pymupdf.pdf")
        In [3]: page = doc[0]
        In [4]: from pprint import pprint
        In [5]: pprint(doc.xref_get_keys(page.xref))
        ('Type', 'Contents', 'Resources', 'MediaBox', 'Parent')

    * 与完整的对象定义进行比较::

        In [6]: print(doc.xref_object(page.xref))
        <<
          /Type /Page
          /Contents 1297 0 R
          /Resources 1296 0 R
          /MediaBox [ 0 0 612 792 ]
          /Parent 1301 0 R
        >>

    * 单个键也可以通过 :meth:`Document.xref_get_key` 直接访问。值 **始终是一个字符串** ，并附带类型信息，有助于解释它::

        In [7]: doc.xref_get_key(page.xref, "MediaBox")
        Out[7]: ('array', '[0 0 612 792]')

    * 以下是上述页面键的完整列表::

        In [9]: for key in doc.xref_get_keys(page.xref):
        ...:        print("%s = %s" % (key, doc.xref_get_key(page.xref, key)))
        ...:
        Type = ('name', '/Page')
        Contents = ('xref', '1297 0 R')
        Resources = ('xref', '1296 0 R')
        MediaBox = ('array', '[0 0 612 792]')
        Parent = ('xref', '1301 0 R')

    * 未定义的键查询将返回 `('null', 'null')` -- PDF 对象类型 `null` 对应于 Python 中的 `None`。对于布尔值 `true` 和 `false` 也类似。
    * 让我们向页面定义中添加一个新的键，将其旋转角度设置为 90 度（你知道其实已经有 :meth:`Page.set_rotation` 方法来实现这一点了吗？）::

        In [11]: doc.xref_get_key(page.xref, "Rotate")  # 没有设置旋转：
        Out[11]: ('null', 'null')
        In [12]: doc.xref_set_key(page.xref, "Rotate", "90")  # 插入一个新键
        In [13]: print(doc.xref_object(page.xref))  # 确认成功
        <<
          /Type /Page
          /Contents 1297 0 R
          /Resources 1296 0 R
          /MediaBox [ 0 0 612 792 ]
          /Parent 1301 0 R
          /Rotate 90
        >>

    * 这种方法还可以通过将键的值设置为 `null` 来从 :data:`xref` 字典中移除一个键：以下将从页面中移除旋转规格：`doc.xref_set_key(page.xref, "Rotate", "null")`。类似地，要从页面中删除所有链接、注释和字段，可以使用 `doc.xref_set_key(page.xref, "Annots", "null")`。因为 `Annots` 本身是一个数组，所以使用语句 `doc.xref_set_key(page.xref, "Annots", "[]")` 来设置一个空数组，也能达到同样的效果。

    * PDF 字典可以是层次结构嵌套的。在以下页面对象定义中，`Font` 和 `XObject` 都是 `Resources` 的子字典::

        In [15]: print(doc.xref_object(page.xref))
        <<
          /Type /Page
          /Contents 1297 0 R
          /Resources <<
            /XObject <<
              /Im1 1291 0 R
            >>
            /Font <<
              /F39 1299 0 R
              /F40 1300 0 R
            >>
          >>
          /MediaBox [ 0 0 612 792 ]
          /Parent 1301 0 R
          /Rotate 90
        >>

    * 上述情况可通过方法 :meth:`Document.xref_set_key` 和 :meth:`Document.xref_get_key` **支持** ：使用类似路径的表示法来指向所需的键。例如，要获取上述 `Im1` 键的值，需要在键参数中指定“位于其上方”的完整字典链：`"Resources/XObject/Im1"`::

        In [16]: doc.xref_get_key(page.xref, "Resources/XObject/Im1")
        Out[16]: ('xref', '1291 0 R')

    * 该路径表示法也可用于 **直接设置值**：使用以下代码让 `Im1` 指向另一个对象::

        In [17]: doc.xref_set_key(page.xref, "Resources/XObject/Im1", "9999 0 R")
        In [18]: print(doc.xref_object(page.xref))  # 确认成功:
        <<
          /Type /Page
          /Contents 1297 0 R
          /Resources <<
            /XObject <<
              /Im1 9999 0 R
            >>
            /Font <<
              /F39 1299 0 R
              /F40 1300 0 R
            >>
          >>
          /MediaBox [ 0 0 612 792 ]
          /Parent 1301 0 R
          /Rotate 90
        >>

      请注意，此处 **不会进行任何语义检查**：如果 PDF 中没有 xref 9999，当前操作不会检测到这一点。

    * 如果键不存在，则在设置其值时会自动创建它。此外，如果中间键也不存在，它们将根据需要自动创建。以下代码在已存在的字典 `A` 之下的多个层级创建了数组 `D`。其中，中间字典 `B` 和 `C` 也会被自动创建::

        In [5]: print(doc.xref_object(xref))  # 一些已存在的 PDF 对象:
        <<
          /A <<
          >>
        >>
        In [6]: # 以下代码将创建 'B'、'C' 和 'D'
        In [7]: doc.xref_set_key(xref, "A/B/C/D", "[1 2 3 4]")
        In [8]: print(doc.xref_object(xref))  # 查看结果:
        <<
          /A <<
            /B <<
              /C <<
                /D [ 1 2 3 4 ]
              >>
            >>
          >>
        >>

    * 在设置键值时，MuPDF 会进行基本的 **PDF 语法检查**。例如，新的键只能创建在 **字典下方**。以下代码尝试在先前创建的数组 `D` 下创建一个新的字符串项 `E`::

        In [9]: # 'D' 是一个数组，而不是字典！
        In [10]: doc.xref_set_key(xref, "A/B/C/D/E", "(hello)")
        mupdf: not a dict (array)
        --- ... ---
        RuntimeError: not a dict (array)

    * 另外，如果某个上层键是 **“间接对象”** （即 xref），则无法创建新键。换句话说，xref 只能被直接修改，不能通过其他引用它的对象间接修改::

        In [13]: # 下面的对象指向了一个 xref
        In [14]: print(doc.xref_object(4))
        <<
          /E 3 0 R
        >>
        In [15]: # 'E' 是一个间接对象，因此无法在这里修改！
        In [16]: doc.xref_set_key(4, "E/F", "90")
        mupdf: path to 'F' has indirects
        --- ... ---
        RuntimeError: path to 'F' has indirects

    .. caution:: 
      
        这些是 **高级功能**！不会验证指定的 PDF 对象、xref 等是否有效。与其他底层方法一样，误用可能会导致 PDF 文件或其中的部分内容变得不可用。
  
.. tab:: 英文

    There also exist granular, elegant ways to access and manipulate selected PDF :data:`dictionary` keys.

    * :meth:`Document.xref_get_keys` returns the PDF keys of the object at :data:`xref`::

        In [1]: import pymupdf
        In [2]: doc = pymupdf.open("pymupdf.pdf")
        In [3]: page = doc[0]
        In [4]: from pprint import pprint
        In [5]: pprint(doc.xref_get_keys(page.xref))
        ('Type', 'Contents', 'Resources', 'MediaBox', 'Parent')

    * Compare with the full object definition::

        In [6]: print(doc.xref_object(page.xref))
        <<
          /Type /Page
          /Contents 1297 0 R
          /Resources 1296 0 R
          /MediaBox [ 0 0 612 792 ]
          /Parent 1301 0 R
        >>

    * Single keys can also be accessed directly via :meth:`Document.xref_get_key`. The value **always is a string** together with type information, that helps with interpreting it::

        In [7]: doc.xref_get_key(page.xref, "MediaBox")
        Out[7]: ('array', '[0 0 612 792]')

    * Here is a full listing of the above page keys::

        In [9]: for key in doc.xref_get_keys(page.xref):
        ...:        print("%s = %s" % (key, doc.xref_get_key(page.xref, key)))
        ...:
        Type = ('name', '/Page')
        Contents = ('xref', '1297 0 R')
        Resources = ('xref', '1296 0 R')
        MediaBox = ('array', '[0 0 612 792]')
        Parent = ('xref', '1301 0 R')

    * An undefined key inquiry returns `('null', 'null')` -- PDF object type `null` corresponds to `None` in Python. Similar for the booleans `true` and `false`.
    * Let us add a new key to the page definition that sets its rotation to 90 degrees (you are aware that there actually exists :meth:`Page.set_rotation` for this?)::

        In [11]: doc.xref_get_key(page.xref, "Rotate")  # no rotation set:
        Out[11]: ('null', 'null')
        In [12]: doc.xref_set_key(page.xref, "Rotate", "90")  # insert a new key
        In [13]: print(doc.xref_object(page.xref))  # confirm success
        <<
          /Type /Page
          /Contents 1297 0 R
          /Resources 1296 0 R
          /MediaBox [ 0 0 612 792 ]
          /Parent 1301 0 R
          /Rotate 90
        >>

    * This method can also be used to remove a key from the :data:`xref` dictionary by setting its value to `null`: The following will remove the rotation specification from the page: `doc.xref_set_key(page.xref, "Rotate", "null")`. Similarly, to remove all links, annotations and fields from a page, use `doc.xref_set_key(page.xref, "Annots", "null")`. Because `Annots` by definition is an array, setting en empty array with the statement `doc.xref_set_key(page.xref, "Annots", "[]")` would do the same job in this case.

    * PDF dictionaries can be hierarchically nested. In the following page object definition both, `Font` and `XObject` are subdictionaries of `Resources`::

        In [15]: print(doc.xref_object(page.xref))
        <<
          /Type /Page
          /Contents 1297 0 R
          /Resources <<
            /XObject <<
              /Im1 1291 0 R
            >>
            /Font <<
              /F39 1299 0 R
              /F40 1300 0 R
            >>
          >>
          /MediaBox [ 0 0 612 792 ]
          /Parent 1301 0 R
          /Rotate 90
        >>

    * The above situation **is supported** by methods :meth:`Document.xref_set_key` and :meth:`Document.xref_get_key`: use a path-like notation to point at the required key. For example, to retrieve the value of key `Im1` above, specify the complete chain of dictionaries "above" it in the key argument: `"Resources/XObject/Im1"`::

        In [16]: doc.xref_get_key(page.xref, "Resources/XObject/Im1")
        Out[16]: ('xref', '1291 0 R')

    * The path notation can also be used to **directly set a value**: use the following to let `Im1` point to a different object::

        In [17]: doc.xref_set_key(page.xref, "Resources/XObject/Im1", "9999 0 R")
        In [18]: print(doc.xref_object(page.xref))  # confirm success:
        <<
          /Type /Page
          /Contents 1297 0 R
          /Resources <<
            /XObject <<
              /Im1 9999 0 R
            >>
            /Font <<
              /F39 1299 0 R
              /F40 1300 0 R
            >>
          >>
          /MediaBox [ 0 0 612 792 ]
          /Parent 1301 0 R
          /Rotate 90
        >>

      Be aware, that **no semantic checks** whatsoever will take place here: if the PDF has no xref 9999, it won't be detected at this point.

    * If a key does not exist, it will be created by setting its value. Moreover, if any intermediate keys do not exist either, they will also be created as necessary. The following creates an array `D` several levels below the existing dictionary `A`. Intermediate dictionaries `B` and `C` are automatically created::

        In [5]: print(doc.xref_object(xref))  # some existing PDF object:
        <<
          /A <<
          >>
        >>
        In [6]: # the following will create 'B', 'C' and 'D'
        In [7]: doc.xref_set_key(xref, "A/B/C/D", "[1 2 3 4]")
        In [8]: print(doc.xref_object(xref))  # check out what happened:
        <<
          /A <<
            /B <<
              /C <<
                /D [ 1 2 3 4 ]
              >>
            >>
          >>
        >>

    * When setting key values, basic **PDF syntax checking** will be done by MuPDF. For example, new keys can only be created **below a dictionary**. The following tries to create some new string item `E` below the previously created array `D`::

        In [9]: # 'D' is an array, no dictionary!
        In [10]: doc.xref_set_key(xref, "A/B/C/D/E", "(hello)")
        mupdf: not a dict (array)
        --- ... ---
        RuntimeError: not a dict (array)

    * It is also **not possible**, to create a key if some higher level key is an **"indirect"** object, i.e. an xref. In other words, xrefs can only be modified directly and not implicitly via other objects referencing them::

        In [13]: # the following object points to an xref
        In [14]: print(doc.xref_object(4))
        <<
          /E 3 0 R
        >>
        In [15]: # 'E' is an indirect object and cannot be modified here!
        In [16]: doc.xref_set_key(4, "E/F", "90")
        mupdf: path to 'F' has indirects
        --- ... ---
        RuntimeError: path to 'F' has indirects

    .. caution:: 
      
        These are expert functions! There are no validations as to whether valid PDF objects, xrefs, etc. are specified. As with other low-level methods there is the risk to render the PDF, or parts of it unusable.

.. include:: footer.rst
