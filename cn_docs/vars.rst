.. include:: header.rst

===============================
常量和枚举
===============================

Constants and Enumerations

.. tab:: 中文

    由 |PyMuPDF| 实现的 MuPDF 的常量和枚举。以下每个值都可以作为 `pymupdf.value` 访问。

.. tab:: 英文

    Constants and enumerations of :title:`MuPDF` as implemented by |PyMuPDF|. Each of the following values is accessible as `pymupdf.value`.


常量
---------

Constants

.. tab:: 中文

    .. py:data:: Base14_Fonts

        预定义的有效 :ref:`Base-14-Fonts` Python 列表。

        :type: list

    .. py:data:: csRGB

        预定义的 RGB 色彩空间 *pymupdf.Colorspace(pymupdf.CS_RGB)*。

        :type: :ref:`Colorspace`

    .. py:data:: csGRAY

        预定义的 GRAY 色彩空间 *pymupdf.Colorspace(pymupdf.CS_GRAY)*。

        :type: :ref:`Colorspace`

    .. py:data:: csCMYK

        预定义的 CMYK 色彩空间 *pymupdf.Colorspace(pymupdf.CS_CMYK)*。

        :type: :ref:`Colorspace`

    .. py:data:: CS_RGB

        1 -- :ref:`Colorspace` 类型为 RGBA。

        :type: int

    .. py:data:: CS_GRAY

        2 -- :ref:`Colorspace` 类型为 GRAY。

        :type: int

    .. py:data:: CS_CMYK

        3 -- :ref:`Colorspace` 类型为 CMYK。

        :type: int

    .. py:data:: mupdf_version

        'x.xx.x' -- 当前 PyMuPDF 使用的 MuPDF 版本。

        :type: string

    .. py:data:: mupdf_version_tuple

        MuPDF 版本，表示为一个整数元组 `(major, minor, patch)`。
        
        :type: tuple

    .. py:data:: pymupdf_version

        'x.xx.x' -- 当前 PyMuPDF 版本。

        :type: string

    .. py:data:: pymupdf_version_tuple

        PyMuPDF 版本，表示为一个整数元组 `(major, minor, patch)`。
        
        :type: tuple

    .. py:data:: pymupdf_date

        这些绑定构建时的 ISO 时间戳 *YYYY-MM-DD HH:MM:SS*。

        :type: string

    .. py:data:: version

        (pymupdf_version, mupdf_version, timestamp) -- 组合的版本信息，其中 `timestamp` 是以 "YYYYMMDDhhmmss" 格式表示的生成时间。

        :type: tuple

    .. py:data:: VersionBind

        旧版等同于 `mupdf_version`。

    .. py:data:: VersionFitz

        旧版等同于 `pymupdf_version`。

    .. py:data:: VersionDate

        旧版等同于 `mupdf_version`。

.. tab:: 英文

    .. py:data:: Base14_Fonts
        :no-index:

        Predefined Python list of valid :ref:`Base-14-Fonts`.

        :type: list

    .. py:data:: csRGB
        :no-index:

        Predefined RGB colorspace *pymupdf.Colorspace(pymupdf.CS_RGB)*.

        :type: :ref:`Colorspace`

    .. py:data:: csGRAY
        :no-index:

        Predefined GRAY colorspace *pymupdf.Colorspace(pymupdf.CS_GRAY)*.

        :type: :ref:`Colorspace`

    .. py:data:: csCMYK
        :no-index:

        Predefined CMYK colorspace *pymupdf.Colorspace(pymupdf.CS_CMYK)*.

        :type: :ref:`Colorspace`

    .. py:data:: CS_RGB
        :no-index:

        1 -- Type of :ref:`Colorspace` is RGBA

        :type: int

    .. py:data:: CS_GRAY
        :no-index:

        2 -- Type of :ref:`Colorspace` is GRAY

        :type: int

    .. py:data:: CS_CMYK
        :no-index:

        3 -- Type of :ref:`Colorspace` is CMYK

        :type: int

    .. py:data:: mupdf_version
        :no-index:

        'x.xx.x' -- MuPDF version that is being used by PyMuPDF.

        :type: string

    .. py:data:: mupdf_version_tuple
        :no-index:

        MuPDF version as a tuple of integers, `(major, minor, patch)`.
        
        :type: tuple

    .. py:data:: pymupdf_version
        :no-index:

        'x.xx.x' -- PyMuPDF version.

        :type: string

    .. py:data:: pymupdf_version_tuple
        :no-index:

        PyMuPDF version as a tuple of integers, `(major, minor, patch)`.
        
        :type: tuple

    .. py:data:: pymupdf_date
        :no-index:

        ISO timestamp *YYYY-MM-DD HH:MM:SS* when these bindings were built.

        :type: string

    .. py:data:: version
        :no-index:

        (pymupdf_version, mupdf_version, timestamp) -- combined version information where `timestamp` is the generation point in time formatted as "YYYYMMDDhhmmss".

        :type: tuple

    .. py:data:: VersionBind
        :no-index:

        Legacy equivalent to `mupdf_version`.

    .. py:data:: VersionFitz
        :no-index:

        Legacy equivalent to `pymupdf_version`.

    .. py:data:: VersionDate
        :no-index:

        Legacy equivalent to `mupdf_version`.


