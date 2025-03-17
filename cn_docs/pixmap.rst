.. include:: header.rst

.. _Pixmap:

================
Pixmap
================

.. tab:: 中文

   Pixmaps（"像素图"）是 MuPDF 渲染功能的核心对象。它们表示一组矩形像素。每个像素由一个字节数（"组件"）来定义其颜色，以及一个可选的 alpha 字节来定义其透明度。

   在 PyMuPDF 中，有几种创建 pixmap 的方式。除第一种外，其他方法都可以作为重载构造函数来使用。Pixmap 可以通过以下方式创建：

   1. 从文档页面（方法：:meth:`Page.get_pixmap`）
   2. 空的，基于 :ref:`Colorspace` 和 :ref:`IRect` 信息
   3. 从文件
   4. 从内存图像
   5. 从纯像素的内存区域
   6. 从 PDF 文档中的图像
   7. 作为另一个 pixmap 的副本

   .. note:: 

      上述第 3 和第 4 点支持多种图像格式作为输入。有关更多信息，请参见 :ref:`ImageFiles` 部分。

   请查看 :ref:`FAQ` 部分，了解一些 pixmap 使用的示例。

   ================================ ===================================================
   **方法 / 属性**               **简要描述**
   ================================ ===================================================
   :meth:`Pixmap.clear_with`        清除 pixmap 的部分区域
   :meth:`Pixmap.color_count`       确定使用的颜色数
   :meth:`Pixmap.color_topusage`    确定最常用颜色的比例
   :meth:`Pixmap.copy`              复制另一个 pixmap 的部分内容
   :meth:`Pixmap.gamma_with`        对 pixmap 应用 gamma 因子
   :meth:`Pixmap.invert_irect`      反转给定区域的像素
   :meth:`Pixmap.pdfocr_save`       将 pixmap 保存为 OCR 处理过的 1 页 PDF
   :meth:`Pixmap.pdfocr_tobytes`    将 pixmap 保存为 OCR 处理过的 1 页 PDF
   :meth:`Pixmap.pil_image`         创建 Pillow 图像
   :meth:`Pixmap.pil_save`          保存为 Pillow 图像
   :meth:`Pixmap.pil_tobytes`       将 pixmap 写入 `bytes` 作为 Pillow 图像
   :meth:`Pixmap.pixel`             返回像素的值
   :meth:`Pixmap.save`              以多种格式保存 pixmap
   :meth:`Pixmap.set_alpha`         设置 alpha 值
   :meth:`Pixmap.set_dpi`           设置图像分辨率
   :meth:`Pixmap.set_origin`        设置 pixmap 的 x, y 值
   :meth:`Pixmap.set_pixel`         设置像素的颜色和 alpha 值
   :meth:`Pixmap.set_rect`          设置矩形区域内所有像素的颜色和 alpha 值
   :meth:`Pixmap.shrink`            缩小大小，保持比例
   :meth:`Pixmap.tint_with`         给 pixmap 上色
   :meth:`Pixmap.tobytes`           以多种格式返回内存区域
   :meth:`Pixmap.warp`              从矩形区域创建 pixmap
   :attr:`Pixmap.alpha`             透明度指示器
   :attr:`Pixmap.colorspace`        pixmap 的 :ref:`Colorspace`
   :attr:`Pixmap.digest`            pixmap 的 MD5 哈希值
   :attr:`Pixmap.height`            pixmap 高度
   :attr:`Pixmap.interpolate`       插值方法指示器
   :attr:`Pixmap.is_monochrome`     检查是否只包含黑白色
   :attr:`Pixmap.is_unicolor`       检查是否只包含一种颜色
   :attr:`Pixmap.irect`             pixmap 的 :ref:`IRect`
   :attr:`Pixmap.n`                 每个像素的字节数
   :attr:`Pixmap.samples_mv`        像素区域的 `memoryview`
   :attr:`Pixmap.samples_ptr`       指向像素区域的 Python 指针
   :attr:`Pixmap.samples`           像素区域的 `bytes` 副本
   :attr:`Pixmap.size`              pixmap 的总长度
   :attr:`Pixmap.stride`            每行图像的大小
   :attr:`Pixmap.width`             pixmap 宽度
   :attr:`Pixmap.x`                 左上角的 X 坐标
   :attr:`Pixmap.xres`              X 方向的分辨率
   :attr:`Pixmap.y`                 左上角的 Y 坐标
   :attr:`Pixmap.yres`              Y 方向的分辨率
   ================================ ===================================================


.. tab:: 英文

   Pixmaps ("pixel maps") are objects at the heart of MuPDF's rendering capabilities. They represent plane rectangular sets of pixels. Each pixel is described by a number of bytes ("components") defining its color, plus an optional alpha byte defining its transparency.

   In PyMuPDF, there exist several ways to create a pixmap. Except the first one, all of them are available as overloaded constructors. A pixmap can be created ...

   1. from a document page (method :meth:`Page.get_pixmap`)
   2. empty, based on :ref:`Colorspace` and :ref:`IRect` information
   3. from a file
   4. from an in-memory image
   5. from a memory area of plain pixels
   6. from an image inside a PDF document
   7. as a copy of another pixmap

   .. note:: 
      
      A number of image formats is supported as input for points 3. and 4. above. See section :ref:`ImageFiles`.

   Have a look at the :ref:`FAQ` section to see some pixmap usage "at work".

   ================================ ===================================================
   **Method / Attribute**           **Short Description**
   ================================ ===================================================
   :meth:`Pixmap.clear_with`        clear parts of the pixmap
   :meth:`Pixmap.color_count`       determine used colors
   :meth:`Pixmap.color_topusage`    determine share of most used color
   :meth:`Pixmap.copy`              copy parts of another pixmap
   :meth:`Pixmap.gamma_with`        apply a gamma factor to the pixmap
   :meth:`Pixmap.invert_irect`      invert the pixels of a given area
   :meth:`Pixmap.pdfocr_save`       save the pixmap as an OCRed 1-page PDF
   :meth:`Pixmap.pdfocr_tobytes`    save the pixmap as an OCRed 1-page PDF
   :meth:`Pixmap.pil_image`         create a Pillow Image
   :meth:`Pixmap.pil_save`          save as a Pillow Image
   :meth:`Pixmap.pil_tobytes`       write to `bytes` as a Pillow Image
   :meth:`Pixmap.pixel`             return the value of a pixel
   :meth:`Pixmap.save`              save the pixmap in a variety of formats
   :meth:`Pixmap.set_alpha`         set alpha values
   :meth:`Pixmap.set_dpi`           set the image resolution
   :meth:`Pixmap.set_origin`        set pixmap x,y values
   :meth:`Pixmap.set_pixel`         set color and alpha of a pixel
   :meth:`Pixmap.set_rect`          set color and alpha of all pixels in a rectangle
   :meth:`Pixmap.shrink`            reduce size keeping proportions
   :meth:`Pixmap.tint_with`         tint the pixmap
   :meth:`Pixmap.tobytes`           return a memory area in a variety of formats
   :meth:`Pixmap.warp`              return a pixmap made from a quad inside
   :attr:`Pixmap.alpha`             transparency indicator
   :attr:`Pixmap.colorspace`        pixmap's :ref:`Colorspace`
   :attr:`Pixmap.digest`            MD5 hashcode of the pixmap
   :attr:`Pixmap.height`            pixmap height
   :attr:`Pixmap.interpolate`       interpolation method indicator
   :attr:`Pixmap.is_monochrome`     check if only black and white occur
   :attr:`Pixmap.is_unicolor`       check if only one color occurs
   :attr:`Pixmap.irect`             :ref:`IRect` of the pixmap
   :attr:`Pixmap.n`                 bytes per pixel
   :attr:`Pixmap.samples_mv`        `memoryview` of pixel area
   :attr:`Pixmap.samples_ptr`       Python pointer to pixel area
   :attr:`Pixmap.samples`           `bytes` copy of pixel area
   :attr:`Pixmap.size`              pixmap's total length
   :attr:`Pixmap.stride`            size of one image row
   :attr:`Pixmap.width`             pixmap width
   :attr:`Pixmap.x`                 X-coordinate of top-left corner
   :attr:`Pixmap.xres`              resolution in X-direction
   :attr:`Pixmap.y`                 Y-coordinate of top-left corner
   :attr:`Pixmap.yres`              resolution in Y-direction
   ================================ ===================================================

**Class API**

