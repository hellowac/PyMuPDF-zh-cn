.. include:: header.rst

.. _Annot:

================
Annot
================

|pdf_only_class|

.. tab:: 中文

   引用自 :ref:`AdobeManual`：“注释将如笔记、声音或电影等对象与 PDF 文档中页面的某个位置关联，或通过鼠标和键盘提供与用户交互的方式。”

   注释与其页面之间存在父子关系。如果页面对象变得不可用（如关闭文档或任何文档结构更改等），则其所有现有的注释对象也会失效——每当访问注释属性或方法时，都会抛出一个异常，提示对象是“孤立的”。

   ================================== ==============================================================
   **属性**                              **简短描述**
   ================================== ==============================================================
   :meth:`Annot.delete_responses`          删除所有响应注释
   :meth:`Annot.get_file`                 获取附加文件内容
   :meth:`Annot.get_oc`                   获取 :data:`xref` 的 :data:`OCG` / :data:`OCMD`
   :meth:`Annot.get_pixmap`               作为位图返回注释图像
   :meth:`Annot.get_sound`                获取音频注释的声音
   :meth:`Annot.get_text`                 提取注释文本
   :meth:`Annot.get_textbox`              提取注释文本
   :meth:`Annot.set_border`               设置注释边框属性
   :meth:`Annot.set_blendmode`            设置注释的混合模式
   :meth:`Annot.set_colors`               设置注释颜色
   :meth:`Annot.set_flags`                设置注释标志字段
   :meth:`Annot.set_irt_xref`             定义注释为“响应于”
   :meth:`Annot.set_name`                 设置注释名称字段
   :meth:`Annot.set_oc`                   设置 :data:`xref` 到 :data:`OCG` / :data:`OCMD`
   :meth:`Annot.set_opacity`              改变透明度
   :meth:`Annot.set_open`                 打开/关闭注释或其弹出窗口
   :meth:`Annot.set_popup`                为注释创建弹出窗口
   :meth:`Annot.set_rect`                 更改注释矩形
   :meth:`Annot.set_rotation`             改变旋转
   :meth:`Annot.update_file`              更新附加文件内容
   :meth:`Annot.update`                   应用累积的注释更改
   :attr:`Annot.blendmode`                注释的混合模式
   :attr:`Annot.border`                   边框详细信息
   :attr:`Annot.colors`                   边框/背景和填充颜色
   :attr:`Annot.file_info`                获取附加文件信息
   :attr:`Annot.flags`                    注释标志
   :attr:`Annot.has_popup`                注释是否有弹出窗口
   :attr:`Annot.irt_xref`                 注释所响应的注释
   :attr:`Annot.info`                     各种信息
   :attr:`Annot.is_open`                  注释或其弹出窗口是否打开
   :attr:`Annot.line_ends`                线型注释的开始/结束外观
   :attr:`Annot.next`                     链接到下一个注释
   :attr:`Annot.opacity`                  注释的透明度
   :attr:`Annot.parent`                   注释的页面对象
   :attr:`Annot.popup_rect`               注释弹出窗口的矩形
   :attr:`Annot.popup_xref`               注释弹出窗口的 PDF :data:`xref` 编号
   :attr:`Annot.rect`                     包含注释的矩形
   :attr:`Annot.type`                     注释类型
   :attr:`Annot.vertices`                 多边形、折线等的点坐标
   :attr:`Annot.xref`                     注释的 PDF :data:`xref` 编号
   ================================== ==============================================================

.. tab:: 英文

   Quote from the :ref:`AdobeManual`: "An annotation associates an object such as a note, sound, or movie with a location on a page of a PDF document, or provides a way to interact with the user by means of the mouse and keyboard."

   There is a parent-child relationship between an annotation and its page. If the page object becomes unusable (closed document, any document structure change, etc.), then so does every of its existing annotation objects -- an exception is raised saying that the object is "orphaned", whenever an annotation property or method is accessed.

   ================================== ==============================================================
   **Attribute**                      **Short Description**
   ================================== ==============================================================
   :meth:`Annot.delete_responses`     delete all responding annotations
   :meth:`Annot.get_file`             get attached file content
   :meth:`Annot.get_oc`               get :data:`xref` of an :data:`OCG` / :data:`OCMD`
   :meth:`Annot.get_pixmap`           image of the annotation as a pixmap
   :meth:`Annot.get_sound`            get the sound of an audio annotation
   :meth:`Annot.get_text`             extract annotation text
   :meth:`Annot.get_textbox`          extract annotation text
   :meth:`Annot.set_border`           set annotation's border properties
   :meth:`Annot.set_blendmode`        set annotation's blend mode
   :meth:`Annot.set_colors`           set annotation's colors
   :meth:`Annot.set_flags`            set annotation's flags field
   :meth:`Annot.set_irt_xref`         define the annotation to being "In Response To"
   :meth:`Annot.set_name`             set annotation's name field
   :meth:`Annot.set_oc`               set :data:`xref` to an :data:`OCG` / :data:`OCMD`
   :meth:`Annot.set_opacity`          change transparency
   :meth:`Annot.set_open`             open / close annotation or its Popup
   :meth:`Annot.set_popup`            create a Popup for the annotation
   :meth:`Annot.set_rect`             change annotation rectangle
   :meth:`Annot.set_rotation`         change rotation
   :meth:`Annot.update_file`          update attached file content
   :meth:`Annot.update`               apply accumulated annot changes
   :attr:`Annot.blendmode`            annotation BlendMode
   :attr:`Annot.border`               border details
   :attr:`Annot.colors`               border / background and fill colors
   :attr:`Annot.file_info`            get attached file information
   :attr:`Annot.flags`                annotation flags
   :attr:`Annot.has_popup`            whether annotation has a Popup
   :attr:`Annot.irt_xref`             annotation to which this one responds
   :attr:`Annot.info`                 various information
   :attr:`Annot.is_open`              whether annotation or its Popup is open
   :attr:`Annot.line_ends`            start / end appearance of line-type annotations
   :attr:`Annot.next`                 link to the next annotation
   :attr:`Annot.opacity`              the annot's transparency
   :attr:`Annot.parent`               page object of the annotation
   :attr:`Annot.popup_rect`           rectangle of the annotation's Popup
   :attr:`Annot.popup_xref`           the PDF :data:`xref` number of the annotation's Popup
   :attr:`Annot.rect`                 rectangle containing the annotation
   :attr:`Annot.type`                 type of the annotation
   :attr:`Annot.vertices`             point coordinates of Polygons, PolyLines, etc.
   :attr:`Annot.xref`                 the PDF :data:`xref` number
   ================================== ==============================================================

**Class API**

