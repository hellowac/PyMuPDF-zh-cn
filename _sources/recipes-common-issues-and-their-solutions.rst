.. include:: header.rst

.. _RecipesCommonIssuesAndTheirSolutions:

==========================================
常见问题及其解决方案
==========================================

Common Issues and their Solutions

如何动态清理损坏的 :title:`PDF`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How To Dynamically Clean Up Corrupt :title:`PDFs`

.. tab:: 中文

    此示例展示了 |PyMuPDF| 与其他 Python PDF 库的潜在结合使用方式（这里以优秀的纯 Python 包 `pdfrw <https://pypi.python.org/pypi/pdfrw>`_ 为例）。

    如果需要一个干净、无损坏、已解压缩的 PDF，可以动态调用 PyMuPDF 来修复许多问题，如下所示::

        import sys
        from io import BytesIO
        from pdfrw import PdfReader
        import pymupdf

        #---------------------------------------
        # “容错” PDF 读取器
        #---------------------------------------
        def reader(fname, password=None):
            idata = open(fname, "rb").read()  # 读取 PDF 到内存
            ibuffer = BytesIO(idata)  # 转换为流
            if password is None:
                try:
                    return PdfReader(ibuffer)  # 如果可以正常读取，则直接返回
                except:
                    pass

            # 需要密码，或遇到问题的 PDF
            # 创建修复 / 解压缩 / 解密版本
            doc = pymupdf.open("pdf", ibuffer)
            if password is not None:  # 如果提供了密码，则解密
                rc = doc.authenticate(password)
                if not rc > 0:
                    raise ValueError("密码错误")
            c = doc.tobytes(garbage=3, deflate=True)
            del doc  # 关闭并删除 doc
            return PdfReader(BytesIO(c))  # 让 pdfrw 重新尝试读取

        #---------------------------------------
        # 主程序
        #---------------------------------------
        pdf = reader("pymupdf.pdf", password=None)  # 如有必要，提供密码
        print pdf.Info
        # 进行后续处理

    使用命令行工具 *pdftk* （ `仅适用于 Windows <https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/>`_ ，但据报道可在 `Wine <https://www.winehq.org/>`_ 下运行），也可以实现类似效果，详情见 `这里 <http://www.overthere.co.uk/2013/07/22/improving-pypdf2-with-pdftk/>`_ 。但在该方法中，需要通过 *subprocess.Popen* 作为独立进程调用，并使用 stdin 和 stdout 进行通信。


.. tab:: 英文

    This shows a potential use of |PyMuPDF| with another Python PDF library (the excellent pure Python package `pdfrw <https://pypi.python.org/pypi/pdfrw>`_ is used here as an example).

    If a clean, non-corrupt / decompressed PDF is needed, one could dynamically invoke PyMuPDF to recover from many problems like so::

        import sys
        from io import BytesIO
        from pdfrw import PdfReader
        import pymupdf

        #---------------------------------------
        # 'Tolerant' PDF reader
        #---------------------------------------
        def reader(fname, password = None):
            idata = open(fname, "rb").read()  # read the PDF into memory and
            ibuffer = BytesIO(idata)  # convert to stream
            if password is None:
                try:
                    return PdfReader(ibuffer)  # if this works: fine!
                except:
                    pass

            # either we need a password or it is a problem-PDF
            # create a repaired / decompressed / decrypted version
            doc = pymupdf.open("pdf", ibuffer)
            if password is not None:  # decrypt if password provided
                rc = doc.authenticate(password)
                if not rc > 0:
                    raise ValueError("wrong password")
            c = doc.tobytes(garbage=3, deflate=True)
            del doc  # close & delete doc
            return PdfReader(BytesIO(c))  # let pdfrw retry
        #---------------------------------------
        # Main program
        #---------------------------------------
        pdf = reader("pymupdf.pdf", password = None) # include a password if necessary
        print pdf.Info
        # do further processing

    With the command line utility *pdftk* (`available <https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/>`_ for Windows only, but reported to also run under `Wine <https://www.winehq.org/>`_) a similar result can be achieved, see `here <http://www.overthere.co.uk/2013/07/22/improving-pypdf2-with-pdftk/>`_. However, you must invoke it as a separate process via *subprocess.Popen*, using stdin and stdout as communication vehicles.



如何将任何文档转换为 |PDF|
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Convert Any Document to |PDF|

