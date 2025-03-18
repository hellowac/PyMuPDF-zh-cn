.. include:: header.rst

.. _Link:

================
Link
================

.. tab:: 中文

   表示指向某处的指针（本文档、其他文档、互联网）。链接存在于每个文档页面上，并且是前向链接的，从初始链接开始，可以通过 :attr:`Page.first_link` 属性访问。

   链接与其页面之间存在父子关系。如果页面对象变得不可用（如文档关闭、文档结构变化等），则其所有现有的链接对象也会变得不可用 -- 当访问链接属性或方法时，会引发异常，提示该对象为“孤立对象”。

   ========================= ============================================
   **属性**                   **简短描述**
   ========================= ============================================
   :meth:`Link.set_border`     修改边框属性
   :meth:`Link.set_colors`     修改颜色属性
   :meth:`Link.set_flags`      修改链接标志
   :attr:`Link.border`         边框特征
   :attr:`Link.colors`         边框线条颜色
   :attr:`Link.dest`           指向目标的详细信息
   :attr:`Link.is_external`    检查链接是否指向外部目的地
   :attr:`Link.flags`          链接注释标志
   :attr:`Link.next`           指向下一个链接
   :attr:`Link.rect`           可点击区域，未转换坐标
   :attr:`Link.uri`            链接目的地
   :attr:`Link.xref`           :data:`xref` 条目的编号
   ========================= ============================================

   **Class API**

   .. class:: Link

      .. method:: set_border(border=None, width=0, style=None, dashes=None)

         仅限 PDF：更改边框宽度和虚线属性。

         *(自版本 1.16.9 起更改)* 允许在不使用字典的情况下指定。如果 *border* 不是字典，则使用直接参数。

         :arg dict border: 一个字典，由 :attr:`border` 属性返回，包含键 *"width"* (*float*)、*"style"* (*str*) 和 *"dashes"* (*sequence*)。省略的键将保持相应属性不变。例如，要移除虚线，请使用 *"dashes": []*。如果 *dashes* 不是空序列，"style" 将自动设置为 "D"（虚线）。

         :arg float width: 如上所述。
         :arg str style: 如上所述。
         :arg sequence dashes: 如上所述。

      .. method:: set_colors(colors=None, stroke=None)

         仅限 PDF：更改 "stroke" 颜色。

         .. note:: 在 PDF 中，链接技术上是注释的子类型，**不支持填充颜色**。然而，为了保持一致的 API，我们允许像所有注释一样指定 `fill=` 参数，但该参数会被忽略并发出警告。

         *(自版本 1.16.9 起更改)* 允许直接设置颜色。如果 *colors* 不是字典，则使用这些参数。

         :arg dict colors: 一个包含颜色规格的字典。有关接受的字典键和值，请参见下文。最实用的方法应该是先复制 *colors* 属性，然后根据需要修改此字典。
         :arg sequence stroke: 如上所述。

      .. method:: set_flags(flags)

         *自版本 1.18.16 起新功能*

         设置链接注释的 PDF `/F` 属性。有关详细信息，请参见 :meth:`Annot.set_flags`。如果不是 PDF，调用此方法将不起作用。

      .. attribute:: flags

         *自版本 1.18.16 起新功能*

         返回链接注释标志，一个整数（有关详细信息，请参见 :attr:`Annot.flags`）。如果不是 PDF，则返回零。

      .. attribute:: colors

         仅限 PDF 有效：一个字典，包含两个浮点元组，范围为 `0 <= float <= 1`，指定 *stroke* 和内部（ *fill* ）颜色。如果不是 PDF，则返回 *None*。如上所述，链接的填充颜色始终为 `None`。边框的颜色用于链接矩形的边框。元组的长度隐式确定颜色空间：1 = 灰度，3 = RGB，4 = CMYK。因此，`(1.0, 0.0, 0.0)` 表示 RGB 颜色红色。每个浮点数 *f* 的值通过计算 *f = i / 255* 被映射到整数值 *i*，范围从 0 到 255。

         :rtype: dict

      .. attribute:: border

         仅限 PDF：包含边框特征的字典。如果不是 PDF，则为 *None*，如果没有边框信息，则为空字典。可能出现以下键：

         * *width* -- 一个浮点数，表示边框的厚度（单位为点）。如果未指定宽度，值为 -1.0。

         * *dashes* -- 一个整数序列，指定线条的虚线模式。`[]` 表示没有虚线，`[n]` 表示等长的开关长度为 *n* 点，较长的列表将被解释为交替开关的长度值。有关详细信息，请参见 :ref:`AdobeManual` 第 126 页。

         * *style* -- 1 字节边框样式： *S* （实线）= 围绕注释的实心矩形，*D*（虚线）= 围绕链接的虚线矩形，虚线模式由 *dashes* 条目指定， *B* （斜角）= 模拟的浮雕矩形，似乎在页面表面上方， *I*（内嵌）= 模拟的刻槽矩形，似乎在页面表面下方，*U*（下划线）= 注释矩形底部的单线。

         :rtype: dict

      .. attribute:: rect

         可以点击的区域，采用未经变换的坐标表示。

         :type: :ref:`Rect`

      .. attribute:: is_external

         一个布尔值，指定链接目标是否在当前文档外部。

         :type: bool

      .. attribute:: uri

         一个字符串，指定链接目标。此属性的含义应与 `is_external` 属性结合使用进行评估：

         * 如果 `is_external` 为真： `uri` 指向当前 PDF 文档外的某个目标，可能是互联网资源（ `uri` 以 ``http://`` 或类似的开头）、另一个文件（ `uri` 以 "file:" 或 "file://" 开头）或其他服务，如电子邮件地址（ `uri` 以 ``mailto:`` 开头）。

         * 如果 `is_external` 为假：`uri` 将为 `None` 或指向内部位置。对于 PDF 文档，这应为 *#nnnn*，表示 1 基数的页面号 *nnnn*，或者是一个命名位置。对于其他文档类型，格式可能不同，例如 XPS 文档中的 "../FixedDoc.fdoc#PG_2_LNK_1" 表示第 2 页（1 基数）。

         :type: str

      .. attribute:: xref

         一个整数，指定 PDF 的 :data:`xref`。如果不是 PDF，则为零。

         :type: int

      .. attribute:: next

         下一个链接或 *None*。

         :type: *Link*

      .. attribute:: dest

         链接目标详细信息对象。

         :type: :ref:`linkDest`


