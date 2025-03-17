.. include:: header.rst

.. _RecipesOCR:


.. |toggleStart| raw:: html

   <details>
   <summary><a>See code</a></summary>

.. |toggleEnd| raw:: html

   </details>

====================================
OCR - 光学字符识别
====================================

OCR - Optical Character Recognition

.. tab:: 中文

   |PyMuPDF| 集成了对 OCR（光学字符识别）的支持。可以将 OCR 用于图像（通过 :ref:`Pixmap` 类）和文档页面。

   该功能目前基于 Tesseract-OCR，必须作为单独的应用程序安装 - 请参阅 :ref:`installation_ocr`。

.. tab:: 英文

   |PyMuPDF| has integrated support for OCR (Optical Character Recognition). It is possible to use OCR for both, images (via the :ref:`Pixmap` class) and for document pages.

   The feature is currently based on Tesseract-OCR which must be installed as a separate application -- see the :ref:`installation_ocr`.

如何对图像进行 OCR 
--------------------

How to OCR an Image

.. tab:: 中文

   必须先将受支持的图像转换为 :ref:`Pixmap`。然后可以将 Pixmap 保存为 1 页 PDF。此页面看起来与原始图像一样，具有相同的宽度和高度。它将包含 Tesseract 识别的文本层。

   可以通过方法之一 :meth:`Pixmap.pdfocr_save` 或 :meth:`Pixmap.pdfocr_tobytes` 生成 PDF，作为磁盘上的文件或内存中的 PDF。

   可以使用常用的文本提取和搜索方法（:meth:`Page.get_text`、:meth:`Page.search_for` 等）提取和搜索文本。还请注意以下重要事实和先决条件：

   * 将图像转换为 Pixmap 时，请确认颜色空间为 RGB 且 alpha 为 `False`（无透明度）。如有必要，请转换原始 Pixmap。
   * 所有文本都使用 Tesseract 自己的 `GlyphLessFont`（一种等宽字体，其度量与 Courier 相当）书写为“隐藏”。
   * 所有文本都具有常规和黑色属性（即无粗体、无斜体、无原始字体信息）。
   * Tesseract 无法识别矢量图形（即无绘图/线条图）。

   还建议使用此方法来对完整的扫描 PDF 进行 OCR：

   * 将每页渲染到具有所需分辨率的 :ref:`Pixmap`
   * 将生成的 1 页 PDF 附加到输出 PDF

.. tab:: 英文

   A supported image must first be converted to a :ref:`Pixmap`. The Pixmap can then be saved to a 1-page PDF. This page will look like the original image with the same width and height. It will contain a layer of text as recognized by Tesseract.

   The PDF can be generated via one of the methods :meth:`Pixmap.pdfocr_save` or :meth:`Pixmap.pdfocr_tobytes`, as a file on disk or as a PDF in memory.

   The text can be extracted and searched with the usual text extraction and search methods (:meth:`Page.get_text`, :meth:`Page.search_for`, etc.). Please also note the following important facts and prerequisites:

   * When converting the image to a Pixmap, please confirm that the color space is RGB and alpha is `False` (no transparency). Convert the original Pixmap if necessary.
   * All text is written as "hidden" with Tesseract's own `GlyphLessFont`, a mono-spaced font with metrics comparable to Courier.
   * All text has the properties regular and black (i.e. no bold, no italic, no information about the original fonts).
   * Tesseract does not recognize vector graphics (i.e. no drawings / line-art).

   This approach is also recommended to OCR a complete scanned PDF:

   * Render each page to a :ref:`Pixmap` with desired resolution
   * Append the resulting 1-page PDF to the output PDF

如何对文档页面进行 OCR
----------------------------

How to OCR a Document Page

.. tab:: 中文

   任何支持的文档页面都可以进行 OCR 处理——可以是整个页面，也可以仅是页面上的图像区域。

   由于光学字符识别（OCR）比标准文本提取慢大约一千倍，因此我们确保每个页面的 OCR 只进行一次，并将结果存储在 :ref:`TextPage` 中。使用此 TextPage 进行后续的所有文本提取和搜索时，将以 |PyMuPDF| 的正常速度进行。

   要对文档页面进行 OCR，可以按照以下方法进行：

   1. 确定是否需要进行 OCR 或是否有益。可以使用以下一些标准来做出此决定，例如：

      * 页面完全被图像覆盖
      * 页面上没有文本
      * 上千个小的矢量图形（表示 *模拟* 文本）

   2. 对页面进行 OCR 处理，并将结果存储在 :ref:`TextPage` 对象中，使用类似 `tp = page.get_textpage_ocr(...)` 的指令。

   3. 在所有后续的文本提取和搜索中，引用生成的 :ref:`TextPage`，通过 `textpage=tp` 参数来实现。


.. tab:: 英文

   Any supported document page can be OCR-ed -- either the complete page or only the image areas on it.

   Because optical character recognition is about one thousand times slower than standard text extraction, we make sure to do OCR only once per page and store the result in a :ref:`TextPage`. Using this TextPage for all subsequent extractions and text searches will then happen with |PyMuPDF|'s usual top speed.

   To OCR a document page, follow this approach:

   1. Determine whether OCR is needed / beneficial at all. A number of criteria can be used for this decision, like:

      * page is completely covered by an image
      * no text exists on the page
      * thousands of small vector graphics (indicating *simulated* text)

   2. OCR the page and store result in a :ref:`TextPage` object using an instruction like `tp = page.get_textpage_ocr(...)`.

   3. Refer to the produced :ref:`TextPage` in all subsequent text extractions and searches via the `textpage=tp` parameter.


.. include:: footer.rst