.. tab:: 中文

    以下是一个脚本，它可以将 |PyMuPDF| :ref:`支持的文档<Supported_File_Types>` 转换为 |PDF|。支持的文件格式包括 XPS、EPUB、FB2、CBZ 以及图像格式（包括多页 TIFF）。

    该脚本支持保留源文档的元数据、目录（书签）和链接::

        """
        演示脚本：将输入文件转换为 PDF
        -----------------------------------------
        适用于 XPS、EPUB 等多页输入文件。

        功能：
        ---------
        1. 还原输入文件的目录（书签）和链接。
        2. 目录的恢复通常较好，而链接仅在其类型不是 "LINK_NAMED" 时有效。
        本脚本会跳过 "LINK_NAMED" 类型的链接。
        3. XPS 和 EPUB 输入的内部链接通常属于 "LINK_NAMED" 类型，
        由于 MuPDF 底层库不会自动解析这些链接到具体页码，因此无法直接恢复。
        4. 了解这些文档格式结构的专家可以进一步解析和处理这些链接。

        依赖项：
        --------------
        PyMuPDF v1.14.0+
        """
        import sys
        import pymupdf

        if not (list(map(int, pymupdf.VersionBind.split("."))) >= [1, 14, 0]):
            raise SystemExit("需要 PyMuPDF v1.14.0+")

        fn = sys.argv[1]

        print("正在转换 '%s' 为 '%s.pdf'" % (fn, fn))

        doc = pymupdf.open(fn)

        b = doc.convert_to_pdf()  # 转换为 PDF
        pdf = pymupdf.open("pdf", b)  # 以 PDF 形式打开

        toc = doc.get_toc()  # 读取输入文档的目录
        pdf.set_toc(toc)  # 直接应用到输出 PDF
        meta = doc.metadata  # 读取并设置元数据

        if not meta["producer"]:
            meta["producer"] = "PyMuPDF v" + pymupdf.VersionBind

        if not meta["creator"]:
            meta["creator"] = "PyMuPDF PDF converter"

        meta["modDate"] = pymupdf.get_pdf_now()
        meta["creationDate"] = meta["modDate"]
        pdf.set_metadata(meta)

        # 处理链接
        link_cnti = 0
        link_skip = 0
        for pinput in doc:  # 遍历输入文档的每一页
            links = pinput.get_links()  # 获取该页的所有链接
            link_cnti += len(links)  # 统计链接总数
            pout = pdf[pinput.number]  # 获取对应的输出 PDF 页
            for l in links:  # 遍历所有链接
                if l["kind"] == pymupdf.LINK_NAMED:  # 不处理 "LINK_NAMED" 类型的链接
                    print("命名链接（跳过）: 页面", pinput.number, l)
                    link_skip += 1  # 统计跳过的链接数量
                    continue
                pout.insert_link(l)  # 直接插入其他类型的链接

        # 保存转换结果
        pdf.save(fn + ".pdf", garbage=4, deflate=True)

        # 输出跳过的命名链接数量
        if link_cnti > 0:
            print("在输入文件中，跳过了 %i 个命名链接，总计 %i 个链接。" % (link_skip, link_cnti))


.. tab:: 英文


    Here is a script that converts any |PyMuPDF| :ref:`supported document<Supported_File_Types>` to a |PDF|. These include XPS, EPUB, FB2, CBZ and image formats, including multi-page TIFF images.

    It features maintaining any metadata, table of contents and links contained in the source document::

        """
        Demo script: Convert input file to a PDF
        -----------------------------------------
        Intended for multi-page input files like XPS, EPUB etc.

        Features:
        ---------
        Recovery of table of contents and links of input file.
        While this works well for bookmarks (outlines, table of contents),
        links will only work if they are not of type "LINK_NAMED".
        This link type is skipped by the script.

        For XPS and EPUB input, internal links however **are** of type "LINK_NAMED".
        Base library MuPDF does not resolve them to page numbers.

        So, for anyone expert enough to know the internal structure of these
        document types, can further interpret and resolve these link types.

        Dependencies
        --------------
        PyMuPDF v1.14.0+
        """
        import sys
        import pymupdf
        if not (list(map(int, pymupdf.VersionBind.split("."))) >= [1,14,0]):
            raise SystemExit("need PyMuPDF v1.14.0+")
        fn = sys.argv[1]

        print("Converting '%s' to '%s.pdf'" % (fn, fn))

        doc = pymupdf.open(fn)

        b = doc.convert_to_pdf()  # convert to pdf
        pdf = pymupdf.open("pdf", b)  # open as pdf

        toc= doc.get_toc()  # table of contents of input
        pdf.set_toc(toc)  # simply set it for output
        meta = doc.metadata  # read and set metadata
        if not meta["producer"]:
            meta["producer"] = "PyMuPDF v" + pymupdf.VersionBind

        if not meta["creator"]:
            meta["creator"] = "PyMuPDF PDF converter"
        meta["modDate"] = pymupdf.get_pdf_now()
        meta["creationDate"] = meta["modDate"]
        pdf.set_metadata(meta)

        # now process the links
        link_cnti = 0
        link_skip = 0
        for pinput in doc:  # iterate through input pages
            links = pinput.get_links()  # get list of links
            link_cnti += len(links)  # count how many
            pout = pdf[pinput.number]  # read corresp. output page
            for l in links:  # iterate though the links
                if l["kind"] == pymupdf.LINK_NAMED:  # we do not handle named links
                    print("named link page", pinput.number, l)
                    link_skip += 1  # count them
                    continue
                pout.insert_link(l)  # simply output the others

        # save the conversion result
        pdf.save(fn + ".pdf", garbage=4, deflate=True)
        # say how many named links we skipped
        if link_cnti > 0:
            print("Skipped %i named links of a total of %i in input." % (link_skip, link_cnti))



