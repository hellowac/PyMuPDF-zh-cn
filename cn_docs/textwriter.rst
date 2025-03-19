.. include:: header.rst

.. _TextWriter:

================
TextWriter
================

|pdf_only_class|

.. tab:: 中文

   * 新功能 v1.16.18

   此类表示 MuPDF *文本* 对象。基本思想是 **解耦 (1) 文本准备和 (2) 文本输出** 到 PDF 页面。

   在 **准备阶段**，文本写入器存储任意数量的文本片段（“跨度”），并附带其位置和各自的字体信息。**输出** 可以多次发生，且每次输出到具有兼容页面大小的 PDF 页面。

   文本写入器是 :meth:`Page.insert_text` 等方法的优雅替代方案：

   * **改进的文本定位：** 选择插入文本的起始点。存储文本后，返回的是该跨度的 *最后一个字符* 后的位置（光标位置）。
   * **自由选择字体：** 每个文本跨度都有自己的字体和 :data:`fontsize`。这使得在组合大量文本时，轻松切换字体。
   * **自动回退字体：** 如果选定的字体不支持某个字符，将自动搜索替代字体。这显著降低了在输出中看到无法打印符号（“TOFU”——看起来像一个小矩形）的风险。PyMuPDF 现在还带有 **通用字体 “Droid Sans Fallback Regular”**，支持 **所有拉丁字符** （包括西里尔字母和希腊字母），以及 **所有 CJK 字符** （中文、日文、韩文）。
   * **西里尔字母和希腊字母支持：** :ref:`Base-14-fonts` 集成了对西里尔字母和希腊字母的支持 **无需指定编码**。您的文本可以是拉丁字母、希腊字母和西里尔字母的混合。
   * **透明度支持：** 参数 *opacity* 被支持。这为创建水印样式的文本提供了便捷方式。
   * **对齐文本：** 支持任何字体——不仅仅是 :meth:`Page.insert_textbox` 中的简单字体。
   * **可重用性：** TextWriter 对象独立于 PDF 页面存在。它可以多次写入，无论是到相同页面还是其他页面，选择不同的颜色或透明度。

   使用此对象涉及三个步骤：

   1. 在 **创建** 时，TextWriter 需要一个固定的 **页面矩形**，并基于该矩形计算文本位置。TextWriter 仅能写入该大小的页面。
   2. 使用 :meth:`TextWriter.append`、:meth:`TextWriter.appendv` 和 :meth:`TextWriter.fill_textbox` 方法存储文本，按需多次存储。
   3. 在某些 PDF 页面上输出 TextWriter 对象。

   .. note::

      * 从 v1.17.0 开始，TextWriters **支持** 通过 :meth:`TextWriter.write_text` 的 *morph* 参数旋转文本。

      * 还有 :meth:`Page.write_text` 方法，它将一个或多个 TextWriter 组合并将它们联合写入给定矩形和旋转角度——类似于 :meth:`Page.show_pdf_page`。

   ================================ ============================================
   **方法 / 属性**              **简短描述**
   ================================ ============================================
   :meth:`~TextWriter.append`      以水平写入模式添加文本
   :meth:`~TextWriter.appendv`     以垂直写入模式添加文本
   :meth:`~TextWriter.fill_textbox` 填充矩形（水平写入模式）
   :meth:`~TextWriter.write_text`  将 TextWriter 输出到 PDF 页面
   :attr:`~TextWriter.color`       文本颜色（可更改）
   :attr:`~TextWriter.last_point`  最后一个字符的结束位置
   :attr:`~TextWriter.opacity`     文本透明度（可更改）
   :attr:`~TextWriter.rect`        该 TextWriter 使用的页面矩形
   :attr:`~TextWriter.text_rect`   目前占用的区域
   ================================ ============================================


   **类 API**

   .. class:: TextWriter

      .. method:: __init__(self, rect, opacity=1, color=None)

         :arg rect-like rect: 内部用于文本定位计算的矩形。
         :arg float opacity: 设置文本透明度。值应在 `[0, 1)` 区间外的将被忽略。例如，0.5 代表 50% 的透明度。
         :arg float,sequ color: 文本的颜色。所有颜色都通过浮动值指定，范围 *0 <= color <= 1*。单个浮动值表示灰度级，序列表示色彩空间通过其长度。

      .. method:: append(pos, text, font=None, fontsize=11, language=None, right_to_left=False, small_caps=0)

         * *在 v1.18.9 中更改*
         * *在 v1.18.15 中更改*

         以水平写入模式添加新的文本。

         :arg point_like pos: 文本的起始位置，即第一个字符的左下角。
         :arg str text: 任意长度的字符串。文本将从位置 "pos" 开始写入。
         :arg font: 一个 :ref:`Font` 对象。如果省略，使用 `pymupdf.Font("helv")`。
         :arg float fontsize: :data:`fontsize`，正数，默认为 11。
         :arg str language: 使用的语言，例如 "en" 表示英文。有效值应符合 ISO 639 标准 1、2、3 或 5。预留待用：目前没有效果。
         :arg bool right_to_left: *(在 v1.18.9 中新增)* 是否按从右到左的顺序写入文本。适用于阿拉伯语或希伯来语等语言。默认为 *False*。若为 *True*，文本中的任何拉丁部分将自动转换。没有其他效果，即 :attr:`TextWriter.last_point` 仍然是最右边的字符，并且不会进行对齐。因此，您可能需要使用 :meth:`TextWriter.fill_textbox` 代替。
         :arg bool small_caps: *(在 v1.18.15 中新增)* 如果字体中有小型大写字母，则使用该字形。否则将使用原始字符（该字体或回退字体）。回退字体永远不会返回小型大写字母。例如，以下代码片段::

            >>> doc = pymupdf.open()
            >>> page = doc.new_page()
            >>> text = "PyMuPDF: the Python bindings for MuPDF"
            >>> font = pymupdf.Font("figo")  # 选择带小型大写字母的字体
            >>> tw = pymupdf.TextWriter(page.rect)
            >>> tw.append((50,100), text, font=font, small_caps=True)
            >>> tw.write_text(page)
            >>> doc.ez_save("x.pdf")

            将生成以下 PDF 文本：

            .. image:: images/img-smallcaps.*


         :returns: :attr:`text_rect` 和 :attr:`last_point`。*(在 v1.18.0 中更改:)* 如果字体不支持，将引发异常——通过 :attr:`Font.is_writable` 检查。

      .. method:: appendv(pos, text, font=None, fontsize=11, language=None, small_caps=0)

         *在 v1.18.15 中更改*

         以垂直、从上到下的写入模式添加新文本。

         :arg point_like pos: 文本的起始位置，即第一个字符的左下角。
         :arg str text: 一个字符串。它将从位置 "pos" 开始写入。
         :arg font: 一个 :ref:`Font` 对象。如果省略，使用 `pymupdf.Font("helv")`。
         :arg float fontsize: :data:`fontsize`，正浮动数，默认为 11。
         :arg str language: 使用的语言，例如 "en" 表示英文。有效值应符合 ISO 639 标准 1、2、3 或 5。预留待用：目前没有效果。
         :arg bool small_caps: *(在 v1.18.15 中新增)* 见 :meth:`append`。

         :returns: :attr:`text_rect` 和 :attr:`last_point`。*(在 v1.18.0 中更改:)* 如果字体不支持，将引发异常——通过 :attr:`Font.is_writable` 检查。

      .. method:: fill_textbox(rect, text, *, pos=None, font=None, fontsize=11, align=0, right_to_left=False, warn=None, small_caps=0)

         * 在 v1.17.3 中更改: 新增 `pos` 参数指定开始写入的矩形位置。
         * 在 v1.18.9 中更改: 返回不适合矩形的行的列表。支持右到左写入（如阿拉伯文、希伯来文）。
         * 在 v1.18.15 中更改: 如果字体支持，优先使用小型大写字母。

         以水平写入模式填充给定矩形中的文本。这是作为 :meth:`append` 的替代方法的便捷方法。

         :arg rect_like rect: 要填充的区域。文本不会超出此区域。
         :arg str,sequ text: 文本。可以是（UTF-8）字符串或字符串的列表/元组。字符串将首先通过 *splitlines()* 转换为列表。每个列表项将从新的一行开始（强制换行）。
         :arg point_like pos: *(v1.17.3 中新增)* 在此点开始存储。默认是在矩形的左上角附近。
         :arg font: :ref:`Font`，默认为 `pymupdf.Font("helv")`。
         :arg float fontsize: :data:`fontsize`。
         :arg int align: 文本对齐。使用 TEXT_ALIGN_LEFT、TEXT_ALIGN_CENTER、TEXT_ALIGN_RIGHT 或 TEXT_ALIGN_JUSTIFY。
         :arg bool right_to_left: *(在 v1.18.9 中新增)* 是否按从右到左写入文本。适用于阿拉伯文或希伯来文等语言。默认为 *False*。如果为 *True*，任何拉丁部分将自动反转。你仍然需要设置对齐方式（如果你想要右对齐），它不会自动发生——其他对齐选项仍然可用。
         :arg bool warn: 如果文本溢出，什么都不做、警告或引发异常。溢出的文本将不会写入。**在 v1.18.9 中更改：**

         * 默认值为 *None*。
         * 将返回溢出的行列表。

         :arg bool small_caps: *(在 v1.18.15 中新增)* 见 :meth:`append`。

         :rtype: list
         :returns: *在 v1.18.9 中新增*——未适配矩形的行的列表。每个项是一个元组 `(text, length)`，其中包含字符串及其长度（在页面上）。

      .. note:: 按需多次使用这些方法——没有技术限制（除非系统的内存限制）。你也可以混合使用 :meth:`append` 和文本框，且可以同时使用两者。文本定位完全由插入点控制。因此，没有必要遵循任何顺序。*(在 v1.18.0 中更改:)* 如果字体不支持，将引发异常——通过 :attr:`Font.is_writable` 检查。

      .. method:: write_text(page, opacity=None, color=None, morph=None, overlay=True, oc=0, render_mode=0)

         将 TextWriter 文本写入页面，页面是唯一必填参数。其他参数可用来临时覆盖创建 TextWriter 时使用的值。

         :arg page: 写入此 :ref:`Page`。
         :arg float opacity: 覆盖 TextWriter 的透明度值。
         :arg sequ color: 覆盖 TextWriter 的颜色值。
         :arg sequ morph: 通过应用矩阵修改文本外观。如果提供，它必须是一个序列 *(fixpoint, matrix)*，其中 *fixpoint* 为类似点的值，*matrix* 为类似矩阵的值。一个典型示例是围绕 *fixpoint* 旋转文本。
         :arg bool overlay: 放在前景（默认）还是背景。
         :arg int oc: *(在 v1.18.4 中新增)* :data:`xref` 的 :data:`OCG` 或 :data:`OCMD`。
         :arg int render_mode: PDF 的 `Tr` 操作符值。值：0（默认）、1、2、3（不可见）。

            .. image:: images/img-rendermode.*

      .. attribute:: text_rect

         当前占用的区域。

         :rtype: :ref:`Rect`

      .. attribute:: last_point

         “光标位置”——一个 :ref:`Point`，表示最后一个已写字符的底部右侧。

         :rtype: :ref:`Point`

      .. attribute:: opacity

         文本的透明度（可修改）。

         :rtype: float

      .. attribute:: color

         文本的颜色（可修改）。

         :rtype: float,tuple

      .. attribute:: rect

         此 TextWriter 创建的页面矩形。不得修改。

         :rtype: :ref:`Rect`

   .. note:: 要查看一些处理 TextWriter 的示例脚本，请查看 `this <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/textwriter>`_ 仓库。

   1. 透明度和颜色适用于 **此对象中的所有文本**。
   2. 如果需要不同的颜色/透明度，必须创建一个单独的 TextWriter。每当您确定颜色应更改时，只需使用先前返回的 :attr:`last_point` 作为新文本跨度的位置，将文本附加到相应的 TextWriter 中。
   3. 项目或文本框的附加可以按任意顺序进行：只有位置参数控制文本出现的地方。
   4. 字体和 :data:`fontsize` 在同一 TextWriter 中可以自由变化。这可以让不同属性的文本出现在同一行上：只需相应地指定 *pos*，例如将其设置为先前添加项的 :attr:`last_point`。
   5. 您可以使用 :meth:`TextWriter.fill_textbox` 的 *pos* 参数设置第一个文本字符的位置。这允许从不同的 :ref:`TextWriter` 对象填充相同的文本框，从而允许使用多种颜色、透明度等。
   6. MuPDF 不支持所有字体的此功能，例如不支持 Type3 字体。从 v1.18.0 开始，可以通过字体属性 :attr:`Font.is_writable` 检查此问题。当使用 :ref:`TextWriter` 方法时，该属性也会被检查。

