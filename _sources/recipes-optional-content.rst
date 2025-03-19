.. include:: header.rst

.. _RecipesOptionalContent:

==============================
可选内容支持
==============================

Optional Content Support

.. tab:: 中文

    本文档解释了 PyMuPDF 对 PDF 概念 **“可选内容”** 的支持。

.. tab:: 英文

    This document explains PyMuPDF's support of the PDF concept **"Optional Content"**.

简介：可选内容概念
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Introduction: The Optional Content Concept

.. tab:: 中文

    PDF 中的 *可选内容* 是一种根据特定条件显示或隐藏文档部分内容的方法：使用支持的 PDF 使用者（查看器）或以编程方式将参数设置为 ON 或 OFF。

    此功能在 CAD 绘图、分层图稿、地图和多语言文档等项目中非常有用。典型用途包括显示或隐藏复杂矢量图形（如地理地图、技术设备、建筑设计等）的细节，包括自动在不同缩放级别之间切换。其他用例可能是在屏幕上显示文档而不是打印文档时自动显示不同的细节级别。

    特殊 PDF 对象，即所谓的 **可选内容组** (OCG) 用于定义这些不同的内容 *层*。

    将 OCG 分配给“普通”PDF 对象（如文本或图像）会导致该对象可见或隐藏，具体取决于分配的 OCG 的当前状态。

    为了便于定义 PDF 可选内容的整体配置，OCG 可以组织成更高级别的分组，称为 **OC 配置**。每个配置都是 OCG 的集合，以及每个 OCG 所需的初始可见性状态。选择其中一个配置（通过 PDF 查看器或以编程方式）会导致整个文档中所有受影响的 PDF 对象的可见性发生相应的变化。

    除默认配置外，OC 配置都是可选的。

    有关更多说明和其他背景信息，请参阅 PDF 规范手册。

.. tab:: 英文

    *Optional Content* in PDF is a way to show or hide parts of a document based on certain conditions: Parameters that can be set to ON or to OFF when using a supporting PDF consumer (viewer), or programmatically.

    This capability is useful in items such as CAD drawings, layered artwork, maps, and multi-language documents. Typical uses include showing or hiding details of complex vector graphics like geographical maps, technical devices, architectural designs and similar, including automatically switching between different zooming levels. Other use cases may be to automatically show different detail levels when displaying a document on screen as opposed to printing it.

    Special PDF objects, so-called **Optional Content Groups** (OCGs) are used to define these different *layers* of content.

    Assigning an OCG to a "normal" PDF object (like a text or an image) causes that object to be visible or hidden, depending on the current state of the assigned OCG.

    To ease definition of the overall configuration of a PDF's Optional Content, OCGs can be organized in higher level groupings, called **OC Configurations**. Each configuration being a collection of OCGs, together with each OCG's desired initial visibility state. Selecting one of these configurations (via the PDF viewer or programmatically) causes a corresponding visibility change of all affected PDF objects throughout the document.

    Except for the default one, OC Configurations are optional.

    For more explanations and additional background please refer to PDF specification manuals.

PyMuPDF 支持 PDF 可选内容
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

PyMuPDF Support for PDF Optional Content

.. tab:: 中文

    PyMuPDF 完全支持查看、定义、更改和删除选项内容组、配置、维护 OCG 对 PDF 对象的分配以及以编程方式在 OC 配置和每个单个 OCG 的可见性状态之间切换。

.. tab:: 英文

    PyMuPDF offers full support for viewing, defining, changing and deleting Option Content Groups, Configurations, maintaining the assignment of OCGs to PDF objects and programmatically switching between OC Configurations and the visibility states of each single OCG.

如何添加可选内容
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Add Optional Content

.. tab:: 中文

    这就像向 PDF 添加可选内容组 OCG 一样简单：:meth:`Document.add_ocg`。

    如果之前的 PDF 根本不支持 OC，则所需的设置（如定义默认 OC 配置）将在此时自动完成。

    该方法返回已创建的 OCG 的 :data:`xref` 。使用此 xref 关联（标记）任何 PDF 对象，您希望使其依赖于此 OCG 的状态。例如，您可以在页面上插入图像并引用 xref，如下所示::

        img_xref = page.insert_image(rect, filename="image.file", oc=xref)

    如果您想将 **现有** 图像置于 OCG 的控制之下，您必须首先找出图像的 xref 编号（此处称为 `img_xref` ），然后执行 `doc.set_oc(img_xref, xref)`。此后，如果 OCG 的状态分别为“ON”和“OFF”，则图像将在整个文档的任何地方可见。您还可以使用此方法分配不同的 OCG。

    要从图像中 **删除** OCG，请执行 `doc.set_oc(img_xref, 0)` 。

    可以将一个 OCG 分配给多个 PDF 对象以控制它们的可见性。

.. tab:: 英文

    This is as simple as adding an Optional Content Group, OCG, to a PDF: :meth:`Document.add_ocg`.

    If previously the PDF had no OC support at all, the required setup (like defining the default OC Configuration) will be done at this point automatically.

    The method returns an :data:`xref` of the created OCG. Use this xref to associate (mark) any PDF object with it, that you want to make dependent on this OCG's state. For example, you can insert an image on a page and refer to the xref like this::

        img_xref = page.insert_image(rect, filename="image.file", oc=xref)

    If you want to put an **existing** image under the control of an OCG, you must first find out the image's xref number (called `img_xref` here) and then do `doc.set_oc(img_xref, xref)`. After this, the image will be (in-) visible everywhere throughout the document if the OCG's state is "ON", respectively "OFF". You can also assign a different OCG with this method.

    To **remove** an OCG from an image, do `doc.set_oc(img_xref, 0)`.

    One single OCG can be assigned to multiple PDF objects to control their visibility.

如何定义复杂的可选内容条件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Define Complex Optional Content Conditions

.. tab:: 中文

    可以建立复杂的逻辑条件来满足复杂的可见性需求。

    例如，您可能想要创建一个多语言文档，以便用户可以根据需要在语言之间切换。

    请查看 `这个 Jupyter Notebook`_ 并根据需要执行它。

    当然，您的要求甚至可能更复杂，涉及多个具有 ON/OFF 状态的 OCG，这些 OCG 通过某种逻辑关系连接在一起——但它应该让您了解什么是可能的以及如何规划您的下一步。

.. tab:: 英文

    Sophisticated logical conditions can be established to address complex visibility needs.

    For example, you might want to create a multi-language document, so the user may switch between languages as required.

    Please have a look at `this Jupyter Notebook`_ and execute it as desired.

    Certainly, your requirements may even be more complex and involve multiple OCGs with ON/OFF states that are connected by some kind of logical relationship -- but it should give you an impression of what is possible and how to plan your next steps.

.. include:: footer.rst

.. External Links:

.. _this Jupyter Notebook: https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/jupyter-notebooks/optional-content.ipynb

.. _这个 Jupyter Notebook: https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/jupyter-notebooks/optional-content.ipynb



