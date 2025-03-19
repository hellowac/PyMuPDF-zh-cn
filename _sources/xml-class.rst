.. include:: header.rst

.. _Xml:

.. role:: htmlTag(emphasis)

================
Xml
================

.. tab:: 中文

   * 新增于 v1.21.0

   该类表示一个 HTML 或 XML 节点。它是一个辅助类，旨在访问 :ref:`Story` 对象的 DOM（文档对象模型）内容。

   无需直接创建 :ref:`Xml` 对象：在创建 :ref:`Story` 之后，直接使用 :attr:`Story.body` （它是一个 Xml 节点），即可遍历 Story 的 DOM 结构。

   ================================ ===========================================================================================
   **方法 / 属性**                   **描述**
   ================================ ===========================================================================================
   :meth:`~.add_bullet_list`        添加 :htmlTag:`ul` 标签（无序列表），上下文管理器。
   :meth:`~.add_codeblock`          添加 :htmlTag:`pre` 标签（代码块），上下文管理器。
   :meth:`~.add_description_list`   添加 :htmlTag:`dl` 标签（描述列表），上下文管理器。
   :meth:`~.add_division`           添加 :htmlTag:`div` 标签（重命名自“section”），上下文管理器。
   :meth:`~.add_header`             添加标题标签（:htmlTag:`h1` 到 :htmlTag:`h6` 之一），上下文管理器。
   :meth:`~.add_horizontal_line`    添加 :htmlTag:`hr` 标签（水平分割线）。
   :meth:`~.add_image`              添加 :htmlTag:`img` 标签（图片）。
   :meth:`~.add_link`               添加 :htmlTag:`a` 标签（超链接）。
   :meth:`~.add_number_list`        添加 :htmlTag:`ol` 标签（有序列表），上下文管理器。
   :meth:`~.add_paragraph`          添加 :htmlTag:`p` 标签（段落）。
   :meth:`~.add_span`               添加 :htmlTag:`span` 标签，上下文管理器。
   :meth:`~.add_subscript`          添加下标文本（:htmlTag:`sub` 标签），作为行内元素，处理方式类似文本。
   :meth:`~.add_superscript`        添加上标文本（:htmlTag:`sup` 标签），作为行内元素，处理方式类似文本。
   :meth:`~.add_code`               添加代码文本（:htmlTag:`code` 标签），作为行内元素，处理方式类似文本。
   :meth:`~.add_var`                添加代码文本（:htmlTag:`code` 标签），作为行内元素，处理方式类似文本。
   :meth:`~.add_samp`               添加代码文本（:htmlTag:`code` 标签），作为行内元素，处理方式类似文本。
   :meth:`~.add_kbd`                添加代码文本（:htmlTag:`code` 标签），作为行内元素，处理方式类似文本。
   :meth:`~.add_text`               添加文本字符串，换行符 ``\n`` 会被解析为 :htmlTag:`br` 标签。
   :meth:`~.append_child`           添加子节点。
   :meth:`~.clone`                  复制当前节点。
   :meth:`~.create_element`         创建一个指定标签的新节点。
   :meth:`~.create_text_node`       为当前节点创建文本内容。
   :meth:`~.find`                   查找具有指定属性的子节点。
   :meth:`~.find_next`              使用相同的查找条件重复执行“查找”操作。
   :meth:`~.insert_after`           在当前节点后插入一个元素。
   :meth:`~.insert_before`          在当前节点前插入一个元素。
   :meth:`~.remove`                 移除当前节点。
   :meth:`~.set_align`              使用 CSS 样式设置对齐方式，仅适用于块级标签。
   :meth:`~.set_attribute`          设置任意属性键及其值（值可以为空）。
   :meth:`~.set_bgcolor`            设置背景颜色，仅适用于块级标签。
   :meth:`~.set_bold`               设置加粗（开启、关闭或指定字符串值）。
   :meth:`~.set_color`              设置文本颜色。
   :meth:`~.set_columns`            设置列数，参数可以是任何有效的数值或字符串。
   :meth:`~.set_font`               设置字体，例如 “sans-serif”。
   :meth:`~.set_fontsize`           设置字体大小，可以是浮点数或有效的 HTML/CSS 字符串。
   :meth:`~.set_id`                 设置 :htmlTag:`id`，并检查唯一性。
   :meth:`~.set_italic`             设置斜体（开启、关闭或指定字符串值）。
   :meth:`~.set_leading`            设置文本块间距（`-mupdf-leading`），仅适用于块级节点。
   :meth:`~.set_lineheight`         设置行高，例如 1.5（表示 `1.5 * fontsize`）。
   :meth:`~.set_margins`            设置边距，可以是浮点数或最多包含 4 个值的字符串。
   :meth:`~.set_pagebreak_after`    在当前节点后插入分页符。
   :meth:`~.set_pagebreak_before`   在当前节点前插入分页符。
   :meth:`~.set_properties`         一次性设置一个或多个属性。
   :meth:`~.add_style`              添加一个 `set_` 方法未提供支持的样式。
   :meth:`~.add_class`              添加 `class` 属性。
   :meth:`~.set_text_indent`        设置首行文本缩进，仅适用于块级节点。
   :attr:`~.tagname`                HTML 标签名称（如 :htmlTag:`p`），如果是文本节点则为 `None`。
   :attr:`~.text`                   节点文本内容，如果是标签节点则为 `None`。
   :attr:`~.is_text`                判断该节点是否为文本节点。
   :attr:`~.first_child`            该节点的第一个子节点（若无则为 `None`）。
   :attr:`~.last_child`             该节点的最后一个子节点（若无则为 `None`）。
   :attr:`~.next`                   该层级中的下一个节点（若无则为 `None`）。
   :attr:`~.previous`               该层级中的上一个节点。
   :attr:`~.root`                   DOM 树的顶级节点，其标签名为 :htmlTag:`html`。
   ================================ ===========================================================================================

   **Class API**

   .. class:: Xml

      .. method:: add_bullet_list

         添加一个 :htmlTag:`ul` 标签（无序列表），上下文管理器。参见 `ul <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul>`_。

      .. method:: add_codeblock

         添加一个 :htmlTag:`pre` 标签（代码块），上下文管理器。参见 `pre <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/pre>`_。

      .. method:: add_description_list

         添加一个 :htmlTag:`dl` 标签（描述列表），上下文管理器。参见 `dl <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl>`_。

      .. method:: add_division

         添加一个 :htmlTag:`div` 标签，上下文管理器。参见 `div <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/div>`_。

      .. method:: add_header(value)

         添加一个标题标签（:htmlTag:`h1` 到 :htmlTag:`h6`），上下文管理器。参见 `headings <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements>`_。

         :arg int value: 一个 1 - 6 之间的数值。

      .. method:: add_horizontal_line

         添加一个 :htmlTag:`hr` 标签（水平分割线）。参见 `hr <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/hr>`_。

      .. method:: add_image(name, width=None, height=None)

         添加一个 :htmlTag:`img` 标签（图片）。该方法将在 DOM 中包含指定名称的图片。

         :arg str name: 图片的文件名。该文件 **必须是** :ref:`Archive` 参数中的成员，且属于 :ref:`Story` 构造函数的输入。
         :arg width: 如果提供，可以是绝对值（int），也可以是类似 "30%" 的百分比字符串。百分比值表示相对于 :meth:`Story.place` 方法中 `where` 矩形区域的宽度。如果提供 `width` 而省略 `height`，则图片将保持其纵横比。
         :arg height: 如果提供，可以是绝对值（int），也可以是类似 "30%" 的百分比字符串。百分比值表示相对于 :meth:`Story.place` 方法中 `where` 矩形区域的高度。如果提供 `height` 而省略 `width`，则图片的纵横比将被保留。

      .. method:: add_link(href, text=None)

         添加一个 :htmlTag:`a` 标签（超链接）—— 行内元素，作为文本处理。

         :arg str href: 目标 URL。
         :arg str text: 要显示的文本。如果省略，则默认显示 `href` 作为文本。

      .. method:: add_number_list

         添加一个 :htmlTag:`ol` 标签（有序列表），上下文管理器。

      .. method:: add_paragraph

         添加一个 :htmlTag:`p` 标签（段落），上下文管理器。

      .. method:: add_span

         添加一个 :htmlTag:`span` 标签，上下文管理器。参见 `span`_。

      .. method:: add_subscript(text)

         添加“下标”文本（:htmlTag:`sub` 标签）—— 行内元素，作为文本处理。

      .. method:: add_superscript(text)

         添加“上标”文本（:htmlTag:`sup` 标签）—— 行内元素，作为文本处理。

      .. method:: add_code(text)

         添加“代码”文本（:htmlTag:`code` 标签）—— 行内元素，作为文本处理。

      .. method:: add_var(text)

         添加“变量”文本（:htmlTag:`var` 标签）—— 行内元素，作为文本处理。

      .. method:: add_samp(text)

         添加“示例输出”文本（:htmlTag:`samp` 标签）—— 行内元素，作为文本处理。

      .. method:: add_kbd(text)

         添加“键盘输入”文本（:htmlTag:`kbd` 标签）—— 行内元素，作为文本处理。

      .. method:: add_text(text)

         添加文本字符串。换行符 ``\n`` 会被视为 :htmlTag:`br` 标签。

      .. method:: set_align(value)

         设置文本对齐方式，仅适用于块级标签。

         :arg value: 可以是 :ref:`TextAlign` 中的值，或者 `text-align <https://developer.mozilla.org/en-US/docs/Web/CSS/text-align>`_ 中的有效值。

      .. method:: set_attribute(key, value=None)

         设置一个属性键及其值（值可以为空）。

         :arg str key: 属性名称。
         :arg str value: （可选）属性值。

      .. method:: get_attributes()

         以字典形式获取当前节点的所有属性。

         :returns: 一个包含节点属性及其值的字典。

      .. method:: get_attribute_value(key)

         获取属性 `key` 的值。

         :arg str key: 属性名称。

         :returns: 一个包含 `key` 对应值的字符串。

      .. method:: remove_attribute(key)

         从节点中移除属性 `key`。

         :arg str key: 属性名称。

      .. method:: set_bgcolor(value)

         设置背景颜色，仅适用于块级标签。

         :arg value: 可以是 RGB 值，例如 (255, 0, 0)（表示“红色”），或者是 `background-color <https://developer.mozilla.org/en-US/docs/Web/CSS/background-color>`_ 允许的值。

      .. method:: set_bold(value)

         设置加粗样式，可以开启、关闭或指定具体的字符串值。

         :arg value: `True`、`False`，或者一个有效的 `font-weight <https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight>`_ 值。

      .. method:: set_color(value)

         设置后续文本的颜色。

         :arg value: 可以是 RGB 值，例如 (255, 0, 0)（表示“红色”），或者是有效的 `color <https://developer.mozilla.org/en-US/docs/Web/CSS/color_value>`_ 值。

      .. method:: set_columns(value)

         设置列数。

         :arg value: 一个有效的 `columns <https://developer.mozilla.org/en-US/docs/Web/CSS/columns>`_ 值。

         .. note:: 目前未生效，未来 MuPDF 版本将支持。

      .. method:: set_font(value)

         设置字体系列。

         :arg str value: 例如 "sans-serif"。

      .. method:: set_fontsize(value)

         设置后续文本的字体大小。

         :arg value: 一个浮点数，或者是有效的 `font-size <https://developer.mozilla.org/en-US/docs/Web/CSS/font-size>`_ 值。

      .. method:: set_id(unqid)

         设置 :htmlTag:`id`，用于在 DOM 中唯一标识该节点。可用于轻松定位节点，以便检查或修改。系统会检查 ID 的唯一性。

         :arg str unqid: 节点的唯一 ID 字符串。

      .. method:: set_italic(value)

         设置斜体，可以开启、关闭或指定具体的字符串值。

         :arg value: `True`、`False`，或一个有效的 `font-style <https://developer.mozilla.org/en-US/docs/Web/CSS/font-style>`_ 值。

      .. method:: set_leading(value)

         设置块间文本间距（`-mupdf-leading`），仅适用于块级节点。

         :arg float value: 以点（points）为单位的间距，表示与前一个块的距离。

      .. method:: set_lineheight(value)

         设置行高。

         :arg value: 例如 1.5（表示 `1.5 * fontsize`），或一个有效的 `line-height <https://developer.mozilla.org/en-US/docs/Web/CSS/line-height>`_ 值。

      .. method:: set_margins(value)

         设置边距。

         :arg value: 一个浮点数或包含最多 4 个值的字符串。参考 `CSS 文档 <https://developer.mozilla.org/en-US/docs/Web/CSS/margin>`_。

      .. method:: set_pagebreak_after

         在该节点后插入分页符。

      .. method:: set_pagebreak_before

         在该节点前插入分页符。

      .. method:: set_properties(align=None, bgcolor=None, bold=None, color=None, columns=None, font=None, fontsize=None, indent=None, italic=None, leading=None, lineheight=None, margins=None, pagebreak_after=False, pagebreak_before=False, unqid=None, cls=None)

         一次性设置一个或多个属性。各参数的含义与对应的 `set_` 方法相同。

         .. note:: 此方法直接将属性附加到节点，而 `set_` 方法通常会在当前节点下生成一个新的 :htmlTag:`span`，并应用相应的属性。因此，例如要“全局”设置 :htmlTag:`body` 的某些属性，必须使用此方法。

      .. method:: add_style(value)

         添加某些 `set_` 方法不支持的样式属性。

         :arg str value: 任何有效的 CSS 样式值。

      .. method:: add_class(value)

         添加 `class` 属性。

         :arg str value: 类名。必须在 HTML 或 CSS 源代码中已定义。

      .. method:: set_text_indent(value)

         设置首行缩进，仅适用于块级节点。

         :arg value: 一个有效的 `text-indent <https://developer.mozilla.org/en-US/docs/Web/CSS/text-indent>`_ 值。请注意，负值无效。

      .. method:: append_child(node)

         添加子节点。这是一个低级方法，其他方法（如 :meth:`Xml.add_paragraph`）会调用它。

         :arg node: 要追加的 :ref:`Xml` 节点。

      .. method:: create_text_node(text)

         为当前节点创建文本内容。

         :arg str text: 要添加的文本。

         :rtype: :ref:`Xml`
         :returns: 创建的文本节点。

      .. method:: create_element(tag)

         创建具有指定标签的新节点。这是一个低级方法，其他方法（如 :meth:`Xml.add_paragraph`）会调用它。

         :arg str tag: 元素标签。

         :rtype: :ref:`Xml`
         :returns: 创建的元素。要将其绑定到 DOM，请使用 :meth:`Xml.append_child`。

      .. method:: insert_before(elem)

         在当前节点之前插入指定元素 `elem`。

         :arg elem: 一个 :ref:`Xml` 元素。

      .. method:: insert_after(elem)

         在当前节点之后插入指定元素 `elem`。

         :arg elem: 一个 :ref:`Xml` 元素。

      .. method:: clone()

         复制当前节点，可通过 :meth:`Xml.append_child` 添加到 DOM，或通过 :meth:`Xml.insert_before` 、 :meth:`Xml.insert_after` 插入。

         :returns: 当前节点的克隆 (:ref:`Xml`)。

      .. method:: remove()

         从 DOM 中移除当前节点。

      .. method:: debug()

         用于调试，以简化的形式打印该节点的结构。

      .. method:: find(tag, att, match)

         在当前节点下，查找第一个符合指定 `tag`、属性 `att` 和值 `match` 的节点。

         :arg str tag: 限定搜索范围的标签。可以为 `None`，表示不限制。
         :arg str att: 需要检查的属性。可以为 `None`。
         :arg str match: 需要匹配的属性值。可以为 `None`。

         :rtype: :ref:`Xml`
         :returns: 如果未找到，则返回 `None`，否则返回第一个匹配的节点。

      .. method:: find_next(tag, att, match)

         继续上一次的 :meth:`Xml.find` 或 :meth:`find_next` 操作，使用相同的搜索条件。

         :rtype: :ref:`Xml`
         :returns: 如果未找到更多匹配项，则返回 `None`，否则返回下一个匹配的节点。

      .. attribute:: tagname

         当前节点的 HTML 标签名，例如 :htmlTag:`p`，如果是文本节点，则为 `None`。

      .. attribute:: text

         当前节点的文本内容，如果是标签节点，则为 `None`。

      .. attribute:: is_text

         检查当前节点是否为文本节点。

      .. attribute:: first_child

         当前节点的第一个子节点，如果没有，则为 `None`。

      .. attribute:: last_child

         当前节点的最后一个子节点，如果没有，则为 `None`。

      .. attribute:: next

         当前节点的同级下一个节点，如果没有，则为 `None`。

      .. attribute:: previous

         当前节点的同级上一个节点。

      .. attribute:: root

         DOM 的顶层节点，其标签名为 :htmlTag:`html`。