.. tab:: 中文

   .. class:: Pixmap

      .. method:: __init__(self, colorspace, irect, alpha=False)

         **新建空 Pixmap：** 创建一个大小和原点由矩形指定的空 Pixmap。因此，*irect.top_left* 表示 Pixmap 的左上角，其宽度和高度分别为 *irect.width* 和 *irect.height*。请注意，图像区域 **未初始化**，将包含无意义的数据 —— 使用例如 :meth:`clear_with` 或 :meth:`set_rect` 来确保图像内容有效。

         :arg colorspace: 颜色空间。
         :type colorspace: :ref:`Colorspace`

         :arg irect_like irect: Pixmap 的位置和尺寸。

         :arg bool alpha: 指定是否包含透明度字节。默认值为 *False*。

      .. method:: __init__(self, colorspace, source)

         **复制并设置颜色空间：** 复制 *source* Pixmap，并转换其颜色空间。任何颜色空间组合都是可以的，但源颜色空间不能是 *None*。

         :arg colorspace: 目标颜色空间。**也可以是** *None*。在这种情况下，将创建一个“掩膜” Pixmap：其 :attr:`Pixmap.samples` 将仅包含源的 alpha 字节。
         :type colorspace: :ref:`Colorspace`

         :arg source: 源 Pixmap。
         :type source: *Pixmap*

      .. method:: __init__(self, source, mask)

         * 从 v1.18.18 新增

         **复制并添加图像掩膜：** 复制 *source* Pixmap，添加一个 alpha 通道，使用来自掩膜 Pixmap 的透明度数据。

         :arg source: 没有 alpha 通道的 Pixmap。
         :type source: :ref:`Pixmap`

         :arg mask: 掩膜 Pixmap。必须是灰度 Pixmap。
         :type mask: :ref:`Pixmap`

      .. method:: __init__(self, source, width, height, [clip])

         **复制并缩放：** 复制 *source* Pixmap，缩放到新的宽度和高度 —— 图像将相应地拉伸或缩小。支持部分复制。源颜色空间可以是 *None*。

         :arg source: 源 Pixmap。
         :type source: *Pixmap*

         :arg float width: 目标宽度。

         :arg float height: 目标高度。

         :arg irect_like clip: 限制结果 Pixmap 为此区域的 **缩放** 版本。

         .. note:: 如果宽度或高度不代表整数（即 `value.is_integer() != True`），则结果 Pixmap **将具有 alpha 通道**。

      .. method:: __init__(self, source, alpha=1)

         **复制并添加或去除 alpha 通道：** 复制 *source* 并添加或去除其 alpha 通道。如果 *alpha* 等于 *source.alpha*，则为相同的复制。如果添加了 alpha 通道，其值将设置为 255。

         :arg source: 源 Pixmap。
         :type source: *Pixmap*

         :arg bool alpha: 目标是否具有 alpha 通道，如果源颜色空间是 *None*，则默认并且是强制的。

         .. note:: 一个典型的用法是将颜色和透明度字节分离到不同的 Pixmap 中。某些应用程序需要这样做，例如 *wxPython* 中的 *wx.Bitmap.FromBufferAndAlpha()*：

            >>> # 'pix' 是 RGBA Pixmap
            >>> pixcolors = pymupdf.Pixmap(pix, 0)    # 提取 RGB 部分（去除 alpha）
            >>> pixalpha = pymupdf.Pixmap(None, pix)  # 提取 alpha 部分
            >>> bm = wx.Bitmap.FromBufferAndAlpha(pix.width, pix.height, pixcolors.samples, pixalpha.samples)

      .. method:: __init__(self, filename)

         **来自文件：** 从 *filename* 创建一个 Pixmap。所有属性都从输入中推断出来。生成的 Pixmap 的原点是 *(0, 0)*。

         :arg str filename: 图像文件的路径。

      .. method:: __init__(self, stream)

         **来自内存：** 从内存区域创建一个 Pixmap。所有属性都从输入中推断出来。生成的 Pixmap 的原点是 *(0, 0)*。

         :arg bytes, bytearray, BytesIO stream: 包含完整有效图像的内存数据。例如，可以使用 *stream = bytearray(open('image.file', 'rb').read())* 创建。*bytes* 类型仅在 **Python 3** 中支持，因为在 Python 2 中 *bytes == str*，该方法将把流当作文件名处理。

            *版本 1.14.13 更改：* 现在也支持 *io.BytesIO*。

      .. method:: __init__(self, colorspace, width, height, samples, alpha)

         **来自原始像素：** 从 *samples* 创建一个 Pixmap。每个像素必须由一定数量的字节表示，具体由 *colorspace* 和 *alpha* 参数控制。生成的 Pixmap 的原点是 *(0, 0)*。此方法适用于当其他程序提供原始图像数据时 —— 参见 :ref:`FAQ`。

         :arg colorspace: 图像的颜色空间。
         :type colorspace: :ref:`Colorspace`

         :arg int width: 图像宽度

         :arg int height: 图像高度

         :arg bytes, bytearray, BytesIO samples: 包含图像所有像素的区域。如果指定了 alpha，则必须包含 alpha 值。

            *版本 1.14.13 更改：* (1) 现在也可以使用 *io.BytesIO*。 (2) 数据现在 **被复制** 到 Pixmap，因此可以安全地删除或使其不可用。

         :arg bool alpha: 是否包含透明度通道。

         .. note::

            1. 必须满足以下公式：*(colorspace.n + alpha) * width * height == len(samples)*。
            2. 从版本 1.14.13 开始，样本数据会被 **复制** 到 Pixmap。

      .. method:: __init__(self, doc, xref)

         **来自 PDF 图像：** 从 PDF *doc* 中的图像创建一个 Pixmap，该图像由其 :data:`xref` 标识。所有 Pixmap 属性由图像设置。查看 `extract-from-pages.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/extract-images/extract-from-pages.py>`_ 和 `extract-from-xref.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/extract-images/extract-from-xref.py>`_ ，了解如何使用它来恢复 PDF 中的所有图像。

         :arg doc: 已打开的 **PDF** 文档。
         :type doc: :ref:`Document`

         :arg int xref: 图像对象的 :data:`xref`。例如，可以使用 :meth:`Document.get_page_images` 创建某一页面上所有图像的列表，该方法还会显示每个图像的 :data:`xref` 数字。

      .. method:: clear_with([value [, irect]])

         初始化样本区域。

         :arg int value: 如果指定，值范围为 0 到 255。每个像素的每个颜色字节将被设置为此值，如果存在 alpha，则设置为 255（非透明）。如果未指定，则所有字节（包括任何 alpha）将清除为 *0x00*。

         :arg irect_like irect: 要清除的区域。如果省略，则清除整个 Pixmap。只有在指定了 *value* 时，才可以指定此区域。

      .. method:: tint_with(black, white)

         通过将黑色和/或白色替换为给定的颜色来为 Pixmap 上色，颜色以 **sRGB 整数** 值表示。仅支持颜色空间 :data:`CS_GRAY` 和 :data:`CS_RGB`，其他颜色空间会发出警告并被忽略。

         如果颜色空间是 :data:`CS_GRAY`，将取平均值 *(red + green + blue)/3*。Pixmap 将就地更改。

         :arg int black: 将黑色替换为此值。指定 0x000000 不会进行更改。
         :arg int white: 将白色替换为此值。指定 0xFFFFFF 不会进行更改。

         示例：

            * `tint_with(0x000000, 0xFFFFFF)` 不做任何操作。
            * `tint_with(0x00FF00, 0xFFFFFF)` 将黑色更改为绿色，保持白色不变。
            * `tint_with(0xFF0000, 0x0000FF)` 将黑色更改为红色，白色更改为蓝色。

      .. method:: gamma_with(gamma)

         对 Pixmap 应用伽马因子，即调亮或调暗图像。颜色空间为 *None* 的 Pixmap 会发出警告并被忽略。

         :arg float gamma: *gamma = 1.0* 不做任何改变，*gamma < 1.0* 使图像变亮，*gamma > 1.0* 使图像变暗。

      .. method:: shrink(n)

         通过将宽度和高度都除以 2\ :sup:``n`` 来缩小 Pixmap。

         :arg int n: 确定新的 Pixmap （样本）大小。例如，值为 2 时，将宽度和高度除以 4，结果大小为原始图像的 16\ :sup:`th`。值小于 1 会发出警告并被忽略。

         .. note:: 使用此方法可以在保留比例的同时减小 Pixmap 的大小。Pixmap 会 "就地" 更改。如果要保留原始图像并具有更细粒度的选择，请使用上述相应的构造函数。

      .. method:: pixel(x, y)

         *版本 1.14.5 新增：* 返回位置 (x, y)（列，行）处像素的值。

         :arg int x: 像素的列号，必须在 `range(pix.width)` 内。
         :arg int y: 像素的行号，必须在 `range(pix.height)` 内。

         :rtype: list
         :returns: 包含颜色值的列表，可能还有 alpha 值。列表的长度和内容取决于 Pixmap 的颜色空间和是否存在 alpha。对于 RGBA Pixmap，返回的结果例如是 *[r, g, b, a]*。所有项都是 `range(256)` 范围内的整数。

      .. method:: set_pixel(x, y, color)

         *版本 1.14.7 新增：* 操作位置 (x, y)（列，行）处的像素。

         :arg int x: 像素的列号，必须在 `range(pix.width)` 内。
         :arg int y: 像素的行号，必须在 `range(pix.height)` 内。
         :arg sequence color: 期望的像素值，作为一个整数序列，范围为 `range(256)`。序列的长度必须与 :attr:`Pixmap.n` 相等，包括任何 alpha 字节。

      .. method:: set_rect(irect, color)

         *版本 1.14.8 新增：* 将矩形区域的像素设置为某个值。

         :arg irect_like irect: 要填充的矩形区域。实际区域是此参数与 :attr:`Pixmap.irect` 的交集。如果交集为空（或参数无效），则不会进行更改。
         :arg sequence color: 期望的值，作为一个整数序列，范围为 `range(256)`。序列的长度必须与 :attr:`Pixmap.n` 相等，包括任何 alpha 字节。

         :rtype: bool
         :returns: 如果矩形无效或与 :attr:`Pixmap.irect` 的交集为空，则返回 *False*，否则返回 *True*。

         .. note::

            1. 此方法等价于对矩形中的每个像素执行 :meth:`Pixmap.set_pixel`，但如果涉及的像素很多，显然 **要快得多**。
            2. 此方法可以像 :meth:`Pixmap.clear_with` 一样使用，用于使用某个颜色初始化 Pixmap，例如： *pix.set_rect(pix.irect, (255, 255, 0))* （RGB 示例，将整个 Pixmap 填充为黄色）。

      .. method:: set_origin(x, y)

         *版本 1.17.7 新增：*

         设置 Pixmap 的左上角点的 x 和 y 坐标。

         :arg int x: x 坐标
         :arg int y: y 坐标


      .. method:: set_dpi(xres, yres)

         *版本 1.16.17 新增：*

         *版本 1.18.0 更改：* 保存为 PNG 图像时，这些值现在会被存储。

         设置 x 和 y 方向的分辨率（dpi）。

         :arg int xres: x 方向的分辨率。
         :arg int yres: y 方向的分辨率。


      .. method:: set_alpha(alphavalues, premultiply=1, opaque=None)

         *版本 1.18.13 更改：*

         更改 alpha 值。Pixmap 必须具有 alpha 通道。

         :arg bytes,bytearray,BytesIO alphavalues: 新的 alpha 值。如果提供，则其长度必须至少为 *width \* height* 。如果省略（`None`），则所有 alpha 值设置为 255（无透明度）。*版本 1.14.13 更改：* 现在也接受 *io.BytesIO*。
         :arg bool premultiply: *版本 1.18.13 新增：* 是否将颜色分量与 alpha 值预乘。
         :arg list,tuple opaque: 忽略 alpha 值并将此颜色设置为完全透明。一个包含整数的序列，范围为 `range(256)`，长度为 :attr:`Pixmap.n`。默认值为 *None*。例如，RGB 的典型选择是 `opaque=(255, 255, 255)` （白色）。


      .. method:: invert_irect([irect])

         反转 :ref:`IRect` *irect* 中所有像素的颜色。如果颜色空间是 *None*，则没有效果。

         :arg irect_like irect: 要反转的区域。省略时，反转整个图像。

      .. method:: copy(source, irect)

         将 *source* Pixmap 的 *irect* 部分复制到此 Pixmap 的相应区域。两个 Pixmap 可以有不同的尺寸，并且每个 Pixmap 都可以具有 :data:`CS_GRAY` 或 :data:`CS_RGB` 颜色空间，但它们当前 **必须** 具有相同的 alpha 属性 [#f4]_。复制机制会自动调整源和目标之间的差异，具体如下：

         - 如果从 :data:`CS_GRAY` 复制到 :data:`CS_RGB`，则源的灰度值将填充到每个 RGB 组件字节中。
         - 如果反向复制，则 *(r + g + b) / 3* 将作为目标的灰度值。

         在 *irect* 和目标 Pixmap 的矩形之间，首先会计算一个“交集”。这会考虑矩形坐标和当前的属性值 :attr:`Pixmap.x` 和 :attr:`Pixmap.y` （你可以通过 :meth:`Pixmap.set_origin` 修改这些值）。然后，复制该交集的对应数据。如果交集为空，则不会发生任何操作。

         :arg source: 源 Pixmap。
         :type source: :ref:`Pixmap`

         :arg irect_like irect: 要复制的区域。

         .. note:: 示例：假设你有两个 Pixmap，`pix1` 和 `pix2`，并且你想将 `pix2` 的右下角四分之一复制到 `pix1`，使得它从 `pix1` 的左上角开始。使用以下代码：

            >>> # 防护：将 pix1 和 pix2 的左上角设置为 (0, 0)
            >>> pix1.set_origin(0, 0)
            >>> pix2.set_origin(0, 0)
            >>> # 计算 pix2 区域的左上角坐标
            >>> x1 = int(pix2.width / 2)
            >>> y1 = int(pix2.height / 2)
            >>> # 将 pix2 的左上角移动到 (0, 0)，
            >>> # 使得待复制区域从这里开始：
            >>> pix2.set_origin(-x1, -y1)
            >>> # 现在复制...
            >>> pix1.copy(pix2, (0, 0, x1, y1))

            .. image:: images/img-pixmapcopy.*
                  :scale: 20

      .. method:: save(filename, output=None, jpg_quality=95)

         *版本 1.22.0 更改：* 新增 **直接支持 JPEG** 图像。图像质量可以通过参数 "jpg_quality" 控制。

         将 Pixmap 保存为图像文件。根据选择的输出，可能仅支持某些或所有颜色空间，并且可以选择不同的文件扩展名。请参见下表。

         :arg str,Path,file filename: 要保存的文件。可以作为字符串、``pathlib.Path`` 或 Python 文件对象提供。在后两种情况下，文件名将从相应的对象中获取。文件名的扩展名决定了图像格式，可以通过 output 参数覆盖。

         :arg str output: 所需的图像格式。默认值为文件名的扩展名。如果这两个值和文件扩展名不受支持，将引发异常。有关可能的值，请参见 :ref:`PixmapOutput`。
         :arg int jpg_quality: 所需的图像质量，默认值为 95。仅适用于 JPEG 图像，其他格式将忽略此值。此参数在质量与文件大小之间进行权衡。值为 98 时接近无损。较高的值不应提高质量。

         :raises ValueError: 对于不支持的图像格式。

      .. method:: tobytes(output="png", jpg_quality=95)

         *版本 1.14.5 新增：* 将 Pixmap 返回为指定格式的 *bytes* 内存对象 -- 类似于 :meth:`save`。

         *版本 1.22.0 更改：* 新增 **直接支持 JPEG**。图像质量可以通过新参数 "jpg_quality" 影响。

         :arg str output: 所需的图像格式。默认值为 "png"。有关其他可能的值，请参见 :ref:`PixmapOutput`。
         :arg int jpg_quality: 所需的图像质量，默认值为 95。仅适用于 JPEG 图像，其他格式将忽略此值。此参数在质量与文件大小之间进行权衡。值为 98 时接近无损。较高的值不应提高质量。

         :raises ValueError: 对于不支持的图像格式。
         :rtype: bytes

         :arg str output: 请求的图像格式。默认值为 "png"。有关其他可能的值，请参见 :ref:`PixmapOutput`。

      .. method:: pdfocr_save(filename, compress=True, language="eng", tessdata=None)

         *版本 1.19.0 新增：*

         *版本 1.22.5 更改：* 支持新的 Tesseract tessdata 参数。

         使用 Tesseract 执行文本识别并将图像保存为带有 OCR 文本层的 1 页 PDF。

         :arg str,fp filename: 要保存的文件，可以是字符串或以 "wb" 模式打开的文件指针（包括 `io.BytesIO()` 对象）。
         :arg bool compress: 是否压缩生成的 PDF，默认值为 `True`。
         :arg str language: 图像中出现的语言。必须以 Tesseract 格式指定，默认值为 "eng"（英文）。对于多种语言，请使用 "+" 分隔的 Tesseract 语言代码，如 "eng+spa"（英语和西班牙语）。
         :arg str tessdata: Tesseract 的语言支持文件夹名。如果省略，则此信息必须作为环境变量 `TESSDATA_PREFIX` 提供。

         .. note:: 
            
            **如果未安装 Tesseract 或未设置环境变量 "TESSDATA_PREFIX" 指向 tessdata 文件夹，并且未作为参数提供，则会失败。**

      .. method:: pdfocr_tobytes(compress=True, language="eng", tessdata=None)

         *版本 1.19.0 新增：*

         *版本 1.22.5 更改：* 支持新的 Tesseract tessdata 参数。

         使用 Tesseract 执行文本识别，并将图像转换为带有 OCR 文本层的 1 页 PDF。内部调用 :meth:`Pixmap.pdfocr_save`。

         :returns: 内存中的 1 页 PDF 文件。可以像这样打开 `doc=pymupdf.open("pdf", pix.pdfocr_tobytes())`，并可以在其 `page=doc[0]` 上执行文本提取。

            .. note::
            
               另一个可能的用途是将其插入到某些 PDF 中。以下代码片段读取文件夹中的图像，并将其作为包含 OCR 文本层的页面存储在新 PDF 中::

                  doc = pymupdf.open()
                  for imgfile in os.listdir(folder):
                     pix = pymupdf.Pixmap(imgfile)
                     imgpdf = pymupdf.open("pdf", pix.pdfocr_tobytes())
                     doc.insert_pdf(imgpdf)
                     pix = None
                     imgpdf.close()
                  doc.save("ocr-images.pdf")

      .. method:: pil_image()

         从 pixmap 创建一个 Pillow 图像。必须安装 PIL / Pillow。

         :raises ImportError: 如果没有安装 Pillow。
         :returns: 一个 `PIL.Image` 对象。

      .. method:: pil_save(*args, unmultiply=False, **kwargs)

         使用 Pillow 将 pixmap 写入图像文件。对于 MuPDF 不支持的输出格式，请使用此方法。例如：

         * 格式 JPX、J2K、WebP 等。
         * 存储 EXIF 信息。
         * 如果未提供 dpi 信息，将自动使用存储在 pixmap 中的 *xres*、*yres* 值。

         一个简单的示例：`pix.pil_save("some.webp", optimize=True, dpi=(150, 150))`。
         
         :arg bool unmultiply: 如果 pixmap 的颜色空间是带透明度的 RGB，alpha 值可能已经或尚未与颜色组件（红/绿/蓝）相乘（称为“预乘”）。若要强制撤销预乘，请将此参数设置为 `True`。要了解更多背景信息，请参考 "预乘 alpha" `here <https://en.wikipedia.org/wiki/Glossary_of_computer_graphics#P>`_。

         有关其他参数的详细信息，请参见 Pillow 文档。

         自 v1.22.0 起，PyMuPDF 已直接支持 JPEG 输出。建议不再使用此方法进行 JPEG 输出，以提高性能并避免不必要的外部依赖。

         :raises ImportError: 如果没有安装 Pillow。

      .. method:: pil_tobytes(*args, unmultiply=False, **kwargs)

         *版本 1.17.3 新增：*

         使用 Pillow 将图像作为指定格式的字节对象返回。例如：`stream = pix.pil_tobytes(format="WEBP", optimize=True, dpi=(150, 150))`。有关其他参数的详细信息，请参见 Pillow 文档。

         :raises ImportError: 如果没有安装 Pillow。

         :rtype: bytes

      .. method:: warp(quad, width, height)

         *版本 1.19.3 新增：*

         通过“变形”四边形返回一个新的 pixmap，使得四边形的角成为新 pixmap 的角。目标 pixmap 的 `irect` 将是 `(0, 0, width, height)`。

         :arg quad_like quad: 一个包含在 :attr:`Pixmap.irect` 内部（包括边界点）的凸四边形坐标。
         :arg int width: 所需的结果宽度。
         :arg int height: 所需的结果高度。
         :returns: 一个新的 pixmap，其中四边形的角以顺时针方向映射到 pixmap 的角：`quad.ul -> irect.tl`，`quad.ur -> irect.tr`，依此类推。
         :rtype: :ref:`Pixmap`

            .. image:: images/img-warp.*
                  :scale: 40
                  :align: center

      .. method:: color_count(colors=False, clip=None)

         *版本 1.19.2 新增：*
         *版本 1.19.3 更改：*

         确定 pixmap 的唯一颜色及其计数。

         :arg bool colors: *(版本 1.19.3 更改)* 如果为 `True`，返回一个颜色像素及其使用计数的字典；否则，仅返回唯一颜色的数量。
         :arg rect_like clip: 一个位于 :attr:`Pixmap.irect` 内的矩形。如果提供，只有这些像素会被考虑。这允许直接检查给定 pixmap 的子矩形，而无需构建子 pixmap。
         :rtype: dict 或 int
         :returns: 颜色的数量，或者是一个包含 `pixel: count` 项的字典。pixel 键是一个长度为 :attr:`Pixmap.n` 的 `bytes` 对象。
         
            .. note:: 要恢复像素的 **元组**，可以使用 `tuple(colors.keys()[i])` 获取第 i 项。

               * 响应时间取决于 pixmap 的样本大小，对于非常大的 pixmap，可能会超过一秒。
               * 在适用的情况下，具有不同 alpha 值的像素将被视为不同的颜色。


      .. method:: color_topusage(clip=None)

         *版本 1.19.3 新增：*

         返回最常用的颜色及其相对频率。

         :arg rect_like clip: 一个位于 :attr:`Pixmap.irect` 内的矩形。如果提供，只有这些像素会被考虑。这允许直接检查给定 pixmap 的子矩形，而无需构建子 pixmap。
         :rtype: tuple
         :returns: 一个元组 `(ratio, pixel)`，其中 `0 < ratio <= 1`，*pixel* 是颜色的像素值。使用此方法来判断图像是否“几乎”是单色：响应 `(0.95, b"\x00\x00\x00")` 表示 95% 的像素是黑色。有关示例，请参见 :ref:`RecipesImages_P`。

      .. attribute:: alpha

         指示 pixmap 是否包含透明度信息。

         :type: bool

      .. attribute:: digest

         pixmap 的 MD5 哈希值（16 字节）。这是一个用于唯一标识的技术值。

         :type: bytes

      .. attribute:: colorspace

         pixmap 的颜色空间。如果图像被视为所谓的 *图像遮罩* 或 *模板遮罩*，则该值可能为 *None*（当前仅在提取的 PDF 文档图像中发生）。

         :type: :ref:`Colorspace`

      .. attribute:: stride

         包含图像数据中每行的长度，即 :attr:`Pixmap.samples` 的长度。这主要用于计算目的。以下表达式成立：

         * `len(samples) == height * stride`
         * `width * n == stride`

         :type: int

      .. attribute:: is_monochrome

         *版本 1.19.2 新增：*

         如果是灰度 pixmap 且仅包含黑色和白色，则为 `True`。

         :type: bool

      .. attribute:: is_unicolor

         *版本 1.19.2 新增：*

         如果所有像素都相同（任何颜色空间），则为 `True`。在适用的情况下，具有不同 alpha 值的像素将被视为不同的颜色。

         :type: bool

      .. attribute:: irect

         包含 pixmap 的 :ref:`IRect`。

         :type: :ref:`IRect`

      .. attribute:: samples

         所有像素的颜色和（如果 :attr:`Pixmap.alpha` 为 true）透明度值。这是一个 `width * height * n` 字节的区域。每 n 字节定义一个像素。每 n 字节依次表示一个像素，按扫描线顺序排列。后续扫描线紧接着排列，不进行填充。例如，对于 RGBA 颜色空间，*samples* 是一个字节序列，如 *..., R, G, B, A, ...*，其中四个字节 R, G, B, A 定义一个像素。

         该区域可以传递给其他图形库，如 PIL（Python Imaging Library），以进行额外处理，例如将 pixmap 保存为其他图像格式。

         .. note::
            * 底层数据通常是一个 **大型** 内存区域，每次访问时会为此属性创建 `bytes` 副本：例如，一个 RGB 渲染的字母页面的 samples 大小接近 1.4 MB。因此，请考虑将其分配给新变量，或使用 `memoryview` 版本 :attr:`Pixmap.samples_mv` （v1.18.17 新增）。
            * 对底层数据的任何更改仅在再次访问此属性时才可见。这与使用 memoryview 版本不同。
         
         :type: bytes


      .. attribute:: samples_mv

         *版本 v1.18.17 新增：*

         类似于 :attr:`Pixmap.samples`，但为 Python `memoryview` 格式。它直接指向 pixmap 中的内存，而不是其副本。因此，它的创建速度与 pixmap 的大小无关，并且对像素的任何更改会立即生效。

         像 `bytearray(pix.samples_mv)` 或 `bytes(pixmap.samples_mv)` 这样的副本等效于并可替代 `pix.samples`。

         我们也可以看到 `len(pix.samples) == len(pix.samples_mv)`。

         以下是一个 2 MB JPEG 的示例：内存视图 **快一万倍**::

            In [3]: %timeit len(pix.samples_mv)
            367 ns ± 1.75 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
            In [4]: %timeit len(pix.samples)
            3.52 ms ± 57.5 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)

         在 Pixmap 被销毁后，任何尝试使用内存视图的操作都会因为 `ValueError` 失败。

         :type: memoryview

      .. attribute:: samples_ptr

         *版本 v1.18.17 新增：*

         指向像素区域的 Python 指针。这是一种特殊的整数格式，可以被支持的应用程序（如 PyQt）用来直接访问样本区域，从而极大加速图像的构建。例如::

            img = QtGui.QImage(pix.samples, pix.width, pix.height, format) # (1)
            img = QtGui.QImage(pix.samples_ptr, pix.width, pix.height, format) # (2)

         上述两种方法得到的 Qt 图像相同，但 (2) 可以 **快上几百倍**，因为它避免了对像素区域的额外拷贝。

         警告：在 Pixmap 被销毁后，Python 指针将失效，尝试使用它可能会导致 Python 解释器崩溃。

         :type: int

      .. attribute:: size

         包含 *len(pixmap)*。这通常等于 *len(pix.samples)* 加上一些平台特定的值，用于定义对象的其他属性。

         :type: int

      .. attribute:: width

      .. attribute:: w

         区域的宽度（以像素为单位）。

         :type: int

      .. attribute:: height

      .. attribute:: h

         区域的高度（以像素为单位）。

         :type: int

      .. attribute:: x

         左上角的 X 坐标（以像素为单位）。不能直接更改 -- 使用 :meth:`Pixmap.set_origin`。

         :type: int

      .. attribute:: y

         左上角的 Y 坐标（以像素为单位）。不能直接更改 -- 使用 :meth:`Pixmap.set_origin`。

         :type: int

      .. attribute:: n

         每个像素的组件数量。这个数值取决于颜色空间和 alpha。如果颜色空间不是 *None*（例如模板遮罩），那么 *Pixmap.n - Pixmap.alpha == pixmap.colorspace.n* 为真。如果颜色空间是 *None*，则 *n == alpha == 1*。

         :type: int

      .. attribute:: xres

         水平分辨率（每英寸点数 dpi）。请参阅 :data:`resolution`。不能直接更改 -- 使用 :meth:`Pixmap.set_dpi`。

         :type: int

      .. attribute:: yres

         垂直分辨率（每英寸点数 dpi）。请参阅 :data:`resolution`。不能直接更改 -- 使用 :meth:`Pixmap.set_dpi`。

         :type: int

      .. attribute:: interpolate

         仅为信息提供的布尔标志。如果图像将使用“线性插值”进行绘制，则设置为 *True*；如果使用“最近邻采样”则为 *False*。

         :type: bool

