.. include:: header.rst

.. _Font:

================
Font
================

.. tab:: 中文

   * v1.16.18 中新增

   该类表示 MuPDF 定义的字体（*fz_font_s* 结构）。它是新类 :ref:`TextWriter` 和新方法 :meth:`Page.write_text` 所必需的。目前，它与方法 :meth:`Page.insert_text` 或 :meth:`Page.insert_textbox` 的字体使用方式无关。

   Font 对象还包含一些有用的常规信息，例如字体的边界框（bbox）、已定义字形的数量、字形名称或单个字形的边界框。

   ==================================== ============================================
   **方法 / 属性**                      **简要描述**
   ==================================== ============================================
   :meth:`~Font.glyph_advance`          字符宽度
   :meth:`~Font.glyph_bbox`             字形矩形
   :meth:`~Font.glyph_name_to_unicode`  通过字形名称获取 Unicode
   :meth:`~Font.has_glyph`              返回 Unicode 的字形 ID
   :meth:`~Font.text_length`            计算字符串长度
   :meth:`~Font.char_lengths`           获取字符串中每个字符的宽度元组
   :meth:`~Font.unicode_to_glyph_name`  获取 Unicode 对应的字形名称
   :meth:`~Font.valid_codepoints`       支持的 Unicode 代码点数组
   :attr:`~Font.ascender`               字体上升距离
   :attr:`~Font.descender`              字体下降距离
   :attr:`~Font.bbox`                   字体矩形
   :attr:`~Font.buffer`                 字体二进制数据的副本
   :attr:`~Font.flags`                  字体属性集合
   :attr:`~Font.glyph_count`            支持的字形数量
   :attr:`~Font.name`                   字体名称
   :attr:`~Font.is_bold`                如果是粗体，则为 `True`
   :attr:`~Font.is_monospaced`          如果是等宽字体，则为 `True`
   :attr:`~Font.is_serif`               如果是衬线字体，则为 `True`，否则为无衬线 (`False`)
   :attr:`~Font.is_italic`              如果是斜体，则为 `True`
   ==================================== ============================================

   **Class API**

   .. class:: Font

      .. index::
         pair: Font, fontfile
         pair: Font, fontbuffer
         pair: Font, script
         pair: Font, ordering
         pair: Font, is_bold
         pair: Font, is_italic
         pair: Font, is_serif
         pair: Font, fontname
         pair: Font, language

      .. method:: __init__(self, fontname=None, fontfile=None,
                     fontbuffer=None, script=0, language=None, ordering=-1, is_bold=0,
                     is_italic=0, is_serif=0)

         字体构造函数。众多参数用于定位最符合要求的字体。并非所有参数都是必需的 —— 请参考下方伪代码，了解参数的评估逻辑。

         :arg str fontname: 其中之一 :ref:`Base-14-Fonts` 或 CJK 字体名称。此外，还可以使用部分特定的其他字体名称（请注意正确拼写）："Arial"、"Times"、"Times Roman"。
         
            *(v1.17.5 版本更改)*

            如果已安装 `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_，还可以使用新的“保留”字体名称。这些名称列在 :attr:`fitz_fonts` 中，并在下表中列出。

         :arg str fontfile: 系统上的某个字体文件的文件名 [#f3]_。
         :arg bytes,bytearray,io.BytesIO fontbuffer: 加载到内存中的字体文件 [#f3]_。
         :arg int script: UCDN 脚本编号。PyMuPDF 目前支持编号 24 以及 32 到 35。
         :arg str language: 语言代码，可选值包括 "zh-Hant"（繁体中文）、"zh-Hans"（简体中文）、"ja"（日语）和 "ko"（韩语）。此外，还支持所有 ISO 639 代码的子集 1、2、3 和 5，但目前仅作文档记录之用。
         :arg int ordering: 选择 CJK 字体的另一种方式。
         :arg bool is_bold: 查找粗体字体。
         :arg bool is_italic: 查找斜体字体。
         :arg bool is_serif: 查找衬线字体。

         :returns: 成功时返回一个 MuPDF 字体。以下是确定适用字体的检查顺序：

            =========== ============================================================
            参数        处理逻辑
            =========== ============================================================
            fontfile?   从文件创建字体，失败则抛出异常。
            fontbuffer? 从缓冲区创建字体，失败则抛出异常。
            ordering>=0 创建通用字体，总是成功。
            fontname?   创建 Base-14 字体、通用字体或
                        `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_ 提供的字体。请参见下表。
            =========== ============================================================

         .. note::

            使用常见的保留名称 "helv"、"tiro" 等，可以创建预期名称的字体，如 "Helvetica"、"Times-Roman" 等。**然而**，与 :meth:`Page.insert_font` 及类似方法不同：

            * 字体文件 **始终** 会被嵌入到 PDF 中，
            * 需要希腊字母和西里尔字母时，无需额外指定 *encoding* 参数。

            使用 *ordering >= 0*，或者指定字体名称为 "cjk"、"china-t"、"china-s"、"japan" 或 "korea"，**都会创建相同的“通用”字体——"Droid Sans Fallback Regular"**。该字体支持 **所有中、日、韩（CJK）字符及拉丁字符**，包括希腊字母和西里尔字母。此字体为无衬线体（sans-serif）。

            实际上，**"Droid Sans Fallback Regular"** 已经足够满足大部分无衬线字体需求。**唯一例外** 是该字体文件较大，压缩后约 1.65 MB，会显著增加 PDF 文件的大小。如果不需要 CJK 支持，可以选择 "helv"、"tiro" 等，这样压缩后仅需约 35 KB。

            如果你 **确定** 文本包含 CJK 和拉丁字符的混合内容，可以考虑直接使用 `Font("cjk")`，因为它不仅能支持所有字符，还能显著加快渲染速度（最多提高三倍）：MuPDF 只需在该字体中查找字符，而无需检查回退字体。

            但如果你选择其他字体，仍然可以自动写入 CJK 字符：MuPDF 会检测到这种情况，并静默回退到通用字体（该字体仍会被嵌入到 PDF 中）。

            *(v1.17.5 版本新增)* 如果安装了 `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_（可通过 `pip install pymupdf-fonts` 安装），则可以使用一些新的“保留”字体名称代码。**"Fira Mono"** 是一款等宽无衬线字体，而 **FiraGO** 是另一款“通用”无衬线字体，支持所有拉丁字符（包括西里尔字母和希腊字母）以及泰文、阿拉伯文、希伯来文和梵文（Devanagari）——但不支持 CJK 语言。相比 "Droid Sans Fallback"，FiraGO 字体的大小仅为其四分之一（压缩后 400 KB vs. 1.65 MB），**且** 额外提供了粗体、斜体、粗斜体等字重，而通用字体不提供这些特性。

            **"Space Mono"** 是另一款来自 Google Fonts 的小巧等宽字体，支持扩展拉丁字符集，并提供四种主要字重。

            下表列出了字体名称代码与对应字体的映射关系。有关当前可用字体的详细内容，请参阅官方文档：


               =========== =========================== ======= =============================
               代码        字体名称                    New in  备注   
               =========== =========================== ======= =============================
               figo        FiraGO Regular              v1.0.0  比 Helvetica 更窄
               figbo       FiraGO Bold                 v1.0.0
               figit       FiraGO Italic               v1.0.0
               figbi       FiraGO Bold Italic          v1.0.0
               fimo        Fira Mono Regular           v1.0.0
               fimbo       Fira Mono Bold              v1.0.0
               spacemo     Space Mono Regular          v1.0.1
               spacembo    Space Mono Bold             v1.0.1
               spacemit    Space Mono Italic           v1.0.1
               spacembi    Space Mono Bold-Italic      v1.0.1
               math        Noto Sans Math Regular      v1.0.2  数学符号
               music       Noto Music Regular          v1.0.2  音乐符号
               symbol1     Noto Sans Symbols Regular   v1.0.2  "symb" 字体的替代方案
               symbol2     Noto Sans Symbols2 Regular  v1.0.2  扩展符号集
               notos       Noto Sans Regular           v1.0.3  Helvetica 的替代方案
               notosit     Noto Sans Italic            v1.0.3
               notosbo     Noto Sans Bold              v1.0.3
               notosbi     Noto Sans BoldItalic        v1.0.3
               =========== =========================== ======= =============================

      .. index::
         pair: Font.has_glyph, language
         pair: Font.has_glyph, script
         pair: Font.has_glyph, fallback

      .. method:: has_glyph(chr, language=None, script=0, fallback=False)

         检查 Unicode 字符 *chr* 是否存在于该字体或（可选）某个回退字体中。可用于检测输出是否会出现 "TOFU" 符号。

         :arg int chr: 要检查的字符的 Unicode 码点（即 *ord()*）。
         :arg str language: 语言 —— 目前未使用。
         :arg int script: UCDN 脚本编号。
         :arg bool fallback: *(v1.17.5 新增)* 是否执行扩展搜索以包含回退字体，或仅限制在当前字体（默认）。
         :returns: *(v1.17.7 变更)* 返回字符的字形编号。返回 `0` 表示未找到相应字形。

      .. method:: valid_codepoints()

         * v1.17.5 新增

         返回当前字体支持的 Unicode 码点数组。

         :returns: 一个 *array.array* [#f4]_，长度最多为 :attr:`Font.glyph_count`。即：数组中每个元素的 *chr()* 都有对应的字形，并且未使用回退字体。这是一个示例，展示了支持的字形：

            >>> import pymupdf
            >>> font = pymupdf.Font("math")
            >>> vuc = font.valid_codepoints()
            >>> for i in vuc:
                  print("%04X %s (%s)" % (i, chr(i), font.unicode_to_glyph_name(i)))
            0000
            000D   (CR)
            0020   (space)
            0021 ! (exclam)
            0022 " (quotedbl)
            0023 # (numbersign)
            0024 $ (dollar)
            0025 % (percent)
            ...
            00AC ¬ (logicalnot)
            00B1 ± (plusminus)
            ...
            21D0 ⇐ (arrowdblleft)
            21D1 ⇑ (arrowdblup)
            21D2 ⇒ (arrowdblright)
            21D3 ⇓ (arrowdbldown)
            21D4 ⇔ (arrowdblboth)
            ...
            221E ∞ (infinity)
            ...

         .. note:: 
            
            该方法仅对具有 CMAP（字符映射表，charmap，对应 PDF 的 `/ToUnicode` 键）的字体返回有效数据。否则，该数组的长度将为 1，并且仅包含 `0`。

      .. index::
         pair: Font.glyph_advance, language
         pair: Font.glyph_advance, script
         pair: Font.glyph_advance, wmode

      .. method:: glyph_advance(chr, language=None, script=0, wmode=0)

         计算字符字形（视觉表示）的“宽度”。

         :arg int chr: 字符的 Unicode 码点。请使用 *ord()*，而不是直接输入字符。即使该字体不支持该字符，该方法通常仍可工作，因为必要时会检查回退字体。
         :arg int wmode: 书写模式，0 = 水平，1 = 垂直。

         其他参数目前未使用。

         :returns: 一个浮点数，表示相对于 **字号 1** 的字形宽度。

      .. method:: glyph_name_to_unicode(name)

         返回指定字形名称对应的 Unicode 值。如果希望输出特定符号，可与 `chr()` 结合使用。

         :arg str name: 字形名称。

         :returns: 对应的 Unicode 整数值，或者 `65533 = 0xFFFD` （如果名称未知）。示例： `font.glyph_name_to_unicode("Sigma") = 931` ， `font.glyph_name_to_unicode("sigma") = 963` 。可参考 `Adobe Glyph List <https://github.com/adobe-type-tools/agl-aglfn/blob/master/glyphlist.txt>`_ 获取字形名称及其对应的 Unicode 编号。示例：

            >>> font = pymupdf.Font("helv")
            >>> font.has_glyph(font.glyph_name_to_unicode("infinity"))
            True

      .. index::
         pair: Font.glyph_bbox, language
         pair: Font.glyph_bbox, script

      .. method:: glyph_bbox(chr, language=None, script=0)

         获取相对于 :data:`fontsize` 1 的字形矩形区域。

         :arg int chr: 字符的 *ord()* 值。

         :returns: 一个 :ref:`Rect` 对象。

      .. method:: unicode_to_glyph_name(ch)

         获取字符对应的字形名称。

         :arg int ch: 字符的 Unicode 码点。请使用 *ord()*，而不是直接输入字符。

         :returns: 一个字符串，表示字形名称。例如：`font.glyph_name(ord("#")) = "numbersign"`。对于无效码点，返回 ".notfound"。

            .. note:: *(v1.18.0 变更)* 该方法与 :meth:`Font.glyph_name_to_unicode` 不再依赖特定字体，而是改为从 **Adobe Glyph List** 获取信息。此外，该方法也可作为 `pymupdf.unicode_to_glyph_name()` 和 `pymupdf.glyph_name_to_unicode()` 调用。

      .. index::
         pair: text_length, fontsize

      .. method:: text_length(text, fontsize=11)

         计算 Unicode 字符串的长度（单位：点）。

         .. note:: 该方法在 Base-14 字体范围内与 :meth:`get_text_length` 功能重叠。

         :arg str text: 以 UTF-8 编码的文本字符串。
         :arg float fontsize: 指定的 :data:`fontsize`。

         :rtype: float

         :returns: 该字符串存储在 PDF 中的长度（单位：点）。如果某个字符不包含在当前字体中，将自动在回退字体中查找。

            .. note:: 该方法最初是基于 Python 实现的，并调用 :meth:`Font.glyph_advance` 进行计算。为了提高性能，从 v1.18.14 开始，该方法已改用 C 语言实现。要计算单个字符的宽度，现在可以使用以下任一方式，而不会影响性能：

               1. `font.glyph_advance(ord("Ä")) * fontsize`
               2. `font.text_length("Ä", fontsize=fontsize)`

               对于多字符字符串，该方法相比之前的实现具有显著的性能提升：每个字符的计算时间从大约 0.5 微秒减少到 12.5 纳秒（从第二个字符开始）。

      .. index::
         pair: char_lengths, fontsize

      .. method:: char_lengths(text, fontsize=11)

         *v1.18.14 新增*

         计算 Unicode 字符串中各个字符的长度（单位：点）。

         :arg str text: 以 UTF-8 编码的文本字符串。
         :arg float fontsize: 指定的 :data:`fontsize`。

         :rtype: tuple

         :returns: 该字符串在 PDF 中存储时，各字符的长度（单位：点）。此方法的作用类似于 :meth:`Font.text_length`，但结果分解到单个字符级别。该方法执行速度极快，例如在 :meth:`TextWriter.fill_textbox` 中被使用。以下等式成立（允许因四舍五入产生的误差）：`font.text_length(text) == sum(font.char_lengths(text))`。

            >>> font = pymupdf.Font("helv")
            >>> text = "PyMuPDF"
            >>> font.text_length(text)
            50.115999937057495
            >>> pymupdf.get_text_length(text, fontname="helv")
            50.115999937057495
            >>> sum(font.char_lengths(text))
            50.115999937057495
            >>> pprint(font.char_lengths(text))
            (7.336999952793121,  # P
            5.5,                 # y
            9.163000047206879,   # M
            6.115999937057495,   # u
            7.336999952793121,   # P
            7.942000031471252,   # D
            6.721000015735626)   # F

      .. attribute:: buffer

         * v1.17.6 新增 

         二进制字体文件内容的副本。
         
         :rtype: bytes

      .. attribute:: flags

         包含各种字体属性的字典，每个属性均表示为布尔值。例如 Helvetica 的示例::

            >>> pprint(font.flags)
            {'bold': 0,
            'fake-bold': 0,
            'fake-italic': 0,
            'invalid-bbox': 0,
            'italic': 0,
            'mono': 0,
            'opentype': 0,
            'serif': 1,
            'stretch': 0,
            'substitute': 0}

         :rtype: dict

      .. attribute:: name

         :rtype: str

         字体名称，可能为空字符串 `""` 或 `"(null)"`。

      .. attribute:: bbox

         字体的边界框（bounding box），其值是该字体所有字形边界框的最大范围。

         :rtype: :ref:`Rect`

      .. attribute:: glyph_count

         :rtype: int

         该字体定义的字形数量。

      .. attribute:: ascender

         * v1.18.0 新增 

         字体的上升高度（ascender），具体含义可参考 `此处 <https://en.wikipedia.org/wiki/Ascender_(typography)>`_。请注意，这里的定义与严格意义上的上升高度略有不同：我们的值包括基线（baseline）之上的所有部分，而不仅仅是大写 "A" 和小写 "a" 之间的高度差。

         :rtype: float

      .. attribute:: descender

         * v1.18.0 新增 

         字体的下降高度（descender），具体含义可参考 `此处 <https://en.wikipedia.org/wiki/Descender>`_。该值始终为负数，表示某些字形（如 "g" 或 "y"）超出基线的部分。因此，`ascender - descender` 代表该字体的整体高度——即所有字形都应适应的总高度。对于大多数字体而言，这一规则成立，但某些特殊字体（如书法字体）可能会有例外情况。

         :rtype: float


      .. attribute:: is_bold

      .. attribute:: is_italic

      .. attribute:: is_monospaced

      .. attribute:: is_serif

         一些含义明显的属性。反映了 :attr:`Font.flags` 字典中的一些值。

         :rtype: bool

