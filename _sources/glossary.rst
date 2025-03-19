.. include:: header.rst

.. _Glossary:

==============
术语
==============

Glossary

.. tab:: 中文

        .. data:: coordinate

                这是一个理解本文档的基本数学/几何术语。有关更详细的讨论，请参见本节：:ref:`Coordinates`。

        .. data:: matrix_like

                一个包含 6 个数字的 Python 序列。

        .. data:: rect_like

                一个包含 4 个数字的 Python 序列。

        .. data:: irect_like

                一个包含 4 个整数的 Python 序列。

        .. data:: point_like

                一个包含 2 个数字的 Python 序列。

        .. data:: quad_like

                一个包含 4 个 :data:`point_like` 项目的 Python 序列。

        .. data:: inheritable

                PDF 中的某些值可以通过父子关系传递给下层对象。例如，页面的媒体框（物理大小）可以只在 :data:`pagetree` 的某些节点中指定一次，然后对所有未指定自己值的“子”页使用该值。


.. tab:: 英文

        .. data:: coordinate
                :no-index:

                This is an essential general mathematical / geometrical term for understanding this documentation. Please see this section for a more detailed discussion: :ref:`Coordinates`.

        .. data:: matrix_like
                :no-index:

                A Python sequence of 6 numbers.

        .. data:: rect_like
                :no-index:

                A Python sequence of 4 numbers.

        .. data:: irect_like
                :no-index:

                A Python sequence of 4 integers.

        .. data:: point_like
                :no-index:

                A Python sequence of 2 numbers.

        .. data:: quad_like
                :no-index:

                A Python sequence of 4 :data:`point_like` items.

        .. data:: inheritable
                :no-index:

                A number of values in a PDF can inherited by objects further down in a parent-child relationship. The mediabox (physical size) of pages may for example be specified only once or in some node(s) of the :data:`pagetree` and will then be taken as value for all *kids*, that do not specify their own value.

.. _Glossary_MediaBox:

.. tab:: 中文

        .. data:: MediaBox

                一个包含 4 个浮动数字的 PDF 数组，指定页面的物理大小 -- (:data:`inheritable`, 强制性)。此矩形应包含所有其他 PDF 页面矩形，这些矩形可以另外指定：CropBox、TrimBox、ArtBox 和 BleedBox。详细信息请参考 :ref:`AdobeManual`。MediaBox 是唯一一个在 MuPDF 和 PDF 坐标系统之间没有差异的矩形：:attr:`Page.mediabox` 将始终显示与页面对象定义中的 `/MediaBox` 键相同的坐标。对于所有其他矩形，MuPDF 会变换 y 坐标，使得 **顶部** 边界成为参考点。有时这可能会让人困惑 -- 例如，您可能会遇到如下情况：

                * 页面定义包含以下相同的值：`/MediaBox [ 36 45 607.5 765 ]`，`/CropBox [ 36 45 607.5 765 ]`。
                * PyMuPDF 显示 `page.mediabox = Rect(36.0, 45.0, 607.5, 765.0)`。
                * **但是：** `page.cropbox = Rect(36.0, 0.0, 607.5, 720.0)`，因为两个 y 坐标已被变换（从两者中减去 45）。

        .. data:: CropBox

                一个包含 4 个浮动数字的 PDF 数组，指定页面的可见区域 -- (:data:`inheritable`, 可选)。它是 TrimBox、ArtBox 和 BleedBox 的默认值。如果未指定，它默认等于 MediaBox。此值 **不受页面旋转的影响** -- 与 :attr:`Page.rect` 相反。此外，除了页面矩形外，cropbox 的左上角坐标可能是 *(0, 0)* 也可能不是。

        .. data:: catalog

                一个中心 PDF :data:`dictionary` -- 也称为“根字典” -- 包含文档范围的参数和指向许多其他信息的指针。它的 :data:`xref` 由 :meth:`Document.pdf_catalog` 返回。

        .. data:: trailer

                更精确地说，**PDF trailer** 包含以 :data:`dictionary` 格式的信息。它通常位于文件的末尾。在此字典中，您将找到诸如目录和元数据的 xref、xref 数量等信息。以下是 PDF 规范中的定义：

                *“PDF 文件的 trailer 使得读取文件的应用程序能够快速找到交叉引用表和某些特殊对象。应用程序应从 PDF 文件的末尾开始读取。”*

                要在 PyMuPDF 中访问 trailer，请使用常用方法 :meth:`Document.xref_object`、:meth:`Document.xref_get_key` 和 :meth:`Document.xref_get_keys`，并使用 `-1` 代替正数 xref 编号。

        .. data:: contents

                **内容流** 是一个 PDF :data:`object`，其附加了一个 :data:`stream`，该流的数据由描述要绘制在页面上的图形元素的指令序列组成，详见 :ref:`AdobeManual` 中的 "Stream Objects" 第 19 页。有关这些流中使用的迷你语言的概述，请参见 :ref:`AdobeManual` 中的 "Operator Summary" 第 643 页。一个 PDF :data:`page` 可以有零到多个 contents 对象。如果没有，它为空页面（但仍然可能显示注释）。如果有多个，它们将按顺序解释，就像它们的指令出现在一个对象中一样（即像一个串联的字符串）。需要注意的是，还有其他使用相同语法的流对象类型：例如与注释关联的外观字典和表单 XObjects。

                PyMuPDF 提供了多个方法来处理 PDF 页面内容：

                * :meth:`Page.read_contents()` -- 读取并将所有页面内容合并为一个 `bytes` 对象。
                * :meth:`Page.clean_contents()` -- 一个 MuPDF 函数的包装器，读取、合并并清理所有页面内容的语法。之后，只会存在一个 `/Contents` 对象。此外，页面 :data:`resources` 将与其同步，确保它包含页面实际引用的所有图像、字体和其他对象。
                * :meth:`Page.get_contents()` -- 返回页面 :data:`contents` 对象的 :data:`xref` 编号列表。可能为空。使用 :meth:`Document.xref_stream()` 和这些 xrefs 中的一个来读取相应的内容部分。
                * :meth:`Page.set_contents()` -- 将页面的 `/Contents` 键设置为提供的 :data:`xref` 编号。

        .. data:: resources

                一个 :data:`dictionary`，包含 PDF :data:`page` （强制性，继承性，参考 :ref:`AdobeManual` 第 81 页）及某些其他对象（如表单 XObjects）所需的所有资源的引用（如图像或字体）。此字典作为对象定义中的子字典出现，位于 *Resources* 键下。作为一个继承对象类型，可能存在所有页面或某些页面子集的“父级”资源。

        .. data:: dictionary

                一个 PDF :data:`object` 类型，类似于 Python 中的同名概念：“字典对象是一个包含对象对的关联表，称为字典条目。每个条目的第一个元素是键，第二个元素是值。键必须是一个名称（...）。值可以是任何类型的对象，包括另一个字典。一个字典条目的值为 null（...）时，相当于该条目不存在。”（参考 :ref:`AdobeManual` 第 18 页）。

                字典是 PDF 中最重要的 :data:`object` 类型。以下是一个示例（描述一个 :data:`page`）：

                <<
                /Contents 40 0 R                  % 值：间接对象
                /Type/Page                        % 值：名称对象
                /MediaBox[0 0 595.32 841.92]      % 值：数组对象
                /Rotate 0                         % 值：数字对象
                /Parent 12 0 R                    % 值：间接对象
                /Resources<<                      % 值：字典对象
                        /ExtGState<</R7 26 0 R>>
                        /Font<<
                        /R8 27 0 R/R10 21 0 R/R12 24 0 R/R14 15 0 R
                        /R17 4 0 R/R20 30 0 R/R23 7 0 R /R27 20 0 R
                        >>
                        /ProcSet[/PDF/Text]           % 值：包含两个名称对象的数组
                        >>
                /Annots[55 0 R]                   % 值：数组，一个条目（间接对象）
                >>

                *Contents*、*Type*、*MediaBox* 等是 **键**，*40 0 R*、*Page*、*[0 0 595.32 841.92]* 等是各自的 **值**。字符串 *"<<"* 和 *">>"* 用于包含对象定义。

                此示例还显示了 **嵌套** 字典值的语法： *Resources* 的值是一个对象，而该对象本身是一个字典，包含如 *ExtGState* （其值为 *<</R7 26 0 R>>*，这是另一个字典）等键。

        .. data:: page

                一个 PDF 页面是一个 :data:`dictionary` 对象，定义了 PDF 中的一个页面，详见 :ref:`AdobeManual` 第 71 页。

        .. data:: pagetree

                文档的页面通过称为页面树的结构进行访问，该结构定义了文档中页面的顺序。树结构允许 PDF 消费应用程序在内存有限的情况下快速打开包含成千上万页面的文档。该树包含两种类型的节点：中间节点，称为页面树节点；叶节点，称为页面对象。 （参考 :ref:`AdobeManual` 第 75 页）。

                虽然可以通过一个数组列出所有页面引用，但包含大量页面的 PDF 通常采用 *平衡树* 结构（“页面树”），以便更快地访问任何单一页面。相对于总页面数，这可以将平均页面访问时间从线性减少到某个对数级别。

                为了实现快速页面访问，MuPDF 可以使用其自己内存中的数组 -- 独立于文档文件中可能存在或不存在的内容。该数组按页面编号索引，因此比通过完美平衡的页面树访问要快得多。

        .. data:: object

                与 Python 类似，PDF 支持 *对象* 的概念，PDF 中有八种基本类型：布尔值（“true” 或 “false”）、整数和实数、字符串（ **始终** 用括号括起来 -- 可以是“()”，或用“<>”表示十六进制）、名称（必须以“/”开头，例如 `/Contents`）、数组（用方括号“[]”括起来）、字典（用双尖括号“<<>>”括起来）、流（由“stream”和“endstream”关键词括起来）和 null 对象（“null”）（参考 :ref:`AdobeManual` 第 13 页）。对象可以通过分配标签来标识。该标签称为 *间接* 对象。PyMuPDF 支持通过交叉引用号（xref number）通过 :meth:`Document.xref_object` 获取间接对象的定义。

        .. data:: stream

                一个 PDF :data:`dictionary` :data:`object` 类型，后跟一系列字节，类似于 Python 中的 *bytes*。 “然而，PDF 应用程序可以增量地读取流，而字符串必须完全读取。此外，流的长度是无限的，而字符串受实现限制。因此，具有大量数据的对象（如图像和页面描述）通常表示为流。” “一个流由一个 :data:`dictionary` 和零个或多个字节组成，这些字节位于 *stream* 和 *endstream* 关键字之间”：

                nnn 0 obj
                <<
                字典定义
                >>
                stream
                (零个或多个字节)
                endstream
                endobj

                参见 :ref:`AdobeManual` 第 19 页。PyMuPDF 支持通过 :meth:`Document.xref_stream` 获取流内容。使用 :meth:`Document.is_stream` 判断对象是否为流类型。

        .. data:: unitvector

                一个数学概念，表示一个范数（“长度”）为 1 的向量 -- 通常隐含的是欧几里得范数。在 PyMuPDF 中，这个术语限制为 :ref:`Point` 对象，详见 :attr:`Point.unit`。

        .. data:: xref

                交叉引用编号的缩写：这是 PDF 中对象的唯一标识符。每个 PDF 中都有一个交叉引用表（可能由多个独立段组成），存储每个对象的位置，以便快速查找。交叉引用表比实际对象数多一个条目：项零是保留的，不能以任何方式使用。许多 PyMuPDF 类有一个 *xref* 属性（对于非 PDF 文件，值为零），可以通过 :meth:`Document.xref_length` *- 1* 获取 PDF 中对象的总数。

        .. data:: fontsize

                指定字体大小时，该度量单位是以点为单位，其中 1 英寸 = 72 点。

        .. data:: resolution

                图像和 :ref:`Pixmap` 对象可能包含每个方向（水平和垂直）的分辨率信息，以 "每英寸点数"（dpi）表示。当 MuPDF 从文件或 PDF 对象读取图像时，它会解析此信息并将其分别放入 :attr:`Pixmap.xres` 和 :attr:`Pixmap.yres` 中。如果输入中没有有效信息（如非正值或超过 4800 的值），它将使用“合理”的默认值。常见的默认值为 96，但在某些情况下（如 JPX 图像）也可能为 72。

        .. data:: OCPD

                可选内容属性字典 - PDF :data:`catalog` 的子 :data:`dictionary`。用于存储可选内容信息，关键字为 `/OCProperties`。该字典有两个必需条目和一个可选条目：
                
                （1） `/OCGs` ，必需，列出所有可选内容组的数组，
                
                （2） `/D` ，必需，默认的可选内容配置字典（OCCD），
                
                （3） `/Configs` ，可选，一个包含备用 OCCD 的数组。

        .. data:: OCCD

                可选内容配置字典 - 一个 PDF :data:`dictionary`，位于 PDF :data:`OCPD` 中。它存储 OCG 的开/关状态及其如何在 PDF 查看器程序中呈现。选择一个配置是快速实现临时大规模可见性状态变化的方式。打开 PDF 后，始终会激活 :data:`OCPD` 的 `/D` 配置。查看器应提供在 `/D` 或 `/Configs` 数组中切换备用配置的方式。

        .. data:: OCG

                可选内容组 -- 一个 :data:`dictionary` 对象，用于控制其他 PDF 对象（如图像或注释）的可见性。不管它们定义在哪一页，具有相同 OCG 的对象可以通过设置它们的 OCG 为 ON 或 OFF 来同时显示或隐藏。这可以通过许多 PDF 查看器（如 Adobe Acrobat）提供的用户界面，或者通过编程方式实现。

        .. data:: OCMD

                可选内容成员字典 -- 一个 :data:`dictionary` 对象，可以像 :data:`OCG` 一样使用：它有一个可见性状态。OCMD 的可见性是 **计算得出的**：它是一个逻辑表达式，使用一个或多个 OCG 的状态来生成布尔值。表达式的结果被解释为 ON（true）或 OFF（false）。

        .. data:: ligature

                一些常见的字符组合在更高级的字体中由它们自己的特殊字形表示。典型的例子有 "fi"、"fl"、"ffi" 和 "ffl"。这些组合被称为 *连字*。在 PyMuPDF 的文本提取中，可以选择返回相应的 Unicode 不变，或将连字拆分为它们的组成部分：“fi” ==> “f” + “i”等。