更改注释：意外行为
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Changing Annotations: Unexpected Behaviour

问题
^^^^^^^^^

Problem

.. tab:: 中文

    有两种情况可能会导致意外的注释变化：

    1. **更新** 由其他软件创建的注释时使用 PyMuPDF 进行修改。
    2. **创建** 注释时使用 PyMuPDF，然后稍后使用其他软件进行更改。

    在这两种情况下，您可能会遇到意外的变化，例如：

    - 注释图标或文本字体发生变化；
    - 填充颜色或线条虚线样式丢失；
    - 线条端点符号大小变化，甚至完全消失。

    这些问题可能是由于不同 PDF 处理软件对注释属性的解析方式不同所导致的。

.. tab:: 英文

    There are two scenarios:

    1. **Updating** an annotation with PyMuPDF which was created by some other software.
    2. **Creating** an annotation with PyMuPDF and later changing it with some other software.

    In both cases you may experience unintended changes, like a different annotation icon or text font, the fill color or line dashing have disappeared, line end symbols have changed their size or even have disappeared too, etc.

原因
^^^^^^

Cause

.. tab:: 中文

    不同的 PDF 维护应用程序对注释的处理方式各不相同。有些注释类型可能不受支持，或者某些细节的处理方式不同。**没有统一的标准。**

    通常，每个 PDF 应用程序都会有自己的图标（如文件附件、便笺和图章）以及一组受支持的文本字体。例如：

    * (Py-) MuPDF 仅支持 5 种基本字体用于 'FreeText' 注释：Helvetica、Times-Roman、Courier、ZapfDingbats 和 Symbol，不支持斜体或加粗变体。因此，在修改其他应用程序创建的 'FreeText' 注释时，如果原始字体不被识别，它可能会被替换为 Helvetica。

    * PyMuPDF 支持所有 PDF 文本标注类型（高亮、下划线、删除线、波浪线），但这些标注类型无法在 Adobe Acrobat Reader 中更新。

    此外，对虚线样式的支持通常也有限，可能会导致原有的虚线被替换为实线。例如：

    * PyMuPDF 完全支持所有虚线样式，而其他查看器可能仅支持有限的子集。


.. tab:: 英文

    Annotation maintenance is handled differently by each PDF maintenance application. Some annotation types may not be supported, or not be supported fully or some details may be handled in a different way than in another application. **There is no standard.**

    Almost always a PDF application also comes with its own icons (file attachments, sticky notes and stamps) and its own set of supported text fonts. For example:

    * (Py-) MuPDF only supports these 5 basic fonts for 'FreeText' annotations: Helvetica, Times-Roman, Courier, ZapfDingbats and Symbol -- no italics / no bold variations. When changing a 'FreeText' annotation created by some other app, its font will probably not be recognized nor accepted and be replaced by Helvetica.

    * PyMuPDF supports all PDF text markers (highlight, underline, strikeout, squiggly), but these types cannot be updated with Adobe Acrobat Reader.

    In most cases there also exists limited support for line dashing which causes existing dashes to be replaced by straight lines. For example:

    * PyMuPDF fully supports all line dashing forms, while other viewers only accept a limited subset.


解决方案
^^^^^^^^^^

Solutions

.. tab:: 中文

    在大多数情况下，您可能无能为力，但可以尝试以下方法：

    1. **始终使用相同的软件** 来创建和修改注释。
    2. 在使用 PyMuPDF 修改“外部”软件创建的注释时，尽量 **避免** 调用 :meth:`Annot.update`。以下方法 **无需调用它** ，因此可以尽可能保持原始外观：

    * :meth:`Annot.set_rect` （更改位置）
    * :meth:`Annot.set_flags` （更改注释行为）
    * :meth:`Annot.set_info` （更改元信息，但不修改 *content*）
    * :meth:`Annot.set_popup` （创建弹出窗口或更改其矩形区域）
    * :meth:`Annot.set_oc` （添加 / 移除对可选内容的引用）
    * :meth:`Annot.set_open` （设置注释的打开状态）
    * :meth:`Annot.update_file` （修改附件文件）