.. tab:: 英文

   .. class:: Pixmap
      :no-index:

      .. method:: __init__(self, colorspace, irect, alpha=False)
         :no-index:

         **New empty pixmap:** Create an empty pixmap of size and origin given by the rectangle. So, *irect.top_left* designates the top left corner of the pixmap, and its width and height are *irect.width* resp. *irect.height*. Note that the image area is **not initialized** and will contain crap data -- use eg. :meth:`clear_with` or :meth:`set_rect` to be sure.

         :arg colorspace: colorspace.
         :type colorspace: :ref:`Colorspace`

         :arg irect_like irect: The pixmap's position and dimension.

         :arg bool alpha: Specifies whether transparency bytes should be included. Default is *False*.

      .. method:: __init__(self, colorspace, source)
         :no-index:

         **Copy and set colorspace:** Copy *source* pixmap converting colorspace. Any colorspace combination is possible, but source colorspace must not be *None*.

         :arg colorspace: desired **target** colorspace. This **may also be** *None*. In this case, a "masking" pixmap is created: its :attr:`Pixmap.samples` will consist of the source's alpha bytes only.
         :type colorspace: :ref:`Colorspace`

         :arg source: the source pixmap.
         :type source: *Pixmap*

      .. method:: __init__(self, source, mask)
         :no-index:

         * New in v1.18.18

         **Copy and add image mask:** Copy *source* pixmap, add an alpha channel with transparency data from a mask pixmap.

         :arg source: pixmap without alpha channel.
         :type source: :ref:`Pixmap`

         :arg mask: a mask pixmap. Must be a graysale pixmap.
         :type mask: :ref:`Pixmap`

      .. method:: __init__(self, source, width, height, [clip])
         :no-index:

         **Copy and scale:** Copy *source* pixmap, scaling new width and height values -- the image will appear stretched or shrunk accordingly. Supports partial copying. The source colorspace may be *None*.

         :arg source: the source pixmap.
         :type source: *Pixmap*

         :arg float width: desired target width.

         :arg float height: desired target height.

         :arg irect_like clip: restrict the resulting pixmap to this region of the **scaled** pixmap.

         .. note:: If width or height do not *represent* integers (i.e. `value.is_integer() != True`), then the resulting pixmap **will have an alpha channel**.

      .. method:: __init__(self, source, alpha=1)
         :no-index:

         **Copy and add or drop alpha:** Copy *source* and add or drop its alpha channel. Identical copy if *alpha* equals *source.alpha*. If an alpha channel is added, its values will be set to 255.

         :arg source: source pixmap.
         :type source: *Pixmap*

         :arg bool alpha: whether the target will have an alpha channel, default and mandatory if source colorspace is *None*.

         .. note:: A typical use includes separation of color and transparency bytes in separate pixmaps. Some applications require this like e.g. *wx.Bitmap.FromBufferAndAlpha()* of *wxPython*:

            >>> # 'pix' is an RGBA pixmap
            >>> pixcolors = pymupdf.Pixmap(pix, 0)    # extract the RGB part (drop alpha)
            >>> pixalpha = pymupdf.Pixmap(None, pix)  # extract the alpha part
            >>> bm = wx.Bitmap.FromBufferAndAlpha(pix.width, pix.height, pixcolors.samples, pixalpha.samples)


      .. method:: __init__(self, filename)
         :no-index:

         **From a file:** Create a pixmap from *filename*. All properties are inferred from the input. The origin of the resulting pixmap is *(0, 0)*.

         :arg str filename: Path of the image file.

      .. method:: __init__(self, stream)
         :no-index:

         **From memory:** Create a pixmap from a memory area. All properties are inferred from the input. The origin of the resulting pixmap is *(0, 0)*.

         :arg bytes,bytearray,BytesIO stream: Data containing a complete, valid image. Could have been created by e.g. *stream = bytearray(open('image.file', 'rb').read())*. Type *bytes* is supported in **Python 3 only**, because *bytes == str* in Python 2 and the method will interpret the stream as a filename.

            *Changed in version 1.14.13:* *io.BytesIO* is now also supported.


      .. method:: __init__(self, colorspace, width, height, samples, alpha)
         :no-index:

         **From plain pixels:** Create a pixmap from *samples*. Each pixel must be represented by a number of bytes as controlled by the *colorspace* and *alpha* parameters. The origin of the resulting pixmap is *(0, 0)*. This method is useful when raw image data are provided by some other program -- see :ref:`FAQ`.

         :arg colorspace: Colorspace of image.
         :type colorspace: :ref:`Colorspace`

         :arg int width: image width

         :arg int height: image height

         :arg bytes,bytearray,BytesIO samples:  an area containing all pixels of the image. Must include alpha values if specified.

            *Changed in version 1.14.13:* (1) *io.BytesIO* can now also be used. (2) Data are now **copied** to the pixmap, so may safely be deleted or become unavailable.

         :arg bool alpha: whether a transparency channel is included.

         .. note::

            1. The following equation **must be true**: *(colorspace.n + alpha) * width * height == len(samples)*.
            2. Starting with version 1.14.13, the samples data are **copied** to the pixmap.


      .. method:: __init__(self, doc, xref)
         :no-index:

         **From a PDF image:** Create a pixmap from an image **contained in PDF** *doc* identified by its :data:`xref`. All pimap properties are set by the image. Have a look at `extract-img1.py <https://github.com/pymupdf/PyMuPDF/tree/master/demo/extract-img1.py>`_ and `extract-img2.py <https://github.com/pymupdf/PyMuPDF/tree/master/demo/extract-img2.py>`_ to see how this can be used to recover all of a PDF's images.

         :arg doc: an opened **PDF** document.
         :type doc: :ref:`Document`

         :arg int xref: the :data:`xref` of an image object. For example, you can make a list of images used on a particular page with :meth:`Document.get_page_images`, which also shows the :data:`xref` numbers of each image.

      .. method:: clear_with([value [, irect]])
         :no-index:

         Initialize the samples area.

         :arg int value: if specified, values from 0 to 255 are valid. Each color byte of each pixel will be set to this value, while alpha will be set to 255 (non-transparent) if present. If omitted, then all bytes (including any alpha) are cleared to *0x00*.

         :arg irect_like irect: the area to be cleared. Omit to clear the whole pixmap. Can only be specified, if *value* is also specified.

      .. method:: tint_with(black, white)
         :no-index:

         Colorize a pixmap by replacing black and / or white with colors given as **sRGB integer** values. Only colorspaces :data:`CS_GRAY` and :data:`CS_RGB` are supported, others are ignored with a warning.

         If the colorspace is :data:`CS_GRAY`, the average *(red + green + blue)/3* will be taken. The pixmap will be changed in place.

         :arg int black: replace black with this value. Specifying 0x000000 makes no changes.
         :arg int white: replace white with this value. Specifying 0xFFFFFF makes no changes.

         Examples:

            * `tint_with(0x000000, 0xFFFFFF)` is a no-op.
            * `tint_with(0x00FF00, 0xFFFFFF)` changes black to green, leaves white intact.
            * `tint_with(0xFF0000, 0x0000FF)` changes black to red and white to blue.


      .. method:: gamma_with(gamma)
         :no-index:

         Apply a gamma factor to a pixmap, i.e. lighten or darken it. Pixmaps with colorspace *None* are ignored with a warning.

         :arg float gamma: *gamma = 1.0* does nothing, *gamma < 1.0* lightens, *gamma > 1.0* darkens the image.

      .. method:: shrink(n)
         :no-index:

         Shrink the pixmap by dividing both, its width and height by 2\ :sup:``n``.

         :arg int n: determines the new pixmap (samples) size. For example, a value of 2 divides width and height by 4 and thus results in a size of one 16\ :sup:`th` of the original. Values less than 1 are ignored with a warning.

         .. note:: Use this methods to reduce a pixmap's size retaining its proportion. The pixmap is changed "in place". If you want to keep original and also have more granular choices, use the resp. copy constructor above.

      .. method:: pixel(x, y)
         :no-index:

         *New in version:: 1.14.5:* Return the value of the pixel at location (x, y) (column, line).

         :arg int x: the column number of the pixel. Must be in `range(pix.width)`.
         :arg int y: the line number of the pixel, Must be in `range(pix.height)`.

         :rtype: list
         :returns: a list of color values and, potentially the alpha value. Its length and content depend on the pixmap's colorspace and the presence of an alpha. For RGBA pixmaps the result would e.g. be *[r, g, b, a]*. All items are integers in `range(256)`.

      .. method:: set_pixel(x, y, color)
         :no-index:

         *New in version 1.14.7:* Manipulate the pixel at location (x, y) (column, line).

         :arg int x: the column number of the pixel. Must be in `range(pix.width)`.
         :arg int y: the line number of the pixel. Must be in `range(pix.height)`.
         :arg sequence color: the desired pixel value given as a sequence of integers in `range(256)`. The length of the sequence must equal :attr:`Pixmap.n`, which includes any alpha byte.

      .. method:: set_rect(irect, color)
         :no-index:

         *New in version 1.14.8:* Set the pixels of a rectangle to a value.

         :arg irect_like irect: the rectangle to be filled with the value. The actual area is the intersection of this parameter and :attr:`Pixmap.irect`. For an empty intersection (or an invalid parameter), no change will happen.
         :arg sequence color: the desired value, given as a sequence of integers in `range(256)`. The length of the sequence must equal :attr:`Pixmap.n`, which includes any alpha byte.

         :rtype: bool
         :returns: *False* if the rectangle was invalid or had an empty intersection with :attr:`Pixmap.irect`, else *True*.

         .. note::

            1. This method is equivalent to :meth:`Pixmap.set_pixel` executed for each pixel in the rectangle, but is obviously **very much faster** if many pixels are involved.
            2. This method can be used similar to :meth:`Pixmap.clear_with` to initialize a pixmap with a certain color like this: *pix.set_rect(pix.irect, (255, 255, 0))* (RGB example, colors the complete pixmap with yellow).

      .. method:: set_origin(x, y)
         :no-index:

         * New in v1.17.7
         
         Set the x and y values of the pixmap's top-left point.

         :arg int x: x coordinate
         :arg int y: y coordinate


      .. method:: set_dpi(xres, yres)
         :no-index:

         * New in v1.16.17

         * Changed in v1.18.0: When saving as a PNG image, these values will be stored now.

         Set the resolution (dpi) in x and y direction.

         :arg int xres: resolution in x direction.
         :arg int yres: resolution in y direction.


      .. method:: set_alpha(alphavalues, premultiply=1, opaque=None)
         :no-index:

         * Changed in v 1.18.13

         Change the alpha values. The pixmap must have an alpha channel.

         :arg bytes,bytearray,BytesIO alphavalues: the new alpha values. If provided, its length must be at least *width * height*. If omitted (`None`), all alpha values are set to 255 (no transparency). *Changed in version 1.14.13:* *io.BytesIO* is now also accepted.
         :arg bool premultiply: *New in v1.18.13:* whether to premultiply color components with the alpha value.
         :arg list,tuple opaque: ignore the alpha value and set this color to fully transparent. A sequence of integers in `range(256)` with a length of :attr:`Pixmap.n`. Default is *None*. For example, a typical choice for RGB would be `opaque=(255, 255, 255)` (white).


      .. method:: invert_irect([irect])
         :no-index:

         Invert the color of all pixels in :ref:`IRect` *irect*. Will have no effect if colorspace is *None*.

         :arg irect_like irect: The area to be inverted. Omit to invert everything.

      .. method:: copy(source, irect)
         :no-index:

         Copy the *irect* part of the *source* pixmap into the corresponding area of this one. The two pixmaps may have different dimensions and can each have :data:`CS_GRAY` or :data:`CS_RGB` colorspaces, but they currently **must** have the same alpha property [#f2]_. The copy mechanism automatically adjusts discrepancies between source and target like so:

         If copying from :data:`CS_GRAY` to :data:`CS_RGB`, the source gray-shade value will be put into each of the three rgb component bytes. If the other way round, *(r + g + b) / 3* will be taken as the gray-shade value of the target.

         Between *irect* and the target pixmap's rectangle, an "intersection" is calculated at first. This takes into account the rectangle coordinates and the current attribute values :attr:`Pixmap.x` and :attr:`Pixmap.y` (which you are free to modify for this purpose via :meth:`Pixmap.set_origin`). Then the corresponding data of this intersection are copied. If the intersection is empty, nothing will happen.

         :arg source: source pixmap.
         :type source: :ref:`Pixmap`

         :arg irect_like irect: The area to be copied.

         .. note:: Example: Suppose you have two pixmaps, `pix1` and `pix2` and you want to copy the lower right quarter of `pix2` to `pix1` such that it starts at the top-left point of `pix1`. Use the following snippet::

            >>> # safeguard: set top-left of pix1 and pix2 to (0, 0)
            >>> pix1.set_origin(0, 0)
            >>> pix2.set_origin(0, 0)
            >>> # compute top-left coordinates of pix2 region to copy
            >>> x1 = int(pix2.width / 2)
            >>> y1 = int(pix2.height / 2)
            >>> # shift top-left of pix2 such, that the to-be-copied
            >>> # area starts at (0, 0):
            >>> pix2.set_origin(-x1, -y1)
            >>> # now copy ...
            >>> pix1.copy(pix2, (0, 0, x1, y1))

            .. image:: images/img-pixmapcopy.*
               :scale: 20

      .. method:: save(filename, output=None, jpg_quality=95)
         :no-index:

         * Changed in v1.22.0: Added **direct support of JPEG** images. Image quality can be controlled via parameter "jpg_quality".

         Save pixmap as an image file. Depending on the output chosen, only some or all colorspaces are supported and different file extensions can be chosen. Please see the table below.

         :arg str,Path,file filename: The file to save to. May be provided as a string, as a ``pathlib.Path`` or as a Python file object. In the latter two cases, the filename is taken from the resp. object. The filename's extension determines the image format, which can be overruled by the output parameter.

         :arg str output: The desired image format. The default is the filename's extension. If both, this value and the file extension are unsupported, an exception is raised. For possible values see :ref:`PixmapOutput`.
         :arg int jpg_quality: The desired image quality, default 95. Only applies to JPEG images, else ignored. This parameter trades quality against file size. A value of 98 is close to lossless. Higher values should not lead to better quality.

         :raises ValueError: For unsupported image formats.

      .. method:: tobytes(output="png", jpg_quality=95)
         :no-index:

         * New in version 1.14.5: Return the pixmap as a *bytes* memory object of the specified format -- similar to :meth:`save`.
         * Changed in v1.22.0: Added **direct JPEG support**. Image quality can be influenced via new parameter "jpg_quality".

         :arg str output: The desired image format. The default is "png". For possible values see :ref:`PixmapOutput`.
         :arg int jpg_quality: The desired image quality, default 95. Only applies to JPEG images, else ignored. This parameter trades quality against file size. A value of 98 is close to lossless. Higher values should not lead to better quality.

         :raises ValueError: For unsupported image formats.
         :rtype: bytes

         :arg str output: The requested image format. The default is "png". For other possible values see :ref:`PixmapOutput`.

      .. method:: pdfocr_save(filename, compress=True, language="eng", tessdata=None)
         :no-index:

         * New in v1.19.0

         * Changed in v1.22.5: Support of new parameter for Tesseract's tessdata.

         Perform text recognition using Tesseract and save the image as a 1-page PDF with an OCR text layer.

         :arg str,fp filename: identifies the file to save to. May be either a string or a pointer to a file opened with "wb" (includes `io.BytesIO()` objects).
         :arg bool compress: whether to compress the resulting PDF, default is `True`.
         :arg str language: the languages occurring in the image. This must be specified in Tesseract format. Default is "eng" for English. Use "+"-separated Tesseract language codes for multiple languages, like "eng+spa" for English and Spanish.
         :arg str tessdata: folder name of Tesseract's language support. If omitted, this information must be present as environment variable `TESSDATA_PREFIX`.

         .. note:: **Will fail** if Tesseract is not installed or if the environment variable "TESSDATA_PREFIX" is not set to the `tessdata` folder name and not provided as parameter.

      .. method:: pdfocr_tobytes(compress=True, language="eng", tessdata=None)
         :no-index:

         * New in v1.19.0

         * Changed in v1.22.5: Support of new parameter for Tesseract's tessdata.

         Perform text recognition using Tesseract and convert the image to a 1-page PDF with an OCR text layer. Internally invokes :meth:`Pixmap.pdfocr_save`.

         :returns: A 1-page PDF file in memory. Could be opened like `doc=pymupdf.open("pdf", pix.pdfocr_tobytes())`, and text extractions could be performed on its `page=doc[0]`.
         
            .. note::
            
               Another possible use is insertion into some pdf. The following snippet reads the images of a folder and stores them as pages in a new PDF that contain an OCR text layer::

                  doc = pymupdf.open()
                  for imgfile in os.listdir(folder):
                     pix = pymupdf.Pixmap(imgfile)
                     imgpdf = pymupdf.open("pdf", pix.pdfocr_tobytes())
                     doc.insert_pdf(imgpdf)
                     pix = None
                     imgpdf.close()
                  doc.save("ocr-images.pdf")


      ..  method:: pil_image()
         :no-index:

         Create a Pillow Image from the pixmap. PIL / Pillow must be installed.

         :raises ImportError: if Pillow is not installed.
         :returns: a ˇˇPIL.Imageˇˇ object

      ..  method:: pil_save(*args, unmultiply=False, **kwargs)
         :no-index:

         Write the pixmap as an image file using Pillow. Use this method for output unsupported by MuPDF. Examples are

         * Formats JPX, J2K, WebP, etc.
         * Storing EXIF information.
         * If you do not provide dpi information, the values *xres*, *yres* stored with the pixmap are automatically used.

         A simple example: `pix.pil_save("some.webp", optimize=True, dpi=(150, 150))`.
         
         :arg bool unmultiply: If the pixmap's colorspace is RGB with transparency, the alpha values may or may not already be multiplied into the color components ref/green/blue (called "premultiplied"). To enforce undoing premultiplication, set this parameter to `True`. To learn about some background, e.g. look for "Premultiplied alpha" `here <https://en.wikipedia.org/wiki/Glossary_of_computer_graphics#P>`_.


         For details on other parameters see the Pillow documentation.

         Since v1.22.0, PyMuPDF supports JPEG output directly. We recommended to no longer use this method for JPEG output -- for performance reasons and for avoiding unnecessary external dependencies.

         :raises ImportError: if Pillow is not installed.

      ..  method:: pil_tobytes(*args, unmultiply=False, **kwargs)
         :no-index:

         * New in v1.17.3

         Return an image as a bytes object in the specified format using Pillow. For example `stream = pix.pil_tobytes(format="WEBP", optimize=True, dpi=(150, 150))`. Also see above. For details on other parameters see the Pillow documentation.
         
         :raises ImportError: if Pillow is not installed.

         :rtype: bytes


      ..  method:: warp(quad, width, height)
         :no-index:

         * New in v1.19.3

         Return a new pixmap by "warping" the quad such that the quad corners become the new pixmap's corners. The target pixmap's `irect` will be `(0, 0, width, height)`.

         :arg quad_like quad: a convex quad with coordinates inside :attr:`Pixmap.irect` (including the border points).
         :arg int width: desired resulting width.
         :arg int height: desired resulting height.
         :returns: A new pixmap where the quad corners are mapped to the pixmap corners in a clockwise fashion: `quad.ul -> irect.tl`, `quad.ur -> irect.tr`, etc.
         :rtype: :ref:`Pixmap`

            .. image:: images/img-warp.*
               :scale: 40
               :align: center


      ..  method:: color_count(colors=False, clip=None)
         :no-index:

         * New in v1.19.2
         * Changed in v1.19.3

         Determine the pixmap's unique colors and their count.

         :arg bool colors: *(changed in v1.19.3)* If `True` return a dictionary of color pixels and their usage count, else just the number of unique colors.
         :arg rect_like clip: a rectangle inside :attr:`Pixmap.irect`. If provided, only those pixels are considered. This allows inspecting sub-rectangles of a given pixmap directly -- instead of building sub-pixmaps.
         :rtype: dict or int
         :returns: either the number of colors, or a dictionary with the items `pixel: count`. The pixel key is a `bytes` object of length :attr:`Pixmap.n`.
         
            .. note:: To recover the **tuple** of a pixel, use `tuple(colors.keys()[i])` for the i-th item.

               * The response time depends on the pixmap's samples size and may be more than a second for very large pixmaps.
               * Where applicable, pixels with different alpha values will be treated as different colors.


      ..  method:: color_topusage(clip=None)
         :no-index:

         * New in v1.19.3

         Return the most frequently used color and its relative frequency.

         :arg rect_like clip: A rectangle inside :attr:`Pixmap.irect`. If provided, only those pixels are considered. This allows inspecting sub-rectangles of a given pixmap directly -- instead of building sub-pixmaps.
         :rtype: tuple
         :returns: A tuple `(ratio, pixel)` where `0 < ratio <= 1` and *pixel* is the pixel value of the color. Use this to decide if the image is "almost" unicolor: a response `(0.95, b"\x00\x00\x00")` means that 95% of all pixels are black. See an example here :ref:`RecipesImages_P`.


      .. attribute:: alpha
         :no-index:

         Indicates whether the pixmap contains transparency information.

         :type: bool

      .. attribute:: digest
         :no-index:

         The MD5 hashcode (16 bytes) of the pixmap. This is a technical value used for unique identifications.

         :type: bytes

      .. attribute:: colorspace
         :no-index:

         The colorspace of the pixmap. This value may be *None* if the image is to be treated as a so-called *image mask* or *stencil mask* (currently happens for extracted PDF document images only).

         :type: :ref:`Colorspace`

      .. attribute:: stride
         :no-index:

         Contains the length of one row of image data in :attr:`Pixmap.samples`. This is primarily used for calculation purposes. The following expressions are true:

         * `len(samples) == height * stride`
         * `width * n == stride`

         :type: int


      .. attribute:: is_monochrome
         :no-index:

         * New in v1.19.2

         Is `True` for a gray pixmap which only has the colors black and white.

         :type: bool


      .. attribute:: is_unicolor
         :no-index:

         * New in v1.19.2

         Is `True` if all pixels are identical (any colorspace). Where applicable, pixels with different alpha values will be treated as different colors.

         :type: bool


      .. attribute:: irect
         :no-index:

         Contains the :ref:`IRect` of the pixmap.

         :type: :ref:`IRect`

      .. attribute:: samples
         :no-index:

         The color and (if :attr:`Pixmap.alpha` is true) transparency values for all pixels. It is an area of `width * height * n` bytes. Each n bytes define one pixel. Each successive n bytes yield another pixel in scanline order. Subsequent scanlines follow each other with no padding. E.g. for an RGBA colorspace this means, *samples* is a sequence of bytes like *..., R, G, B, A, ...*, and the four byte values R, G, B, A define one pixel.

         This area can be passed to other graphics libraries like PIL (Python Imaging Library) to do additional processing like saving the pixmap in other image formats.

         .. note::
            * The underlying data is typically a **large** memory area, from which a `bytes` copy is made for this attribute ... each time you access it: for example an RGB-rendered letter page has a samples size of almost 1.4 MB. So consider assigning a new variable to it or use the `memoryview` version :attr:`Pixmap.samples_mv` (new in v1.18.17).
            * Any changes to the underlying data are available only after accessing this attribute again. This is different from using the memoryview version.

         :type: bytes

      .. attribute:: samples_mv
         :no-index:

         * New in v1.18.17

         Like :attr:`Pixmap.samples`, but in Python `memoryview` format. It is built pointing to the memory in the pixmap -- not from a copy of it. So its creation speed is independent from the pixmap size, and any changes to pixels will be available immediately.

         Copies like `bytearray(pix.samples_mv)`, or `bytes(pixmap.samples_mv)` are equivalent to and can be used in place of `pix.samples`.
         
         We also have `len(pix.samples) == len(pix.samples_mv)`.
         
         Look at this example from a 2 MB JPEG: the memoryview is **ten thousand times faster**::

            In [3]: %timeit len(pix.samples_mv)
            367 ns ± 1.75 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
            In [4]: %timeit len(pix.samples)
            3.52 ms ± 57.5 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
         
         After the Pixmap has been destroyed, any attempt to use the memoryview
         will fail with ValueError.

         :type: memoryview

      .. attribute:: samples_ptr
         :no-index:

         * New in v1.18.17

         Python pointer to the pixel area. This is a special integer format, which can be used by supporting applications (such as PyQt) to directly address the samples area and thus build their images extremely fast. For example::

            img = QtGui.QImage(pix.samples, pix.width, pix.height, format) # (1)
            img = QtGui.QImage(pix.samples_ptr, pix.width, pix.height, format) # (2)

         Both of the above lead to the same Qt image, but (2) can be **many hundred times faster**, because it avoids an additional copy of the pixel area.
         
         Warning: after the Pixmap has been destroyed, the Python pointer will be
         invalid and attempting to use it may crash the Python interpreter.

         :type: int

      .. attribute:: size
         :no-index:

         Contains *len(pixmap)*. This will generally equal *len(pix.samples)* plus some platform-specific value for defining other attributes of the object.

         :type: int

      .. attribute:: width
         :no-index:

      .. attribute:: w
         :no-index:

         Width of the region in pixels.

         :type: int

      .. attribute:: height
         :no-index:

      .. attribute:: h
         :no-index:

         Height of the region in pixels.

         :type: int

      .. attribute:: x
         :no-index:

         X-coordinate of top-left corner in pixels. Cannot directly be changed -- use :meth:`Pixmap.set_origin`.

         :type: int

      .. attribute:: y
         :no-index:

         Y-coordinate of top-left corner in pixels. Cannot directly be changed -- use :meth:`Pixmap.set_origin`.

         :type: int

      .. attribute:: n
         :no-index:

         Number of components per pixel. This number depends on colorspace and alpha. If colorspace is not *None* (stencil masks), then *Pixmap.n - Pixmap.alpha == pixmap.colorspace.n* is true. If colorspace is *None*, then *n == alpha == 1*.

         :type: int

      .. attribute:: xres
         :no-index:

         Horizontal resolution in dpi (dots per inch). Please also see :data:`resolution`. Cannot directly be changed -- use :meth:`Pixmap.set_dpi`.

         :type: int

      .. attribute:: yres
         :no-index:

         Vertical resolution in dpi (dots per inch). Please also see :data:`resolution`. Cannot directly be changed -- use :meth:`Pixmap.set_dpi`.

         :type: int

      .. attribute:: interpolate
         :no-index:

         An information-only boolean flag set to *True* if the image will be drawn using "linear interpolation". If *False* "nearest neighbour sampling" will be used.

         :type: bool

.. _ImageFiles:

支持的输入图片格式
-----------------------------------------------

Supported Input Image Formats

.. tab:: 中文

   以下文件类型被支持作为 **输入** 来构建 pixmaps: **BMP, JPEG, GIF, TIFF, JXR, JPX**, **PNG**, **PAM** 和所有 **Portable Anymap** 系列（**PBM, PGM, PNM, PPM**）。这种支持是双重的：

   1. 通过 *Pixmap(filename)* 或 *Pixmap(byterray)* 直接创建一个 pixmap。该 pixmap 将具有由图像决定的属性。

   2. 使用 *pymupdf.open(...)* 打开这些文件。结果将显示为一个包含单个页面的文档。为该页面创建一个 pixmap 将提供在此上下文中可用的所有选项：应用矩阵、选择颜色空间和 alpha、将 pixmap 限制为剪切区域等。

   **SVG 图像** 仅通过上述第 2 种方法支持，而不能直接作为 pixmaps。但请记住：结果是一个 **光栅图像**，正如所有 pixmap 一样 [#f3]_。


.. tab:: 英文

   The following file types are supported as **input** to construct pixmaps: **BMP, JPEG, GIF, TIFF, JXR, JPX**, **PNG**, **PAM** and all of the **Portable Anymap** family (**PBM, PGM, PNM, PPM**). This support is two-fold:

   1. Directly create a pixmap with *Pixmap(filename)* or *Pixmap(byterray)*. The pixmap will then have properties as determined by the image.

   2. Open such files with *pymupdf.open(...)*. The result will then appear as a document containing one single page. Creating a pixmap of this page offers all the options available in this context: apply a matrix, choose colorspace and alpha, confine the pixmap to a clip area, etc.

   **SVG images** are only supported via method 2 above, not directly as pixmaps. But remember: the result of this is a **raster image** as is always the case with pixmaps [#f1]_.

.. _PixmapOutput:

支持的输出图片格式
---------------------------------------------------------------------------

Supported Output Image Formats

.. tab:: 中文

   支持多种图像 **输出** 格式。您可以选择直接将图像写入文件（:meth:`Pixmap.save`），或者生成一个字节对象（:meth:`Pixmap.tobytes`）。这两种方法都接受一个字符串来标识所需的格式（下表中的 **Format** 列）。请注意，并非所有组合的 pixmap 颜色空间、透明度支持（alpha）和图像格式都是可能的。

   ========== =============== ========== ============== =================================
   **格式**   **颜色空间**    **透明度** **扩展名**     **描述**
   ========== =============== ========== ============== =================================
   jpg, jpeg  gray, rgb, cmyk no         .jpg, .jpeg    联合图像专家组 
   pam        gray, rgb, cmyk yes        .pam           可移植任意映射
   pbm        gray, rgb       no         .pbm           可移植位图
   pgm        gray, rgb       no         .pgm           可移植灰度图
   png        gray, rgb       yes        .png           可移植网络图形
   pnm        gray, rgb       no         .pnm           可移植任意图形
   ppm        gray, rgb       no         .ppm           可移植像素图
   ps         gray, rgb, cmyk no         .ps            Adobe PostScript 图像
   psd        gray, rgb, cmyk yes        .psd           Adobe Photoshop 文档
   ========== =============== ========== ============== =================================

   .. note::

      * 并非所有图像文件类型在所有操作系统平台上都受到支持（或至少常见）。例如 PAM 和可移植任意映射格式在 Windows 上较为罕见或甚至未知。
      * 特别是在涉及 CMYK 颜色空间时，您可以通过 *rgb_pix = pymupdf.Pixmap(pymupdf.csRGB, cmyk_pix)* 将 CMYK pixmap 转换为 RGB pixmap，然后以所需格式保存。
      * 如上所示，MuPDF 的图像支持范围在输入和输出方面有所不同。在两者都支持的格式中，PNG 和 JPEG 可能是最常见的。
      * 我们还推荐将 "ppm" 格式用于 tkinter 的 *PhotoImage* 方法，像这样： *tkimg = tkinter.PhotoImage(data=pix.tobytes("ppm"))* （请参见教程）。这 **非常** 快（比 PNG 快 **60 倍**）。


.. tab:: 英文

   A number of image **output** formats are supported. You have the option to either write an image directly to a file (:meth:`Pixmap.save`), or to generate a bytes object (:meth:`Pixmap.tobytes`). Both methods accept a string identifying the desired format (**Format** column below). Please note that not all combinations of pixmap colorspace, transparency support (alpha) and image format are possible.

   ========== =============== ========= ============== =================================
   **Format** **Colorspaces** **alpha** **Extensions** **Description**
   ========== =============== ========= ============== =================================
   jpg, jpeg  gray, rgb, cmyk no        .jpg, .jpeg    Joint Photographic Experts Group 
   pam        gray, rgb, cmyk yes       .pam           Portable Arbitrary Map
   pbm        gray, rgb       no        .pbm           Portable Bitmap
   pgm        gray, rgb       no        .pgm           Portable Graymap
   png        gray, rgb       yes       .png           Portable Network Graphics
   pnm        gray, rgb       no        .pnm           Portable Anymap
   ppm        gray, rgb       no        .ppm           Portable Pixmap
   ps         gray, rgb, cmyk no        .ps            Adobe PostScript Image
   psd        gray, rgb, cmyk yes       .psd           Adobe Photoshop Document
   ========== =============== ========= ============== =================================

   .. note::

      * Not all image file types are supported (or at least common) on all OS platforms. E.g. PAM and the Portable Anymap formats are rare or even unknown on Windows.
      * Especially pertaining to CMYK colorspaces, you can always convert a CMYK pixmap to an RGB pixmap with *rgb_pix = pymupdf.Pixmap(pymupdf.csRGB, cmyk_pix)* and then save that in the desired format.
      * As can be seen, MuPDF's image support range is different for input and output. Among those supported both ways, PNG and JPEG are probably the most popular.
      * We also recommend using "ppm" formats as input to tkinter's *PhotoImage* method like this: *tkimg = tkinter.PhotoImage(data=pix.tobytes("ppm"))* (also see the tutorial). This is **very** fast (**60 times** faster than PNG).

.. rubric:: Footnotes

.. [#f1] If you need a **vector image** from the SVG, you must first convert it to a PDF. Try :meth:`Document.convert_to_pdf`. If this is not good enough, look for other SVG-to-PDF conversion tools like the Python packages `svglib <https://pypi.org/project/svglib>`_, `CairoSVG <https://pypi.org/project/cairosvg>`_, `Uniconvertor <https://sk1project.net/modules.php?name=Products&product=uniconvertor&op=download>`_ or the Java solution `Apache Batik <https://github.com/apache/batik>`_. Have a look at our Wiki for more examples.

.. [#f2] To also set the alpha property, add an additional step to this method by dropping or adding an alpha channel to the result.

.. [#f3] 如果需要从 SVG 获取 **矢量图像**，必须首先将其转换为 PDF。尝试使用 :meth:`Document.convert_to_pdf`。如果这还不够好，可以寻找其他 SVG 到 PDF 的转换工具，如 Python 包 `svglib <https://pypi.org/project/svglib>`_、`CairoSVG <https://pypi.org/project/cairosvg>`_、`Uniconvertor <https://sk1project.net/modules.php?name=Products&product=uniconvertor&op=download>`_ 或 Java 解决方案 `Apache Batik <https://github.com/apache/batik>`_。可以查看我们的 Wiki 获取更多示例。

.. [#f4] 要设置 alpha 属性，请在此方法中添加额外的步骤，通过删除或添加 alpha 通道来处理结果。


.. include:: footer.rst
