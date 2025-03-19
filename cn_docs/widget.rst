.. include:: header.rst

.. _Widget:

================
Widget
================

|pdf_only_class|

.. tab:: 中文

   此类表示一个 PDF 表单字段，也称为“小部件”（widget）。在本文档中，这两个术语可互换使用。从技术上讲，表单字段是 PDF 注释的一种特殊情况，它允许具有有限权限的用户在 PDF 中输入信息，主要用于填写表单。

   与注释类似，小部件存在于 PDF 页面上。与注释类似，页面上的第一个小部件可通过 :attr:`Page.first_widget` 访问，而后续小部件可通过 :attr:`Widget.next` 属性访问。

   *(版本 1.16.0 变更)* MuPDF 不再将小部件视为通用注释的子集。因此，:attr:`Page.first_annot` 和 :meth:`Annot.next` 只会返回 **非小部件注释** ，如果页面上仅存在表单字段，则返回 *None*。反之，:attr:`Page.first_widget` 和 :meth:`Widget.next` 仅返回小部件。这一设计决定仅影响 MuPDF 内部实现；从技术上讲，链接、注释和表单字段有许多共同点，在（Py-）MuPDF 中仍然共享大部分代码。

   **类 API**

   .. class:: Widget

      .. method:: button_states

         *版本 1.18.15 新增*

         返回按钮字段的开启 / 关闭（即选中 / 未选中）状态名称。“关闭”状态通常被命名为“Off”，“开启”状态则通常根据功能上下文命名，例如“Yes”、“Female”等。

         此方法可用于确定 :attr:`field_value` 在这些情况下可能的值。

         :returns: 一个字典，包含按钮小部件的 *正常* 和 *按下* 外观状态对应的“On”和“Off”名称。例如，以下示例显示“选中”值为“Male”：

            >>> print(field.field_name, field.button_states())
            Gender Second person {'down': ['Male', 'Off'], 'normal': ['Male', 'Off']}

      .. method:: on_state

         * 版本 1.22.2 新增*

         返回复选框和单选按钮的“ON”状态值。对于复选框，此值始终为“Yes”。对于单选按钮，此值用于选中 / 激活按钮。

         :returns: 使按钮处于“选中”状态的值。对于非复选框、非单选按钮字段，始终返回 `None`。对于复选框，返回 `True`。对于单选按钮，以下示例返回值为“Male”：

            >>> print(field.field_name, field.button_states())
            Gender Second person {'down': ['Male', 'Off'], 'normal': ['Male', 'Off']}
            >>> print(field.on_state())
            Male

         因此，对于复选框和单选按钮，推荐使用以下方法来设置其“选中”状态或检查其状态：
         
            >>> field.field_value = field.on_state()
            >>> field.field_value == field.on_state()
            True

      .. method:: update

         对小部件进行任何更改后，**必须使用** 此方法将更改存储到 PDF [#f2]_。

      .. method:: reset

         将字段值重置为默认值（如果已定义），或将其删除。不要忘记在之后调用 :meth:`update`。

      .. attribute:: next

         指向页面上的下一个表单字段。最后一个小部件返回 *None*。

      .. attribute:: border_color

         由最多 4 个浮点数组成的列表，定义字段边框颜色。默认值为 *None*，此时边框样式和边框宽度将被忽略。

      .. attribute:: border_style

         定义字段边框线样式的字符串。参考 :attr:`Annot.border`。默认值为 "s"（"Solid"），即连续线。在创建小部件时，仅考虑首字母（大小写均可）。

      .. attribute:: border_width

         定义边框线宽度的浮点数。默认值为 1。

      .. attribute:: border_dashes

         由整数组成的列表/元组，定义边框线的虚线属性。仅当 *border_style == "D"* 且 :attr:`border_color` 存在时才有意义。

      .. attribute:: choice_values

         由字符串组成的 Python 序列，定义列表框和组合框的有效选项。对于这些小部件类型，此属性是必需的，且必须包含至少两个元素。对其他类型忽略。

      .. attribute:: field_name

         定义字段名称的必填字符串。不进行重复性检查。

      .. attribute:: field_label

         可选字符串，包含字段的“备用”名称。通常用于注释、字段使用帮助等。默认值为字段名称。

      .. attribute:: field_value

         字段的值。

      .. attribute:: field_flags

         整数值，定义字段的大量属性。更改此属性时需谨慎，因为它可能会更改字段类型。

      .. attribute:: field_type

         定义字段类型的必填整数。取值范围为 0 到 6。在更新小部件时无法更改。

      .. attribute:: field_type_string

         由字段类型派生出的描述性字符串。

      .. attribute:: fill_color

         由最多 4 个浮点数组成的列表，定义字段的背景颜色。

      .. attribute:: button_caption

         按钮类型字段的标题字符串。

      .. attribute:: is_signed

         布尔值，指示签名字段的签署状态，否则为 *None*。

      .. attribute:: rect

         包含字段的矩形区域。

      .. attribute:: text_color

         由 **1、3 或 4 个浮点数** 组成的列表，定义文本颜色。默认值为黑色 (`[0, 0, 0]`)。

      .. attribute:: text_font

         定义使用字体的字符串。默认值及无效值的替代值为 *"Helv"*。有效字体参考名称请见下表。

      .. attribute:: text_fontsize

         定义文本 :data:`fontsize` 的浮点数。默认值为 0，这会导致 PDF 查看器软件根据注释矩形和文本量动态选择合适的字体大小。

      .. attribute:: text_maxlen

         定义最大文本字符数的整数。PDF 查看器不会（或不应）接受更长的文本。

      .. attribute:: text_type

         定义可接受文本类型的整数（如数字、日期、时间等）。当前仅作参考，创建或更新小部件时将被忽略。

      .. attribute:: xref

         小部件的 PDF :data:`xref`。

      .. attribute:: script

         * 版本 1.16.12 新增

         关联到小部件的 JavaScript 代码（Unicode 文本），或 *None*。**按钮类型** 小部件仅支持此脚本操作。

      .. attribute:: script_stroke

         * 版本 1.16.12 新增

         在用户输入文本字段或组合框，或修改可滚动列表框的选项时执行的 JavaScript 代码（Unicode 文本）。可用于验证按键输入，并拒绝或修改输入内容。若不存在，则为 *None*。

      .. attribute:: script_format

         * 版本 1.16.12 新增

         在字段格式化以显示当前值之前执行的 JavaScript 代码（Unicode 文本）。此操作可在格式化前修改字段值。若不存在，则为 *None*。

      .. attribute:: script_change

         * 版本 1.16.12 新增

         在字段值更改时执行的 JavaScript 代码（Unicode 文本）。此操作可用于验证新值。若不存在，则为 *None*。

      .. attribute:: script_calc

         * 版本 1.16.12 新增

         在其他字段值更改时重新计算此字段值的 JavaScript 代码（Unicode 文本）。若不存在，则为 *None*。

      .. attribute:: script_blur

         * 版本 1.22.6 新增

         失去焦点时执行的 JavaScript 代码（Unicode 文本）。若不存在，则为 *None*。

      .. attribute:: script_focus

         * 版本 1.22.6 新增

         获取焦点时执行的 JavaScript 代码（Unicode 文本）。若不存在，则为 *None*。

      .. note::

         1. **添加** 或 **更改** 上述脚本时，只需将相应的 JavaScript 源代码赋值给小部件（widget）属性。若要 **删除** 脚本，只需将相应属性设置为 *None*。

         2. 按钮（Button）字段仅支持 :attr:`script` 属性。其他脚本属性会自动设置为 *None*。

         3. 建议查阅 `这份手册 <https://experienceleague.adobe.com/docs/experience-manager-learn/assets/FormsAPIReference.pdf?lang=en>`_ ，其中包含大量关于 Adobe 标准脚本的信息，适用于各种字段类型。例如，如果要添加一个表示日期的文本字段，可以使用以下脚本：  

            这些脚本可确保日期格式符合模式要求，并在支持的查看器中显示日期选择器::

               widget.script_format = 'AFDate_FormatEx("mm/dd/yyyy");'
               widget.script_stroke = 'AFDate_KeystrokeEx("mm/dd/yyyy");'

.. tab:: 英文

   This class represents a PDF Form field, also called a "widget". Throughout this documentation, we are using these terms synonymously. Fields technically are a special case of PDF annotations, which allow users with limited permissions to enter information in a PDF. This is primarily used for filling out forms.

   Like annotations, widgets live on PDF pages. Similar to annotations, the first widget on a page is accessible via :attr:`Page.first_widget` and subsequent widgets can be accessed via the :attr:`Widget.next` property.

   *(Changed in version 1.16.0)* MuPDF no longer treats widgets as a subset of general annotations. Consequently, :attr:`Page.first_annot` and :meth:`Annot.next` will deliver **non-widget annotations exclusively**, and be *None* if only form fields exist on a page. Vice versa, :attr:`Page.first_widget` and :meth:`Widget.next` will only show widgets. This design decision is purely internal to MuPDF; technically, links, annotations and fields have a lot in common and also continue to share the better part of their code within (Py-) MuPDF.


   **Class API**

   .. class:: Widget
      :no-index:

      .. method:: button_states
         :no-index:

         *New in version 1.18.15*

         Return the names of On / Off (i.e. selected / clicked or not) states a button field may have. While the 'Off' state usually is also named like so, the 'On' state is often given a name relating to the functional context, for example 'Yes', 'Female', etc.

         This method helps finding out the possible values of :attr:`field_value` in these cases.

         :returns: a dictionary with the names of 'On' and 'Off' for the *normal* and the *pressed-down* appearance of button widgets. The following example shows that the "selected" value is "Male":

            >>> print(field.field_name, field.button_states())
            Gender Second person {'down': ['Male', 'Off'], 'normal': ['Male', 'Off']}


      .. method:: on_state
         :no-index:

         * New in version 1.22.2

         Return the value of the "ON" state of check boxes and radio buttons. For check boxes this is always the value "Yes". For radio buttons, this is the value to select / activate the button.

         :returns: the value that sets the button to "selected". For non-checkbox, non-radiobutton fields, always `None` is returned. For check boxes the return is `True`. For radio buttons this is the value "Male" in the following example:

            >>> print(field.field_name, field.button_states())
            Gender Second person {'down': ['Male', 'Off'], 'normal': ['Male', 'Off']}
            >>> print(field.on_state())
            Male

         So for check boxes and radio buttons, the recommended method to set them to "selected", or to check the state is the following:
         
            >>> field.field_value = field.on_state()
            >>> field.field_value == field.on_state()
            True


      .. method:: update
         :no-index:

         After any changes to a widget, this method **must be used** to store them in the PDF [#f1]_.

      .. method:: reset
         :no-index:

         Reset the field's value to its default -- if defined -- or remove it. Do not forget to issue :meth:`update` afterwards.

      .. attribute:: next
         :no-index:

         Point to the next form field on the page. The last widget returns *None*.

      .. attribute:: border_color
         :no-index:

         A list of up to 4 floats defining the field's border color. Default value is *None* which causes border style and border width to be ignored.

      .. attribute:: border_style
         :no-index:

         A string defining the line style of the field's border. See :attr:`Annot.border`. Default is "s" ("Solid") -- a continuous line. Only the first character (upper or lower case) will be regarded when creating a widget.

      .. attribute:: border_width
         :no-index:

         A float defining the width of the border line. Default is 1.

      .. attribute:: border_dashes
         :no-index:

         A list/tuple of integers defining the dash properties of the border line. This is only meaningful if *border_style == "D"* and :attr:`border_color` is provided.

      .. attribute:: choice_values
         :no-index:

         Python sequence of strings defining the valid choices of list boxes and combo boxes. For these widget types, this property is mandatory and must contain at least two items. Ignored for other types.

      .. attribute:: field_name
         :no-index:

         A mandatory string defining the field's name. No checking for duplicates takes place.

      .. attribute:: field_label
         :no-index:

         An optional string containing an "alternate" field name. Typically used for any notes, help on field usage, etc. Default is the field name.

      .. attribute:: field_value
         :no-index:

         The value of the field.

      .. attribute:: field_flags
         :no-index:

         An integer defining a large amount of properties of a field. Be careful when changing this attribute as this may change the field type.

      .. attribute:: field_type
         :no-index:

         A mandatory integer defining the field type. This is a value in the range of 0 to 6. It cannot be changed when updating the widget.

      .. attribute:: field_type_string
         :no-index:

         A string describing (and derived from) the field type.

      .. attribute:: fill_color
         :no-index:

         A list of up to 4 floats defining the field's background color.

      .. attribute:: button_caption
         :no-index:

         The caption string of a button-type field.

      .. attribute:: is_signed
         :no-index:

         A bool indicating the signing status of a signature field, else *None*.

      .. attribute:: rect
         :no-index:

         The rectangle containing the field.

      .. attribute:: text_color
         :no-index:

         A list of **1, 3 or 4 floats** defining the text color. Default value is black (`[0, 0, 0]`).

      .. attribute:: text_font
         :no-index:

         A string defining the font to be used. Default and replacement for invalid values is *"Helv"*. For valid font reference names see the table below.

      .. attribute:: text_fontsize
         :no-index:

         A float defining the text :data:`fontsize`. Default value is zero, which causes PDF viewer software to dynamically choose a size suitable for the annotation's rectangle and text amount.

      .. attribute:: text_maxlen
         :no-index:

         An integer defining the maximum number of text characters. PDF viewers will (should) not accept a longer text.

      .. attribute:: text_type
         :no-index:

         An integer defining acceptable text types (e.g. numeric, date, time, etc.). For reference only for the time being -- will be ignored when creating or updating widgets.

      .. attribute:: xref
         :no-index:

         The PDF :data:`xref` of the widget.

      .. attribute:: script
         :no-index:

         * New in version 1.16.12
         
         JavaScript text (unicode) for an action associated with the widget, or *None*. This is the only script action supported for **button type** widgets.

      .. attribute:: script_stroke
         :no-index:

         * New in version 1.16.12
         
         JavaScript text (unicode) to be performed when the user types a key-stroke into a text field or combo box or modifies the selection in a scrollable list box. This action can check the keystroke for validity and reject or modify it. *None* if not present.

      .. attribute:: script_format
         :no-index:

         * New in version 1.16.12
         
         JavaScript text (unicode) to be performed before the field is formatted to display its current value. This action can modify the field’s value before formatting. *None* if not present.

      .. attribute:: script_change
         :no-index:

         * New in version 1.16.12
         
         JavaScript text (unicode) to be performed when the field’s value is changed. This action can check the new value for validity. *None* if not present.

      .. attribute:: script_calc
         :no-index:

         * New in version 1.16.12
         
         JavaScript text (unicode) to be performed to recalculate the value of this field when that of another field changes. *None* if not present.

      .. attribute:: script_blur
         :no-index:

         * New in version 1.22.6
         
         JavaScript text (unicode) to be performed on losing the focus of this field. *None* if not present.

      .. attribute:: script_focus
         :no-index:

         * New in version 1.22.6
         
         JavaScript text (unicode) to be performed on focusing this field. *None* if not present.

      .. note::

         1. For **adding** or **changing** one of the above scripts,
            just put the appropriate JavaScript source code in the widget attribute.
            To **remove** a script, set the respective attribute to *None*.

         2. Button fields only support :attr:`script`.
            Other script entries will automatically be set to *None*.

         3. It is worthwhile to look at
            `this <https://experienceleague.adobe.com/docs/experience-manager-learn/assets/FormsAPIReference.pdf?lang=en>`_
            manual with lots of information about Adobe's standard scripts for various field types.
            For example, if you want to add a text field representing a date,
            you may want to store the following scripts.
            They will ensure pattern-compatible date formats and display date pickers in supporting viewers::

               widget.script_format = 'AFDate_FormatEx("mm/dd/yyyy");'
               widget.script_stroke = 'AFDate_KeystrokeEx("mm/dd/yyyy");'


小部件的标准字体
----------------------------------

Standard Fonts for Widgets

.. tab:: 中文

   小部件使用它们自己的资源对象 */DR* 。一个小部件资源对象至少必须包含一个 */Font* 对象。小部件字体独立于页面字体。当前支持14种PDF基本字体，可使用下表中的固定参考名称，或已存在的表单字体名称。当为新的或已更改的小部件指定文本字体时， **可以选择** 第一列表列中的名称（支持大小写）， **或者** 已存在的表单字体。在后者情况下，拼写必须完全匹配。

   要查看已存在的表单字体，请检查列表 :attr:`Document.FormFonts`。

   ============= =======================
   **参考名称**  **Base14字体名称**
   ============= =======================
   CoBI          Courier-BoldOblique
   CoBo          Courier-Bold
   CoIt          Courier-Oblique
   Cour          Courier
   HeBI          Helvetica-BoldOblique
   HeBo          Helvetica-Bold
   HeIt          Helvetica-Oblique
   Helv          Helvetica **(默认)**
   Symb          Symbol
   TiBI          Times-BoldItalic
   TiBo          Times-Bold
   TiIt          Times-Italic
   TiRo          Times-Roman
   ZaDb          ZapfDingbats
   ============= =======================

   通常情况下，可以为每个小部件使用任何字体。但是，我们建议对复选框使用 *ZaDb* （"ZapfDingbats"）和 :data:`fontsize` 设为0：大多数PDF查看器在单击复选框时会在字段矩形内正确显示一个合适大小的勾号。

.. tab:: 英文

   Widgets use their own resources object */DR*. A widget resources object must at least contain a */Font* object. Widget fonts are independent from page fonts. We currently support the 14 PDF base fonts using the following fixed reference names, or any name of an already existing field font. When specifying a text font for new or changed widgets, **either** choose one in the first table column (upper and lower case supported), **or** one of the already existing form fonts. In the latter case, spelling must exactly match.

   To find out already existing field fonts, inspect the list :attr:`Document.FormFonts`.

   ============= =======================
   **Reference** **Base14 Fontname**
   ============= =======================
   CoBI          Courier-BoldOblique
   CoBo          Courier-Bold
   CoIt          Courier-Oblique
   Cour          Courier
   HeBI          Helvetica-BoldOblique
   HeBo          Helvetica-Bold
   HeIt          Helvetica-Oblique
   Helv          Helvetica **(default)**
   Symb          Symbol
   TiBI          Times-BoldItalic
   TiBo          Times-Bold
   TiIt          Times-Italic
   TiRo          Times-Roman
   ZaDb          ZapfDingbats
   ============= =======================

   You are generally free to use any font for every widget. However, we recommend using *ZaDb* ("ZapfDingbats") and :data:`fontsize` 0 for check boxes: typical viewers will put a correctly sized tickmark in the field's rectangle, when it is clicked.

支持的小部件类型
-----------------------

Supported Widget Types

.. tab:: 中文

   PyMuPDF 支持创建和更新许多类型的小部件，但并非所有类型都受支持。

   * 文本 (`PDF_WIDGET_TYPE_TEXT`)
   * 按钮 (`PDF_WIDGET_TYPE_BUTTON`)
   * 复选框 (`PDF_WIDGET_TYPE_CHECKBOX`)
   * 组合框 (`PDF_WIDGET_TYPE_COMBOBOX`)
   * 列表框 (`PDF_WIDGET_TYPE_LISTBOX`)
   * 单选按钮 (`PDF_WIDGET_TYPE_RADIOBUTTON`): 
   
     目前，PyMuPDF 不支持 **创建** （互相关联的）单选按钮组，即选中一个按钮时，自动取消选中同组中的其他按钮。此外，小部件对象也不会反映按钮组的存在。然而， **一致地选中或取消选中单选按钮是支持的** ，包括正确设置所属按钮组中维护的值。选中单选按钮可通过将 `True` 或 `field.on_state()` 赋值给字段值来完成。 **取消选中** 可通过赋值 `False` 来完成。

   * 签名 (`PDF_WIDGET_TYPE_SIGNATURE`) **只读**。

.. tab:: 英文

   PyMuPDF supports the creation and update of many, but not all widget types.

   * text (`PDF_WIDGET_TYPE_TEXT`)
   * push button (`PDF_WIDGET_TYPE_BUTTON`)
   * check box (`PDF_WIDGET_TYPE_CHECKBOX`)
   * combo box (`PDF_WIDGET_TYPE_COMBOBOX`)
   * list box (`PDF_WIDGET_TYPE_LISTBOX`)
   * radio button (`PDF_WIDGET_TYPE_RADIOBUTTON`): PyMuPDF does not currently support the **creation** of groups of (interconnected) radio buttons, where setting one automatically unsets the other buttons in the group. The widget object also does not reflect the presence of a button group. However: consistently selecting (or unselecting) a radio button is supported. This includes correctly setting the value maintained in the owning button group. Selecting a radio button may be done by either assigning `True` or `field.on_state()` to the field value. **De-selecting** the button should be done assigning `False`.
   * signature (`PDF_WIDGET_TYPE_SIGNATURE`) **read only**.

.. rubric:: Footnotes

.. [#f1] If you intend to re-access a new or updated field (e.g. for making a pixmap), make sure to reload the page first. Either close and re-open the document, or load another page first, or simply do `page = doc.reload_page(page)`.

.. [#f2] 如果您计划重新访问新建或更新的字段（例如生成位图），请确保先重新加载页面。可以关闭并重新打开文档，或者先加载另一个页面，或者简单地执行 `page = doc.reload_page(page)`。

.. include:: footer.rst