.. tab:: 英文

   * New in v1.16.18

   This class represents a MuPDF *text* object. The basic idea is to **decouple (1) text preparation, and (2) text output** to PDF pages.

   During **preparation**, a text writer stores any number of text pieces ("spans") together with their positions and individual font information. The **output** of the writer's prepared content may happen multiple times to any PDF page with a compatible page size.

   A text writer is an elegant alternative to methods :meth:`Page.insert_text` and friends:

   * **Improved text positioning:** Choose any point where insertion of text should start. Storing text returns the "cursor position" after the *last character* of the span.
   * **Free font choice:** Each text span has its own font and :data:`fontsize`. This lets you easily switch when composing a larger text.
   * **Automatic fallback fonts:** If a character is not supported by the chosen font, alternative fonts are automatically searched. This significantly reduces the risk of seeing unprintable symbols in the output ("TOFUs" -- looking like a small rectangle). PyMuPDF now also comes with the **universal font "Droid Sans Fallback Regular"**, which supports **all Latin** characters (including Cyrillic and Greek), and **all CJK** characters (Chinese, Japanese, Korean).
   * **Cyrillic and Greek Support:** The :ref:`Base-14-fonts` have integrated support of Cyrillic and Greek characters **without specifying encoding.** Your text may be a mixture of Latin, Greek and Cyrillic.
   * **Transparency support:** Parameter *opacity* is supported. This offers a handy way to create watermark-style text.
   * **Justified text:** Supported for any font -- not just simple fonts as in :meth:`Page.insert_textbox`.
   * **Reusability:** A TextWriter object exists independent from PDF pages. It can be written multiple times, either to the same or to other pages, in the same or in different PDFs, choosing different colors or transparency.

   Using this object entails three steps:

   1. When **created**, a TextWriter requires a fixed **page rectangle** in relation to which it calculates text positions. A text writer can write to pages of this size only.
   2. Store text in the TextWriter using methods :meth:`TextWriter.append`, :meth:`TextWriter.appendv` and :meth:`TextWriter.fill_textbox` as often as is desired.
   3. Output the TextWriter object on some PDF page(s).

   .. note::

      * Starting with version 1.17.0, TextWriters **do support** text rotation via the *morph* parameter of :meth:`TextWriter.write_text`.

      * There also exists :meth:`Page.write_text` which combines one or more TextWriters and jointly writes them to a given rectangle and with a given rotation angle -- much like :meth:`Page.show_pdf_page`.


   ================================ ============================================
   **Method / Attribute**           **Short Description**
   ================================ ============================================
   :meth:`~TextWriter.append`       Add text in horizontal write mode
   :meth:`~TextWriter.appendv`      Add text in vertical write mode
   :meth:`~TextWriter.fill_textbox` Fill rectangle (horizontal write mode)
   :meth:`~TextWriter.write_text`   Output TextWriter to a PDF page
   :attr:`~TextWriter.color`        Text color (can be changed)
   :attr:`~TextWriter.last_point`   Last written character ends here
   :attr:`~TextWriter.opacity`      Text opacity (can be changed)
   :attr:`~TextWriter.rect`         Page rectangle used by this TextWriter
   :attr:`~TextWriter.text_rect`    Area occupied so far
   ================================ ============================================


   **Class API**

   .. class:: TextWriter
      :no-index:

      .. method:: __init__(self, rect, opacity=1, color=None)
         :no-index:

         :arg rect-like rect: rectangle internally used for text positioning computations.
         :arg float opacity: sets the transparency for the text to store here. Values outside the interval `[0, 1)` will be ignored. A value of e.g. 0.5 means 50% transparency.
         :arg float,sequ color: the color of the text. All colors are specified as floats *0 <= color <= 1*. A single float represents some gray level, a sequence implies the colorspace via its length.


      .. method:: append(pos, text, font=None, fontsize=11, language=None, right_to_left=False, small_caps=0)
         :no-index:

         * *Changed in v1.18.9*
         * *Changed in v1.18.15*

         Add some new text in horizontal writing.

         :arg point_like pos: start position of the text, the bottom left point of the first character.
         :arg str text: a string of arbitrary length. It will be written starting at position "pos".
         :arg font: a :ref:`Font`. If omitted, `pymupdf.Font("helv")` will be used.
         :arg float fontsize: the :data:`fontsize`, a positive number, default 11.
         :arg str language: the language to use, e.g. "en" for English. Meaningful values should be compliant with the ISO 639 standards 1, 2, 3 or 5. Reserved for future use: currently has no effect as far as we know.
         :arg bool right_to_left: *(New in v1.18.9)* whether the text should be written from right to left. Applicable for languages like Arabian or Hebrew. Default is *False*. If *True*, any Latin parts within the text will automatically converted. There are no other consequences, i.e. :attr:`TextWriter.last_point` will still be the rightmost character, and there neither is any alignment taking place. Hence you may want to use :meth:`TextWriter.fill_textbox` instead.
         :arg bool small_caps: *(New in v1.18.15)* look for the character's Small Capital version in the font. If present, take that value instead. Otherwise the original character (this font or the fallback font) will be taken. The fallback font will never return small caps. For example, this snippet::

            >>> doc = pymupdf.open()
            >>> page = doc.new_page()
            >>> text = "PyMuPDF: the Python bindings for MuPDF"
            >>> font = pymupdf.Font("figo")  # choose a font with small caps
            >>> tw = pymupdf.TextWriter(page.rect)
            >>> tw.append((50,100), text, font=font, small_caps=True)
            >>> tw.write_text(page)
            >>> doc.ez_save("x.pdf")

            will produce this PDF text:

            .. image:: images/img-smallcaps.*


         :returns: :attr:`text_rect` and :attr:`last_point`. *(Changed in v1.18.0:)* Raises an exception for an unsupported font -- checked via :attr:`Font.is_writable`.


      .. method:: appendv(pos, text, font=None, fontsize=11, language=None, small_caps=0)
         :no-index:

         *Changed in v1.18.15*

         Add some new text in vertical, top-to-bottom writing.

         :arg point_like pos: start position of the text, the bottom left point of the first character.
         :arg str text: a string. It will be written starting at position "pos".
         :arg font: a :ref:`Font`. If omitted, `pymupdf.Font("helv")` will be used.
         :arg float fontsize: the :data:`fontsize`, a positive float, default 11.
         :arg str language: the language to use, e.g. "en" for English. Meaningful values should be compliant with the ISO 639 standards 1, 2, 3 or 5. Reserved for future use: currently has no effect as far as we know.
         :arg bool small_caps: *(New in v1.18.15)* see :meth:`append`.

         :returns: :attr:`text_rect` and :attr:`last_point`. *(Changed in v1.18.0:)* Raises an exception for an unsupported font -- checked via :attr:`Font.is_writable`.

      .. method:: fill_textbox(rect, text, *, pos=None, font=None, fontsize=11, align=0, right_to_left=False, warn=None, small_caps=0)
         :no-index:

         * Changed in 1.17.3: New parameter `pos` to specify where to start writing within rectangle.
         * Changed in v1.18.9: Return list of lines which do not fit in rectangle. Support writing right-to-left (e.g. Arabian, Hebrew).
         * Changed in v1.18.15: Prefer small caps if supported by the font.

         Fill a given rectangle with text in horizontal writing mode. This is a convenience method to use as an alternative for :meth:`append`.

         :arg rect_like rect: the area to fill. No part of the text will appear outside of this.
         :arg str,sequ text: the text. Can be specified as a (UTF-8) string or a list / tuple of strings. A string will first be converted to a list using *splitlines()*. Every list item will begin on a new line (forced line breaks).
         :arg point_like pos: *(new in v1.17.3)* start storing at this point. Default is a point near rectangle top-left.
         :arg font: the :ref:`Font`, default `pymupdf.Font("helv")`.
         :arg float fontsize: the :data:`fontsize`.
         :arg int align: text alignment. Use one of TEXT_ALIGN_LEFT, TEXT_ALIGN_CENTER, TEXT_ALIGN_RIGHT or TEXT_ALIGN_JUSTIFY.
         :arg bool right_to_left: *(New in v1.18.9)* whether the text should be written from right to left. Applicable for languages like Arabian or Hebrew. Default is *False*. If *True*, any Latin parts are automatically reverted. You must still set the alignment (if you want right alignment), it does not happen automatically -- the other alignment options remain available as well.
         :arg bool warn: on text overflow do nothing, warn, or raise an exception. Overflow text will never be written. **Changed in v1.18.9:**

         * Default is *None*.
         * The list of overflow lines will be returned.

         :arg bool small_caps: *(New in v1.18.15)* see :meth:`append`.

         :rtype: list
         :returns: *New in v1.18.9* -- List of lines that did not fit in the rectangle. Each item is a tuple `(text, length)` containing a string and its length (on the page).

      .. note:: Use these methods as often as is required -- there is no technical limit (except memory constraints of your system). You can also mix :meth:`append` and text boxes and have multiple of both. Text positioning is exclusively controlled by the insertion point. Therefore there is no need to adhere to any order. *(Changed in v1.18.0:)* Raise an exception for an unsupported font -- checked via :attr:`Font.is_writable`.


      .. method:: write_text(page, opacity=None, color=None, morph=None, overlay=True, oc=0, render_mode=0)
         :no-index:

         Write the TextWriter text to a page, which is the only mandatory parameter. The other parameters can be used to temporarily override the values used when the TextWriter was created.

         :arg page: write to this :ref:`Page`.
         :arg float opacity: override the value of the TextWriter for this output.
         :arg sequ color: override the value of the TextWriter for this output.
         :arg sequ morph: modify the text appearance by applying a matrix to it. If provided, this must be a sequence *(fixpoint, matrix)* with a point-like *fixpoint* and a matrix-like *matrix*. A typical example is rotating the text around *fixpoint*.
         :arg bool overlay: put in foreground (default) or background.
         :arg int oc: *(new in v1.18.4)* the :data:`xref` of an :data:`OCG` or :data:`OCMD`.
         :arg int render_mode: The PDF `Tr` operator value. Values: 0 (default), 1, 2, 3 (invisible).

            .. image:: images/img-rendermode.*


      .. attribute:: text_rect
         :no-index:

         The area currently occupied.

         :rtype: :ref:`Rect`

      .. attribute:: last_point
         :no-index:

         The "cursor position" -- a :ref:`Point` -- after the last written character (its bottom-right).

         :rtype: :ref:`Point`

      .. attribute:: opacity
         :no-index:

         The text opacity (modifiable).

         :rtype: float

      .. attribute:: color
         :no-index:

         The text color (modifiable).

         :rtype: float,tuple

      .. attribute:: rect
         :no-index:

         The page rectangle for which this TextWriter was created. Must not be modified.

         :rtype: :ref:`Rect`


   .. note:: To see some demo scripts dealing with TextWriter, have a look at `this <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/textwriter>`_ repository.

   1. Opacity and color apply to **all the text** in this object.
   2. If you need different colors / transparency, you must create a separate TextWriter. Whenever you determine the color should change, simply append the text to the respective TextWriter using the previously returned :attr:`last_point` as position for the new text span.
   3. Appending items or text boxes can occur in arbitrary order: only the position parameter controls where text appears.
   4. Font and :data:`fontsize` can freely vary within the same TextWriter. This can be used to let text with different properties appear on the same displayed line: just specify *pos* accordingly, and e.g. set it to :attr:`last_point` of the previously added item.
   5. You can use the *pos* argument of :meth:`TextWriter.fill_textbox` to set the position of the first text character. This allows filling the same textbox with contents from different :ref:`TextWriter` objects, thus allowing for multiple colors, opacities, etc.
   6. MuPDF does not support all fonts with this feature, e.g. no Type3 fonts. Starting with v1.18.0 this can be checked via the font attribute :attr:`Font.is_writable`. This attribute is also checked when using :ref:`TextWriter` methods.

.. include:: footer.rst
