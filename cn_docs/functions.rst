.. include:: header.rst

============
函数
============

Functions

.. tab:: 中文

   以下是一些技术细节相对较低的杂项函数和属性。

   一些函数提供对 PDF 结构的详细访问。其他函数是其他函数的精简版高性能版本, 可提供更多信息。

   还有一些是方便的通用实用程序。

   ==================================== ==============================================================
   **函数**                              **简要描述**
   ==================================== ==============================================================
   :attr:`Annot.apn_bbox`               仅限 PDF：外观对象的 bbox
   :attr:`Annot.apn_matrix`             仅限 PDF：外观对象的矩阵
   :attr:`Page.is_wrapped`              检查内容是否存在换行
   :meth:`adobe_glyph_names`            Adobe 字形列表中定义的字形名称列表
   :meth:`adobe_glyph_unicodes`         Adobe 字形列表中定义的 Unicode 列表
   :meth:`Annot.clean_contents`         仅限 PDF：清理批注的 :data:`contents` 对象
   :meth:`Annot.set_apn_bbox`           仅限 PDF：设置外观对象的 bbox
   :meth:`Annot.set_apn_matrix`         仅限 PDF：设置外观对象的矩阵
   :meth:`ConversionHeader`             返回 *get_text* 方法的头部字符串
   :meth:`ConversionTrailer`            返回 *get_text* 方法的尾部字符串
   :meth:`Document.del_xml_metadata`    仅限 PDF：删除 XML 元数据
   :meth:`Document.get_char_widths`     仅限 PDF：返回字体的字形宽度列表
   :meth:`Document.get_new_xref`        仅限 PDF：创建并返回新的 :data:`xref` 条目
   :meth:`Document.is_stream`           仅限 PDF：检查 :data:`xref` 是否为流对象
   :meth:`Document.xml_metadata_xref`   仅限 PDF：返回 XML 元数据的 :data:`xref` 号
   :meth:`Document.xref_length`         仅限 PDF：返回 :data:`xref` 表的长度
   :meth:`EMPTY_IRECT`                  返回(标准的)空 / 无效矩形
   :meth:`EMPTY_QUAD`                   返回(标准的)空 / 无效四边形
   :meth:`EMPTY_RECT`                   返回(标准的)空 / 无效矩形
   :meth:`get_pdf_now`                  以 PDF 格式返回当前时间戳
   :meth:`get_pdf_str`                  返回 PDF 兼容字符串
   :meth:`get_text_length`              返回给定字体和 :data:`fontsize` 的字符串长度
   :meth:`glyph_name_to_unicode`        从字形名称返回 Unicode
   :meth:`image_profile`                返回包含基本图像属性的字典
   :meth:`INFINITE_IRECT`               返回(唯一存在的)无限矩形
   :meth:`INFINITE_QUAD`                返回(唯一存在的)无限四边形
   :meth:`INFINITE_RECT`                返回(唯一存在的)无限矩形
   :meth:`make_table`                   将矩形拆分为子矩形
   :meth:`Page.clean_contents`          仅限 PDF：清理页面的 :data:`contents` 对象
   :meth:`Page.get_bboxlog`             获取包围文本、绘图或图像对象的矩形列表
   :meth:`Page.get_contents`            仅限 PDF：返回内容的 :data:`xref` 号列表
   :meth:`Page.get_displaylist`         创建页面的显示列表
   :meth:`Page.get_text_blocks`         以 Python 列表的形式提取文本块
   :meth:`Page.get_text_words`          以 Python 列表的形式提取文本单词
   :meth:`Page.get_texttrace`           提供底层文本信息
   :meth:`Page.read_contents`           仅限 PDF：获取完整的、拼接后的 /Contents 源数据
   :meth:`Page.run`                     通过设备运行页面
   :meth:`Page.set_contents`            仅限 PDF：将页面的 :data:`contents` 设置为某个 :data:`xref`
   :meth:`Page.wrap_contents`           使用堆叠命令包裹内容
   :meth:`css_for_pymupdf_font`         为 pymupdf_fonts 包中的字体创建 CSS 源代码
   :meth:`paper_rect`                   返回已知纸张格式的矩形
   :meth:`paper_size`                   返回已知纸张格式的宽度和高度
   :meth:`paper_sizes`                  预定义纸张格式的字典
   :meth:`planish_line`                 计算映射到 x 轴的矩阵
   :meth:`recover_char_quad`            计算字符的四边形 ("rawdict")
   :meth:`recover_line_quad`            计算行跨度子集的四边形
   :meth:`recover_quad`                 计算跨度的四边形 ("dict", "rawdict")
   :meth:`recover_span_quad`            计算跨度字符子集的四边形
   :meth:`set_messages`                 设置 |PyMuPDF| 消息的输出位置
   :meth:`sRGB_to_pdf`                  从 sRGB 整数返回 PDF RGB 颜色元组
   :meth:`sRGB_to_rgb`                  从 sRGB 整数返回 (R, G, B) 颜色元组
   :meth:`unicode_to_glyph_name`        从 Unicode 返回字形名称
   :meth:`get_tessdata`                 定位 Tesseract-OCR 安装的语言支持数据
   :meth:`colors_pdf_dict`              返回颜色名称字典.
   :meth:`colors_wx_list`               返回颜色名称字典.
   :attr:`fitz_fontdescriptors`         可用的补充字体字典
   :attr:`PYMUPDF_MESSAGE`              |PyMuPDF| 消息的输出位置
   :attr:`pdfcolor`                     近 500 种 PDF 格式的 RGB 颜色字典
   ==================================== ==============================================================

   .. method:: paper_size(s)

      便捷函数, 用于返回已知纸张格式代码的宽度和高度。这些值以像素为单位, 基于标准分辨率 72 像素 = 1 英寸。

      目前定义的格式包括 **'A0'** 到 **'A10'**, **'B0'** 到 **'B10'**, **'C0'** 到 **'C10'**, **'Card-4x6'**, **'Card-5x7'**, **'Commercial'**, **'Executive'**, **'Invoice'**, **'Ledger'**, **'Legal'**, **'Legal-13'**, **'Letter'**, **'Monarch'** 和 **'Tabloid-Extra'**, 每种格式均可选择纵向(portrait)或横向(landscape)。

      需要以字符串形式提供格式名称(不区分大小写), 并可选择性地添加后缀 "-L"(横向)或 "-P"(纵向)。如果没有后缀, 默认采用纵向。

      :arg str s: 以上任意格式名称, 可使用大写或小写, 例如 *"A4"* 或 *"letter-l"*。

      :rtype: tuple
      :returns: 纸张格式的 *(宽度, 高度)*。如果格式未知, 则返回 *(-1, -1)*。示例：*pymupdf.paper_size("A4")* 返回 *(595, 842)*, *pymupdf.paper_size("letter-l")* 返回 *(792, 612)*。


   -----

   .. method:: paper_rect(s)

      便捷函数, 用于返回已知纸张格式的 :ref:`Rect`。

      :arg str s: 任何受 :meth:`paper_size` 支持的格式名称。

      :rtype: :ref:`Rect`
      :returns: *pymupdf.Rect(0, 0, width, height)*, 其中 *width, height=pymupdf.paper_size(s)*。

      >>> import pymupdf
      >>> pymupdf.paper_rect("letter-l")
      pymupdf.Rect(0.0, 0.0, 792.0, 612.0)
      >>>

   -----

   .. method:: set_messages(*, text=None, fd=None, stream=None, path=None, path_append=None, pylogging=None, pylogging_logger=None, pylogging_level=None, pylogging_name=None, )

      设置 |PyMuPDF| 消息的输出位置, 可以是文件描述符、文件、已有的流或 `Python 的日志系统 <https://docs.python.org/3/library/logging.html>`_。

      通常情况下, 只需设置一个参数, 或者一个或多个 `pylogging*` 参数。

      :arg str text:
            目标位置的文本描述；详细信息请参阅环境变量 `PYMUPDF_MESSAGE` 的说明。
      :arg int fd:
            写入文件描述符。
      :arg stream:
            写入已有的流, 该流必须具有 `.write(text)` 和 `.flush()` 方法。
      :arg str path:
            写入文件。
      :arg str path_append:
            追加写入文件。
      :arg pylogging:
            写入 Python 的 `logging` 系统。
      :arg logging.Logger pylogging_logger:
            使用指定的 Logger 写入 Python 的 `logging` 系统。
      :arg int pylogging_level:
            使用指定的日志级别写入 Python 的 `logging` 系统。
      :arg str pylogging_name:
            使用指定的日志记录器名称写入 Python 的 `logging` 系统。
            仅在 `pylogging_logger` 为 None 时使用, 默认值为 `pymupdf`。

      如果任何 `pylogging*` 参数不为 None, 则消息将写入 `Python 的日志系统 <https://docs.python.org/3/library/logging.html>`_。

   -----

   .. method:: sRGB_to_pdf(srgb)

      *自 v1.17.4 版本起新增*

      便捷函数, 将给定的 sRGB 颜色整数转换为 PDF 颜色三元组(red, green, blue)。该格式用于 :meth:`Page.get_text` 返回的 "dict" 和 "rawdict" 结构中。

      :arg int srgb: 形如 RRGGBB 的整数, 每个颜色分量的取值范围为 0-255。

      :returns: 颜色三元组 (red, green, blue), 其中每个值都是 *0 <= item <= 1* 之间的浮点数, 表示相同颜色。例如  `sRGB_to_pdf(0xff0000) = (1, 0, 0)` (红色)。


   -----

   .. method:: sRGB_to_rgb(srgb)

      *New in v1.17.4*

      Convenience function returning a color (red, green, blue) for a given *sRGB* color integer.

      :arg int srgb: an integer of format RRGGBB, where each color component is an integer in range(255).

      :returns: a tuple (red, green, blue) with integer items in `range(256)` representing the same color. Example `sRGB_to_pdf(0xff0000) = (255, 0, 0)` (red).

   -----

   .. method:: glyph_name_to_unicode(name)

      *自 v1.18.0 版本起新增*

      根据 **Adobe Glyph List** 返回字形名称对应的 Unicode 编码。

      :arg str name: 字形名称。本函数基于 `Adobe Glyph List <https://github.com/adobe-type-tools/agl-aglfn/blob/master/glyphlist.txt>`_。

      :rtype: int
      :returns: 对应的 Unicode 编码。如果 *name* 无效，则返回 `0xfffd (65533)`。

      .. note:: 类似功能也由 `fontTools <https://pypi.org/project/fonttools/>`_ 包的 *agl* 子包提供。

   -----

   .. method:: unicode_to_glyph_name(ch)

      *自 v1.18.0 版本起新增*

      根据 **Adobe Glyph List** 返回 Unicode 编码对应的字形名称。

      :arg int ch: Unicode 编码，例如 `ord("ß")`。本函数基于 `Adobe Glyph List <https://github.com/adobe-type-tools/agl-aglfn/blob/master/glyphlist.txt>`_。

      :rtype: str
      :returns: 对应的字形名称。例如 `pymupdf.unicode_to_glyph_name(ord("Ä"))` 返回 `'Adieresis'`。

      .. note:: 类似功能也由 `fontTools <https://pypi.org/project/fonttools/>`_ 包的 *agl* 子包提供。

   -----

   .. method:: adobe_glyph_names()

      *自 v1.18.0 版本起新增*

      返回 **Adobe Glyph List** 中定义的字形名称列表。

      :rtype: list
      :returns: 字符串列表。

      .. note:: 类似功能也由 `fontTools <https://pypi.org/project/fonttools/>`_ 包的 *agl* 子包提供。

   -----

   .. method:: adobe_glyph_unicodes()

      *自 v1.18.0 版本起新增*

      返回 **Adobe Glyph List** 中存在字形名称的 Unicode 编码列表。

      :rtype: list
      :returns: 整数列表。

      .. note:: 类似功能也由 `fontTools <https://pypi.org/project/fonttools/>`_ 包的 *agl* 子包提供。

   -----

   .. method:: css_for_pymupdf_font(fontcode, *, CSS=None, archive=None, name=None)

      *自 v1.21.0 版本起新增*

      **用于 "Story" 应用的实用函数。**

      为 `pymupdf-fonts` 中的指定字体代码生成 CSS `@font-face` 规则，并创建一个包含所有匹配字体的 `font-family` 定义。

      在 `pymupdf-fonts` 包中，字体的命名规则为 `"fontcode<sf>"` ，其中后缀 `"sf"` 可以是 `""` (空，表示常规体)、 `"it"/"i"` (斜体)、 `"bo"/"b"` (加粗)或 `"bi"` (加粗斜体)。这些后缀代表该字体系列的不同变体。

      例如，字体代码 `"notos"` 代表以下字体：

      * `"notos"` - "Noto Sans Regular"
      * `"notosit"` - "Noto Sans Italic"
      * `"notosbo"` - "Noto Sans Bold"
      * `"notosbi"` - "Noto Sans Bold Italic"

      该函数会生成最多四个 `@font-face` 规则，并将它们统一分配给 `font-family` 名称 `"notos"` (或者使用 `name` 参数指定的名称)。相关字体数据将存储或添加到提供的 `archive` 归档对象中。

      在 :ref:`Story` 的 Python API 中，可以执行 `.set_font(fontcode)` (或提供的 `name`)，以确保根据需要自动选择正确的字重或样式。

      例如，要用 `"notos"` 字体替换 HTML 标准的 `"sans-serif"` (即 Helvetica)，可以执行以下代码。这样，每当使用 `"sans-serif"` (无论是显式还是隐式)时，都会选择 Noto Sans 字体：

      `CSS = pymupdf.css_for_pymupdf_font("notos", name="sans-serif", archive=...)`

      该函数接受并返回 CSS 源码，会在提供的 CSS 源码末尾追加新的 `@font-face` 规则。

      :arg str fontcode: `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_ 包中的字体代码，通常代表该字体系列的常规体。
      :arg str CSS: 现有的 CSS 源码，或 `None`。函数会将新的 `@font-face` 规则追加到此字符串中。 **此字符串必须作为** `user_css` **传递给** :ref:`Story`。
      :arg archive: :ref:`Archive`， **必填** 。所有找到的字体二进制数据(最多四个变体)将被添加到此归档对象。 **此对象必须作为** `archive` **传递给** :ref:`Story`。
      :arg str name: 字体的 `font-family` 名称。如果未提供，则默认使用 `fontcode`。

      :rtype: str
      :returns: 修改后的 CSS 代码，其中包含 `fontcode` 对应的 `@font-face` 规则。同时，相关字体数据会被添加到 `archive` 中。该函数会自动查找最多四种字体变体。`pymupdf-fonts` 中的所有常规字体(非数学或音乐特殊用途字体)均有常规、加粗、斜体和加粗斜体四个变体。可以执行 `pymupdf.fitz_fontdescriptors.keys()` 以查看当前可用的字体代码，例如::

         dict_keys(['cascadia', 'cascadiai', 'cascadiab', 'cascadiabi', 'figbo', 'figo', 'figbi', 'figit',
               'fimbo', 'fimo', 'spacembo', 'spacembi', 'spacemit', 'spacemo', 'math', 'music',
               'symbol1', 'symbol2', 'notosbo', 'notosbi', 'notosit', 'notos', 'ubuntu', 'ubuntubo',
               'ubuntubi', 'ubuntuit', 'ubuntm', 'ubuntmbo', 'ubuntmbi', 'ubuntmit'])

      以下是完整示例，使用 `"Noto Sans"` 代替 `"Helvetica"`::

         arch = pymupdf.Archive()
         CSS = pymupdf.css_for_pymupdf_font("notos", name="sans-serif", archive=arch)
         story = pymupdf.Story(user_css=CSS, archive=arch)


   -----

   .. _Functions_make_table:

   .. method:: make_table(rect, cols=1, rows=1)

      *自 v1.17.4 版本起新增*

      该方法用于将一个矩形区域均匀划分为多个子矩形，并返回一个列表，其中每个子列表代表一行，包含 `cols` 个 :ref:`Rect` 对象。每个子矩形可以通过其行索引和列索引进行访问。

      :arg rect_like rect: 需要拆分的矩形区域。
      :arg int cols: 需要划分的列数。
      :arg int rows: 需要划分的行数。
      :returns: 一个包含 :ref:`Rect` 对象的列表，这些矩形大小相等，并且它们的并集等于 `rect`。以下是使用 `cell = pymupdf.make_table(rect, cols=4, rows=3)` 创建的 3x4 表格的示例布局：

      .. image:: images/img-make-table.*
         :scale: 60

   -----

   .. method:: planish_line(p1, p2)

      *自 v1.16.2 版本起新增*

      计算一个矩阵，该矩阵可将从 `p1` 到 `p2` 的直线映射到 x 轴，使得 `p1` 变为 (0,0)，而 `p2` 变为一个与 (0,0) 具有相同距离的点。

      :arg point_like p1: 线段的起点。
      :arg point_like p2: 线段的终点。

      :rtype: :ref:`Matrix`
      :returns: 结合旋转和平移的变换矩阵。例如:

         >>> p1 = pymupdf.Point(1, 1)
         >>> p2 = pymupdf.Point(4, 5)
         >>> abs(p2 - p1)  # 计算点之间的距离
         5.0
         >>> m = pymupdf.planish_line(p1, p2)
         >>> p1 * m
         Point(0.0, 0.0)
         >>> p2 * m
         Point(5.0, -5.960464477539063e-08)
         >>> # 计算变换后的点之间的距离
         >>> abs(p2 * m - p1 * m)
         5.0

      .. image:: images/img-planish.png
         :scale: 40

   -----

   .. method:: paper_sizes

      一个包含预定义纸张格式的字典，作为 :meth:`paper_size` 的基础数据。

   -----

   .. attribute:: fitz_fontdescriptors

      *自 v1.17.5 版本起新增*

      一个字典，包含 `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_ 仓库中的可用字体。键是字体的预留名称，字典的值提供如下信息::

         In [2]: pymupdf.fitz_fontdescriptors.keys()
         Out[2]: dict_keys(['figbo', 'figo', 'figbi', 'figit', 'fimbo', 'fimo',
         'spacembo', 'spacembi', 'spacemit', 'spacemo', 'math', 'music', 'symbol1',
         'symbol2'])
         In [3]: pymupdf.fitz_fontdescriptors["fimo"]
         Out[3]:
         {'name': 'Fira Mono Regular',
         'size': 125712,
         'mono': True,
         'bold': False,
         'italic': False,
         'serif': True,
         'glyphs': 1485}

      如果 `pymupdf-fonts` 未安装，该字典将为空。

      这些字典键可用于定义 :ref:`Font`，例如::

         font = pymupdf.Font("fimo")

      这种方式与内置字体（如 `"Helvetica"`）的使用方式相同。

   -----

   .. attribute:: PYMUPDF_MESSAGE

      如果在 |PyMuPDF| 导入时 `os.environ` 中包含该变量，则该变量用于设置 |PyMuPDF| 消息的目标位置。否则，消息将发送到 `sys.stdout`。

      * 若值以 `fd:` 开头，则剩余部分应为一个整数文件描述符，消息将被写入该描述符。

         * 例如，`PYMUPDF_MESSAGE=fd:2` 将消息发送到 `stderr`。

      * 若值以 `path:` 开头，则剩余部分应为一个文件路径，消息将被写入该文件。如果文件已存在，则会被清空。

      * 若值以 `path+:` 开头，则剩余部分应为一个文件路径，消息将被追加写入该文件（若已存在）。

      * 若值以 `logging:` 开头，则消息将被写入 `Python 日志系统 <https://docs.python.org/3/library/logging.html>`_。剩余部分可以包含逗号分隔的 `name=value` 项：

         * `level=<int>` 设置日志级别。
         * `name=<str>` 设置日志器名称（默认为 `pymupdf`）。

         其他项将被忽略。

      * 其他前缀将导致错误。

      另见 `set_messages()`。

   -----

   .. attribute:: pdfcolor

      *自 v1.19.6 版本起新增*

      包含约 500 种 PDF 颜色格式的 RGB 颜色，颜色名称作为键。可以使用 `pymupdf.pdfcolor.keys()` 查看所有可用颜色。

      示例：

         * `pymupdf.pdfcolor["red"] = (1.0, 0.0, 0.0)`
         * `pymupdf.pdfcolor["skyblue"] = (0.5294117647058824, 0.807843137254902, 0.9215686274509803)`
         * `pymupdf.pdfcolor["wheat"] = (0.9607843137254902, 0.8705882352941177, 0.7019607843137254)`

   -----

   .. method:: get_pdf_now()

      该方法用于返回当前本地时间戳，并以 PDF 兼容格式表示，例如 *D:20170501121525-04'00'* 对应于 2017 年 5 月 1 日 12:15:25（本地时区比 UTC 偏西 4 小时）。

      :rtype: str
      :returns: 当前本地 PDF 时间戳。

   -----

   .. method:: get_text_length(text, fontname="helv", fontsize=11, encoding=TEXT_ENCODING_LATIN)

      *自 v1.14.7 版本起新增*

      计算给定 **内置** 字体、:data:`fontsize` 和编码情况下，文本的输出长度。

      :arg str text: 需要计算长度的文本字符串。
      :arg str fontname: 字体名称，必须是 :ref:`Base-14-Fonts` 或 CJK 字体之一，可通过其“预留”字体名称标识（参见 :meth:`Page.insert_font`）。
      :arg float fontsize: :data:`fontsize`，即字体大小。
      :arg int encoding: 要使用的编码。除了 0 = Latin，还支持 1 = Greek 和 2 = Cyrillic（俄语）。仅适用于 Base-14 字体（如 "Helvetica"、"Courier" 和 "Times" 及其变体）。请确保使用与插入文本时相同的编码值。

      :rtype: float
      :returns: 计算出的文本长度（单位：点），例如用于 :meth:`Page.insert_text`。

      .. note:: 该函数仅计算长度，不会插入字体或文本。

      .. note:: :ref:`Font` 类提供了类似的方法 :meth:`Font.text_length`，支持 Base-14 字体以及任何具有字符映射（CMap，Type 0 字体）的字体。

      .. warning:: 
         
         若使用该方法来计算 (:ref:`Page` 或 :ref:`Shape`) `insert_textbox` 方法所需的矩形宽度，需注意它们是按 **字符级** 计算的。由于四舍五入的影响，以下计算通常会产生稍大的数值：
      
         *sum([pymupdf.get_text_length(c) for c in text]) > pymupdf.get_text_length(text)*

         解决方案：
         (1) 采用相同的逐字符计算方式；或
         (2) 使用 `pymupdf.get_text_length(text + "'")` 进行计算，以确保宽度足够。

   -----

   .. method:: get_pdf_str(text)

      生成 PDF 兼容的字符串：如果文本包含 Unicode 代码点 *ord(c) > 255*，则转换为 UTF-16BE 编码，并作为十六进制字符字符串用 "<>" 括起来，如 *<feff...>*。否则，返回字符串，并用圆括号 *()* 括起来，同时将 ASCII 范围外的字符替换为特殊代码。此外，所有 `"("`、`")"` 和反斜杠 `"\"` 都会被转义为 `"\\"`。

      :arg str text: 需要转换的对象。

      :rtype: str
      :returns: 以 *()* 或 *<>* 括起的 PDF 兼容字符串。

   -----

   .. method:: image_profile(stream)

      * 自 v1.16.7 版本起新增
      * v1.19.5 版本变更：如果 EXIF 数据中存在自然图像方向信息，则返回该信息。
      * v1.22.5 版本变更：错误情况下始终返回 `None`，而不是空字典。

      获取提供的图像的关键属性，避免使用其他 Python 库来确定这些信息。

      :arg bytes|bytearray|BytesIO|file stream: 传入的图像可以是内存中的数据或 **已打开的** 文件。内存中的图像可以是 `bytes`、 `bytearray` 或 `io.BytesIO` 格式。

      :rtype: dict
      :returns:
         方法不会引发异常，若出错，则返回 `None`。否则，返回包含以下内容的字典::

            In [2]: pymupdf.image_profile(open("nur-ruhig.jpg", "rb").read())
            Out[2]:
            {'width': 439,
            'height': 501,
            'orientation': 0,  # 自然方向（来自 EXIF）
            'transform': (1.0, 0.0, 0.0, 1.0, 0.0, 0.0),  # 方向变换矩阵
            'xres': 96,
            'yres': 96,
            'colorspace': 3,
            'bpc': 8,
            'ext': 'jpeg',
            'cs-name': 'DeviceRGB'}

         其中， `orientation` 及相应的 `transform` 变换矩阵与 **Exif** 信息的对应关系如下（引自 MuPDF 文档， *ccw* 表示逆时针旋转）：

            1. 未定义
            2. 0° 逆时针旋转（Exif = 1）
            3. 90° 逆时针旋转（Exif = 8）
            4. 180° 逆时针旋转（Exif = 3）
            5. 270° 逆时针旋转（Exif = 6）
            6. X 轴翻转（Exif = 2）
            7. X 轴翻转后再 90° 逆时针旋转（Exif = 5）
            8. X 轴翻转后再 180° 逆时针旋转（Exif = 4）
            9. X 轴翻转后再 270° 逆时针旋转（Exif = 7）

         .. note::

            * 对于某些“特殊”图像（如 FAX 编码、RAW 格式等），该方法可能无法使用。但仍可在 PyMuPDF 中处理这些图像，例如使用 :meth:`Document.extract_image` 方法或通过 `Pixmap(doc, xref)` 创建像素图。这些方法会自动将特殊图像转换为 PNG 格式后返回结果。
            * 可通过 :data:`xref` 获取嵌入在 PDF 文档中的图像属性。在这种情况下，请确保提取原始数据流：`pymupdf.image_profile(doc.xref_stream_raw(xref))`。
            * 通过 :meth:`Page.get_text` 使用 `"dict"` 或 `"rawdict"` 选项返回的图像数据块也受支持。

   -----

   .. method:: ConversionHeader("text", filename="UNKNOWN")

      返回用于构造有效文档的页文本输出所需的头部字符串。

      :arg str output: 文档类型。应与 *get_text()* 方法的输出参数一致。
      :arg str filename: （可选）在 "json" 和 "xml" 输出类型下使用的任意文件名。

      :rtype: str

   -----

   .. method:: ConversionTrailer(output)

      返回用于构造有效文档的页文本输出所需的尾部字符串。示例请参见 :meth:`Page.get_text`。

      :arg str output: 文档类型。应与 *get_text()* 方法的输出参数一致。

      :rtype: str

   -----

   .. method:: Document.del_xml_metadata()

      删除 PDF 中包含 XML 元数据的对象。(Py-) MuPDF 不支持 XML 格式的元数据。如果希望仅使用传统的元数据字典，请使用此方法。许多第三方 PDF 程序会插入 XML 格式的元数据，这可能会覆盖传统字典中的存储内容。本方法会删除该 XML 参考，并在下次文件垃圾回收时移除相应的 PDF 对象。

   -----

   .. method:: Document.xml_metadata_xref()

      返回 PDF 文件级 XML 元数据的 :data:`xref` （如果存在）。参考 :meth:`Document.del_xml_metadata`。可以使用 :meth:`Document.xref_stream` 获取其内容，并使用 XML 处理工具进行操作。

      :rtype: int
      :returns: PDF 文件级 XML 元数据的 :data:`xref`，若无则返回 0。

   -----

   .. method:: Page.run(dev, transform)

      通过设备处理页面。

      :arg dev: 设备对象，由 :ref:`Device` 构造方法之一获取。
      :type dev: :ref:`Device`

      :arg transform: 应用于页面的变换。若不需要变换，则设为 :ref:`Identity`。
      :type transform: :ref:`Matrix`

   -----

   .. method:: Page.get_bboxlog(layers=False)

      * 自 v1.19.0 版本起新增
      * v1.22.0 版本变更：可选地返回适用于边界框的 OCG（可选内容组）名称。

      :returns: 一个矩形列表，表示文本、图像或绘图对象的包围盒。列表中的每个项为一个元组 `(type, (x0, y0, x1, y1))`，其中第二个元组为矩形坐标，`type` 取以下值之一。如果 `layers=True`，则元组中会包含第三项，即 OCG 名称或 `None` ： `(type, (x0, y0, x1, y1), None)`。

         * `"fill-text"` —— 普通文本（绘制时不带字符边框）
         * `"stroke-text"` —— 仅显示字符边框的文本
         * `"ignore-text"` —— 不应显示的文本（如 OCR 文本层）
         * `"fill-path"` —— 仅填充颜色的绘图（无边框）
         * `"stroke-path"` —— 仅绘制边框的绘图（无填充色）
         * `"fill-image"` —— 显示图像
         * `"fill-shade"` —— 显示渐变填充

         元素的顺序表示 **页面渲染这些命令的顺序**。因此，如果某个元素的包围盒与前一个元素相交或包含前一个元素，则前一个元素可能会被部分或完全覆盖。

         该列表可用于检测此类情况。元素在此列表中的索引等于 :meth:`Page.get_drawings` 和 :meth:`Page.get_texttrace` 返回的字典中的 `"seqno"` 值。

   -----

   .. method:: Page.get_texttrace()

      * 自 v1.18.16 版本起新增
      * v1.19.0 版本变更：新增键 `"seqno"`。
      * v1.19.1 版本变更：描边和填充颜色现在始终为 RGB 或 GRAY。
      * v1.19.3 版本变更：当 `dir != (1, 0)` 时，跨度和字符的 bbox 也能正确计算。
      * v1.22.0 版本变更：新增字典键 `"layer"`。

      返回页面的底层文本信息。本方法适用于 **所有** 文档类型。结果是一个包含以下内容的 Python 字典列表::

         {
            'ascender': 0.83251953125,          # 字体上升部 (1)
            'bbox': (458.14019775390625,        # 跨度 bbox x0 (7)
                     749.4671630859375,         # 跨度 bbox y0
                     467.76458740234375,        # 跨度 bbox x1
                     757.5071411132812),        # 跨度 bbox y1
            'bidi': 0,                          # 双向级别 (1)
            'chars': (                          # 字符信息，tuple[tuple]
                        (45,                    # Unicode 编码 (4)
                        16,                     # 字形 ID（字体相关）
                        (458.14019775390625,    # 原点 x 坐标 (1)
                        755.3758544921875),     # 原点 y 坐标 (1)
                        (458.14019775390625,    # 字符 bbox x0 (6)
                        749.4671630859375,      # 字符 bbox y0
                        462.9649963378906,      # 字符 bbox x1
                        757.5071411132812)),    # 字符 bbox y1
                        ( ... ),                # 更多字符
                     ),
            'color': (0.0,),                    # 文本颜色，tuple[float] (1)
            'colorspace': 1,                    # 颜色空间分量数 (1)
            'descender': -0.30029296875,        # 字体下降部 (1)
            'dir': (1.0, 0.0),                  # 书写方向 (1)
            'flags': 12,                        # 字体标志 (1)
            'font': 'CourierNewPSMT',           # 字体名称 (1)
            'linewidth': 0.4019999980926514,    # 当前线宽值 (3)
            'opacity': 1.0,                     # 文本透明度 (5)
            'layer': None,                      # 可选内容组（OCG）名称 (9)
            'seqno': 246,                       # 序列号 (8)
            'size': 8.039999961853027,          # 字体大小 (1)
            'spacewidth': 4.824785133358091,    # 空格字符宽度
            'type': 0,                          # 跨度类型 (2)
            'wmode': 0                          # 书写模式 (1)
         }

      详细信息：

      1. 标记为 "(1)" 的信息与 :ref:`TextPage` 解释的含义和数值相同。

         - 请注意，字体 `"flags"` 值不会包含 *上标* 标志位：上标检测是在 MuPDF :ref:`TextPage` 代码中执行的，而不是字体的固有属性。
         - 还要注意，文本 *颜色* 以 `0 <= f <= 1` 的浮点数元组编码，而不是 sRGB 格式。根据 `span["type"]`，解释为填充颜色或描边颜色。

      2. 文本跨度类型共有 3 种：

         - 0：填充文本 —— 等同于 PDF 渲染模式 0 (`0 Tr`，PDF 默认)，仅显示字符内部。
         - 1：描边文本 —— 等同于 `1 Tr`，仅显示字符边框。
         - 3：隐藏文本 —— 等同于 `3 Tr` （隐藏文本）。

      3. 线宽对 `span["type"] != 0` 情况较为重要：它决定了字符边框线的厚度。如果文本数据未提供此值，则会生成 `5%` 字体大小的默认值 (`span["size"] * 0.05`)。PDF 中常用 `2 Tr` 方式创建 *伪加粗* 文本，此情况下没有对应的跨度类型，而是生成两个连续跨度，分别对应 `0` 和 `1`，需要自行处理。

      4. 字符 Unicode 提供 `chr()` 以获取字符本身。

      5. 透明度 (`0 <= opacity <= 1`) 影响文本是否可见，0 表示完全透明，1 表示完全不透明。根据 `span["type"]`，可解释为填充或描边透明度。

      6. **（v1.19.0 变更）** `bbox` 值等同或接近 `"rawdict"` 中的 `char["bbox"]`，高度计算始终假设启用了 *小字形高度*。

      7. **（v1.19.0 新增）** `bbox` 是所有字符 `bbox` 的并集。

      8. **（v1.19.0 新增）** `seqno` 枚举页面外观的渲染顺序。较大序列号的对象可能会遮挡较小序列号的文本。

      9. **（v1.22.0 新增）** `"layer"` 为可选内容组 (OCG) 名称，或 `None`。

      **与** `page.get_text("rawdict")` **的比较：**

      - 本方法 **提取速度提高约 2 倍** （视文本量而定）。
      - 结果数据 **更小**，但提供更多信息。
      - 可 **检测文本不可见性** （透明度为 0，类型 > 1，或被更高序列号对象遮挡）。
      - MuPDF 若返回 `0xFFFD (65533)` 作为未识别字符，可根据字形 ID 推测信息。
      - `span["chars"]` **不包含空格**，除非文档创建者显式编码；但提供空格宽度，以便计算文本间距。
      - 文本不会像 :ref:`TextPage` 组织为块、行、跨度、字符，而是按顺序提取，遇到样式变化即开始新跨度。同一跨度可能包含不同 `origin.y` 值的字符（即处于不同行）。不保证跨度字符排序，需要自行处理，如使用 `span["dir"]`、`span["wmode"]` 等。

      **连字（Ligature）处理：**
      - MuPDF 处理 `"fi"`, `"ff"`, `"fl"`, `"ft"`, `"st"`, `"ffi"`, `"ffl"` 这些连字（常见前三种）。
      - 例如："fi" 可能存储为两个字符::
      
         (102, glyph, (x, y), (x0, y0, x1, y1))  # 'f' 的 bbox
         (105, -1, (x, y), (x0, y0, x0, y1))     # 'i'，bbox 宽度为 0
      
      - 可替换为单个 Unicode 代码：
      
        + `"ff"` -> `0xFB00`
        + `"fi"` -> `0xFB01`
        + `"fl"` -> `0xFB02`
        + `"ffi"` -> `0xFB03`
        + `"ffl"` -> `0xFB04`
        + `"ft"` -> `0xFB05`
        + `"st"` -> `0xFB06`
      
      - 例如，将上述示例转换为 `(0xFB01, glyph, (x, y), (x0, y0, x1, y1))`。

      **（v1.19.3 变更）** 跨度和字符 `bbox` 现在包围字符四边形，可用 :meth:`recover_quad`、:meth:`recover_char_quad`、:meth:`recover_span_quad` 还原。

      **（v1.21.1 变更）** `"layer"` 字段提供 OCG 名称（如适用）。

   -----

   .. method:: Page.wrap_contents()

      确保页面的所谓图形状态是平衡的，并且新内容能够正确插入。

      在 PyMuPDF v1.24.1+ 版本中，此方法得到了改进，并在需要时自动执行，因此您无需再关心此方法。

      我们不建议使用 :meth:`Page.clean_contents` 来实现此功能。

   -----

   .. attribute:: Page.is_wrapped

      指示页面的所谓图形状态是否平衡。如果为 `False`，则在插入新内容时应执行 :meth:`Page.wrap_contents` （仅在 `overlay=True` 模式下相关）。在较新的版本中（1.24.1+），此检查和相应的调整会自动执行，因此您无需再担心此问题。

      :rtype: bool

   -----

   .. method:: Page.get_text_blocks(flags=None)

      已废弃的 :meth:`TextPage.extractBLOCKS` 的包装器。请改用 :meth:`Page.get_text` 并使用 `"blocks"` 选项。

      :rtype: list[tuple]

   -----

   .. method:: Page.get_text_words(flags=None, delimiters=None)

      已废弃的 :meth:`TextPage.extractWORDS` 的包装器。请改用 :meth:`Page.get_text` 并使用 `"words"` 选项。

      :rtype: list[tuple]

   -----

   .. method:: Page.get_displaylist()

      将页面传递给列表设备并返回其显示列表。

      :rtype: :ref:`DisplayList`
      :returns: 页面的显示列表。

   -----

   .. method:: Page.get_contents()

      仅适用于 PDF：检索页面的 :data:`xref` 列表，表示页面的 :data:`contents` 对象。可能为空，或包含多个整数。如果页面已清理（使用 :meth:`Page.clean_contents` ），则该列表最多包含一个条目。每个 `/Contents` 对象的 "源" 可以通过 :meth:`Document.xref_stream` 使用此列表中的项单独读取。与此相对，方法 :meth:`Page.read_contents` 会遍历此列表并将相应的源连接成一个 `bytes` 对象。

      :rtype: list[int]

   -----

   .. method:: Page.set_contents(xref)

      仅适用于 PDF： 让页面的 `/Contents` 键指向此 xref。任何先前使用的内容对象将被忽略，并且可以通过垃圾回收删除。

   -----

   .. method:: Page.clean_contents(sanitize=True)

      * 在 v1.17.6 中更改

      仅适用于 PDF：清理并连接与此页面相关的所有 :data:`contents` 对象。"清理" 包括语法修正、标准化和 "美化" 内容流。如果 sanitize 为 true，内容对象中的资源与实际使用的资源之间的差异也将被修正。有关更多详细信息，请参阅 :meth:`Page.get_contents`。

      在 v1.16.0 版本中，注释不再由此方法隐式清理。请单独使用 :meth:`Annot.clean_contents`。

      :arg bool sanitize: *(在 v1.17.6 中新增)* 如果为真，则会同步资源与其在内容对象中的实际使用。例如，如果某个字体没有实际用于页面中的任何文本，则会从 `/Resources/Font` 对象中删除该字体。

      .. warning:: 
         
         这是一个复杂的功能，可能会生成大量的新数据，并使旧数据变得无效。 **不推荐** 与 **增量保存** 选项一起使用。还请注意，生成的单例新 */Contents* 对象是 **未压缩** 的。因此，您应该使用选项 *"deflate=True, garbage=3"* 将其保存到 **新文件** 中。

         不再使用此方法来确保 PDF 页面上正确的插入。自 PyMuPDF v1.24.2 版本起，这一功能已自动处理。

   -----

   .. method:: Page.read_contents()

      *在 v1.17.0 中新增。*
      返回与页面关联的所有 :data:`contents` 对象的连接体 -- 不进行清理或其他修改。每当您需要解析整个源而不关心有多少个独立的内容对象时，请使用此方法。

      :rtype: bytes

   -----

   .. method:: Annot.clean_contents(sanitize=True)

      清理与注释相关的 :data:`contents` 流。这与 :meth:`Page.clean_contents` 执行的操作相同，只是此操作仅限于该注释。

   -----

   .. method:: Document.get_char_widths(xref=0, limit=256)

      返回文档中某字体的字符字形及其宽度的列表。必须通过其 PDF 交叉引用号 :data:`xref` 来指定字体。此函数会在 :meth:`Page.insert_text` 和 :meth:`Page.insert_textbox` 中自动调用，因此您通常无需手动调用。

      :arg int xref: 嵌入 PDF 中的字体的交叉引用号。要查找字体的 :data:`xref`，可以使用例如 *doc.get_page_fonts(pno)* 获取页面号 *pno* 的字体，并取返回列表中的第一个条目。

      :arg int limit: 限制返回的条目数。默认值为 256，适用于仅支持 1 字节字符的字体，所谓的 "简单字体"（通过此方法检查）。所有 :ref:`Base-14-Fonts` 都是简单字体。

      :rtype: list
      :returns: 一个 *limit* 元组的列表。每个字符 *c* 在该列表中有一个条目 *(g, w)*，其中 *g* 是字符的字形 ID（整数）， *w* 是其标准化宽度。对于某些 :data:`fontsize`，实际宽度可以计算为 *w * fontsize*。对于简单字体，*g* 条目可以安全地忽略。在所有其他情况下，*g* 是图形表示 *c* 的基础。

      该函数计算一个字符串 *text* 的像素宽度，如下所示::

         def pixlen(text, widthlist, fontsize):
            try:
               return sum([widthlist[ord(c)] for c in text]) * fontsize
            except IndexError:
               raise ValueError:("max. code point found: %i, increase limit" % ord(max(text)))

   -----

   .. method:: Document.is_stream(xref)

      * 在 v1.14.14 中新增

      仅适用于 PDF：检查由 :data:`xref` 表示的对象是否为 :data:`stream` 类型。如果不是 PDF 或者该数字超出了有效的 xref 范围，则返回 *False*。

      :arg int xref: :data:`xref` 号。

      :returns: 如果对象定义后跟有以 *stream* 和 *endstream* 关键字对包裹的数据，则返回 *True*。

   -----

   .. method:: Document.get_new_xref()

      将 :data:`xref` 号增加一个条目并返回该号码。然后可以使用该号码插入一个新的对象。

      :rtype: int
      :returns: 新的 :data:`xref` 条目的号码。请注意，这仅在 PDF 的交叉引用表中创建一个新条目。此时，还不会与之关联任何 PDF 对象。要使用此号码创建（空的）对象，请使用 `doc.update_xref(xref, "<<>>")`。

   -----

   .. method:: Document.xref_length()

      返回 :data:`xref` 表的长度。

      :rtype: int
      :returns: :data:`xref` 表中的条目数。

   -----

   .. method:: recover_quad(line_dir, span)

      计算通过 :meth:`Page.get_text` 的 "dict" 或 "rawdict" 选项提取的文本跨度的四边形。

      :arg tuple line_dir: 所属行的 `line["dir"]`。对于从 :meth:`Page.get_texttrace` 提取的跨度，请使用 `None`。
      :arg dict span: 跨度。
      :returns: 跨度的 :ref:`Quad`，可用于文本标记注释（例如“高亮”）。

   -----

   .. method:: recover_char_quad(line_dir, span, char)

      计算通过 :meth:`Page.get_text` 的 "rawdict" 选项提取的文本字符的四边形。

      :arg tuple line_dir: 所属行的 `line["dir"]`。对于从 :meth:`Page.get_texttrace` 提取的跨度，请使用 `None`。
      :arg dict span: 跨度。
      :arg dict char: 字符。
      :returns: 字符的 :ref:`Quad`，可用于文本标记注释（例如“高亮”）。

   -----

   .. method:: recover_span_quad(line_dir, span, chars=None)

      计算通过 :meth:`Page.get_text` 的 "rawdict" 选项提取的一个跨度中子集字符的四边形。

      :arg tuple line_dir: 所属行的 `line["dir"]`。对于从 :meth:`Page.get_texttrace` 提取的跨度，请使用 `None`。
      :arg dict span: 跨度。
      :arg list chars: 要考虑的字符。如果给定，选择的提取选项必须是 "rawdict"。
      :returns: 所选字符的 :ref:`Quad`，可用于文本标记注释（例如“高亮”）。

   -----

   .. method:: recover_line_quad(line, spans=None)

      计算通过 :meth:`Page.get_text` 的 "dict" 或 "rawdict" 选项提取的文本行的子集跨度的四边形。

      :arg dict line: 该行。
      :arg list spans: `line["spans"]` 的子列表。如果省略，将返回完整的行四边形。
      :returns: 所选行跨度的 :ref:`Quad`，可用于文本标记注释（例如“高亮”）。

   -----

   .. method:: get_tessdata(tessdata=None)

      检测 Tesseract 语言支持文件夹。

      此函数用于启用 Tesseract 的 OCR，即使语言支持文件夹未直接指定或未在环境变量 TESSDATA_PREFIX 中设置。

      * 如果设置了 <tessdata>，我们将直接返回它。
      
      * 否则，如果设置了 `os.environ['TESSDATA_PREFIX']`，则返回它。
      
      * 否则，我们将搜索 Tesseract 安装并返回其语言支持文件夹。

      * 否则，我们会引发异常。

   -----

   .. method:: INFINITE_QUAD()

   .. method:: INFINITE_RECT()

   .. method:: INFINITE_IRECT()

      返回 (唯一的) 无限矩形 `Rect(-2147483648.0, -2147483648.0, 2147483520.0, 2147483520.0)`，分别是 :ref:`IRect` 和 :ref:`Quad` 对应物。它是可能的最大矩形：所有有效的矩形都包含在其中。

   -----

   .. method:: EMPTY_QUAD()

   .. method:: EMPTY_RECT()

   .. method:: EMPTY_IRECT()

      返回 "标准" 空和无效矩形 `Rect(2147483520.0, 2147483520.0, -2147483648.0, -2147483648.0)`，分别是四边形。它的左上角和右下角点的值与无限矩形相反。它将用于表示 `page.get_text("dict")` 字典中的空 bbox。然而，也有无数空的或无效的矩形。

   -----

   .. method:: colors_pdf_dict()

      返回一个字典，将小写颜色名称映射到 `(red, green, blue)` 元组，其中 `red`、`green`、`blue` 是范围在 0 到 1 之间的浮点数。

   .. method:: colors_wx_list()

      返回一个列表，其中包含 `(colorname, red, green, blue)` 元组，其中 `colorname` 为大写，`red`、`green`、`blue` 是范围在 0 到 255 之间的整数。