.. _PermissionCodes:

文档权限
----------------------------

Document Permissions

.. tab:: 中文

    ====================== =======================================================================
    Code                   Permitted Action
    ====================== =======================================================================
    PDF_PERM_PRINT         打印文档
    PDF_PERM_MODIFY        修改文档内容
    PDF_PERM_COPY          复制或提取文本和图形
    PDF_PERM_ANNOTATE      添加或修改文本注释和交互式表单字段
    PDF_PERM_FORM          填写表单并签署文档
    PDF_PERM_ACCESSIBILITY 废弃，始终允许
    PDF_PERM_ASSEMBLE      插入、旋转或删除页面、书签、缩略图
    PDF_PERM_PRINT_HQ      高质量打印
    ====================== =======================================================================


.. tab:: 英文

    ====================== =======================================================================
    Code                   Permitted Action
    ====================== =======================================================================
    PDF_PERM_PRINT         Print the document
    PDF_PERM_MODIFY        Modify the document's contents
    PDF_PERM_COPY          Copy or otherwise extract text and graphics
    PDF_PERM_ANNOTATE      Add or modify text annotations and interactive form fields
    PDF_PERM_FORM          Fill in forms and sign the document
    PDF_PERM_ACCESSIBILITY Obsolete, always permitted
    PDF_PERM_ASSEMBLE      Insert, rotate, or delete pages, bookmarks, thumbnail images
    PDF_PERM_PRINT_HQ      High quality printing
    ====================== =======================================================================

.. _OptionalContentCodes:

PDF可选内容代码
----------------------------

PDF Optional Content Codes

.. tab:: 中文

    ====================== =======================================================================
    代码                   含义
    ====================== =======================================================================
    PDF_OC_ON              暂时将 OCG 设置为 ON
    PDF_OC_TOGGLE          暂时切换 OCG 状态
    PDF_OC_OFF             暂时将 OCG 设置为 OFF
    ====================== =======================================================================

.. tab:: 英文

    ====================== =======================================================================
    Code                   Meaning
    ====================== =======================================================================
    PDF_OC_ON              Set an OCG to ON temporarily
    PDF_OC_TOGGLE          Toggle OCG status temporarily
    PDF_OC_OFF             Set an OCG to OFF temporarily
    ====================== =======================================================================

.. _EncryptionMethods:

PDF 加密方法代码
----------------------------

PDF encryption method codes

.. tab:: 中文

    =================== ====================================================
    Code                Meaning
    =================== ====================================================
    PDF_ENCRYPT_KEEP    不更改
    PDF_ENCRYPT_NONE    移除任何加密
    PDF_ENCRYPT_RC4_40  RC4 40 位
    PDF_ENCRYPT_RC4_128 RC4 128 位
    PDF_ENCRYPT_AES_128 *高级加密标准* 128 位
    PDF_ENCRYPT_AES_256 *高级加密标准* 256 位
    PDF_ENCRYPT_UNKNOWN 未知
    =================== ====================================================

.. tab:: 英文

    =================== ====================================================
    Code                Meaning
    =================== ====================================================
    PDF_ENCRYPT_KEEP    do not change
    PDF_ENCRYPT_NONE    remove any encryption
    PDF_ENCRYPT_RC4_40  RC4 40 bit
    PDF_ENCRYPT_RC4_128 RC4 128 bit
    PDF_ENCRYPT_AES_128 *Advanced Encryption Standard* 128 bit
    PDF_ENCRYPT_AES_256 *Advanced Encryption Standard* 256 bit
    PDF_ENCRYPT_UNKNOWN unknown
    =================== ====================================================

.. _FontExtensions:

字体文件扩展名
-----------------------

Font File Extensions

.. tab:: 中文

    下表显示了保存从 PDF 中提取的字体文件缓冲区时应使用的文件扩展名。此字符串由 :meth:`Document.get_page_fonts`、:meth:`Page.get_fonts` 和 :meth:`Document.extract_font` 返回。

    ==== ============================================================================
    Ext  Description
    ==== ============================================================================
    ttf  TrueType 字体
    pfa  Postscript ASCII 字体（各种子类型）
    cff  Type1C 字体（等同于压缩字体 Type1）
    cid  字符标识符字体（Postscript 格式）
    otf  OpenType 字体
    n/a  无法提取，例如 :ref:`Base-14-Fonts` 、Type 3 字体及其他
    ==== ============================================================================


