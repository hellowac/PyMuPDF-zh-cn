.. include:: header.rst

.. _DisplayList:

================
DisplayList
================

.. tab:: 中文

   DisplayList 是一个包含绘图命令（文本、图像等）的列表。其目的是双重的：

   1. 作为缓存机制，以减少页面解析。
   2. 作为多线程设置中的数据结构，其中一个线程解析页面，另一个线程渲染页面。此功能目前在 PyMuPDF 中不支持。

   显示列表通过从页面中提取对象进行填充，通常通过执行 :meth:`Page.get_displaylist`。还存在一个独立的构造函数。

   通过调用其方法之一：:meth:`~DisplayList.run`、:meth:`~DisplayList.get_pixmap` 或 :meth:`~DisplayList.get_textpage`，可以“重播”列表（一次或多次）。

   ================================= ============================================
   **方法**                          **简短描述**         
   ================================= ============================================
   :meth:`~DisplayList.run`          通过设备运行显示列表。
   :meth:`~DisplayList.get_pixmap`   生成一个 pixmap。
   :meth:`~DisplayList.get_textpage` 生成文本页面。
   :attr:`~DisplayList.rect`         显示列表的媒体框。
   ================================= ============================================

   **类 API**

   .. class:: DisplayList

      .. method:: __init__(self, mediabox)

         创建一个新的显示列表。

         :arg mediabox: 页面矩形。
         :type mediabox: :ref:`Rect`

         :rtype: *DisplayList*

      .. method:: run(device, matrix, area)
      
         通过设备运行显示列表。设备将使用其“命令”填充显示列表（即文本提取或图像创建）。稍后，您可以多次使用该显示列表来“读取”页面，而无需从文档文件重新解析。

         您通常会使用下面的专用运行方法之一——:meth:`get_pixmap` 或 :meth:`get_textpage`。

         :arg device: 设备
         :type device: :ref:`Device`

         :arg matrix: 要应用于显示列表内容的转换矩阵。
         :type matrix: :ref:`Matrix`

         :arg area: 仅在此区域内可见的部分将被视为当列表通过设备运行时。
         :type area: :ref:`Rect`

      .. index::
         pair: matrix; DisplayList.get_pixmap
         pair: colorspace; DisplayList.get_pixmap
         pair: clip; DisplayList.get_pixmap
         pair: alpha; DisplayList.get_pixmap

      .. method:: get_pixmap(matrix=pymupdf.Identity, colorspace=pymupdf.csRGB, alpha=0, clip=None)

         通过绘图设备运行显示列表并返回一个 pixmap。

         :arg matrix: 要使用的矩阵。默认是单位矩阵。
         :type matrix: :ref:`Matrix`

         :arg colorspace: 所需的颜色空间。默认是 RGB。
         :type colorspace: :ref:`Colorspace`

         :arg int alpha: 确定是否包含透明通道（0，默认）。
         
         :arg irect_like clip: 将渲染限制为此区域与 :attr:`DisplayList.rect` 的交集。

         :rtype: :ref:`Pixmap`
         :returns: 显示列表的 pixmap。

      .. method:: get_textpage(flags)

         通过文本设备运行显示列表并返回一个文本页面。

         :arg int flags: 控制哪些信息被解析为文本页面。PyMuPDF 中的默认值为 `3 = TEXT_PRESERVE_LIGATURES | TEXT_PRESERVE_WHITESPACE`，即： :data:`ligatures` **传递**，空格 **传递** （不转换为空格），图像 **不包含** 。参见 :ref:`TextPreserve`。

         :rtype: :ref:`TextPage`
         :returns: 显示列表的文本页面。

      .. attribute:: rect

         包含显示列表的媒体框。如果是通过 :meth:`Page.get_displaylist` 创建的，则此值将等于页面的矩形。

         :type: :ref:`Rect`


.. tab:: 英文

   DisplayList is a list containing drawing commands (text, images, etc.). The intent is two-fold:

   1. as a caching-mechanism to reduce parsing of a page
   2. as a data structure in multi-threading setups, where one thread parses the page and another one renders pages. This aspect is currently not supported by PyMuPDF.

   A display list is populated with objects from a page, usually by executing :meth:`Page.get_displaylist`. There also exists an independent constructor.

   "Replay" the list (once or many times) by invoking one of its methods :meth:`~DisplayList.run`, :meth:`~DisplayList.get_pixmap` or :meth:`~DisplayList.get_textpage`.


   ================================= ============================================
   **Method**                        **Short Description**
   ================================= ============================================
   :meth:`~DisplayList.run`          Run a display list through a device.
   :meth:`~DisplayList.get_pixmap`   generate a pixmap
   :meth:`~DisplayList.get_textpage` generate a text page
   :attr:`~DisplayList.rect`         mediabox of the display list
   ================================= ============================================


   **Class API**

   .. class:: DisplayList
      :no-index:

      .. method:: __init__(self, mediabox)
         :no-index:

         Create a new display list.

         :arg mediabox: The page's rectangle.
         :type mediabox: :ref:`Rect`

         :rtype: *DisplayList*

      .. method:: run(device, matrix, area)
         :no-index:
      
         Run the display list through a device. The device will populate the display list with its "commands" (i.e. text extraction or image creation). The display list can later be used to "read" a page many times without having to re-interpret it from the document file.

         You will most probably instead use one of the specialized run methods below -- :meth:`get_pixmap` or :meth:`get_textpage`.

         :arg device: Device
         :type device: :ref:`Device`

         :arg matrix: Transformation matrix to apply to the display list contents.
         :type matrix: :ref:`Matrix`

         :arg area: Only the part visible within this area will be considered when the list is run through the device.
         :type area: :ref:`Rect`

      .. index::
         pair: matrix; DisplayList.get_pixmap
         pair: colorspace; DisplayList.get_pixmap
         pair: clip; DisplayList.get_pixmap
         pair: alpha; DisplayList.get_pixmap

      .. method:: get_pixmap(matrix=pymupdf.Identity, colorspace=pymupdf.csRGB, alpha=0, clip=None)
         :no-index:

         Run the display list through a draw device and return a pixmap.

         :arg matrix: matrix to use. Default is the identity matrix.
         :type matrix: :ref:`Matrix`

         :arg colorspace: the desired colorspace. Default is RGB.
         :type colorspace: :ref:`Colorspace`

         :arg int alpha: determine whether or not (0, default) to include a transparency channel.

         :arg irect_like clip: restrict rendering to the intersection of this area with :attr:`DisplayList.rect`.

         :rtype: :ref:`Pixmap`
         :returns: pixmap of the display list.

      .. method:: get_textpage(flags)
         :no-index:

         Run the display list through a text device and return a text page.

         :arg int flags: control which information is parsed into a text page. Default value in PyMuPDF is `3 = TEXT_PRESERVE_LIGATURES | TEXT_PRESERVE_WHITESPACE`, i.e. :data:`ligatures` are **passed through**, white spaces are **passed through** (not translated to spaces), and images are **not included**. See :ref:`TextPreserve`.

         :rtype: :ref:`TextPage`
         :returns: text page of the display list.

      .. attribute:: rect
         :no-index:

         Contains the display list's mediabox. This will equal the page's rectangle if it was created via :meth:`Page.get_displaylist`.

         :type: :ref:`Rect`


.. include:: footer.rst
