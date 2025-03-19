.. include:: header.rst

.. _Outline:

================
Outline
================

.. tab:: 中文

   **Outline** （或“书签”）是 *Document* 的一个属性。如果它不是 *None*，则表示文档的第一个书签项。其属性定义了该书签项的特性，并且指向“水平方向”或向下的其他书签项。通过跟踪这些“指针”，可以恢复整个书签树，例如传统的目录（TOC）。

   ============================ ==================================================
   **方法 / 属性**              **简短描述**         
   ============================ ==================================================
   :attr:`Outline.down`         下一级书签项
   :attr:`Outline.next`         同一级的下一个书签项
   :attr:`Outline.page`         页码（从0开始）
   :attr:`Outline.title`        书签项的标题
   :attr:`Outline.uri`          进一步指定书签目标的字符串
   :attr:`Outline.is_external`  目标是否在文档外部
   :attr:`Outline.is_open`      子书签是否展开（打开）或折叠
   :attr:`Outline.dest`         指向目标详情对象
   ============================ ==================================================

   **类 API**

   .. class:: Outline

      .. attribute:: down

         下一级书签项。如果该项没有子项，则为 *None*。

         :type: :ref:`Outline`

      .. attribute:: next

         同一级的下一个书签项。如果这是该层级的最后一个书签项，则为 *None*。

         :type: :ref:`Outline`

      .. attribute:: page

         此书签指向的页码（从0开始）。

         :type: int

      .. attribute:: title

         书签项的标题，以字符串形式表示，如果没有标题则为 *None*。

         :type: str

      .. attribute:: is_open

         指示是否展开任何子书签项（*True* 为展开，*False* 为折叠）。此信息由PDF阅读器软件解释。

         :type: bool

      .. attribute:: is_external

         布尔值，指定目标是否在当前文档之外（*True* 为外部目标）。

         :type: bool

      .. attribute:: uri

         字符串，指定书签目标。在结合 `is_external` 属性时，`uri` 的含义如下：
         
         * `is_external` 为真时，``uri`` 指向当前 PDF 之外的某个目标，可能是互联网资源（``uri`` 以 ``http://`` 或类似开头）、其他文件（``uri`` 以 ``file:`` 或 ``file://`` 开头）或其他服务（如电子邮件地址，``uri`` 以 ``mailto:`` 开头）。

         * `is_external` 为假时，``uri`` 将为 `None` 或指向一个内部位置。对于PDF文档，这应当是 *#nnnn*，表示一个基于1的页码 *nnnn*，或一个命名位置。其他文档类型的格式可能有所不同，例如对于XPS文档，格式可能为 "../FixedDoc.fdoc#PG_2_LNK_1"，表示XPS文档第2页（基于1的页码）。

         :type: str

      .. attribute:: dest

         书签链接目标的详情对象。

         :type: :ref:`linkDest`

.. tab:: 英文

   *outline* (or "bookmark"), is a property of *Document*. If not *None*, it stands for the first outline item of the document. Its properties in turn define the characteristics of this item and also point to other outline items in "horizontal" or downward direction. The full tree of all outline items for e.g. a conventional table of contents (TOC) can be recovered by following these "pointers".

   ============================ ==================================================
   **Method / Attribute**       **Short Description**
   ============================ ==================================================
   :attr:`Outline.down`         next item downwards
   :attr:`Outline.next`         next item same level
   :attr:`Outline.page`         page number (0-based)
   :attr:`Outline.title`        title
   :attr:`Outline.uri`          string further specifying outline target
   :attr:`Outline.is_external`  target outside document
   :attr:`Outline.is_open`      whether sub-outlines are open or collapsed
   :attr:`Outline.dest`         points to destination details object
   ============================ ==================================================

   **Class API**

   .. class:: Outline
      :no-index:

      .. attribute:: down
         :no-index:

         The next outline item on the next level down. Is *None* if the item has no children.

         :type: :ref:`Outline`

      .. attribute:: next
         :no-index:

         The next outline item at the same level as this item. Is *None* if this is the last one in its level.

         :type: `Outline`

      .. attribute:: page
         :no-index:

         The page number (0-based) this bookmark points to.

         :type: int

      .. attribute:: title
         :no-index:

         The item's title as a string or *None*.

         :type: str

      .. attribute:: is_open
         :no-index:

         Indicator showing whether any sub-outlines should be expanded (*True*) or be collapsed (*False*). This information is interpreted by PDF reader software.

         :type: bool

      .. attribute:: is_external
         :no-index:

         A bool specifying whether the target is outside (*True*) of the current document.

         :type: bool

      .. attribute:: uri
         :no-index:

         A string specifying the link target. The meaning of this property should
         be evaluated in conjunction with property `is_external`:
         
         *
         `is_external` is true: ``uri`` points to some target outside the current
         PDF, which may be an internet resource (``uri`` starts with ``http://`` or
         similar), another file (``uri`` starts with ``file:`` or ``file://``) or some
         other service like an e-mail address (``uri`` starts with ``mailto:``).

         *
         `is_external` is false: ``uri`` will be `None` or point to an
         internal location. In case of PDF documents, this should either be
         *#nnnn* to indicate a 1-based (!) page number *nnnn*, or a named
         location. The format varies for other document types, for example
         "../FixedDoc.fdoc#PG_2_LNK_1" for page number 2 (1-based) in an XPS
         document.

         :type: str

      .. attribute:: dest
         :no-index:

         The link destination details object.

         :type: :ref:`linkDest`

.. include:: footer.rst
