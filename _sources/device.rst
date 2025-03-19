.. include:: header.rst

.. _Device:

================
设备
================

Device

.. tab:: 中文

   不同的格式处理器（如 pdf, xps 等）将页面解释为“设备”。设备是对页面进行所有操作的基础：渲染、文本提取和搜索。设备类型由所选的构造方法决定。

   **类 API**

   .. class:: Device

      .. method:: __init__(self, object, clip)

         构造函数，用于像素图或显示列表设备。

         :arg object: 可以是 *Pixmap* 或 *DisplayList*。
         :type object: :ref:`Pixmap` 或 :ref:`DisplayList`

         :arg clip: 可选的 `IRect`，用于 *Pixmap* 设备，限制渲染在页面的特定区域。如果需要完整的页面，指定 *None*。对于显示列表设备，此参数必须省略。
         :type clip: :ref:`IRect`

      .. method:: __init__(self, textpage, flags=0)

         构造函数，用于文本页面设备。

         :arg textpage: *TextPage* 对象
         :type textpage: :ref:`TextPage`

         :arg int flags: 控制如何将文本解析到文本页面。目前，3 个选项可以编码到此参数中，参见 :ref:`TextPreserve`。要设置这些选项，可以使用类似 *flags=0 | TEXT_PRESERVE_LIGATURES | ...* 的方式。


.. tab:: 英文

   The different format handlers (pdf, xps, etc.) interpret pages to a "device". Devices are the basis for everything that can be done with a page: rendering, text extraction and searching. The device type is determined by the selected construction method.

   **Class API**

   .. class:: Device
      :no-index:

      .. method:: __init__(self, object, clip)
         :no-index:

         Constructor for either a pixel map or a display list device.

         :arg object: either a *Pixmap* or  a *DisplayList*.
         :type object: :ref:`Pixmap` or :ref:`DisplayList`

         :arg clip: An optional `IRect` for *Pixmap* devices to restrict rendering to a certain area of the page. If the complete page is required, specify *None*. For display list devices, this parameter must be omitted.
         :type clip: :ref:`IRect`

      .. method:: __init__(self, textpage, flags=0)
         :no-index:

         Constructor for a text page device.

         :arg textpage: *TextPage* object
         :type textpage: :ref:`TextPage`

         :arg int flags: control the way how text is parsed into the text page. Currently 3 options can be coded into this parameter, see :ref:`TextPreserve`. To set these options use something like *flags=0 | TEXT_PRESERVE_LIGATURES | ...*.

.. include:: footer.rst