.. tab:: 英文

   Represents a pointer to somewhere (this document, other documents, the internet). Links exist per document page, and they are forward-chained to each other, starting from an initial link which is accessible by the :attr:`Page.first_link` property.

   There is a parent-child relationship between a link and its page. If the page object becomes unusable (closed document, any document structure change, etc.), then so does every of its existing link objects -- an exception is raised saying that the object is "orphaned", whenever a link property or method is accessed.

   ========================= ============================================
   **Attribute**             **Short Description**
   ========================= ============================================
   :meth:`Link.set_border`   modify border properties
   :meth:`Link.set_colors`   modify color properties
   :meth:`Link.set_flags`    modify link flags
   :attr:`Link.border`       border characteristics
   :attr:`Link.colors`       border line color
   :attr:`Link.dest`         points to destination details
   :attr:`Link.is_external`  checks if the link is an external destination
   :attr:`Link.flags`        link annotation flags
   :attr:`Link.next`         points to next link
   :attr:`Link.rect`         clickable area in untransformed coordinates
   :attr:`Link.uri`          link destination
   :attr:`Link.xref`         :data:`xref` number of the entry
   ========================= ============================================

   **Class API**

   .. class:: Link
      :no-index:

      .. method:: set_border(border=None, width=0, style=None, dashes=None)
         :no-index:

         PDF only: Change border width and dashing properties.

         *(Changed in version 1.16.9)* Allow specification without using a dictionary. The direct parameters are used if *border* is not a dictionary.

         :arg dict border: a dictionary as returned by the :attr:`border` property, with keys *"width"* (*float*), *"style"* (*str*) and *"dashes"* (*sequence*). Omitted keys will leave the resp. property unchanged. To e.g. remove dashing use: *"dashes": []*. If dashes is not an empty sequence, "style" will automatically be set to "D" (dashed).

         :arg float width: see above.
         :arg str style: see above.
         :arg sequence dashes: see above.

      .. method:: set_colors(colors=None, stroke=None)
         :no-index:

         PDF only: Changes the "stroke" color.

         .. note:: In PDF, links are a subtype of annotations technically and **do not support fill colors**. However, to keep a consistent API, we do allow specifying a `fill=` parameter like with all annotations, which will be ignored with a warning.

         *(Changed in version 1.16.9)* Allow colors to be directly set. These parameters are used if *colors* is not a dictionary.

         :arg dict colors: a dictionary containing color specifications. For accepted dictionary keys and values see below. The most practical way should be to first make a copy of the *colors* property and then modify this dictionary as required.
         :arg sequence stroke: see above.

      .. method:: set_flags(flags)
         :no-index:

         *New in v1.18.16*

         Set the PDF `/F` property of the link annotation. See :meth:`Annot.set_flags` for details. If not a PDF, this method is a no-op.


      .. attribute:: flags
         :no-index:

         *New in v1.18.16*

         Return the link annotation flags, an integer (see :attr:`Annot.flags` for details). Zero if not a PDF.


      .. attribute:: colors
         :no-index:

         Meaningful for PDF only: A dictionary of two tuples of floats in range `0 <= float <= 1` specifying the *stroke* and the interior (*fill*) colors. If not a PDF, *None* is returned. As mentioned above, the fill color is always `None` for links. The stroke color is used for the border of the link rectangle. The length of the tuple implicitly determines the colorspace: 1 = GRAY, 3 = RGB, 4 = CMYK. So `(1.0, 0.0, 0.0)` stands for RGB color red. The value of each float *f* is mapped to the integer value *i* in range 0 to 255 via the computation *f = i / 255*.

         :rtype: dict

      .. attribute:: border
         :no-index:

         Meaningful for PDF only: A dictionary containing border characteristics. It will be *None* for non-PDFs and an empty dictionary if no border information exists. The following keys can occur:

         * *width* -- a float indicating the border thickness in points. The value is -1.0 if no width is specified.

         * *dashes* -- a sequence of integers specifying a line dash pattern. *[]* means no dashes, *[n]* means equal on-off lengths of *n* points, longer lists will be interpreted as specifying alternating on-off length values. See the :ref:`AdobeManual` page 126 for more detail.

         * *style* -- 1-byte border style: *S* (Solid) = solid rectangle surrounding the annotation, *D* (Dashed) = dashed rectangle surrounding the link, the dash pattern is specified by the *dashes* entry, *B* (Beveled) = a simulated embossed rectangle that appears to be raised above the surface of the page, *I* (Inset) = a simulated engraved rectangle that appears to be recessed below the surface of the page, *U* (Underline) = a single line along the bottom of the annotation rectangle.

         :rtype: dict

      .. attribute:: rect
         :no-index:

         The area that can be clicked in untransformed coordinates.

         :type: :ref:`Rect`

      .. attribute:: is_external
         :no-index:

         A bool specifying whether the link target is outside of the current document.

         :type: bool

      .. attribute:: uri
         :no-index:

         A string specifying the link target. The meaning of this property should
         be evaluated in conjunction with property `is_external`:
         
         *
         `is_external` is true: `uri` points to some target outside the current
         PDF, which may be an internet resource (`uri` starts with ``http://`` or
         similar), another file (`uri` starts with "file:" or "file://") or some
         other service like an e-mail address (`uri` starts with ``mailto:``).

         *
         `is_external` is false: `uri` will be `None` or point to an
         internal location. In case of PDF documents, this should either be
         *#nnnn* to indicate a 1-based (!) page number *nnnn*, or a named
         location. The format varies for other document types, for example
         "../FixedDoc.fdoc#PG_2_LNK_1" for page number 2 (1-based) in an XPS
         document.

         :type: str

      .. attribute:: xref
         :no-index:

         An integer specifying the PDF :data:`xref`. Zero if not a PDF.

         :type: int

      .. attribute:: next
         :no-index:

         The next link or *None*.

         :type: *Link*

      .. attribute:: dest
         :no-index:

         The link destination details object.

         :type: :ref:`linkDest`

.. include:: footer.rst