.. tab:: 英文

   * New in v1.21.0

   This represents an HTML or an XML node. It is a helper class intended to access the DOM (Document Object Model) content of a :ref:`Story` object.

   There is no need to ever directly construct an :ref:`Xml` object: after creating a :ref:`Story`, simply take :attr:`Story.body` -- which is an Xml node -- and use it to navigate your way through the story's DOM.


   ================================ ===========================================================================================
   **Method / Attribute**             **Description**
   ================================ ===========================================================================================
   :meth:`~.add_bullet_list`        Add a :htmlTag:`ul` tag - bulleted list, context manager.
   :meth:`~.add_codeblock`          Add a :htmlTag:`pre` tag, context manager.
   :meth:`~.add_description_list`   Add a :htmlTag:`dl` tag, context manager.
   :meth:`~.add_division`           add a :htmlTag:`div` tag (renamed from “section”), context manager.
   :meth:`~.add_header`             Add a header tag (one of :htmlTag:`h1` to :htmlTag:`h6`), context manager.
   :meth:`~.add_horizontal_line`    Add a :htmlTag:`hr` tag.
   :meth:`~.add_image`              Add a :htmlTag:`img` tag.
   :meth:`~.add_link`               Add a :htmlTag:`a` tag.
   :meth:`~.add_number_list`        Add a :htmlTag:`ol` tag, context manager.
   :meth:`~.add_paragraph`          Add a :htmlTag:`p` tag.
   :meth:`~.add_span`               Add a :htmlTag:`span` tag, context manager.
   :meth:`~.add_subscript`          Add subscript text(:htmlTag:`sub` tag) - inline element, treated like text.
   :meth:`~.add_superscript`        Add subscript text (:htmlTag:`sup` tag) - inline element, treated like text.
   :meth:`~.add_code`               Add code text (:htmlTag:`code` tag) - inline element, treated like text.
   :meth:`~.add_var`                Add code text (:htmlTag:`code` tag) - inline element, treated like text.
   :meth:`~.add_samp`               Add code text (:htmlTag:`code` tag) - inline element, treated like text.
   :meth:`~.add_kbd`                Add code text (:htmlTag:`code` tag) - inline element, treated like text.
   :meth:`~.add_text`               Add a text string. Line breaks ``\n`` are honored as :htmlTag:`br` tags.
   :meth:`~.append_child`           Append a child node.
   :meth:`~.clone`                  Make a copy if this node.
   :meth:`~.create_element`         Make a new node with a given tag name.
   :meth:`~.create_text_node`       Create direct text for the current node.
   :meth:`~.find`                   Find a sub-node with given properties.
   :meth:`~.find_next`              Repeat previous "find" with the same criteria.
   :meth:`~.insert_after`           Insert an element after current node.
   :meth:`~.insert_before`          Insert an element before current node.
   :meth:`~.remove`                 Remove this node.
   :meth:`~.set_align`              Set the alignment using a CSS style spec. Only works for block-level tags.
   :meth:`~.set_attribute`          Set an arbitrary key to some value (which may be empty).
   :meth:`~.set_bgcolor`            Set the background color. Only works for block-level tags.
   :meth:`~.set_bold`               Set bold on or off or to some string value.
   :meth:`~.set_color`              Set text color.
   :meth:`~.set_columns`            Set the number of columns. Argument may be any valid number or string.
   :meth:`~.set_font`               Set the font-family, e.g. “sans-serif”.
   :meth:`~.set_fontsize`           Set the font size. Either a float or a valid HTML/CSS string.
   :meth:`~.set_id`                 Set a :htmlTag:`id`. A check for uniqueness is performed.
   :meth:`~.set_italic`             Set italic on or off or to some string value.
   :meth:`~.set_leading`            Set inter-block text distance (`-mupdf-leading`), only works on block-level nodes.
   :meth:`~.set_lineheight`         Set height of a line. Float like 1.5, which sets to `1.5 * fontsize`.
   :meth:`~.set_margins`            Set the margin(s), float or string with up to 4 values.
   :meth:`~.set_pagebreak_after`    Insert a page break after this node.
   :meth:`~.set_pagebreak_before`   Insert a page break before this node.
   :meth:`~.set_properties`         Set any or all desired properties in one call.
   :meth:`~.add_style`              Set (add) a “style” that is not supported by its own `set_` method.
   :meth:`~.add_class`              Set (add) a “class” attribute.
   :meth:`~.set_text_indent`        Set indentation for first textblock line. Only works for block-level nodes.
   :attr:`~.tagname`                Either the HTML tag name like :htmlTag:`p` or `None` if a text node.
   :attr:`~.text`                   Either the node's text or `None` if a tag node.
   :attr:`~.is_text`                Check if the node is a text.
   :attr:`~.first_child`            Contains the first node one level below this one (or `None`).
   :attr:`~.last_child`             Contains the last node one level below this one (or `None`).
   :attr:`~.next`                   The next node at the same level (or `None`).
   :attr:`~.previous`               The previous node at the same level.
   :attr:`~.root`                   The top node of the DOM, which hence has the tagname :htmlTag:`html`.
   ================================ ===========================================================================================



   **Class API**

   .. class:: Xml
      :no-index:

      .. method:: add_bullet_list
         :no-index:

         Add an :htmlTag:`ul` tag - bulleted list, context manager. See `ul <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul>`_.

      .. method:: add_codeblock
         :no-index:

         Add a :htmlTag:`pre` tag, context manager. See `pre <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/pre>`_.

      .. method:: add_description_list
         :no-index:

         Add a :htmlTag:`dl` tag, context manager. See `dl <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl>`_.

      .. method:: add_division
         :no-index:

         Add a :htmlTag:`div` tag, context manager. See `div <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/div>`_.

      .. method:: add_header(value)
         :no-index:

         Add a header tag (one of :htmlTag:`h1` to :htmlTag:`h6`), context manager. See `headings <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements>`_.

         :arg int value: a value 1 - 6.

      .. method:: add_horizontal_line
         :no-index:

         Add a :htmlTag:`hr` tag. See `hr <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/hr>`_.

      .. method:: add_image(name, width=None, height=None)
         :no-index:

         Add an :htmlTag:`img` tag. This causes the inclusion of the named image in the DOM.

         :arg str name: the filename of the image. This **must be the member name** of some entry of the :ref:`Archive` parameter of the :ref:`Story` constructor.
         :arg width: if provided, either an absolute (int) value, or a percentage string like "30%". A percentage value refers to the width of the specified `where` rectangle in :meth:`Story.place`. If this value is provided and `height` is omitted, the image will be included keeping its aspect ratio.
         :arg height: if provided, either an absolute (int) value, or a percentage string like "30%". A percentage value refers to the height of the specified `where` rectangle in :meth:`Story.place`. If this value is provided and `width` is omitted, the image's aspect ratio will be honored.

      .. method:: add_link(href, text=None)
         :no-index:

         Add an :htmlTag:`a` tag - inline element, treated like text.

         :arg str href: the URL target.
         :arg str text: the text to display. If omitted, the `href` text is shown instead.

      .. method:: add_number_list
         :no-index:

         Add an :htmlTag:`ol` tag, context manager.

      .. method:: add_paragraph
         :no-index:

         Add a :htmlTag:`p` tag, context manager.

      .. method:: add_span
         :no-index:

         Add a :htmlTag:`span` tag, context manager. See `span`_

      .. method:: add_subscript(text)
         :no-index:

         Add "subscript" text(:htmlTag:`sub` tag) - inline element, treated like text.

      .. method:: add_superscript(text)
         :no-index:

         Add "superscript" text (:htmlTag:`sup` tag) - inline element, treated like text.

      .. method:: add_code(text)
         :no-index:

         Add "code" text (:htmlTag:`code` tag) - inline element, treated like text.

      .. method:: add_var(text)
         :no-index:

         Add "variable" text (:htmlTag:`var` tag) - inline element, treated like text.

      .. method:: add_samp(text)
         :no-index:

         Add "sample output" text (:htmlTag:`samp` tag) - inline element, treated like text.

      .. method:: add_kbd(text)
         :no-index:

         Add "keyboard input" text (:htmlTag:`kbd` tag) - inline element, treated like text.

      .. method:: add_text(text)
         :no-index:

         Add a text string. Line breaks ``\n`` are honored as :htmlTag:`br` tags.

      .. method:: set_align(value)
         :no-index:

         Set the text alignment. Only works for block-level tags.

         :arg value: either one of the :ref:`TextAlign` or the `text-align <https://developer.mozilla.org/en-US/docs/Web/CSS/text-align>`_ values.

      .. method:: set_attribute(key, value=None)
         :no-index:

         Set an arbitrary key to some value (which may be empty).

         :arg str key: the name of the attribute.
         :arg str value: the (optional) value of the attribute.

      .. method:: get_attributes()
         :no-index:

         Retrieve all attributes of the current nodes as a dictionary.

         :returns: a dictionary with the attributes and their values of the node.

      .. method:: get_attribute_value(key)
         :no-index:

         Get the attribute value of `key`.

         :arg str key: the name of the attribute.

         :returns: a string with the value of `key`.

      .. method:: remove_attribute(key)
         :no-index:

         Remove the attribute `key` from the node.

         :arg str key: the name of the attribute.

      .. method:: set_bgcolor(value)
         :no-index:

         Set the background color. Only works for block-level tags.

         :arg value: either an RGB value like (255, 0, 0) (for "red") or a valid `background-color <https://developer.mozilla.org/en-US/docs/Web/CSS/background-color>`_ value.

      .. method:: set_bold(value)
         :no-index:

         Set bold on or off or to some string value.

         :arg value: `True`, `False` or a valid `font-weight <https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight>`_ value.

      .. method:: set_color(value)
         :no-index:

         Set the color of the text following.

         :arg value: either an RGB value like (255, 0, 0) (for "red") or a valid `color <https://developer.mozilla.org/en-US/docs/Web/CSS/color_value>`_ value.

      .. method:: set_columns(value)
         :no-index:

         Set the number of columns.

         :arg value: a valid `columns <https://developer.mozilla.org/en-US/docs/Web/CSS/columns>`_ value.

         .. note:: Currently ignored - supported in a future MuPDF version.

      .. method:: set_font(value)
         :no-index:

         Set the font-family.

         :arg str value: e.g. "sans-serif".

      .. method:: set_fontsize(value)
         :no-index:

         Set the font size for text following.

         :arg value: a float or a valid `font-size <https://developer.mozilla.org/en-US/docs/Web/CSS/font-size>`_ value.

      .. method:: set_id(unqid)
         :no-index:

         Set a :htmlTag:`id`. This serves as a unique identification of the node within the DOM. Use it to easily locate the node to inspect or modify it. A check for uniqueness is performed.

         :arg str unqid: id string of the node.

      .. method:: set_italic(value)
         :no-index:

         Set italic on or off or to some string value for the text following it.

         :arg value: `True`, `False` or some valid `font-style <https://developer.mozilla.org/en-US/docs/Web/CSS/font-style>`_ value.

      .. method:: set_leading(value)
         :no-index:

         Set inter-block text distance (`-mupdf-leading`), only works on block-level nodes.

         :arg float value: the distance in points to the previous block.

      .. method:: set_lineheight(value)
         :no-index:

         Set height of a line.

         :arg value:  a float like 1.5 (which sets to `1.5 * fontsize`), or some valid `line-height <https://developer.mozilla.org/en-US/docs/Web/CSS/line-height>`_ value.

      .. method:: set_margins(value)
         :no-index:

         Set the margin(s).

         :arg value: float or string with up to 4 values. See `CSS documentation <https://developer.mozilla.org/en-US/docs/Web/CSS/margin>`_.

      .. method:: set_pagebreak_after
         :no-index:

         Insert a page break after this node.

      .. method:: set_pagebreak_before
         :no-index:

         Insert a page break before this node.

      .. method:: set_properties(align=None, bgcolor=None, bold=None, color=None, columns=None, font=None, fontsize=None, indent=None, italic=None, leading=None, lineheight=None, margins=None, pagebreak_after=False, pagebreak_before=False, unqid=None, cls=None)
         :no-index:

         Set any or all desired properties in one call. The meaning of argument values equal the values of the corresponding `set_` methods.

         .. note:: The properties set by this method are directly attached to the node, whereas every `set_` method generates a new :htmlTag:`span` below the current node that has the respective property. So to e.g. "globally" set some property for the :htmlTag:`body`, this method must be used.

      .. method:: add_style(value)
         :no-index:

         Set (add) some style attribute not supported by its own `set_` method.

         :arg str value: any valid CSS style value.

      .. method:: add_class(value)
         :no-index:

         Set (add) some "class" attribute.

         :arg str value: the name of the class. Must have been defined in either the HTML or the CSS source of the DOM.

      .. method:: set_text_indent(value)
         :no-index:

         Set indentation for the first textblock line. Only works for block-level nodes.

         :arg value: a valid `text-indent <https://developer.mozilla.org/en-US/docs/Web/CSS/text-indent>`_ value. Please note that negative values do not work.


      .. method:: append_child(node)
         :no-index:

         Append a child node. This is a low-level method used by other methods like :meth:`Xml.add_paragraph`.

         :arg node: the :ref:`Xml` node to append.

      .. method:: create_text_node(text)
         :no-index:

         Create direct text for the current node.

         :arg str text: the text to append.

         :rtype: :ref:`Xml`
         :returns: the created element.

      .. method:: create_element(tag)
         :no-index:

         Create a new node with a given tag. This a low-level method used by other methods like :meth:`Xml.add_paragraph`.

         :arg str tag: the element tag.

         :rtype: :ref:`Xml`
         :returns: the created element. To actually bind it to the DOM, use :meth:`Xml.append_child`.

      .. method:: insert_before(elem)
         :no-index:

         Insert the given element `elem` before this node.

         :arg elem: some :ref:`Xml` element.

      .. method:: insert_after(elem)
         :no-index:

         Insert the given element `elem` after this node.

         :arg elem: some :ref:`Xml` element.

      .. method:: clone()
         :no-index:

         Make a copy of this node, which then may be appended (using :meth:`Xml.append_child`) or inserted (using one of :meth:`Xml.insert_before`, :meth:`Xml.insert_after`) in this DOM.

         :returns: the clone (:ref:`Xml`) of the current node.

      .. method:: remove()
         :no-index:

         Remove this node from the DOM.


      .. method:: debug()
         :no-index:

         For debugging purposes, print this node's structure in a simplified form.

      .. method:: find(tag, att, match)
         :no-index:

         Under the current node, find the first node with the given `tag`, attribute `att` and value `match`.

         :arg str tag: restrict search to this tag. May be `None` for unrestricted searches.
         :arg str att: check this attribute. May be `None`.
         :arg str match: the desired attribute value to match. May be `None`.

         :rtype: :ref:`Xml`.
         :returns: `None` if nothing found, otherwise the first matching node.

      .. method:: find_next( tag, att, match)
         :no-index:

         Continue a previous :meth:`Xml.find` (or :meth:`find_next`) with the same values.

         :rtype: :ref:`Xml`.
         :returns: `None` if none more found, otherwise the next matching node.


      .. attribute:: tagname
         :no-index:

         Either the HTML tag name like :htmlTag:`p` or `None` if a text node.

      .. attribute:: text
         :no-index:

         Either the node's text or `None` if a tag node.

      .. attribute:: is_text
         :no-index:

         Check if a text node.

      .. attribute:: first_child
         :no-index:

         Contains the first node one level below this one (or `None`).

      .. attribute:: last_child
         :no-index:

         Contains the last node one level below this one (or `None`).

      .. attribute:: next
         :no-index:

         The next node at the same level (or `None`).

      .. attribute:: previous
         :no-index:

         The previous node at the same level.

      .. attribute:: root
         :no-index:

         The top node of the DOM, which hence has the tagname :htmlTag:`html`.