.. tab:: 中文

   .. class:: Annot

      .. index::
         pair: matrix; Annot.get_pixmap
         pair: colorspace; Annot.get_pixmap
         pair: alpha; Annot.get_pixmap
         pair: dpi; Annot.get_pixmap

      .. method:: get_pixmap(matrix=pymupdf.Identity, dpi=None, colorspace=pymupdf.csRGB, alpha=False)

         * 变更于 v1.19.2: 添加了对 dpi 参数的支持。

         从注释创建一个像素图，按照页面上的未变换坐标显示。像素图的 :ref:`IRect` 等于 *Annot.rect.irect*（见下文）。**所有参数仅支持关键字传递。**

         :arg matrix_like matrix: 用于图像创建的矩阵。默认值为 :ref:`Identity`。

         :arg int dpi: （v1.19.2 新增） 期望的分辨率（每英寸点数）。如果不为 `None`，则忽略 matrix 参数。

         :arg colorspace: 用于图像创建的颜色空间。默认值为 *pymupdf.csRGB*。
         :type colorspace: :ref:`Colorspace`

         :arg bool alpha: 是否包含透明度信息。默认值为 *False*。

         :rtype: :ref:`Pixmap`

         .. note::
            
            * 如果注释刚刚被创建或修改，建议先使用 :meth:`Document.reload_page` 重新加载页面，例如：`page = doc.reload_page(page)`。

            * 当 `alpha=True` 时，生成的像素图将具有 *"预乘"* 像素。要了解更多相关背景信息，可参考 "Premultiplied alpha" 术语，例如查看 `此处 <https://en.wikipedia.org/wiki/Glossary_of_computer_graphics#P>`_。

      .. index::
         pair: blocks; Annot.get_text
         pair: dict; Annot.get_text
         pair: clip; Annot.get_text
         pair: flags; Annot.get_text
         pair: html; Annot.get_text
         pair: json; Annot.get_text
         pair: rawdict; Annot.get_text
         pair: text; Annot.get_text
         pair: words; Annot.get_text
         pair: xhtml; Annot.get_text
         pair: xml; Annot.get_text

      .. method:: get_text(opt, clip=None, flags=None)

         * 新增于 1.18.0

         以多种格式获取注释内容 —— 类似于 :ref:`Page` 的同名方法。目前仅对 'FreeText' 和 'Stamp' 类型的注释返回相关数据，其他类型返回空字符串（或等效对象）。

         :arg str opt: （仅限位置参数）所需的文本格式，必须是以下值之一。请注意，此方法的行为与 :ref:`Page` 的同名方法完全相同。

            * "text" -- :meth:`TextPage.extractTEXT`，默认选项
            * "blocks" -- :meth:`TextPage.extractBLOCKS`
            * "words" -- :meth:`TextPage.extractWORDS`
            * "html" -- :meth:`TextPage.extractHTML`
            * "xhtml" -- :meth:`TextPage.extractXHTML`
            * "xml" -- :meth:`TextPage.extractXML`
            * "dict" -- :meth:`TextPage.extractDICT`
            * "json" -- :meth:`TextPage.extractJSON`
            * "rawdict" -- :meth:`TextPage.extractRAWDICT`

         :arg rect-like clip: （仅限关键字参数）限制提取区域。通常无需使用，默认为 :attr:`Annot.rect`。
         :arg int flags: （仅限关键字参数）控制返回数据的详细程度。默认为简单文本提取。

      .. method:: get_textbox(rect)

         * 新增于 1.18.0

         返回注释的文本内容。与 :meth:`Annot.get_text` 的 "text" 选项类似（但不包含换行符）。

         :arg rect-like rect: 指定区域，默认为 :attr:`Annot.rect`。

      .. method:: set_info(info=None, content=None, title=None, creationDate=None, modDate=None, subject=None)

         * 变更于 1.16.10

         更改注释的属性，包括日期、内容、主题和作者（标题）。*name* 和 *id* 的变更将被忽略。更新是选择性的：要保持某个属性不变，请将其设置为 *None*；要删除现有数据，请使用空字符串。

         :arg dict info: 与 *info* 属性兼容的字典（见下文）。所有条目必须为字符串。如果此参数不是字典，则使用其他参数，否则它们将被忽略。
         :arg str content: *(v1.16.10 新增)* 参见 :attr:`info` 的描述。
         :arg str title: *(v1.16.10 新增)* 参见 :attr:`info` 的描述。
         :arg str creationDate: *(v1.16.10 新增)* 注释的创建日期。应使用 PDF 日期时间格式。
         :arg str modDate: *(v1.16.10 新增)* 最后修改日期。应使用 PDF 日期时间格式。
         :arg str subject: *(v1.16.10 新增)* 参见 :attr:`info` 的描述。

      .. method:: set_line_ends(start, end)

         设置注释的线端样式。这些注释类型由一系列点定义，这些点由线段连接。*start* 符号附加到第一点，*end* 符号附加到最后一点。如果注释类型不支持该功能，则不会执行任何操作，并会发出警告信息。

         .. note::

            * 虽然 'FreeText'、'Line'、'PolyLine' 和 'Polygon' 注释支持线端样式，但（Py-) MuPDF **不支持** 'FreeText' 的线端样式，因为它不支持该类型的标注。
            * *(变更于 v1.16.16)* 一些符号（如菱形、圆形、方形等）具有内部区域。默认情况下，这些区域填充为注释的填充颜色。如果该颜色为 *None*，则默认为白色。现在，可以使用 :meth:`Annot.update` 的 *fill_color* 参数覆盖默认颜色，为线端符号指定独立的填充颜色。

         :arg int start: 第一个点的符号编号。
         :arg int end: 最后一个点的符号编号。

      .. method:: set_oc(xref)

         使用 PDF 可选内容机制设置注释的可见性。可见性由支持的 PDF 查看器的用户界面控制，独立于 :attr:`Annot.flags` 等其他属性。

         :arg int xref: 可选内容组（OCG 或 OCMD）的 :data:`xref`。任何之前的 xref 都会被覆盖。如果设为零，则删除先前的条目。如果 xref 既不为零又不是有效的 PDF 对象，则会引发异常。

         .. note:: 这 **不需要执行** :meth:`Annot.update` 即可生效。

      .. method:: get_oc()

         返回可选内容对象的 :data:`xref`，如果没有则返回零。

         :returns: 零或 OCG（或 OCMD）的 xref。

      .. method:: set_irt_xref(xref)

         * 新增于 v1.19.3

         将注释设置为“响应于”另一个注释。

         :arg int xref: 另一个注释的 :data:`xref`。

            .. note:: 必须引用该页面上的现有注释。设置此属性不需要后续调用 `update()`。

      .. method:: set_open(value)

         * 新增于 v1.18.4

         设置注释的弹出注释（Popup）是否打开或关闭 —— **或者** 如果其类型为 'Text'（“便签”），则直接控制该注释的状态。

         :arg bool value: 期望的打开状态。

      .. method:: set_popup(rect)

         * 新增于 v1.18.4

         为该注释创建一个弹出注释（Popup）并指定其矩形区域。如果 Popup 已存在，则仅更新其矩形区域。

         :arg rect_like rect: 期望的矩形区域。

      .. method:: set_opacity(value)

         设置注释的透明度。透明度也可以在 :meth:`Annot.update` 方法中设置。

         :arg float value: 取值范围为 *[0, 1]* 的浮点数。超出此范围的值将被视为 1。例如，值 0.5 表示透明度为 50%。

         下面是三个重叠的 'Circle' 注释，每个的透明度均设置为 0.5：

         .. image:: images/img-opacity.*

      .. attribute:: blendmode

         * 新增于 v1.18.4

         注释的混合模式。详细说明请参阅 :ref:`AdobeManual` 第 324 页。

         :rtype: str
         :returns: 混合模式或 *None*。

      .. method:: set_blendmode(blendmode)

         * 新增于 v1.16.14
         
         设置注释的混合模式。详细说明请参阅 :ref:`AdobeManual` 第 324 页。混合模式也可以在 :meth:`Annot.update` 方法中设置。

         :arg str blendmode: 设定混合模式。使用 :meth:`Annot.update` 以在视觉外观上生效。可用的预定义值请参阅 :ref:`BlendModes`。使用 `PDF_BM_Normal` **移除** 混合模式。

      .. method:: set_name(name)

         * 新增于 v1.16.0
         
         更改任何类型注释的名称字段。对于 'FileAttachment' 和 'Text' 注释，该字段表示图标名称；对于 'Stamp' 注释，该字段表示印章上的文本。具体的视觉效果取决于 PDF 查看器。请参阅 :ref:`mupdficons`。

         :arg str name: 新的名称。

         .. caution:: 
            
            如果为 'Stamp' 注释设置名称，该操作 **不会更改** 其矩形区域，也不会自动调整文本布局。如果名称是 :ref:`StampIcons` 中的标准文本（即 `"STAMP_"` 之后的 **确切** 名称），那么应当保留原始布局。若使用 **任意文本**，则不会自动转换为大写，而是会以 "Times-Bold" 字体显示，**水平居中**，**单行显示**，并被截断以适应尺寸。要确保文本完整显示，其在字号 `fontsize=20` 下的长度不得超过 190 点。因此，请确保以下不等式成立：
            
            `pymupdf.get_text_length(text, fontname="tibo", fontsize=20) <= 190`

      .. method:: set_rect(rect)

         更改注释的矩形区域。可以移动注释，并且矩形的两侧可以独立缩放。但注释的外观不会被旋转、翻转或倾斜。此方法仅影响某些注释类型 [#f4]_，对于其他类型会在 Python 的 `sys.stderr` 产生消息，但不会引发异常，而是返回 `False`。

         :arg rect_like rect: 注释的新矩形区域（必须是有限且非空的）。例如，使用 *annot.rect + (5, 5, 5, 5)* 将注释位置向右和向下移动 5 个像素。

         .. note:: **无需** 调用 :meth:`Annot.update` 以使更改生效。

      .. method:: set_rotation(angle)

         设置注释的旋转角度。旋转操作围绕矩形的中心点进行，然后计算一个 **新的注释矩形** 来适应旋转后的形状。

         :arg int angle: 旋转角度（以度为单位）。可以使用任意值，但实际值会被限制在 `[0, 360)` 范围内。

         .. note::
            * **必须调用** :meth:`Annot.update` 以使更改生效。
            * 对于 `PDF_ANNOT_FREE_TEXT`，仅支持 0、90、180 和 270 度，这些值将 **旋转文本**，但矩形区域保持不变。其他值会被忽略并自动替换为 0。
            * 仅支持以下 :ref:`AnnotationTypes` 旋转：'Square'、'Circle'、'Caret'、'Text'、'FileAttachment'、'Ink'、'Line'、'Polyline'、'Polygon' 和 'Stamp'。其他类型的注释不会受影响。

      .. method:: set_border(border=None, width=None, style=None, dashes=None, clouds=None)

         * 版本 1.16.9 变更: 允许直接指定参数，而不使用字典。
         * 版本 1.22.5 变更: 支持“云状”边框效果。

         仅适用于 PDF：更改边框的宽度、虚线样式、边框风格和云状效果。详细信息请参阅 :attr:`Annot.border` 属性。

         :arg dict border: 一个字典，键包括 *"width"* (*float*)、*"style"* (*str*)、*"dashes"* (*sequence*) 和 *"clouds"* (*int*)。省略的键不会被更改。若 `border=None`（默认值），则使用其他参数。

         :arg float width: 非负值将更改边框线的宽度。
         :arg str style: 若提供非 `None` 值，则修改边框样式。
         :arg sequence dashes: 序列中的所有元素必须是整数，否则该参数会被忽略。要移除虚线样式，使用 `dashes=[]`。如果 `dashes` 是非空序列，则 `style` 会自动设为 `"D"` （虚线）。
         :arg int clouds: 若值大于等于 0，则更改该属性。使用 `clouds=0` 可完全移除云状效果。仅 'Square'、'Circle' 和 'Polygon' 类型的注释支持该属性。

      .. method:: set_flags(flags)

         更改注释的标志。可以使用 `|` 运算符组合多个标志。

         :arg int flags: 指定所需标志的整数值。

      .. method:: set_colors(colors=None, stroke=None, fill=None)

         * 版本 1.16.9 变更: 允许直接设置颜色，而不仅仅是字典方式。

         更改可用注释类型的“描边”和“填充”颜色（并非所有注释都接受这两种颜色）。

         :arg dict colors: 包含颜色信息的字典。最便捷的方法是先复制 *colors* 属性的内容，再进行修改。
         :arg sequence stroke: 见上文说明。
         :arg sequence fill: 见上文说明。

         * 版本 1.18.5 变更: 若要完全移除颜色配置，请使用空序列 `[]`。如果指定 `None`，则现有配置不会被更改。

      .. method:: delete_responses()

         * 新增于 v1.16.12

         删除所有引用当前注释的其他注释。这包括所有 'Popup'（弹出）注释以及所有对其作出响应的注释。

      .. index::
         pair: blend_mode; Annot.update
         pair: fontsize; Annot.update
         pair: text_color; Annot.update
         pair: border_color; Annot.update
         pair: fill_color; Annot.update
         pair: cross_out; Annot.update
         pair: rotate; Annot.update

      .. method:: update(opacity=None, blend_mode=None, fontsize=0, text_color=None, border_color=None, fill_color=None, cross_out=True, rotate=-1)

         在更改相关属性后，使注释的外观与其属性同步。

         仅在以下更改中可以 **安全地省略** 此方法：

            * :meth:`Annot.set_rect`
            * :meth:`Annot.set_flags`
            * :meth:`Annot.set_oc`
            * :meth:`Annot.update_file`
            * :meth:`Annot.set_info` （除非更改 *"content"*）

         所有参数都是可选的。 *(v1.16.14 变更)* 混合模式和透明度适用于 **所有注释类型**，其他参数主要用于特定用途，如下所述。

         颜色规范应采用 PuMuPDF 通常使用的格式，即 0.0 到 1.0 之间的浮点数序列（包含 0.0 和 1.0）。序列长度必须为 1、3 或 4（分别支持 GRAY、RGB 和 CMYK 颜色空间）。对于 GRAY 颜色，也可以只使用一个浮点数。

         :arg float opacity: *(v1.16.14 新增)* **适用于所有注释类型**：更改或设置注释的透明度。有效值范围为 *0 <= opacity < 1*。
         :arg str blend_mode: *(v1.16.14 新增)* **适用于所有注释类型**：更改或设置注释的混合模式。有效值请参阅 :ref:`BlendModes`。
         :arg float fontsize: 更改文本的 :data:`fontsize`，仅适用于 'FreeText' 注释。
         :arg sequence,float text_color: 更改文本颜色，仅适用于 'FreeText' 注释。
         :arg sequence,float border_color: 更改边框颜色，仅适用于 'FreeText' 注释。
         :arg sequence,float fill_color: 设置填充颜色。

               * 'Line'、'Polyline'、'Polygon' 注释：用于为适用的线端符号指定不同于注释的填充颜色 *(v1.16.16 变更)*。

         :arg bool cross_out: *(v1.17.2 新增)* 在注释矩形上添加两条对角线，仅适用于 'Redact' 注释。如果不需要，应显式指定 *False*，即使该注释创建时默认值为 *False*。
         :arg int rotate: 设置新的旋转角度。默认值 (-1) 表示不更改。支持 'FreeText' 和其他几种注释类型（参见 :meth:`Annot.set_rotation`） [#f3]_。对于 'FreeText'，仅可选 0、90、180 或 270 度，否则任何整数都是可接受的。

         :rtype: bool

         .. note:: **不建议** 在 :meth:`Page.annots` 循环中使用此方法！因为大多数注释更新都需要重新加载所属页面，而这无法在该循环内完成。请参考该生成器文档中的示例代码模式。

      .. attribute:: file_info

         附加文件的基本信息。

         :rtype: dict
         :returns: 一个包含以下键的字典： *filename* （文件名）、 *ufilename* （Unicode 文件名）、 *desc* （描述）、 *size* （未压缩文件大小）、 *length* （压缩长度）。仅适用于 'FileAttachment' 注释类型，否则返回 *None*。

      .. method:: get_file()

         返回附加文件的内容。

         :rtype: bytes
         :returns: 附加文件的二进制内容。

      .. index::
         pair: buffer; Annot.update_file
         pair: filename; Annot.update_file
         pair: ufilename; Annot.update_file
         pair: desc; Annot.update_file

      .. method:: update_file(buffer=None, filename=None, ufilename=None, desc=None)

         更新附加文件的内容。所有参数都是可选的。如果不提供任何参数，则此方法不会执行任何操作。

         :arg bytes|bytearray|BytesIO buffer: 新的文件内容。如果省略，则只修改元信息。

            *(v1.14.13 变更)* 现在支持 *io.BytesIO*。

         :arg str filename: 关联的文件名。
         :arg str ufilename: 关联的 Unicode 文件名。
         :arg str desc: 文件内容的新描述。

      .. method:: get_sound()

         返回音频注释中嵌入的声音。

         :rtype: dict
         :returns: 包含音频文件及其相关属性的字典。其中，只有 "rate" 和 "stream" 始终存在，其余键可能会缺失。

            =========== =======================================================
            键名        描述
            =========== =======================================================
            rate        (float, 必需) 采样率（每秒样本数）
            channels    (int, 可选) 声道数
            bps         (int, 可选) 每通道每个样本的比特数
            encoding    (str, 可选) 编码格式：Raw, Signed, muLaw, ALaw
            compression (str, 可选) 压缩过滤器名称
            stream      (bytes, 必需) 声音文件内容
            =========== =======================================================

      .. attribute:: opacity

         注释的透明度。如果设置了该值，它的范围为 *[0, 1]*。PDF 的默认值是 1。然而，为了区分未设置的情况，当透明度未定义时，返回 *-1.0*。

         :rtype: float

      .. attribute:: parent

         注释所属的页面对象。

         :rtype: :ref:`Page`

      .. attribute:: rotation

         注释的旋转角度。

         :rtype: int
         :returns: 范围在 [-1, 359] 之间的整数。如果没有旋转，则返回 -1（意味着旋转角度为 0）。其他可能的值将被归一化到 *0 <= angle < 360* 的范围内。

      .. attribute:: rect

         包含注释的矩形区域。

         :rtype: :ref:`Rect`

      .. attribute:: next

         该页面上的下一个注释，如果没有则返回 None。

         :rtype: *Annot*

      .. attribute:: type

         一个列表，包含一个数字和一个或两个字符串，用于描述注释类型，例如 **[2, 'FreeText', 'FreeTextCallout']**。第二个字符串项是可选的，可能为空。完整的可能值及其含义请参见附录 :ref:`AnnotationTypes`。

         :rtype: list

      .. attribute:: info

         一个包含各种信息的字典。所有字段都是可选的字符串。对于未提供的信息项，返回空字符串。

         * *name* —— 例如，对于 'Stamp' 注释，该字段将包含印章文本，如 "Sold" 或 "Experimental"；对于其他注释类型，此字段将显示注释图标的名称（例如 "PushPin" 适用于 FileAttachment）。
         * *content* —— 包含 *Text* 和 *FreeText* 类型注释的文本。通常用于填充注释弹出窗口的文本字段。
         * *title* —— 包含注释弹出窗口标题的字符串。按照惯例，该字段用于存储 **注释作者** 的信息。
         * *creationDate* —— 创建时间戳。
         * *modDate* —— 最后修改时间戳。
         * *subject* —— 主题。
         * *id* —— *(v1.16.10 新增)* 注释的唯一标识符。该值取自 PDF 关键字 */NM*。PyMuPDF 添加的注释都会有唯一的名称，该名称将显示在此字段中。

         :rtype: dict

      .. attribute:: flags

         一个整数，其低位包含注释的显示方式标志。

         :rtype: int

      .. attribute:: line_ends

         指定 'FreeText'、'Line'、'PolyLine' 和 'Polygon' 类型注释的起始和结束符号的整数对。如果不适用，则为 *None*。可能的值及其描述请参考 :ref:`AdobeManual`，第 400 页的表 1.76。

         :rtype: tuple

      .. attribute:: vertices

         一个列表，包含各种注释类型的多个点（“顶点”）坐标，每个点由一对浮点数表示：

         * 'Line' —— 起点和终点坐标（2 组浮点数）。
         * 'FreeText' —— 2 组或 3 组浮点数，分别表示起点、（可选）折点和终点坐标。
         * 'PolyLine' / 'Polygon' —— 由线段连接的各个边的坐标（n 组浮点数代表 n 个点）。
         * 文本标注注释 —— 4 组浮点数，指定标注文本范围的 *QuadPoints* （见 :ref:`AdobeManual`，第 403 页）。
         * 'Ink' —— 一个或多个子列表，每个子列表包含一系列顶点坐标，每个子列表代表绘图中的一条独立的线。

         :rtype: list

      .. attribute:: colors

         一个字典，包含两个浮点数列表（取值范围 *0 <= float <= 1*），分别指定“描边”颜色（stroke）和内部填充颜色（fill）。描边颜色用于边框及所有需要描绘或书写的部分。填充颜色用于对象内部，如线端、圆形和方形。列表长度决定所使用的颜色空间：1 = GRAY，3 = RGB，4 = CMYK。例如，"[1.0, 0.0, 0.0]" 代表 RGB 颜色红色。如果没有颜色指定，这两个列表可能为空。

         :rtype: dict

      .. attribute:: xref

         该注释的 PDF :data:`xref` 号。

         :rtype: int

      .. attribute:: irt_xref

         该注释所响应的另一个注释的 PDF :data:`xref` 号。如果该注释不是响应注释，则返回 0。

         :rtype: int

      .. attribute:: popup_xref

         关联的弹出式注释（Popup）的 PDF :data:`xref` 号。如果不存在，则返回 0。

         :rtype: int

      .. attribute:: has_popup

         该注释是否具有弹出式注释（Popup）。

         :rtype: bool

      .. attribute:: is_open

         该注释的弹出式窗口是否打开 —— **或** 该注释本身是否打开（仅适用于 'Text' 类型注释）。

         :rtype: bool

      .. attribute:: popup_rect

         关联弹出式注释（Popup）的矩形区域。如果不存在，则返回一个无限大矩形。

         :rtype: :ref:`Rect`

      .. attribute:: rect_delta

         一个包含四个浮点数的元组，表示注释的 `/RD` 条目。这四个数值描述了两个矩形的数值差异（左、上、-右、-下）：分别对应注释的 :attr:`rect` 和该矩形内部的另一个矩形。如果此条目缺失，则该属性为 `(0, 0, 0, 0)`。

         如果注释的边框是普通直线，则这些数值通常是边框宽度的一半。如果边框是“云状”（cloudy），那么这些数值会表示云形半圆的宽度。一般来说，这些数值可以不相等。要计算内部矩形，可以使用 `a.rect + a.rect_delta`。

      .. attribute:: border

         一个包含边框特性的字典。如果没有边框信息，则为空字典。可能包含以下键：

         * *width* —— 一个浮点数，表示边框厚度（单位：点）。如果未指定宽度，则值为 -1.0。

         * *dashes* —— 一个整数序列，指定线条的虚线模式。`[]` 表示实线（无虚线），`[n]` 表示等长的虚实交替模式（每段长度为 *n* 点），更长的列表表示交替的虚实长度值。详细信息请参考 :ref:`AdobeManual` 第 126 页。

         * *style* —— 一个 1 字节的字符串，表示边框样式：
         
         - **"S"** (Solid) —— 实线边框，围绕注释。
         - **"D"** (Dashed) —— 虚线边框，围绕注释，虚线模式由 *dashes* 指定。
         - **"B"** (Beveled) —— 模拟的浮雕矩形，使边框看起来高于页面表面。
         - **"I"** (Inset) —— 模拟的雕刻矩形，使边框看起来低于页面表面。
         - **"U"** (Underline) —— 仅在注释矩形的底部绘制一条实线。

         * *clouds* —— 一个整数，表示“云状”边框的类型，范围 `-1 <= n <= 2`。 `n = 0` 表示直线（无云状效果），1 表示小型云状半圆，2 表示大型云状半圆。如果值为 -1，则未指定云状边框。

         :rtype: dict


.. tab:: 英文

   .. class:: Annot
      :no-index:

      .. method:: get_pixmap(matrix=pymupdf.Identity, dpi=None, colorspace=pymupdf.csRGB, alpha=False)
         :no-index:

         * Changed in v1.19.2: added support of dpi parameter.

         Creates a pixmap from the annotation as it appears on the page in untransformed coordinates. The pixmap's :ref:`IRect` equals *Annot.rect.irect* (see below). **All parameters are keyword only.**

         :arg matrix_like matrix: a matrix to be used for image creation. Default is :ref:`Identity`.

         :arg int dpi: (new in v1.19.2) desired resolution in dots per inch. If not `None`, the matrix parameter is ignored.

         :arg colorspace: a colorspace to be used for image creation. Default is *pymupdf.csRGB*.
         :type colorspace: :ref:`Colorspace`

         :arg bool alpha: whether to include transparency information. Default is *False*.

         :rtype: :ref:`Pixmap`

         .. note::
            
            * If the annotation has just been created or modified, you should :meth:`Document.reload_page` the page first via `page = doc.reload_page(page)`.

            * The pixmap will have *"premultiplied"* pixels if `alpha=True`. To learn about some background, e.g. look for "Premultiplied alpha" `here <https://en.wikipedia.org/wiki/Glossary_of_computer_graphics#P>`_.

      .. method:: get_text(opt, clip=None, flags=None)
         :no-index:

         * New in 1.18.0

         Retrieves the content of the annotation in a variety of formats -- much like the same method for :ref:`Page`.. This currently only delivers relevant data for annotation types 'FreeText' and 'Stamp'. Other types return an empty string (or equivalent objects).

         :arg str opt: (positional only) the desired format - one of the following values. Please note that this method works exactly like the same-named method of :ref:`Page`.

            * "text" -- :meth:`TextPage.extractTEXT`, default
            * "blocks" -- :meth:`TextPage.extractBLOCKS`
            * "words" -- :meth:`TextPage.extractWORDS`
            * "html" -- :meth:`TextPage.extractHTML`
            * "xhtml" -- :meth:`TextPage.extractXHTML`
            * "xml" -- :meth:`TextPage.extractXML`
            * "dict" -- :meth:`TextPage.extractDICT`
            * "json" -- :meth:`TextPage.extractJSON`
            * "rawdict" -- :meth:`TextPage.extractRAWDICT`

         :arg rect-like clip: (keyword only) restrict the extraction to this area. Should hardly ever be required, defaults to :attr:`Annot.rect`.
         :arg int flags: (keyword only) control the amount of data returned. Defaults to simple text extraction.

      .. method:: get_textbox(rect)
         :no-index:

         * New in 1.18.0

         Return the annotation text. Mostly (except line breaks) equal to :meth:`Annot.get_text` with the "text" option.

         :arg rect-like rect: the area to consider, defaults to :attr:`Annot.rect`.


      .. method:: set_info(info=None, content=None, title=None, creationDate=None, modDate=None, subject=None)
         :no-index:

         * Changed in version 1.16.10

         Changes annotation properties. These include dates, contents, subject and author (title). Changes for *name* and *id* will be ignored. The update happens selectively: To leave a property unchanged, set it to *None*. To delete existing data, use an empty string.

         :arg dict info: a dictionary compatible with the *info* property (see below). All entries must be strings. If this argument is not a dictionary, the other arguments are used instead -- else they are ignored.
         :arg str content: *(new in v1.16.10)* see description in :attr:`info`.
         :arg str title: *(new in v1.16.10)* see description in :attr:`info`.
         :arg str creationDate: *(new in v1.16.10)* date of annot creation. If given, should be in PDF datetime format.
         :arg str modDate: *(new in v1.16.10)* date of last modification. If given, should be in PDF datetime format.
         :arg str subject: *(new in v1.16.10)* see description in :attr:`info`.

      .. method:: set_line_ends(start, end)
         :no-index:

         Sets an annotation's line ending styles. Each of these annotation types is defined by a list of points which are connected by lines. The symbol identified by *start* is attached to the first point, and *end* to the last point of this list. For unsupported annotation types, a no-operation with a warning message results.

         .. note::

            * While 'FreeText', 'Line', 'PolyLine', and 'Polygon' annotations can have these properties, (Py-) MuPDF does not support line ends for 'FreeText', because the call-out variant of it is not supported.
            * *(Changed in v1.16.16)* Some symbols have an interior area (diamonds, circles, squares, etc.). By default, these areas are filled with the fill color of the annotation. If this is *None*, then white is chosen. The *fill_color* argument of :meth:`Annot.update` can now be used to override this and give line end symbols their own fill color.

         :arg int start: The symbol number for the first point.
         :arg int end: The symbol number for the last point.

      .. method:: set_oc(xref)
         :no-index:

         Set the annotation's visibility using PDF optional content mechanisms. This visibility is controlled by the user interface of supporting PDF viewers. It is independent from other attributes like :attr:`Annot.flags`.

         :arg int xref: the :data:`xref` of an optional contents group (OCG or OCMD). Any previous xref will be overwritten. If zero, a previous entry will be removed. An exception occurs if the xref is not zero and does not point to a valid PDF object.

         .. note:: This does **not require executing** :meth:`Annot.update` to take effect.

      .. method:: get_oc()
         :no-index:

         Return the :data:`xref` of an optional content object, or zero if there is none.

         :returns: zero or the xref of an OCG (or OCMD).


      .. method:: set_irt_xref(xref)
         :no-index:

         * New in v1.19.3

         Set annotation to be "In Response To" another one.

         :arg int xref: The :data:`xref` of another annotation.

            .. note:: Must refer to an existing annotation on this page. Setting this property requires no subsequent `update()`.


      .. method:: set_open(value)
         :no-index:

         * New in v1.18.4

         Set the annotation's Popup annotation to open or closed -- **or** the annotation itself, if its type is 'Text' ("sticky note").

         :arg bool value: the desired open state.


      .. method:: set_popup(rect)
         :no-index:

         * New in v1.18.4

         Create a Popup annotation for the annotation and specify its rectangle. If the Popup already exists, only its rectangle is updated.

         :arg rect_like rect: the desired rectangle.



      .. method:: set_opacity(value)
         :no-index:

         Set the annotation's transparency. Opacity can also be set in :meth:`Annot.update`.

         :arg float value: a float in range *[0, 1]*. Any value outside is assumed to be 1. E.g. a value of 0.5 sets the transparency to 50%.

         Three overlapping 'Circle' annotations with each opacity set to 0.5:

         .. image:: images/img-opacity.*

      .. attribute:: blendmode
         :no-index:

         * New in v1.18.4

         The annotation's blend mode. See :ref:`AdobeManual`, page 324 for explanations.

         :rtype: str
         :returns: the blend mode or *None*.


      .. method:: set_blendmode(blendmode)
         :no-index:

         * New in v1.16.14
         
         Set the annotation's blend mode. See :ref:`AdobeManual`, page 324 for explanations. The blend mode can also be set in :meth:`Annot.update`.

         :arg str blendmode: set the blend mode. Use :meth:`Annot.update` to reflect this in the visual appearance. For predefined values see :ref:`BlendModes`. Use `PDF_BM_Normal` to **remove** a blend mode.


      .. method:: set_name(name)
         :no-index:

         * New in version 1.16.0
         
         Change the name field of any annotation type. For 'FileAttachment' and 'Text' annotations, this is the icon name, for 'Stamp' annotations the text in the stamp. The visual result (if any) depends on your PDF viewer. See also :ref:`mupdficons`.

         :arg str name: the new name.

         .. caution:: If you set the name of a 'Stamp' annotation, then this will **not change** the rectangle, nor will the text be layouted in any way. If you choose a standard text from :ref:`StampIcons` (the **exact** name piece after `"STAMP_"`), you should receive the original layout. An **arbitrary text** will not be changed to upper case, but be written in font "Times-Bold" as is, horizontally centered in **one line** and be shortened to fit. To get your text fully displayed, its length using :data:`fontsize` 20 must not exceed 190 points. So please make sure that the following inequality is true: `pymupdf.get_text_length(text, fontname="tibo", fontsize=20) <= 190`.

      .. method:: set_rect(rect)
         :no-index:

         Change the rectangle of an annotation. The annotation can be moved around and both sides of the rectangle can be independently scaled. However, the annotation appearance will never get rotated, flipped or sheared. This method only affects certain annotation types [#f2]_ and will lead to a message on Python's `sys.stderr` in other cases. No exception will be raised, but `False` will be returned.

         :arg rect_like rect: the new rectangle of the annotation (finite and not empty). E.g. using a value of *annot.rect + (5, 5, 5, 5)* will shift the annot position 5 pixels to the right and downwards.

         .. note:: You **need not** invoke :meth:`Annot.update` for activation of the effect.


      .. method:: set_rotation(angle)
         :no-index:

         Set the rotation of an annotation. This rotates the annotation rectangle around its center point. Then a **new annotation rectangle** is calculated from the resulting quad.

         :arg int angle: rotation angle in degrees. Arbitrary values are possible, but will be clamped to the interval `[0, 360)`.

         .. note::
         * You **must invoke** :meth:`Annot.update` to activate the effect.
         * For PDF_ANNOT_FREE_TEXT, only one of the values 0, 90, 180 and 270 is possible and will **rotate the text** inside the current rectangle (which remains unchanged). Other values are silently ignored and replaced by 0.
         * Otherwise, only the following :ref:`AnnotationTypes` can be rotated: 'Square', 'Circle', 'Caret', 'Text', 'FileAttachment', 'Ink', 'Line', 'Polyline', 'Polygon', and 'Stamp'. For all others the method is a no-op.


      .. method:: set_border(border=None, width=None, style=None, dashes=None, clouds=None)
         :no-index:

         * Changed in version 1.16.9: Allow specification without using a dictionary. The direct parameters are used if *border* is not a dictionary.

         * Changed in version 1.22.5: Support of the "cloudy" border effect.

         PDF only: Change border width, dashing, style and cloud effect. See the :attr:`Annot.border` attribute for more details.


         :arg dict border: a dictionary as returned by the :attr:`border` property, with keys *"width"* (*float*), *"style"* (*str*),  *"dashes"* (*sequence*) and *clouds* (*int*). Omitted keys will leave the resp. property unchanged. Set the border argument to `None` (the default) to use the other arguments.

         :arg float width: A non-negative value will change the border line width.
         :arg str style: A value other than `None` will change this border property.
         :arg sequence dashes: All items of the sequence must be integers, otherwise the parameter is ignored. To remove dashing use: `dashes=[]`. If dashes is a non-empty sequence, "style" will automatically be set to "D" (dashed). 
         :arg int clouds: A value >= 0 will change this property. Use `clouds=0` to remove the cloudy appearance completely. Only annotation types 'Square', 'Circle', and 'Polygon' are supported with this property.

      .. method:: set_flags(flags)
         :no-index:

         Changes the annotation flags. Use the `|` operator to combine several.

         :arg int flags: an integer specifying the required flags.

      .. method:: set_colors(colors=None, stroke=None, fill=None)
         :no-index:

         * Changed in version 1.16.9: Allow colors to be directly set. These parameters are used if *colors* is not a dictionary.

         Changes the "stroke" and "fill" colors for supported annotation types -- not all annotations accept both.

         :arg dict colors: a dictionary containing color specifications. For accepted dictionary keys and values see below. The most practical way should be to first make a copy of the *colors* property and then modify this dictionary as required.
         :arg sequence stroke: see above.
         :arg sequence fill: see above.

         *Changed in v1.18.5:* To completely remove a color specification, use an empty sequence like `[]`. If you specify `None`, an existing specification will not be changed.


      .. method:: delete_responses()
         :no-index:

         * New in version 1.16.12
         
         Delete annotations referring to this one. This includes any 'Popup' annotations and all annotations responding to it.

      .. method:: update(opacity=None, blend_mode=None, fontsize=0, text_color=None, border_color=None, fill_color=None, cross_out=True, rotate=-1)
         :no-index:

         Synchronize the appearance of an annotation with its properties after relevant changes. 

         You can safely **omit** this method **only** for the following changes:

            * :meth:`Annot.set_rect`
            * :meth:`Annot.set_flags`
            * :meth:`Annot.set_oc`
            * :meth:`Annot.update_file`
            * :meth:`Annot.set_info` (except any changes to *"content"*)

         All arguments are optional. *(Changed in v1.16.14)* Blend mode and opacity are applicable to **all annotation types**. The other arguments are mostly special use, as described below.

         Color specifications may be made in the usual format used in PuMuPDF as sequences of floats ranging from 0.0 to 1.0 (including both). The sequence length must be 1, 3 or 4 (supporting GRAY, RGB and CMYK colorspaces respectively). For GRAY, just a float is also acceptable.

         :arg float opacity: *(new in v1.16.14)* **valid for all annotation types:** change or set the annotation's transparency. Valid values are *0 <= opacity < 1*.
         :arg str blend_mode: *(new in v1.16.14)* **valid for all annotation types:** change or set the annotation's blend mode. For valid values see :ref:`BlendModes`.
         :arg float fontsize: change :data:`fontsize` of the text. 'FreeText' annotations only.
         :arg sequence,float text_color: change the text color. 'FreeText' annotations only.
         :arg sequence,float border_color: change the border color. 'FreeText' annotations only.
         :arg sequence,float fill_color: the fill color.

            * 'Line', 'Polyline', 'Polygon' annotations: use it to give applicable line end symbols a fill color other than that of the annotation *(changed in v1.16.16)*.

         :arg bool cross_out: *(new in v1.17.2)* add two diagonal lines to the annotation rectangle. 'Redact' annotations only. If not desired, *False* must be specified even if the annotation was created with *False*.
         :arg int rotate: new rotation value. Default (-1) means no change. Supports 'FreeText' and several other annotation types (see :meth:`Annot.set_rotation`), [#f1]_. Only choose 0, 90, 180, or 270 degrees for 'FreeText'. Otherwise any integer is acceptable.

         :rtype: bool

         .. note:: Using this method inside a :meth:`Page.annots` loop is **not recommended!** This is because most annotation updates require the owning page to be reloaded -- which cannot be done inside this loop. Please use the example coding pattern given in the documentation of this generator.


      .. attribute:: file_info
         :no-index:

         Basic information of the annot's attached file.

         :rtype: dict
         :returns: a dictionary with keys *filename*, *ufilename*, *desc* (description), *size* (uncompressed file size), *length* (compressed length) for FileAttachment annot types, else *None*.

      .. method:: get_file()
         :no-index:

         Returns attached file content.

         :rtype: bytes
         :returns: the content of the attached file.

      .. method:: update_file(buffer=None, filename=None, ufilename=None, desc=None)
         :no-index:

         Updates the content of an attached file. All arguments are optional. No arguments lead to a no-op.

         :arg bytes|bytearray|BytesIO buffer: the new file content. Omit to only change meta-information.

            *(Changed in version 1.14.13)* *io.BytesIO* is now also supported.

         :arg str filename: new filename to associate with the file.

         :arg str ufilename: new unicode filename to associate with the file.

         :arg str desc: new description of the file content.

      .. method:: get_sound()
         :no-index:

         Return the embedded sound of an audio annotation.

         :rtype: dict
         :returns: the sound audio file and accompanying properties. These are the possible dictionary keys, of which only "rate" and "stream" are always present.

         =========== =======================================================
         Key         Description
         =========== =======================================================
         rate        (float, requ.) samples per second
         channels    (int, opt.) number of sound channels
         bps         (int, opt.) bits per sample value per channel
         encoding    (str, opt.) encoding format: Raw, Signed, muLaw, ALaw
         compression (str, opt.) name of compression filter
         stream      (bytes, requ.) the sound file content
         =========== =======================================================


      .. attribute:: opacity
         :no-index:

         The annotation's transparency. If set, it is a value in range *[0, 1]*. The PDF default is 1. However, in an effort to tell the difference, we return *-1.0* if not set.

         :rtype: float

      .. attribute:: parent
         :no-index:

         The owning page object of the annotation.

         :rtype: :ref:`Page`

      .. attribute:: rotation
         :no-index:

         The annot rotation.

         :rtype: int
         :returns: a value [-1, 359]. If rotation is not at all, -1 is returned (and implies a rotation angle of 0). Other possible values are normalized to some value value 0 <= angle < 360.

      .. attribute:: rect
         :no-index:

         The rectangle containing the annotation.

         :rtype: :ref:`Rect`

      .. attribute:: next
         :no-index:

         The next annotation on this page or None.

         :rtype: *Annot*

      .. attribute:: type
         :no-index:

         A number and one or two strings describing the annotation type, like **[2, 'FreeText', 'FreeTextCallout']**. The second string entry is optional and may be empty. See the appendix :ref:`AnnotationTypes` for a list of possible values and their meanings.

         :rtype: list

      .. attribute:: info
         :no-index:

         A dictionary containing various information. All fields are optional strings. For information items not provided, an empty string is returned.

         * *name* -- e.g. for 'Stamp' annotations it will contain the stamp text like "Sold" or "Experimental", for other annot types you will see the name of the annot's icon here ("PushPin" for FileAttachment).

         * *content* -- a string containing the text for type *Text* and *FreeText* annotations. Commonly used for filling the text field of annotation pop-up windows.

         * *title* -- a string containing the title of the annotation pop-up window. By convention, this is used for the **annotation author**.

         * *creationDate* -- creation timestamp.
         * *modDate* -- last modified timestamp.
         * *subject* -- subject.
         * *id* -- *(new in version 1.16.10)* a unique identification of the annotation. This is taken from PDF key */NM*. Annotations added by PyMuPDF will have a unique name, which appears here.

         :rtype: dict


      .. attribute:: flags
         :no-index:

         An integer whose low order bits contain flags for how the annotation should be presented.

         :rtype: int

      .. attribute:: line_ends
         :no-index:

         A pair of integers specifying start and end symbol of annotations types 'FreeText', 'Line', 'PolyLine', and 'Polygon'. *None* if not applicable. For possible values and descriptions in this list, see the :ref:`AdobeManual`, table 1.76 on page 400.

         :rtype: tuple

      .. attribute:: vertices
         :no-index:

         A list containing a variable number of point ("vertices") coordinates (each given by a pair of floats) for various types of annotations:

         * 'Line' -- the starting and ending coordinates (2 float pairs).
         * 'FreeText' -- 2 or 3 float pairs designating the starting, the (optional) knee point, and the ending coordinates.
         * 'PolyLine' / 'Polygon' -- the coordinates of the edges connected by line pieces (n float pairs for n points).
         * text markup annotations -- 4 float pairs specifying the *QuadPoints* of the marked text span (see :ref:`AdobeManual`, page 403).
         * 'Ink' -- list of one to many sublists of vertex coordinates. Each such sublist represents a separate line in the drawing.

         :rtype: list


      .. attribute:: colors
         :no-index:

         dictionary of two lists of floats in range *0 <= float <= 1* specifying the "stroke" and the interior ("fill") colors. The stroke color is used for borders and everything that is actively painted or written ("stroked"). The fill color is used for the interior of objects like line ends, circles and squares. The lengths of these lists implicitly determine the colorspaces used: 1 = GRAY, 3 = RGB, 4 = CMYK. So "[1.0, 0.0, 0.0]" stands for RGB color red. Both lists can be empty if no color is specified.

         :rtype: dict

      .. attribute:: xref
         :no-index:

         The PDF :data:`xref`.

         :rtype: int

      .. attribute:: irt_xref
         :no-index:

         The PDF :data:`xref` of an annotation to which this one responds. Return zero if this is no response annotation.

         :rtype: int

      .. attribute:: popup_xref
         :no-index:

         The PDF :data:`xref` of the associated Popup annotation. Zero if non-existent.

         :rtype: int

      .. attribute:: has_popup
         :no-index:

         Whether the annotation has a Popup annotation.

         :rtype: bool

      .. attribute:: is_open
         :no-index:

         Whether the annotation's Popup is open -- **or** the annotation itself ('Text' annotations only).

         :rtype: bool

      .. attribute:: popup_rect
         :no-index:

         The rectangle of the associated Popup annotation. Infinite rectangle if non-existent.

         :rtype: :ref:`Rect`

      .. attribute:: rect_delta
         :no-index:

         A tuple of four floats representing the `/RD` entry of the annotation. The four numbers describe the numerical differences (left, top, -right, -bottom) between two rectangles: the :attr:`rect` of the annotation and a rectangle contained within that rectangle. If the entry is missing, this property is `(0, 0, 0, 0)`. If the annotation border is a normal, straight line, these numbers are typically border width divided by 2. If the annotation has a "cloudy" border, you will see the breadth of the cloud semi-circles here. In general, the numbers need not be identical. To compute the inner rectangle do `a.rect + a.rect_delta`.

      .. attribute:: border
         :no-index:

         A dictionary containing border characteristics. Empty if no border information exists. The following keys may be present:

         * *width* -- a float indicating the border thickness in points. The value is -1.0 if no width is specified.

         * *dashes* -- a sequence of integers specifying a line dashing pattern. *[]* means no dashes, *[n]* means equal on-off lengths of *n* points, longer lists will be interpreted as specifying alternating on-off length values. See the :ref:`AdobeManual` page 126 for more details.

         * *style* -- 1-byte border style: **"S"** (Solid) = solid line surrounding the annotation, **"D"** (Dashed) = dashed line surrounding the annotation, the dash pattern is specified by the *dashes* entry, **"B"** (Beveled) = a simulated embossed rectangle that appears to be raised above the surface of the page, **"I"** (Inset) = a simulated engraved rectangle that appears to be recessed below the surface of the page, **"U"** (Underline) = a single line along the bottom of the annotation rectangle.

         * *clouds* -- an integer indicating a "cloudy" border, where ``n`` is an integer `-1 <= n <= 2`. A value `n = 0` indicates a straight line (no clouds), 1 means small and 2 means large semi-circles, mimicking the cloudy appearance. If -1, then no specification is present.

         :rtype: dict


.. _mupdficons:

MuPDF 中的注释图标
-------------------------

Annotation Icons in MuPDF

.. tab:: 中文

   这是注释类型“文本”和“文件附件”的可按名称引用的图标列表。您可以在添加注释时通过 *icon* 参数使用它们，或者在 :meth:`Annot.set_name` 中使用 as 参数。您可以自行决定何时选择哪个项目 - 没有任何机制可以阻止您使用例如“文件附件”的“扬声器”图标。

.. tab:: 英文

   This is a list of icons referenceable by name for annotation types 'Text' and 'FileAttachment'. You can use them via the *icon* parameter when adding an annotation, or use the as argument in :meth:`Annot.set_name`. It is left to your discretion which item to choose when -- no mechanism will keep you from using e.g. the "Speaker" icon for a 'FileAttachment'.

.. image:: images/mupdf-icons.*


例子
--------

Example

.. tab:: 中文

   更改注释的图形图像。还可以更新“作者”和要显示在弹出窗口中的文本::

      doc = pymupdf.open("circle-in.pdf")
      page = doc[0]                          # 页面 0
      annot = page.first_annot                # 获取注释
      annot.set_border(dashes=[3])           # 设置虚线为 "3 on, 3 off ..."

      # 设置边框和填充颜色为蓝色
      annot.set_colors({"stroke":(0, 0, 1), "fill":(0.75, 0.8, 0.95)})
      info = annot.info                      # 获取信息字典
      info["title"] = "Jorj X. McKie"        # 设置作者

      # 弹出窗口中的文本...
      info["content"] = "我更改了边框和颜色，并将图像放大了 20%。"
      info["subject"] = "PyMuPDF 演示"     # 一些 PDF 查看器也会显示此内容
      annot.set_info(info)                   # 更新信息字典
      r = annot.rect                         # 获取注释矩形
      r.x1 = r.x0 + r.width  * 1.2           # 新位置与左上角相同
      r.y1 = r.y0 + r.height * 1.2           # 但边长延长了 20%
      annot.set_rect(r)                      # 更新矩形
      annot.update()                         # 更新注释外观
      doc.save("circle-out.pdf")             # 保存

   这就是圆形注释在更改前后的样子（使用 Nitro PDF 查看器显示的弹出窗口）：

.. tab:: 英文

   Change the graphical image of an annotation. Also update the "author" and the text to be shown in the popup window::

   doc = pymupdf.open("circle-in.pdf")
   page = doc[0]                          # page 0
   annot = page.first_annot                # get the annotation
   annot.set_border(dashes=[3])           # set dashes to "3 on, 3 off ..."

   # set stroke and fill color to some blue
   annot.set_colors({"stroke":(0, 0, 1), "fill":(0.75, 0.8, 0.95)})
   info = annot.info                      # get info dict
   info["title"] = "Jorj X. McKie"        # set author

   # text in popup window ...
   info["content"] = "I changed border and colors and enlarged the image by 20%."
   info["subject"] = "Demonstration of PyMuPDF"     # some PDF viewers also show this
   annot.set_info(info)                   # update info dict
   r = annot.rect                         # take annot rect
   r.x1 = r.x0 + r.width  * 1.2           # new location has same top-left
   r.y1 = r.y0 + r.height * 1.2           # but 20% longer sides
   annot.set_rect(r)                      # update rectangle
   annot.update()                         # update the annot's appearance
   doc.save("circle-out.pdf")             # save

   This is how the circle annotation looks like before and after the change (pop-up windows displayed using Nitro PDF viewer):

|circle|

.. |circle| image:: images/img-circle.*


.. rubric:: Footnotes

.. [#f1] Rotating an annotation also changes its rectangle. Depending on how the annotation was defined, the original rectangle is **cannot be reconstructed** by setting the rotation value to zero again and will be lost.

.. [#f2] Only the following annotation types support method :meth:`Annot.set_rect`: Text, FreeText, Square, Circle, Redact, Stamp, Caret, FileAttachment, Sound, and Movie.

.. [#f3] 旋转注释会同时改变其矩形区域。根据注释的定义方式，原始矩形 **无法通过将旋转值重置为零来恢复** ，并且会永久丢失。

.. [#f4] 仅以下类型的注释支持方法 :meth:`Annot.set_rect`：Text、FreeText、Square、Circle、Redact、Stamp、Caret、FileAttachment、Sound 和 Movie。

.. include:: footer.rst
