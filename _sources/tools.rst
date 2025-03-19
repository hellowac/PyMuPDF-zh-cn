.. include:: header.rst

.. _Tools:

Tools
================

.. tab:: 中文

   该类是一个实用方法和属性的集合，主要用于内存管理。为了简化和加速其使用，当PyMuPDF被导入时，它会自动实例化为名为 *TOOLS* 的对象。

   ====================================== =================================================
   **方法 / 属性**                        **描述**
   ====================================== =================================================
   :meth:`Tools.gen_id`                   生成唯一的标识符
   :meth:`Tools.store_shrink`             缩小存储缓存 [#f3]_
   :meth:`Tools.mupdf_warnings`           返回累积的MuPDF警告
   :meth:`Tools.mupdf_display_errors`     控制是否将MuPDF错误作为消息显示
   :meth:`Tools.mupdf_display_warnings`   控制是否将MuPDF警告作为消息显示
   :meth:`Tools.reset_mupdf_warnings`     清空MuPDF警告/错误消息缓冲区
   :meth:`Tools.set_aa_level`             设置抗锯齿值
   :meth:`Tools.set_annot_stem`           设置新注释/链接ID的前缀
   :meth:`Tools.set_small_glyph_heights`  使用较小的bbox高度进行搜索和提取
   :meth:`Tools.set_subset_fontnames`     控制子集字体名标签的抑制
   :meth:`Tools.show_aa_level`            返回抗锯齿值
   :meth:`Tools.unset_quad_corrections`   禁用PyMuPDF特有的代码
   :attr:`Tools.fitz_config`              PyMuPDF的配置设置
   :attr:`Tools.store_maxsize`            最大存储缓存大小
   :attr:`Tools.store_size`               当前存储缓存大小
   ====================================== =================================================

   **类 API**

   .. class:: Tools

      .. method:: gen_id()

         一个方便的方法，返回一个唯一的正整数，每次调用时该整数会增加1。示例用法包括在数据库中创建唯一的键——它的创建速度应比使用时间戳快一个数量级。

         .. note:: MuPDF已在v1.14.0版本中不再支持此功能，因此我们重新实现了一个类似的函数，具有以下区别：

               * 它不是MuPDF的全局上下文的一部分，也不是线程安全的（因为我们在PyMuPDF中不支持线程，所以这不是问题）。
               * 它被实现为 *int* 类型。这意味着最大值为 *sys.maxsize* 。如果这个数字被超过，计数器将从1重新开始。

         :rtype: int
         :returns: 一个唯一的正整数。

      .. method:: set_annot_stem(stem=None)

         * 新增于v1.18.6

         设置或查询新注释、字段或链接ID的前缀。

         :arg str stem: 如果省略，则返回当前值，默认值为“fitz”。注释、字段/小部件和链接在PDF文档中实际上是同一类型对象(`/Annot`)的子类型。一个`/Annot`对象可以在页面中具有唯一的标识符。对于每种适用的子类型，PyMuPDF会生成“stem-Annn”、“stem-Wnnn”或“stem-Lnnn”的标识符，其中“nnn”用于强制要求唯一性。

         :rtype: str
         :returns: 当前值。

      .. method:: set_small_glyph_heights(on=None)

         * 新增于v1.18.5

         设置或查询文本提取和文本搜索方法中的缩小bbox高度。

         :arg bool on: 如果省略或为 `None`，则返回当前设置。对于其他值，将应用 *bool()* 函数来设置全局变量。如果为 `True`，则 :meth:`Page.search_for` 和 :meth:`Page.get_text` 方法返回的字符、跨度、行或块的bbox高度将是 *字体大小* 。如果为 `False` （PyMuPDF导入时的标准设置），则bbox高度将基于字体属性，并通常等于 *行高* 。

         :rtype: bool
         :returns: *True* 或 *False*。

         .. note:: 文本提取选项“xml”、“xhtml”和“html”，这些直接封装了MuPDF代码，不受此设置的影响。

      .. method:: set_subset_fontnames(on=None)

         * 新增于v1.18.9

         控制在文本提取时是否抑制子集字体名称标签。

         :arg bool on: 如果省略或为 `None` ，则返回当前设置。将传入 `True` 或 `False` 值会设置全局变量。如果为 `True` ，选项"dict"、"json"、"rawdict"和"rawjson"将返回 `"NOHSJV+Calibri-Light"` ，否则将仅返回 `"Calibri-Light"` （默认值）。该设置会一直生效，直到再次更改。

         :rtype: bool
         :returns: *True* 或 *False*。

         .. note:: 
            
            除上述提到的选项外，其他文本提取变体不受此设置的影响。特别是对于"xml"、"xhtml"和"html"选项，这些基于MuPDF代码的选项，会提取字体名称 `"Calibri-Light"` ，甚至只是 **字体家族** 名称——例如 `Calibri` 。

      .. method:: unset_quad_corrections(on=None)

         * 新增于v1.18.10

         启用/禁用PyMuPDF特有的代码，该代码在遇到 `Page.get_text` 文本提取中的无效字符四边形时，尝试重建有效的字符四边形。该代码依赖于某些字体属性（升高字形和下降字形），在某些罕见情况下，这些属性不存在，并且在尝试访问时会导致段错误。此方法设置PyMuPDF中的全局参数，抑制此代码的执行。

         :arg bool on: 如果省略或为 `None` ，则返回当前设置。对于其他值，将应用 *bool()* 函数来设置全局变量。如果为 `True` ，PyMuPDF将不尝试访问相应的字体属性，而是使用 `ascender=0.8` 和 `descender=-0.2` 作为默认值。

         :rtype: bool
         :returns: *True* 或 *False*。

      .. method:: store_shrink(percent)

         按当前大小的百分比减少存储缓存。

         :arg int percent: 要释放的当前大小的百分比。如果为100或更大，缓存将被清空；如果为零，则不做任何操作。MuPDF的缓存策略是“最近最少使用”，因此最少使用的元素将被优先删除。

         :rtype: int
         :returns: 新的当前存储大小。根据情况，减少的大小可能大于请求的百分比。

      .. method:: show_aa_level()

         * 新增于v1.16.14

         返回当前的抗锯齿值。这些值控制图形和文本元素的渲染质量。

         :rtype: dict
         :returns: 一个字典，初始内容为：`{'graphics': 8, 'text': 8, 'graphics_min_line_width': 0.0}`。

      .. method:: set_aa_level(level)

         * 新增于v1.16.14

         设置用于抗锯齿的位数。当前该值适用于图形和文本渲染。未来的MuPDF版本可能会有所不同。

         :arg int level: 一个介于0到8之间的整数。范围之外的值会被自动更改为有效值。该值将在当前会话期间或直到再次更改为止保持有效。

      .. method:: reset_mupdf_warnings()

         * 新增于v1.16.0

         清空MuPDF警告消息缓冲区。

      .. method:: mupdf_display_errors(value=None)

         控制是否应将MuPDF错误显示为|PyMuPDF|消息。

         :arg value:

            * 如果为 `None` ，当前设置保持不变。
            * 否则更改当前设置为 `bool(value)` ；如果为 *True* ，未来的MuPDF错误将作为消息显示。
            * 无论此设置如何，MuPDF错误总是会存储在警告存储区中。
            * 在导入 |PyMuPDF| 时，该值默认为 *True* 。

         :returns: 当前设置为 *True* 或 *False* 。

         * 新增于v1.16.8

      .. method:: mupdf_display_warnings(value=None)

         控制是否应将MuPDF警告显示为|PyMuPDF|消息。

         :arg value:

            * 如果为 `None` ，当前设置保持不变。
            * 否则更改当前设置为 `bool(value)` ； 如果为 *True* ，未来的MuPDF警告将作为消息显示。
            * 无论此设置如何，MuPDF警告总是会存储在警告存储区中。
            * 在导入 |PyMuPDF| 时，该值默认为 *True* 。

         :returns: 当前设置为 *True* 或 *False* 。

         * 新增于v1.16.8

      .. method:: mupdf_warnings(reset=True)

         * 新增于v1.16.0

         返回所有存储的MuPDF消息，格式为字符串并带有换行符。

         :arg bool reset: *(v1.16.7中新功能)* 是否自动清空存储区。

      .. attribute:: fitz_config

         一个字典，包含用于配置PyMuPDF和MuPDF的实际值。也请参考安装章节。以下是各个键的概述，每个键描述了某个支持方面的状态。

         ================= ===================================================
         **键**            **支持的内容**
         ================= ===================================================
         plotter-g         灰度颜色空间渲染
         plotter-rgb       RGB颜色空间渲染
         plotter-cmyk      CMYK颜色空间渲染
         plotter-n         覆印渲染
         pdf               PDF文档
         xps               XPS文档
         svg               SVG文档
         cbz               CBZ文档
         img               IMG文档
         html              HTML文档
         epub              EPUB文档
         jpx               JPEG2000图像
         js                JavaScript
         tofu              所有TOFU字体
         tofu-cjk          CJK字体子集（中国、日本、韩国）
         tofu-cjk-ext      CJK字体扩展
         tofu-cjk-lang     CJK字体语言扩展
         tofu-emoji        TOFU表情符号字体
         tofu-historic     TOFU历史字体
         tofu-symbol       TOFU符号字体
         tofu-sil          TOFU SIL字体
         icc               ICC色彩配置文件
         py-memory         使用Python内存管理 [#f4]_
         base14            基础14字体（应始终为True）
         ================= ===================================================

         关于“TOFU”一词的解释，请参阅 `此维基百科条目 <https://zh.wikipedia.org/wiki/Noto>`_ ::

            In [1]: import pymupdf
            In [2]: TOOLS.fitz_config
            Out[2]:
            {'plotter-g': True,
            'plotter-rgb': True,
            'plotter-cmyk': True,
            'plotter-n': True,
            'pdf': True,
            'xps': True,
            'svg': True,
            'cbz': True,
            'img': True,
            'html': True,
            'epub': True,
            'jpx': True,
            'js': True,
            'tofu': False,
            'tofu-cjk': True,
            'tofu-cjk-ext': False,
            'tofu-cjk-lang': False,
            'tofu-emoji': False,
            'tofu-historic': False,
            'tofu-symbol': False,
            'tofu-sil': False,
            'icc': True,
            'py-memory': False,
            'base14': True}

         :rtype: dict

      .. attribute:: store_maxsize

         存储缓存的最大大小，以字节为单位。 **PyMuPDF** 默认值为 268'435'456（256 MB），因此您应始终看到这个值。如果该值为零，则允许“无限制”增长。

         :rtype: int

      .. attribute:: store_size

         当前存储缓存的大小，以字节为单位。此值可能会随每次使用 **PyMuPDF** 函数而变化（通常会增加）。只有当超出 :attr:`Tools.store_maxsize` 时，这个值才会（自动）减少：在这种情况下， **MuPDF** 将逐出低使用率的对象，直到值重新回到正常范围。

         :rtype: int


.. tab:: 英文

   This class is a collection of utility methods and attributes, mainly around memory management. To simplify and speed up its use, it is automatically instantiated under the name *TOOLS* when PyMuPDF is imported.

   ====================================== =================================================
   **Method / Attribute**                 **Description**
   ====================================== =================================================
   :meth:`Tools.gen_id`                   generate a unique identifier
   :meth:`Tools.store_shrink`             shrink the storables cache [#f1]_
   :meth:`Tools.mupdf_warnings`           return the accumulated MuPDF warnings
   :meth:`Tools.mupdf_display_errors`     control whether MuPDF errors are displayed as messages.
   :meth:`Tools.mupdf_display_warnings`   control whether MuPDF warnings are displayed as messages.
   :meth:`Tools.reset_mupdf_warnings`     empty MuPDF warnings/errors message buffer.
   :meth:`Tools.set_aa_level`             set the anti-aliasing values
   :meth:`Tools.set_annot_stem`           set the prefix of new annotation / link ids
   :meth:`Tools.set_small_glyph_heights`  search and extract using small bbox heights
   :meth:`Tools.set_subset_fontnames`     control suppression of subset fontname tags
   :meth:`Tools.show_aa_level`            return the anti-aliasing values
   :meth:`Tools.unset_quad_corrections`   disable PyMuPDF-specific code
   :attr:`Tools.fitz_config`              configuration settings of PyMuPDF
   :attr:`Tools.store_maxsize`            maximum storables cache size
   :attr:`Tools.store_size`               current storables cache size
   ====================================== =================================================

   **Class API**

   .. class:: Tools
      :no-index:

      .. method:: gen_id()
         :no-index:

         A convenience method returning a unique positive integer which will increase by 1 on every invocation. Example usages include creating unique keys in databases - its creation should be faster than using timestamps by an order of magnitude.

         .. note:: MuPDF has dropped support for this in v1.14.0, so we have re-implemented a similar function with the following differences:

               * It is not part of MuPDF's global context and not threadsafe (not an issue because we do not support threads in PyMuPDF anyway).
               * It is implemented as *int*. This means that the maximum number is *sys.maxsize*. Should this number ever be exceeded, the counter starts over again at 1.

         :rtype: int
         :returns: a unique positive integer.


      .. method:: set_annot_stem(stem=None)
         :no-index:

         * New in v1.18.6

         Set or inquire the prefix for the id of new annotations, fields or links.

         :arg str stem: if omitted, the current value is returned, default is "fitz". Annotations, fields / widgets and links technically are subtypes of the same type of object (`/Annot`) in PDF documents. An `/Annot` object may be given a unique identifier within a page. For each of the applicable subtypes, PyMuPDF generates identifiers "stem-Annn", "stem-Wnnn" or "stem-Lnnn" respectively. The number "nnn" is used to enforce the required uniqueness.

         :rtype: str
         :returns: the current value.


      .. method:: set_small_glyph_heights(on=None)
         :no-index:

         * New in v1.18.5

         Set or inquire reduced bbox heights in text extract and text search methods.

         :arg bool on: if omitted or `None`, the current setting is returned. For other values the *bool()* function is applied to set a global variable. If `True`, :meth:`Page.search_for` and :meth:`Page.get_text` methods return character, span, line or block bboxes that have a height of *font size*. If `False` (standard setting when PyMuPDF is imported), bbox height will be based on font properties and normally equal *line height*.

         :rtype: bool
         :returns: *True* or *False*.

         .. note:: Text extraction options "xml", "xhtml" and "html", which directly wrap MuPDF code, are not influenced by this.

      .. method:: set_subset_fontnames(on=None)
         :no-index:

         * New in v1.18.9

         Control suppression of subset fontname tags in text extractions.

         :arg bool on: if omitted / `None`, the current setting is returned. Arguments evaluating to `True` or `False` set a global variable. If `True`, options "dict", "json", "rawdict" and "rawjson" will return e.g. `"NOHSJV+Calibri-Light"`, otherwise only `"Calibri-Light"` (the default). The setting remains in effect until changed again.

         :rtype: bool
         :returns: *True* or *False*.

         .. note:: Except mentioned above, no other text extraction variants are influenced by this. This is especially true for the options "xml", "xhtml" and "html", which are based on MuPDF code. They extract the font name `"Calibri-Light"`, or even just the **family** name -- `Calibri` in this example.


      .. method:: unset_quad_corrections(on=None)
         :no-index:

         * New in v1.18.10

         Enable / disable PyMuPDF-specific code, that tries to rebuild valid character quads when encountering nonsense in :meth:`Page.get_text` text extractions. This code depends on certain font properties (ascender and descender), which do not exist in rare situations and cause segmentation faults when trying to access them. This method sets a global parameter in PyMuPDF, which suppresses execution of this code.

         :arg bool on: if omitted or `None`, the current setting is returned. For other values the *bool()* function is applied to set a global variable. If `True`, PyMuPDF will not try to access the resp. font properties and use values `ascender=0.8` and `descender=-0.2` instead.

         :rtype: bool
         :returns: *True* or *False*.


      .. method:: store_shrink(percent)
         :no-index:

         Reduce the storables cache by a percentage of its current size.

         :arg int percent: the percentage of current size to free. If 100+ the store will be emptied, if zero, nothing will happen. MuPDF's caching strategy is "least recently used", so low-usage elements get deleted first.

         :rtype: int
         :returns: the new current store size. Depending on the situation, the size reduction may be larger than the requested percentage.

      .. method:: show_aa_level()
         :no-index:

         * New in version 1.16.14
         
         Return the current anti-aliasing values. These values control the rendering quality of graphics and text elements.

         :rtype: dict
         :returns: A dictionary with the following initial content: `{'graphics': 8, 'text': 8, 'graphics_min_line_width': 0.0}`.


      .. method:: set_aa_level(level)
         :no-index:

         * New in version 1.16.14
         
         Set the new number of bits to use for anti-aliasing. The same value is taken currently for graphics and text rendering. This might change in a future MuPDF release.

         :arg int level: an integer ranging between 0 and 8. Value outside this range will be silently changed to valid values. The value will remain in effect throughout the current session or until changed again.


      .. method:: reset_mupdf_warnings()
         :no-index:

         * New in version 1.16.0

         Empty MuPDF warnings message buffer.


      .. method:: mupdf_display_errors(value=None)
         :no-index:

         Control whether MuPDF errors should be displayed as |PyMuPDF| messages.

         :arg value:
         * If `None`, the current setting is left unchanged.
         * Otherwise changes the current setting to `bool(value)`;
         if *True*, future MuPDF errors will be shown as :ref:`Messages`.
         * Regardless of this setting, MuPDF errors will always be stored in the warnings store.
         * Upon import of |PyMuPDF| this value is *True*.

         :returns: The current setting as *True* or *False*.

         * New in version 1.16.8


      .. method:: mupdf_display_warnings(value=None)
         :no-index:

         Control whether MuPDF warnings should be displayed as |PyMuPDF| messages.

         :arg value:
         * If `None`, the current setting is left unchanged.
         * Otherwise changes the current setting to `bool(value)`;
         if *True*, future MuPDF warnings will be shown as :ref:`Messages`.
         * Regardless of this setting, MuPDF warnings will always be stored in the warnings store.
         * Upon import of |PyMuPDF| this value is *True*.

         :returns: The current setting as *True* or *False*.

         * New in version 1.16.8


      .. method:: mupdf_warnings(reset=True)
         :no-index:

         * New in version 1.16.0

         Return all stored MuPDF messages as a string with interspersed line-breaks.

         :arg bool reset: *(new in version 1.16.7)* whether to automatically empty the store.


      .. attribute:: fitz_config
         :no-index:

         A dictionary containing the actual values used for configuring PyMuPDF and MuPDF. Also refer to the installation chapter. This is an overview of the keys, each of which describes the status of a support aspect.

         ================= ===================================================
         **Key**           **Support included for ...**
         ================= ===================================================
         plotter-g         Gray colorspace rendering
         plotter-rgb       RGB colorspace rendering
         plotter-cmyk      CMYK colorspcae rendering
         plotter-n         overprint rendering
         pdf               PDF documents
         xps               XPS documents
         svg               SVG documents
         cbz               CBZ documents
         img               IMG documents
         html              HTML documents
         epub              EPUB documents
         jpx               JPEG2000 images
         js                JavaScript
         tofu              all TOFU fonts
         tofu-cjk          CJK font subset (China, Japan, Korea)
         tofu-cjk-ext      CJK font extensions
         tofu-cjk-lang     CJK font language extensions
         tofu-emoji        TOFU emoji fonts
         tofu-historic     TOFU historic fonts
         tofu-symbol       TOFU symbol fonts
         tofu-sil          TOFU SIL fonts
         icc               ICC profiles
         py-memory         using Python memory management [#f2]_
         base14            Base-14 fonts (should always be true)
         ================= ===================================================

         For an explanation of the term "TOFU" see `this Wikipedia article <https://en.wikipedia.org/wiki/Noto_fonts>`_::

         In [1]: import pymupdf
         In [2]: TOOLS.fitz_config
         Out[2]:
         {'plotter-g': True,
         'plotter-rgb': True,
         'plotter-cmyk': True,
         'plotter-n': True,
         'pdf': True,
         'xps': True,
         'svg': True,
         'cbz': True,
         'img': True,
         'html': True,
         'epub': True,
         'jpx': True,
         'js': True,
         'tofu': False,
         'tofu-cjk': True,
         'tofu-cjk-ext': False,
         'tofu-cjk-lang': False,
         'tofu-emoji': False,
         'tofu-historic': False,
         'tofu-symbol': False,
         'tofu-sil': False,
         'icc': True,
         'py-memory': False,
         'base14': True}

         :rtype: dict

      .. attribute:: store_maxsize
         :no-index:

         Maximum storables cache size in bytes. **PyMuPDF** is generated with a value of 268'435'456 (256 MB, the default value), which you should therefore always see here. If this value is zero, then an "unlimited" growth is permitted.

         :rtype: int

      .. attribute:: store_size
         :no-index:

         Current storables cache size in bytes. This value may change (and will usually increase) with every use of a **PyMuPDF** function. It will (automatically) decrease only when :attr:`Tools.store_maxsize` is going to be exceeded: in this case, **MuPDF** will evict low-usage objects until the value is again in range.

         :rtype: int

Example Session
----------------

Example Session

.. tab:: 中文

   .. code-block:: python

      >>> import pymupdf
      # 打印最大和当前缓存大小
      >>> pymupdf.TOOLS.store_maxsize
      268435456
      >>> pymupdf.TOOLS.store_size
      0
      >>> doc = pymupdf.open("demo1.pdf")
      # 创建pixmap会将许多对象放入缓存（文本、图像、字体），
      # 除了pixmap本身
      >>> pix = doc[0].get_pixmap(alpha=False)
      >>> pymupdf.TOOLS.store_size
      454519
      # 释放（至少）50%的存储空间
      >>> pymupdf.TOOLS.store_shrink(50)
      13471
      >>> pymupdf.TOOLS.store_size
      13471
      # 获取几个唯一的编号
      >>> pymupdf.TOOLS.gen_id()
      1
      >>> pymupdf.TOOLS.gen_id()
      2
      >>> pymupdf.TOOLS.gen_id()
      3
      # 关闭文档并查看仍然使用的缓存量
      >>> doc.close()
      >>> pymupdf.TOOLS.store_size
      0


.. tab:: 英文

   .. code-block:: python

      >>> import pymupdf
      # print the maximum and current cache sizes
      >>> pymupdf.TOOLS.store_maxsize
      268435456
      >>> pymupdf.TOOLS.store_size
      0
      >>> doc = pymupdf.open("demo1.pdf")
      # pixmap creation puts lots of object in cache (text, images, fonts),
      # apart from the pixmap itself
      >>> pix = doc[0].get_pixmap(alpha=False)
      >>> pymupdf.TOOLS.store_size
      454519
      # release (at least) 50% of the storage
      >>> pymupdf.TOOLS.store_shrink(50)
      13471
      >>> pymupdf.TOOLS.store_size
      13471
      # get a few unique numbers
      >>> pymupdf.TOOLS.gen_id()
      1
      >>> pymupdf.TOOLS.gen_id()
      2
      >>> pymupdf.TOOLS.gen_id()
      3
      # close document and see how much cache is still in use
      >>> doc.close()
      >>> pymupdf.TOOLS.store_size
      0
      >>>


.. rubric:: Footnotes

.. [#f1] This memory area is internally used by MuPDF, and it serves as a cache for objects that have already been read and interpreted, thus improving performance. The most bulky object types are images and also fonts. When an application starts up the MuPDF library (in our case this happens as part of *import pymupdf*), it must specify a maximum size for this area. PyMuPDF's uses the default value (256 MB) to limit memory consumption. Use the methods here to control or investigate store usage. For example: even after a document has been closed and all related objects have been deleted, the store usage may still not drop down to zero. So you might want to enforce that before opening another document.

.. [#f2] By default PyMuPDF and MuPDF use `malloc()`/`free()` for dynamic memory management. One can instead force them to use the Python allocation functions `PyMem_New()`/`PyMem_Del()`, by modifying *fitz/fitz.i* to do `#define JM_MEMORY 1` and rebuilding PyMuPDF.

.. [#f3] 该内存区域是MuPDF内部使用的，用作已经读取和解释的对象的缓存，从而提高性能。最大的对象类型是图像和字体。当应用程序启动MuPDF库时（在我们的案例中，这发生在 *import pymupdf* 过程中），它必须指定该区域的最大大小。PyMuPDF使用默认值（256 MB）来限制内存消耗。使用这里的方法可以控制或调查存储使用情况。例如：即使文档已经关闭并且所有相关对象都已删除，存储使用情况仍然可能不会降到零。因此，您可能希望在打开另一个文档之前强制执行此操作。

.. [#f4] 默认情况下，PyMuPDF和MuPDF使用 `malloc()`/`free()` 进行动态内存管理。可以通过修改 *fitz/fitz.i* 文件，将 `#define JM_MEMORY 1` 并重新构建PyMuPDF，强制它们使用Python的分配函数 `PyMem_New()`/`PyMem_Del()`。


.. include:: footer.rst
