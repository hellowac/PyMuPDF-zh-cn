.. include:: header.rst

.. _linkDest:

================
linkDest
================

.. tab:: 中文

   表示大纲条目或链接的 `dest` 属性的类。描述了这些条目指向的目的地。

   .. note:: 在 MuPDF v1.9.0 版本之前，此类存在于 MuPDF 内部，并在 v1.10.0 版本中被删除。为了向后兼容，PyMuPDF 仍然维护它，尽管它的一些属性不再由 MuPDF 中实际可用的数据支持。

   =========================== ====================================
   **属性**                    **简短描述**
   =========================== ====================================
   :attr:`linkDest.dest`        目标位置
   :attr:`linkDest.fileSpec`    文件规范（路径，文件名）
   :attr:`linkDest.flags`       描述性标志
   :attr:`linkDest.isMap`       是否为 MAP？
   :attr:`linkDest.isUri`       是否为 URI？
   :attr:`linkDest.kind`        目的地类型
   :attr:`linkDest.lt`          左上坐标
   :attr:`linkDest.named`       如果是命名的目的地，则为名称
   :attr:`linkDest.newWindow`   新窗口的名称
   :attr:`linkDest.page`        页码
   :attr:`linkDest.rb`          右下坐标
   :attr:`linkDest.uri`         URI
   =========================== ====================================

   **类 API**

   .. class:: linkDest

      .. attribute:: dest

         如果 :attr:`linkDest.kind` 是 :data:`LINK_GOTOR` 并且 :attr:`linkDest.page` 为 *-1*，则为目标目的地名称。

         :type: str

      .. attribute:: fileSpec

         如果 :attr:`linkDest.kind` 是 :data:`LINK_GOTOR` 或 :data:`LINK_LAUNCH`，则包含此链接指向的文件名和路径。

         :type: str

      .. attribute:: flags

         一个描述目的地各个方面有效性和含义的位字段。尽可能地，链接目的地被构建为例如 :attr:`linkDest.lt` 和 :attr:`linkDest.rb` 可以被视为定义一个边界框。但标志指示哪些值实际上是指定的，请参见 :ref:`linkDest Flags`。

         :type: int

      .. attribute:: isMap

         该标志指定是否在解析 URI 时跟踪鼠标位置。默认值：False。

         :type: bool

      .. attribute:: isUri

         指定该目的地是否为互联网资源（与例如本地文件规范的 URI 格式相对）。

         :type: bool

      .. attribute:: kind

         指示该目的地的类型，例如本文件中的位置、URI、文件启动、动作或另一个文件中的位置。查看 :ref:`linkDest Kinds` 以查看名称和数值。

         :type: int

      .. attribute:: lt

         目标的左上 :ref:`Point`。

         :type: :ref:`Point`

      .. attribute:: named

         此目的地指向某个命名的操作（例如 JavaScript，参见 :ref:`AdobeManual`）。提供的标准操作有 *NextPage*、*PrevPage*、*FirstPage* 和 *LastPage*。

         :type: str

      .. attribute:: newWindow

         如果为真，目标应在新窗口中打开。

         :type: bool

      .. attribute:: page

         此目的地指向的页码（在本文件或目标文件中）。只有当 :attr:`linkDest.kind` 为 :data:`LINK_GOTOR` 或 :data:`LINK_GOTO` 时才设置。如果 :attr:`linkDest.kind` 为 :data:`LINK_GOTOR`，则可能为 *-1*。在这种情况下，:attr:`linkDest.dest` 包含目标文件中目的地的 **名称** 。

         :type: int

      .. attribute:: rb

         此目的地的右下 :ref:`Point`。

         :type: :ref:`Point`

      .. attribute:: uri

         此目的地指向的 URI 名称。

         :type: str


.. tab:: 英文

   Class representing the `dest` property of an outline entry or a link. Describes the destination to which such entries point.

   .. note:: Up to MuPDF v1.9.0 this class existed inside MuPDF and was dropped in version 1.10.0. For backward compatibility, PyMuPDF is still maintaining it, although some of its attributes are no longer backed by data actually available via MuPDF.

   =========================== ====================================
   **Attribute**               **Short Description**
   =========================== ====================================
   :attr:`linkDest.dest`       destination
   :attr:`linkDest.fileSpec`   file specification (path, filename)
   :attr:`linkDest.flags`      descriptive flags
   :attr:`linkDest.isMap`      is this a MAP?
   :attr:`linkDest.isUri`      is this a URI?
   :attr:`linkDest.kind`       kind of destination
   :attr:`linkDest.lt`         top left coordinates
   :attr:`linkDest.named`      name if named destination
   :attr:`linkDest.newWindow`  name of new window
   :attr:`linkDest.page`       page number
   :attr:`linkDest.rb`         bottom right coordinates
   :attr:`linkDest.uri`        URI
   =========================== ====================================

   **Class API**

   .. class:: linkDest
      :no-index:

      .. attribute:: dest
         :no-index:

         Target destination name if :attr:`linkDest.kind` is :data:`LINK_GOTOR` and :attr:`linkDest.page` is *-1*.

         :type: str

      .. attribute:: fileSpec
         :no-index:

         Contains the filename and path this link points to, if :attr:`linkDest.kind` is :data:`LINK_GOTOR` or :data:`LINK_LAUNCH`.

         :type: str

      .. attribute:: flags
         :no-index:

         A bitfield describing the validity and meaning of the different aspects of the destination. As far as possible, link destinations are constructed such that e.g. :attr:`linkDest.lt` and :attr:`linkDest.rb` can be treated as defining a bounding box. But the flags indicate which of the values were actually specified, see :ref:`linkDest Flags`.

         :type: int

      .. attribute:: isMap
         :no-index:

         This flag specifies whether to track the mouse position when the URI is resolved. Default value: False.

         :type: bool

      .. attribute:: isUri
         :no-index:

         Specifies whether this destination is an internet resource (as opposed to e.g. a local file specification in URI format).

         :type: bool

      .. attribute:: kind
         :no-index:

         Indicates the type of this destination, like a place in this document, a URI, a file launch, an action or a place in another file. Look at :ref:`linkDest Kinds` to see the names and numerical values.

         :type: int

      .. attribute:: lt
         :no-index:

         The top left :ref:`Point` of the destination.

         :type: :ref:`Point`

      .. attribute:: named
         :no-index:

         This destination refers to some named action to perform (e.g. a javascript, see :ref:`AdobeManual`). Standard actions provided are *NextPage*, *PrevPage*, *FirstPage*,  and *LastPage*.

         :type: str

      .. attribute:: newWindow
         :no-index:

         If true, the destination should be launched in a new window.

         :type: bool

      .. attribute:: page
         :no-index:

         The page number (in this or the target document) this destination points to. Only set if :attr:`linkDest.kind` is :data:`LINK_GOTOR` or :data:`LINK_GOTO`. May be *-1* if :attr:`linkDest.kind` is :data:`LINK_GOTOR`. In this case :attr:`linkDest.dest` contains the **name** of a destination in the target document.

         :type: int

      .. attribute:: rb
         :no-index:

         The bottom right :ref:`Point` of this destination.

         :type: :ref:`Point`

      .. attribute:: uri
         :no-index:

         The name of the URI this destination points to.

         :type: str

.. include:: footer.rst