.. tab:: 英文

    The table show file extensions you should use when saving fontfile buffers extracted from a PDF. This string is returned by :meth:`Document.get_page_fonts`, :meth:`Page.get_fonts` and :meth:`Document.extract_font`.

    ==== ============================================================================
    Ext  Description
    ==== ============================================================================
    ttf  TrueType font
    pfa  Postscript for ASCII font (various subtypes)
    cff  Type1C font (compressed font equivalent to Type1)
    cid  character identifier font (postscript format)
    otf  OpenType font
    n/a  not extractable, e.g. :ref:`Base-14-Fonts`, Type 3 fonts and others
    ==== ============================================================================

.. _TextAlign:

文本对齐
-----------------------

Text Alignment

.. tab:: 中文

    .. py:data:: TEXT_ALIGN_LEFT

        0 -- 左对齐.

    .. py:data:: TEXT_ALIGN_CENTER

        1 -- 中间对齐.

    .. py:data:: TEXT_ALIGN_RIGHT

        2 -- 右对齐.

    .. py:data:: TEXT_ALIGN_JUSTIFY

        3 -- 平均对齐.

.. tab:: 英文

    .. py:data:: TEXT_ALIGN_LEFT
        :no-index:

        0 -- align left.

    .. py:data:: TEXT_ALIGN_CENTER
        :no-index:

        1 -- align center.

    .. py:data:: TEXT_ALIGN_RIGHT
        :no-index:

        2 -- align right.

    .. py:data:: TEXT_ALIGN_JUSTIFY
        :no-index:

        3 -- align justify.

.. _TextPreserve:

.. _FontProperties:

字体属性
-----------------------

Font Properties

.. tab:: 中文

    请注意，以下位是从字体的属性中推导出来的。它可能不完全准确（实际上，通常并不准确）。

    .. py:data:: TEXT_FONT_SUPERSCRIPT

        1 -- 字符或范围是上标。此属性由 MuPDF 计算，而不是字体信息的一部分。

    .. py:data:: TEXT_FONT_ITALIC

        2 -- 字体是斜体。

    .. py:data:: TEXT_FONT_SERIFED

        4 -- 字体有衬线。

    .. py:data:: TEXT_FONT_MONOSPACED

        8 -- 字体是等宽的。

    .. py:data:: TEXT_FONT_BOLD

        16 -- 字体是加粗的。


.. tab:: 英文

    Please note that the following bits are derived from what a font has to say about its properties. It may not be (and quite often is not) correct.

    .. py:data:: TEXT_FONT_SUPERSCRIPT
        :no-index:

        1 -- the character or span is a superscript. This property is computed by MuPDF and not part of any font information.

    .. py:data:: TEXT_FONT_ITALIC
        :no-index:

        2 -- the font is italic.

    .. py:data:: TEXT_FONT_SERIFED
        :no-index:

        4 -- the font is serifed.

    .. py:data:: TEXT_FONT_MONOSPACED
        :no-index:

        8 -- the font is mono-spaced.

    .. py:data:: TEXT_FONT_BOLD
        :no-index:

        16 -- the font is bold.

文本提取标志
---------------------

Text Extraction Flags