.. tab:: 英文

   * New in v1.16.18

   This class represents a font as defined in MuPDF (*fz_font_s* structure). It is required for the new class :ref:`TextWriter` and the new :meth:`Page.write_text`. Currently, it has no connection to how fonts are used in methods :meth:`Page.insert_text` or :meth:`Page.insert_textbox`, respectively.

   A Font object also contains useful general information, like the font bbox, the number of defined glyphs, glyph names or the bbox of a single glyph.


   ==================================== ============================================
   **Method / Attribute**               **Short Description**
   ==================================== ============================================
   :meth:`~Font.glyph_advance`          Width of a character
   :meth:`~Font.glyph_bbox`             Glyph rectangle
   :meth:`~Font.glyph_name_to_unicode`  Get unicode from glyph name
   :meth:`~Font.has_glyph`              Return glyph id of unicode
   :meth:`~Font.text_length`            Compute string length
   :meth:`~Font.char_lengths`           Tuple of char widths of a string
   :meth:`~Font.unicode_to_glyph_name`  Get glyph name of a unicode
   :meth:`~Font.valid_codepoints`       Array of supported unicodes
   :attr:`~Font.ascender`               Font ascender
   :attr:`~Font.descender`              Font descender
   :attr:`~Font.bbox`                   Font rectangle
   :attr:`~Font.buffer`                 Copy of the font's binary image
   :attr:`~Font.flags`                  Collection of font properties
   :attr:`~Font.glyph_count`            Number of supported glyphs
   :attr:`~Font.name`                   Name of font
   :attr:`~Font.is_bold`                `True` if bold
   :attr:`~Font.is_monospaced`          `True` if mono-spaced
   :attr:`~Font.is_serif`               `True` if serif, `False` if sans-serif
   :attr:`~Font.is_italic`              `True` if italic
   ==================================== ============================================


   **Class API**

   .. class:: Font
      :no-index:

      .. method:: __init__(self, fontname=None, fontfile=None,
                     fontbuffer=None, script=0, language=None, ordering=-1, is_bold=0,
                     is_italic=0, is_serif=0)
         :no-index:

         Font constructor. The large number of parameters are used to locate font, which most closely resembles the requirements. Not all parameters are ever required -- see the below pseudo code explaining the logic how the parameters are evaluated.

         :arg str fontname: one of the :ref:`Base-14-Fonts` or CJK fontnames. Also possible are a select few other names like (watch the correct spelling): "Arial", "Times", "Times Roman".
         
            *(Changed in v1.17.5)*

            If you have installed `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_, there are also new "reserved" fontnames available, which are listed in :attr:`fitz_fonts` and in the table further down.

         :arg str fontfile: the filename of a fontfile somewhere on your system [#f1]_.
         :arg bytes,bytearray,io.BytesIO fontbuffer: a fontfile loaded in memory [#f1]_.
         :arg in script: the number of a UCDN script. Currently supported in PyMuPDF are numbers 24, and 32 through 35.
         :arg str language: one of the values "zh-Hant" (traditional Chinese), "zh-Hans" (simplified Chinese), "ja" (Japanese) and "ko" (Korean). Otherwise, all ISO 639 codes from the subsets 1, 2, 3 and 5 are also possible, but are currently documentary only.
         :arg int ordering: an alternative selector for one of the CJK fonts.
         :arg bool is_bold: look for a bold font.
         :arg bool is_italic: look for an italic font.
         :arg bool is_serif: look for a serifed font.

         :returns: a MuPDF font if successful. This is the overall sequence of checks to determine an appropriate font:

            =========== ============================================================
            Argument    Action
            =========== ============================================================
            fontfile?   Create font from file, exception if failure.
            fontbuffer? Create font from buffer, exception if failure.
            ordering>=0 Create universal font, always succeeds.
            fontname?   Create a Base-14 font, universal font, or font
                        provided by `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_. See table below.
            =========== ============================================================


         .. note::

         With the usual reserved names "helv", "tiro", etc., you will create fonts with the expected names "Helvetica", "Times-Roman" and so on. **However**, and in contrast to :meth:`Page.insert_font` and friends,

            * a font file will **always** be embedded in your PDF,
            * Greek and Cyrillic characters are supported without needing the *encoding* parameter.

         Using *ordering >= 0*, or fontnames "cjk", "china-t", "china-s", "japan" or "korea" will **always create the same "universal"** font **"Droid Sans Fallback Regular"**. This font supports **all Chinese, Japanese, Korean and Latin characters**, including Greek and Cyrillic. This is a sans-serif font.

         Actually, you would rarely ever need another sans-serif font than **"Droid Sans Fallback Regular"**. **Except** that this font file is relatively large and adds about 1.65 MB (compressed) to your PDF file size. If you do not need CJK support, stick with specifying "helv", "tiro" etc., and you will get away with about 35 KB compressed.

         If you **know** you have a mixture of CJK and Latin text, consider just using `Font("cjk")` because this supports everything and also significantly (by a factor of up to three) speeds up execution: MuPDF will always find any character in this single font and never needs to check fallbacks.

         But if you do use some other font, you will still automatically be able to also write CJK characters: MuPDF detects this situation and silently falls back to the universal font (which will then of course also be embedded in your PDF).

         *(New in v1.17.5)* Optionally, some new "reserved" fontname codes become available if you install `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_, `pip install pymupdf-fonts`. **"Fira Mono"** is a mono-spaced sans font set and **FiraGO** is another non-serifed "universal" font set which supports all Latin (including Cyrillic and Greek) plus Thai, Arabian, Hewbrew and Devanagari -- but none of the CJK languages. The size of a FiraGO font is only a quarter of the "Droid Sans Fallback" size (compressed 400 KB vs. 1.65 MB) -- **and** it provides the weights bold, italic, bold-italic -- which the universal font doesn't.

         **"Space Mono"** is another nice and small mono-spaced font from Google Fonts, which supports Latin Extended characters and comes with all 4 important weights.

         The following table maps a fontname code to the corresponding font. For the current content of the package please see its documentation:

               =========== =========================== ======= =============================
               Code        Fontname                    New in  Comment
               =========== =========================== ======= =============================
               figo        FiraGO Regular              v1.0.0  narrower than Helvetica
               figbo       FiraGO Bold                 v1.0.0
               figit       FiraGO Italic               v1.0.0
               figbi       FiraGO Bold Italic          v1.0.0
               fimo        Fira Mono Regular           v1.0.0
               fimbo       Fira Mono Bold              v1.0.0
               spacemo     Space Mono Regular          v1.0.1
               spacembo    Space Mono Bold             v1.0.1
               spacemit    Space Mono Italic           v1.0.1
               spacembi    Space Mono Bold-Italic      v1.0.1
               math        Noto Sans Math Regular      v1.0.2  math symbols
               music       Noto Music Regular          v1.0.2  musical symbols
               symbol1     Noto Sans Symbols Regular   v1.0.2  replacement for "symb"
               symbol2     Noto Sans Symbols2 Regular  v1.0.2  extended symbol set
               notos       Noto Sans Regular           v1.0.3  alternative to Helvetica
               notosit     Noto Sans Italic            v1.0.3
               notosbo     Noto Sans Bold              v1.0.3
               notosbi     Noto Sans BoldItalic        v1.0.3
               =========== =========================== ======= =============================

      .. method:: has_glyph(chr, language=None, script=0, fallback=False)
         :no-index:

         Check whether the unicode *chr* exists in the font or (option) some fallback font. May be used to check whether any "TOFU" symbols will appear on output.

         :arg int chr: the unicode of the character (i.e. *ord()*).
         :arg str language: the language -- currently unused.
         :arg int script: the UCDN script number.
         :arg bool fallback: *(new in v1.17.5)* perform an extended search in fallback fonts or restrict to current font (default).
         :returns: *(changed in 1.17.7)* the glyph number. Zero indicates no glyph found.

      .. method:: valid_codepoints()
         :no-index:

         * New in v1.17.5

         Return an array of unicodes supported by this font.

         :returns: an *array.array* [#f2]_ of length at most :attr:`Font.glyph_count`. I.e. *chr()* of every item in this array has a glyph in the font without using fallbacks. This is an example display of the supported glyphs:

            >>> import pymupdf
            >>> font = pymupdf.Font("math")
            >>> vuc = font.valid_codepoints()
            >>> for i in vuc:
                  print("%04X %s (%s)" % (i, chr(i), font.unicode_to_glyph_name(i)))
            0000
            000D   (CR)
            0020   (space)
            0021 ! (exclam)
            0022 " (quotedbl)
            0023 # (numbersign)
            0024 $ (dollar)
            0025 % (percent)
            ...
            00AC ¬ (logicalnot)
            00B1 ± (plusminus)
            ...
            21D0 ⇐ (arrowdblleft)
            21D1 ⇑ (arrowdblup)
            21D2 ⇒ (arrowdblright)
            21D3 ⇓ (arrowdbldown)
            21D4 ⇔ (arrowdblboth)
            ...
            221E ∞ (infinity)
            ...

         .. note:: This method only returns meaningful data for fonts having a CMAP (character map, charmap, the `/ToUnicode` PDF key). Otherwise, this array will have length 1 and contain zero only.

      .. method:: glyph_advance(chr, language=None, script=0, wmode=0)
         :no-index:

         Calculate the "width" of the character's glyph (visual representation).

         :arg int chr: the unicode number of the character. Use *ord()*, not the character itself. Again, this should normally work even if a character is not supported by that font, because fallback fonts will be checked where necessary.
         :arg int wmode: write mode, 0 = horizontal, 1 = vertical.

         The other parameters are not in use currently.

         :returns: a float representing the glyph's width relative to **fontsize 1**.

      .. method:: glyph_name_to_unicode(name)
         :no-index:

         Return the unicode value for a given glyph name. Use it in conjunction with `chr()` if you want to output e.g. a certain symbol.

         :arg str name: The name of the glyph.

         :returns: The unicode integer, or 65533 = 0xFFFD if the name is unknown. Examples: `font.glyph_name_to_unicode("Sigma") = 931`, `font.glyph_name_to_unicode("sigma") = 963`. Refer to the `Adobe Glyph List <https://github.com/adobe-type-tools/agl-aglfn/blob/master/glyphlist.txt>`_ publication for a list of glyph names and their unicode numbers. Example:

            >>> font = pymupdf.Font("helv")
            >>> font.has_glyph(font.glyph_name_to_unicode("infinity"))
            True

      .. method:: glyph_bbox(chr, language=None, script=0)
         :no-index:

         The glyph rectangle relative to :data:`fontsize` 1.

         :arg int chr: *ord()* of the character.

         :returns: a :ref:`Rect`.


      .. method:: unicode_to_glyph_name(ch)
         :no-index:

         Show the name of the character's glyph.

         :arg int ch: the unicode number of the character. Use *ord()*, not the character itself.

         :returns: a string representing the glyph's name. E.g. `font.glyph_name(ord("#")) = "numbersign"`. For an invalid code ".notfound" is returned.
         
         .. note:: *(Changed in v1.18.0)* This method and :meth:`Font.glyph_name_to_unicode` no longer depend on a font and instead retrieve information from the **Adobe Glyph List**. Also available as `pymupdf.unicode_to_glyph_name()` and resp. `pymupdf.glyph_name_to_unicode()`.

      .. method:: text_length(text, fontsize=11)
         :no-index:

         Calculate the length in points of a unicode string.

         .. note:: There is a functional overlap with :meth:`get_text_length` for Base-14 fonts only.

         :arg str text: a text string, UTF-8 encoded.

         :arg float fontsize: the :data:`fontsize`.

         :rtype: float

         :returns: the length of the string in points when stored in the PDF. If a character is not contained in the font, it will automatically be looked up in a fallback font.

            .. note:: This method was originally implemented in Python, based on calling :meth:`Font.glyph_advance`. For performance reasons, it has been rewritten in C for v1.18.14. To compute the width of a single character, you can now use either of the following without performance penalty:

               1. `font.glyph_advance(ord("Ä")) * fontsize`
               2. `font.text_length("Ä", fontsize=fontsize)`

               For multi-character strings, the method offers a huge performance advantage compared to the previous implementation: instead of about 0.5 microseconds for each character, only 12.5 nanoseconds are required for the second and subsequent ones.

      .. method:: char_lengths(text, fontsize=11)
         :no-index:

         *New in v1.18.14*

         Sequence of character lengths in points of a unicode string.

         :arg str text: a text string, UTF-8 encoded.

         :arg float fontsize: the :data:`fontsize`.

         :rtype: tuple

         :returns: the lengths in points of the characters of a string when stored in the PDF. It works like :meth:`Font.text_length` broken down to single characters. This is a high speed method, used e.g. in :meth:`TextWriter.fill_textbox`. The following is true (allowing rounding errors): `font.text_length(text) == sum(font.char_lengths(text))`.

            >>> font = pymupdf.Font("helv")
            >>> text = "PyMuPDF"
            >>> font.text_length(text)
            50.115999937057495
            >>> pymupdf.get_text_length(text, fontname="helv")
            50.115999937057495
            >>> sum(font.char_lengths(text))
            50.115999937057495
            >>> pprint(font.char_lengths(text))
            (7.336999952793121,  # P
            5.5,                 # y
            9.163000047206879,   # M
            6.115999937057495,   # u
            7.336999952793121,   # P
            7.942000031471252,   # D
            6.721000015735626)   # F


      .. attribute:: buffer
         :no-index:

         * New in v1.17.6

         Copy of the binary font file content.
         
         :rtype: bytes

      .. attribute:: flags
         :no-index:

         A dictionary with various font properties, each represented as bools. Example for Helvetica::

            >>> pprint(font.flags)
            {'bold': 0,
            'fake-bold': 0,
            'fake-italic': 0,
            'invalid-bbox': 0,
            'italic': 0,
            'mono': 0,
            'opentype': 0,
            'serif': 1,
            'stretch': 0,
            'substitute': 0}

         :rtype: dict

      .. attribute:: name
         :no-index:

         :rtype: str

         Name of the font. May be "" or "(null)".

      .. attribute:: bbox
         :no-index:

         The font bbox. This is the maximum of its glyph bboxes.

         :rtype: :ref:`Rect`

      .. attribute:: glyph_count
         :no-index:

         :rtype: int

         The number of glyphs defined in the font.

      .. attribute:: ascender
         :no-index:

         * New in v1.18.0

         The ascender value of the font, see `here <https://en.wikipedia.org/wiki/Ascender_(typography)>`_ for details. Please note that there is a difference to the strict definition: our value includes everything above the baseline -- not just the height difference between upper case "A" and and lower case "a".

         :rtype: float

      .. attribute:: descender
         :no-index:

         * New in v1.18.0

         The descender value of the font, see `here <https://en.wikipedia.org/wiki/Descender>`_ for details. This value always is negative and is the portion that some glyphs descend below the base line, for example "g" or "y". As a consequence, the value `ascender - descender` is the total height, that every glyph of the font fits into. This is true at least for most fonts -- as always, there are exceptions, especially for calligraphic fonts, etc.

         :rtype: float

      .. attribute:: is_bold
         :no-index:

      .. attribute:: is_italic
         :no-index:

      .. attribute:: is_monospaced
         :no-index:

      .. attribute:: is_serif
         :no-index:

         A number of attributes with obvious meanings. Reflect some values of the :attr:`Font.flags` dictionary.

         :rtype: bool

.. rubric:: Footnotes

.. [#f1] MuPDF does not support all fontfiles with this feature and will raise exceptions like *"mupdf: FT_New_Memory_Face((null)): unknown file format"*, if it encounters issues.

.. [#f2] The built-in Python module `array` has been chosen for its speed and low memory requirement.

.. [#f3] MuPDF 并不支持所有具有此功能的字体文件。如果遇到问题，它会抛出异常，例如 *"mupdf: FT_New_Memory_Face((null)): unknown file format"*。

.. [#f4] 选用 Python 内置模块 `array`，是因为其速度快且占用内存较少。

.. include:: footer.rst
