.. include:: header.rst

.. _Page:

================
Page
================

.. tab:: 中文

   表示文档页面的类。页面对象由 :meth:`Document.load_page` 创建，或者等效地通过 `doc[n]` 进行索引访问——它没有独立的构造函数。

   文档和页面之间存在父子关系。如果文档被关闭或删除，所有已存在的页面对象（以及它们的子对象）都将变得不可用（"孤立"）。在这种情况下，任何页面属性或方法的调用都会引发异常。

   为了方便使用，许多页面方法都有一个相应的 :ref:`Document` 级别的替代方法。本章节末尾提供了这些方法的概览。

   .. note::

      本章节中多次提及 **坐标** 这一术语。理解坐标系统至关重要，建议先熟悉 :ref:`Coordinates` 章节，以便更好地掌握相关概念。


.. tab:: 英文

   Class representing a document page. A page object is created by :meth:`Document.load_page` or, equivalently, via indexing the document like `doc[n]` - it has no independent constructor.

   There is a parent-child relationship between a document and its pages. If the document is closed or deleted, all page objects (and their respective children, too) in existence will become unusable ("orphaned"): If a page property or method is being used, an exception is raised.

   Several page methods have a :ref:`Document` counterpart for convenience. At the end of this chapter you will find a synopsis.

   .. note:: 
      
      Many times in this chapter we are using the term **coordinate**. It is of high importance to have at least a basic understanding of what that is and that you feel comfortable with the section :ref:`Coordinates`.

更改 Page
---------------

Modifying Pages

.. tab:: 中文

   仅 PDF 文档支持更改页面属性和修改页面内容。

   简而言之，您可以使用 PyMuPDF 执行以下操作：

   * 修改页面旋转角度及其可见部分（"裁剪框"）。
   * 插入图像、其他 PDF 页面、文本和简单的几何对象。
   * 添加注释和表单字段。

   .. note::

      许多方法需要使用坐标（点、矩形）来将内容放置在指定位置。请注意，自 v1.17.0 以来，这些坐标 **必须始终** 以 **未旋转** 的页面为基准提供。反之亦然：除了 :attr:`Page.rect` 和 :meth:`Page.bound` （它们会随页面旋转而变化），其他方法和属性返回的所有坐标均基于未旋转页面。

      例如，:meth:`Page.get_image_bbox` 的返回值不会因 :meth:`Page.set_rotation` 调用而变化。同样，:meth:`Page.get_text` 返回的坐标、注释矩形等也不会变化。如果要获取对象在 **旋转坐标系** 中的位置，可以将坐标与 :attr:`Page.rotation_matrix` 相乘。相应地，:attr:`Page.derotation_matrix` 提供了逆变换，可用于与其他可能有不同行为的阅读器交互。

   .. note::

      如果您在页面上添加或更新了注释、链接或表单字段，并且 **立即** 需要处理这些内容（即 **未离开页面**），请在访问这些新或更新的项目之前，使用 :meth:`Document.reload_page` 重新加载页面。

      通常建议重新加载页面——尽管在所有情况下都不一定是强制性的。然而，与 MuPDF 相比，PyMuPDF 对某些注释和表单控件提供了扩展功能，未来可能还会增加更多扩展。

      重新加载页面可确保所有更改已完全应用到 PDF 结构，因此您可以安全地创建 Pixmap，或成功地遍历注释、链接和表单字段。

   ================================== =======================================================
   **方法 / 属性**                     **简要描述**
   ================================== =======================================================
   :meth:`Page.add_caret_annot`       仅限 PDF：添加脱字符（Caret）注释
   :meth:`Page.add_circle_annot`      仅限 PDF：添加圆形注释
   :meth:`Page.add_file_annot`        仅限 PDF：添加文件附件注释
   :meth:`Page.add_freetext_annot`    仅限 PDF：添加自由文本注释
   :meth:`Page.add_highlight_annot`   仅限 PDF：添加高亮注释
   :meth:`Page.add_ink_annot`         仅限 PDF：添加手写墨迹注释
   :meth:`Page.add_line_annot`        仅限 PDF：添加直线注释
   :meth:`Page.add_polygon_annot`     仅限 PDF：添加多边形注释
   :meth:`Page.add_polyline_annot`    仅限 PDF：添加折线注释
   :meth:`Page.add_rect_annot`        仅限 PDF：添加矩形注释
   :meth:`Page.add_redact_annot`      仅限 PDF：添加遮蔽（Redaction）注释
   :meth:`Page.add_squiggly_annot`    仅限 PDF：添加波浪线注释
   :meth:`Page.add_stamp_annot`       仅限 PDF：添加印章（Stamp）注释
   :meth:`Page.add_strikeout_annot`   仅限 PDF：添加删除线（Strikeout）注释
   :meth:`Page.add_text_annot`        仅限 PDF：添加文本注释（评论）
   :meth:`Page.add_underline_annot`   仅限 PDF：添加下划线注释
   :meth:`Page.add_widget`            仅限 PDF：添加表单控件字段
   :meth:`Page.annot_names`           仅限 PDF：获取页面上所有注释（及控件）名称的列表
   :meth:`Page.annot_xrefs`           仅限 PDF：获取页面上所有注释（及控件）xref 的列表
   :meth:`Page.annots`                返回页面上所有注释的生成器
   :meth:`Page.apply_redactions`      仅限 PDF：处理页面上的遮蔽内容
   :meth:`Page.bound`                 获取页面边界矩形
   :meth:`Page.cluster_drawings`      仅限 PDF：获取矢量图形的边界框
   :meth:`Page.delete_annot`          仅限 PDF：删除指定注释
   :meth:`Page.delete_image`          仅限 PDF：删除页面上的图像
   :meth:`Page.delete_link`           仅限 PDF：删除链接
   :meth:`Page.delete_widget`         仅限 PDF：删除表单控件字段
   :meth:`Page.draw_bezier`           仅限 PDF：绘制三次贝塞尔曲线
   :meth:`Page.draw_circle`           仅限 PDF：绘制圆
   :meth:`Page.draw_curve`            仅限 PDF：绘制贝塞尔曲线
   :meth:`Page.draw_line`             仅限 PDF：绘制直线
   :meth:`Page.draw_oval`             仅限 PDF：绘制椭圆
   :meth:`Page.draw_polyline`         仅限 PDF：绘制折线
   :meth:`Page.draw_quad`             仅限 PDF：绘制四边形
   :meth:`Page.draw_rect`             仅限 PDF：绘制矩形
   :meth:`Page.draw_sector`           仅限 PDF：绘制扇形
   :meth:`Page.draw_squiggle`         仅限 PDF：绘制波浪线
   :meth:`Page.draw_zigzag`           仅限 PDF：绘制锯齿线
   :meth:`Page.find_tables`           在页面上检测表格
   :meth:`Page.get_drawings`          获取页面上的矢量图形
   :meth:`Page.get_fonts`             仅限 PDF：获取页面中引用的字体列表
   :meth:`Page.get_image_bbox`        仅限 PDF：获取嵌入图像的边界框和矩阵
   :meth:`Page.get_image_info`        获取页面中所有图像的元信息列表
   :meth:`Page.get_image_rects`       仅限 PDF：改进版的 :meth:`Page.get_image_bbox`
   :meth:`Page.get_images`            仅限 PDF：获取页面中引用的图像列表
   :meth:`Page.get_label`             仅限 PDF：获取页面标签
   :meth:`Page.get_links`             获取页面上所有链接
   :meth:`Page.get_pixmap`            以光栅格式创建页面图像
   :meth:`Page.get_svg_image`         以 SVG 格式创建页面图像
   :meth:`Page.get_text`              提取页面文本
   :meth:`Page.get_textbox`           提取矩形区域内的文本
   :meth:`Page.get_textpage_ocr`      使用 OCR 解析页面文本
   :meth:`Page.get_textpage`          解析页面文本
   :meth:`Page.get_xobjects`          仅限 PDF：获取页面中引用的 xobjects 列表
   :meth:`Page.insert_font`           仅限 PDF：在页面上插入字体
   :meth:`Page.insert_image`          仅限 PDF：插入图像
   :meth:`Page.insert_link`           仅限 PDF：插入链接
   :meth:`Page.insert_text`           仅限 PDF：插入文本
   :meth:`Page.insert_htmlbox`        仅限 PDF：在矩形区域内插入 HTML 文本
   :meth:`Page.insert_textbox`        仅限 PDF：插入文本框
   :meth:`Page.links`                 返回页面上所有链接的生成器
   :meth:`Page.load_annot`            仅限 PDF：加载指定注释
   :meth:`Page.load_widget`           仅限 PDF：加载指定表单控件字段
   :meth:`Page.load_links`            返回页面上的第一个链接
   :meth:`Page.new_shape`             仅限 PDF：创建新的 :ref:`Shape`
   :meth:`Page.remove_rotation`       仅限 PDF：移除页面旋转（重置为 0）
   :meth:`Page.replace_image`         仅限 PDF：替换页面上的图像
   :meth:`Page.search_for`            搜索字符串
   :meth:`Page.set_artbox`            仅限 PDF：修改 `/ArtBox`
   :meth:`Page.set_bleedbox`          仅限 PDF：修改 `/BleedBox`
   :meth:`Page.set_cropbox`           仅限 PDF：修改 :data:`cropbox` （可见页面）
   :meth:`Page.set_mediabox`          仅限 PDF：修改 `/MediaBox`
   :meth:`Page.set_rotation`          仅限 PDF：设置页面旋转角度
   :meth:`Page.set_trimbox`           仅限 PDF：修改 `/TrimBox`
   :meth:`Page.show_pdf_page`         仅限 PDF：显示 PDF 页面图像
   :meth:`Page.update_link`           仅限 PDF：修改链接
   :meth:`Page.widgets`               返回页面上所有表单控件字段的生成器
   :meth:`Page.write_text`            写入一个或多个 :ref:`Textwriter` 对象
   :attr:`Page.cropbox_position`      :data:`cropbox` 的偏移量
   :attr:`Page.cropbox`               页面的 :data:`cropbox`
   :attr:`Page.artbox`                页面的 `/ArtBox`
   :attr:`Page.bleedbox`              页面的 `/BleedBox`
   :attr:`Page.trimbox`               页面的 `/TrimBox`
   :attr:`Page.derotation_matrix`     仅限 PDF：获取未旋转坐标
   :attr:`Page.first_annot`           页面上的第一个 :ref:`Annot`
   :attr:`Page.first_link`            页面上的第一个 :ref:`Link`
   :attr:`Page.first_widget`          页面上的第一个表单控件字段
   :attr:`Page.mediabox_size`         :data:`mediabox` 的右下角点
   :attr:`Page.mediabox`              页面的 :data:`mediabox`
   :attr:`Page.number`                页码
   :attr:`Page.parent`                所属文档对象
   :attr:`Page.rect`                  页面矩形区域
   :attr:`Page.rotation_matrix`       仅限 PDF：获取旋转坐标
   :attr:`Page.rotation`              仅限 PDF：页面旋转角度
   :attr:`Page.transformation_matrix` 仅限 PDF：转换 PDF 和 MuPDF 坐标
   :attr:`Page.xref`                  仅限 PDF：页面的 :data:`xref`
   ================================== =======================================================

.. tab:: 英文

   Changing page properties and adding or changing page content is available for PDF documents only.

   In a nutshell, this is what you can do with PyMuPDF:

   * Modify page rotation and the visible part ("cropbox") of the page.
   * Insert images, other PDF pages, text and simple geometrical objects.
   * Add annotations and form fields.

   .. note::

      Methods require coordinates (points, rectangles) to put content in desired places. Please be aware that these coordinates **must always** be provided relative to the **unrotated** page (since v1.17.0). The reverse is also true: except :attr:`Page.rect`, resp. :meth:`Page.bound` (both *reflect* when the page is rotated), all coordinates returned by methods and attributes pertain to the unrotated page.

      So the returned value of e.g. :meth:`Page.get_image_bbox` will not change if you do a :meth:`Page.set_rotation`. The same is true for coordinates returned by :meth:`Page.get_text`, annotation rectangles, and so on. If you want to find out, where an object is located in **rotated coordinates**, multiply the coordinates with :attr:`Page.rotation_matrix`. There also is its inverse, :attr:`Page.derotation_matrix`, which you can use when interfacing with other readers, which may behave differently in this respect.

   .. note::

      If you add or update annotations, links or form fields on the page and immediately afterwards need to work with them (i.e. **without leaving the page**), you should reload the page using :meth:`Document.reload_page` before referring to these new or updated items.

      Reloading the page is generally recommended -- although not strictly required in all cases. However, some annotation and widget types have extended features in PyMuPDF compared to MuPDF. More of these extensions may also be added in the future.

      Releoading the page ensures all your changes have been fully applied to PDF structures, so you can safely create Pixmaps or successfully iterate over annotations, links and form fields.

   ================================== =======================================================
   **Method / Attribute**             **Short Description**
   ================================== =======================================================
   :meth:`Page.add_caret_annot`       PDF only: add a caret annotation
   :meth:`Page.add_circle_annot`      PDF only: add a circle annotation
   :meth:`Page.add_file_annot`        PDF only: add a file attachment annotation
   :meth:`Page.add_freetext_annot`    PDF only: add a text annotation
   :meth:`Page.add_highlight_annot`   PDF only: add a "highlight" annotation
   :meth:`Page.add_ink_annot`         PDF only: add an ink annotation
   :meth:`Page.add_line_annot`        PDF only: add a line annotation
   :meth:`Page.add_polygon_annot`     PDF only: add a polygon annotation
   :meth:`Page.add_polyline_annot`    PDF only: add a multi-line annotation
   :meth:`Page.add_rect_annot`        PDF only: add a rectangle annotation
   :meth:`Page.add_redact_annot`      PDF only: add a redaction annotation
   :meth:`Page.add_squiggly_annot`    PDF only: add a "squiggly" annotation
   :meth:`Page.add_stamp_annot`       PDF only: add a "rubber stamp" annotation
   :meth:`Page.add_strikeout_annot`   PDF only: add a "strike-out" annotation
   :meth:`Page.add_text_annot`        PDF only: add a comment
   :meth:`Page.add_underline_annot`   PDF only: add an "underline" annotation
   :meth:`Page.add_widget`            PDF only: add a PDF Form field
   :meth:`Page.annot_names`           PDF only: a list of annotation (and widget) names
   :meth:`Page.annot_xrefs`           PDF only: a list of annotation (and widget) xrefs
   :meth:`Page.annots`                return a generator over the annots on the page
   :meth:`Page.apply_redactions`      PDF only: process the redactions of the page
   :meth:`Page.bound`                 rectangle of the page
   :meth:`Page.cluster_drawings`      PDF only: bounding boxes of vector graphics
   :meth:`Page.delete_annot`          PDF only: delete an annotation
   :meth:`Page.delete_image`          PDF only: delete an image
   :meth:`Page.delete_link`           PDF only: delete a link
   :meth:`Page.delete_widget`         PDF only: delete a widget / field
   :meth:`Page.draw_bezier`           PDF only: draw a cubic Bezier curve
   :meth:`Page.draw_circle`           PDF only: draw a circle
   :meth:`Page.draw_curve`            PDF only: draw a special Bezier curve
   :meth:`Page.draw_line`             PDF only: draw a line
   :meth:`Page.draw_oval`             PDF only: draw an oval / ellipse
   :meth:`Page.draw_polyline`         PDF only: connect a point sequence
   :meth:`Page.draw_quad`             PDF only: draw a quad
   :meth:`Page.draw_rect`             PDF only: draw a rectangle
   :meth:`Page.draw_sector`           PDF only: draw a circular sector
   :meth:`Page.draw_squiggle`         PDF only: draw a squiggly line
   :meth:`Page.draw_zigzag`           PDF only: draw a zig-zagged line
   :meth:`Page.find_tables`           locate tables on the page
   :meth:`Page.get_drawings`          get vector graphics on page
   :meth:`Page.get_fonts`             PDF only: get list of referenced fonts
   :meth:`Page.get_image_bbox`        PDF only: get bbox and matrix of embedded image
   :meth:`Page.get_image_info`        get list of meta information for all used images
   :meth:`Page.get_image_rects`       PDF only: improved version of :meth:`Page.get_image_bbox`
   :meth:`Page.get_images`            PDF only: get list of referenced images
   :meth:`Page.get_label`             PDF only: return the label of the page
   :meth:`Page.get_links`             get all links
   :meth:`Page.get_pixmap`            create a page image in raster format
   :meth:`Page.get_svg_image`         create a page image in SVG format
   :meth:`Page.get_text`              extract the page's text
   :meth:`Page.get_textbox`           extract text contained in a rectangle
   :meth:`Page.get_textpage_ocr`      create a TextPage with OCR for the page
   :meth:`Page.get_textpage`          create a TextPage for the page
   :meth:`Page.get_xobjects`          PDF only: get list of referenced xobjects
   :meth:`Page.insert_font`           PDF only: insert a font for use by the page
   :meth:`Page.insert_image`          PDF only: insert an image
   :meth:`Page.insert_link`           PDF only: insert a link
   :meth:`Page.insert_text`           PDF only: insert text
   :meth:`Page.insert_htmlbox`        PDF only: insert html text in a rectangle
   :meth:`Page.insert_textbox`        PDF only: insert a text box
   :meth:`Page.links`                 return a generator of the links on the page
   :meth:`Page.load_annot`            PDF only: load a specific annotation
   :meth:`Page.load_widget`           PDF only: load a specific field
   :meth:`Page.load_links`            return the first link on a page
   :meth:`Page.new_shape`             PDF only: create a new :ref:`Shape`
   :meth:`Page.remove_rotation`       PDF only: set page rotation to 0
   :meth:`Page.replace_image`         PDF only: replace an image
   :meth:`Page.search_for`            search for a string
   :meth:`Page.set_artbox`            PDF only: modify `/ArtBox`
   :meth:`Page.set_bleedbox`          PDF only: modify `/BleedBox`
   :meth:`Page.set_cropbox`           PDF only: modify the :data:`cropbox` (visible page)
   :meth:`Page.set_mediabox`          PDF only: modify `/MediaBox`
   :meth:`Page.set_rotation`          PDF only: set page rotation
   :meth:`Page.set_trimbox`           PDF only: modify `/TrimBox`
   :meth:`Page.show_pdf_page`         PDF only: display PDF page image
   :meth:`Page.update_link`           PDF only: modify a link
   :meth:`Page.widgets`               return a generator over the fields on the page
   :meth:`Page.write_text`            write one or more :ref:`Textwriter` objects
   :attr:`Page.cropbox_position`      displacement of the :data:`cropbox`
   :attr:`Page.cropbox`               the page's :data:`cropbox`
   :attr:`Page.artbox`                the page's `/ArtBox`
   :attr:`Page.bleedbox`              the page's `/BleedBox`
   :attr:`Page.trimbox`               the page's `/TrimBox`
   :attr:`Page.derotation_matrix`     PDF only: get coordinates in unrotated page space
   :attr:`Page.first_annot`           first :ref:`Annot` on the page
   :attr:`Page.first_link`            first :ref:`Link` on the page
   :attr:`Page.first_widget`          first widget (form field) on the page
   :attr:`Page.mediabox_size`         bottom-right point of :data:`mediabox`
   :attr:`Page.mediabox`              the page's :data:`mediabox`
   :attr:`Page.number`                page number
   :attr:`Page.parent`                owning document object
   :attr:`Page.rect`                  rectangle of the page
   :attr:`Page.rotation_matrix`       PDF only: get coordinates in rotated page space
   :attr:`Page.rotation`              PDF only: page rotation
   :attr:`Page.transformation_matrix` PDF only: translate between PDF and MuPDF space
   :attr:`Page.xref`                  PDF only: page :data:`xref`
   ================================== =======================================================

**Class API**