.. tab:: 中文

    控制解析到 :ref:`TextPage` 的数据量的选项位。

    对于 PyMuPDF 程序员，可以通过 Python 的 `|` 操作符，或简单地使用 `+` 将这些值的组合聚合成 `flags` 整数，这是所有文本搜索和文本提取方法的参数。根据不同的方法，会使用不同的默认值组合。请使用适合您情况的值。特别是，除非您确实需要图像提取，否则务必关闭图像提取。因为它对性能和内存的影响非常显著！

    .. py:data:: TEXT_PRESERVE_LIGATURES

        1 -- 如果设置，则连字将以其原始形式传递给应用程序。否则，连字将展开成其组成部分，例如连字 "ffi" 展开为三个字符 f、f 和 i。在 PyMuPDF 中，默认是“开启”的。MuPDF 支持以下 7 个连字：“ff”、“fi”、“fl”、“ffi”、“ffl”、“ft”、“st”。

    .. py:data:: TEXT_PRESERVE_WHITESPACE

        2 -- 如果设置，空格会被保留。否则，任何类型的水平空白（包括水平制表符）将被替换为可变宽度的空格字符。默认在 PyMuPDF 中是“开启”的。

    .. py:data:: TEXT_PRESERVE_IMAGES

        4 -- 如果设置，则图像将存储在 :ref:`TextPage` 中。这会导致在 "blocks"、"dict"、"json"、"rawdict"、"rawjson"、"html" 和 "xhtml" 类型的文本提取输出中存在（通常是大容量的！）二进制图像内容，并且默认情况下会启用。如果与 "blocks" 一起使用，返回的只是图像的元数据，而不是图像本身。

    .. py:data:: TEXT_INHIBIT_SPACES

        8 -- 如果设置，MuPDF 将不会尝试在字符之间存在大间隙时添加缺失的空格字符。在 PDF 中，创建者通常不会插入空格来指向下一个字符的位置，而是提供直接的位置信息。PyMuPDF 中的默认设置是“关闭”--因此会生成空格。

    .. py:data:: TEXT_DEHYPHENATE

        16 -- 忽略行末的连字符并与下一行合并。内部用于文本搜索功能。但是，它通常是可用的：如果开启，文本提取将返回合并的文本行（或跨度），并去除第一行末尾的连字符。因此，两个分开的跨度 **"first meth-"** 和 **"od leads to wrong results"** 会合并为一个跨度 **"first method leads to wrong results"**，并相应更新边界框：结果跨度的字符将不再具有相同的 y 坐标。

    .. py:data:: TEXT_PRESERVE_SPANS

        32 -- 为每个跨度生成新的一行。PyMuPDF 中未使用（"关闭"），但可以使用。在 "dict"、"json"、"rawdict"、"rawjson" 中，每行将包含确切的一个跨度。

    .. py:data:: TEXT_MEDIABOX_CLIP

        64 -- 完全位于页面 **mediabox** 之外的字符或位于其他“裁剪”区域中的字符将被忽略。这是 PyMuPDF 中的默认设置。

    .. py:data:: TEXT_CID_FOR_UNKNOWN_UNICODE

        128 -- 使用原始字符代码代替 U+FFFD。这是 PyMuPDF 中文本提取的默认设置。如果您 **想要检测** 编码信息缺失或不确定的情况，可以切换此标志并扫描结果文本中是否存在 U+FFFD（= `chr(0xfffd)`）码点。

    .. py:data:: TEXT_COLLECT_STRUCTURE

        256 -- 不支持。

    .. py:data:: TEXT_ACCURATE_BBOXES

        512 -- 计算字符边界框时忽略所有字体的度量值，最突出的是 `ascender <https://en.wikipedia.org/wiki/Ascender_(typography)>`_ 和 `descender <https://en.wikipedia.org/wiki/Descender>`_ 值。相反，遵循每个字符的字形绘制命令并计算其矩形外壳。这是包装所有绘制视觉外观点的最小矩形 - 有关详细背景，请参见 :ref:`Shape` 类。尤其会导致单个字符的高度。例如，空格字符（白色）将有一个 **高度为 0** 的边界框（因为没有绘制任何内容） -- 与使用字体度量时生成的非零边界框相反。此选项可能对处理即使在包含错误的字体中仍能获取有意义的边界框有所帮助。由于涉及的计算工作，这将稍微减慢文本提取过程。

        注意，默认情况下没有效果 - 必须禁用全局四边形修正设置，使用 `pymupdf.TOOLS.unset_quad_corrections(True)`。

    .. py:data:: TEXT_COLLECT_VECTORS

        1024 -- 不支持。

    .. py:data:: TEXT_IGNORE_ACTUALTEXT

        2048 -- 忽略例如 PDF 查看器中显示的文本与 PDF 中存储的文本之间的内置差异。有关背景，请参见 :ref:`AdobeManual` 第 615 页。如果设置，则忽略 **存储的**（“替代”）文本，使用显示的文本。

    .. py:data:: TEXT_STEXT_SEGMENT

        4096 -- 尝试将页面分割为不同的区域。

    以下常量表示用于文本提取和搜索的默认组合：