.. tab:: 英文

    Unfortunately there is not much you can do in most of these cases.

    1. Stay with the same software for **creating and changing** an annotation.
    2. When using PyMuPDF to change an "alien" annotation, try to **avoid** :meth:`Annot.update`. The following methods **can be used without it,** so that the original appearance should be maintained:

    * :meth:`Annot.set_rect` (location changes)
    * :meth:`Annot.set_flags` (annotation behaviour)
    * :meth:`Annot.set_info` (meta information, except changes to *content*)
    * :meth:`Annot.set_popup` (create popup or change its rect)
    * :meth:`Annot.set_oc` (add / remove reference to optional content information)
    * :meth:`Annot.set_open`
    * :meth:`Annot.update_file` (file attachment changes)


提取的文本缺失或不可读
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Missing or Unreadable Extracted Text

.. tab:: 中文

    很多时候，文本提取并不像您预期​​的那样：文本可能缺失，或者可能没有出现在屏幕上可见的阅读顺序中，或者包含乱码（例如？或“TOFU”符号）等。这可能是由许多不同的问题引起的。

.. tab:: 英文

    Fairly often, text extraction does not work text as you would expect: text may be missing, or may not appear in the reading sequence visible on your screen, or contain garbled characters (like a ? or a "TOFU" symbol), etc. This can be caused by a number of different problems.

问题：未提取任何文本
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Problem: no text is extracted

.. tab:: 中文

    您的 PDF 查看器确实显示文本，但您无法用光标选择它，并且文本提取没有任何效果。

.. tab:: 英文

    Your PDF viewer does display text, but you cannot select it with your cursor, and text extraction delivers nothing.

原因
^^^^^^

Cause

.. tab:: 中文

    1. 您可能正在查看 PDF 页面中嵌入的图像（例如扫描的 PDF）。
    2. PDF 创建者没有使用字体，而是通过使用小线条和曲线绘制文本来 **模拟** 文本。例如，大写字母“D”可以用直线“|”和左开半圆绘制，大写字母“o”可以用椭圆绘制，等等。

.. tab:: 英文

    1. You may be looking at an image embedded in the PDF page (e.g. a scanned PDF).
    2. The PDF creator used no font, but **simulated** text by painting it, using little lines and curves. E.g. a capital "D" could be painted by a line "|" and a left-open semi-circle, an "o" by an ellipse, and so on.

解决方案
^^^^^^^^^^

Solution

.. tab:: 中文

    使用 OCR 软件（例如 `OCRmyPDF <https://pypi.org/project/ocrmypdf/>`_ ）在可见页面下方插入隐藏文本层。生成的 PDF 应按预期运行。

.. tab:: 英文

    Use an OCR software like `OCRmyPDF <https://pypi.org/project/ocrmypdf/>`_ to insert a hidden text layer underneath the visible page. The resulting PDF should behave as expected.

问题：文本不可读
^^^^^^^^^^^^^^^^^^^^^^^^

Problem: unreadable text

.. tab:: 中文

    文本提取未按可读顺序提供文本、重复某些文本或出现乱码。

.. tab:: 英文

    Text extraction does not deliver the text in readable order, duplicates some text, or is otherwise garbled.

原因
^^^^^^

Cause

.. tab:: 中文

    1. 单个字符本身是可读的（没有“<?>”符号），但文本在 **文件中的编码** 顺序与阅读顺序不同。背后的动机可能是技术原因，或是为了保护数据免遭不必要的复制。
    2. 出现许多“<?>”符号，表明 MuPDF 无法解释这些字符。该字体可能确实不受 MuPDF 支持，或者 PDF 创建者可能使用了显示可读文本的字体，但故意混淆了原始的相应 unicode 字符。

.. tab:: 英文

    1. The single characters are readable as such (no "<?>" symbols), but the sequence in which the text is **coded in the file** deviates from the reading order. The motivation behind may be technical or protection of data against unwanted copies.
    2. Many "<?>" symbols occur, indicating MuPDF could not interpret these characters. The font may indeed be unsupported by MuPDF, or the PDF creator may haved used a font that displays readable text, but on purpose obfuscates the originating corresponding unicode character.

解决方案
^^^^^^^^

Solution

.. tab:: 中文

    1. 使用保留布局的文本提取： `python -m fitz gettext file.pdf` 。
    2. 如果其他文本提取工具也不起作用，那么唯一的解决方案就是对页面进行 OCR 处理。

.. tab:: 英文

    1. Use layout preserving text extraction: `python -m fitz gettext file.pdf`.
    2. If other text extraction tools also don't work, then the only solution again is OCRing the page.

.. include:: footer.rst