.. tab:: 中文

   .. class:: Page

      .. method:: bound()

         确定页面的矩形区域。等同于属性 :attr:`Page.rect`。对于 PDF 文档，该矩形 **通常** 也与 :data:`mediabox` 和 :data:`cropbox` 一致，但并非总是如此。例如，如果页面经过旋转，则此方法会反映该变化，而 :attr:`Page.cropbox` 不会受到影响。

         :rtype: :ref:`Rect`

      .. method:: add_caret_annot(point)

         仅适用于 PDF：添加一个插入标记图标。插入标记（Caret Annotation）通常用于指示页面上的文本编辑。

         :arg point_like point: 插入标记图标的左上角位置，该图标位于一个 20 × 20 的矩形内，由 MuPDF 提供。

         :rtype: :ref:`Annot`
         :returns: 创建的注释对象。描边颜色为蓝色 (0, 0, 1)，不支持填充颜色。

         .. image:: images/img-caret-annot.*
            :scale: 70

         |history_begin|

         * 新增于 v1.16.0

         |history_end|

      .. method:: add_text_annot(point, text, icon="Note")

         仅适用于 PDF：添加评论图标（“便签”），并附带文本。仅图标可见，文本默认隐藏，许多 PDF 查看器可在鼠标悬停或双击图标时显示文本内容。

         :arg point_like point: 评论图标的左上角位置，该图标位于 20 × 20 的矩形内，由 MuPDF 提供。

         :arg str text: 评论文本，双击或悬停图标时显示。可包含任意拉丁字符。

         :arg str icon: 选择用于表示评论的图标，可选值包括：
            - `"Note"` （默认）
            - `"Comment"`
            - `"Help"`
            - `"Insert"`
            - `"Key"`
            - `"NewParagraph"`
            - `"Paragraph"`
         （新增于 v1.16.0）

         :rtype: :ref:`Annot`
         :returns: 创建的注释对象。描边颜色为黄色 (1, 1, 0)，不支持填充颜色。


      .. index::
         pair: rect; add_freetext_annot
         pair: fontsize; add_freetext_annot
         pair: fontname; add_freetext_annot
         pair: text_color; add_freetext_annot
         pair: fill_color; add_freetext_annot
         pair: border_width; add_freetext_annot
         pair: dashes; add_freetext_annot
         pair: callout; add_freetext_annot
         pair: line_end; add_freetext_annot
         pair: opacity; add_freetext_annot
         pair: align; add_freetext_annot
         pair: rotate; add_freetext_annot
         pair: richtext; add_freetext_annot
         pair: style; add_freetext_annot

      .. method:: add_freetext_annot(rect, text, *, fontsize=11, fontname="helv", text_color=0, fill_color=None, border_width=0, dashes=None, callout=None, line_end=PDF_ANNOT_LE_OPEN_ARROW, opacity=1, align=TEXT_ALIGN_LEFT, rotate=0, richtext=False, style=None)

         仅适用于 PDF：在给定矩形区域内添加文本。可以通过指定两个或三个点状对象来选择性地请求显示“标注”形状（见下文）。

         :arg rect_like rect: 要插入文本的矩形区域。文本会根据框宽自动换行，超出矩形区域的文本部分将不可见且不提示警告。

         :arg str text: 文本内容。可以包含任何混合的拉丁字母、希腊字母、 Cyrillic、汉字、日文和韩文字符。如果 `richtext=True` （见下文），则字符串会被解释为 HTML 语法，这为文本效果提供了更多的选择。

         :arg float fontsize: 字体大小。默认值为 11。如果 `richtext=True`，则此参数会被忽略。

         :arg str fontname: 字体名称。默认值为 "Helv"。如果 `richtext=True`，则此参数被忽略；否则，以下字体是可接受的：
            
            * 可接受的字体有 "Helv"（Helvetica），"Cour"（Courier），"TiRo"（Times-Roman），"ZaDb"（ZapfDingBats）和 "Symb"（Symbol）。名称可以缩写为前两个字母，如 "Co" 代表 "Cour"，可使用小写字母。

            * 不支持字体的粗体或斜体变体。

         :arg list,tuple,float text_color: 文本颜色。默认值为黑色。如果 `richtext=True`，则此参数被忽略。

         :arg list,tuple,float fill_color: 填充颜色。用于 ``rect`` 和适用情况下的标注线条的终点。默认值为 ``None``。

         :arg list,tuple,float border_color: 该参数仅对 `richtext=True` 有效，否则使用 ``text_color``。

         :arg float border_width: 边框和标注线条的宽度。默认值为 0（无边框），在这种情况下，标注线条可能会在不同的 PDF 查看器中显示为细线。

         :arg list,tuple dashes: 指定边框和标注线条虚线样式的浮动列表。默认值为 ``None``。

         :arg list,tuple callout: 一个包含两个或三个 :data:`point_like` 对象的列表/元组，这些对象会被解释为起点、膝点和终点（按此顺序），将该标注转换为一个标注形状。

         :arg int line_end: 标注线条的终点符号。它会被绘制在 `callout` 列表中的第一个点。默认值为开放箭头。可以参考 :ref:`AnnotationLineEnds` 获取更多可能的值。

         :arg float opacity: 透明度，值为 `0 <= opacity < 1`，使标注变得透明。默认值为无透明度。

         :arg int align: 文本对齐方式，取值为 TEXT_ALIGN_LEFT、TEXT_ALIGN_CENTER 或 TEXT_ALIGN_RIGHT — 不支持两端对齐。如果 `richtext=True`，此参数将被忽略。

         :arg int rotate: 文本方向，接受的值为 90° 的整数倍。无效值将会设置为 0。

         :arg bool richtext: 将 ``text`` 视为 HTML 语法。允许实现 **粗体**、*斜体*、任意文本颜色、字体大小、文本对齐（包括两端对齐）等效果，具体取决于 HTML 和样式指令的支持。类似于 :meth:`Page.insert_htmlbox` 中的处理方式。基础库会在遇到不包含在标准字体中的字符时引入所需的字体。如果设置了此选项，一些参数将被忽略，如上所述。默认值为 ``False``。

         :arg str style: 提供可选的 CSS 语法的 HTML 样式信息。如果 `richtext=False`，则此参数被忽略。

         :rtype: :ref:`Annot`
         :returns: 创建的注释对象。

         |history_begin|

         * 在 v1.19.6 中更改：添加边框颜色参数

         |history_end|

      .. method:: add_file_annot(pos, buffer, filename, ufilename=None, desc=None, icon="PushPin")

         仅适用于 PDF：在指定位置添加文件附件标注，并显示 "PushPin" 图标。

         :arg point_like pos: "PushPin" 图标的左上角位置，该图标位于一个 18x18 的矩形内。

         :arg bytes,bytearray,BytesIO buffer: 要存储的数据（实际文件内容、任何数据等）。

            在 v1.14.13 中更改：现在也支持 *io.BytesIO*。

         :arg str filename: 与数据关联的文件名。

         :arg str ufilename: 文件名的可选 PDF Unicode 版本。默认为文件名。

         :arg str desc: 文件的可选描述。默认为文件名。

         :arg str icon: 选择以下图标之一作为附件的可视符号：
            - `"PushPin"` （默认）
            - `"Graph"`
            - `"Paperclip"`
            - `"Tag"`
            （新增于 v1.16.0）

         :rtype: :ref:`Annot`
         :returns: 创建的注释对象。描边颜色为黄色 (1, 1, 0)，不支持填充颜色。


      .. method:: add_ink_annot(list)

         仅适用于 PDF：添加“手绘”涂鸦标注。

         :arg sequence list: 一个包含一个或多个子列表的列表，每个子列表包含 :data:`point_like` 项。每个子列表中的每个项都被解释为 :ref:`Point`，并通过这些点绘制连接线。不同的子列表代表不同的绘制线条。

         :rtype: :ref:`Annot`
         :returns: 创建的注释，默认外观为黑色 (0, 0, 0)，线条宽度为 1。没有填充颜色支持。

      .. method:: add_line_annot(p1, p2)

         仅适用于 PDF：添加一条直线标注。

         :arg point_like p1: 直线的起始点。

         :arg point_like p2: 直线的终点。

         :rtype: :ref:`Annot`
         :returns: 创建的注释。直线（描边）颜色为红色 = (1, 0, 0)，线条宽度为 1。没有填充颜色支持。 **标注矩形** 会自动创建，包含两个点，每个点周围有半径为 3 * 线条宽度的圆圈，以便为任何线条结束符号腾出空间。

      .. method:: add_rect_annot(rect)

      .. method:: add_circle_annot(rect)

         仅适用于 PDF：添加矩形或圆形标注。

         :arg rect_like rect: 绘制矩形或圆形的矩形区域，必须是有限且非空的。如果矩形不是等边的，则绘制椭圆。

         :rtype: :ref:`Annot`
         :returns: 创建的注释。它以红色 (1, 0, 0) 作为描边颜色，线条宽度为 1，支持填充颜色。


      ---------

      删节/Redactions
      ~~~~~~~~~~~~~~~~~~~

      .. method:: add_redact_annot(quad, text=None, fontname=None, fontsize=11, align=TEXT_ALIGN_LEFT, fill=(1, 1, 1), text_color=(0, 0, 0), cross_out=True)
         
         **仅适用于 PDF**：添加一个编辑标注。编辑标注标识出需要从文档中移除的内容。添加此类标注是两个步骤中的第一个，它会显示出在随后的步骤中将被移除的内容，具体步骤为 :meth:`Page.apply_redactions`。

         :arg quad_like,rect_like quad: 指定要移除的（矩形）区域，这个区域始终等于标注矩形。它可以是 :data:`rect_like` 或 :data:`quad_like` 对象。如果指定了四边形，则取其包围矩形。

         :arg str text: 在应用编辑后（即移除旧内容）要放入矩形中的文本。（新增于 v1.16.12）

         :arg str fontname: 使用的字体名称，当提供 *text* 时才有效，否则忽略。此处适用与 :meth:`Page.insert_textbox` 相同的规则 -- 即 :meth:`Page.apply_redactions` 内部调用的方法。替换文本将 **垂直居中**，如果使用的是 CJK 或 :ref:`Base-14-Fonts` 字体。（新增于 v1.16.12）

            .. note::

               * 对于页面中的 **现有** 字体，使用其引用名称作为 *fontname* （即 :meth:`Page.get_fonts` 中条目的 *item[4]*）。
               * 对于 **新字体，非内建字体**，操作如下::

                  page.insert_text(point,  # 任意位置，但不在所有编辑矩形内
                        "something",  # 非空字符串
                        fontname="newname",  # 新的、未使用的引用名称
                        fontfile="...",  # 所需的字体文件
                        render_mode=3,  # 使文本不可见
                  )
                  page.add_redact_annot(..., fontname="newname")

         :arg float fontsize: 替换文本的 :data:`fontsize`。如果文本太大以至于无法适应，会尝试多次插入，逐渐将 :data:`fontsize` 减小，最低不低于 4。如果仍然无法适配，则不插入文本。（新增于 v1.16.12）

         :arg int align: 替换文本的水平对齐方式。参见 :meth:`insert_textbox` 以了解可用值。若使用 PDF 内建字体（CJK 或 :ref:`Base-14-Fonts`），垂直对齐将（大致）居中。（新增于 v1.16.12）

         :arg sequence fill: 编辑后矩形的填充颜色。默认值为 *白色 = (1, 1, 1)*，如果指定为 ``None``，也会取此值。若完全不希望填充颜色，可指定 ``False``。此时矩形保持透明。（新增于 v1.16.12）

         :arg sequence text_color: 替换文本的颜色。默认值为 *黑色 = (0, 0, 0)*。（新增于 v1.16.12）

         :arg bool cross_out: 向标注矩形添加两条对角线。（新增于 v1.17.2）

         :rtype: :ref:`Annot`
         :returns: 创建的标注。其标准外观为一个红色矩形（无填充颜色），可选择显示两条对角线。颜色、线宽、虚线、透明度和混合模式可以像其他标注一样通过 :meth:`Annot.update` 设置和应用。（在 v1.17.2 中更改）

         .. image:: images/img-redact.*

         |history_begin|

         * 新增于 v1.16.11
         * 在 v1.16.12 中更改：删除了先前的 *mark* 参数。现在，每个编辑标注的矩形将被填充各自的 *fill* 颜色。如果标注中提供了 *text*，则会调用 :meth:`insert_textbox` 插入替换文本。
         * 在 v1.18.0 中更改：增加了处理重叠图片的选项。
         * 在 v1.23.27 中更改：增加了移除图形的选项。
         * 在 v1.24.2 中更改：增加了 `keep_text` 选项，用于保持文本不受影响。

         |history_end|

      .. method:: apply_redactions(images=PDF_REDACT_IMAGE_PIXELS|2, graphics=PDF_REDACT_LINE_ART_REMOVE_IF_TOUCHED|2, text=PDF_REDACT_TEXT_REMOVE|0)

         **仅适用于 PDF**：移除页面上所有 **内容**，这些内容被任何编辑矩形覆盖。

         **此方法会应用并删除页面上的所有编辑标注。**

         :arg int images: 如何处理重叠的图片。默认值（2）会遮蔽重叠的像素。`PDF_REDACT_IMAGE_NONE | 0` 会忽略，`PDF_REDACT_IMAGE_REMOVE | 1` 会完全移除任何与编辑标注重叠的图片。选项 `PDF_REDACT_IMAGE_REMOVE_UNLESS_INVISIBLE | 3` 仅移除可见的图片。

         :arg int graphics: 如何处理重叠的矢量图形（也称为“线条艺术”或“图形”）。默认值（2）会移除任何重叠的矢量图形。`PDF_REDACT_LINE_ART_NONE | 0` 会忽略，`PDF_REDACT_LINE_ART_REMOVE_IF_COVERED | 1` 会移除完全被编辑标注覆盖的图形。移除线条艺术时，请注意 **描边** 矢量图形（即类型为 "s" 或 "sf"）的 **包围矩形** 比预期的大：首先，每个方向至少要加上路径线宽的 50%。如果提供了所谓的“斜接限制”（参见 PDF 规范第 121 页），扩大值为 `miter * width / 2`。因此，当使用默认设置（线宽=1，斜接=10）时，编辑矩形应至少比实际图形大 5 个点。

         :arg int text: 是否移除重叠的文本。默认值 `PDF_REDACT_TEXT_REMOVE | 0` 会移除所有与编辑矩形重叠的字符边界框中的字符。这符合编辑标注的原始法律/数据保护意图。然而，其他用例可能需要在编辑矢量图形或图片的同时 **保留文本**。这可以通过设置 `text=True|PDF_REDACT_TEXT_NONE | 1` 实现。这种做法 **不符合** 数据保护的意图。**使用此功能时请自担风险。**

         :returns: 如果处理了至少一个编辑标注，返回 `True`，否则返回 `False`。

         .. note::
            * 被编辑矩形覆盖的文本将被 **物理** 移除（假设 :meth:`Document.save` 配合适当的垃圾回收选项），并且将不再出现在文本提取或其他地方。所有编辑标注也将被移除。其他标注不受影响。

            * 所有重叠的链接将被移除。如果链接矩形覆盖了文本，则仅移除与链接矩形重叠的文本部分。类似的情况也适用于被链接矩形覆盖的图片。

            * **图片** 的重叠部分将被遮蔽，默认选项为 `PDF_REDACT_IMAGE_PIXELS` （在 v1.18.0 中更改）。选项 0 不触及任何图片，1 会移除所有与编辑标注重叠的图片。

            * 对于选项 `images=PDF_REDACT_IMAGE_REMOVE`，仅会移除该页面对图片的 **引用** - 并不一定移除图片本身。仅在图片完全不再被引用时（假设垃圾回收选项合适），才会将图片完全从文件中移除。

            * 对于选项 `images=PDF_REDACT_IMAGE_PIXELS`，会创建一个新的 PNG 格式图片，页面将使用该图片替换原图。原始图片不会在此过程中删除或替换，因此其他页面可能仍显示原图。此外，修改后的 PNG 图片目前是 **无压缩存储** 的。在选择合适的垃圾回收方法和压缩选项时，请考虑这些因素。

            * **文本移除** 是按字符进行的：如果字符的边界框与编辑矩形有 **非空重叠**，则该字符将被移除（在 MuPDF v1.17 中更改）。根据字体属性和/或所选的行间距，删除可能会影响不希望移除的文本部分。在执行文本搜索之前，使用 :meth:`Tools.set_small_glyph_heights` 配合 ``True`` 参数，可能有助于避免这种情况。

            * 编辑标注是替换 PDF 中单个单词的简单方法，或者仅仅是物理地移除它们。可以使用某些文本提取或搜索方法定位“秘密”一词，并使用“xxxxxx”作为每个实例的替换文本插入编辑标注。

            - 如果替换文本比原文本长，请小心 -- 这可能会导致外观不协调、行断裂，或者完全没有新文本。

            - 由于多种原因，新文本的定位可能与旧文本不完全对齐 -- 尤其是当替换字体不是 CJK 或 :ref:`Base-14-Fonts` 字体时，情况尤为明显。

         |history_begin|

         * 新增于 v1.16.11
         * 在 v1.16.12 中更改：删除了先前的 *mark* 参数。现在，每个编辑标注的矩形将被填充各自的 *fill* 颜色。如果标注中提供了 *text*，则会调用 :meth:`insert_textbox` 插入替换文本。
         * 在 v1.18.0 中更改：增加了处理重叠图片的选项。
         * 在 v1.23.27 中更改：增加了移除图形的选项。
         * 在 v1.24.2 中更改：增加了 `keep_text` 选项，用于保持文本不受影响。

         |history_end|

      ---------

      .. method:: add_polyline_annot(points)

      .. method:: add_polygon_annot(points)

         **仅适用于 PDF**：添加一个由连接给定点的直线构成的标注。 **多边形** 的首尾点会自动连接，而 **多线段** 则不会如此。 **矩形** 会自动创建为包含所有点的最小矩形，矩形中的每个点会被一个半径为 3 的圆圈包围（= 3 * 线宽）。以下是一个被修改过颜色和线条末端样式的 “多线段” 示例。

         :arg list points: 一组 :data:`point_like` 对象。

         :rtype: :ref:`Annot`
         :returns: 创建的标注。它的绘制方式是黑色的线条、宽度为 1，且没有填充颜色，但支持填充颜色。可以使用 :ref:`Annot` 的方法来进行修改，以实现类似于以下效果：

         .. image:: images/img-polyline.*
            :scale: 70

      .. method:: add_underline_annot(quads=None, start=None, stop=None, clip=None)

      .. method:: add_strikeout_annot(quads=None, start=None, stop=None, clip=None)

      .. method:: add_squiggly_annot(quads=None, start=None, stop=None, clip=None)

      .. method:: add_highlight_annot(quads=None, start=None, stop=None, clip=None)

         **仅适用于 PDF**：这些标注通常用于 **标记已定位的文本** （例如通过 :meth:`Page.search_for` 定位）。但这并不是必须的：您可以自由地“标记”任何内容。

         标准颜色（仅线条，不支持填充颜色）被指定为每种标注类型的默认颜色： **黄色** 用于高亮， **红色** 用于删除线， **绿色** 用于下划线， **品红** 用于波浪下划线。

         这四个方法将参数转换为一组 :ref:`Quad` 对象。然后， **标注** 矩形被计算为包含所有这些四边形的矩形。

         .. note::

            :meth:`search_for` 返回的是一组 :ref:`Rect` 或 :ref:`Quad` 对象。该列表可以直接作为这些标注类型的参数，且会为搜索字符串的所有匹配项生成 **一个公共标注** ：

               >>> # 在文本搜索中优先使用 quads=True！
               >>> quads = page.search_for("pymupdf", quads=True)
               >>> page.add_highlight_annot(quads)

         .. note::
            显然，文本标记标注需要知道被标记区域的顶部、底部、左侧和右侧位置。如果参数是四边形，则这些信息由四边形的点序列提供。而矩形则提供了较少的信息——例如，四个矩形角点可以构成 4! = 24 种不同的四边形。

            因此，我们 **强烈建议** 在进行文本搜索时使用 `quads` 参数，以确保标注的正确性。对于通过 "dict" 或 "rawdict" 选项从 :meth:`Page.get_text` 提取的 **文本片段** 进行标记时，类似的考虑也适用。有关如何计算此类四边形的更多细节，请参见 :ref:`FAQ` 中的“如何标记非水平文本”。

         :arg rect_like,quad_like,list,tuple quads:
            要标记的矩形或四边形的位置。（从 v1.14.20 开始更改）
            如果传递的是列表或元组，它们必须由 :data:`rect_like` 或 :data:`quad_like` 项组成（甚至可以是这两者的混合）。每个项必须是有限的、凸的且不为空（如果适用）。
            如果将该参数设置为 `None`，则必须使用以下参数（从 v1.16.14 开始）。
            反之亦然：如果不是 `None`，则其余参数必须是 `None`。

         :arg point_like start: 从此点开始标记文本。默认为 *clip* 的左上角。如果 `quads` 为 `None`，则必须提供该参数。（从 v1.16.14 新增）
         :arg point_like stop: 在此点停止文本标记。默认为 *clip* 的右下角。如果使用 `start` 参数，必须提供此参数。（从 v1.16.14 新增）
         :arg rect_like clip: 仅考虑与该区域相交的文本行。默认为页面矩形。如果提供了 `start` 和 `stop` 参数，则必须使用此参数。（从 v1.16.14 新增）

         :rtype: :ref:`Annot` 或  ``None`` （从 v1.16.14 开始更改）。
         :returns: 创建的标注。如果 *quads* 是空列表， **不会创建标注** （从 v1.16.14 开始更改）。

         .. note::
            您可以使用 `start`、`stop` 和 `clip` 参数在 `start` 和 `stop` 之间的连续行上进行标记（从 v1.16.14 开始）。
            使用 *clip* 来进一步缩小选定行的边界框，从而处理例如多列页面等问题。
            以下是一个多行标记示例，该标记使用了两个红点并适当设置了 *clip*。

         .. image:: images/img-markers.*
            :scale: 100


      .. method:: cluster_drawings(clip=None, drawings=None, x_tolerance=3, y_tolerance=3)

         根据几何相似性将矢量图形（同义词：线条艺术或绘图）进行聚类。该方法遍历 :meth:`Page.get_drawings` 的输出，连接那些 `path["rect"]` 彼此距离小于某些容差值的路径（由参数指定）。结果是一个矩形列表，每个矩形包裹着表格（带网格线）、饼图、条形图等。

         :arg rect_like clip: 仅考虑位于该区域内的路径。默认是整个页面。

         :arg list drawings: （可选）提供一个之前生成的 :meth:`Page.get_drawings` 输出。如果为 `None`，则该方法将执行 `Page.get_drawings`。

         :arg float x_tolerance: x 方向的容差值。

         :arg float y_tolerance: y 方向的容差值。

      .. method:: find_tables(clip=None, strategy=None, vertical_strategy=None, horizontal_strategy=None, vertical_lines=None, horizontal_lines=None, snap_tolerance=None, snap_x_tolerance=None, snap_y_tolerance=None, join_tolerance=None, join_x_tolerance=None, join_y_tolerance=None, edge_min_length=3, min_words_vertical=3, min_words_horizontal=1, intersection_tolerance=None, intersection_x_tolerance=None, intersection_y_tolerance=None, text_tolerance=None, text_x_tolerance=None, text_y_tolerance=None, add_lines=None)

         在页面中查找表格并返回一个包含相关信息的对象。通常，许多参数的默认值足够使用。调整仅在特殊情况中才需要。

         :arg rect_like clip: 指定页面矩形内要考虑的区域，忽略其余部分。默认是整个页面。

         :arg str strategy: 请求 **表格检测** 策略。有效值为 "lines"、"lines_strict" 和 "text"。
            
            默认值是 **"lines"**，使用页面上的所有矢量图形来检测网格线。
            
            **"lines_strict"** 策略忽略无边框的矩形矢量图形。有时，单个文本可能具有背景色，这可能导致错误的列或行。此策略会忽略它们，从而提高检测精度。
            
            如果指定了 **"text"**，则使用文本位置生成“虚拟”列和/或行边界。使用 `min_words_*` 参数来请求用于考虑其坐标的单词数量。
            
            使用参数 `vertical_strategy` 和 `horizontal_strategy` **代替** 以更精细地处理维度。

         :arg sequence[floats] horizontal_lines: 行的 y 坐标。如果提供，将不会尝试识别其他表格行。这会影响表格检测。

         :arg sequence[floats] vertical_lines: 列的 x 坐标。如果提供，将不会尝试识别其他表格列。这会影响表格检测。

         :arg int min_words_vertical: 适用于垂直策略选项 "text"：至少需要此数量的单词重合才能确定一个 **虚拟列** 边界。

         :arg int min_words_horizontal: 适用于水平策略选项 "text"：至少需要此数量的单词重合才能确定一个 **虚拟行** 边界。

         :arg float snap_tolerance: 任何两个水平线的 y 值之间的差异不超过该值时，将被 **吸附** 为一条线。对于垂直线也是如此。默认值为 3。可以为不同维度分别指定值，使用 `snap_x_tolerance` 和 `snap_y_tolerance`。

         :arg float join_tolerance: 任何两条线段的起始和结束点差异不超过此值时，将 **连接** 为一条线（单位：点）。默认值为 3。可以为不同维度分别指定值，使用 `join_x_tolerance` 和 `join_y_tolerance`。

         :arg float edge_min_length: 如果一条线的长度不超过此值（单位：点），则忽略此线。默认值为 3。

         :arg float intersection_tolerance: 当将线条组合为单元格边界时，正交线必须在此值（单位：点）范围内才算作交点。默认值为 3。可以为不同维度分别指定值，使用 `intersection_x_tolerance` 和 `intersection_y_tolerance`。

         :arg float text_tolerance: 字符只有在它们的距离不大于此值（单位：点）时才会组合成单词。默认值为 3。可以为不同维度分别指定值，使用 `text_x_tolerance` 和 `text_y_tolerance`。

         :arg tuple,list add_lines: 指定一组“线”（即一对 :data:`point_like` 对象）作为 **附加** 、"虚拟" 矢量图形。这些线可以帮助表格和/或单元格的检测，并且不会对检测策略产生其他影响。特别是，与参数 `horizontal_lines` 和 `vertical_lines` 相比，它们不会阻止使用其他方式检测行或列。这些线将被视为与“真实”矢量图形一样的线条，适用于连接、吸附、交点、最小长度和包含在 `clip` 矩形中的规则。类似地，非平行于坐标轴的线将被忽略。

         .. image:: images/img-findtables.*

         :returns: 一个 `TableFinder` 对象，具有以下重要属性：

            * `cells`: 一个列表，包含页面上识别出的所有表格单元格的边界框（所有表格）。每个单元格是一个 :data:`rect_like` 元组 `(x0, y0, x1, y1)` 坐标，或者为 `None`。
            * `tables`: 一个包含 `Table` 对象的列表。如果页面没有表格，则为 `[]`。可以通过该列表找到单个表格。`TableFinder` 对象本身也是其表格的序列。这意味着如果 `tabs` 是一个 `TableFinder` 对象，则表格 "n" 可以通过 `tabs.tables[n]` 或更简短的 `tabs[n]` 来获取。

            * `Table` 对象具有以下属性：

               * ``bbox``: 表格的边界框，作为 `(x0, y0, x1, y1)` 元组。
               * ``cells``: 表格单元格的边界框（元组列表）。单元格也可以是 `None`。
               * ``extract()``: 该方法返回每个表格单元格的文本内容，作为一个字符串的列表的列表。
               * ``to_markdown()``: 该方法将表格作为 **markdown 格式的字符串** 返回（兼容 Github）。支持的查看器可以将字符串渲染为表格。此输出经过优化，适用于 **小型标记** ，这对于 LLM/RAG 输入非常有利。Pandas DataFrame（参见 `to_pandas()` 方法）提供等效的 markdown 表格输出，但对于人眼而言更易读。
               * `to_pandas()`: 该方法将表格作为一个 `pandas <https://pypi.org/project/pandas/>`_ `DataFrame <https://pandas.pydata.org/docs/reference/frame.html>`_ 返回。DataFrames 是非常灵活的对象，允许进行大量的表格操作，并支持将表格输出到几乎 20 种常见格式，其中包括 Excel 文件、CSV、JSON、markdown 格式的表格等。`DataFrame.to_markdown()` 生成的 markdown 格式适用于 Github，优化了人类可读性。然而，该方法需要额外安装 `tabulate <https://pypi.org/project/tabulate/>`_ 包。
               * ``header``: 一个 `TableHeader` 对象，包含表格的头部信息。
               * ``col_count``: 表格列数的整数。
               * ``row_count``: 表格行数的整数。
               * ``rows``: 一个 `TableRow` 对象的列表，其中包含两个属性： ``bbox`` 是该行的边界框， `cells` 是该行包含的单元格列表。

            * `TableHeader` 对象具有以下属性：

               * ``bbox``: 头部的边界框。
               * `cells`: 包含每个列名的单元格的边界框列表。
               * `names`: 包含每个单元格文本的字符串列表，表示列名 —— 用于将表格导出为 Pandas DataFrame、markdown 等格式。
               * `external`: 一个布尔值，指示头部的边界框是否在表格主体之外（ `True` ）或不在之外（ `False` ）。表格头部从不由 `TableFinder` 逻辑识别。因此，如果 `external` 为 `True`，则头部单元格不属于任何由 `TableFinder` 识别的单元格。如果 `external == False`，则表格的第一行就是表头。

            请查看这些 `Jupyter notebooks <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/table-analysis>`_，它们涵盖了标准情况，例如一页上有多个表格，或跨多页连接表格片段。

            .. caution:: 
               
               `TableFinder` 对象以及它的所有表格的生命周期 **等同于页面的生命周期** 。如果页面对象被删除或重新分配，则所有表格将不再有效。

               唯一能够在页面失效后保持表格内容的方法是通过 `Table.to_markdown()` 、 `Table.to_pandas()` 或 `Table.extract()` 的副本（例如 `Table.extract()[:]`）来 **提取** 它。

            .. note::

               一旦表格通过 `to_pandas()` 提取为 **Pandas DataFrame**，就可以使用 **Pandas API** 将其转换为其他文件类型：

               - 表格转为 Markdown，使用 `to_markdown <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_markdown.html#pandas.DataFrame.to_markdown>`_
               - 表格转为 JSON，使用： `to_json <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_json.html>`_
               - 表格转为 Excel，使用： `to_excel <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_excel.html>`_
               - 表格转为 CSV，使用： `to_csv <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_csv.html>`_
               - 表格转为 HTML，使用： `to_html <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_html.html>`_
               - 表格转为 SQL，使用： `to_sql <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_sql.html>`_

         |history_begin|

         * 版本 1.23.0 新增
         * 版本 1.23.19 更改：新增参数 `add_lines`。

         |history_end|

         .. important::

            还有 `pdf2docx extract tables method`_ ，如果你更喜欢使用它进行表格提取。


      .. method:: add_stamp_annot(rect, stamp=0)

         仅适用于 PDF：添加类似“橡皮图章”的注释，例如表示文档的预定用途（如“草稿”、“机密”等）。

         :arg rect_like rect: 指定放置注释的矩形区域。

         :arg int stamp: 图章文本的 ID 编号。有关可用图章，请参见 :ref:`StampIcons`。

         .. note::

            * 图章的文本及其边框线将自动调整大小，并水平和垂直居中于给定的矩形中。 :attr:`Annot.rect` 会自动计算，以适应给定的 **宽度** ，通常会小于此参数。
            * 选用的字体为“Times Bold”，文本将以大写字母呈现。
            * 外观可以通过 :meth:`Annot.set_opacity` 和设置“描边”颜色进行更改（不支持“填充”颜色）。
            * 这可以用来创建水印图像：在临时 PDF 页面上创建一个图章注释并设置较低的不透明度值，使用 *alpha=True* 从中生成一个 pixmap（可能还需要旋转它），丢弃临时 PDF 页面，然后使用 :meth:`insert_image` 将 pixmap 插入目标 PDF。

         .. image:: images/img-stampannot.*
            :scale: 80

      .. method:: add_widget(widget)

         仅适用于 PDF：将 PDF 表单字段（“小部件”）添加到页面。这也 **将 PDF 转换为表单 PDF** 。由于小部件有大量不同的选项，我们开发了一个新的类 :ref:`Widget`，它包含所有可能的 PDF 字段属性。必须在创建和更新表单字段时使用该类。

         :arg widget: 一个已经创建的小部件 :ref:`Widget` 对象。
         :type widget: :ref:`Widget`

         :returns: 一个小部件注释。

      .. method:: delete_annot(annot)

         * 删除操作现在会包括任何绑定的“弹出”或响应注释及相关对象（在 v1.16.6 中更改）。

         仅适用于 PDF：删除页面上的注释并返回下一个注释。

         :arg annot: 要删除的注释。
         :type annot: :ref:`Annot`

         :rtype: :ref:`Annot`
         :returns: 删除后的下一个注释。请记住，物理删除需要将文件保存为新文件，并且垃圾收集设置为大于 0。

      .. method:: delete_widget(widget)

         仅适用于 PDF：删除页面上的表单字段并返回下一个小部件。

         :arg widget: 要删除的小部件。
         :type widget: :ref:`Widget`

         :rtype: :ref:`Widget`
         :returns: 删除后的下一个小部件。请记住，物理删除需要将文件保存为新文件，并且垃圾收集设置为大于 0。

         |history_begin|

         (v1.18.4 新增)

         |history_end|

      .. method:: delete_link(linkdict)

         仅适用于 PDF：删除页面上的指定链接。参数必须是 :meth:`get_links()` 提供的 **原始项** ，请参阅 :ref:`link_dict_description`。这是因为字典中的 *"xref"* 键标识了要删除的 PDF 对象。

         :arg dict linkdict: 要删除的链接。

      .. method:: insert_link(linkdict)

         仅适用于 PDF：在此页面上插入新链接。参数必须是 :meth:`get_links()` 提供的字典格式，请参阅 :ref:`link_dict_description`。

         :arg dict linkdict: 要插入的链接。

      .. method:: update_link(linkdict)

         仅适用于 PDF：修改指定的链接。参数必须是 :meth:`get_links()` 提供的（已修改的） **原始项** ，请参阅 :ref:`link_dict_description`。这是因为字典中的 *"xref"* 键标识了要更改的 PDF 对象。

         :arg dict linkdict: 要修改的链接。

         .. warning:: 
            
            如果更新/插入 URI 链接（ `"kind": LINK_URI` ），请确保 `"uri"` 键的值以如 `"http://"`, `"https://"`, `"file://"`, `"ftp://"`, `"mailto:"` 等消歧义字符串开始。否则，取决于您的浏览器或其他“消费者”软件，意外的默认假设可能导致不想要的行为。

      .. method:: get_label()

         仅适用于 PDF：返回页面的标签。

         :rtype: str

         :returns: 标签字符串，例如 Roman 编号的“vii”或如果未定义则为 ""。

         |history_begin|

         * v1.18.6 中新增

         |history_end|

      .. method:: get_links()

         获取页面的 **所有** 链接。

         :rtype: list
         :returns: 字典列表。有关字典条目的说明，请参见 :ref:`link_dict_description`。如果您打算更改页面的链接，请始终使用此方法或 :meth:`Page.links` 方法。

      .. method:: links(kinds=None)

         返回页面链接的生成器。结果与 :meth:`Page.get_links` 的条目相同。

         :arg sequence kinds: 一个整数序列，用于选择一个或多个链接类型。默认为所有链接。例如： *kinds=(pymupdf.LINK_GOTO,)* 将只返回内部链接。

         :rtype: generator
         :returns: 每次迭代返回 :meth:`Page.get_links()` 的一个条目。

         |history_begin|

         * v1.16.4 中新增

         |history_end|

      .. method:: annots(types=None)

         返回页面注释的生成器。

         :arg sequence types: 一个整数序列，用于选择一个或多个注释类型。默认为所有注释。例如：`types=(pymupdf.PDF_ANNOT_FREETEXT, pymupdf.PDF_ANNOT_TEXT)` 将只返回 'FreeText' 和 'Text' 注释。

         :rtype: generator
         :returns: 每次迭代返回一个 :ref:`Annot`。

            .. caution::
            
               您 **不能安全地在此生成器内更新注释** 。这是因为大多数注释更新需要通过 `page = doc.reload_page(page)` 重新加载页面。为了规避此限制，首先列出注释的 xref 编号，然后遍历这些编号::

                  In [4]: xrefs = [annot.xref for annot in page.annots(types=[...])]
                  In [5]: for xref in xrefs:
                     ...:     annot = page.load_annot(xref)
                     ...:     annot.update()
                     ...:     page = doc.reload_page(page)
                  In [6]:

         |history_begin|

         * v1.16.4 中新增

         |history_end|

      .. method:: widgets(types=None)

         返回页面表单字段的生成器。

         :arg sequence types: 一个整数序列，用于选择一个或多个小部件类型。默认为所有表单字段。例如： `types=(pymupdf.PDF_WIDGET_TYPE_TEXT,)` 将只返回 'Text' 字段。

         :rtype: generator
         :returns: 每次迭代返回一个 :ref:`Widget`。

         |history_begin|

         * v1.16.4 中新增

         |history_end|

      .. method:: write_text(rect=None, writers=None, overlay=True, color=None, opacity=None, keep_proportion=True, rotate=0, oc=0)

         仅适用于 PDF：将一个或多个 :ref:`Textwriter` 对象的文本写入页面。

         :arg rect_like rect: 文本放置的位置。如果省略，则使用文本写入器的矩形联合区域。
         :arg sequence writers: 非空的 :ref:`TextWriter` 对象元组/列表，或单个 :ref:`TextWriter`。
         :arg float opacity: 设置透明度，覆盖文本写入器中的相应值。
         :arg sequence color: 设置文本颜色，覆盖文本写入器中的相应值。
         :arg bool overlay: 将文本放置在前景还是背景中。
         :arg bool keep_proportion: 保持纵横比。
         :arg float rotate: 以任意角度旋转文本。
         :arg int oc: :data:`OCG` 或 :data:`OCMD` 的 :data:`xref`。（v1.18.4 中新增）

         .. note:: 
            
            参数 *overlay, keep_proportion, rotate* 和 *oc* 的含义与 :meth:`Page.show_pdf_page` 中相同。

         |history_begin|

         * v1.16.18 中新增

         |history_end|

      .. index::
         pair: border_width; insert_text
         pair: color; insert_text
         pair: encoding; insert_text
         pair: fill; insert_text
         pair: fontfile; insert_text
         pair: fontname; insert_text
         pair: fontsize; insert_text
         pair: morph; insert_text
         pair: overlay; insert_text
         pair: render_mode; insert_text
         pair: miter_limit; insert_text
         pair: rotate; insert_text
         pair: stroke_opacity; insert_text
         pair: fill_opacity; insert_text
         pair: oc; insert_text

      .. method:: insert_text(point, text, *, fontsize=11, fontname="helv", fontfile=None, idx=0, color=None, fill=None, render_mode=0, miter_limit=1, border_width=0.05, encoding=TEXT_ENCODING_LATIN, rotate=0, morph=None, stroke_opacity=1, fill_opacity=1, overlay=True, oc=0)

         仅适用于 PDF：从 :data:`point_like` 类型的 ``point`` 开始插入文本行。参见 :meth:`Shape.insert_text`。

         |history_begin|

         * v1.18.4 中更改

         |history_end|

      .. index::
         pair: align; insert_textbox
         pair: border_width; insert_textbox
         pair: color; insert_textbox
         pair: encoding; insert_textbox
         pair: expandtabs; insert_textbox
         pair: fill; insert_textbox
         pair: fontfile; insert_textbox
         pair: fontname; insert_textbox
         pair: fontsize; insert_textbox
         pair: morph; insert_textbox
         pair: overlay; insert_textbox
         pair: render_mode; insert_textbox
         pair: miter_limit; insert_textbox
         pair: rotate; insert_textbox
         pair: stroke_opacity; insert_textbox
         pair: fill_opacity; insert_textbox
         pair: oc; insert_textbox

      .. method:: insert_textbox(rect, buffer, *, fontsize=11, fontname="helv", fontfile=None, idx=0, color=None, fill=None, render_mode=0, miter_limit=1, border_width=1, encoding=TEXT_ENCODING_LATIN, expandtabs=8, align=TEXT_ALIGN_LEFT, charwidths=None, rotate=0, morph=None, stroke_opacity=1, fill_opacity=1, oc=0, overlay=True)

         **仅适用于 PDF：** 将文本插入到指定的 :data:`rect_like` *rect* 中。参见 :meth:`Shape.insert_textbox`。

         |history_begin|

         * v1.18.4 中进行了修改。

         |history_end|

      .. index::
         pair: rect; insert_htmlbox
         pair: text; insert_htmlbox
         pair: css; insert_htmlbox
         pair: adjust; insert_htmlbox
         pair: archive; insert_htmlbox
         pair: overlay; insert_htmlbox
         pair: rotate; insert_htmlbox
         pair: oc; insert_htmlbox
         pair: opacity; insert_htmlbox
         pair: morph; insert_htmlbox
      
      .. method:: insert_htmlbox(rect, text, *, css=None, scale_low=0, archive=None, rotate=0, oc=0, opacity=1, overlay=True)

         **仅适用于 PDF：** 将文本插入到指定的矩形区域。此方法与 :meth:`Page.insert_textbox` 和 :meth:`TextWriter.fill_textbox` 方法类似，但 **功能更强大** 。这是通过让 :ref:`Story` 对象完成所有必要的处理来实现的。

         * 参数 ``text`` 可以是一个字符串，正如其他方法中一样。但它将 **作为 HTML 源代码进行解释** ，因此也可以包含 HTML 语言元素——包括样式。可以使用 ``css`` 参数传递附加的样式指令。

         * 自动换行会在单词边界处生成。字符 `"&#173;"` （或 `&shy;` ）可用于触发连字符，因此也可能导致换行。然而， **强制性** 换行只能通过 HTML 标签 ``<br>`` 来实现， ``\n`` 会被忽略并当作空格处理。

         * 使用此方法，可以实现以下功能：

            - 样式效果，如粗体、斜体、文本颜色、文本对齐、字体大小或字体切换。
            - 文本可以包含任意语言—— **包括从右到左的语言** 。
            - 像 `Devanagari <https://en.wikipedia.org/wiki/Devanagari>`_ 等亚洲地区的脚本有一个复杂的连字系统，其中两个或更多的 Unicode 字符组合成一个字形。Story 使用软件包 `HarfBuzz <https://harfbuzz.github.io/>`_ 来处理这些内容并生成正确的输出。
            - 还可以通过 HTML 标签 `<img>` 包含图像——Story 将处理适当的布局。这是插入图像的替代方法，相较于 :meth:`Page.insert_image` 。
            - HTML 表格（ `<table>` 标签 ）可以包含在文本中，并将被适当处理。
            - 自动生成链接。

         * 如果内容未能适应矩形区域，开发者有两个选择：

            - **或者** 仅被告知此情况（并接受无操作，就像其他文本框插入方法一样），
            - **或者** （`scale_low=0` - 默认值）缩放内容直到适应。

         :arg rect_like rect: 页面上用于接收文本的矩形区域。
         :arg str,Story text: 要插入的文本。可以包含纯文本和 HTML 标签混合的内容，包含样式指令。或者，可以指定一个 :ref:`Story` 对象（此时将省略内部的 Story 生成步骤）。必须使用所有所需的样式和归档信息生成 Story。
         :arg str css: 可选的附加 CSS 指令字符串。如果 ``text`` 是一个 Story，此参数将被忽略。
         :arg float scale_low: 如果需要，将内容缩小直到适应目标矩形。这设置了下缩放的限制。默认值为 0，无限制。值为 1 表示不允许缩小。例如，0.2 表示最多缩小 80%。
         :arg Archive archive: 指向图像或非标准字体所在位置的 Archive 对象。如果 ``text`` 中引用了图像或非标准字体，则此参数是必需的。如果 ``text`` 是一个 Story，则此参数将被忽略。
         :arg int rotate: 值可以为 0、90、180 或 270。根据该值，文本将按以下方式填充：

               - 0: 从左上到右下。
               - 90: 从左下到右上。
               - 180: 从右下到左上。
               - 270: 从右上到左下。

               .. image:: images/img-rotate.*

         :arg int oc: :data:`OCG` / :data:`OCMD` 的 xref 或 0。有关详细信息，请参阅 :meth:`Page.show_pdf_page`。
         :arg float opacity: 设置填充和描边的不透明度。仅考虑 `0 <= opacity < 1` 的值。
         :arg bool overlay: 将文本置于其他内容的前面。请参阅 :meth:`Page.show_pdf_page` 了解详细信息。

         :returns: 一个包含浮动数值的元组 `(spare_height, scale)`。

            - `spare_height`: 如果内容未适应，则为 -1，否则大于等于 0。它是未使用（仍然可用）矩形条带的高度。当 `scale = 1` （没有缩放）时，它才会为正。
            - `scale`: 缩放因子， `0 < scale <= 1` 。

            请参阅本节的示例： :ref:`RecipesText_I_c` 。

         |history_begin|

         * 新增于 v1.23.8；仅重构。
         * 新增于 v1.23.9: `opacity` 参数。

         |history_end|

      **Drawing Methods**

      .. index::
         pair: closePath; draw_line
         pair: color; draw_line
         pair: dashes; draw_line
         pair: fill; draw_line
         pair: lineCap; draw_line
         pair: lineJoin; draw_line
         pair: lineJoin; draw_line
         pair: morph; draw_line
         pair: overlay; draw_line
         pair: width; draw_line
         pair: stroke_opacity; draw_line
         pair: fill_opacity; draw_line
         pair: oc; draw_line

      .. method:: draw_line(p1, p2, color=(0,), width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         **仅适用于 PDF：** 从 *p1* 到 *p2* (:data:`point_like` \s) 绘制一条直线。参见 :meth:`Shape.draw_line`。

         |history_begin|

         * v1.18.4 中进行了修改。

         |history_end|

      .. index::
         pair: breadth; draw_zigzag
         pair: closePath; draw_zigzag
         pair: color; draw_zigzag
         pair: dashes; draw_zigzag
         pair: fill; draw_zigzag
         pair: lineCap; draw_zigzag
         pair: lineJoin; draw_zigzag
         pair: morph; draw_zigzag
         pair: overlay; draw_zigzag
         pair: width; draw_zigzag
         pair: stroke_opacity; draw_zigzag
         pair: fill_opacity; draw_zigzag
         pair: oc; draw_zigzag

      .. method:: draw_zigzag(p1, p2, breadth=2, color=(0,), width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         **仅适用于 PDF：** 从 *p1* 到 *p2* ( :data:`point_like` \s) 绘制一条锯齿形线。参见 :meth:`Shape.draw_zigzag`。

         |history_begin|

         * v1.18.4 中进行了修改。

         |history_end|

      .. index::
         pair: breadth; draw_squiggle
         pair: closePath; draw_squiggle
         pair: color; draw_squiggle
         pair: dashes; draw_squiggle
         pair: fill; draw_squiggle
         pair: lineCap; draw_squiggle
         pair: lineJoin; draw_squiggle
         pair: morph; draw_squiggle
         pair: overlay; draw_squiggle
         pair: width; draw_squiggle
         pair: stroke_opacity; draw_squiggle
         pair: fill_opacity; draw_squiggle
         pair: oc; draw_squiggle

      .. method:: draw_squiggle(p1, p2, breadth=2, color=(0,), width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         **仅适用于 PDF：** 从 *p1* 到 *p2* ( :data:`point_like` \s) 绘制一条波浪形（弯曲、起伏）的线。参见 :meth:`Shape.draw_squiggle`。

         |history_begin|

         * v1.18.4 中进行了修改。

         |history_end|

      .. index::
         pair: closePath; draw_circle
         pair: color; draw_circle
         pair: dashes; draw_circle
         pair: fill; draw_circle
         pair: lineCap; draw_circle
         pair: lineJoin; draw_circle
         pair: morph; draw_circle
         pair: overlay; draw_circle
         pair: width; draw_circle
         pair: stroke_opacity; draw_circle
         pair: fill_opacity; draw_circle
         pair: oc; draw_circle

      .. method:: draw_circle(center, radius, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         **仅适用于 PDF：** 以 *center* (:data:`point_like`) 为中心，半径为 *radius* 绘制一个圆。参见 :meth:`Shape.draw_circle`。

         |history_begin|

         * v1.18.4 中进行了修改。

         |history_end|

      .. index::
         pair: closePath; draw_oval
         pair: color; draw_oval
         pair: dashes; draw_oval
         pair: fill; draw_oval
         pair: lineCap; draw_oval
         pair: lineJoin; draw_oval
         pair: morph; draw_oval
         pair: overlay; draw_oval
         pair: width; draw_oval
         pair: stroke_opacity; draw_oval
         pair: fill_opacity; draw_oval
         pair: oc; draw_oval

      .. method:: draw_oval(quad, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         **仅适用于 PDF：** 在给定的 :data:`rect_like` 或 :data:`quad_like` 内绘制一个椭圆 (ellipse)。参见 :meth:`Shape.draw_oval`。

         |history_begin|

         * v1.18.4 中进行了修改。

         |history_end|

      .. index::
         pair: closePath; draw_sector
         pair: color; draw_sector
         pair: dashes; draw_sector
         pair: fill; draw_sector
         pair: fullSector; draw_sector
         pair: lineCap; draw_sector
         pair: lineJoin; draw_sector
         pair: morph; draw_sector
         pair: overlay; draw_sector
         pair: width; draw_sector
         pair: stroke_opacity; draw_sector
         pair: fill_opacity; draw_sector
         pair: oc; draw_sector

      .. method:: draw_sector(center, point, angle, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, fullSector=True, overlay=True, closePath=False, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         **仅适用于 PDF：** 绘制一个圆形扇形， 可选地将弧连接到圆心（类似于一块披萨）。参见 :meth:`Shape.draw_sector`。

         |history_begin|

         * v1.18.4 中进行了修改。

         |history_end|

      .. index::
         pair: closePath; draw_polyline
         pair: color; draw_polyline
         pair: dashes; draw_polyline
         pair: fill; draw_polyline
         pair: lineCap; draw_polyline
         pair: lineJoin; draw_polyline
         pair: morph; draw_polyline
         pair: overlay; draw_polyline
         pair: width; draw_polyline
         pair: stroke_opacity; draw_polyline
         pair: fill_opacity; draw_polyline
         pair: oc; draw_polyline

      .. method:: draw_polyline(points, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, closePath=False, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         **仅适用于 PDF：** 绘制由一系列 :data:`point_like` 点定义的多个连接线。参见 :meth:`Shape.draw_polyline`。

         |history_begin|

         * v1.18.4 中进行了修改。

         |history_end|

      .. index::
         pair: closePath; draw_bezier
         pair: color; draw_bezier
         pair: dashes; draw_bezier
         pair: fill; draw_bezier
         pair: lineCap; draw_bezier
         pair: lineJoin; draw_bezier
         pair: morph; draw_bezier
         pair: overlay; draw_bezier
         pair: width; draw_bezier
         pair: stroke_opacity; draw_bezier
         pair: fill_opacity; draw_bezier
         pair: oc; draw_bezier

      .. method:: draw_bezier(p1, p2, p3, p4, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, closePath=False, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         **仅适用于 PDF：** 绘制从 *p1* 到 *p4* 的三次 Bézier 曲线，控制点为 *p2* 和 *p3* （所有点为 :data:`point_like`）。参见 :meth:`Shape.draw_bezier`。

         |history_begin|

         * v1.18.4 中进行了修改。

         |history_end|

      .. index::
         pair: closePath; draw_curve
         pair: color; draw_curve
         pair: dashes; draw_curve
         pair: fill; draw_curve
         pair: lineCap; draw_curve
         pair: lineJoin; draw_curve
         pair: morph; draw_curve
         pair: overlay; draw_curve
         pair: width; draw_curve
         pair: stroke_opacity; draw_curve
         pair: fill_opacity; draw_curve
         pair: oc; draw_curve

      .. method:: draw_curve(p1, p2, p3, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, closePath=False, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         **仅适用于 PDF：** 这是 *draw_bezier()* 的特殊情况。参见 :meth:`Shape.draw_curve`。

         |history_begin|

         * v1.18.4 中进行了修改。

         |history_end|

      .. index::
         pair: closePath; draw_rect
         pair: color; draw_rect
         pair: dashes; draw_rect
         pair: fill; draw_rect
         pair: lineCap; draw_rect
         pair: lineJoin; draw_rect
         pair: morph; draw_rect
         pair: overlay; draw_rect
         pair: width; draw_rect
         pair: stroke_opacity; draw_rect
         pair: fill_opacity; draw_rect
         pair: radius; draw_rect
         pair: oc; draw_rect

      .. method:: draw_rect(rect, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, radius=None, oc=0)

         **仅适用于 PDF：** 绘制一个矩形。参见 :meth:`Shape.draw_rect`。

         |history_begin|

         * v1.18.4 中进行了修改。
         * v1.22.0 中进行了修改：新增了 *radius* 参数。

         |history_end|

      .. index::
         pair: closePath; draw_quad
         pair: color; draw_quad
         pair: dashes; draw_quad
         pair: fill; draw_quad
         pair: lineCap; draw_quad
         pair: lineJoin; draw_quad
         pair: morph; draw_quad
         pair: overlay; draw_quad
         pair: width; draw_quad
         pair: stroke_opacity; draw_quad
         pair: fill_opacity; draw_quad
         pair: oc; draw_quad

      .. method:: draw_quad(quad, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         **仅适用于 PDF：** 绘制一个四边形。参见 :meth:`Shape.draw_quad`。

         |history_begin|

         * v1.18.4 中进行了修改。

         |history_end|

      .. index::
         pair: encoding; insert_font
         pair: fontbuffer; insert_font
         pair: fontfile; insert_font
         pair: fontname; insert_font
         pair: set_simple; insert_font

      .. method:: insert_font(fontname="helv", fontfile=None, fontbuffer=None, set_simple=False, encoding=TEXT_ENCODING_LATIN)

         **仅适用于 PDF：** 添加一个新的字体供文本输出方法使用，并返回其 :data:`xref`。如果字体文件尚未在文件中定义，则该字体定义将被添加。支持内建的 :data:`Base14_Fonts` 和通过 **"reserved"** 字体名的 CJK 字体。字体也可以通过文件路径或包含字体文件图像的内存区域提供。

         :arg str fontname: 在输出文本时，使用此字体的引用名称。通常，您可以自由选择名称（但请参考 :ref:`AdobeManual`，第16页，第7.3.5节，了解构建合法 PDF 名称的正式描述）。然而，如果它与 :data:`Base14_Fonts` 中的某个字体或某个 CJK 字体相匹配，则 *fontfile* 和 *fontbuffer* **将被忽略**。

            换句话说，您不能通过 *fontfile* / *fontbuffer* 插入字体并同时指定一个保留的 *fontname*。

            .. note:: 
               
               保留的字体名可以以任意大小写指定，并且仍然匹配正确的内建字体定义：字体名 "helv"、"Helv"、"HELV"、"Helvetica" 等都将映射到相同的字体定义 "Helvetica"。但是，从 :ref:`Page` 角度来看，这些是 **不同的引用**。您可以利用这一点，在页面上使用相同字体的不同 *encoding* 变体（拉丁文、希腊文、斯拉夫文）。

         :arg str fontfile: 字体文件的路径。如果使用此参数，则 *fontname* 必须 **与所有保留名称不同** 。

         :arg bytes/bytearray fontbuffer: 字体文件的内存图像。如果使用此参数，则 *fontname* 必须 **与所有保留名称不同**。此参数通常与 :attr:`Font.buffer` 一起使用，适用于通过 :ref:`Font` 支持/可用的字体。

         :arg int set_simple: 仅适用于 *fontfile* / *fontbuffer* 的情况：强制将字体视为 "简单" 字体，即仅使用字符代码 255 以内的字符。

         :arg int encoding: 仅适用于 "Helvetica"、"Courier" 和 "Times" 等 :data:`Base14_Fonts` 字体集。选择以下可用的编码方式：拉丁文 (0)、斯拉夫文 (2) 或希腊文 (1)。仅对 "Symbol" 和 "ZapfDingBats" 使用默认值 (0 = 拉丁文)。

         :returns: 安装的字体的 :data:`xref`。

         .. note:: 
            
            内建字体不会导致字体文件的包含，因此生成的 PDF 文件将保持较小。然而，您的 PDF 查看器软件负责生成适当的显示效果 —— 并且 **确实存在** 各种差异，取决于每个查看器如何处理这些字体，尤其是 CJK 字体。此外，Symbol 和 ZapfDingbats 字体在某些情况下也会被错误处理。以下是 **字体名称** 及其对应的 **安装字体** 名称：

            **Base-14 字体** [#f1]_

            ============= ============================ =========================================
            **字体名称**  **已安装的字体**             **备注**    
            ============= ============================ =========================================
            helv          Helvetica                    normal
            heit          Helvetica-Oblique            italic
            hebo          Helvetica-Bold               bold
            hebi          Helvetica-BoldOblique        bold-italic
            cour          Courier                      normal
            coit          Courier-Oblique              italic
            cobo          Courier-Bold                 bold
            cobi          Courier-BoldOblique          bold-italic
            tiro          Times-Roman                  normal
            tiit          Times-Italic                 italic
            tibo          Times-Bold                   bold
            tibi          Times-BoldItalic             bold-italic
            symb          Symbol                       [#f3]_
            zadb          ZapfDingbats                 [#f3]_
            ============= ============================ =========================================

            **CJK 字体** [#f2]_ （中国、日本、韩国）

            ============= ============================ =========================================
            **字体名称**  **已安装的字体**             **备注**    
            ============= ============================ =========================================
            china-s       Heiti                        简体中文
            china-ss      Song                         简体中文（衬线）
            china-t       Fangti                       繁体中文
            china-ts      Ming                         繁体中文（衬线）
            japan         Gothic                       日文
            japan-s       Mincho                       日文（衬线）
            korea         Dotum                        韩文
            korea-s       Batang                       韩文（衬线）
            ============= ============================ =========================================

      .. index::
         pair: filename; insert_image
         pair: keep_proportion; insert_image
         pair: overlay; insert_image
         pair: pixmap; insert_image
         pair: rotate; insert_image
         pair: stream; insert_image
         pair: mask; insert_image
         pair: oc; insert_image
         pair: xref; insert_image

      .. method:: insert_image(rect, *, alpha=-1, filename=None, height=0, keep_proportion=True, mask=None, oc=0, overlay=True, pixmap=None, rotate=0, stream=None, width=0, xref=0)

         **仅适用于 PDF：** 将图像放入给定的矩形区域。图像可以是已经存在于 PDF 中的，或者来自像素图、文件或内存区域。

         :arg rect_like rect: 图像放置的区域。必须是有限的且非空的。
         :arg int alpha: 已废弃并被忽略。
         :arg str filename:
            图像文件的名称（MuPDF 支持的所有格式，详见 :ref:`ImageFiles`）。
         :arg int height:
         :arg bool keep_proportion:
            保持图像的宽高比。
         :arg bytes,bytearray,io.BytesIO mask:
            图像内存 -- 用作基础图像的图像掩码（alpha 值）。指定时，基础图像必须通过文件名或流提供，并且不能是已有掩码的图像。
         :arg int oc:
            (:data:`xref`) 使图像的可见性依赖于此 :data:`OCG` 或 :data:`OCMD`。多个插入后的第一次被忽略。该属性与生成的 PDF 图像对象一起存储，从而控制图像在整个 PDF 中的可见性。
         :arg overlay: 参见 :ref:`CommonParms`。
         :arg pixmap: 包含图像的像素图。
         :arg int rotate: 旋转图像。
            必须是 90 度的整数倍。
            正值表示逆时针旋转。
            如果需要任意角度的旋转，考虑先将图像转换为 PDF
            (:meth:`Document.convert_to_pdf`)，然后使用 :meth:`Page.show_pdf_page`。
         :arg bytes,bytearray,io.BytesIO stream:
            图像内存（MuPDF 支持的所有格式，详见 :ref:`ImageFiles`）。
         :arg int width:
         :arg int xref:
            已存在于 PDF 中图像的 :data:`xref`。如果给定，则忽略参数 `filename`、`pixmap`、`stream`、`alpha` 和 `mask`。页面将仅接收对现有图像的引用。

         :type pixmap: :ref:`Pixmap`
         
         :returns:
            嵌入图像的 `xref`。如果再次插入相同图像，这可以作为 `xref` 参数，极大提高性能。

         该示例在文档的每一页上插入相同的图像::

            >>> doc = pymupdf.open(...)
            >>> rect = pymupdf.Rect(0, 0, 50, 50)       # 将缩略图放在左上角
            >>> img = open("some.jpg", "rb").read()  # 图像文件
            >>> img_xref = 0                         # 第一次执行嵌入图像
            >>> for page in doc:
                  img_xref = page.insert_image(rect, stream=img,
                              xref=img_xref,  # 第二次执行时重用现有图像
                        )
            >>> doc.save(...)

         .. note::

            1.
               该方法会检测相同图像的多次插入（如上例所示），并且只会在第一次执行时存储其数据。即便使用默认的 `xref=0`，这一点仍然适用（虽然效率较低）。
            2.
               该方法无法检测图像是否已经是文件中打开之前的一部分。

            3.
               您可以使用此方法为页面提供背景或前景图像，例如版权或水印。请记住，水印需要透明图像，如果放在前景中 ...

            4.
               图像可能以未压缩的形式插入，例如，如果使用了 `Pixmap` 或图像具有 alpha 通道。因此，保存文件时请考虑使用 `deflate=True`。此外，还有方法可以控制图像大小 -- 即使透明性涉及其中。请参见 :ref:`RecipesImages_O`。

            5.
               图像在 PDF 中按原始质量级别存储。这可能比显示所需的要高得多。在插入之前考虑 **减小图像大小** ，例如通过使用像素图选项，然后将其缩小或缩放（参见 :ref:`Pixmap` 章节）。PIL 方法 `Image.thumbnail()` 也可以用于此目的。文件大小节省可能会非常显著。

            6.
               另一种有效的方法是通过 :meth:`show_pdf_page` 在多个页面上显示相同的图像。请参考 :meth:`Document.convert_to_pdf`，了解如何获取适用于该方法的中介 PDF。

      .. index::
         pair: filename; replace_image
         pair: pixmap; replace_image
         pair: stream; replace_image
         pair: xref; replace_image

      .. method:: replace_image(xref, filename=None, pixmap=None, stream=None)

         替换位于指定 `xref` 的图像。

         :arg int xref: 图像的 :data:`xref`。
         :arg filename: 新图像的文件名。
         :arg pixmap: 新图像的 :ref:`Pixmap`。
         :arg stream: 包含新图像的内存区域。

         参数 `filename`、`pixmap`、`stream` 与 :meth:`Page.insert_image` 中的意义相同，特别是必须提供其中之一。

         这是一个 **全局替换：** 新图像将出现在文件中所有旧图像显示的地方。

         该方法主要用于技术目的。典型用途包括通过较小的版本替换较大的图像，例如低分辨率图像，或替换颜色图像为灰度图像等，或者更改透明度。

         |history_begin|

         * 新增于 v1.21.0

         |history_end|

      .. index::
         pair: xref; delete_image

      .. method:: delete_image(xref)

         删除位于指定 `xref` 的图像。这个方法有些误导：实际上，图像会被一个小的透明 :ref:`Pixmap` 替换，使用上述的 :meth:`Page.replace_image` 方法。可见效果是等效的。

         :arg int xref: 图像的 :data:`xref`。

         这是一个 **全局替换：** 该图像会在文件中所有显示它的地方消失。

         如果通过 :meth:`Page.get_images`、:meth:`Page.get_image_info` 或 :meth:`Page.get_text` 等方法检查或提取页面图像，
         替换后的“虚拟”图像将会被检测为 `(45, 47, 1, 1, 8, 'DeviceGray', '', 'Im1', 'FlateDecode')`，并且似乎会“覆盖”页面上的相同边界框。

         |history_begin|

         * 新增于 v1.21.0

         |history_end|
   
      .. index::
         pair: blocks; Page.get_text
         pair: dict; Page.get_text
         pair: clip; Page.get_text
         pair: flags; Page.get_text
         pair: html; Page.get_text
         pair: json; Page.get_text
         pair: rawdict; Page.get_text
         pair: text; Page.get_text
         pair: words; Page.get_text
         pair: xhtml; Page.get_text
         pair: xml; Page.get_text
         pair: textpage; Page.get_text
         pair: sort; Page.get_text
         pair: delimiters; Page.get_text

      .. method:: get_text(option, *, clip=None, flags=None, textpage=None, sort=False, delimiters=None)

         获取页面内容的多种格式。根据 ``flags`` 的值，返回的内容可能包括文本、图像以及其他几种对象类型。该方法是对多个 :ref:`TextPage` 方法的封装，通过选择输出选项 `opt` 来指定格式，具体如下：

         * "text" -- :meth:`TextPage.extractTEXT`，默认。仅包含 **文本**。
         * "blocks" -- :meth:`TextPage.extractBLOCKS`。包括文本，并 **可能** 包括图像的元信息。
         * "words" -- :meth:`TextPage.extractWORDS`。仅包含 **文本**。
         * "html" -- :meth:`TextPage.extractHTML`。可能包含文本和图像。
         * "xhtml" -- :meth:`TextPage.extractXHTML`。可能包含文本和图像。
         * "xml" -- :meth:`TextPage.extractXML`。仅包含 **文本**。
         * "dict" -- :meth:`TextPage.extractDICT`。可能包含文本和图像。
         * "json" -- :meth:`TextPage.extractJSON`。可能包含文本和图像。
         * "rawdict" -- :meth:`TextPage.extractRAWDICT`。可能包含文本和图像。
         * "rawjson" -- :meth:`TextPage.extractRAWJSON`。可能包含文本和图像。

         :arg str opt: 请求的格式的字符串，选项之一。如拼写错误，则默认为 "text"。

         :arg rect-like clip: 限制提取内容为此矩形区域。如果为 ``None`` （默认值），则提取页面的可见部分。任何 **不完全包含** 在 ``clip`` 中的内容（文本、图像）将被完全忽略。要完全避免裁剪，可以使用 ``clip=pymupdf.INFINITE_RECT()``。仅在 "html"、"xhtml" 和 "xml" 选项中，此参数无效。

         :arg int flags: 控制是否包括图像或文本如何处理空格和 :data:`ligatures` 的标志位。详细信息参见 :ref:`TextPreserve` 和 :ref:`text_extraction_flags`。 (v1.16.2 新增)

         :arg textpage: 使用先前创建的 :ref:`TextPage`。这将显著减少执行时间 **超过 50%**，最大可达 **95%**，具体取决于提取选项。如果指定了此参数，则忽略 `flags` 和 `clip` 参数，因为它们是专属于 textpage 的属性。如果未指定，则会创建一个新的临时 textpage。

         :arg bool sort: 按垂直坐标然后水平坐标对输出进行排序。在大多数情况下，这应该足以生成“自然”的阅读顺序。对于选项 "blocks"、"dict"、"json"、"rawdict"、"rawjson" 来说，排序是基于各自块的 bbox 坐标 `(y1, x0)`。对于选项 "words" 和 "text" 来说，文本行会被完全重新合成，以遵循文档中的阅读顺序和显示顺序，这在某种程度上会恢复原始布局。

         :arg str delimiters: 使用这些字符作为 *附加* 的单词分隔符，仅在 "words" 输出选项中有效（其他情况下忽略）。默认情况下，所有空白字符（包括不换行的空格 `0xA0`）表示单词的开始和结束。现在，您可以指定更多字符来作为分隔符。例如，默认情况下 `"john.doe@outlook.com"` 会作为 **一个** 单词返回。如果指定 `delimiters="@."`，则会返回四个单词 `"john"`、`"doe"`、`"outlook"`、`"com"`。其他用途包括忽略标点字符 `delimiters=string.punctuation`。返回的“单词”字符串将不包含分隔符字符。（v1.23.5 新增）

         :rtype: *str, list, dict*
         :returns: 页面内容，以字符串、列表或字典形式返回。具体请参考对应的 :ref:`TextPage` 方法。

         .. note::

            1. 您可以将此方法用作 **文档转换工具**，将 :ref:`任何支持的文档类型<Supported_File_Types>` 转换为 TEXT、HTML、XHTML 或 XML 格式的文档。
            2. 通过 *clip* 参数包含的文本是基于字符级别决定的：只有当字符的 bbox 被完全包含在 `clip` 内时，该字符才会成为输出的一部分。这与涂抹注释中使用的算法不同：如果字符的 bbox 与任何涂抹注释相交，该字符将被 **移除**。

         |history_begin|

         * 新增于 v1.19.0: 增加了 `textpage` 参数
         * 新增于 v1.19.1: 增加了 `sort` 参数
         * 新增于 v1.19.6: 增加了用于定义每个方法默认标志的常量
         * 新增于 v1.23.5: 增加了 `delimiters` 参数
         * 更改于 v1.24.11: 更改了 `sort=True` 对 "text" 和 "words" 选项的效果，使其更接近自然的阅读顺序。

         |history_end|

      .. index::
         pair: rect; get_textbox
         pair: textpage; get_textbox

      .. method:: get_textbox(rect, textpage=None)

         获取矩形区域内的文本。

         :arg rect-like rect: 矩形区域。
         :arg textpage: 用于提取的 :ref:`TextPage`。如果未指定，将创建一个新的临时 textpage。

         :returns: 一个包含必要换行符的字符串。此结果基于专门的代码（v1.19.0 更改）。一个典型的用法是检查 :meth:`Page.search_for` 的结果：

            >>> rl = page.search_for("currency:")
            >>> page.get_textbox(rl[0])
            'Currency:'
            >>>

         |history_begin|

         * 新增于 v1.17.7
         * 更改于 v1.19.0: 增加了 `textpage` 参数

         |history_end|

      .. index::
         pair: flags; get_textpage
         pair: clip; get_textpage

      .. method:: get_textpage(clip=None, flags=3)

         为页面创建一个 :ref:`TextPage`。

         :arg int flags: 控制后续文本提取和搜索可用内容的标志位 —— 参见 :meth:`Page.get_text` 的参数说明。

         :arg rect-like clip: 限制提取的文本范围到此区域。（v1.17.7 新增）

         :returns: :ref:`TextPage`

         |history_begin|

         * 新增于 v1.16.5
         * 更改于 v1.17.7: 引入了 `clip` 参数。

         |history_end|

      .. index::
         pair: flags; get_textpage_ocr
         pair: language; get_textpage_ocr
         pair: dpi; get_textpage_ocr
         pair: full; get_textpage_ocr
         pair: tessdata; get_textpage_ocr

      .. method:: get_textpage_ocr(flags=3, language="eng", dpi=72, full=False, tessdata=None)

         **光学字符识别** （ **OCR** ）技术可用于从仅包含栅格图像格式文本的文档中提取文本数据。使用此方法对页面进行 **OCR** 处理以提取文本。

         该方法返回一个包含 OCR 识别文本的 :ref:`TextPage`。如果调用此方法，MuPDF 将调用 Tesseract-OCR，否则返回的对象与普通的 :ref:`TextPage` 相同。

         :arg int flags: 控制后续文本提取和搜索可用内容的标志位 —— 参见 :meth:`Page.get_text` 的参数说明。
         :arg str language: 预期的语言。若需要识别多种语言，可使用 `+` 连接，例如 `"eng+spa"` 表示英文和西班牙文。
         :arg int dpi: 期望的分辨率（每英寸点数，DPI）。该参数影响识别质量（及执行时间）。
         :arg bool full: 是否对整页进行 OCR，还是仅处理页面中的图像部分。
         :arg str tessdata: Tesseract 语言数据文件 `tessdata` 的文件夹名称。如果省略，则此信息必须通过环境变量 `TESSDATA_PREFIX` 提供。可使用 :meth:`get_tessdata` 方法获取。

         .. note:: 
            
            此方法 **不支持** `clip` 参数 —— OCR 始终针对整个页面矩形区域进行。

         :returns:
         
            一个 :ref:`TextPage` 对象。执行时间可能明显长于 :meth:`Page.get_textpage`。

            对于整页 OCR 处理，**所有文本** 都将使用 Tesseract 生成的 "GlyphlessFont" 字体。对于部分 OCR 处理，原始文本将保持其原有属性，仅从图像提取的文本会使用 "GlyphlessFont"。

            .. note::
            
               **OCR 识别的文本仅在** PyMuPDF 进行文本提取和搜索时可用，前提是 `textpage` 参数指定此方法的输出。

               `此 Jupyter Notebook <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/jupyter-notebooks/partial-ocr.ipynb>`_ 提供了 OCR 文本页面的使用示例。

         |history_begin|

         * 新增于 v1.19.0
         * 更改于 v1.19.1: 支持完整和部分 OCR 处理页面。

         |history_end|

      .. method:: get_drawings(extended=False)

         返回页面的矢量图形。这些是绘制线条、矩形、四边形或曲线的指令，并包含颜色、透明度、线宽、虚线模式等属性。其他术语包括 **"线条艺术"（line art）** 和 **"绘图"（drawings）**。

         :returns: 一个字典列表。每个字典项包含一个或多个属于同一绘制操作的指令，它们共享相同的属性（如颜色、虚线等）。在 PDF 中，这称为 **"路径"（path）**，但该方法适用于所有文档类型。

         适用于填充（fill）、描边（stroke）和填充+描边（fill-stroke）的路径字典设计为兼容 :ref:`Shape` 类，包含以下键：

         ============== ============================================================================
         键名           值
         ============== ============================================================================
         closePath      与 :ref:`Shape` 参数相同
         color          描边颜色（参见 :ref:`Shape`）
         dashes         虚线样式（参见 :ref:`Shape`）
         even_odd       填充区域重叠颜色模式，与 :ref:`Shape` 参数相同
         fill           填充颜色（参见 :ref:`Shape`）
         items          绘制指令列表：线条、矩形、四边形或曲线
         lineCap        线端样式（三元组），输出时应使用最大值（参见 :ref:`Shape`）
         lineJoin       线连接方式，与 :ref:`Shape` 参数相同
         fill_opacity   填充颜色透明度（参见 :ref:`Shape`）（新增于 v1.18.17）
         stroke_opacity 描边颜色透明度（参见 :ref:`Shape`）（新增于 v1.18.17）
         rect           此路径覆盖的页面区域，仅供参考
         layer          适用的可选内容组（Optional Content Group）名称（新增于 v1.22.0）
         level          如果 `extended=True`，则表示路径的层级（新增于 v1.22.0）
         seqno          绘制页面外观时的命令编号（新增于 v1.19.0）
         type           该路径的类型（新增于 v1.18.17）
         width          线条宽度（参见 :ref:`Shape`）
         ============== ============================================================================

         由于 `opacity` 键已被 `"fill_opacity"` 和 `"stroke_opacity"` 替代，因此该方法现在与 :meth:`Shape.finish` 的参数兼容。（更改于 v1.18.17）

         除剪裁（clip）或分组（group）路径外，`"type"` 可能具有以下值：

         * **"f"** —— *仅填充* 路径。仅相关的键有效，不适用的键（如 `"color"`、`"lineCap"`、`"width"` 等）将设置为 ``None``。
         * **"s"** —— *仅描边* 路径。类似于上一个，仅 `"fill"` 键的值为 ``None``。
         * **"fs"** —— 进行 *填充+描边* 操作的路径。

         `path["items"]` 中的每个项目为以下类型之一：

         * `("l", p1, p2)` —— 从 `p1` 到 `p2` 的直线（:ref:`Point` 对象）。
         * `("c", p1, p2, p3, p4)` —— 三次贝塞尔曲线 **从** `p1` **到** `p4` （ `p2` 和 `p3` 为控制点）。
         * `("re", rect, orientation)` —— 一个 :ref:`Rect` 对象，表示矩形。多个矩形可以被检测到（更改于 v1.18.17）。`orientation` 为 `1` 表示逆时针旋转（anti-clockwise），`-1` 表示顺时针旋转（更改于 v1.19.2）。
         * `("qu", quad)` —— 一个 :ref:`Quad` 对象。检测到 3 或 4 条连续直线实际形成一个四边形（更改于 v1.19.2，新增于 v1.18.17）。

         .. note:: 从 v1.19.2 开始，四边形和矩形的识别更加可靠。

         使用 :ref:`Shape` 类，可以在新的 PDF 页面上高保真地重现原始绘图（在正常情况下）。更多限制条件请参阅 `如何提取绘图 <RecipesDrawingAndGraphics_Extract_Drawings>`_。

         指定 `extended=True` 将显著改变输出格式。最重要的变化是增加了新的字典类型："clip" 和 "group"。所有路径现在按层级结构组织，并通过 `level` 键编码。（新增于 v1.22.0）

         若某个路径的 `level` 小于前一个路径的 `level`，则表明其前面的层级已结束。相同 `level` 值的 "clip" 或 "group" 表示当前剪裁或分组的作用范围已结束。例如::

            +------+------+--------+------+--------+
            | 行号 | lvl0 | lvl1   | lvl2 |  lvl3  |
            +------+------+--------+------+--------+
            |  0   | clip |        |      |        |
            |  1   |      | fill   |      |        |
            |  2   |      | group  |      |        |
            |  3   |      |        | clip |        |
            |  4   |      |        |      | stroke |
            |  5   |      |        | fill |        |  结束行 3 的 clip 作用范围
            |  6   |      | stroke |      |        |  结束行 2 的 group 作用范围
            |  7   |      | clip   |      |        |
            |  8   | fill |        |      |        |  结束行 0 的 clip 作用范围
            +------+------+--------+------+--------+

         在上表中：
         
         * 行 0 的 clip 作用范围直到行 7 结束。
         * 行 2 的 group 作用于行 3 到 5，行 3 的 clip 仅适用于行 4。
         * 行 4 的 stroke 受到行 2 的 group 和行 3 的 clip 的影响，而行 3 的 clip 本身又属于行 0 的 clip 范围。

         * **"clip"** 字典。其值（尤其是 `"scissor"`）在后续 **更高 "level" 值** 的路径上仍然有效。

            ============== ============================================================================
            键名           值
            ============== ============================================================================
            closePath      与 "stroke" 或 "fill" 字典中的相同
            even_odd       与 "stroke" 或 "fill" 字典中的相同
            items          与 "stroke" 或 "fill" 字典中的相同
            rect           与 "stroke" 或 "fill" 字典中的相同
            layer          与 "stroke" 或 "fill" 字典中的相同
            level          层级信息
            scissor        剪裁区域
            type           "clip"
            ============== ============================================================================

         * **"group"** 字典。其值在后续 **更高 "level" 值** 的路径上仍然有效，遇到相同或更低 `level` 值的路径时结束作用范围。

            ============== ============================================================================
            键名           值
            ============== ============================================================================
            rect           与 "stroke" 或 "fill" 字典中的相同
            layer          与 "stroke" 或 "fill" 字典中的相同
            level          层级信息
            isolated       (bool) 该组是否为独立组
            knockout       (bool) 是否为 "Knockout Group"
            blendmode      混合模式名称，默认值为 "Normal"
            opacity        透明度，取值范围 [0, 1]
            type           "group"
            ============== ============================================================================

         .. note:: 此方法基于 :meth:`Page.get_cdrawings` 输出，后者速度更快，但需要额外处理。

         |history_begin|
         
         * 新增于 v1.18.0
         * 更改于 v1.18.17
         * 更改于 v1.19.0: 添加 `"seqno"` 键，移除 `"clippings"` 键
         * 更改于 v1.19.1: 统一 `"color"` / `"fill"` 值为 RGB 元组或 `None`
         * 更改于 v1.19.2: 添加 `"orientation"` 指示 `"re"` 项的旋转方向
         * 更改于 v1.22.0: 添加 `"layer"` 键，存储可选内容组名称
         * 更改于 v1.22.0: 添加 `extended` 参数，返回剪裁和分组路径
         
         |history_end|

      .. method:: get_cdrawings(extended=False)

         提取页面上的矢量图形。除了以下技术性差异外，在功能上等同于 :meth:`Page.get_drawings`，但速度更快：

         * 每种路径类型仅包含相关的键值，例如，描边路径不包含 `"fill"` 颜色键。请参阅 :meth:`Page.get_drawings` 方法中的注释。
         * 坐标以 :data:`point_like`、:data:`rect_like` 和 :data:`quad_like` **元组** 形式提供，而不是 :ref:`Point`、:ref:`Rect`、:ref:`Quad` 对象。

         如果性能是一个关注点，建议使用此方法：与 1.18.17 之前的版本相比，响应时间大幅缩短。例如，某些页面以前需要 2 秒，现在只需 200 毫秒。

         |history_begin|

         * 新增于 v1.18.17
         * v1.19.0 变更：移除 `"clippings"` 键，新增 `"seqno"` 键。
         * v1.19.1 变更：始终生成 RGB 颜色元组。
         * v1.22.0 变更：新增 `"layer"` 键，包含路径的可选内容组（Optional Content Group）名称（或 `None`）。
         * v1.22.0 变更：新增 `extended` 参数，可用于返回剪裁路径。

         |history_end|


      .. method:: get_fonts(full=False)

         仅适用于 PDF：返回页面引用的字体列表。此方法是 :meth:`Document.get_page_fonts` 的包装器。


      .. method:: get_images(full=False)

         仅适用于 PDF：返回页面引用的图像列表。此方法是 :meth:`Document.get_page_images` 的包装器。

      .. index::
         pair: hashes; get_image_info
         pair: xrefs; get_image_info

      .. method:: get_image_info(hashes=False, xrefs=False)

         返回页面上显示的所有图像的元信息字典列表。适用于所有文档类型。

         :arg bool hashes: 计算每个图像的 MD5 哈希值，以便识别重复图像。这会在输出中添加 `"digest"` 键，其值为 16 字节的 `bytes` 对象。（新增于 v1.18.13）

         :arg bool xrefs: **仅适用于 PDF。** 尝试查找每个图像的 :data:`xref`，隐含启用 `hashes=True`。在字典中添加 `"xref"` 键。如果找不到，其值为 0，表示该图像是“内嵌图像”或其 `xref` 无法检测到。请注意，该选项会延长响应时间，因为 MD5 哈希值至少会计算两次。（新增于 v1.18.13）

         :rtype: list[dict]
         :returns: 一个字典列表，仅包含 **页面上实际显示的图像**，包括 *“内嵌图像”*。字典结构类似于 `page.get_text("dict")` 方法返回的图像块。

            与 :meth:`Page.get_text` 提取的图像不同，此方法 **不会** 加载图像的二进制内容，从而显著减少内存使用。此外，此方法的图像检测不受 `clip` 参数或页面可见区域的限制，而 :meth:`Page.get_text` 仅提取 **完全包含** 在 `clip` 内的图像。

            =============== ===============================================================
            **键**             **值**
            =============== ===============================================================
            number          块编号（``int``）
            bbox            页面上的图像边界框，:data:`rect_like`
            width           原始图像宽度（``int``）
            height          原始图像高度（``int``）
            cs-name         颜色空间名称（``str``）
            colorspace      颜色空间.n（``int``）
            xres            x 方向分辨率（``int``）
            yres            y 方向分辨率（``int``）
            bpc             每通道位数（``int``）
            size            图像占用的存储空间（``int``）
            digest          MD5 哈希值（``bytes``），仅在 ``hashes`` 为 `True` 时可用
            xref            图像的 :data:`xref`，若 *xrefs* 为 `True`，否则为 0
            transform       变换矩阵，将图像矩形转换为边界框，:data:`matrix_like`
            has-mask        图像是否透明且具有遮罩（``bool``）
            =============== ===============================================================

            同一图像的多个实例都会被报告。可以通过比较 `digest` 值来检测重复图像。

         |history_begin|

         * 新增于 v1.18.11
         * v1.18.13 变更：新增图像 MD5 哈希值计算和 :data:`xref` 搜索。

         |history_end|

      .. method:: get_xobjects()

         仅适用于 PDF：返回页面引用的 Form XObjects 列表。此方法是 :meth:`Document.get_page_xobjects` 的包装器。

      .. index::
         pair: transform; get_image_rects

      .. method:: get_image_rects(item, transform=False)

         仅适用于 PDF：返回嵌入图像的边界框和变换矩阵。这是 :meth:`Page.get_image_bbox` 的改进版，具有以下区别：

         * **无论** 图像如何被调用（页面本身或其 Form XObjects 之一），结果始终完整且正确。
         * 结果是一个 :ref:`Rect` 或 (:ref:`Rect`, :ref:`Matrix`) 对象的列表，具体取决于 *transform* 参数。列表中的每个项目表示图像在页面上的一个位置。:meth:`Page.get_image_bbox` 可能无法检测到图像的多个实例。
         * 该方法调用 :meth:`Page.get_image_info` 并设置 `xrefs=True`，因此响应时间比 :meth:`Page.get_image_bbox` 明显更长。

         :arg list,str,int item: 来自 :meth:`Page.get_images` 列表的一个项目，或该项目的引用 **名称** （item[7]），或图像的 :data:`xref`。
         :arg bool transform: 是否返回用于将图像矩形变换为页面边界框的矩阵。如果为 `True`，则返回 `(bbox, matrix)` 元组。

         :rtype: list
         :returns: 每个图像实例的边界框及其相应的变换矩阵。如果该图像不在页面上，则返回空列表 `[]`。

         |history_begin|

         新增于 v1.18.13

         |history_end|

      .. index::
         pair: transform; get_image_bbox

      .. method:: get_image_bbox(item, transform=False)

         仅适用于 PDF：返回嵌入图像的边界框和变换矩阵。

         :arg list,str item: 需要指定 `full=True` 时 :meth:`Page.get_images` 返回的列表项，或该项目的引用 **名称** （item[-3] 或 item[7]）。
         :arg bool transform: 是否返回用于将图像矩形变换为页面边界框的矩阵（新增于 v1.18.11）。默认情况下，仅返回边界框。如果 `True`，则返回 `(bbox, matrix)` 元组。

         :rtype: :ref:`Rect` 或 (:ref:`Rect`, :ref:`Matrix`)
         :returns: 图像的边界框，可选地返回其变换矩阵。

            |history_begin|
            
            * (v1.16.7 变更): 如果页面实际上未显示此图像，则现在返回一个无限矩形。在之前的版本中，会抛出异常。对于形式上无效的参数仍然会抛出异常。
            * (v1.17.0 变更): 仅考虑页面直接引用的图像。这意味着嵌入的 PDF 页面中的图像将被忽略，并且会抛出异常。
            * (v1.18.5 变更): 移除了 v1.17.0 引入的限制：现在可以指定页面图像列表中的任何项目。
            * (v1.18.11 变更): 部分恢复了一些限制：仅考虑页面直接引用的图像，或由页面直接引用的 Form XObject 引用的图像。
            * (v1.18.11 变更): 现在可选择同时返回变换矩阵，格式为 `(bbox, transform)` 元组。

            |history_end|

         .. note::

            1. 请注意，:meth:`Page.get_images` 可能包含“无效”条目，即页面 **未实际显示** 的图像。这不是错误，而是 PDF 创建者的有意行为。在这种情况下，不会抛出异常，而是返回一个无限矩形。可以通过在调用此方法前执行 :meth:`Page.clean_contents` 来避免此问题。
            2. 图像的“变换矩阵”定义如下：满足 `bbox / transform == pymupdf.Rect(0, 0, 1, 1)` 的矩阵。详情请参考 :ref:`ImageTransformation`。

         |history_begin|

         * v1.18.11 变更：返回图像变换矩阵。

         |history_end|

      .. index::
         pair: matrix; get_svg_image

      .. method:: get_svg_image(matrix=pymupdf.Identity, text_as_path=True)

         从页面创建一个 SVG 图像。目前仅支持完整页面图像。

         :arg matrix_like matrix: 变换矩阵，默认值为 :ref:`Identity`。
         :arg bool text_as_path: 控制文本的表示方式。 ``True`` 时，每个字符都被输出为一系列基本绘图命令，这样在浏览器中可以更精确地显示文本，但对于以文本为主的页面来说，输出文件会 **大得多**。如果选择 ``False``，则显示质量取决于当前系统上是否存在引用的字体。对于缺失的字体，浏览器会使用默认字体进行替换，可能导致外观不佳。选择 ``False`` 以便能够解析 SVG 中的文本。（新增于 v1.17.5）

         :returns: 一个 UTF-8 编码的字符串，包含生成的 SVG 图像。由于 SVG 采用 XML 语法，因此可以将其保存为文本文件，标准扩展名为 `.svg`。

         .. note:: 对于 PDF，可以通过修改页面的 CropBox 来规避“仅支持完整页面图像”的限制。

      .. index::
         pair: alpha; get_pixmap
         pair: annots; get_pixmap
         pair: clip; get_pixmap
         pair: colorspace; get_pixmap
         pair: matrix; get_pixmap
         pair: dpi; get_pixmap

      .. method:: get_pixmap(*, matrix=pymupdf.Identity, dpi=None, colorspace=pymupdf.csRGB, clip=None, alpha=False, annots=True)

         从页面创建一个 Pixmap。这可能是最常用的方法之一，用于生成 :ref:`Pixmap`。

         所有参数均为 *仅限关键字参数*。

         :arg matrix_like matrix: 变换矩阵，默认值为 :ref:`Identity`。
         :arg int dpi: 期望的 x 和 y 方向的分辨率。如果不为 `None`，则 `"matrix"` 参数将被忽略。（新增于 v1.19.2）
         :arg colorspace: 期望的颜色空间，可以是 `"GRAY"`、`"RGB"` 或 `"CMYK"` （不区分大小写）。或者指定一个 :ref:`Colorspace`，即预定义的颜色空间之一：:data:`csGRAY`、:data:`csRGB` 或 :data:`csCMYK`。
         :type colorspace: str 或 :ref:`Colorspace`
         :arg irect_like clip: 仅渲染与页面矩形相交的区域。
         :arg bool alpha: 是否添加 alpha 通道。如果不需要透明度，建议保持默认值 ``False``，这样可以节省大量内存（RGB 情况下减少 25%）并加快处理速度。此外，这一参数会影响图像的渲染方式:

            * 设为 ``True`` 时，Pixmap 采样区域的预清理值为 *0x00*，即页面空白区域将呈现 **透明**。
            * 设为 ``False`` 时，Pixmap 采样区域的预清理值为 *0xff*，即页面空白区域将呈现 **白色**。

            |history_begin|

            变更于 v1.14.17
            默认 `alpha` 值现在为 ``False``。

            * 使用 `alpha=True` 生成的图像：

            .. image:: images/img-alpha-1.*

            * 使用 `alpha=False` 生成的图像：

            .. image:: images/img-alpha-0.*

            |history_end|

         :arg bool annots: *(新增于 v1.16.0)* 是否同时渲染注释，默认值为 `True`。可以单独为注释创建 Pixmap。

         :rtype: :ref:`Pixmap`
         :returns: 该页面的 Pixmap。控制生成图像的最重要参数是 **matrix**。例如，可以使用 **Matrix(xzoom, yzoom)** 调整图像分辨率。`zoom > 1` 将提高分辨率，例如 `zoom=2` 会使图像在该方向上像素数量翻倍，从而生成 2 倍大小的图像。非正值会水平或垂直翻转。此外，矩阵还可用于旋转、倾斜，并且可以通过矩阵乘法组合多个效果。更多详情请参阅 :ref:`Matrix` 章节。

         .. note::

            * 如果 `alpha=True`，生成的 Pixmap 将使用 *“预乘”* 像素。有关背景知识，可参考 `预乘 Alpha 通道 <https://en.wikipedia.org/wiki/Glossary_of_computer_graphics#P>`_。
            * 此方法会考虑页面旋转，并不会超出 `clip` 和 :attr:`Page.cropbox` 的交集范围。如果需要页面的 `mediabox` （并且它不同于 `cropbox` ），可以使用以下代码片段::

               import pymupdf
               doc = pymupdf.open("demo1.pdf")
               page = doc[0]
               rotation = page.rotation
               cropbox = page.cropbox
               page.set_cropbox(page.mediabox)
               page.set_rotation(0)
               pix = page.get_pixmap()
               page.set_cropbox(cropbox)
               if rotation != 0:
                  page.set_rotation(rotation)

         |history_begin|

         * 变更于 v1.19.2：新增对 `dpi` 参数的支持。

         |history_end|

      .. method:: annot_names()

         仅适用于 PDF：返回注释、小部件和链接的名称列表。从技术上讲，这些是页面的 */Annots* 数组中每个 PDF 对象的 */NM* 值。

         :rtype: list

         |history_begin|

         * 新增于 v1.16.10

         |history_end|

      .. method:: annot_xrefs()

         仅适用于 PDF：返回注释、小部件和链接的 :data:`xref` 编号列表，即页面 */Annots* 数组中的所有条目。

         :rtype: list
         :returns: 包含 *(xref, type)* 元组的列表，其中 `type` 是注释类型。可以使用 `type` 来区分链接、表单字段和注释，详见 :ref:`AnnotationTypes`。

         |history_begin|

         * 新增于 v1.17.1

         |history_end|

      .. method:: load_annot(ident)

         仅适用于 PDF：返回由 *ident* 标识的注释。*ident* 可以是其唯一名称（PDF `/NM` 键）或 :data:`xref`。

         :arg str,int ident: 注释的名称或 xref 编号。

         :rtype: :ref:`Annot`
         :returns: 该注释对象，若未找到则返回 ``None``。

         .. note:: 方法 :meth:`Page.annot_names` 和 :meth:`Page.annot_xrefs` 可分别提供名称或 xref 列表，可以使用这些方法获取标识符并通过本方法加载相应的注释。

         |history_begin|

         * 新增于 v1.17.1

         |history_end|

      .. method:: load_widget(xref)

         仅适用于 PDF：返回由 *xref* 标识的表单字段。

         :arg int xref: 表单字段的 xref 编号。

         :rtype: :ref:`Widget`
         :returns: 该字段对象，若未找到则返回 ``None``。

         .. note:: 该方法类似于 :meth:`Page.load_annot`，但仅支持使用 xref 作为标识符。

         |history_begin|

         * 新增于 v1.19.6

         |history_end|

      .. method:: load_links()

         返回页面上的第一个链接。本方法是属性 :attr:`first_link` 的同义方法。

         :rtype: :ref:`Link`
         :returns: 页面上的第一个链接（或 ``None``）。

      .. index::
         pair: rotate; set_rotation

      .. method:: set_rotation(rotate)

         仅适用于 PDF：设置页面的旋转角度。

         :arg int rotate: 以度为单位的旋转角度，必须是 90 的整数倍。该值会被转换为 0、90、180 或 270 之一。

      .. method:: remove_rotation()

         仅适用于 PDF：将页面旋转角度重置为 0，同时保持外观和内容不变。

         :returns: 进行此更改所使用的逆变换矩阵。如果页面未旋转（旋转角度为 0），则返回 :ref:`Identity`。该方法会自动重新计算页面上所有注释、链接和小部件的矩形区域。

            该方法在使用 :meth:`Page.show_pdf_page` 时可能会派上用场。

      .. index::
         pair: clip; show_pdf_page
         pair: keep_proportion; show_pdf_page
         pair: overlay; show_pdf_page
         pair: rotate; show_pdf_page

      .. method:: show_pdf_page(rect, docsrc, pno=0, keep_proportion=True, overlay=True, oc=0, rotate=0, clip=None)

         仅适用于 PDF：在当前页面上以 **矢量图像** 形式显示另一个 PDF 的页面（功能类似于 :meth:`Page.insert_image`）。该方法用途广泛，例如：

         * 创建现有 PDF 文件的 “n-up” 版本，即将多个输入页面合并到 **一个输出页面** 中（示例： `combine.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/combine-pages/combine.py>`_），
         * 创建 “海报化” PDF 文件，即将每个输入页面拆分为多个部分，每个部分分别生成一个输出页面（示例： `posterize.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/posterize-document/posterize.py>`_），
         * 包含基于 PDF 的矢量图像，如公司徽标、水印等（示例： `svg-logo.py <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/examples/svg-logo.py>`_，该示例在每个页面上放置基于 SVG 的徽标，需要额外的软件包进行 SVG 到 PDF 的转换）。

         :arg rect_like rect: 在当前页面上的放置位置。必须是有限矩形，并且与页面的交集不能为空。
         :arg docsrc: 包含要显示页面的源 PDF 文档。必须是不同的文档对象，但可以是相同的文件。
         :type docsrc: :ref:`Document`

         :arg int pno: 要显示的页码（从 0 开始，范围：`-∞ < pno < docsrc.page_count`）。

         :arg bool keep_proportion: 是否保持宽高比（默认值为 ``True``）。如果设为 ``False``，则所有四个角始终位于目标矩形的边界上，无论旋转值如何。这可能会导致图像变形或变为非矩形。

         :arg bool overlay: 是否将图像置于前景（默认值）。若设为 ``False`` ，则图像将置于背景。

         :arg int oc: (:data:`xref`) 使显示依赖于此 :data:`OCG` / :data:`OCMD` （必须在目标 PDF 中定义） [#f9]_ 。（新增于 v1.18.3）

         :arg float rotate: 旋转源矩形显示的角度。支持任意角度（在 v1.14.11 版本中更改）。（新增于 v1.14.10）

         :arg rect_like clip: 指定要显示的源页面区域。默认为整个页面，否则必须是有限矩形，并且其与源页面的交集不能为空。

         .. note:: 
            
            与方法 :meth:`Document.insert_pdf` 不同，此方法不会复制注释、小部件或链接，因此这些元素不会包含在目标页面中 [#f6]_。但页面的 **其他资源（文本、图像、字体等）** 都会被导入当前 PDF，因此它们仍然会出现在文本提取结果以及 :meth:`get_fonts` 和 :meth:`get_images` 列表中，即使它们不在由 *clip* 指定的可见区域内。

         示例：在同一页面上显示相同的源页面，分别旋转 90 度和 -90 度:

         >>> doc = pymupdf.open()  # 创建一个新的空 PDF
         >>> page = doc.new_page()  # 新建一个 A4 页面
         >>>
         >>> # 上半部分页面
         >>> r1 = pymupdf.Rect(0, 0, page.rect.width, page.rect.height / 2)
         >>>
         >>> # 下半部分页面
         >>> r2 = r1 + (0, page.rect.height / 2, 0, page.rect.height / 2)
         >>>
         >>> src = pymupdf.open("PyMuPDF.pdf")  # 载入源 PDF
         >>>
         >>> page.show_pdf_page(r1, src, 0, rotate=90)
         >>> page.show_pdf_page(r2, src, 0, rotate=-90)
         >>> doc.save("show.pdf")

         .. image:: images/img-showpdfpage.*
            :scale: 70

         |history_begin|

         * v1.14.11 版本更改：弃用参数 *reuse_xref*。源矩形在目标矩形内居中定位，并支持任意旋转角度。
         * v1.18.3 版本更改：新增参数 `oc`。

         |history_end|

      .. method:: new_shape()

         仅适用于 PDF：为当前页面创建一个新的 :ref:`Shape` 对象。

         :rtype: :ref:`Shape`
         :returns: 一个新的 :ref:`Shape` 对象，用于复合绘图。详情请参考 :ref:`Shape` 说明。

      .. index::
         pair: flags; search_for
         pair: quads; search_for
         pair: clip; search_for
         pair: textpage; search_for

      .. method:: search_for(needle, *, clip=None, quads=False, flags=TEXT_DEHYPHENATE | TEXT_PRESERVE_WHITESPACE | TEXT_PRESERVE_LIGATURES | TEXT_MEDIABOX_CLIP, textpage=None)

         在页面上搜索 *needle*。此方法是 :meth:`TextPage.search` 的封装。

         :arg str needle: 要搜索的文本，可包含空格。搜索不区分大小写，但仅适用于 ASCII 字符。例如，"COMPÉTENCES" 不会匹配 "compétences"，但可以匹配 "compÉtences"。德语变音符号等也遵循相同的规则。
         :arg rect_like clip: 仅在此区域内搜索。（新增于 v1.18.2）
         :arg bool quads: 是否返回 :ref:`Quad` 对象（默认返回 :ref:`Rect`）。
         :arg int flags: 控制底层 :ref:`TextPage` 的数据提取行为。默认情况下，保留连字和空格，并检测连字符 [#f8]_。
         :arg textpage: 使用预先创建的 :ref:`TextPage` 对象，这可以显著减少执行时间。如果提供了此参数，则 *flags* 和 *clip* 参数将被忽略。如果未提供，将临时创建一个 `TextPage`。（新增于 v1.19.0）

         :rtype: list
         :returns: 

            返回一个 :ref:`Rect` 或 :ref:`Quad` 对象的列表，每个对象 **通常** 包围 *needle* 的一个匹配项。**然而**，如果 *needle* 分布在多行上，则每一部分都会单独返回。例如，如果 `needle = "search string"`，则可能返回两个矩形。

         |history_begin|
         
         v1.18.2 版本更新：
         
         * 取消了结果列表长度限制（移除 `hit_max` 参数）。
         * 支持跨行 **连字符** 匹配。例如，搜索 "method" 时，即使它在换行处被拆分为 "meth-od"，仍然可以找到，并返回两个矩形：一个围绕 "meth"（不包含连字符），另一个围绕 "od"。

         |history_end|

         .. note:: 该方法支持 **多行文本标注**，可将返回列表直接用作创建标注的参数。

         .. caution::

            * 该方法有一个特殊行为：连续多次出现的 *needle* 会被视为 **一个** 匹配项。例如，如果页面上有 "abc" 和 "abcabc"，则仅返回两个矩形，一个对应 "abc"，另一个对应 "abcabc"。
            * 可使用 :meth:`Page.get_textbox` 检查每个矩形实际包含的文本。

         .. note:: 该方法 **不支持正则表达式** 进行搜索。如需类似功能，可先提取文本，再使用正则表达式进行匹配。例如，匹配单词的方法如下::

            >>> pattern = re.compile(r"...")  # 定义正则表达式模式
            >>> words = page.get_text("words")  # 提取页面上的单词
            >>> matches = [w for w in words if pattern.search(w[4])]

            `matches` 列表将包含所有匹配正则表达式的单词。同理，可以从 `page.get_text("dict")` 输出的 `span["text"]` 中筛选匹配项。

         |history_begin|

         * v1.18.2 版本更新：新增 `clip` 参数，移除 `hit_max` 参数，默认启用“去连字符”功能。
         * v1.19.0 版本更新：新增 `textpage` 参数。

         |history_end|


      .. method:: set_mediabox(r)

         仅适用于 PDF：通过修改页面对象的 :data:`mediabox` 来更改页面的物理尺寸。

         :arg rect_like r: 要设置的新 :data:`mediabox` 值。

         .. note:: 
            
            此方法会移除页面的所有其他（可选）矩形定义（如 :data:`cropbox`、ArtBox、TrimBox 和 BleedBox），以防止不一致的情况。这些矩形将恢复默认值。

         .. caution:: 
            
            对于非空页面，调整 `mediabox` 可能会产生意外效果。因为所有内容的位置都依赖于此值，修改后可能导致内容偏移甚至消失。

         |history_begin|

         * 新增于 v1.16.13
         * v1.19.4 版本更新：移除所有其他矩形定义。

         |history_end|

      .. method:: set_cropbox(r)

         仅适用于 PDF：更改页面的可见部分。

         :arg rect_like r: 页面新的可见区域。注意，这 **必须** 以 **未旋转坐标** 指定，且不能为空或无限，必须完全包含在 :attr:`Page.mediabox` 中。

         执行后 **（如果页面未旋转）**， :attr:`Page.rect` 将等于此矩形，但如果需要，会将其移动到左上角（0, 0）的位置。示例：

         >>> page = doc.new_page()
         >>> page.rect
         pymupdf.Rect(0.0, 0.0, 595.0, 842.0)
         >>>
         >>> page.cropbox  # cropbox 和 mediabox 相等
         pymupdf.Rect(0.0, 0.0, 595.0, 842.0)
         >>>
         >>> # 现在将 cropbox 设置为页面的一部分
         >>> page.set_cropbox(pymupdf.Rect(100, 100, 400, 400))
         >>> # 这也会改变 "rect" 属性：
         >>> page.rect
         pymupdf.Rect(0.0, 0.0, 300.0, 300.0)
         >>>
         >>> # 但 mediabox 保持不变
         >>> page.mediabox
         pymupdf.Rect(0.0, 0.0, 595.0, 842.0)
         >>>
         >>> # 恢复 CropBox 更改
         >>> # 可以将其设置为 MediaBox
         >>> page.set_cropbox(page.mediabox)
         >>> # 或通过“刷新” MediaBox：将移除所有其他矩形
         >>> page.set_mediabox(page.mediabox)

      .. method:: set_artbox(r)

      .. method:: set_bleedbox(r)

      .. method:: set_trimbox(r)

         仅适用于 PDF：在页面对象中设置相应的矩形。有关这些对象的意义，请参见 :ref:`AdobeManual`，第 77 页。参数和限制与 :meth:`Page.set_cropbox` 相同。

         |history_begin|

         * 新增于 v1.19.4

         |history_end|

      .. attribute:: rotation

         包含页面的旋转角度（对于非 PDF 类型始终为 0）。这是 PDF 文件中该值的副本。PDF 文档中说明：
         
            *"页面显示或打印时应该顺时针旋转的度数。该值必须是 90 的倍数。默认值：0。"*

            在 PyMuPDF 中，我们确保此属性始终为 0、90、180 或 270 其中之一。

         :type: int

      .. attribute:: cropbox_position

         包含页面 `/CropBox` 的左上角点（仅适用于 PDF），否则为 *Point(0, 0)*。

         :type: :ref:`Point`

      .. attribute:: cropbox

         页面 `/CropBox` （仅适用于 PDF）。始终返回 **未旋转** 的页面矩形。对于非 PDF 类型，这将始终等于页面矩形。

         .. note:: 在 PDF 中，`/MediaBox`、`/CropBox` 和页面矩形之间的关系有时可能会令人困惑，请查阅词汇表中的 :data:`MediaBox`。

         :type: :ref:`Rect`

      .. attribute:: artbox

      .. attribute:: bleedbox

      .. attribute:: trimbox

         页面 `/ArtBox`、`/BleedBox`、`/TrimBox`，如果没有提供，则默认为 :attr:`Page.cropbox`。

         :type: :ref:`Rect`

      .. attribute:: mediabox_size

         包含页面 :attr:`Page.mediabox` 的宽度和高度（仅适用于 PDF），否则是 :attr:`Page.rect` 的右下角坐标。

         :type: :ref:`Point`

      .. attribute:: mediabox

         页面 :data:`mediabox` （仅适用于 PDF），否则为 :attr:`Page.rect`。

         :type: :ref:`Rect`

         .. note:: 
            
            对于大多数 PDF 文档和 **所有其他文档类型**， `page.rect == page.cropbox == page.mediabox` 为真。然而，对于某些 PDF，页面的可见部分是真正的 :data:`mediabox` 的子集。此外，如果页面已旋转，则其 `Page.rect` 可能不等于 `Page.cropbox`。在这些情况下，上述属性有助于正确定位页面元素。

      .. attribute:: transformation_matrix

         该矩阵将 PDF 空间中的坐标转换为 MuPDF 空间中的坐标。例如，在 PDF 中， `/Rect [x0 y0 x1 y1]` 中的 (x0, y0) 指定矩形的 **左下角** 点 -- 与 MuPDF 的系统相对，在该系统中，(x0, y0) 指定的是左上角。将 PDF 坐标与此矩阵相乘将得到 (Py-) MuPDF 矩形版本。显然，逆矩阵将再次得出 PDF 矩形。

         :type: :ref:`Matrix`

      .. attribute:: rotation_matrix

      .. attribute:: derotation_matrix

         这些矩阵可用于处理旋转的 PDF 页面。当向 PDF 页面添加/插入任何内容时，始终使用 **未旋转** 页面上的坐标。这些矩阵有助于在两种状态之间转换。例如，如果页面旋转了 90 度 -- 那么一个 A4 页面的左上角点 (0, 0) 会位于何处？

            >>> page.set_rotation(90)  # 旋转 ISO A4 页面
            >>> page.rect
            Rect(0.0, 0.0, 842.0, 595.0)
            >>> p = pymupdf.Point(0, 0)  # 左上角点到哪里去了？
            >>> p * page.rotation_matrix
            Point(842.0, 0.0)
            >>>

         :type: :ref:`Matrix`

      .. attribute:: first_link

         包含页面的第一个 :ref:`Link` （如果没有，则为 ``None`` ）。

         :type: :ref:`Link`

      .. attribute:: first_annot

         包含页面的第一个 :ref:`Annot` （如果没有，则为 ``None`` ）。

         :type: :ref:`Annot`

      .. attribute:: first_widget

         包含页面的第一个 :ref:`Widget` （如果没有，则为 ``None`` ）。

         :type: :ref:`Widget`

      .. attribute:: number

         页面编号。

         :type: int

      .. attribute:: parent

         拥有该页面的文档对象。

         :type: :ref:`Document`

      .. attribute:: rect

         包含页面的矩形。与 :meth:`Page.bound()` 的结果相同。

         :type: :ref:`Rect`

      .. attribute:: xref

         页面在 PDF 中的 :data:`xref`。如果不是 PDF，则为零。

         :type: :ref:`Rect`

.. tab:: 英文

   .. class:: Page
      :no-index:

      .. method:: bound()
         :no-index:

         Determine the rectangle of the page. Same as property :attr:`Page.rect`. For PDF documents this **usually** also coincides with :data:`mediabox` and :data:`cropbox`, but not always. For example, if the page is rotated, then this is reflected by this method -- the :attr:`Page.cropbox` however will not change.

         :rtype: :ref:`Rect`

      .. method:: add_caret_annot(point)
         :no-index:

         PDF only: Add a caret icon. A caret annotation is a visual symbol normally used to indicate the presence of text edits on the page.

         :arg point_like point: the top left point of a 20 x 20 rectangle containing the MuPDF-provided icon.

         :rtype: :ref:`Annot`
         :returns: the created annotation. Stroke color blue = (0, 0, 1), no fill color support.

         .. image:: images/img-caret-annot.*
            :scale: 70

         |history_begin|

         * New in v1.16.0

         |history_end|

      .. method:: add_text_annot(point, text, icon="Note")
         :no-index:

         PDF only: Add a comment icon ("sticky note") with accompanying text. Only the icon is visible, the accompanying text is hidden and can be visualized by many PDF viewers by hovering the mouse over the symbol.

         :arg point_like point: the top left point of a 20 x 20 rectangle containing the MuPDF-provided "note" icon.

         :arg str text: the commentary text. This will be shown on double clicking or hovering over the icon. May contain any Latin characters.
         :arg str icon: choose one of "Note" (default), "Comment", "Help", "Insert", "Key", "NewParagraph", "Paragraph" as the visual symbol for the embodied text [#f4]_. (New in v1.16.0)

         :rtype: :ref:`Annot`
         :returns: the created annotation. Stroke color yellow = (1, 1, 0), no fill color support.

      .. method:: add_freetext_annot(rect, text, *, fontsize=11, fontname="helv", text_color=0, fill_color=None, border_width=0, dashes=None, callout=None, line_end=PDF_ANNOT_LE_OPEN_ARROW, opacity=1, align=TEXT_ALIGN_LEFT, rotate=0, richtext=False, style=None)
         :no-index:

         PDF only: Add text in a given rectangle. Optionally, the appearance of a "callout" shape can be requested by specifying two or three point-like objects -- see below.

         :arg rect_like rect: the rectangle into which the text should be inserted. Text is automatically wrapped to a new line at box width. Text portions not fitting into the rectangle will be invisible without warning.

         :arg str text: the text. May contain any mixture of Latin, Greek, Cyrillic, Chinese, Japanese and Korean characters. If `richtext=True` (see below), the string is interpreted as HTML syntax. This adds a plethora of ways for attractive effects.

         :arg float fontsize: the :data:`fontsize`. Default is 11. Ignored if `richtext=True`.

         :arg str fontname: The font name. Default is "Helv". Ignored if `richtext=True`, otherwise the following **restritions apply:**
         
         * Accepted alternatives are "Helv" (Helvetica), "Cour" (Courier), "TiRo" (Timnes-Roman), "ZaDb" (ZapfDingBats) and "Symb" (Symbol). The name may be abbreviated to the first two characters, like "Co" for "Cour", lower case accepted.

         * Bold or italic variants of the fonts are **not supported.**
         
         :arg list,tuple,float text_color: the text color. Default is black. Ignored if `richtext=True`.

         :arg list,tuple,float fill_color: the fill color. This is used for ``rect`` and the end point of the callout lines when applicable. Default is ``None``.

         :arg list,tuple,float border_color:  This parameter only has an effect if `richtext=True`. Otherwise, ``text_color`` is used.
         
         :arg float border_width: the width of border and ``callout`` lines. Default is 0 (no border), in which case callout lines may still appear with some hairline width, depending on the PDF viewer used.
         
         :arg list,tuple dashes: a list of floats specifying how border and callout lines should be dashed. Default is ``None``.
         
         :arg list,tuple callout: a list / tuple of two or three :data:`point_like` objects, which will be interpreted as end point [, knee point] and start point (in this sequence) of up to two line segments, converting this annotation into a call-out shape.
         
         :arg int line_end: the line end symbol of the call-out line. It is drawn at the first point specified in the `callout` list. Default is an open arrow. For possible values see :ref:`AnnotationLineEnds`.
         
         :arg float opacity: a float `0 <= opacity < 1` turning the annotation transparent. Default is no transparency.
         
         :arg int align: text alignment, one of TEXT_ALIGN_LEFT, TEXT_ALIGN_CENTER, TEXT_ALIGN_RIGHT - justify is **not supported**. Ignored if `richtext=True`.
         
         :arg int rotate: the text orientation. Accepted values are integer multiples of 90°. Invalid entries receive a rotation of 0.
         
         :arg bool richtext: treat ``text`` as HTML syntax. This allows to achieve **bold**, *italic*, arbitrary text colors, font sizes, text alignment including justify and more - as far as HTML and styling instructions support this. This is similar to what happens in :meth:`Page.insert_htmlbox`. The base library will for example pull in required fonts if it encounters characters not contained in the standard ones. Some parameters are ignored if this option is set, as mentioned above. Default is ``False``.
         
         :arg str style: supply optional HTML styling information in CSS syntax. Ignored if `richtext=False`.

         :rtype: :ref:`Annot`
         :returns: the created annotation.

         |history_begin|

         * Changed in v1.19.6: add border color parameter

         |history_end|

      .. method:: add_file_annot(pos, buffer, filename, ufilename=None, desc=None, icon="PushPin")
         :no-index:

         PDF only: Add a file attachment annotation with a "PushPin" icon at the specified location.

         :arg point_like pos: the top-left point of a 18x18 rectangle containing the MuPDF-provided "PushPin" icon.

         :arg bytes,bytearray,BytesIO buffer: the data to be stored (actual file content, any data, etc.).

            Changed in v1.14.13: *io.BytesIO* is now also supported.

         :arg str filename: the filename to associate with the data.
         :arg str ufilename: the optional PDF unicode version of filename. Defaults to filename.
         :arg str desc: an optional description of the file. Defaults to filename.
         :arg str icon: choose one of "PushPin" (default), "Graph", "Paperclip", "Tag" as the visual symbol for the attached data [#f4]_. (New in v1.16.0)

         :rtype: :ref:`Annot`
         :returns: the created annotation.  Stroke color yellow = (1, 1, 0), no fill color support.

      .. method:: add_ink_annot(list)
         :no-index:

         PDF only: Add a "freehand" scribble annotation.

         :arg sequence list: a list of one or more lists, each containing :data:`point_like` items. Each item in these sublists is interpreted as a :ref:`Point` through which a connecting line is drawn. Separate sublists thus represent separate drawing lines.

         :rtype: :ref:`Annot`
         :returns: the created annotation in default appearance black =(0, 0, 0),line width 1. No fill color support.

      .. method:: add_line_annot(p1, p2)
         :no-index:

         PDF only: Add a line annotation.

         :arg point_like p1: the starting point of the line.

         :arg point_like p2: the end point of the line.

         :rtype: :ref:`Annot`
         :returns: the created annotation. It is drawn with line (stroke) color red = (1, 0, 0) and line width 1. No fill color support. The **annot rectangle** is automatically created to contain both points, each one surrounded by a circle of radius 3 * line width to make room for any line end symbols.

      .. method:: add_rect_annot(rect)
         :no-index:

      .. method:: add_circle_annot(rect)
         :no-index:

         PDF only: Add a rectangle, resp. circle annotation.

         :arg rect_like rect: the rectangle in which the circle or rectangle is drawn, must be finite and not empty. If the rectangle is not equal-sided, an ellipse is drawn.

         :rtype: :ref:`Annot`
         :returns: the created annotation. It is drawn with line (stroke) color red = (1, 0, 0), line width 1, fill color is supported.

      ---------

      Redactions
      ~~~~~~~~~~~

      .. method:: add_redact_annot(quad, text=None, fontname=None, fontsize=11, align=TEXT_ALIGN_LEFT, fill=(1, 1, 1), text_color=(0, 0, 0), cross_out=True)
         :no-index:
         
         **PDF only**: Add a redaction annotation. A redaction annotation identifies content to be removed from the document. Adding such an annotation is the first of two steps. It makes visible what will be removed in the subsequent step, :meth:`Page.apply_redactions`.

         :arg quad_like,rect_like quad: specifies the (rectangular) area to be removed which is always equal to the annotation rectangle. This may be a :data:`rect_like` or :data:`quad_like` object. If a quad is specified, then the enveloping rectangle is taken.

         :arg str text: text to be placed in the rectangle after applying the redaction (and thus removing old content). (New in v1.16.12)

         :arg str fontname: the font to use when *text* is given, otherwise ignored. The same rules apply as for :meth:`Page.insert_textbox` -- which is the method :meth:`Page.apply_redactions` internally invokes. The replacement text will be **vertically centered**, if this is one of the CJK or :ref:`Base-14-Fonts`. (New in v1.16.12)

            .. note::

               * For an **existing** font of the page, use its reference name as *fontname* (this is *item[4]* of its entry in :meth:`Page.get_fonts`).
               * For a **new, non-builtin** font, proceed as follows::

                  page.insert_text(point,  # anywhere, but outside all redaction rectangles
                     "something",  # some non-empty string
                     fontname="newname",  # new, unused reference name
                     fontfile="...",  # desired font file
                     render_mode=3,  # makes the text invisible
                  )
                  page.add_redact_annot(..., fontname="newname")

         :arg float fontsize: the :data:`fontsize` to use for the replacing text. If the text is too large to fit, several insertion attempts will be made, gradually reducing the :data:`fontsize` to no less than 4. If then the text will still not fit, no text insertion will take place at all. (New in v1.16.12)

         :arg int align: the horizontal alignment for the replacing text. See :meth:`insert_textbox` for available values. The vertical alignment is (approximately) centered if a PDF built-in font is used (CJK or :ref:`Base-14-Fonts`). (New in v1.16.12)

         :arg sequence fill: the fill color of the rectangle **after applying** the redaction. The default is *white = (1, 1, 1)*, which is also taken if ``None`` is specified. To suppress a fill color altogether, specify ``False``. In this cases the rectangle remains transparent. (New in v1.16.12)

         :arg sequence text_color: the color of the replacing text. Default is *black = (0, 0, 0)*. (New in v1.16.12)

         :arg bool cross_out: add two diagonal lines to the annotation rectangle. (New in v1.17.2)

         :rtype: :ref:`Annot`
         :returns: the created annotation. Its standard appearance looks like a red rectangle (no fill color), optionally showing two diagonal lines. Colors, line width, dashing, opacity and blend mode can now be set and applied via :meth:`Annot.update` like with other annotations. (Changed in v1.17.2)

         .. image:: images/img-redact.*

         |history_begin|

         * New in v1.16.11

         |history_end|


      .. method:: apply_redactions(images=PDF_REDACT_IMAGE_PIXELS|2, graphics=PDF_REDACT_LINE_ART_REMOVE_IF_TOUCHED|2, text=PDF_REDACT_TEXT_REMOVE|0)
         :no-index:

         **PDF only**: Remove all **content** contained in any redaction rectangle on the page.

         **This method applies and then deletes all redactions from the page.**

         :arg int images: How to redact overlapping images. The default (2) blanks out overlapping pixels. `PDF_REDACT_IMAGE_NONE | 0` ignores, and `PDF_REDACT_IMAGE_REMOVE | 1` completely removes images overlapping any redaction annotation. Option `PDF_REDACT_IMAGE_REMOVE_UNLESS_INVISIBLE | 3` only removes images that are actually visible.

         :arg int graphics: How to redact overlapping vector graphics (also called "line-art" or "drawings"). The default (2) removes any overlapping vector graphics. `PDF_REDACT_LINE_ART_NONE | 0` ignores, and `PDF_REDACT_LINE_ART_REMOVE_IF_COVERED | 1` removes graphics fully contained in a redaction annotation. When removing line-art, please be aware that **stroked** vector graphics (i.e. type "s" or "sf") have a **larger wrapping rectangle** than one might expect: first of all, at least 50% of the path's line width have to be added in each direction to truly include all of the drawing. If a so-called "miter limit" is provided (see page 121 of the PDF specification), the enlarging value is `miter * width / 2`. So, when letting everything default (width = 1, miter = 10), the redaction rectangle should be at least 5 points larger in every direction.

         :arg int text: Whether to redact overlapping text. The default `PDF_REDACT_TEXT_REMOVE | 0` removes all characters whose boundary box overlaps any redaction rectangle. This complies with the original legal / data protection intentions of redaction annotations. Other use cases however may require to **keep text** while redacting vector graphics or images. This can be achieved by setting `text=True|PDF_REDACT_TEXT_NONE | 1`. This does **not comply** with the data protection intentions of redaction annotations. **Do so at your own risk.**

         :returns: `True` if at least one redaction annotation has been processed, `False` otherwise.

         .. note::
            * Text contained in a redaction rectangle will be **physically** removed from the page (assuming :meth:`Document.save` with a suitable garbage option) and will no longer appear in e.g. text extractions or anywhere else. All redaction annotations will also be removed. Other annotations are unaffected.

            * All overlapping links will be removed. If the rectangle of the link was covering text, then only the overlapping part of the text is being removed. Similar applies to images covered by link rectangles.

            * The overlapping parts of **images** will be blanked-out for default option `PDF_REDACT_IMAGE_PIXELS` (changed in v1.18.0). Option 0 does not touch any images and 1 will remove any image with an overlap.

            * For option `images=PDF_REDACT_IMAGE_REMOVE` only this page's **references to the images** are removed - not necessarily the images themselves. Images are completely removed from the file only, if no longer referenced at all (assuming suitable garbage collection options).

            * For option `images=PDF_REDACT_IMAGE_PIXELS` a new image of format PNG is created, which the page will use in place of the original one. The original image is not deleted or replaced as part of this process, so other pages may still show the original. In addition, the new, modified PNG image currently is **stored uncompressed**. Do keep these aspects in mind when choosing the right garbage collection method and compression options during save.

            * **Text removal** is done by character: A character is removed if its bbox has a **non-empty overlap** with a redaction rectangle (changed in MuPDF v1.17). Depending on the font properties and / or the chosen line height, deletion may occur for undesired text parts. Using :meth:`Tools.set_small_glyph_heights` with a ``True`` argument before text search may help to prevent this.

            * Redactions are a simple way to replace single words in a PDF, or to just physically remove them. Locate the word "secret" using some text extraction or search method and insert a redaction using "xxxxxx" as replacement text for each occurrence.

            - Be wary if the replacement is longer than the original -- this may lead to an awkward appearance, line breaks or no new text at all.

            - For a number of reasons, the new text may not exactly be positioned on the same line like the old one -- especially true if the replacement font was not one of CJK or :ref:`Base-14-Fonts`.

         |history_begin|

         * New in v1.16.11
         * Changed in v1.16.12: The previous *mark* parameter is gone. Instead, the respective rectangles are filled with the individual *fill* color of each redaction annotation. If a *text* was given in the annotation, then :meth:`insert_textbox` is invoked to insert it, using parameters provided with the redaction.
         * Changed in v1.18.0: added option for handling images that overlap redaction areas.
         * Changed in v1.23.27: added option for removing graphics as well.
         * Changed in v1.24.2: added option `keep_text` to leave text untouched.

         |history_end|

         ---------

      .. method:: add_polyline_annot(points)
         :no-index:

      .. method:: add_polygon_annot(points)
         :no-index:

         PDF only: Add an annotation consisting of lines which connect the given points. A **Polygon's** first and last points are automatically connected, which does not happen for a **PolyLine**. The **rectangle** is automatically created as the smallest rectangle containing the points, each one surrounded by a circle of radius 3 (= 3 * line width). The following shows a 'PolyLine' that has been modified with colors and line ends.

         :arg list points: a list of :data:`point_like` objects.

         :rtype: :ref:`Annot`
         :returns: the created annotation. It is drawn with line color black, line width 1 no fill color but fill color support. Use methods of :ref:`Annot` to make any changes to achieve something like this:

         .. image:: images/img-polyline.*
            :scale: 70

      .. method:: add_underline_annot(quads=None, start=None, stop=None, clip=None)
         :no-index:

      .. method:: add_strikeout_annot(quads=None, start=None, stop=None, clip=None)
         :no-index:

      .. method:: add_squiggly_annot(quads=None, start=None, stop=None, clip=None)
         :no-index:

      .. method:: add_highlight_annot(quads=None, start=None, stop=None, clip=None)
         :no-index:

         PDF only: These annotations are normally used for **marking text** which has previously been somehow located (for example via :meth:`Page.search_for`). But this is not required: you are free to "mark" just anything.

         Standard (stroke only -- no fill color support) colors are chosen per annotation type: **yellow** for highlighting, **red** for striking out, **green** for underlining, and **magenta** for wavy underlining.

         All these four methods convert the arguments into a list of :ref:`Quad` objects. The **annotation** rectangle is then calculated to envelop all these quadrilaterals.

         .. note::

         :meth:`search_for` delivers a list of either :ref:`Rect` or :ref:`Quad` objects. Such a list can be directly used as an argument for these annotation types and will deliver **one common annotation** for all occurrences of the search string::

            >>> # prefer quads=True in text searching for annotations!
            >>> quads = page.search_for("pymupdf", quads=True)
            >>> page.add_highlight_annot(quads)

         .. note::
         Obviously, text marker annotations need to know what is the top, the bottom, the left, and the right side of the area(s) to be marked. If the arguments are quads, this information is given by the sequence of the quad points. In contrast, a rectangle delivers much less information -- this is illustrated by the fact, that 4! = 24 different quads can be constructed with the four corners of a rectangle.

         Therefore, we **strongly recommend** to use the `quads` option for text searches, to ensure correct annotations. A similar consideration applies to marking **text spans** extracted with the "dict" / "rawdict" options of :meth:`Page.get_text`. For more details on how to compute quadrilaterals in this case, see section "How to Mark Non-horizontal Text" of :ref:`FAQ`.

         :arg rect_like,quad_like,list,tuple quads:
         the location(s) -- rectangle(s) or quad(s) -- to be marked. (Changed in v1.14.20)
         A list or tuple must consist of :data:`rect_like` or :data:`quad_like` items (or even a mixture of either).
         Every item must be finite, convex and not empty (as applicable).
         **Set this parameter to** ``None`` if you want to use the following arguments (Changed in v1.16.14).
         And vice versa: if not ``None``, the remaining parameters must be ``None``.
         
         :arg point_like start: start text marking at this point. Defaults to the top-left point of *clip*. Must be provided if `quads` is ``None``. (New in v1.16.14)
         :arg point_like stop: stop text marking at this point. Defaults to the bottom-right point of *clip*. Must be used if `quads` is ``None``. (New in v1.16.14)
         :arg rect_like clip: only consider text lines intersecting this area. Defaults to the page rectangle. Only use if `start` and `stop` are provided. (New in v1.16.14)

         :rtype: :ref:`Annot` or ``None`` (changed in v1.16.14).
         :returns: the created annotation. If *quads* is an empty list, **no annotation** is created (changed in v1.16.14).

         .. note::
         You can use parameters *start*, *stop* and *clip* to highlight consecutive lines between the points *start* and *stop* (starting with v1.16.14).
         Make use of *clip* to further reduce the selected line bboxes and thus deal with e.g. multi-column pages.
         The following multi-line highlight on a page with three text columns was created by specifying the two red points and setting clip accordingly.

         .. image:: images/img-markers.*
            :scale: 100

      .. method:: cluster_drawings(clip=None, drawings=None, x_tolerance=3, y_tolerance=3)
         :no-index:

         Cluster vector graphics (synonyms are line-art or drawings) based on their geometrical vicinity. The method walks through the output of :meth:`Page.get_drawings` and joins paths whose `path["rect"]` are closer to each other than some tolerance values (given in the arguments). The result is a list of rectangles that each wrap things like tables (with gridlines), pie charts, bar charts, etc.

         :arg rect_like clip: only consider paths inside this area. The default is the full page.

         :arg list drawings: (optional) provide a previously generated output of :meth:`Page.get_drawings`. If `None` the method will execute the method.

         :arg float x_tolerance: 

      .. method:: find_tables(clip=None, strategy=None, vertical_strategy=None, horizontal_strategy=None, vertical_lines=None, horizontal_lines=None, snap_tolerance=None, snap_x_tolerance=None, snap_y_tolerance=None, join_tolerance=None, join_x_tolerance=None, join_y_tolerance=None, edge_min_length=3, min_words_vertical=3, min_words_horizontal=1, intersection_tolerance=None, intersection_x_tolerance=None, intersection_y_tolerance=None, text_tolerance=None, text_x_tolerance=None, text_y_tolerance=None, add_lines=None)
         :no-index:

         Find tables on the page and return an object with related information. Typically, the default values of the many parameters will be sufficient. Adjustments should ever only be needed in corner case situations.

         :arg rect_like clip: specify a region to consider within the page rectangle and ignore the rest. Default is the full page.

         :arg str strategy: Request a **table detection** strategy. Valid values are "lines", "lines_strict" and "text".
         
            Default is **"lines"** which uses all vector graphics on the page to detect grid lines.
            
            Strategy **"lines_strict"** ignores borderless rectangle vector graphics. Sometimes single text pieces have background colors which may lead to false columns or lines. This strategy ignores them and can thus increase detection precision.
            
            If **"text"** is specified, text positions are used to generate "virtual" column and / or row boundaries. Use `min_words_*` to request the number of words for considering their coordinates.
            
            Use parameters `vertical_strategy` and `horizontal_strategy` **instead** for a more fine-grained treatment of the dimensions.

         :arg sequence[floats] horizontal_lines: y-coordinates of rows. If provided, there will be no attempt to identify additional table rows. This influences table detection.

         :arg sequence[floats] vertical_lines: x-coordinates of columns. If provided, there will be no attempt to identify additional table columns. This influences table detection.

         :arg int min_words_vertical: relevant for vertical strategy option "text": at least this many words must coincide to establish a **virtual column** boundary.

         :arg int min_words_horizontal: relevant for horizontal strategy option "text": at least this many words must coincide to establish a **virtual row** boundary.

         :arg float snap_tolerance: Any two horizontal lines whose y-values differ by no more than this value will be **snapped** into one. Accordingly for vertical lines. Default is 3. Separate values can be specified instead for the dimensions, using `snap_x_tolerance` and `snap_y_tolerance`.

         :arg float join_tolerance: Any two lines will be **joined** to one if the end and the start points differ by no more than this value (in points). Default is 3. Instead of this value, separate values can be specified for the dimensions using `join_x_tolerance` and `join_y_tolerance`.

         :arg float edge_min_length: Ignore a line if its length does not exceed this value (points). Default is 3.

         :arg float intersection_tolerance: When combining lines into cell borders, orthogonal lines must be within this value (points) to be considered intersecting. Default is 3. Instead of this value, separate values can be specified for the dimensions using `intersection_x_tolerance` and `intersection_y_tolerance`.

         :arg float text_tolerance: Characters will be combined into words only if their distance is no larger than this value (points). Default is 3. Instead of this value, separate values can be specified for the dimensions using `text_x_tolerance` and `text_y_tolerance`.

         :arg tuple,list add_lines: Specify a list of "lines" (i.e. pairs of :data:`point_like` objects) as **additional**, "virtual" vector graphics. These lines may help with table and / or cell detection and will not otherwise influence the detection strategy. Especially, in contrast to parameters `horizontal_lines` and `vertical_lines`, they will not prevent detecting rows or columns in other ways. These lines will be treated exactly like "real" vector graphics in terms of joining, snapping, intersectiing, minimum length and containment in the `clip` rectangle. Similarly, lines not parallel to any of the coordinate axes will be ignored.

         .. image:: images/img-findtables.*

         :returns: a `TableFinder` object that has the following significant attributes:

            * `cells`: a list of **all bboxes** on the page, that have been identified as table cells (across all tables). Each cell is a :data:`rect_like` tuple `(x0, y0, x1, y1)` of coordinates or `None`.
            * `tables`: a list of `Table` objects. This is `[]` if the page has no tables. Single tables can be found as items of this list. But the `TableFinder` object itself is also a sequence of its tables. This means that if `tabs` is a `TableFinder` object, then table "n" is delivered by `tabs.tables[n]` as well as by the shorter `tabs[n]`.


            * The `Table` object has the following attributes:

            * ``bbox``: the bounding box of the table as a tuple `(x0, y0, x1, y1)`.
            * ``cells``: bounding boxes of the table's cells (list of tuples). A cell may also be `None`.
            * ``extract()``: this method returns the text content of each table cell as a list of list of strings.
            * ``to_markdown()``: this method returns the table as a **string in markdown format** (compatible to Github). Supporting viewers can render the string as a table. This output is optimized for **small token** sizes, which is especially beneficial for LLM/RAG feeds. Pandas DataFrames (see method `to_pandas()` below) offer an equivalent markdown table output which however is better readable for the human eye.
            * `to_pandas()`: this method returns the table as a `pandas <https://pypi.org/project/pandas/>`_ `DataFrame <https://pandas.pydata.org/docs/reference/frame.html>`_. DataFrames are very versatile objects allowing a plethora of table manipulation methods and outputs to almost 20 well-known formats, among them Excel files, CSV, JSON, markdown-formatted tables and more. `DataFrame.to_markdown()` generates a Github-compatible markdown format optimized for human readability. This method however requires the package `tabulate <https://pypi.org/project/tabulate/>`_ to be installed in addition to pandas itself.
            * ``header``: a `TableHeader` object containing header information of the table.
            * ``col_count``: an integer containing the number of table columns.
            * ``row_count``: an integer containing the number of table rows.
            * ``rows``: a list of `TableRow` objects containing two attributes, ``bbox`` is the boundary box of the row, and `cells` is a list of table cells contained in this row.

            * The `TableHeader` object has the following attributes:

            * ``bbox``: the bounding box of the header.
            * `cells`: a list of bounding boxes containing the name of the respective column.
            * `names`: a list of strings containing the text of each of the cell bboxes. They represent the column names -- which are used when exporting the table to pandas DataFrames, markdown, etc.
            * `external`: a bool indicating whether the header bbox is outside the table body (`True`) or not. Table headers are never identified by the `TableFinder` logic. Therefore, if `external` is true, then the header cells are not part of any cell identified by `TableFinder`. If `external == False`, then the first table row is the header.

            Please have a look at these `Jupyter notebooks <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/table-analysis>`_, which cover standard situations like multiple tables on one page or joining table fragments across multiple pages.

            .. caution:: The lifetime of the `TableFinder` object, as well as that of all its tables **equals the lifetime of the page**. If the page object is deleted or reassigned, all tables are no longer valid.
            
               The only way to keep table content beyond the page's availability is to **extract it** via methods `Table.to_markdown()`, `Table.to_pandas()` or a copy of `Table.extract()` (e.g. `Table.extract()[:]`).

            .. note::

               Once a table has been extracted to a **Pandas DataFrame** with `to_pandas()` it is easy to convert to other file types with the **Pandas API**:

               - table to Markdown, use `to_markdown <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_markdown.html#pandas.DataFrame.to_markdown>`_
               - table to JSON, use: `to_json <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_json.html>`_
               - table to Excel, use: `to_excel <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_excel.html>`_
               - table to CSV, use: `to_csv <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_csv.html>`_
               - table to HTML, use: `to_html <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_html.html>`_
               - table to SQL, use: `to_sql <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_sql.html>`_


         |history_begin|

         * New in version 1.23.0
         * Changed in version 1.23.19: new argument `add_lines`.

         |history_end|

         .. important::

            There is also the `pdf2docx extract tables method`_ which is capable of table extraction if you prefer.


      .. method:: add_stamp_annot(rect, stamp=0)
         :no-index:

         PDF only: Add a "rubber stamp" like annotation to e.g. indicate the document's intended use ("DRAFT", "CONFIDENTIAL", etc.).

         :arg rect_like rect: rectangle where to place the annotation.

         :arg int stamp: id number of the stamp text. For available stamps see :ref:`StampIcons`.

         .. note::

            * The stamp's text and its border line will automatically be sized and be put horizontally and vertically centered in the given rectangle. :attr:`Annot.rect` is automatically calculated to fit the given **width** and will usually be smaller than this parameter.
            * The font chosen is "Times Bold" and the text will be upper case.
            * The appearance can be changed using :meth:`Annot.set_opacity` and by setting the "stroke" color (no "fill" color supported).
            * This can be used to create watermark images: on a temporary PDF page create a stamp annotation with a low opacity value, make a pixmap from it with *alpha=True* (and potentially also rotate it), discard the temporary PDF page and use the pixmap with :meth:`insert_image` for your target PDF.


         .. image:: images/img-stampannot.*
            :scale: 80

      .. method:: add_widget(widget)
         :no-index:

         PDF only: Add a PDF Form field ("widget") to a page. This also **turns the PDF into a Form PDF**. Because of the large amount of different options available for widgets, we have developed a new class :ref:`Widget`, which contains the possible PDF field attributes. It must be used for both, form field creation and updates.

         :arg widget: a :ref:`Widget` object which must have been created upfront.
         :type widget: :ref:`Widget`

         :returns: a widget annotation.

      .. method:: delete_annot(annot)
         :no-index:

         * The removal will now include any bound 'Popup' or response annotations and related objects (changed in v1.16.6).

         PDF only: Delete annotation from the page and return the next one.

         :arg annot: the annotation to be deleted.
         :type annot: :ref:`Annot`

         :rtype: :ref:`Annot`
         :returns: the annotation following the deleted one. Please remember that physical removal requires saving to a new file with garbage > 0.

      .. method:: delete_widget(widget)
         :no-index:

         PDF only: Delete field from the page and return the next one.

         :arg widget: the widget to be deleted.
         :type widget: :ref:`Widget`

         :rtype: :ref:`Widget`
         :returns: the widget following the deleted one. Please remember that physical removal requires saving to a new file with garbage > 0.

         |history_begin|

         (New in v1.18.4)

         |history_end|


      .. method:: delete_link(linkdict)
         :no-index:

         PDF only: Delete the specified link from the page. The parameter must be an **original item** of :meth:`get_links()`, see :ref:`link_dict_description`. The reason for this is the dictionary's *"xref"* key, which identifies the PDF object to be deleted.

         :arg dict linkdict: the link to be deleted.

      .. method:: insert_link(linkdict)
         :no-index:

         PDF only: Insert a new link on this page. The parameter must be a dictionary of format as provided by :meth:`get_links()`, see :ref:`link_dict_description`.

         :arg dict linkdict: the link to be inserted.

      .. method:: update_link(linkdict)
         :no-index:

         PDF only: Modify the specified link. The parameter must be a (modified) **original item** of :meth:`get_links()`, see :ref:`link_dict_description`. The reason for this is the dictionary's *"xref"* key, which identifies the PDF object to be changed.

         :arg dict linkdict: the link to be modified.

         .. warning:: If updating / inserting a URI link (`"kind": LINK_URI`), please make sure to start the value for the `"uri"` key with a disambiguating string like `"http://"`, `"https://"`, `"file://"`, `"ftp://"`, `"mailto:"`, etc. Otherwise -- depending on your browser or other "consumer" software -- unexpected default assumptions may lead to unwanted behaviours.


      .. method:: get_label()
         :no-index:

         PDF only: Return the label for the page.

         :rtype: str

         :returns: the label string like "vii" for Roman numbering or "" if not defined.

         |history_begin|

         * New in v1.18.6

         |history_end|

      .. method:: get_links()
         :no-index:

         Retrieves **all** links of a page.

         :rtype: list
         :returns: A list of dictionaries. For a description of the dictionary entries, see :ref:`link_dict_description`. Always use this or the :meth:`Page.links` method if you intend to make changes to the links of a page.

      .. method:: links(kinds=None)
         :no-index:

         Return a generator over the page's links. The results equal the entries of :meth:`Page.get_links`.

         :arg sequence kinds: a sequence of integers to down-select to one or more link kinds. Default is all links. Example: *kinds=(pymupdf.LINK_GOTO,)* will only return internal links.

         :rtype: generator
         :returns: an entry of :meth:`Page.get_links()` for each iteration.

         |history_begin|

         * New in v1.16.4

         |history_end|

      .. method:: annots(types=None)
         :no-index:

         Return a generator over the page's annotations.

         :arg sequence types: a sequence of integers to down-select to one or more annotation types. Default is all annotations. Example: `types=(pymupdf.PDF_ANNOT_FREETEXT, pymupdf.PDF_ANNOT_TEXT)` will only return 'FreeText' and 'Text' annotations.

         :rtype: generator
         :returns: an :ref:`Annot` for each iteration.

            .. caution::
               You **cannot safely update annotations** from within this generator. This is because most annotation updates require reloading the page via `page = doc.reload_page(page)`. To circumvent this restriction, make a list of annotations xref numbers first and then iterate over these numbers::

                  In [4]: xrefs = [annot.xref for annot in page.annots(types=[...])]
                  In [5]: for xref in xrefs:
                     ...:     annot = page.load_annot(xref)
                     ...:     annot.update()
                     ...:     page = doc.reload_page(page)
                  In [6]:

         |history_begin|

         * New in v1.16.4

         |history_end|

      .. method:: widgets(types=None)
         :no-index:

         Return a generator over the page's form fields.

         :arg sequence types: a sequence of integers to down-select to one or more widget types. Default is all form fields. Example: `types=(pymupdf.PDF_WIDGET_TYPE_TEXT,)` will only return 'Text' fields.

         :rtype: generator
         :returns: a :ref:`Widget` for each iteration.

         |history_begin|

         * New in v1.16.4

         |history_end|


      .. method:: write_text(rect=None, writers=None, overlay=True, color=None, opacity=None, keep_proportion=True, rotate=0, oc=0)
         :no-index:

         PDF only: Write the text of one or more :ref:`Textwriter` objects to the page.

         :arg rect_like rect: where to place the text. If omitted, the rectangle union of the text writers is used.
         :arg sequence writers: a non-empty tuple / list of :ref:`TextWriter` objects or a single :ref:`TextWriter`.
         :arg float opacity: set transparency, overwrites resp. value in the text writers.
         :arg sequ color: set the text color, overwrites  resp. value in the text writers.
         :arg bool overlay: put the text in foreground or background.
         :arg bool keep_proportion: maintain the aspect ratio.
         :arg float rotate: rotate the text by an arbitrary angle.
         :arg int oc: the :data:`xref` of an :data:`OCG` or :data:`OCMD`. (New in v1.18.4)

         .. note:: Parameters *overlay, keep_proportion, rotate* and *oc* have the same meaning as in :meth:`Page.show_pdf_page`.

         |history_begin|

         * New in v1.16.18

         |history_end|

      .. method:: insert_text(point, text, *, fontsize=11, fontname="helv", fontfile=None, idx=0, color=None, fill=None, render_mode=0, miter_limit=1, border_width=0.05, encoding=TEXT_ENCODING_LATIN, rotate=0, morph=None, stroke_opacity=1, fill_opacity=1, overlay=True, oc=0)
         :no-index:

         PDF only: Insert text lines starting at :data:`point_like` ``point``. See :meth:`Shape.insert_text`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: insert_textbox(rect, buffer, *, fontsize=11, fontname="helv", fontfile=None, idx=0, color=None, fill=None, render_mode=0, miter_limit=1, border_width=1, encoding=TEXT_ENCODING_LATIN, expandtabs=8, align=TEXT_ALIGN_LEFT, charwidths=None, rotate=0, morph=None, stroke_opacity=1, fill_opacity=1, oc=0, overlay=True)
         :no-index:

         PDF only: Insert text into the specified :data:`rect_like` *rect*. See :meth:`Shape.insert_textbox`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: insert_htmlbox(rect, text, *, css=None, scale_low=0, archive=None, rotate=0, oc=0, opacity=1, overlay=True)
         :no-index:

         **PDF only:** Insert text into the specified rectangle. The method has similarities with methods :meth:`Page.insert_textbox` and :meth:`TextWriter.fill_textbox`, but is **much more powerful**. This is achieved by letting a :ref:`Story` object do all the required processing.

         * Parameter ``text`` may be a string as in the other methods. But it will be **interpreted as HTML source** and may therefore also contain HTML language elements -- including styling. The `css` parameter may be used to pass in additional styling instructions.

         * Automatic line breaks are generated at word boundaries. The "soft hyphen" character `"&#173;"` (or `&shy;`) can be used to cause hyphenation and thus may also cause line breaks. **Forced** line breaks however are only achievable via the HTML tag ``<br>`` - ``\n`` is ignored and will be treated like a space.

         * With this method the following can be achieved:

         - Styling effects like bold, italic, text color, text alignment, font size or font switching.
         - The text may include arbitrary languages -- **including right-to-left** languages.
         - Scripts like `Devanagari <https://en.wikipedia.org/wiki/Devanagari>`_ and several others in Asia have a highly complex system of ligatures, where two or more unicodes together yield one glyph. The Story uses the software package `HarfBuzz <https://harfbuzz.github.io/>`_ , to deal with these things and produce correct output.
         - One can also **include images** via HTML tag `<img>` -- the Story will take care of the appropriate layout. This is an alternative option to insert images, compared to :meth:`Page.insert_image`.
         - HTML tables (tag `<table>`) may be included in the text and will be handled appropriately.
         - Links are automatically generated when present.

         * If content does not fit in the rectangle, the developer has two choices:
            
         - **either** only be informed about this (and accept a no-op, just like with the other textbox insertion methods), 
         - **or** (`scale_low=0` - the default) scale down the content until it fits.

         :arg rect_like rect: rectangle on page to receive the text.
         :arg str,Story text: the text to be written. Can contain a mixture of plain text and HTML tags with styling instructions. Alternatively, a :ref:`Story` object may be specified (in which case the internal Story generation step will be omitted). A Story must have been generated with all required styling and Archive information.
         :arg str css: optional string containing additional CSS instructions. This parameter is ignored if ``text`` is a Story.
         :arg float scale_low: if necessary, scale down the content until it fits in the target rectangle. This sets the down scaling limit. Default is 0, no limit. A value of 1 means no down-scaling permitted. A value of e.g. 0.2 means maximum down-scaling by 80%.
         :arg Archive archive: an Archive object that points to locations where to find images or non-standard fonts. If ``text`` refers to images or non-standard fonts, this parameter is required. This parameter is ignored if ``text`` is a Story.
         :arg int rotate: one of the values 0, 90, 180, 270. Depending on this, text will be filled:
         
            - 0: top-left to bottom-right.
            - 90: bottom-left to top-right.
            - 180: bottom-right to top-left.
            - 270: top-right to bottom-left.

            .. image:: images/img-rotate.*

         :arg int oc:  the xref of an :data:`OCG` / :data:`OCMD` or 0. Please refer to :meth:`Page.show_pdf_page` for details.
         :arg float opacity: set the fill and stroke opacity of the content. Only values `0 <= opacity < 1` are considered.
         :arg bool overlay: put the text in front of other content. Please refer to :meth:`Page.show_pdf_page` for details.

         :returns: A tuple of floats `(spare_height, scale)`.

            - `spare_height`: -1 if content did not fit, else >= 0. It is the height of the unused (still available) rectangle stripe. Positive only if scale = 1 (no down-scaling happened).
            - `scale`: down-scaling factor, 0 < scale <= 1.

            Please refer to examples in this section of the recipes: :ref:`RecipesText_I_c`.

         |history_begin|

         * New in v1.23.8; rebased-only.
         * New in v1.23.9: `opacity` parameter.

         |history_end|
         

      **Drawing Methods**

      .. method:: draw_line(p1, p2, color=(0,), width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         PDF only: Draw a line from *p1* to *p2* (:data:`point_like` \s). See :meth:`Shape.draw_line`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: draw_zigzag(p1, p2, breadth=2, color=(0,), width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         PDF only: Draw a zigzag line from *p1* to *p2* (:data:`point_like` \s). See :meth:`Shape.draw_zigzag`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: draw_squiggle(p1, p2, breadth=2, color=(0,), width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         PDF only: Draw a squiggly (wavy, undulated) line from *p1* to *p2* (:data:`point_like` \s). See :meth:`Shape.draw_squiggle`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: draw_circle(center, radius, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         PDF only: Draw a circle around *center* (:data:`point_like`) with a radius of *radius*. See :meth:`Shape.draw_circle`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: draw_oval(quad, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         PDF only: Draw an oval (ellipse) within the given :data:`rect_like` or :data:`quad_like`. See :meth:`Shape.draw_oval`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: draw_sector(center, point, angle, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, fullSector=True, overlay=True, closePath=False, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         PDF only: Draw a circular sector, optionally connecting the arc to the circle's center (like a piece of pie). See :meth:`Shape.draw_sector`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: draw_polyline(points, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, closePath=False, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         PDF only: Draw several connected lines defined by a sequence of :data:`point_like` \s. See :meth:`Shape.draw_polyline`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: draw_bezier(p1, p2, p3, p4, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, closePath=False, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         PDF only: Draw a cubic Bézier curve from *p1* to *p4* with the control points *p2* and *p3* (all are :data:`point_like` \s). See :meth:`Shape.draw_bezier`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: draw_curve(p1, p2, p3, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, closePath=False, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         PDF only: This is a special case of *draw_bezier()*. See :meth:`Shape.draw_curve`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: draw_rect(rect, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, radius=None, oc=0)
         :no-index:

         PDF only: Draw a rectangle. See :meth:`Shape.draw_rect`.

         |history_begin|

         * Changed in v1.18.4
         * Changed in v1.22.0: Added parameter *radius*.

         |history_end|

      .. method:: draw_quad(quad, color=(0,), fill=None, width=1, dashes=None, lineCap=0, lineJoin=0, overlay=True, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         PDF only: Draw a quadrilateral. See :meth:`Shape.draw_quad`.

         |history_begin|

         * Changed in v1.18.4

         |history_end|

      .. method:: insert_font(fontname="helv", fontfile=None, fontbuffer=None, set_simple=False, encoding=TEXT_ENCODING_LATIN)
         :no-index:

         PDF only: Add a new font to be used by text output methods and return its :data:`xref`. If not already present in the file, the font definition will be added. Supported are the built-in :data:`Base14_Fonts` and the CJK fonts via **"reserved"** fontnames. Fonts can also be provided as a file path or a memory area containing the image of a font file.

         :arg str fontname: The name by which this font shall be referenced when outputting text on this page. In general, you have a "free" choice here (but consult the :ref:`AdobeManual`, page 16, section 7.3.5 for a formal description of building legal PDF names). However, if it matches one of the :data:`Base14_Fonts` or one of the CJK fonts, *fontfile* and *fontbuffer* **are ignored**.

         In other words, you cannot insert a font via *fontfile* / *fontbuffer* and also give it a reserved *fontname*.

         .. note:: A reserved fontname can be specified in any mixture of upper or lower case and still match the right built-in font definition: fontnames "helv", "Helv", "HELV", "Helvetica", etc. all lead to the same font definition "Helvetica". But from a :ref:`Page` perspective, these are **different references**. You can exploit this fact when using different *encoding* variants (Latin, Greek, Cyrillic) of the same font on a page.

         :arg str fontfile: a path to a font file. If used, *fontname* must be **different from all reserved names**.

         :arg bytes/bytearray fontbuffer: the memory image of a font file. If used, *fontname* must be **different from all reserved names**. This parameter would typically be used with :attr:`Font.buffer` for fonts supported / available via :ref:`Font`.

         :arg int set_simple: applicable for *fontfile* / *fontbuffer* cases only: enforce treatment as a "simple" font, i.e. one that only uses character codes up to 255.

         :arg int encoding: applicable for the "Helvetica", "Courier" and "Times" sets of :data:`Base14_Fonts` only. Select one of the available encodings Latin (0), Cyrillic (2) or Greek (1). Only use the default (0 = Latin) for "Symbol" and "ZapfDingBats".

         :rytpe: int
         :returns: the :data:`xref` of the installed font.

         .. note:: Built-in fonts will not lead to the inclusion of a font file. So the resulting PDF file will remain small. However, your PDF viewer software is responsible for generating an appropriate appearance -- and there **exist** differences on whether or how each one of them does this. This is especially true for the CJK fonts. But also Symbol and ZapfDingbats are incorrectly handled in some cases. Following are the **Font Names** and their correspondingly installed **Base Font** names:

            **Base-14 Fonts** [#f1]_

            ============= ============================ =========================================
            **Font Name** **Installed Base Font**      **Comments**
            ============= ============================ =========================================
            helv          Helvetica                    normal
            heit          Helvetica-Oblique            italic
            hebo          Helvetica-Bold               bold
            hebi          Helvetica-BoldOblique        bold-italic
            cour          Courier                      normal
            coit          Courier-Oblique              italic
            cobo          Courier-Bold                 bold
            cobi          Courier-BoldOblique          bold-italic
            tiro          Times-Roman                  normal
            tiit          Times-Italic                 italic
            tibo          Times-Bold                   bold
            tibi          Times-BoldItalic             bold-italic
            symb          Symbol                       [#f3]_
            zadb          ZapfDingbats                 [#f3]_
            ============= ============================ =========================================

            **CJK Fonts** [#f2]_ (China, Japan, Korea)

            ============= ============================ =========================================
            **Font Name** **Installed Base Font**      **Comments**
            ============= ============================ =========================================
            china-s       Heiti                        simplified Chinese
            china-ss      Song                         simplified Chinese (serif)
            china-t       Fangti                       traditional Chinese
            china-ts      Ming                         traditional Chinese (serif)
            japan         Gothic                       Japanese
            japan-s       Mincho                       Japanese (serif)
            korea         Dotum                        Korean
            korea-s       Batang                       Korean (serif)
            ============= ============================ =========================================

      .. method:: insert_image(rect, *, alpha=-1, filename=None, height=0, keep_proportion=True, mask=None, oc=0, overlay=True, pixmap=None, rotate=0, stream=None, width=0, xref=0)
         :no-index:

         PDF only: Put an image inside the given rectangle. The image may already
         exist in the PDF or be taken from a pixmap, a file, or a memory area.

         :arg rect_like rect: where to put the image. Must be finite and not empty.
         :arg int alpha: deprecated and ignored.
         :arg str filename:
         name of an image file (all formats supported by MuPDF -- see
         :ref:`ImageFiles`).
         :arg int height:
         :arg bool keep_proportion:
         maintain the aspect ratio of the image.
         :arg bytes,bytearray,io.BytesIO mask:
         image in memory -- to be used as image mask (alpha values) for the base
         image. When specified, the base image must be provided as a filename or
         a stream -- and must not be an image that already has a mask.
         :arg int oc:
         (:data:`xref`) make image visibility dependent on this :data:`OCG`
         or :data:`OCMD`. Ignored after the first of multiple insertions. The
         property is stored with the generated PDF image object and therefore
         controls the image's visibility throughout the PDF.
         :arg overlay: see :ref:`CommonParms`.
         :arg pixmap: a pixmap containing the image.
         :arg int rotate: rotate the image.
         Must be an integer multiple of 90 degrees.
         Positive values rotate anti-clockwise.
         If you need a rotation by an arbitrary angle,
         consider converting the image to a PDF
         (:meth:`Document.convert_to_pdf`)
         first and then use :meth:`Page.show_pdf_page` instead.
         :arg bytes,bytearray,io.BytesIO stream:
         image in memory (all formats supported by MuPDF -- see :ref:`ImageFiles`).
         :arg int width:
         :arg int xref:
         the :data:`xref` of an image already present in the PDF. If given,
         parameters `filename`, `pixmap`, `stream`, `alpha` and `mask` are
         ignored. The page will simply receive a reference to the existing
         image.

         :type pixmap: :ref:`Pixmap`
         
         :returns:
         The `xref` of the embedded image. This can be used as the `xref`
         argument for very significant performance boosts, if the image is
         inserted again.

         This example puts the same image on every page of a document::

            >>> doc = pymupdf.open(...)
            >>> rect = pymupdf.Rect(0, 0, 50, 50)       # put thumbnail in upper left corner
            >>> img = open("some.jpg", "rb").read()  # an image file
            >>> img_xref = 0                         # first execution embeds the image
            >>> for page in doc:
                  img_xref = page.insert_image(rect, stream=img,
                           xref=img_xref,  2nd time reuses existing image
                     )
            >>> doc.save(...)

         .. note::

            1.
            The method detects multiple insertions of the same image (like
            in the above example) and will store its data only on the first
            execution.  This is even true (although less performant), if using
            the default `xref=0`.
            2.
            The method cannot detect if the same image had already been part of
            the file before opening it.

            3.
            You can use this method to provide a background or foreground image
            for the page, like a copyright or a watermark. Please remember, that
            watermarks require a transparent image if put in foreground ...

            4.
            The image may be inserted uncompressed, e.g. if a `Pixmap` is used
            or if the image has an alpha channel. Therefore, consider using
            `deflate=True` when saving the file. In addition, there are ways to
            control the image size -- even if transparency comes into play. Have
            a look at :ref:`RecipesImages_O`.

            5.
            The image is stored in the PDF at its original quality level. This
            may be much better than what you need for your display. Consider
            **decreasing the image size** before insertion -- e.g. by using
            the pixmap option and then shrinking it or scaling it down (see
            :ref:`Pixmap` chapter). The PIL method `Image.thumbnail()` can
            also be used for that purpose. The file size savings can be very
            significant.

            6.
            Another efficient way to display the same image on multiple
            pages is another method: :meth:`show_pdf_page`. Consult
            :meth:`Document.convert_to_pdf` for how to obtain intermediary PDFs
            usable for that method.

         |history_begin|

         * Changed in v1.14.1: By default, the image keeps its aspect ratio.
         * Changed in v1.14.11: Added args `keep_proportion`, `rotate`.
         * Changed in v1.14.13:

         *
            The image is now always placed **centered** in the rectangle, i.e.
            the centers of image and rectangle are equal.
         * Added support for `stream` as `io.BytesIO`.
         
         * Changed in v1.17.6:
         Insertion rectangle no longer needs to have a non-empty intersection
         with the page's :attr:`Page.cropbox` [#f5]_.
         * Changed in v1.18.1: Added `mask` arg.
         * Changed in v1.18.3: Added `oc` arg.
         * Changed in v1.18.13:
         
         * Allow providing the image as the xref of an existing one.
         * Added `xref` arg.
         * Return `xref` of stored image.
         
         * Changed in v1.19.3: deprecate and ignore `alpha` arg.

         |history_end|

      .. method:: replace_image(xref, filename=None, pixmap=None, stream=None)
         :no-index:

         Replace the image at xref with another one.

         :arg int xref: the :data:`xref` of the image.
         :arg filename: the filename of the new image.
         :arg pixmap: the :ref:`Pixmap` of the new image.
         :arg stream: the memory area containing the new image.

         Arguments `filename`, `pixmap`, `stream` have the same meaning as in :meth:`Page.insert_image`, especially exactly one of these must be provided.

         This is a **global replacement:** the new image will also be shown wherever the old one has been displayed throughout the file.

         This method mainly exists for technical purposes. Typical uses include replacing large images by smaller versions, like a lower resolution, graylevel instead of colored, etc., or changing transparency.

         |history_begin|

         * New in v1.21.0

         |history_end|

      .. method:: delete_image(xref)
         :no-index:

         Delete the image at xref. This is slightly misleading: actually the image is being replaced with a small transparent :ref:`Pixmap` using above :meth:`Page.replace_image`. The visible effect however is equivalent.

         :arg int xref: the :data:`xref` of the image.

         This is a **global replacement:** the image will disappear wherever the old one has been displayed throughout the file.
      
         If you inspect / extract a page's images by methods like :meth:`Page.get_images`,
         :meth:`Page.get_image_info` or :meth:`Page.get_text`,
         the replacing "dummy" image will be detected like so
         `(45, 47, 1, 1, 8, 'DeviceGray', '', 'Im1', 'FlateDecode')`
         and also seem to "cover" the same boundary box on the page.

         |history_begin|

         * New in v1.21.0

         |history_end|

      .. method:: get_text(option,*, clip=None, flags=None, textpage=None, sort=False, delimiters=None)
         :no-index:

         Retrieves the content of a page in a variety of formats. Depending on the ``flags`` value, this may include text, images and several other object types. The method is a wrapper for multiple :ref:`TextPage` methods by choosing the output option `opt` as follows:

         * "text" -- :meth:`TextPage.extractTEXT`, default. Always includes **text only.**
         * "blocks" -- :meth:`TextPage.extractBLOCKS`. Includes text and **may** include image meta information.
         * "words" -- :meth:`TextPage.extractWORDS`. Always includes **text only.**
         * "html" -- :meth:`TextPage.extractHTML`. May include text and images.
         * "xhtml" -- :meth:`TextPage.extractXHTML`. May include text and images.
         * "xml" -- :meth:`TextPage.extractXML`. Always includes **text only.**
         * "dict" -- :meth:`TextPage.extractDICT`. May include text and images.
         * "json" -- :meth:`TextPage.extractJSON`. May include text and images.
         * "rawdict" -- :meth:`TextPage.extractRAWDICT`. May include text and images.
         * "rawjson" -- :meth:`TextPage.extractRAWJSON`. May include text and images.

         :arg str opt: A string indicating the requested format, one of the above. A mixture of upper and lower case is supported. If misspelled, option "text" is silently assumed.

         :arg rect-like clip: restrict the extraction to this rectangle. If ``None`` (default), the visible part of the page is taken. Any content (text, images) that is **not fully contained** in ``clip`` will be completely omitted. To avoid clipping altogether use ``clip=pymupdf.INFINITE_RECT()``. Only then the extraction will contain all items. This parameter has **no effect** on options "html", "xhtml" and "xml".

         :arg int flags: indicator bits to control whether to include images or how text should be handled with respect to white spaces and :data:`ligatures`. See :ref:`TextPreserve` for available indicators and :ref:`text_extraction_flags` for default settings. (New in v1.16.2)

         :arg textpage: use a previously created :ref:`TextPage`. This reduces execution time **very significantly:** by more than 50% and up to 95%, depending on the extraction option. If specified, the 'flags' and 'clip' arguments are ignored, because they are textpage-only properties. If omitted, a new, temporary textpage will be created.

         :arg bool sort: sort the output by vertical, then horizontal coordinates. In many cases, this should suffice to generate a "natural" reading order. Has no effect on (X)HTML and XML. For options "blocks", "dict", "json", "rawdict", "rawjson", sorting happens by coordinates `(y1, x0)` of the respective block bbox. For options "words" and "text", the text lines are completely re-synthesized to follow the reading sequence and appearance in the document -- which even establishes the original layout to some extent.

         :arg str delimiters: use these characters as *additional* word separators with the "words" output option (ignored otherwise). By default, all white spaces (including non-breaking space `0xA0`) indicate start and end of a word. Now you can specify more characters causing this. For instance, the default will return `"john.doe@outlook.com"` as **one** word. If you specify `delimiters="@."` then the **four** words `"john"`, `"doe"`, `"outlook"`, `"com"` will be returned. Other possible uses include ignoring punctuation characters `delimiters=string.punctuation`. The "word" strings will not contain any delimiting character. (New in v1.23.5)

         :rtype: *str, list, dict*
         :returns: The page's content as a string, a list or a dictionary. Refer to the corresponding :ref:`TextPage` method for details.

         .. note::

         1. You can use this method as a **document conversion tool** from :ref:`any supported document type<Supported_File_Types>` to one of TEXT, HTML, XHTML or XML documents.
         2. The inclusion of text via the *clip* parameter is decided on a by-character level: a character becomes part of the output, if its bbox is contained in `clip`. This **deviates** from the algorithm used in redaction annotations: a character will be **removed if its bbox intersects** any redaction annotation.

         |history_begin|

         * Changed in v1.19.0: added `textpage` parameter
         * Changed in v1.19.1: added `sort` parameter
         * Changed in v1.19.6: added new constants for defining default flags per method.
         * Changed in v1.23.5: added `delimiters` parameter
         * Changed in v1.24.11: changed the effect of `sort_True` for "text" and "words" to closely follow natural reading sequence.

         |history_end|

      .. method:: get_textbox(rect, textpage=None)
         :no-index:

         Retrieve the text contained in a rectangle.

         :arg rect-like rect: rect-like.
         :arg textpage: a :ref:`TextPage` to use. If omitted, a new, temporary textpage will be created.

         :returns: a string with interspersed linebreaks where necessary. It is based on dedicated code (changed in v1.19.0). A typical use is checking the result of :meth:`Page.search_for`:

         >>> rl = page.search_for("currency:")
         >>> page.get_textbox(rl[0])
         'Currency:'
         >>>

         |history_begin|

         * New in v1.17.7
         * Changed in v1.19.0: add `textpage` parameter

         |history_end|

      .. method:: get_textpage(clip=None, flags=3)
         :no-index:

         Create a :ref:`TextPage` for the page.

         :arg int flags: indicator bits controlling the content available for subsequent text extractions and searches -- see the parameter of :meth:`Page.get_text`.

         :arg rect-like clip: restrict extracted text to this area. (New in v1.17.7)

         :returns: :ref:`TextPage`

         |history_begin|

         * New in v1.16.5
         * Changed in v1.17.7: introduced `clip` parameter.

         |history_end|

      .. method:: get_textpage_ocr(flags=3, language="eng", dpi=72, full=False, tessdata=None)
         :no-index:

         **Optical Character Recognition** (**OCR**) technology can be used to extract text data for documents where text is in a raster image format throughout the page. Use this method to **OCR** a page for text extraction.

         This method returns a :ref:`TextPage` for the page that includes OCRed text. MuPDF will invoke Tesseract-OCR if this method is used. Otherwise this is a normal :ref:`TextPage` object.

         :arg int flags: indicator bits controlling the content available for subsequent test extractions and searches -- see the parameter of :meth:`Page.get_text`.
         :arg str language: the expected language(s). Use "+"-separated values if multiple languages are expected, "eng+spa" for English and Spanish.
         :arg int dpi: the desired resolution in dots per inch. Influences recognition quality (and execution time).
         :arg bool full: whether to OCR the full page, or just the displayed images.
         :arg str tessdata: The name of Tesseract's language support folder `tessdata`. If omitted, this information must be present as environment variable `TESSDATA_PREFIX`. Can be determined by function :meth:`get_tessdata`.

         .. note:: This method does **not** support a clip parameter -- OCR will always happen for the complete page rectangle.

         :returns:
         
            a :ref:`TextPage`. Execution may be significantly longer than :meth:`Page.get_textpage`.

            For a full page OCR, **all text** will have the font "GlyphlessFont" from Tesseract. In case of partial OCR, normal text will keep its properties, and only text coming from images will have the GlyphlessFont.

            .. note::
            
               **OCRed text is only available** to PyMuPDF's text extractions and searches if their `textpage` parameter specifies the output of this method.

               `This Jupyter notebook <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/jupyter-notebooks/partial-ocr.ipynb>`_ walks through an example for using OCR textpages.

         |history_begin|

         * New in v.1.19.0
         * Changed in v1.19.1: support full and partial OCRing a page.

         |history_end|


      .. method:: get_drawings(extended=False)
         :no-index:

         Return the vector graphics of the page. These are instructions which draw lines, rectangles, quadruples or curves, including properties like colors, transparency, line width and dashing, etc. Alternative terms are "line art" and "drawings".

         :returns: a list of dictionaries. Each dictionary item contains one or more single draw commands belonging together: they have the same properties (colors, dashing, etc.). This is called a **"path"** in PDF, so we adopted that name here, but the method **works for all document types**.

         The path dictionary for fill, stroke and fill-stroke paths has been designed to be compatible with class :ref:`Shape`. There are the following keys:

         ============== ============================================================================
         Key            Value
         ============== ============================================================================
         closePath      Same as the parameter in :ref:`Shape`.
         color          Stroke color (see :ref:`Shape`).
         dashes         Dashed line specification (see :ref:`Shape`).
         even_odd       Fill colors of area overlaps -- same as the parameter in :ref:`Shape`.
         fill           Fill color  (see :ref:`Shape`).
         items          List of draw commands: lines, rectangles, quads or curves.
         lineCap        Number 3-tuple, use its max value on output with :ref:`Shape`.
         lineJoin       Same as the parameter in :ref:`Shape`.
         fill_opacity   fill color transparency (see :ref:`Shape`). (New in v1.18.17)
         stroke_opacity stroke color transparency  (see :ref:`Shape`). (New in v1.18.17)
         rect           Page area covered by this path. Information only.
         layer          name of applicable Optional Content Group. (New in v1.22.0)
         level          the hierarchy level if `extended=True`. (New in v1.22.0)
         seqno          command number when building page appearance. (New in v1.19.0)
         type           type of this path. (New in v1.18.17)
         width          Stroke line width. (see :ref:`Shape`).
         ============== ============================================================================

         Key `"opacity"` has been replaced by the new keys `"fill_opacity"` and `"stroke_opacity"`. This is now compatible with the corresponding parameters of :meth:`Shape.finish`. (Changed in v1.18.17)


         For paths other than groups or clips, key `"type"` takes one of the following values:

         * **"f"** -- this is a *fill-only* path. Only key-values relevant for this operation have a meaning, not applicable ones are present with a value of ``None``: `"color"`, `"lineCap"`, `"lineJoin"`, `"width"`, `"closePath"`, `"dashes"` and should be ignored.
         * **"s"** -- this is a *stroke-only* path. Similar to previous, key `"fill"` is present with value ``None``.
         * **"fs"** -- this is a path performing combined *fill* and *stroke* operations.

         Each item in `path["items"]` is one of the following:

         * `("l", p1, p2)` - a line from p1 to p2 (:ref:`Point` objects).
         * `("c", p1, p2, p3, p4)` - cubic Bézier curve **from p1 to p4** (p2 and p3 are the control points). All objects are of type :ref:`Point`.
         * `("re", rect, orientation)` - a :ref:`Rect`. Multiple rectangles within the same path are now detected (changed in v1.18.17). Integer `orientation` is 1 resp. -1 indicating whether the enclosed area is rotated left (1 = anti-clockwise), or resp. right [#f7]_ (changed in v1.19.2).
         * `("qu", quad)` - a :ref:`Quad`. 3 or 4 consecutive lines are detected to actually represent a :ref:`Quad` (changed in v1.19.2:). (New in v1.18.17)

         .. note::, quads and rectangles are more reliably recognized as such. (Starting with v1.19.2)

         Using class :ref:`Shape`, you should be able to recreate the original drawings on a separate (PDF) page with high fidelity under normal, not too sophisticated circumstances. Please see the following comments on restrictions. A coding draft can be found in :ref:`How to Extract Drawings <RecipesDrawingAndGraphics_Extract_Drawings>`.

         Specifying `extended=True` significantly alters the output. Most importantly, new dictionary types are present: "clip" and "group". All paths will now be organized in a hierarchic structure which is encoded by the new integer key "level", the hierarchy level. Each group or clip establishes a new hierarchy, which applies to all subsequent paths having a *larger* level value. (New in v1.22.0)

         Any path with a smaller level value than its predecessor will end the scope of (at least) the preceding hierarchy level. A "clip" path with the same level as the preceding clip will end the scope of that clip. Same is true for groups. This is best explained by an example::

            +------+------+--------+------+--------+
            | line | lvl0 | lvl1   | lvl2 |  lvl3  |
            +------+------+--------+------+--------+
            |  0   | clip |        |      |        |
            |  1   |      | fill   |      |        |
            |  2   |      | group  |      |        |
            |  3   |      |        | clip |        |
            |  4   |      |        |      | stroke |
            |  5   |      |        | fill |        |  ends scope of clip in line 3
            |  6   |      | stroke |      |        |  ends scope of group in line 2
            |  7   |      | clip   |      |        |
            |  8   | fill |        |      |        |  ends scope of line 0
            +------+------+--------+------+--------+

         The clip in line 0 applies to line including line 7. Group in line 2 applies to lines 3 to 5, clip in line 3 only applies to line 4.

         "stroke" in line 4 is under control of "group" in line 2 and "clip" in line 3 (which in turn is a subset of line 0 clip).

         * **"clip"** dictionary. Its values (most importantly "scissor") remain valid / apply as long as following dictionaries have a **larger "level"** value.

         ============== ============================================================================
         Key            Value
         ============== ============================================================================
         closePath      Same as in "stroke" or "fill" dictionaries
         even_odd       Same as in "stroke" or "fill" dictionaries
         items          Same as in "stroke" or "fill" dictionaries
         rect           Same as in "stroke" or "fill" dictionaries
         layer          Same as in "stroke" or "fill" dictionaries
         level          Same as in "stroke" or "fill" dictionaries
         scissor        the clip rectangle
         type           "clip"
         ============== ============================================================================

         * "group" dictionary. Its values remain valid (apply) as long as following dictionaries have a **larger "level"** value. Any dictionary with an equal or lower level end this group.

         ============== ============================================================================
         Key            Value
         ============== ============================================================================
         rect           Same as in "stroke" or "fill" dictionaries
         layer          Same as in "stroke" or "fill" dictionaries
         level          Same as in "stroke" or "fill" dictionaries
         isolated       (bool) Whether this group is isolated
         knockout       (bool) Whether this is a "Knockout Group"
         blendmode      Name of the BlendMode, default is "Normal"
         opacity        Float value in range [0, 1].
         type           "group"
         ============== ============================================================================

         .. note:: The method is based on the output of :meth:`Page.get_cdrawings` -- which is much faster, but requires somewhat more attention processing its output.

         |history_begin|
         
         * New in v1.18.0
         * Changed in v1.18.17
         * Changed in v1.19.0: add "seqno" key, remove "clippings" key
         * Changed in v1.19.1: "color" / "fill" keys now always are either are RGB tuples or `None`. This resolves issues caused by exotic colorspaces.
         * Changed in v1.19.2: add an indicator for the *"orientation"* of the area covered by an "re" item.
         * Changed in v1.22.0: add new key `"layer"` which contains the name of the Optional Content Group of the path (or `None`).
         * Changed in v1.22.0: add parameter `extended` to also return clipping and group paths.
         
         |history_end|



      .. method:: get_cdrawings(extended=False)
         :no-index:

         Extract the vector graphics on the page. Apart from following technical differences, functionally equivalent to :meth:`Page.get_drawings`, but much faster:

         * Every path type only contains the relevant keys, e.g. a stroke path has no `"fill"` color key. See comment in method :meth:`Page.get_drawings`.
         * Coordinates are given as :data:`point_like`, :data:`rect_like` and :data:`quad_like` **tuples** -- not as :ref:`Point`, :ref:`Rect`, :ref:`Quad` objects.

         If performance is a concern, consider using this method: Compared to versions earlier than 1.18.17, you should see much shorter response times. We have seen pages that required 2 seconds then, now only need 200 ms with this method.

         |history_begin|

         * New in v1.18.17
         * Changed in v1.19.0: removed "clippings" key, added "seqno" key.
         * Changed in v1.19.1: always generate RGB color tuples.
         * Changed in v1.22.0: added new key `"layer"` which contains the name of the Optional Content Group of the path (or `None`).
         * Changed in v1.22.0: added parameter `extended` to also return clipping paths.
         
         |history_end|


      .. method:: get_fonts(full=False)
         :no-index:

         PDF only: Return a list of fonts referenced by the page. Wrapper for :meth:`Document.get_page_fonts`.


      .. method:: get_images(full=False)
         :no-index:

         PDF only: Return a list of images referenced by the page. Wrapper for :meth:`Document.get_page_images`.

      .. method:: get_image_info(hashes=False, xrefs=False)
         :no-index:

         Return a list of meta information dictionaries for all images displayed by the page. This works for all document types.

         :arg bool hashes: Compute the MD5 hashcode for each encountered image, which allows identifying image duplicates. This adds the key `"digest"` to the output, whose value is a 16 byte `bytes` object. (New in v1.18.13)

         :arg bool xrefs: **PDF only.** Try to find the :data:`xref` for each image. Implies `hashes=True`. Adds the `"xref"` key to the dictionary. If not found, the value is 0, which means, the image is either "inline" or its xref is undetectable for some reason. Please note that this option has an extended response time, because the MD5 hashcode will be computed at least two times for each image with an xref. (New in v1.18.13)

         :rtype: list[dict]
         :returns: A list of dictionaries. This includes information for **exactly those** images, that are shown on the page -- including *"inline images"*. The dictionary layout is similar to that of image blocks in `page.get_text("dict")`.
         
            In contrast to images included in :meth:`Page.get_text`, image **binary content** is not loaded by this method, which drastically reduces memory usage. Another difference is that image detection is not restricted to the visible part of the page or any ``clip`` parameter: method :meth:`Page.get_text` will only extract images **fully contained** in the provided ``clip``.

            =============== ===============================================================
            **Key**             **Value**
            =============== ===============================================================
            number          block number (``int``)
            bbox            image bbox on page, :data:`rect_like`
            width           original image width (``int``)
            height          original image height (``int``)
            cs-name         colorspace name (``str``)
            colorspace      colorspace.n (``int``)
            xres            resolution in x-direction (``int``)
            yres            resolution in y-direction (``int``)
            bpc             bits per component (``int``)
            size            storage occupied by image (``int``)
            digest          MD5 hashcode (``bytes``), if ``hashes`` is true
            xref            image :data:`xref` or 0, if *xrefs* is true
            transform       matrix transforming image rect to bbox, :data:`matrix_like`
            has-mask        whether the image is transparent and has a mask (``bool``)
            =============== ===============================================================

            Multiple occurrences of the same image are always reported. You can detect duplicates by comparing their `digest` values.

         |history_begin|

         * New in v1.18.11
         * Changed in v1.18.13: added image MD5 hashcode computation and :data:`xref` search.

         |history_end|


      .. method:: get_xobjects()
         :no-index:

         PDF only: Return a list of Form XObjects referenced by the page. Wrapper for :meth:`Document.get_page_xobjects`.

      .. method:: get_image_rects(item, transform=False)
         :no-index:

         PDF only: Return boundary boxes and transformation matrices of an embedded image. This is an improved version of :meth:`Page.get_image_bbox` with the following differences:

         * There is no restriction on **how** the image is invoked (by the page or one of its Form XObjects). The result is always complete and correct.
         * The result is a list of :ref:`Rect` or (:ref:`Rect`, :ref:`Matrix`) objects -- depending on *transform*. Each list item represents one location of the image on the page. Multiple occurrences might not be detectable by :meth:`Page.get_image_bbox`.
         * The method invokes :meth:`Page.get_image_info` with `xrefs=True` and therefore has a noticeably longer response time than :meth:`Page.get_image_bbox`.

         :arg list,str,int item: an item of the list :meth:`Page.get_images`, or the reference **name** entry of such an item (item[7]), or the image :data:`xref`.
         :arg bool transform: also return the matrix used to transform the image rectangle to the bbox on the page. If true, then tuples `(bbox, matrix)` are returned.

         :rtype: list
         :returns: Boundary boxes and respective transformation matrices for each image occurrence on the page. If the item is not on the page, an empty list `[]` is returned.

         |history_begin|

         New in v1.18.13

         |history_end|

      .. method:: get_image_bbox(item, transform=False)
         :no-index:

         PDF only: Return boundary box and transformation matrix of an embedded image.

         :arg list,str item: an item of the list :meth:`Page.get_images` with *full=True* specified, or the reference **name** entry of such an item, which is item[-3] (or item[7] respectively).
         :arg bool transform: return the matrix used to transform the image rectangle to the bbox on the page (new in v1.18.11). Default is just the bbox. If true, then a tuple `(bbox, matrix)` is returned.

         :rtype: :ref:`Rect` or (:ref:`Rect`, :ref:`Matrix`)
         :returns: the boundary box of the image -- optionally also its transformation matrix.

         |history_begin|
         
         * (Changed in v1.16.7): If the page in fact does not display this image, an infinite rectangle is returned now. In previous versions, an exception was raised. Formally invalid parameters still raise exceptions.
         * (Changed in v1.17.0): Only images referenced directly by the page are considered. This means that images occurring in embedded PDF pages are ignored and an exception is raised.
         * (Changed in v1.18.5): Removed the restriction introduced in v1.17.0: any item of the page's image list may be specified.
         * (Changed in v1.18.11): Partially re-instated a restriction: only those images are considered, that are either directly referenced by the page or by a Form XObject directly referenced by the page.
         * (Changed in v1.18.11): Optionally also return the transformation matrix together with the bbox as the tuple `(bbox, transform)`.

         |history_end|

         .. note::

            1. Be aware that :meth:`Page.get_images` may contain "dead" entries i.e. images, which the page **does not display**. This is no error, but intended by the PDF creator. No exception will be raised in this case, but an infinite rectangle is returned. You can avoid this from happening by executing :meth:`Page.clean_contents` before this method.
            2. The image's "transformation matrix" is defined as the matrix, for which the expression `bbox / transform == pymupdf.Rect(0, 0, 1, 1)` is true, lookup details here: :ref:`ImageTransformation`.

         |history_begin|

         * Changed in v1.18.11: return image transformation matrix

         |history_end|

      .. method:: get_svg_image(matrix=pymupdf.Identity, text_as_path=True)
         :no-index:

         Create an SVG image from the page. Only full page images are currently supported.

         :arg matrix_like matrix: a matrix, default is :ref:`Identity`.
         :arg bool text_as_path: -- controls how text is represented. ``True`` outputs each character as a series of elementary draw commands, which leads to a more precise text display in browsers, but a **very much larger** output for text-oriented pages. Display quality for ``False`` relies on the presence of the referenced fonts on the current system. For missing fonts, the internet browser will fall back to some default -- leading to unpleasant appearances. Choose ``False`` if you want to parse the text of the SVG. (New in v1.17.5)

         :returns: a UTF-8 encoded string that contains the image. Because SVG has XML syntax it can be saved in a text file, the standard extension is `.svg`.

               .. note:: In case of a PDF, you can circumvent the "full page image only" restriction by modifying the page's CropBox before using the method.

      .. method:: get_pixmap(*, matrix=pymupdf.Identity, dpi=None, colorspace=pymupdf.csRGB, clip=None, alpha=False, annots=True)
         :no-index:

         Create a pixmap from the page. This is probably the most often used method to create a :ref:`Pixmap`.

         All parameters are *keyword-only.*

         :arg matrix_like matrix: default is :ref:`Identity`.
         :arg int dpi: desired resolution in x and y direction. If not `None`, the `"matrix"` parameter is ignored. (New in v1.19.2)
         :arg colorspace: The desired colorspace, one of "GRAY", "RGB" or "CMYK" (case insensitive). Or specify a :ref:`Colorspace`, ie. one of the predefined ones: :data:`csGRAY`, :data:`csRGB` or :data:`csCMYK`.
         :type colorspace: str or :ref:`Colorspace`
         :arg irect_like clip: restrict rendering to the intersection of this area with the page's rectangle.
         :arg bool alpha: whether to add an alpha channel. Always accept the default ``False`` if you do not really need transparency. This will save a lot of memory (25% in case of RGB ... and pixmaps are typically **large**!), and also processing time. Also note an **important difference** in how the image will be rendered: with ``True`` the pixmap's samples area will be pre-cleared with *0x00*. This results in **transparent** areas where the page is empty. With ``False`` the pixmap's samples will be pre-cleared with *0xff*. This results in **white** where the page has nothing to show.

            |history_begin|
            
            Changed in v1.14.17
               The default alpha value is now ``False``.

               * Generated with *alpha=True*

               .. image:: images/img-alpha-1.*


               * Generated with *alpha=False*

               .. image:: images/img-alpha-0.*

            |history_end|

         :arg bool annots: *(new in version 1.16.0)* whether to also render annotations or to suppress them. You can create pixmaps for annotations separately.

         :rtype: :ref:`Pixmap`
         :returns: Pixmap of the page. For fine-controlling the generated image, the by far most important parameter is **matrix**. E.g. you can increase or decrease the image resolution by using **Matrix(xzoom, yzoom)**. If zoom > 1, you will get a higher resolution: zoom=2 will double the number of pixels in that direction and thus generate a 2 times larger image. Non-positive values will flip horizontally, resp. vertically. Similarly, matrices also let you rotate or shear, and you can combine effects via e.g. matrix multiplication. See the :ref:`Matrix` section to learn more.

         .. note::

               * The pixmap will have *"premultiplied"* pixels if `alpha=True`. To learn about some background, e.g. look for "Premultiplied alpha" `here <https://en.wikipedia.org/wiki/Glossary_of_computer_graphics#P>`_.

               * The method will respect any page rotation and will not exceed the intersection of `clip` and :attr:`Page.cropbox`. If you need the page's mediabox (and if this is a different rectangle), you can use a snippet like the following to achieve this::

                  In [1]: import pymupdf
                  In [2]: doc=pymupdf.open("demo1.pdf")
                  In [3]: page=doc[0]
                  In [4]: rotation = page.rotation
                  In [5]: cropbox = page.cropbox
                  In [6]: page.set_cropbox(page.mediabox)
                  In [7]: page.set_rotation(0)
                  In [8]: pix = page.get_pixmap()
                  In [9]: page.set_cropbox(cropbox)
                  In [10]: if rotation != 0:
                     ...:     page.set_rotation(rotation)
                     ...:
                  In [11]:

         |history_begin|

         * Changed in v1.19.2: added support of parameter dpi.

         |history_end|



      .. method:: annot_names()
         :no-index:

         PDF only: return a list of the names of annotations, widgets and links. Technically, these are the */NM* values of every PDF object found in the page's */Annots*  array.

         :rtype: list

         |history_begin|

         * New in v1.16.10

         |history_end|


      .. method:: annot_xrefs()
         :no-index:

         PDF only: return a list of the :data:`xref` numbers of annotations, widgets and links -- technically of all entries found in the page's */Annots*  array.

         :rtype: list
         :returns: a list of items *(xref, type)* where type is the annotation type. Use the type to tell apart links, fields and annotations, see :ref:`AnnotationTypes`.

         |history_begin|

         * New in v1.17.1

         |history_end|


      .. method:: load_annot(ident)
         :no-index:

         PDF only: return the annotation identified by *ident*. This may be its unique name (PDF `/NM` key), or its :data:`xref`.

         :arg str,int ident: the annotation name or xref.

         :rtype: :ref:`Annot`
         :returns: the annotation or ``None``.

         .. note:: Methods :meth:`Page.annot_names`, :meth:`Page.annot_xrefs` provide lists of names or xrefs, respectively, from where an item may be picked and loaded via this method.

         |history_begin|

         * New in v1.17.1

         |history_end|

      .. method:: load_widget(xref)
         :no-index:

         PDF only: return the field identified by *xref*.

         :arg int xref: the field's xref.

         :rtype: :ref:`Widget`
         :returns: the field or ``None``.

         .. note:: This is similar to the analogous method :meth:`Page.load_annot` -- except that here only the xref is supported as identifier.

         |history_begin|

         * New in v1.19.6

         |history_end|

      .. method:: load_links()
         :no-index:

         Return the first link on a page. Synonym of property :attr:`first_link`.

         :rtype: :ref:`Link`
         :returns: first link on the page (or ``None``).

      .. method:: set_rotation(rotate)
         :no-index:

         PDF only: Set the rotation of the page.

         :arg int rotate: An integer specifying the required rotation in degrees. Must be an integer multiple of 90. Values will be converted to one of 0, 90, 180, 270.

      .. method:: remove_rotation()
         :no-index:

         PDF only: Set page rotation to 0 while maintaining appearance and page content.

         :returns: The inverted matrix used to achieve this change. If the page was not rotated (rotation 0), :ref:`Identity` is returned. The method automatically recomputes the rectangles of any annotations, links and widgets present on the page.

            This method may come in handy when e.g. used with :meth:`Page.show_pdf_page`.

      .. method:: show_pdf_page(rect, docsrc, pno=0, keep_proportion=True, overlay=True, oc=0, rotate=0, clip=None)
         :no-index:

         PDF only: Display a page of another PDF as a **vector image** (otherwise similar to :meth:`Page.insert_image`). This is a multi-purpose method. For example, you can use it to:

         * create "n-up" versions of existing PDF files, combining several input pages into **one output page** (see example `combine.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/combine-pages/combine.py>`_),
         * create "posterized" PDF files, i.e. every input page is split up in parts which each create a separate output page (see `posterize.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/posterize-document/posterize.py>`_),
         * include PDF-based vector images like company logos, watermarks, etc., see `svg-logo.py <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/examples/svg-logo.py>`_, which puts an SVG-based logo on each page (requires additional packages to deal with SVG-to-PDF conversions).

         :arg rect_like rect: where to place the image on current page. Must be finite and its intersection with the page must not be empty.
         :arg docsrc: source PDF document containing the page. Must be a different document object, but may be the same file.
         :type docsrc: :ref:`Document`

         :arg int pno: page number (0-based, in `-∞ < pno < docsrc.page_count`) to be shown.

         :arg bool keep_proportion: whether to maintain the width-height-ratio (default). If false, all 4 corners are always positioned on the border of the target rectangle -- whatever the rotation value. In general, this will deliver distorted and /or non-rectangular images.

         :arg bool overlay: put image in foreground (default) or background.

         :arg int oc: (:data:`xref`) make visibility dependent on this :data:`OCG` / :data:`OCMD` (which must be defined in the target PDF) [#f9]_. (New in v1.18.3)
         :arg float rotate: show the source rectangle rotated by some angle. Any angle is supported (changed in v1.14.11). (New in v1.14.10)

         :arg rect_like clip: choose which part of the source page to show. Default is the full page, else must be finite and its intersection with the source page must not be empty.

         .. note:: In contrast to method :meth:`Document.insert_pdf`, this method does not copy annotations, widgets or links, so these are not included in the target [#f6]_. But all its **other resources (text, images, fonts, etc.)** will be imported into the current PDF. They will therefore appear in text extractions and in :meth:`get_fonts` and :meth:`get_images` lists -- even if they are not contained in the visible area given by *clip*.

         Example: Show the same source page, rotated by 90 and by -90 degrees:

         >>> doc = pymupdf.open()  # new empty PDF
         >>> page=doc.new_page()  # new page in A4 format
         >>>
         >>> # upper half page
         >>> r1 = pymupdf.Rect(0, 0, page.rect.width, page.rect.height/2)
         >>>
         >>> # lower half page
         >>> r2 = r1 + (0, page.rect.height/2, 0, page.rect.height/2)
         >>>
         >>> src = pymupdf.open("PyMuPDF.pdf")  # show page 0 of this
         >>>
         >>> page.show_pdf_page(r1, src, 0, rotate=90)
         >>> page.show_pdf_page(r2, src, 0, rotate=-90)
         >>> doc.save("show.pdf")

         .. image:: images/img-showpdfpage.*
            :scale: 70

         |history_begin|

         * Changed in v1.14.11: Parameter *reuse_xref* has been deprecated. Position the source rectangle centered in target rectangle. Any rotation angle is now supported.
         * Changed in v1.18.3: New parameter `oc`.

         |history_end|

      .. method:: new_shape()
         :no-index:

         PDF only: Create a new :ref:`Shape` object for the page.

         :rtype: :ref:`Shape`
         :returns: a new :ref:`Shape` to use for compound drawings. See description there.

      .. method:: search_for(needle, *, clip=None, quads=False, flags=TEXT_DEHYPHENATE | TEXT_PRESERVE_WHITESPACE | TEXT_PRESERVE_LIGATURES | TEXT_MEDIABOX_CLIP, textpage=None)
         :no-index:

         Search for *needle* on a page. Wrapper for :meth:`TextPage.search`.

         :arg str needle: Text to search for. May contain spaces. Upper / lower case is ignored, but only works for ASCII characters: For example, "COMPÉTENCES" will not be found if needle is "compétences" -- "compÉtences" however will. Similar is true for German umlauts and the like.
         :arg rect_like clip: only search within this area. (New in v1.18.2)
         :arg bool quads: Return object type :ref:`Quad` instead of :ref:`Rect`.
         :arg int flags: Control the data extracted by the underlying :ref:`TextPage`. By default, ligatures and white spaces are kept, and hyphenation [#f8]_ is detected.
         :arg textpage: use a previously created :ref:`TextPage`. This reduces execution time **significantly.** If specified, the 'flags' and 'clip' arguments are ignored. If omitted, a temporary textpage will be created. (New in v1.19.0)

         :rtype: list

         :returns:

         A list of :ref:`Rect` or  :ref:`Quad` objects, each of which  -- **normally!** -- surrounds one occurrence of *needle*. **However:** if parts of *needle* occur on more than one line, then a separate item is generated for each these parts. So, if `needle = "search string"`, two rectangles may be generated.

         |history_begin|
         
         Changes in v1.18.2:

         * There no longer is a limit on the list length (removal of the `hit_max` parameter).
         * If a word is **hyphenated** at a line break, it will still be found. E.g. the needle "method" will be found even if hyphenated as "meth-od" at a line break, and two rectangles will be returned: one surrounding "meth" (without the hyphen) and another one surrounding "od".

         |history_end|

         .. note:: The method supports multi-line text marker annotations: you can use the full returned list as **one single** parameter for creating the annotation.

         .. caution::

            * There is a tricky aspect: the search logic regards **contiguous multiple occurrences** of *needle* as one: assuming *needle* is "abc", and the page contains "abc" and "abcabc", then only **two** rectangles will be returned, one for "abc", and a second one for "abcabc".
            * You can always use :meth:`Page.get_textbox` to check what text actually is being surrounded by each rectangle.

         .. note:: A feature repeatedly asked for is supporting **regular expressions** when specifying the `"needle"` string: **There is no way to do this.** If you need something in that direction, first extract text in the desired format and then subselect the result by matching with some regex pattern. Here is an example for matching words::

            >>> pattern = re.compile(r"...")  # the regex pattern
            >>> words = page.get_text("words")  # extract words on page
            >>> matches = [w for w in words if pattern.search(w[4])]

            The `matches` list will contain the words matching the given pattern. In the same way you can select `span["text"]` from the output of `page.get_text("dict")`.

         |history_begin|

         * Changed in v1.18.2: added `clip` parameter. Remove `hit_max` parameter. Add default "dehyphenate".
         * Changed in v1.19.0: added `textpage` parameter.

         |history_end|


      .. method:: set_mediabox(r)
         :no-index:

         PDF only: Change the physical page dimension by setting :data:`mediabox` in the page's object definition.

         :arg rect-like r: the new :data:`mediabox` value.

         .. note:: This method also removes the page's other (optional) rectangles (:data:`cropbox`, ArtBox, TrimBox and Bleedbox) to prevent inconsistent situations. This will cause those to assume their default values.

         .. caution:: For non-empty pages this may have undesired effects, because the location of all content depends on this value and will therefore change position or even disappear.

         |history_begin|

         * New in v1.16.13
         * Changed in v1.19.4: remove all other rectangle definitions.

         |history_end|


      .. method:: set_cropbox(r)
         :no-index:

         PDF only: change the visible part of the page.

         :arg rect_like r: the new visible area of the page. Note that this **must** be specified in **unrotated coordinates**, not empty, nor infinite and be completely contained in the :attr:`Page.mediabox`.

         After execution **(if the page is not rotated)**, :attr:`Page.rect` will equal this rectangle, but be shifted to the top-left position (0, 0) if necessary. Example session:

         >>> page = doc.new_page()
         >>> page.rect
         pymupdf.Rect(0.0, 0.0, 595.0, 842.0)
         >>>
         >>> page.cropbox  # cropbox and mediabox still equal
         pymupdf.Rect(0.0, 0.0, 595.0, 842.0)
         >>>
         >>> # now set cropbox to a part of the page
         >>> page.set_cropbox(pymupdf.Rect(100, 100, 400, 400))
         >>> # this will also change the "rect" property:
         >>> page.rect
         pymupdf.Rect(0.0, 0.0, 300.0, 300.0)
         >>>
         >>> # but mediabox remains unaffected
         >>> page.mediabox
         pymupdf.Rect(0.0, 0.0, 595.0, 842.0)
         >>>
         >>> # revert CropBox change
         >>> # either set it to MediaBox
         >>> page.set_cropbox(page.mediabox)
         >>> # or 'refresh' MediaBox: will remove all other rectangles
         >>> page.set_mediabox(page.mediabox)

      .. method:: set_artbox(r)
         :no-index:

      .. method:: set_bleedbox(r)
         :no-index:

      .. method:: set_trimbox(r)
         :no-index:

         PDF only: Set the resp. rectangle in the page object. For the meaning of these objects see :ref:`AdobeManual`, page 77. Parameter and restrictions are the same as for :meth:`Page.set_cropbox`.

         |history_begin|

         * New in v1.19.4

         |history_end|

      .. attribute:: rotation
         :no-index:

         Contains the rotation of the page in degrees (always 0 for non-PDF types). This is a copy of the value in the PDF file. The PDF documentation says:
         
            *"The number of degrees by which the page should be rotated clockwise when displayed or printed. The value must be a multiple of 90. Default value: 0."*

            In PyMuPDF, we make sure that this attribute is always one of 0, 90, 180 or 270.

         :type: int

      .. attribute:: cropbox_position
         :no-index:

         Contains the top-left point of the page's `/CropBox` for a PDF, otherwise *Point(0, 0)*.

         :type: :ref:`Point`

      .. attribute:: cropbox
         :no-index:

         The page's `/CropBox` for a PDF. Always the **unrotated** page rectangle is returned. For a non-PDF this will always equal the page rectangle.

         .. note:: In PDF, the relationship between `/MediaBox`, `/CropBox` and page rectangle may sometimes be confusing, please do lookup the glossary for :data:`MediaBox`.

         :type: :ref:`Rect`

      .. attribute:: artbox
         :no-index:

      .. attribute:: bleedbox
         :no-index:

      .. attribute:: trimbox
         :no-index:

         The page's `/ArtBox`, `/BleedBox`, `/TrimBox`, respectively. If not provided, defaulting to :attr:`Page.cropbox`.

         :type: :ref:`Rect`

      .. attribute:: mediabox_size
         :no-index:

         Contains the width and height of the page's :attr:`Page.mediabox` for a PDF, otherwise the bottom-right coordinates of :attr:`Page.rect`.

         :type: :ref:`Point`

      .. attribute:: mediabox
         :no-index:

         The page's :data:`mediabox` for a PDF, otherwise :attr:`Page.rect`.

         :type: :ref:`Rect`

         .. note:: For most PDF documents and for **all other document types**, `page.rect == page.cropbox == page.mediabox` is true. However, for some PDFs the visible page is a true subset of :data:`mediabox`. Also, if the page is rotated, its `Page.rect` may not equal `Page.cropbox`. In these cases the above attributes help to correctly locate page elements.

      .. attribute:: transformation_matrix
         :no-index:

         This matrix translates coordinates from the PDF space to the MuPDF space. For example, in PDF `/Rect [x0 y0 x1 y1]` the pair (x0, y0) specifies the **bottom-left** point of the rectangle -- in contrast to MuPDF's system, where (x0, y0) specify top-left. Multiplying the PDF coordinates with this matrix will deliver the (Py-) MuPDF rectangle version. Obviously, the inverse matrix will again yield the PDF rectangle.

         :type: :ref:`Matrix`

      .. attribute:: rotation_matrix
         :no-index:

      .. attribute:: derotation_matrix
         :no-index:

         These matrices may be used for dealing with rotated PDF pages. When adding / inserting anything to a PDF page, the coordinates of the **unrotated** page are always used. These matrices help translating between the two states. Example: if a page is rotated by 90 degrees -- what would then be the coordinates of the top-left Point(0, 0) of an A4 page?

            >>> page.set_rotation(90)  # rotate an ISO A4 page
            >>> page.rect
            Rect(0.0, 0.0, 842.0, 595.0)
            >>> p = pymupdf.Point(0, 0)  # where did top-left point land?
            >>> p * page.rotation_matrix
            Point(842.0, 0.0)
            >>>

         :type: :ref:`Matrix`

      .. attribute:: first_link
         :no-index:

         Contains the first :ref:`Link` of a page (or ``None``).

         :type: :ref:`Link`

      .. attribute:: first_annot
         :no-index:

         Contains the first :ref:`Annot` of a page (or ``None``).

         :type: :ref:`Annot`

      .. attribute:: first_widget
         :no-index:

         Contains the first :ref:`Widget` of a page (or ``None``).

         :type: :ref:`Widget`

      .. attribute:: number
         :no-index:

         The page number.

         :type: int

      .. attribute:: parent
         :no-index:

         The owning document object.

         :type: :ref:`Document`


      .. attribute:: rect
         :no-index:

         Contains the rectangle of the page. Same as result of :meth:`Page.bound()`.

         :type: :ref:`Rect`

      .. attribute:: xref
         :no-index:

         The page's PDF :data:`xref`. Zero if not a PDF.

         :type: :ref:`Rect`

-----

.. _link_dict_description:

*get_links()* 条目的描述
----------------------------------------

Description of *get_links()* Entries

.. tab:: 中文

   :meth:`Page.get_links` 方法返回的列表中，每个条目都是一个包含以下键的字典：

   * *kind* （必需）: 一个整数，指示链接的类型。可能的值包括 *LINK_NONE*、*LINK_GOTO*、*LINK_GOTOR*、*LINK_LAUNCH* 或 *LINK_URI*。有关这些名称的值和含义，请参阅 :ref:`linkDest Kinds`。

   * *from* （必需）: 一个 :ref:`Rect` 对象，描述页面上可见区域的 "热区"（通常鼠标悬停时会变成手型指针）。
*
   * *page*: 一个从 0 开始的整数，表示目标页面。对于 *LINK_GOTO* 和 *LINK_GOTOR* 链接类型，此字段为必需项，否则会被忽略。

   * *to*: 目标位置，可以是：
   
   - 一个 *pymupdf.Point*，表示目标页面上的具体位置（默认为 *pymupdf.Point(0, 0)*）。
   - 一个符号名称（间接名称）。如果使用符号名称，则必须设置 *page = -1*，且该名称必须在 PDF 文件中定义，否则无法正确解析。
   
   此字段适用于 *LINK_GOTO* 和 *LINK_GOTOR* 类型的链接，否则会被忽略。

   * *file*: 一个字符串，指定目标文件路径。适用于 *LINK_GOTOR* 和 *LINK_LAUNCH* 类型的链接，否则会被忽略。

   * *uri*: 一个字符串，指定目标互联网资源。适用于 *LINK_URI* 类型的链接，否则会被忽略。请确保该字符串以明确的协议前缀开头，如 `"http://"`, `"https://"`, `"file://"`, `"ftp://"`, `"mailto:"` 等，否则浏览器可能会错误地解释 URL 类型。

   * *xref*: 一个整数，指定该链接对象在 PDF 文件中的 :data:`xref` 号。请勿修改此字段，它用于删除或更新链接。如果是非 PDF 文档，该字段的值为 *-1*。如果 *get_links()* 方法返回的列表中 **任何** 链接类型不受 MuPDF 支持，则该列表中的所有 *xref* 值都将为 *-1*，详见 :ref:`notes_on_supporting_links`。


.. tab:: 英文

   Each entry of the :meth:`Page.get_links` list is a dictionary with the following keys:

   * *kind*:  (required) an integer indicating the kind of link. This is one of *LINK_NONE*, *LINK_GOTO*, *LINK_GOTOR*, *LINK_LAUNCH*, or *LINK_URI*. For values and meaning of these names refer to :ref:`linkDest Kinds`.

   * *from*:  (required) a :ref:`Rect` describing the "hot spot" location on the page's visible representation (where the cursor changes to a hand image, usually).

   * *page*:  a 0-based integer indicating the destination page. Required for *LINK_GOTO* and *LINK_GOTOR*, else ignored.

   * *to*:   either a *pymupdf.Point*, specifying the destination location on the provided page, default is *pymupdf.Point(0, 0)*, or a symbolic (indirect) name. If an indirect name is specified, *page = -1* is required and the name must be defined in the PDF in order for this to work. Required for *LINK_GOTO* and *LINK_GOTOR*, else ignored.

   * *file*: a string specifying the destination file. Required for *LINK_GOTOR* and *LINK_LAUNCH*, else ignored.

   * *uri*:  a string specifying the destination internet resource. Required for *LINK_URI*, else ignored. You should make sure to start this string with an unambiguous substring, that classifies the subtype of the URL, like `"http://"`, `"https://"`, `"file://"`, `"ftp://"`, `"mailto:"`, etc. Otherwise your browser will try to interpret the text and come to unwanted / unexpected conclusions about the intended URL type.

   * *xref*: an integer specifying the PDF :data:`xref` of the link object. Do not change this entry in any way. Required for link deletion and update, otherwise ignored. For non-PDF documents, this entry contains *-1*. It is also *-1* for **all** entries in the *get_links()* list, if **any** of the links is not supported by MuPDF - see :ref:`notes_on_supporting_links`.

.. _notes_on_supporting_links:

支持链接的说明
---------------------------

Notes on Supporting Links

.. tab:: 中文

   MuPDF 对链接的支持在 **v1.10a** 版本中发生了变化。这些更改影响了链接类型 :data:`LINK_GOTO` 和 :data:`LINK_GOTOR`。

.. tab:: 英文

   MuPDF's support for links has changed in **v1.10a**. These changes affect link types :data:`LINK_GOTO` and :data:`LINK_GOTOR`.

读取（涉及方法 *get_links()* 和 *first_link* 属性链）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Reading (pertains to method *get_links()* and the *first_link* property chain)

.. tab:: 中文

   如果 MuPDF 检测到指向另一个文件的链接，它会提供 *LINK_GOTOR* 或 *LINK_LAUNCH* 类型的链接。对于 *LINK_GOTOR* 类型的链接，其目标可以是具体的页码（可能包括位置信息），或者是一个间接目标。

   如果目标是间接目标，则该链接的 *page* 值将被设置为 `-1`，而 *link.dest.dest* 将包含该目标名称。在 *get_links()* 返回的字典列表中，该信息将作为 *to* 值提供。

   **内部链接始终** 为 *LINK_GOTO* 类型。如果内部链接指定了间接目标，它 **将始终被解析**，并返回解析后的直接目标。对于内部链接，**不会返回目标名称**，如果目标未定义，该链接将被忽略。


.. tab:: 英文

   If MuPDF detects a link to another file, it will supply either a *LINK_GOTOR* or a *LINK_LAUNCH* link kind. In case of *LINK_GOTOR* destination details may either be given as page number (eventually including position information), or as an indirect destination.

   If an indirect destination is given, then this is indicated by *page = -1*, and *link.dest.dest* will contain this name. The dictionaries in the *get_links()* list will contain this information as the *to* value.

   **Internal links are always** of kind *LINK_GOTO*. If an internal link specifies an indirect destination, it **will always be resolved** and the resulting direct destination will be returned. Names are **never returned for internal links**, and undefined destinations will cause the link to be ignored.

写入
~~~~~~~~~

Writing

.. tab:: 中文

   PyMuPDF 通过构造并写入适当的 PDF 对象 **源** 来写入（更新、插入）链接。这使得可以为 *LINK_GOTOR* **和** *LINK_GOTO* 类型的链接指定间接目标（不支持 *PDF 1.2* 之前的文件格式）。

   .. warning:: 

      如果 *LINK_GOTO* 链接的间接目标指定了一个未定义的名称，则该链接在 MuPDF / PyMuPDF 中将无法再次被找到 / 读取。然而，其他 PDF 阅读器 **可以** 识别它，但会将其标记为错误。

      由于间接 *LINK_GOTOR* 目标通常无法进行有效性检查，因此 **始终被接受**。

   **示例：如何插入指向同一文档中另一页的链接**

   1. 确定当前页面上链接应放置的矩形区域。这可以是图像的边界框（bbox）或某些文本的位置。

   2. 确定目标页码（"pno"，从 0 开始）及其上的一个 :ref:`Point`，该链接应指向该点。

   3. 创建字典 `d = {"kind": pymupdf.LINK_GOTO, "page": pno, "from": bbox, "to": point}`。

   4. 执行 `page.insert_link(d)` 以插入链接。


.. tab:: 英文

   PyMuPDF writes (updates, inserts) links by constructing and writing the appropriate PDF object **source**. This makes it possible to specify indirect destinations for *LINK_GOTOR* **and** *LINK_GOTO* link kinds (pre *PDF 1.2* file formats are **not supported**).

   .. warning:: If a *LINK_GOTO* indirect destination specifies an undefined name, this link can later on not be found / read again with MuPDF / PyMuPDF. Other readers however **will** detect it, but flag it as erroneous.

   Indirect *LINK_GOTOR* destinations can in general of course not be checked for validity and are therefore **always accepted**.

   **Example: How to insert a link pointing to another page in the same document**

   1. Determine the rectangle on the current page, where the link should be placed. This may be the bbox of an image or some text.

   2. Determine the target page number ("pno", 0-based) and a :ref:`Point` on it, where the link should be directed to.

   3. Create a dictionary `d = {"kind": pymupdf.LINK_GOTO, "page": pno, "from": bbox, "to": point}`.

   4. Execute `page.insert_link(d)`.


:ref:`Document` 和 :ref:`Page` 的同源方法
--------------------------------------------------------

Homologous Methods of :ref:`Document` and :ref:`Page`

.. tab:: 中文

   这是 :ref:`Document` 级别和 :ref:`Page` 级别上同源方法的概览。

   ====================================== =====================================
   **Document 级别**                      **Page 级别**
   ====================================== =====================================
   *Document.get_page_fonts(pno)*         :meth:`Page.get_fonts`
   *Document.get_page_images(pno)*        :meth:`Page.get_images`
   *Document.get_page_pixmap(pno, ...)*   :meth:`Page.get_pixmap`
   *Document.get_page_text(pno, ...)*     :meth:`Page.get_text`
   *Document.search_page_for(pno, ...)*   :meth:`Page.search_for`
   ====================================== =====================================

   其中，页码 "pno" 是一个以 0 为基准的整数，范围 `-∞ < pno < page_count`。

   .. note::

      大多数文档方法（左列）仅用于提供便利，实际上只是对 `Document[pno].<page method>` 的封装。因此，每次执行时都会 **加载并丢弃页面**。

      然而，前两种方法的行为有所不同。它们仅需要页面的对象定义语句，而 **不会** 加载页面。例如，:meth:`Page.get_fonts` 反向封装了 `Document.get_page_fonts(pno)`，其定义如下：*page.get_fonts == page.parent.get_page_fonts(page.number)*。


.. tab:: 英文

   This is an overview of homologous methods on the :ref:`Document` and on the :ref:`Page` level.

   ====================================== =====================================
   **Document Level**                     **Page Level**
   ====================================== =====================================
   *Document.get_page_fonts(pno)*         :meth:`Page.get_fonts`
   *Document.get_page_images(pno)*        :meth:`Page.get_images`
   *Document.get_page_pixmap(pno, ...)*   :meth:`Page.get_pixmap`
   *Document.get_page_text(pno, ...)*     :meth:`Page.get_text`
   *Document.search_page_for(pno, ...)*   :meth:`Page.search_for`
   ====================================== =====================================

   The page number "pno" is a 0-based integer `-∞ < pno < page_count`.

   .. note::

      Most document methods (left column) exist for convenience reasons, and are just wrappers for: *Document[pno].<page method>*. So they **load and discard the page** on each execution.

      However, the first two methods work differently. They only need a page's object definition statement - the page itself will **not** be loaded. So e.g. :meth:`Page.get_fonts` is a wrapper the other way round and defined as follows: *page.get_fonts == page.parent.get_page_fonts(page.number)*.

.. rubric:: Footnotes

.. [#f1] If your existing code already uses the installed base name as a font reference (as it was supported by PyMuPDF versions earlier than 1.14), this will continue to work.

.. [#f2] Not all PDF reader software (including internet browsers and office software) display all of these fonts. And if they do, the difference between the **serifed** and the **non-serifed** version may hardly be noticeable. But serifed and non-serifed versions lead to different installed base fonts, thus providing an option to be displayable with your specific PDF viewer.

.. [#f3] Not all PDF readers display these fonts at all. Some others do, but use a wrong character spacing, etc.

.. [#f4] You are generally free to choose any of the :ref:`mupdficons` you consider adequate.

.. [#f5] The previous algorithm caused images to be **shrunk** to this intersection. Now the image can be anywhere on :attr:`Page.mediabox`, potentially being invisible or only partially visible if the cropbox (representing the visible page part) is smaller.

.. [#f6] If you need to also see annotations or fields in the target page, you can convert the source PDF using :meth:`Document.bake`. The underlying MuPDF function of that method will convert these objects to normal page content. Then use :meth:`Page.show_pdf_page` with the converted PDF page.

.. [#f7] In PDF, an area enclosed by some lines or curves can have a property called "orientation". This is significant for switching on or off the fill color of that area when there exist multiple area overlaps - see discussion in method :meth:`Shape.finish` using the "non-zero winding number" rule. While orientation of curves, quads, triangles and other shapes enclosed by lines always was detectable, this has been impossible for "re" (rectangle) items in the past. Adding the orientation parameter now delivers the missing information.

.. [#f8] Hyphenation detection simply means that if the last character of a line is "-", it will be assumed to be a continuation character. That character will not be found by text searching with its default flag setting. Please take note, that a MuPDF *line* may not always be what you expect: words separated by overly large gaps (e.g. caused by text justification) may constitute separate MuPDF lines. If then any of these words ends with a hyphen, it will only be found by text searching if hyphenation is switched off.

.. [#f9] Objects inside the source page, like images, text or drawings, are never aware of whether their owning page now is under OC control inside the target PDF. If source page objects are OC-controlled in the source PDF, then this will not be retained on the target: they will become unconditionally visible.

.. include:: footer.rst