.. tab:: 英文

    Option bits controlling the amount of data, that are parsed into a :ref:`TextPage`.

    For the PyMuPDF programmer, some combination (using Python's `|` operator, or simply use `+`) of these values are aggregated in the ``flags`` integer, a parameter of all text search and text extraction methods. Depending on the individual method, different default combinations of the values are used. Please use a value that meets your situation. Especially make sure to switch off image extraction unless you really need them. The impact on performance and memory is significant!

    .. py:data:: TEXT_PRESERVE_LIGATURES
        :no-index:

        1 -- If set, ligatures are passed through to the application in their original form. Otherwise ligatures are expanded into their constituent parts, e.g. the ligature "ffi" is expanded into three  eparate characters f, f and i. Default is "on" in PyMuPDF. MuPDF supports the following 7 ligatures: "ff", "fi", "fl", "ffi", "ffl", , "ft", "st".

    .. py:data:: TEXT_PRESERVE_WHITESPACE
        :no-index:

        2 -- If set, whitespace is passed through. Otherwise any type of horizontal whitespace (including horizontal tabs) will be replaced with space characters of variable width. Default is "on" in PyMuPDF.

    .. py:data:: TEXT_PRESERVE_IMAGES
        :no-index:

        4 -- If set, then images will be stored in the :ref:`TextPage`. This causes the presence of (usually large!) binary image content in the output of text extractions of types "blocks", "dict", "json", "rawdict", "rawjson", "html", and "xhtml" and is the default there. If used with "blocks" however, only image metadata will be returned, not the image itself.

    .. py:data:: TEXT_INHIBIT_SPACES
        :no-index:

        8 -- If set, Mupdf will not try to add missing space characters where there are large gaps between characters. In PDF, the creator often does not insert spaces to point to the next character's position, but will provide the direct location address. The default in PyMuPDF is "off" -- so spaces **will be generated**.

    .. py:data:: TEXT_DEHYPHENATE
        :no-index:

        16 -- Ignore hyphens at line ends and join with next line. Used internally with the text search functions. However, it is generally available: if on, text extractions will return joined text lines (or spans) with the ending hyphen of the first line eliminated. So two separate spans **"first meth-"** and **"od leads to wrong results"** on different lines will be joined to one span **"first method leads to wrong results"** and correspondingly updated bboxes: the characters of the resulting span will no longer have identical y-coordinates.

    .. py:data:: TEXT_PRESERVE_SPANS
        :no-index:

        32 -- Generate a new line for every span. Not used ("off") in PyMuPDF, but available for your use. Every line in "dict", "json", "rawdict", "rawjson" will contain exactly one span.

    .. py:data:: TEXT_MEDIABOX_CLIP
        :no-index:

        64 -- Characters entirely outside a page's **mediabox** or contained in other "clipped" areas will be ignored. This is default in PyMuPDF.

    .. py:data:: TEXT_CID_FOR_UNKNOWN_UNICODE
        :no-index:

        128 -- Use raw character codes instead of U+FFFD. This is the default for **text extraction** in PyMuPDF. If you **want to detect** when encoding information is missing or uncertain, toggle this flag and scan for the presence of U+FFFD (= `chr(0xfffd)`) code points in the resulting text.

    .. py:data:: TEXT_COLLECT_STRUCTURE
        :no-index:

        256 -- Not supported.

    .. py:data:: TEXT_ACCURATE_BBOXES
        :no-index:

        512 -- Ignore metric values of all fonts when computing character boundary boxes -- most prominently the `ascender <https://en.wikipedia.org/wiki/Ascender_(typography)>`_ and `descender <https://en.wikipedia.org/wiki/Descender>`_ values. Instead, follow the drawing commands of each character's glyph and compute its rectangle hull. This is the smallest rectangle wrapping all points used for drawing the visual appearance - see the :ref:`Shape` class for understanding the background. This will especially result in individual character heights. For instance a (white) space will have a **bbox of height 0** (because nothing is drawn) -- in contrast to the non-zero boundary box generated when using font metrics. This option may be useful to cope with getting meaningful boundary boxes even for fonts containing errors. Its use will slow down text extraction somewhat because of the incurred computational effort.
        
        Note that this has no effect by default - one must also disable the global
        quad corrections setting with `pymupdf.TOOLS.unset_quad_corrections(True)`.

    .. py:data:: TEXT_COLLECT_VECTORS
        :no-index:

        1024 -- Not supported.

    .. py:data:: TEXT_IGNORE_ACTUALTEXT
        :no-index:

        2048 -- Ignore built-in differences between text appearing in e.g. PDF viewers versus text stored in the PDF. See :ref:`AdobeManual`, page 615 for background. If set, the **stored** ("replacement" text) is ignored in favor of the displayed text.

    .. py:data:: TEXT_STEXT_SEGMENT
        :no-index:

        4096 -- Attempt to segment page into different regions.

    The following constants represent the default combinations of the above for text extraction and searching:

.. py:data:: TEXTFLAGS_TEXT

    `TEXT_PRESERVE_LIGATURES | TEXT_PRESERVE_WHITESPACE | TEXT_MEDIABOX_CLIP | TEXT_CID_FOR_UNKNOWN_UNICODE`

.. py:data:: TEXTFLAGS_WORDS

    `TEXT_PRESERVE_LIGATURES | TEXT_PRESERVE_WHITESPACE | TEXT_MEDIABOX_CLIP | TEXT_CID_FOR_UNKNOWN_UNICODE`

.. py:data:: TEXTFLAGS_BLOCKS

    `TEXT_PRESERVE_LIGATURES | TEXT_PRESERVE_WHITESPACE | TEXT_MEDIABOX_CLIP | TEXT_CID_FOR_UNKNOWN_UNICODE`

.. py:data:: TEXTFLAGS_DICT

    `TEXT_PRESERVE_LIGATURES | TEXT_PRESERVE_WHITESPACE | TEXT_MEDIABOX_CLIP | TEXT_PRESERVE_IMAGES | TEXT_CID_FOR_UNKNOWN_UNICODE`

.. py:data:: TEXTFLAGS_RAWDICT

    `TEXT_PRESERVE_LIGATURES | TEXT_PRESERVE_WHITESPACE | TEXT_MEDIABOX_CLIP | TEXT_PRESERVE_IMAGES | TEXT_CID_FOR_UNKNOWN_UNICODE`

.. py:data:: TEXTFLAGS_HTML

    `TEXT_PRESERVE_LIGATURES | TEXT_PRESERVE_WHITESPACE | TEXT_MEDIABOX_CLIP | TEXT_PRESERVE_IMAGES | TEXT_CID_FOR_UNKNOWN_UNICODE`

.. py:data:: TEXTFLAGS_XHTML

    `TEXT_PRESERVE_LIGATURES | TEXT_PRESERVE_WHITESPACE | TEXT_MEDIABOX_CLIP | TEXT_PRESERVE_IMAGES | TEXT_CID_FOR_UNKNOWN_UNICODE`

.. py:data:: TEXTFLAGS_XML

    `TEXT_PRESERVE_LIGATURES | TEXT_PRESERVE_WHITESPACE | TEXT_MEDIABOX_CLIP | TEXT_CID_FOR_UNKNOWN_UNICODE`

.. py:data:: TEXTFLAGS_SEARCH

    `TEXT_PRESERVE_WHITESPACE | TEXT_MEDIABOX_CLIP | TEXT_DEHYPHENATE`


.. _linkDest Kinds:

链接目标类型
-----------------------

Link Destination Kinds

.. tab:: 中文

    :attr:`linkDest.kind` （链接目标类型）的可能值：

    .. py:data:: LINK_NONE

        0 -- 无目标。表示一个虚拟链接。

        :type: int

    .. py:data:: LINK_GOTO

        1 -- 指向当前文档中的某个位置。

        :type: int

    .. py:data:: LINK_URI

        2 -- 指向 URI -- 通常是使用互联网语法指定的资源。
        
        * PyMuPDF 将任何包含冒号且不以 `file:` 开头的外部链接视为 `LINK_URI`。

        :type: int

    .. py:data:: LINK_LAUNCH

        3 -- 启动（打开）另一个文件（任何“可执行”类型）。

        * PyMuPDF 将任何以 `file:` 开头或不包含冒号的外部链接视为 `LINK_LAUNCH`。

        :type: int

    .. py:data:: LINK_NAMED

        4 -- 指向一个命名位置。

        :type: int

    .. py:data:: LINK_GOTOR

        5 -- 指向另一个 PDF 文档中的某个位置。

        :type: int


.. tab:: 英文

    Possible values of :attr:`linkDest.kind` (link destination kind).

    .. py:data:: LINK_NONE
        :no-index:

        0 -- No destination. Indicates a dummy link.

        :type: int

    .. py:data:: LINK_GOTO
        :no-index:

        1 -- Points to a place in this document.

        :type: int

    .. py:data:: LINK_URI
        :no-index:

        2 -- Points to a URI -- typically a resource specified with internet syntax.
        
        * PyMuPDF treats any external link that contains a colon and does not start
        with `file:`, as `LINK_URI`.

        :type: int

    .. py:data:: LINK_LAUNCH
        :no-index:

        3 -- Launch (open) another file (of any "executable" type).
        
        * |PyMuPDF| treats any external link that starts with `file:` or doesn't
        contain a colon, as `LINK_LAUNCH`.

        :type: int

    .. py:data:: LINK_NAMED
        :no-index:

        4 -- points to a named location.

        :type: int

    .. py:data:: LINK_GOTOR
        :no-index:

        5 -- Points to a place in another PDF document.

        :type: int

.. _linkDest Flags:

链接目标标志
-------------------------

Link Destination Flags

.. tab:: 中文

    .. Note:: 该整数的最右边字节是一个位字段，因此可以使用 *&* 运算符来测试这些位的真值。

    .. py:data:: LINK_FLAG_L_VALID

        1  (bit 0) 左上角 x 值有效

        :type: bool

    .. py:data:: LINK_FLAG_T_VALID

        2  (bit 1) 左上角 y 值有效

        :type: bool

    .. py:data:: LINK_FLAG_R_VALID

        4  (bit 2) 右下角 x 值有效

        :type: bool

    .. py:data:: LINK_FLAG_B_VALID

        8  (bit 3) 右下角 y 值有效

        :type: bool

    .. py:data:: LINK_FLAG_FIT_H

        16 (bit 4) 水平适配

        :type: bool

    .. py:data:: LINK_FLAG_FIT_V

        32 (bit 5) 垂直适配

        :type: bool

    .. py:data:: LINK_FLAG_R_IS_ZOOM

        64 (bit 6) 右下角 x 值是一个缩放值

        :type: bool


.. tab:: 英文

    .. Note:: The rightmost byte of this integer is a bit field, so test the truth of these bits with the *&* operator.

    .. py:data:: LINK_FLAG_L_VALID
        :no-index:

        1  (bit 0) Top left x value is valid

        :type: bool

    .. py:data:: LINK_FLAG_T_VALID
        :no-index:

        2  (bit 1) Top left y value is valid

        :type: bool

    .. py:data:: LINK_FLAG_R_VALID
        :no-index:

        4  (bit 2) Bottom right x value is valid

        :type: bool

    .. py:data:: LINK_FLAG_B_VALID
        :no-index:

        8  (bit 3) Bottom right y value is valid

        :type: bool

    .. py:data:: LINK_FLAG_FIT_H
        :no-index:

        16 (bit 4) Horizontal fit

        :type: bool

    .. py:data:: LINK_FLAG_FIT_V
        :no-index:

        32 (bit 5) Vertical fit

        :type: bool

    .. py:data:: LINK_FLAG_R_IS_ZOOM
        :no-index:

        64 (bit 6) Bottom right x is a zoom figure

        :type: bool


注释相关常量
-----------------------------

Annotation Related Constants

.. tab:: 中文

    有关详细信息，请参阅： :ref:`AdobeManual` 的第 8.4.5 章第 615 页。

.. tab:: 英文

    See chapter 8.4.5, pp. 615 of the :ref:`AdobeManual` for details.

.. _AnnotationTypes:

注释类型
~~~~~~~~~~~~~~~~~

Annotation Types

.. tab:: 中文

    这些标识符还包括 **链接** 和 **小部件** ：PDF 规范在技术上以相同的方式处理它们，而 **MuPDF** （和 PyMuPDF）将它们视为三种基本上不同类型的对象。

.. tab:: 英文

    These identifiers also cover **links** and **widgets**: the PDF specification technically handles them all in the same way, whereas **MuPDF** (and PyMuPDF) treats them as three basically different types of objects.

::

    PDF_ANNOT_TEXT 0
    PDF_ANNOT_LINK 1  # <=== Link object in PyMuPDF
    PDF_ANNOT_FREE_TEXT 2
    PDF_ANNOT_LINE 3
    PDF_ANNOT_SQUARE 4
    PDF_ANNOT_CIRCLE 5
    PDF_ANNOT_POLYGON 6
    PDF_ANNOT_POLY_LINE 7
    PDF_ANNOT_HIGHLIGHT 8
    PDF_ANNOT_UNDERLINE 9
    PDF_ANNOT_SQUIGGLY 10
    PDF_ANNOT_STRIKE_OUT 11
    PDF_ANNOT_REDACT 12
    PDF_ANNOT_STAMP 13
    PDF_ANNOT_CARET 14
    PDF_ANNOT_INK 15
    PDF_ANNOT_POPUP 16
    PDF_ANNOT_FILE_ATTACHMENT 17
    PDF_ANNOT_SOUND 18
    PDF_ANNOT_MOVIE 19
    PDF_ANNOT_RICH_MEDIA 20
    PDF_ANNOT_WIDGET 21  # <=== Widget object in PyMuPDF
    PDF_ANNOT_SCREEN 22
    PDF_ANNOT_PRINTER_MARK 23
    PDF_ANNOT_TRAP_NET 24
    PDF_ANNOT_WATERMARK 25
    PDF_ANNOT_3D 26
    PDF_ANNOT_PROJECTION 27
    PDF_ANNOT_UNKNOWN -1

.. _AnnotationFlags:

注释标志位
~~~~~~~~~~~~~~~~~~~~~

Annotation Flag Bits

::

    PDF_ANNOT_IS_INVISIBLE 1 << (1-1)
    PDF_ANNOT_IS_HIDDEN 1 << (2-1)
    PDF_ANNOT_IS_PRINT 1 << (3-1)
    PDF_ANNOT_IS_NO_ZOOM 1 << (4-1)
    PDF_ANNOT_IS_NO_ROTATE 1 << (5-1)
    PDF_ANNOT_IS_NO_VIEW 1 << (6-1)
    PDF_ANNOT_IS_READ_ONLY 1 << (7-1)
    PDF_ANNOT_IS_LOCKED 1 << (8-1)
    PDF_ANNOT_IS_TOGGLE_NO_VIEW 1 << (9-1)
    PDF_ANNOT_IS_LOCKED_CONTENTS 1 << (10-1)

.. _AnnotationLineEnds:

注释行尾样式
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Annotation Line Ending Styles

::

    PDF_ANNOT_LE_NONE 0
    PDF_ANNOT_LE_SQUARE 1
    PDF_ANNOT_LE_CIRCLE 2
    PDF_ANNOT_LE_DIAMOND 3
    PDF_ANNOT_LE_OPEN_ARROW 4
    PDF_ANNOT_LE_CLOSED_ARROW 5
    PDF_ANNOT_LE_BUTT 6
    PDF_ANNOT_LE_R_OPEN_ARROW 7
    PDF_ANNOT_LE_R_CLOSED_ARROW 8
    PDF_ANNOT_LE_SLASH 9


Widget Constants
-----------------

.. _WidgetTypes:

小部件类型 (*field_type*)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Widget Types (*field_type*)

::

    PDF_WIDGET_TYPE_UNKNOWN 0
    PDF_WIDGET_TYPE_BUTTON 1
    PDF_WIDGET_TYPE_CHECKBOX 2
    PDF_WIDGET_TYPE_COMBOBOX 3
    PDF_WIDGET_TYPE_LISTBOX 4
    PDF_WIDGET_TYPE_RADIOBUTTON 5
    PDF_WIDGET_TYPE_SIGNATURE 6
    PDF_WIDGET_TYPE_TEXT 7

文本小部件子类型 (*text_format*)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Text Widget Subtypes (*text_format*)

::

    PDF_WIDGET_TX_FORMAT_NONE 0
    PDF_WIDGET_TX_FORMAT_NUMBER 1
    PDF_WIDGET_TX_FORMAT_SPECIAL 2
    PDF_WIDGET_TX_FORMAT_DATE 3
    PDF_WIDGET_TX_FORMAT_TIME 4


小部件标志 (*field_flags*)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Widget flags (*field_flags*)

**Common to all field types**::

    PDF_FIELD_IS_READ_ONLY 1
    PDF_FIELD_IS_REQUIRED 1 << 1
    PDF_FIELD_IS_NO_EXPORT 1 << 2

**Text widgets**::

    PDF_TX_FIELD_IS_MULTILINE  1 << 12
    PDF_TX_FIELD_IS_PASSWORD  1 << 13
    PDF_TX_FIELD_IS_FILE_SELECT  1 << 20
    PDF_TX_FIELD_IS_DO_NOT_SPELL_CHECK  1 << 22
    PDF_TX_FIELD_IS_DO_NOT_SCROLL  1 << 23
    PDF_TX_FIELD_IS_COMB  1 << 24
    PDF_TX_FIELD_IS_RICH_TEXT  1 << 25

**Button widgets**::

    PDF_BTN_FIELD_IS_NO_TOGGLE_TO_OFF  1 << 14
    PDF_BTN_FIELD_IS_RADIO  1 << 15
    PDF_BTN_FIELD_IS_PUSHBUTTON  1 << 16
    PDF_BTN_FIELD_IS_RADIOS_IN_UNISON  1 << 25

**Choice widgets**::

    PDF_CH_FIELD_IS_COMBO  1 << 17
    PDF_CH_FIELD_IS_EDIT  1 << 18
    PDF_CH_FIELD_IS_SORT  1 << 19
    PDF_CH_FIELD_IS_MULTI_SELECT  1 << 21
    PDF_CH_FIELD_IS_DO_NOT_SPELL_CHECK  1 << 22
    PDF_CH_FIELD_IS_COMMIT_ON_SEL_CHANGE  1 << 26


.. _BlendModes:

PDF 标准混合模式
----------------------------

PDF Standard Blend Modes

.. tab:: 中文

    有关解释，请参阅： :ref:`AdobeManual`，第 324 页

.. tab:: 英文

    For an explanation see :ref:`AdobeManual`, page 324

::

    PDF_BM_Color "Color"
    PDF_BM_ColorBurn "ColorBurn"
    PDF_BM_ColorDodge "ColorDodge"
    PDF_BM_Darken "Darken"
    PDF_BM_Difference "Difference"
    PDF_BM_Exclusion "Exclusion"
    PDF_BM_HardLight "HardLight"
    PDF_BM_Hue "Hue"
    PDF_BM_Lighten "Lighten"
    PDF_BM_Luminosity "Luminosity"
    PDF_BM_Multiply "Multiply"
    PDF_BM_Normal "Normal"
    PDF_BM_Overlay "Overlay"
    PDF_BM_Saturation "Saturation"
    PDF_BM_Screen "Screen"
    PDF_BM_SoftLight "Softlight"


.. _StampIcons:

图章注释图标
----------------------------

Stamp Annotation Icons

.. tab:: 中文

    MuPDF 为 **橡皮图章** 注释定义了以下图标

.. tab:: 英文

    MuPDF has defined the following icons for **rubber stamp** annotations

::

    STAMP_Approved 0
    STAMP_AsIs 1
    STAMP_Confidential 2
    STAMP_Departmental 3
    STAMP_Experimental 4
    STAMP_Expired 5
    STAMP_Final 6
    STAMP_ForComment 7
    STAMP_ForPublicRelease 8
    STAMP_NotApproved 9
    STAMP_NotForPublicRelease 10
    STAMP_Sold 11
    STAMP_TopSecret 12
    STAMP_Draft 13

.. include:: footer.rst