.. tab:: 英文

   The following are miscellaneous functions and attributes on a fairly low-level technical detail.

   Some functions provide detail access to PDF structures. Others are stripped-down, high performance versions of other functions which provide more information.

   Yet others are handy, general-purpose utilities.


   ==================================== ==============================================================
   **Function**                         **Short Description**
   ==================================== ==============================================================
   :attr:`Annot.apn_bbox`               PDF only: bbox of the appearance object
   :attr:`Annot.apn_matrix`             PDF only: the matrix of the appearance object
   :attr:`Page.is_wrapped`              check whether contents wrapping is present
   :meth:`adobe_glyph_names`            list of glyph names defined in **Adobe Glyph List**
   :meth:`adobe_glyph_unicodes`         list of unicodes defined in **Adobe Glyph List**
   :meth:`Annot.clean_contents`         PDF only: clean the annot's :data:`contents` object
   :meth:`Annot.set_apn_bbox`           PDF only: set the bbox of the appearance object
   :meth:`Annot.set_apn_matrix`         PDF only: set the matrix of the appearance object
   :meth:`ConversionHeader`             return header string for *get_text* methods
   :meth:`ConversionTrailer`            return trailer string for *get_text* methods
   :meth:`Document.del_xml_metadata`    PDF only: remove XML metadata
   :meth:`Document.get_char_widths`     PDF only: return a list of glyph widths of a font
   :meth:`Document.get_new_xref`        PDF only: create and return a new :data:`xref` entry
   :meth:`Document.is_stream`           PDF only: check whether an :data:`xref` is a stream object
   :meth:`Document.xml_metadata_xref`   PDF only: return XML metadata :data:`xref` number
   :meth:`Document.xref_length`         PDF only: return length of :data:`xref` table
   :meth:`EMPTY_IRECT`                  return the (standard) empty / invalid rectangle
   :meth:`EMPTY_QUAD`                   return the (standard) empty / invalid quad
   :meth:`EMPTY_RECT`                   return the (standard) empty / invalid rectangle
   :meth:`get_pdf_now`                  return the current timestamp in PDF format
   :meth:`get_pdf_str`                  return PDF-compatible string
   :meth:`get_text_length`              return string length for a given font & :data:`fontsize`
   :meth:`glyph_name_to_unicode`        return unicode from a glyph name
   :meth:`image_profile`                return a dictionary of basic image properties
   :meth:`INFINITE_IRECT`               return the (only existing) infinite rectangle
   :meth:`INFINITE_QUAD`                return the (only existing) infinite quad
   :meth:`INFINITE_RECT`                return the (only existing) infinite rectangle
   :meth:`make_table`                   split rectangle in sub-rectangles
   :meth:`Page.clean_contents`          PDF only: clean the page's :data:`contents` objects
   :meth:`Page.get_bboxlog`             list of rectangles that envelop text, drawing or image objects
   :meth:`Page.get_contents`            PDF only: return a list of content :data:`xref` numbers
   :meth:`Page.get_displaylist`         create the page's display list
   :meth:`Page.get_text_blocks`         extract text blocks as a Python list
   :meth:`Page.get_text_words`          extract text words as a Python list
   :meth:`Page.get_texttrace`           low-level text information
   :meth:`Page.read_contents`           PDF only: get complete, concatenated /Contents source
   :meth:`Page.run`                     run a page through a device
   :meth:`Page.set_contents`            PDF only: set page's :data:`contents` to some :data:`xref`
   :meth:`Page.wrap_contents`           wrap contents with stacking commands
   :meth:`css_for_pymupdf_font`         create CSS source for a font in package pymupdf_fonts
   :meth:`paper_rect`                   return rectangle for a known paper format
   :meth:`paper_size`                   return width, height for a known paper format
   :meth:`paper_sizes`                  dictionary of pre-defined paper formats
   :meth:`planish_line`                 matrix to map a line to the x-axis
   :meth:`recover_char_quad`            compute the quad of a char ("rawdict")
   :meth:`recover_line_quad`            compute the quad of a subset of line spans
   :meth:`recover_quad`                 compute the quad of a span ("dict", "rawdict")
   :meth:`recover_span_quad`            compute the quad of a subset of span characters
   :meth:`set_messages`                 set destination of |PyMuPDF| messages.
   :meth:`sRGB_to_pdf`                  return PDF RGB color tuple from an sRGB integer
   :meth:`sRGB_to_rgb`                  return (R, G, B) color tuple from an sRGB integer
   :meth:`unicode_to_glyph_name`        return glyph name from a unicode
   :meth:`get_tessdata`                 locates the language support of the Tesseract-OCR installation
   :meth:`colors_pdf_dict`              return dict of color names.
   :meth:`colors_wx_list`               return list of color names.
   :attr:`fitz_fontdescriptors`         dictionary of available supplement fonts
   :attr:`PYMUPDF_MESSAGE`              destination of |PyMuPDF| messages.
   :attr:`pdfcolor`                     dictionary of almost 500 RGB colors in PDF format.
   ==================================== ==============================================================

   .. method:: paper_size(s)
      :no-index:

      Convenience function to return width and height of a known paper format code. These values are given in pixels for the standard resolution 72 pixels = 1 inch.

      Currently defined formats include **'A0'** through **'A10'**, **'B0'** through **'B10'**, **'C0'** through **'C10'**, **'Card-4x6'**, **'Card-5x7'**, **'Commercial'**, **'Executive'**, **'Invoice'**, **'Ledger'**, **'Legal'**, **'Legal-13'**, **'Letter'**, **'Monarch'** and **'Tabloid-Extra'**, each in either portrait or landscape format.

      A format name must be supplied as a string (case **in** \sensitive), optionally suffixed with "-L" (landscape) or "-P" (portrait). No suffix defaults to portrait.

      :arg str s: any format name from above in upper or lower case, like *"A4"* or *"letter-l"*.

      :rtype: tuple
      :returns: *(width, height)* of the paper format. For an unknown format *(-1, -1)* is returned. Examples: *pymupdf.paper_size("A4")* returns *(595, 842)* and *pymupdf.paper_size("letter-l")* delivers *(792, 612)*.

   -----

   .. method:: paper_rect(s)
      :no-index:

      Convenience function to return a :ref:`Rect` for a known paper format.

      :arg str s: any format name supported by :meth:`paper_size`.

      :rtype: :ref:`Rect`
      :returns: *pymupdf.Rect(0, 0, width, height)* with *width, height=pymupdf.paper_size(s)*.

      >>> import pymupdf
      >>> pymupdf.paper_rect("letter-l")
      pymupdf.Rect(0.0, 0.0, 792.0, 612.0)
      >>>

   -----

   .. method:: set_messages(*, text=None, fd=None, stream=None, path=None, path_append=None, pylogging=None, pylogging_logger=None, pylogging_level=None, pylogging_name=None, )
      :no-index:

      Sets destination of |PyMuPDF| messages to a file descriptor,
      a file, an existing stream or `Python's logging system
      <https://docs.python.org/3/library/logging.html>`_.

      Usually one would only set one arg, or one or more `pylogging*` args.

      :arg str text:
            A text specification of destination; for details see description of
            environmental variable `PYMUPDF_MESSAGE`.
      :arg int fd:
            Write to file descriptor.
      :arg stream:
            Write to existing stream, which must have methods `.write(text)` and
            `.flush()`.
      :arg str path:
            Write to a file.
      :arg str path_append:
            Append to a file.
      :arg pylogging:
            Write to Python's `logging` system.
      :arg logging.Logger pylogging_logger:
            Write to Python's `logging` system using specified Logger.
      :arg int pylogging_level:
            Write to Python's `logging` system using specified level.
      :arg str pylogging_name:
            Write to Python's `logging` system using specified logger name.
            Only used if `pylogging_logger` is None. Default is `pymupdf`.

      If any `pylogging*` arg is not None, we write to `Python's logging system
      <https://docs.python.org/3/library/logging.html>`_.

   -----

   .. method:: sRGB_to_pdf(srgb)
      :no-index:

      *New in v1.17.4*

      Convenience function returning a PDF color triple (red, green, blue) for a given sRGB color integer as it occurs in :meth:`Page.get_text` dictionaries "dict" and "rawdict".

      :arg int srgb: an integer of format RRGGBB, where each color component is an integer in range(255).

      :returns: a tuple (red, green, blue) with float items in interval *0 <= item <= 1* representing the same color. Example `sRGB_to_pdf(0xff0000) = (1, 0, 0)` (red).

   -----

   .. method:: sRGB_to_rgb(srgb)
      :no-index:

      *New in v1.17.4*

      Convenience function returning a color (red, green, blue) for a given *sRGB* color integer.

      :arg int srgb: an integer of format RRGGBB, where each color component is an integer in range(255).

      :returns: a tuple (red, green, blue) with integer items in `range(256)` representing the same color. Example `sRGB_to_pdf(0xff0000) = (255, 0, 0)` (red).

   -----

   .. method:: glyph_name_to_unicode(name)
      :no-index:

      *New in v1.18.0*

      Return the unicode number of a glyph name based on the **Adobe Glyph List**.

      :arg str name: the name of some glyph. The function is based on the `Adobe Glyph List <https://github.com/adobe-type-tools/agl-aglfn/blob/master/glyphlist.txt>`_.

      :rtype: int
      :returns: the unicode. Invalid *name* entries return `0xfffd (65533)`.

      .. note:: A similar functionality is provided by package `fontTools <https://pypi.org/project/fonttools/>`_ in its *agl* sub-package.

   -----

   .. method:: unicode_to_glyph_name(ch)
      :no-index:

      *New in v1.18.0*

      Return the glyph name of a unicode number, based on the **Adobe Glyph List**.

      :arg int ch: the unicode given by e.g. `ord("ß")`. The function is based on the `Adobe Glyph List <https://github.com/adobe-type-tools/agl-aglfn/blob/master/glyphlist.txt>`_.

      :rtype: str
      :returns: the glyph name. E.g. `pymupdf.unicode_to_glyph_name(ord("Ä"))` returns `'Adieresis'`.

      .. note:: A similar functionality is provided by package `fontTools <https://pypi.org/project/fonttools/>`_: in its *agl* sub-package.

   -----

   .. method:: adobe_glyph_names()
      :no-index:

      *New in v1.18.0*

      Return a list of glyph names defined in the **Adobe Glyph List**.

      :rtype: list
      :returns: list of strings.

      .. note:: A similar functionality is provided by package `fontTools <https://pypi.org/project/fonttools/>`_ in its *agl* sub-package.

   -----

   .. method:: adobe_glyph_unicodes()
      :no-index:

      *New in v1.18.0*

      Return a list of unicodes for there exists a glyph name in the **Adobe Glyph List**.

      :rtype: list
      :returns: list of integers.

      .. note:: A similar functionality is provided by package `fontTools <https://pypi.org/project/fonttools/>`_ in its *agl* sub-package.

   -----

   .. method:: css_for_pymupdf_font(fontcode, *, CSS=None, archive=None, name=None)
      :no-index:

      *New in v1.21.0*

      **Utility function for use with "Story" applications.**

      Create CSS `@font-face` items for the given fontcode in pymupdf-fonts. Creates a CSS font-family for all fonts starting with string "fontcode".

      The font naming convention in package pymupdf-fonts is "fontcode<sf>", where the suffix "sf" is one of "" (empty), "it"/"i", "bo"/"b" or "bi". These suffixes thus represent the regular, italic, bold or bold-italic variants of that font.

      For example, font code "notos" refers to fonts

      *  "notos" - "Noto Sans Regular"
      *  "notosit" - "Noto Sans Italic"
      *  "notosbo" - "Noto Sans Bold"
      *  "notosbi" - "Noto Sans Bold Italic"

      The function creates (up to) four CSS `@font-face` definitions and collectively assigns the `font-family` name "notos" to them (or the "name" value if provided). Associated font buffers are placed / added to the provided archive.

      To use the font in the Python API for :ref:`Story`, execute `.set_font(fontcode)` (or "name" if given). The correct font weight or style will automatically be selected as required.

      For example to replace the "sans-serif" HTML standard (i.e. Helvetica) with the above "notos", execute the following. Whenever "sans-serif" is used (whether explicitly or implicitly), the Noto Sans fonts will be selected.

      `CSS = pymupdf.css_for_pymupdf_font("notos", name="sans-serif", archive=...)`

      Expects and returns the CSS source, with the new CSS definitions appended.

      :arg str fontcode: one of the font codes present in package `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_ (usually) representing the regular version of the font family.
      :arg str CSS: any already existing CSS source, or `None`. The function will append its new definitions to this. This is the string that **must be used** as `user_css` when creating the :ref:`Story`.
      :arg archive: :ref:`Archive`, **mandatory**. All font binaries (i.e. up to four) found for "fontcode" will be added to the archive. This is the archive that **must be used** as `archive` when creating the :ref:`Story`.
      :arg str name: the name under which the "fontcode" fonts should be found. If omitted, "fontcode" will be used.

      :rtype: str
      :returns: Modified CSS, with appended `@font-face` statements for each font variant of fontcode. Fontbuffers associated with "fontcode" will have been added to 'archive'. The function will automatically find up to 4 font variants. All pymupdf-fonts (that are no special purpose like math or music, etc.) have regular, bold, italic and bold-italic variants. To see currently available font codes check `pymupdf.fitz_fontdescriptors.keys()`. This will show something like `dict_keys(['cascadia', 'cascadiai', 'cascadiab', 'cascadiabi', 'figbo', 'figo', 'figbi', 'figit', 'fimbo', 'fimo', 'spacembo', 'spacembi', 'spacemit', 'spacemo', 'math', 'music', 'symbol1', 'symbol2', 'notosbo', 'notosbi', 'notosit', 'notos', 'ubuntu', 'ubuntubo', 'ubuntubi', 'ubuntuit', 'ubuntm', 'ubuntmbo', 'ubuntmbi', 'ubuntmit'])`.

      Here is a complete snippet for using the "Noto Sans" font instead of "Helvetica"::

         arch = pymupdf.Archive()
         CSS = pymupdf.css_for_pymupdf_font("notos", name="sans-serif", archive=arch)
         story = pymupdf.Story(user_css=CSS, archive=arch)


   -----

   .. method:: make_table(rect, cols=1, rows=1)
      :no-index:

      *New in v1.17.4*

      Convenience function to split a rectangle into sub-rectangles of equal size. Returns a list of `rows` lists, each containing `cols` :ref:`Rect` items. Each sub-rectangle can then be addressed by its row and column index.

      :arg rect_like rect: the rectangle to split.
      :arg int cols: the desired number of columns.
      :arg int rows: the desired number of rows.
      :returns: a list of :ref:`Rect` objects of equal size, whose union equals *rect*. Here is the layout of a 3x4 table created by `cell = pymupdf.make_table(rect, cols=4, rows=3)`:

      .. image:: images/img-make-table.*
         :scale: 60


   -----

   .. method:: planish_line(p1, p2)
      :no-index:

      * New in version 1.16.2)*

      Return a matrix which maps the line from p1 to p2 to the x-axis such that p1 will become (0,0) and p2 a point with the same distance to (0,0).

      :arg point_like p1: starting point of the line.
      :arg point_like p2: end point of the line.

      :rtype: :ref:`Matrix`
      :returns: a matrix which combines a rotation and a translation::

            >>> p1 = pymupdf.Point(1, 1)
            >>> p2 = pymupdf.Point(4, 5)
            >>> abs(p2 - p1)  # distance of points
            5.0
            >>> m = pymupdf.planish_line(p1, p2)
            >>> p1 * m
            Point(0.0, 0.0)
            >>> p2 * m
            Point(5.0, -5.960464477539063e-08)
            >>> # distance of the resulting points
            >>> abs(p2 * m - p1 * m)
            5.0


         .. image:: images/img-planish.png
            :scale: 40


   -----

   .. method:: paper_sizes
      :no-index:

      A dictionary of pre-defines paper formats. Used as basis for :meth:`paper_size`.

   -----

   .. attribute:: fitz_fontdescriptors
      :no-index:

      * New in v1.17.5

      A dictionary of usable fonts from repository `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_. Items are keyed by their reserved fontname and provide information like this::

         In [2]: pymupdf.fitz_fontdescriptors.keys()
         Out[2]: dict_keys(['figbo', 'figo', 'figbi', 'figit', 'fimbo', 'fimo',
         'spacembo', 'spacembi', 'spacemit', 'spacemo', 'math', 'music', 'symbol1',
         'symbol2'])
         In [3]: pymupdf.fitz_fontdescriptors["fimo"]
         Out[3]:
         {'name': 'Fira Mono Regular',
         'size': 125712,
         'mono': True,
         'bold': False,
         'italic': False,
         'serif': True,
         'glyphs': 1485}

      If `pymupdf-fonts` is not installed, the dictionary is empty.

      The dictionary keys can be used to define a :ref:`Font` via e.g. `font = pymupdf.Font("fimo")` -- just like you can do it with the builtin fonts "Helvetica" and friends.

   -----

   .. attribute:: PYMUPDF_MESSAGE
      :no-index:

      If in `os.environ` when |PyMuPDF| is imported, sets destination of
      |PyMuPDF| messages. Otherwise messages are sent to `sys.stdout`.
      
      *
         If the value starts with `fd:`, the remaining text should be an integer
         file descriptor to which messages are written.
      
         * For example `PYMUPDF_MESSAGE=fd:2` will send messages to stderr.
      *
         If the value starts with `path:`, the remaining text is the path of a
         file to which messages are written. If the file already exists, it is
         truncated.
      *
         If the value starts with `path+:`, the remaining text is the path of
         file to which messages are written. If the file already exists, we
         append output.

      *
         If the value starts with `logging:`, messages are written to `Python's
         logging system <https://docs.python.org/3/library/logging.html>`_. The
         remaining text can contain comma-separated name=value items:
      
         * `level=<int>` sets the logging level.
         * `name=<str>` sets the logger name (default is `pymupdf`).
      
         Other items are ignored.
      
      * Other prefixes will cause an error.
      
      Also see `set_messages()`.


   -----

   .. attribute:: pdfcolor
      :no-index:

      * New in v1.19.6

      Contains about 500 RGB colors in PDF format with the color name as key. To see what is there, you can obviously look at `pymupdf.pdfcolor.keys()`.

      Examples:

         * `pymupdf.pdfcolor["red"] = (1.0, 0.0, 0.0)`
         * `pymupdf.pdfcolor["skyblue"] = (0.5294117647058824, 0.807843137254902, 0.9215686274509803)`
         * `pymupdf.pdfcolor["wheat"] = (0.9607843137254902, 0.8705882352941177, 0.7019607843137254)`

   -----

   .. method:: get_pdf_now()
      :no-index:

      Convenience function to return the current local timestamp in PDF compatible format, e.g. *D:20170501121525-04'00'* for local datetime May 1, 2017, 12:15:25 in a timezone 4 hours westward of the UTC meridian.

      :rtype: str
      :returns: current local PDF timestamp.

   -----

   .. method:: get_text_length(text, fontname="helv", fontsize=11, encoding=TEXT_ENCODING_LATIN)
      :no-index:

      * New in version 1.14.7

      Calculate the length of text on output with a given **builtin** font, :data:`fontsize` and encoding.

      :arg str text: the text string.
      :arg str fontname: the fontname. Must be one of either the :ref:`Base-14-Fonts` or the CJK fonts, identified by their "reserved" fontnames (see table in :meth:`Page.insert_font`).
      :arg float fontsize: the :data:`fontsize`.
      :arg int encoding: the encoding to use. Besides 0 = Latin, 1 = Greek and 2 = Cyrillic (Russian) are available. Relevant for Base-14 fonts "Helvetica", "Courier" and "Times" and their variants only. Make sure to use the same value as in the corresponding text insertion.
      :rtype: float
      :returns: the length in points the string will have (e.g. when used in :meth:`Page.insert_text`).

      .. note:: This function will only do the calculation -- it won't insert font nor text.

      .. note:: The :ref:`Font` class offers a similar method, :meth:`Font.text_length`, which supports Base-14 fonts and any font with a character map (CMap, Type 0 fonts).

      .. warning:: If you use this function to determine the required rectangle width for the (:ref:`Page` or :ref:`Shape`) *insert_textbox* methods, be aware that they calculate on a **by-character level**. Because of rounding effects, this will mostly lead to a slightly larger number: *sum([pymupdf.get_text_length(c) for c in text]) > pymupdf.get_text_length(text)*. So either (1) do the same, or (2) use something like *pymupdf.get_text_length(text + "'")* for your calculation.

   -----

   .. method:: get_pdf_str(text)
      :no-index:

      Make a PDF-compatible string: if the text contains code points *ord(c) > 255*, then it will be converted to UTF-16BE with BOM as a hexadecimal character string enclosed in "<>" brackets like *<feff...>*. Otherwise, it will return the string enclosed in (round) brackets, replacing any characters outside the ASCII range with some special code. Also, every "(", ")" or backslash is escaped with a backslash.

      :arg str text: the object to convert

      :rtype: str
      :returns: PDF-compatible string enclosed in either *()* or *<>*.

   -----

   .. method:: image_profile(stream)
      :no-index:

      * New in v1.16.7
      * Changed in v1.19.5: also return natural image orientation extracted from EXIF data if present.
      * Changed in v1.22.5: always return `None` in error cases instead of an empty dictionary.

      Show important properties of an image provided as a memory area. Its main purpose is to avoid using other Python packages just to determine them.

      :arg bytes|bytearray|BytesIO|file stream: either an image in memory or an **opened** file. An image in memory may be any of the formats `bytes`, `bytearray` or `io.BytesIO`.

      :rtype: dict
      :returns:
         No exception is ever raised. In case of an error, `None` is returned. Otherwise, there are the following items::

            In [2]: pymupdf.image_profile(open("nur-ruhig.jpg", "rb").read())
            Out[2]:
            {'width': 439,
            'height': 501,
            'orientation': 0,  # natural orientation (from EXIF)
            'transform': (1.0, 0.0, 0.0, 1.0, 0.0, 0.0),  # orientation matrix
            'xres': 96,
            'yres': 96,
            'colorspace': 3,
            'bpc': 8,
            'ext': 'jpeg',
            'cs-name': 'DeviceRGB'}

         There is the following relation to **Exif** information encoded in `orientation`, and correspondingly in the `transform` matrix-like (quoted from MuPDF documentation, *ccw* = counter-clockwise):

            1. Undefined
            2. 0 degree ccw rotation. (Exif = 1)
            3. 90 degree ccw rotation. (Exif = 8)
            4. 180 degree ccw rotation. (Exif = 3)
            5. 270 degree ccw rotation. (Exif = 6)
            6. flip on X. (Exif = 2)
            7. flip on X, then rotate ccw by 90 degrees. (Exif = 5)
            8. flip on X, then rotate ccw by 180 degrees. (Exif = 4)
            9. flip on X, then rotate ccw by 270 degrees. (Exif = 7)


         .. note::

            * For some "exotic" images (FAX encodings, RAW formats and the like), this method will not work. You can however still work with such images in PyMuPDF, e.g. by using :meth:`Document.extract_image` or create pixmaps via `Pixmap(doc, xref)`. These methods will automatically convert exotic images to the PNG format before returning results.
            * You can also get the properties of images embedded in a PDF, via their :data:`xref`. In this case make sure to extract the raw stream: `pymupdf.image_profile(doc.xref_stream_raw(xref))`.
            * Images as returned by the image blocks of :meth:`Page.get_text` using "dict" or "rawdict" options are also supported.


   -----

   .. method:: ConversionHeader("text", filename="UNKNOWN")
      :no-index:

      Return the header string required to make a valid document out of page text outputs.

      :arg str output: type of document. Use the same as the output parameter of *get_text()*.

      :arg str filename: optional arbitrary name to use in output types "json" and "xml".

      :rtype: str

   -----

   .. method:: ConversionTrailer(output)
      :no-index:

      Return the trailer string required to make a valid document out of page text outputs. See :meth:`Page.get_text` for an example.

      :arg str output: type of document. Use the same as the output parameter of *get_text()*.

      :rtype: str

   -----

   .. method:: Document.del_xml_metadata()
      :no-index:

      Delete an object containing XML-based metadata from the PDF. (Py-) MuPDF does not support XML-based metadata. Use this if you want to make sure that the conventional metadata dictionary will be used exclusively. Many thirdparty PDF programs insert their own metadata in XML format and thus may override what you store in the conventional dictionary. This method deletes any such reference, and the corresponding PDF object will be deleted during next garbage collection of the file.

   -----

   .. method:: Document.xml_metadata_xref()
      :no-index:

      Return the XML-based metadata :data:`xref` of the PDF if present -- also refer to :meth:`Document.del_xml_metadata`. You can use it to retrieve the content via :meth:`Document.xref_stream` and then work with it using some XML software.

      :rtype: int
      :returns: :data:`xref` of PDF file level XML metadata -- or 0 if none exists.

   -----

   .. method:: Page.run(dev, transform)
      :no-index:

      Run a page through a device.

      :arg dev: Device, obtained from one of the :ref:`Device` constructors.
      :type dev: :ref:`Device`

      :arg transform: Transformation to apply to the page. Set it to :ref:`Identity` if no transformation is desired.
      :type transform: :ref:`Matrix`

   -----

   .. method:: Page.get_bboxlog(layers=False)
      :no-index:

      * New in v1.19.0
      * Changed in v1.22.0: optionally also return the OCG name applicable to the boundary box.

      :returns: a list of rectangles that envelop text, image or drawing objects. Each item is a tuple `(type, (x0, y0, x1, y1))` where the second tuple consists of rectangle coordinates, and *type* is one of the following values. If `layers=True`, there is a third item containing the OCG name or `None`: `(type, (x0, y0, x1, y1), None)`.

         * `"fill-text"` -- normal text (painted without character borders)
         * `"stroke-text"` -- text showing character borders only
         * `"ignore-text"` -- text that should not be displayed (e.g. as used by OCR text layers)
         * `"fill-path"` -- drawing with fill color (and no border)
         * `"stroke-path"` -- drawing with border (and no fill color)
         * `"fill-image"` -- displays an image
         * `"fill-shade"` -- display a shading

         The item sequence represents the **sequence in which these commands are executed** to build the page's appearance. Therefore, if an item's bbox intersects or contains that of a previous item, then the previous item may be (partially) covered / hidden.


         So this list can be used to detect such situations. An item's index in this list equals the value of a `"seqno"` in dictionaries as returned by :meth:`Page.get_drawings` and :meth:`Page.get_texttrace`.


   -----

   .. method:: Page.get_texttrace()
      :no-index:

      * New in v1.18.16
      * Changed in v1.19.0: added key "seqno".
      * Changed in v1.19.1: stroke and fill colors now always are either RGB or GRAY
      * Changed in v1.19.3: span and character bboxes are now also correct if `dir != (1, 0)`.
      * Changed in v1.22.0: add new dictionary key "layer".


      Return low-level text information of the page. The method is available for **all** document types. The result is a list of Python dictionaries with the following content::

         {
            'ascender': 0.83251953125,          # font ascender (1)
            'bbox': (458.14019775390625,        # span bbox x0 (7)
                     749.4671630859375,         # span bbox y0
                     467.76458740234375,        # span bbox x1
                     757.5071411132812),        # span bbox y1
            'bidi': 0,                          # bidirectional level (1)
            'chars': (                          # char information, tuple[tuple]
                        (45,                    # unicode (4)
                        16,                     # glyph id (font dependent)
                        (458.14019775390625,    # origin.x (1)
                        755.3758544921875),     # origin.y (1)
                        (458.14019775390625,    # char bbox x0 (6)
                        749.4671630859375,      # char bbox y0
                        462.9649963378906,      # char bbox x1
                        757.5071411132812)),    # char bbox y1
                        ( ... ),                # more characters
                     ),
            'color': (0.0,),                    # text color, tuple[float] (1)
            'colorspace': 1,                    # number of colorspace components (1)
            'descender': -0.30029296875,        # font descender (1)
            'dir': (1.0, 0.0),                  # writing direction (1)
            'flags': 12,                        # font flags (1)
            'font': 'CourierNewPSMT',           # font name (1)
            'linewidth': 0.4019999980926514,    # current line width value (3)
            'opacity': 1.0,                     # alpha value of the text (5)
            'layer': None,                      # name of Optional Content Group (9)
            'seqno': 246,                       # sequence number (8)
            'size': 8.039999961853027,          # font size (1)
            'spacewidth': 4.824785133358091,    # width of space char
            'type': 0,                          # span type (2)
            'wmode': 0                          # writing mode (1)
         }

      Details:

      1. Information above tagged with "(1)" has the same meaning and value as explained in :ref:`TextPage`.

         - Please note that the font ``flags`` value will never contain a *superscript* flag bit: the detection of superscripts is done within MuPDF :ref:`TextPage` code -- it is not a property of any font.
         - Also note, that the text *color* is encoded as the usual tuple of floats 0 <= f <= 1 -- not in sRGB format. Depending on `span["type"]`, interpret this as fill color or stroke color.

      2. There are 3 text span types:

         - 0: Filled text -- equivalent to PDF text rendering mode 0 (`0 Tr`, the default in PDF), only each character's "inside" is shown.
         - 1: Stroked text -- equivalent to `1 Tr`, only the character borders are shown.
         - 3: Ignored text -- equivalent to `3 Tr` (hidden text).

      3. Line width in this context is important only for processing `span["type"] != 0`: it determines the thickness of the character's border line. This value may not be provided at all with the text data. In this case, a value of 5% of the :data:`fontsize` (`span["size"] * 0,05`) is generated. Often, an "artificial" bold text in PDF is created by `2 Tr`. There is no equivalent span type for this case. Instead, respective text is represented by two consecutive spans -- which are identical in every aspect, except for their types, which are 0, resp 1. It is your responsibility to handle this type of situation - in :meth:`Page.get_text`, MuPDF is doing this for you.
      4. For data compactness, the character's unicode is provided here. Use built-in function `chr()` for the character itself.
      5. The alpha / opacity value of the span's text, `0 <= opacity <= 1`, 0 is invisible text, 1 (100%) is intransparent. Depending on `span["type"]`, interpret this value as *fill* opacity or, resp. *stroke* opacity.
      6. *(Changed in v1.19.0)* This value is equal or close to `char["bbox"]` of "rawdict". In particular, the bbox **height** value is always computed as if **"small glyph heights"** had been requested.
      7. *(New in v1.19.0)* This is the union of all character bboxes.
      8. *(New in v1.19.0)* Enumerates the commands that build up the page's appearance. Can be used to find out whether text is effectively hidden by objects, which are painted "later", or *over* some object. So if there is a drawing or image with a higher sequence number, whose bbox overlaps (parts of) this text span, one may assume that such an object hides the resp. text. Different text spans have identical sequence numbers if they were created in one go.
      9. *(New in v1.22.0)* The name of the Optional Content Group (OCG) if applicable or `None`.

      Here is a list of similarities and differences of `page.get_texttrace()` compared to `page.get_text("rawdict")`:

      * The method is up to **twice as fast,** compared to "rawdict" extraction. Depends on the amount of text.
      * The returned data is very **much smaller in size** -- although it provides more information.
      * Additional types of text **invisibility can be detected**: opacity = 0 or type > 1 or overlapping bbox of an object with a higher sequence number.
      * If MuPDF returns unicode 0xFFFD (65533) for unrecognized characters, you may still be able to deduct desired information from the glyph id.
      * The `span["chars"]` **contains no spaces**, **except** the document creator has explicitly coded them. They **will never be generated** like it happens in :meth:`Page.get_text` methods. To provide some help for doing your own computations here, the width of a space character is given. This value is derived from the font where possible. Otherwise the value of a fallback font is taken.
      * There is no effort to organize text like it happens for a :ref:`TextPage` (the hierarchy of blocks, lines, spans, and characters). Characters are simply extracted in sequence, one by one, and put in a span. Whenever any of the span's characteristics changes, a new span is started. So you may find characters with different `origin.y` values in the same span (which means they would appear in different lines). You cannot assume, that span characters are sorted in any particular order -- you must make sense of the info yourself, taking `span["dir"]`, `span["wmode"]`, etc. into account.
      * Ligatures are represented like this:
         - MuPDF handles the following ligatures: "fi", "ff", "fl", "ft", "st", "ffi", and "ffl" (only the first 3 are mostly ever used). If the page contains e.g. ligature "fi", you will find the following two character items subsequent to each other::

            (102, glyph, (x, y), (x0, y0, x1, y1))  # 102 = ord("f")
            (105, -1, (x, y), (x0, y0, x0, y1))     # 105 = ord("i"), empty bbox!

         - This means that the bbox of the first ligature character is the area containing the complete, compound glyph. Subsequent ligature components are recognizable by their glyph value -1 and a bbox of width zero.
         - You may want to replace those 2 or 3 char tuples by one, that represents the ligature itself. Use the following mapping of ligatures to unicodes:

            + `"ff" -> 0xFB00`
            + `"fi" -> 0xFB01`
            + `"fl" -> 0xFB02`
            + `"ffi" -> 0xFB03`
            + `"ffl" -> 0xFB04`
            + `"ft" -> 0xFB05`
            + `"st" -> 0xFB06`

            So you may want to replace the two example tuples above by the following single one: `(0xFB01, glyph, (x, y), (x0, y0, x1, y1))` (there is usually no need to lookup the correct glyph id for 0xFB01 in the resp. font, but you may execute `font.has_glyph(0xFB01)` and use its return value).

      * **Changed in v1.19.3:** Similar to other text extraction methods, the character and span bboxes envelop the character quads. To recover the quads, follow the same methods :meth:`recover_quad`, :meth:`recover_char_quad` or :meth:`recover_span_quad` as explained in :ref:`textpagedict`. Use either `None` or `span["dir"]` for the writing direction.

      * **Changed in v1.21.1:** If applicable, the name of the OCG is shown in `"layer"`.

   -----

   .. method:: Page.wrap_contents()
      :no-index:

      Ensures that the page's so-called graphics state is balanced and new content can be inserted correctly.
      
      In versions 1.24.1+ of PyMuPDF the method was improved and is being executed automatically as required, so you should no longer need to concern yourself with it.

      We discourage using :meth:`Page.clean_contents` to achieve this.

   -----

   .. attribute:: Page.is_wrapped
      :no-index:

      Indicate whether the page's so-called graphic state is balanced. If `False`, :meth:`Page.wrap_contents` should be executed if new content is inserted (only relevant in `overlay=True` mode). In newer versions (1.24.1+), this check and corresponding adjustments are automatically executed -- you therefore should not be concerned about this anymore.

      :rtype: bool

   -----

   .. method:: Page.get_text_blocks(flags=None)
      :no-index:

      Deprecated wrapper for :meth:`TextPage.extractBLOCKS`.  Use :meth:`Page.get_text` with the "blocks" option instead.

      :rtype: list[tuple]

   -----

   .. method:: Page.get_text_words(flags=None, delimiters=None)
      :no-index:

      Deprecated wrapper for :meth:`TextPage.extractWORDS`. Use :meth:`Page.get_text` with the "words" option instead.

      :rtype: list[tuple]

   -----

   .. method:: Page.get_displaylist()
      :no-index:

      Run a page through a list device and return its display list.

      :rtype: :ref:`DisplayList`
      :returns: the display list of the page.

   -----

   .. method:: Page.get_contents()
      :no-index:

      PDF only: Retrieve a list of :data:`xref` of :data:`contents` objects of a page. May be empty or contain multiple integers. If the page is cleaned (:meth:`Page.clean_contents`), it will be no more than one entry. The "source" of each `/Contents` object can be individually read by :meth:`Document.xref_stream` using an item of this list. Method :meth:`Page.read_contents` in contrast walks through this list and concatenates the corresponding sources into one `bytes` object.

      :rtype: list[int]

   -----

   .. method:: Page.set_contents(xref)
      :no-index:

      PDF only: Let the page's `/Contents` key point to this xref. Any previously used contents objects will be ignored and can be removed via garbage collection.

   -----

   .. method:: Page.clean_contents(sanitize=True)
      :no-index:

      * Changed in v1.17.6

      PDF only: Clean and concatenate all :data:`contents` objects associated with this page. "Cleaning" includes syntactical corrections, standardizations and "pretty printing" of the contents stream. Discrepancies between :data:`contents` and :data:`resources` objects will also be corrected if sanitize is true. See :meth:`Page.get_contents` for more details.

      Changed in version 1.16.0 Annotations are no longer implicitly cleaned by this method. Use :meth:`Annot.clean_contents` separately.

      :arg bool sanitize: *(new in v1.17.6)* if true, synchronization between resources and their actual use in the contents object is snychronized. For example, if a font is not actually used for any text of the page, then it will be deleted from the `/Resources/Font` object.

      .. warning:: This is a complex function which may generate large amounts of new data and render old data unused. It is **not recommended** using it together with the **incremental save** option. Also note that the resulting singleton new */Contents* object is **uncompressed**. So you should save to a **new file** using options *"deflate=True, garbage=3"*.

         Do not any longer use this method to ensure correct insertions on PDF pages. Since PyMuPDF version 1.24.2 this is taken care of automatically.

   -----

   .. method:: Page.read_contents()
      :no-index:

      *New in version 1.17.0.*
      Return the concatenation of all :data:`contents` objects associated with the page -- without cleaning or otherwise modifying them. Use this method whenever you need to parse this source in its entirety without having to bother how many separate contents objects exist.

      :rtype: bytes

   -----

   .. method:: Annot.clean_contents(sanitize=True)
      :no-index:

      Clean the :data:`contents` streams associated with the annotation. This is the same type of action which :meth:`Page.clean_contents` performs -- just restricted to this annotation.


   -----

   .. method:: Document.get_char_widths(xref=0, limit=256)
      :no-index:

      Return a list of character glyphs and their widths for a font that is present in the document. A font must be specified by its PDF cross reference number :data:`xref`. This function is called automatically from :meth:`Page.insert_text` and :meth:`Page.insert_textbox`. So you should rarely need to do this yourself.

      :arg int xref: cross reference number of a font embedded in the PDF. To find a font :data:`xref`, use e.g. *doc.get_page_fonts(pno)* of page number *pno* and take the first entry of one of the returned list entries.

      :arg int limit: limits the number of returned entries. The default of 256 is enforced for all fonts that only support 1-byte characters, so-called "simple fonts" (checked by this method). All :ref:`Base-14-Fonts` are simple fonts.

      :rtype: list
      :returns: a list of *limit* tuples. Each character *c* has an entry  *(g, w)* in this list with an index of *ord(c)*. Entry *g* (integer) of the tuple is the glyph id of the character, and float *w* is its normalized width. The actual width for some :data:`fontsize` can be calculated as *w * fontsize*. For simple fonts, the *g* entry can always be safely ignored. In all other cases *g* is the basis for graphically representing *c*.

      This function calculates the pixel width of a string called *text*::

         def pixlen(text, widthlist, fontsize):
            try:
               return sum([widthlist[ord(c)] for c in text]) * fontsize
            except IndexError:
               raise ValueError:("max. code point found: %i, increase limit" % ord(max(text)))

   -----

   .. method:: Document.is_stream(xref)
      :no-index:

      * New in version 1.14.14

      PDF only: Check whether the object represented by :data:`xref` is a :data:`stream` type. Return is *False* if not a PDF or if the number is outside the valid xref range.

      :arg int xref: :data:`xref` number.

      :returns: *True* if the object definition is followed by data wrapped in keyword pair *stream*, *endstream*.

   -----

   .. method:: Document.get_new_xref()
      :no-index:

      Increase the :data:`xref` by one entry and return that number. This can then be used to insert a new object.

      :rtype: int
            :returns: the number of the new :data:`xref` entry. Please note, that only a new entry in the PDF's cross reference table is created. At this point, there will not yet exist a PDF object associated with it. To create an (empty) object with this number use `doc.update_xref(xref, "<<>>")`.

   -----

   .. method:: Document.xref_length()
      :no-index:

      Return length of :data:`xref` table.

      :rtype: int
      :returns: the number of entries in the :data:`xref` table.

   -----

   .. method:: recover_quad(line_dir, span)
      :no-index:

      Compute the quadrilateral of a text span extracted via options "dict" or "rawdict" of :meth:`Page.get_text`.

      :arg tuple line_dir: `line["dir"]` of the owning line.  Use `None` for a span from :meth:`Page.get_texttrace`.
      :arg dict span: the span.
      :returns: the :ref:`Quad` of the span, usable for text marker annotations ('Highlight', etc.).

   -----

   .. method:: recover_char_quad(line_dir, span, char)
      :no-index:

      Compute the quadrilateral of a text character extracted via option "rawdict" of :meth:`Page.get_text`.

      :arg tuple line_dir: `line["dir"]` of the owning line. Use `None` for a span from :meth:`Page.get_texttrace`.
      :arg dict span: the span.
      :arg dict char: the character.
      :returns: the :ref:`Quad` of the character, usable for text marker annotations ('Highlight', etc.).

   -----

   .. method:: recover_span_quad(line_dir, span, chars=None)
      :no-index:

      Compute the quadrilateral of a subset of characters of a span extracted via option "rawdict" of :meth:`Page.get_text`.

      :arg tuple line_dir: `line["dir"]` of the owning line. Use `None` for a span from :meth:`Page.get_texttrace`.
      :arg dict span: the span.
      :arg list chars: the characters to consider. If given, the selected extraction option must be "rawdict".
      :returns: the :ref:`Quad` of the selected characters, usable for text marker annotations ('Highlight', etc.).

   -----

   .. method:: recover_line_quad(line, spans=None)
      :no-index:

      Compute the quadrilateral of a subset of spans of a text line extracted via options "dict" or "rawdict" of :meth:`Page.get_text`.

      :arg dict line: the line.
      :arg list spans: a sub-list of `line["spans"]`. If omitted, the full line quad will be returned.
      :returns: the :ref:`Quad` of the selected line spans, usable for text marker annotations ('Highlight', etc.).

   -----

   .. method:: get_tessdata(tessdata=None)
      :no-index:
      
      Detect Tesseract language support folder.

      This function is used to enable OCR via Tesseract even if the language
      support folder is not specified directly or in environment variable
      TESSDATA_PREFIX.

      * If <tessdata> is set we return it directly.
      
      * Otherwise we return `os.environ['TESSDATA_PREFIX']` if set.
      
      * Otherwise we search for a Tesseract installation and return its language
      support folder.

      * Otherwise we raise an exception.

   -----

   .. method:: INFINITE_QUAD()
      :no-index:

   .. method:: INFINITE_RECT()
      :no-index:

   .. method:: INFINITE_IRECT()
      :no-index:

      Return the (unique) infinite rectangle `Rect(-2147483648.0, -2147483648.0, 2147483520.0, 2147483520.0)`, resp. the :ref:`IRect` and :ref:`Quad` counterparts. It is the largest possible rectangle: all valid rectangles are contained in it.

   -----

   .. method:: EMPTY_QUAD()
      :no-index:

   .. method:: EMPTY_RECT()
      :no-index:

   .. method:: EMPTY_IRECT()
      :no-index:

      Return the "standard" empty and invalid rectangle `Rect(2147483520.0, 2147483520.0, -2147483648.0, -2147483648.0)` resp. quad. Its top-left and bottom-right point values are reversed compared to the infinite rectangle. It will e.g. be used to indicate empty bboxes in `page.get_text("dict")` dictionaries. There are however infinitely many empty or invalid rectangles.

   -----

   .. method:: colors_pdf_dict()
      :no-index:
   
      Returns a dict mapping lower-case color name to `(red, green, blue)`
      tuple, and `red`, `green`, `blue` are floats in range 0..1.

   .. method:: colors_wx_list()
      :no-index:
   
      Returns a list of `(colorname, red, green, blue)` tuples, where
      `colorname` is upper case and `red`, `green`, `blue` are integers in
      range 0..255.

.. include:: footer.rst