.. tab:: 英文

        .. data:: MediaBox
                :no-index:

                A PDF array of 4 floats specifying a physical page size -- (:data:`inheritable`, mandatory). This rectangle should contain all other PDF  -- optional -- page rectangles, which may be specified in addition: CropBox, TrimBox, ArtBox and BleedBox. Please consult :ref:`AdobeManual` for details. The MediaBox is the only rectangle, for which there is no difference between MuPDF and PDF coordinate systems: :attr:`Page.mediabox` will always show the same coordinates as the `/MediaBox` key in a page's object definition. For all other rectangles, MuPDF transforms y coordinates such that the **top** border is the point of reference. This can sometimes be confusing -- you may for example encounter a situation like this one:

                * The page definition contains the following identical values: `/MediaBox [ 36 45 607.5 765 ]`, `/CropBox [ 36 45 607.5 765 ]`.
                * PyMuPDF accordingly shows `page.mediabox = Rect(36.0, 45.0, 607.5, 765.0)`.
                * **BUT:** `page.cropbox = Rect(36.0, 0.0, 607.5, 720.0)`, because the two y-coordinates have been transformed (45 subtracted from both of them).

        .. data:: CropBox
                :no-index:

                A PDF array of 4 floats specifying a page's visible area -- (:data:`inheritable`, optional). It is the default for TrimBox, ArtBox and BleedBox. If not present, it defaults to MediaBox. This value is **not affected** if the page is rotated -- in contrast to :attr:`Page.rect`. Also, other than the page rectangle, the top-left corner of the cropbox may or may not be *(0, 0)*.


        .. data:: catalog
                :no-index:

                A central PDF :data:`dictionary` -- also called the "root" -- containing document-wide parameters and pointers to many other information. Its :data:`xref` is returned by :meth:`Document.pdf_catalog`.

        .. data:: trailer
                :no-index:

                More precisely, the **PDF trailer** contains information in :data:`dictionary` format. It is usually located at the file's end. In this dictionary, you will find things like the xrefs of the catalog and the metadata, the number of :data:`xref` numbers, etc. Here is the definition of the PDF spec:
                
                *"The trailer of a PDF file enables an application reading the file to quickly find the cross-reference table and certain special objects. Applications should read a PDF file from its end."*

                To access the trailer in PyMuPDF, use the usual methods :meth:`Document.xref_object`, :meth:`Document.xref_get_key` and :meth:`Document.xref_get_keys` with `-1` instead of a positive xref number.

        .. data:: contents
                :no-index:

                A **content stream** is a PDF :data:`object` with an attached :data:`stream`, whose data consists of a sequence of instructions describing the graphical elements to be painted on a page, see "Stream Objects" on page 19 of :ref:`AdobeManual`. For an overview of the mini-language used in these streams, see chapter "Operator Summary" on page 643 of the :ref:`AdobeManual`. A PDF :data:`page` can have none to many contents objects. If it has none, the page is empty (but still may show annotations). If it has several, they will be interpreted in sequence as if their instructions had been present in one such object (i.e. like in a concatenated string). It should be noted that there are more stream object types which use the same syntax: e.g. appearance dictionaries associated with annotations and Form XObjects.

                PyMuPDF provides a number of methods to deal with contents of PDF pages:

                * :meth:`Page.read_contents()` -- reads and concatenates all page contents into one `bytes` object.
                * :meth:`Page.clean_contents()` -- a wrapper of a MuPDF function that reads, concatenates and syntax-cleans all page contents. After this, only one `/Contents` object will exist. In addition, page :data:`resources` will have been synchronized with it such that it will contain exactly those images, fonts and other objects that the page actually references.
                * :meth:`Page.get_contents()` -- return a list of :data:`xref` numbers of a page's :data:`contents` objects. May be empty. Use :meth:`Document.xref_stream()` with one of these xrefs to read the resp. contents section.
                * :meth:`Page.set_contents()` -- set a page's `/Contents` key to the provided :data:`xref` number.

        .. data:: resources
                :no-index:

                A :data:`dictionary` containing references to any resources (like images or fonts) required by a PDF :data:`page` (required, inheritable, :ref:`AdobeManual` p. 81) and certain other objects (Form XObjects). This dictionary appears as a sub-dictionary in the object definition under the key */Resources*. Being an inheritable object type, there may exist "parent" resources for all pages or certain subsets of pages.

        .. data:: dictionary
                :no-index:

                A PDF :data:`object` type, which is somewhat comparable to the same-named Python notion: "A dictionary object is an associative table containing pairs of objects, known as the dictionary's entries. The first element of each entry is the key and the second element is the value. The key must be a name (...). The value can be any kind of object, including another dictionary. A dictionary entry whose value is null (...) is equivalent to an absent entry." (:ref:`AdobeManual` p. 18).

                Dictionaries are the most important :data:`object` type in PDF. Here is an example (describing a :data:`page`)::

                <<
                /Contents 40 0 R                  % value: an indirect object
                /Type/Page                        % value: a name object
                /MediaBox[0 0 595.32 841.92]      % value: an array object
                /Rotate 0                         % value: a number object
                /Parent 12 0 R                    % value: an indirect object
                /Resources<<                      % value: a dictionary object
                        /ExtGState<</R7 26 0 R>>
                        /Font<<
                        /R8 27 0 R/R10 21 0 R/R12 24 0 R/R14 15 0 R
                        /R17 4 0 R/R20 30 0 R/R23 7 0 R /R27 20 0 R
                        >>
                        /ProcSet[/PDF/Text]           % value: array of two name objects
                        >>
                /Annots[55 0 R]                   % value: array, one entry (indirect object)
                >>

                *Contents*, *Type*, *MediaBox*, etc. are **keys**, *40 0 R*, *Page*, *[0 0 595.32 841.92]*, etc. are the respective **values**. The strings *"<<"* and *">>"* are used to enclose object definitions.

                This example also shows the syntax of **nested** dictionary values: *Resources* has an object as its value, which in turn is a dictionary with keys like *ExtGState* (with the value *<</R7 26 0 R>>*, which is another dictionary), etc.

        .. data:: page
                :no-index:

                A PDF page is a :data:`dictionary` object which defines one page in a PDF, see :ref:`AdobeManual` p. 71.

        .. data:: pagetree
                :no-index:

                The pages of a document are accessed through a structure known as the page tree, which defines the ordering of pages in the document. The tree structure allows PDF consumer applications, using only limited memory, to quickly open a document containing thousands of pages. The tree contains nodes of two types: intermediate nodes, called page tree nodes, and leaf nodes, called page objects. (:ref:`AdobeManual` p. 75).

                While it is possible to list all page references in just one array, PDFs with many pages are often created using *balanced tree* structures ("page trees") for faster access to any single page. In relation to the total number of pages, this can reduce the average page access time by page number from a linear to some logarithmic order of magnitude.

                For fast page access, MuPDF can use its own array in memory -- independently from what may or may not be present in the document file. This array is indexed by page number and therefore much faster than even the access via a perfectly balanced page tree.

        .. data:: object
                :no-index:

                Similar to Python, PDF supports the notion *object*, which can come in eight basic types: boolean values ("true" or "false"), integer and real numbers, strings (**always** enclosed in brackets -- either "()", or "<>" to indicate hexadecimal), names (must always start with a "/", e.g. `/Contents`), arrays (enclosed in brackets "[]"), dictionaries (enclosed in brackets "<<>>"), streams (enclosed by keywords "stream" / "endstream"), and the null object ("null") (:ref:`AdobeManual` p. 13). Objects can be made identifiable by assigning a label. This label is then called *indirect* object. PyMuPDF supports retrieving definitions of indirect objects via their cross reference number via :meth:`Document.xref_object`.

        .. data:: stream
                :no-index:

                A PDF :data:`dictionary` :data:`object` type which is followed by a sequence of bytes, similar to Python *bytes*. "However, a PDF application can read a stream incrementally, while a string must be read in its entirety. Furthermore, a stream can be of unlimited length, whereas a string is subject to an implementation limit. For this reason, objects with potentially large amounts of data, such as images and page descriptions, are represented as streams." "A stream consists of a :data:`dictionary` followed by zero or more bytes bracketed between the keywords *stream* and *endstream*"::

                nnn 0 obj
                <<
                dictionary definition
                >>
                stream
                (zero or more bytes)
                endstream
                endobj

                See :ref:`AdobeManual` p. 19. PyMuPDF supports retrieving stream content via :meth:`Document.xref_stream`. Use :meth:`Document.is_stream` to determine whether an object is of stream type.

        .. data:: unitvector
                :no-index:

                A mathematical notion meaning a vector of norm ("length") 1 -- usually the Euclidean norm is implied. In PyMuPDF, this term is restricted to :ref:`Point` objects, see :attr:`Point.unit`.

        .. data:: xref
                :no-index:

                Abbreviation for cross-reference number: this is an integer unique identification for objects in a PDF. There exists a cross-reference table (which may physically consist of several separate segments) in each PDF, which stores the relative position of each object for quick lookup. The cross-reference table is one entry longer than the number of existing object: item zero is reserved and must not be used in any way. Many PyMuPDF classes have an *xref* attribute (which is zero for non-PDFs), and one can find out the total number of objects in a PDF via :meth:`Document.xref_length` *- 1*.


        .. data:: fontsize
                :no-index:

                When referring to font size this metric is measured in points where 1 inch = 72 points.

        .. data:: resolution
                :no-index:

                Images and :ref:`Pixmap` objects may contain resolution information provided as "dots per inch", dpi, in each direction (horizontal and vertical). When MuPDF reads an image from a file or from a PDF object, it will parse this information and put it in :attr:`Pixmap.xres`, :attr:`Pixmap.yres`, respectively. If it finds no meaningful information in the input (like non-positive values or values exceeding 4800), it will use "sane" defaults instead. The usual default value is 96, but it may also be 72 in some cases (e.g. for JPX images).

        .. data:: OCPD
                :no-index:

                Optional content properties dictionary - a sub :data:`dictionary` of the PDF :data:`catalog`. The central place to store optional content information, which is identified by the key `/OCProperties`. This dictionary has two required and one optional entry: (1) `/OCGs`, required, an array listing all optional content groups, (2) `/D`, required, the default optional content configuration dictionary (OCCD), (3) `/Configs`, optional, an array of alternative OCCDs.


        .. data:: OCCD
                :no-index:

                Optional content configuration dictionary - a PDF :data:`dictionary` inside the PDF :data:`OCPD`. It stores a setting of ON / OFF states of OCGs and how they are presented to a PDF viewer program. Selecting a configuration is quick way to achieve temporary mass visibility state changes. After opening a PDF, the `/D` configuration of the :data:`OCPD` is always activated. Viewer should offer a way to switch between the `/D`, or one of the optional configurations contained in array `/Configs`.


        .. data:: OCG
                :no-index:

                Optional content group -- a :data:`dictionary` object used to control the visibility of other PDF objects like images or annotations. Independently on which page they are defined, objects with the same OCG can simultaneously be shown or hidden by setting their OCG to ON or OFF. This can be achieved via the user interface provided by many PDF viewers (Adobe Acrobat), or programmatically.

        .. data:: OCMD
                :no-index:
                
                Optional content membership dictionary -- a :data:`dictionary` object which can be used like an :data:`OCG`: it has a visibility state. The visibility of an OCMD is **computed:** it is a logical expression, which uses the state of one or more OCGs to produce a boolean value. The expression's result is interpreted as ON (true) or OFF (false).

        .. data:: ligature
                :no-index:
                
                Some frequent character combinations are represented by their own special glyphs in more advanced fonts. Typical examples are "fi", "fl", "ffi" and "ffl". These compounds are called *ligatures*. In PyMuPDF text extractions, there is the option to either return the corresponding unicode unchanged, or split ligatures up into their constituent parts: "fi" ==> "f" + "i", etc.

.. include:: footer.rst