设置文本属性
------------------------

Setting Text properties

.. tab:: 中文

   在 HTML 中，标签可以嵌套，使最内层文本 **继承** 其父标签外层标签的属性。例如：

   ``<p><b>some bold text<i>this is bold and italic</i></b>regular text</p>``。

   为了实现相同的效果，方法如 :meth:`Xml.set_bold` 和 :meth:`Xml.set_italic` 会在当前节点下创建一个临时的 :htmlTag:`span`，并应用相应的样式。

   此外，这些方法会返回它们的父节点，因此可以进行链式调用。

.. tab:: 英文


   In HTML tags can be nested such that innermost text **inherits properties** from the tag enveloping its parent tag. For example `<p><b>some bold text<i>this is bold and italic</i></b>regular text</p>`.

   To achieve the same effect, methods like :meth:`Xml.set_bold` and :meth:`Xml.set_italic` each open a temporary :htmlTag:`span` with the desired property underneath the current node.

   In addition, these methods return there parent node, so they can be concatenated with each other.



上下文管理器支持
------------------------

Context Manager support

.. tab:: 中文

   向 DOM 添加节点的标准方式如下::

      body = story.body
      para = body.add_paragraph()  # 添加一个段落
      para.set_bold()  # 接下来的文本将加粗
      para.add_text("some bold text")
      para.set_italic()  # 接下来的文本将额外变为斜体
      para.add_text("this is bold and italic")
      para.set_italic(False).set_bold(False)  # 之后的所有文本恢复正常
      para.add_text("regular text")


   标记为“上下文管理器”的方法可以方便地这样使用::

      body = story.body
      with body.add_paragraph() as para:
         para.set_bold().add_text("some bold text")
         para.set_italic().add_text("this is bold and italic")
         para.set_italic(False).set_bold(False).add_text("regular text")
         para.add_text("more regular text")

.. tab:: 英文

   The standard way to add nodes to a DOM is this::

      body = story.body
      para = body.add_paragraph()  # add a paragraph
      para.set_bold()  # text that follows will be bold
      para.add_text("some bold text")
      para.set_italic()  # text that follows will additionally be italic
      para.add_txt("this is bold and italic")
      para.set_italic(False).set_bold(False)  # all following text will be regular
      para.add_text("regular text")



   Methods that are flagged as "context managers" can conveniently be used in this way::

      body = story.body
      with body.add_paragraph() as para:
         para.set_bold().add_text("some bold text")
         para.set_italic().add_text("this is bold and italic")
         para.set_italic(False).set_bold(False).add_text("regular text")
         para.add_text("more regular text")

.. include:: footer.rst

.. External links:

.. _span: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/span
