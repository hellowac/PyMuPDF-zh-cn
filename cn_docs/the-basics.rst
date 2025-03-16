.. include:: header.rst

.. _TheBasics:


==============================
基础用法
==============================

The Basics

.. _The_Basics_Opening_Files:

打开文件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Opening a File

.. tab:: 中文

    要打开文件，请执行以下操作:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("a.pdf") # open a document


    .. note::

        **进一步**

       有关更多高级选项，请参阅 :ref:`支持的文件类型列表 <Supported_File_Types>` 和 :ref:`如何打开文件的指南 <HowToOpenAFile>`。

.. tab:: 英文


    To open a file, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("a.pdf") # open a document


    .. note::

        **Taking it further**

        See the :ref:`list of supported file types<Supported_File_Types>` and :ref:`The How to Guide on Opening Files <HowToOpenAFile>` for more advanced options.


----------


.. _The_Basics_Extracting_Text:

从 |PDF| 中提取文本
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Extract text from a |PDF|

.. tab:: 中文

    要从 |PDF| 文件中提取所有文本，请执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("a.pdf") # 打开 PDF 文档
        out = open("output.txt", "wb") # 创建文本输出文件
        for page in doc: # 遍历文档页面
            text = page.get_text().encode("utf8") # 获取纯文本（UTF-8 编码）
            out.write(text) # 写入页面文本
            out.write(bytes((12,))) # 写入页面分隔符（换页符 0x0C）
        out.close()

    当然，不仅仅是 |PDF| 可以提取文本，所有 :ref:`支持的文档文件格式 <About_Feature_Matrix>`，如 :title:`MOBI`、 :title:`EPUB`、 :title:`TXT`，都可以提取文本。

    .. note::

        **进一步提升**

        如果您的文档包含基于图像的文本内容，可以对页面使用 OCR 进行文本提取：

        .. code-block:: python

            tp = page.get_textpage_ocr()
            text = page.get_text(textpage=tp)

        还有许多示例讲解了如何从特定区域提取文本，或者如何从文档中提取表格。请参考 :ref:`文本处理指南<RecipesText>`。

        现在，您还可以 :ref:`以 Markdown 格式提取文本<rag_outputting_as_md>`。

        **API 参考**

        - :meth:`Page.get_text`


.. tab:: 英文

    To extract all the text from a |PDF| file, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("a.pdf") # open a document
        out = open("output.txt", "wb") # create a text output
        for page in doc: # iterate the document pages
            text = page.get_text().encode("utf8") # get plain text (is in UTF-8)
            out.write(text) # write text of page
            out.write(bytes((12,))) # write page delimiter (form feed 0x0C)
        out.close()

    Of course it is not just |PDF| which can have text extracted - all the :ref:`supported document file formats <About_Feature_Matrix>` such as :title:`MOBI`, :title:`EPUB`, :title:`TXT` can have their text extracted.

    .. note::

        **Taking it further**

        If your document contains image based text content the use OCR on the page for subsequent text extraction:

        .. code-block:: python

            tp = page.get_textpage_ocr()
            text = page.get_text(textpage=tp)

        There are many more examples which explain how to extract text from specific areas or how to extract tables from documents. Please refer to the :ref:`How to Guide for Text<RecipesText>`.

        You can now also :ref:`extract text in Markdown format<rag_outputting_as_md>`.

        **API reference**

        - :meth:`Page.get_text`





----------


.. _The_Basics_Extracting_Images:

从 |PDF| 中提取图像
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Extract images from a |PDF|

.. tab:: 中文

    要从 |PDF| 文件中提取所有图像，请执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # 打开 PDF 文档

        for page_index in range(len(doc)): # 遍历 PDF 页面
            page = doc[page_index] # 获取页面
            image_list = page.get_images()

            # 输出当前页面找到的图像数量
            if image_list:
                print(f"在第 {page_index} 页找到 {len(image_list)} 张图片")
            else:
                print(f"第 {page_index} 页未找到图片")

            for image_index, img in enumerate(image_list, start=1): # 遍历图像列表
                xref = img[0] # 获取图像的 XREF
                pix = pymupdf.Pixmap(doc, xref) # 创建 Pixmap 对象

                if pix.n - pix.alpha > 3: # 如果是 CMYK 模式，则先转换为 RGB
                    pix = pymupdf.Pixmap(pymupdf.csRGB, pix)

                pix.save("page_%s-image_%s.png" % (page_index, image_index)) # 以 PNG 格式保存图片
                pix = None


    .. note::

        **进一步提升**

        还有许多示例讲解了如何从特定区域提取文本，或者如何从文档中提取表格。请参考 :ref:`文本处理指南<RecipesText>`。

        **API 参考**

        - :meth:`Page.get_images`
        - :ref:`Pixmap<Pixmap>`


.. tab:: 英文

    To extract all the images from a |PDF| file, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # open a document

        for page_index in range(len(doc)): # iterate over pdf pages
            page = doc[page_index] # get the page
            image_list = page.get_images()

            # print the number of images found on the page
            if image_list:
                print(f"Found {len(image_list)} images on page {page_index}")
            else:
                print("No images found on page", page_index)

            for image_index, img in enumerate(image_list, start=1): # enumerate the image list
                xref = img[0] # get the XREF of the image
                pix = pymupdf.Pixmap(doc, xref) # create a Pixmap

                if pix.n - pix.alpha > 3: # CMYK: convert to RGB first
                    pix = pymupdf.Pixmap(pymupdf.csRGB, pix)

                pix.save("page_%s-image_%s.png" % (page_index, image_index)) # save the image as png
                pix = None



    .. note::

        **Taking it further**

        There are many more examples which explain how to extract text from specific areas or how to extract tables from documents. Please refer to the :ref:`How to Guide for Text<RecipesText>`.

        **API reference**

        - :meth:`Page.get_images`
        - :ref:`Pixmap<Pixmap>`



.. _The_Basics_Extracting_Vector_Graphics:

提取矢量图形
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Extract vector graphics

.. tab:: 中文

    要从文档页面中提取所有矢量图形，请执行以下操作：

    .. code-block:: python

        doc = pymupdf.open("some.file")
        page = doc[0]
        paths = page.get_drawings()

    这将返回一个包含页面上所有矢量绘图路径的字典。

    .. note::

        **进一步提升**

        请参考: :ref:`如何提取绘图<RecipesDrawingAndGraphics_Extract_Drawings>`。

        **API 参考**

        - :meth:`Page.get_drawings`


.. tab:: 英文

    To extract all the vector graphics from a document page, do the following:


    .. code-block:: python

        doc = pymupdf.open("some.file")
        page = doc[0]
        paths = page.get_drawings()


    This will return a dictionary of paths for any vector drawings found on the page.

    .. note::

        **Taking it further**

        Please refer to: :ref:`How to Extract Drawings<RecipesDrawingAndGraphics_Extract_Drawings>`.

        **API reference**

        - :meth:`Page.get_drawings`



----------

.. _The_Basics_Merging_PDF:
.. _merge PDF:
.. _join PDF:

合并 |PDF| 文件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Merging |PDF| files

.. tab:: 中文

    要合并 |PDF| 文件，请执行以下操作：

    .. code-block:: python

        import pymupdf

        doc_a = pymupdf.open("a.pdf") # 打开第一个文档
        doc_b = pymupdf.open("b.pdf") # 打开第二个文档

        doc_a.insert_pdf(doc_b) # 合并文档
        doc_a.save("a+b.pdf") # 以新文件名保存合并后的文档


.. tab:: 英文

    To merge |PDF| files, do the following:

    .. code-block:: python

        import pymupdf

        doc_a = pymupdf.open("a.pdf") # open the 1st document
        doc_b = pymupdf.open("b.pdf") # open the 2nd document

        doc_a.insert_pdf(doc_b) # merge the docs
        doc_a.save("a+b.pdf") # save the merged document with a new filename


将 |PDF| 文件与其他类型的文件合并
"""""""""""""""""""""""""""""""""""""""""""""""""""""

Merging |PDF| files with other types of file

.. tab:: 中文

    使用 :meth:`Document.insert_file`，您可以调用该方法将 :ref:`支持的文件<Supported_File_Types>` 与 |PDF| 合并。例如：

    .. code-block:: python

        import pymupdf

        doc_a = pymupdf.open("a.pdf") # 打开第一个文档
        doc_b = pymupdf.open("b.svg") # 打开第二个文档

        doc_a.insert_file(doc_b) # 合并文档
        doc_a.save("a+b.pdf") # 以新文件名保存合并后的文档


    .. note::

        **深入探索**

        使用 :meth:`Document.insert_pdf` 和 :meth:`Document.insert_file` 轻松合并 PDF。对于已打开的 |PDF| 文档，您可以将一个文档中的页面范围复制到另一个文档中。您可以选择插入位置、翻转页面顺序以及更改页面旋转角度。

        GUI 脚本 `join.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/join-documents/join.py>`_ 采用此方法合并文件列表，并同时合并相应的目录结构。界面如下所示：

        .. image:: images/img-pdfjoiner.*
        :scale: 60

        **API 参考**

        - :meth:`Document.insert_pdf`
        - :meth:`Document.insert_file`


.. tab:: 英文

    With :meth:`Document.insert_file` you can invoke the method to merge :ref:`supported files<Supported_File_Types>` with |PDF|. For example:

    .. code-block:: python

        import pymupdf

        doc_a = pymupdf.open("a.pdf") # open the 1st document
        doc_b = pymupdf.open("b.svg") # open the 2nd document

        doc_a.insert_file(doc_b) # merge the docs
        doc_a.save("a+b.pdf") # save the merged document with a new filename


    .. note::

        **Taking it further**

        It is easy to join PDFs with :meth:`Document.insert_pdf` & :meth:`Document.insert_file`. Given open |PDF| documents, you can copy page ranges from one to the other. You can select the point where the copied pages should be placed, you can revert the page sequence and also change page rotation.

        The GUI script `join.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/join-documents/join.py>`_ uses this method to join a list of files while also joining the respective table of contents segments. It looks like this:

        .. image:: images/img-pdfjoiner.*
        :scale: 60

        **API reference**

        - :meth:`Document.insert_pdf`
        - :meth:`Document.insert_file`


----------


使用坐标
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Working with Coordinates

.. tab:: 中文

    在使用 **PyMuPDF** 时，有一个 *数学术语* 您需要熟悉—— **“坐标”** 。请快速浏览 :ref:`Coordinates` 部分，以了解坐标系统。这将帮助您准确定位对象，并更好地理解您的文档空间。


.. tab:: 英文

    There is one *mathematical term* that you should feel comfortable with when using **PyMuPDF** -  **"coordinates"**. Please have a quick look at the :ref:`Coordinates` section to understand the coordinate system to help you with positioning objects and understand your document space.



----------

.. _The_Basics_Watermarks:

向 |PDF| 添加水印
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Adding a watermark to a |PDF|

.. tab:: 中文

    要向 |PDF| 文件添加水印，请执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("document.pdf") # 打开 PDF 文档

        for page_index in range(len(doc)): # 遍历 PDF 页
            page = doc[page_index] # 获取页面

            # 从文件插入图像水印，使其适应页面边界
            page.insert_image(page.bound(), filename="watermark.png", overlay=False)

        doc.save("watermarked-document.pdf") # 以新文件名保存水印后的文档

    .. note::

        **更进一步**

        添加水印的本质就是在每个 |PDF| 页面底部添加一个图像。您应该确保该图像具有所需的不透明度和纵横比，以使其呈现符合需求的效果。

        在上面的示例中，每次引用文件时都会创建一个新的图像。但为了提高性能（减少内存占用和文件大小），应仅引用一次此图像数据。请参考 :meth:`Page.insert_image` 的代码示例和解释，了解具体实现方式。

        **API 参考**

        - :meth:`Page.bound`
        - :meth:`Page.insert_image`


.. tab:: 英文

    To add a watermark to a |PDF| file, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("document.pdf") # open a document

        for page_index in range(len(doc)): # iterate over pdf pages
            page = doc[page_index] # get the page

            # insert an image watermark from a file name to fit the page bounds
            page.insert_image(page.bound(),filename="watermark.png", overlay=False)

        doc.save("watermarked-document.pdf") # save the document with a new filename

    .. note::

        **Taking it further**

        Adding watermarks is essentially as simple as adding an image at the base of each |PDF| page. You should ensure that the image has the required opacity and aspect ratio to make it look the way you need it to.

        In the example above a new image is created from each file reference, but to be more performant (by saving memory and file size) this image data should be referenced only once - see the code example and explanation on :meth:`Page.insert_image` for the implementation.

        **API reference**

        - :meth:`Page.bound`
        - :meth:`Page.insert_image`


----------


.. _The_Basics_Images:

向 |PDF| 添加图像
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Adding an image to a |PDF|

.. tab:: 中文

    要向 |PDF| 文件添加图像（例如 Logo），请执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("document.pdf") # 打开 PDF 文档

        for page_index in range(len(doc)): # 遍历 PDF 页
            page = doc[page_index] # 获取页面

            # 从文件插入 Logo 图像，放置在文档左上角
            page.insert_image(pymupdf.Rect(0,0,50,50), filename="my-logo.png")

        doc.save("logo-document.pdf") # 以新文件名保存文档

    .. note::

        **更进一步**

        与水印示例类似，为了提高性能，应尽可能仅引用一次图像数据。请参考 :meth:`Page.insert_image` 的代码示例和解释，了解具体实现方式。

        **API 参考**

        - :ref:`Rect<Rect>`
        - :meth:`Page.insert_image`


.. tab:: 英文

    To add an image to a |PDF| file, for example a logo, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("document.pdf") # open a document

        for page_index in range(len(doc)): # iterate over pdf pages
            page = doc[page_index] # get the page

            # insert an image logo from a file name at the top left of the document
            page.insert_image(pymupdf.Rect(0,0,50,50),filename="my-logo.png")

        doc.save("logo-document.pdf") # save the document with a new filename

    .. note::

        **Taking it further**

        As with the watermark example you should ensure to be more performant by only referencing the image once if possible - see the code example and explanation on :meth:`Page.insert_image`.

        **API reference**

        - :ref:`Rect<Rect>`
        - :meth:`Page.insert_image`


----------


.. _The_Basics_Rotating:

旋转 |PDF|
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Rotating a |PDF|

.. tab:: 中文

    要旋转 PDF 页面，请执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # 打开 PDF 文档
        page = doc[0] # 获取文档的第一页
        page.set_rotation(90) # 旋转页面 90 度
        doc.save("rotated-page-1.pdf") # 以新文件名保存文档

    .. note::

        **API 参考**

        - :meth:`Page.set_rotation`


.. tab:: 英文

    To add a rotation to a page, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # open document
        page = doc[0] # get the 1st page of the document
        page.set_rotation(90) # rotate the page
        doc.save("rotated-page-1.pdf")

    .. note::

        **API reference**

        - :meth:`Page.set_rotation`


----------

.. _The_Basics_Cropping:

裁剪 |PDF|
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Cropping a |PDF|

.. tab:: 中文

    要裁剪 PDF 页面到指定的 :ref:`Rect<Rect>` 区域，请执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # 打开 PDF 文档
        page = doc[0] # 获取文档的第一页
        page.set_cropbox(pymupdf.Rect(100, 100, 400, 400)) # 设置裁剪区域
        doc.save("cropped-page-1.pdf") # 以新文件名保存裁剪后的文档

    .. note::

        **API 参考**

        - :


.. tab:: 英文

    To crop a page to a defined :ref:`Rect<Rect>`, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # open document
        page = doc[0] # get the 1st page of the document
        page.set_cropbox(pymupdf.Rect(100, 100, 400, 400)) # set a cropbox for the page
        doc.save("cropped-page-1.pdf")

    .. note::

        **API reference**

        - :meth:`Page.set_cropbox`


----------


.. _The_Basics_Attaching_Files:

:index:`附加文件 <triple: attach;embed;file>`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

:index:`Attaching Files <triple: attach;embed;file>`

.. tab:: 中文

    要将另一个文件附加到页面中，请执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # 打开主文档
        attachment = pymupdf.open("my-attachment.pdf") # 打开要附加的文档

        page = doc[0] # 获取文档的第一页
        point = pymupdf.Point(100, 100) # 创建添加附件的位置
        attachment_data = attachment.tobytes() # 获取文档的字节数据作为缓冲区

        # 添加文件注释，指定位置、数据和文件名
        file_annotation = page.add_file_annot(point, attachment_data, "attachment.pdf")

        doc.save("document-with-attachment.pdf") # 保存包含附件的文档


    .. note::

        **进一步操作**

        在使用 :meth:`Page.add_file_annot` 添加文件时，注意 `filename` 的第三个参数应包含实际的文件扩展名。没有扩展名，附件可能无法被识别为可打开的文件。例如，如果 `filename` 只是 *"attachment"*，在查看结果 PDF 并尝试打开附件时，可能会收到错误。但如果是 *"attachment.pdf"*，PDF 查看器就能识别并成功打开附件。

        附件的默认图标是 "推针"，但可以通过设置 `icon` 参数来更改它。

        **API 参考**

        - :ref:`Point<Point>`
        - :meth:`Document.tobytes`
        - :meth:`Page.add_file_annot`


.. tab:: 英文

    To attach another file to a page, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # open main document
        attachment = pymupdf.open("my-attachment.pdf") # open document you want to attach

        page = doc[0] # get the 1st page of the document
        point = pymupdf.Point(100, 100) # create the point where you want to add the attachment
        attachment_data = attachment.tobytes() # get the document byte data as a buffer

        # add the file annotation with the point, data and the file name
        file_annotation = page.add_file_annot(point, attachment_data, "attachment.pdf")

        doc.save("document-with-attachment.pdf") # save the document


    .. note::

        **Taking it further**

        When adding the file with :meth:`Page.add_file_annot` note that the third parameter for the `filename` should include the actual file extension. Without this the attachment possibly will not be able to be recognized as being something which can be opened. For example, if the `filename` is just *"attachment"* when view the resulting PDF and attempting to open the attachment you may well get an error. However, with *"attachment.pdf"* this can be recognized and opened by PDF viewers as a valid file type.

        The default icon for the attachment is by default a "push pin", however you can change this by setting the `icon` parameter.

        **API reference**

        - :ref:`Point<Point>`
        - :meth:`Document.tobytes`
        - :meth:`Page.add_file_annot`


----------


.. _The_Basics_Embedding_Files:

:index:`嵌入文件 <triple:attach;embed;file>`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

:index:`Embedding Files <triple:attach;embed;file>`

.. tab:: 中文

    要将文件嵌入到文档中，请执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # 打开主文档
        embedded_doc = pymupdf.open("my-embed.pdf") # 打开要嵌入的文档

        embedded_data = embedded_doc.tobytes() # 获取文档的字节数据作为缓冲区

        # 使用文件名和数据进行嵌入
        doc.embfile_add("my-embedded_file.pdf", embedded_data)

        doc.save("document-with-embed.pdf") # 保存包含嵌入文件的文档

    .. note::

        **进一步操作**

        与 :ref:`附加文件<The_Basics_Attaching_Files>` 类似，使用 :meth:`Document.embfile_add` 添加文件时，请注意 `filename` 的第一个参数应包含实际的文件扩展名。

        **API 参考**

        - :meth:`Document.tobytes`
        - :meth:`Document.embfile_add`


.. tab:: 英文

    To embed a file to a document, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # open main document
        embedded_doc = pymupdf.open("my-embed.pdf") # open document you want to embed

        embedded_data = embedded_doc.tobytes() # get the document byte data as a buffer

        # embed with the file name and the data
        doc.embfile_add("my-embedded_file.pdf", embedded_data)

        doc.save("document-with-embed.pdf") # save the document

    .. note::

        **Taking it further**

        As with :ref:`attaching files<The_Basics_Attaching_Files>`, when adding the file with :meth:`Document.embfile_add` note that the first parameter for the `filename` should include the actual file extension.

        **API reference**

        - :meth:`Document.tobytes`
        - :meth:`Document.embfile_add`


----------



.. _The_Basics_Deleting_Pages:

删除页面
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Deleting Pages

.. tab:: 中文

    要从文档中删除页面，请执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # 打开文档
        doc.delete_page(0) # 删除文档中的第1页
        doc.save("test-deleted-page-one.pdf") # 保存文档

    要从文档中删除多个页面，请执行以下操作：

    .. raw:: html

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # 打开文档
        doc.delete_pages(from_page=9, to_page=14) # 删除文档中的页面范围
        doc.save("test-deleted-pages.pdf") # 保存文档


.. tab:: 英文

    To delete a page from a document, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # open a document
        doc.delete_page(0) # delete the 1st page of the document
        doc.save("test-deleted-page-one.pdf") # save the document

    To delete a multiple pages from a document, do the following:

    .. raw:: html

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # open a document
        doc.delete_pages(from_page=9, to_page=14) # delete a page range from the document
        doc.save("test-deleted-pages.pdf") # save the document


如果我删除书签或超链接引用的页面会发生什么？
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

What happens if I delete a page referred to by bookmarks or hyperlinks?

.. tab:: 中文

    - 书签（目录中的条目）将变为无效，并且将不再导航到任何页面。

    - 超链接将从包含它的页面中删除。该页面上的可见内容将不会以其他方式更改。

    .. note::

        **进一步操作**

        页面索引是从零开始的，因此要删除文档中的第10页，您可以执行以下操作 `doc.delete_page(9)`。

        同样，`doc.delete_pages(from_page=9, to_page=14)` 将删除第10到第15页（包括这两页）。

        **API参考**

        - :meth:`Document.delete_page`
        - :meth:`Document.delete_pages`


.. tab:: 英文

    - A bookmark (entry in the Table of Contents) will become inactive and will no longer navigate to any page.

    - A hyperlink will be removed from the page that contains it. The visible content on that page will not otherwise be changed in any way.

    .. note::

        **Taking it further**

        The page index is zero-based, so to delete page 10 of a document you would do the following `doc.delete_page(9)`.

        Similarly, `doc.delete_pages(from_page=9, to_page=14)` will delete pages 10 - 15 inclusive.


        **API reference**

        - :meth:`Document.delete_page`
        - :meth:`Document.delete_pages`

----------


.. _The_Basics_Rearrange_Pages:

重新排列页面
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Re-Arranging Pages

.. tab:: 中文

    要更改页面的顺序，即重新排列页面，可以执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # 打开文档
        doc.move_page(1,0) # 将文档中的第2页移到文档的开头
        doc.save("test-page-moved.pdf") # 保存文档

    .. note::

        **API参考**

        - :meth:`Document.move_page`


.. tab:: 英文

    To change the sequence of pages, i.e. re-arrange pages, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # open a document
        doc.move_page(1,0) # move the 2nd page of the document to the start of the document
        doc.save("test-page-moved.pdf") # save the document


    .. note::

        **API reference**

        - :meth:`Document.move_page`

----------



.. _The_Basics_Copying_Pages:

复制页面
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Copying Pages

.. tab:: 中文

    要复制页面，可以执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # 打开文档
        doc.copy_page(0) # 复制第1页并将其放置在文档的末尾
        doc.save("test-page-copied.pdf") # 保存文档

    .. note::

        **API参考**

        - :meth:`Document.copy_page`


.. tab:: 英文


    To copy pages, do the following:


    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # open a document
        doc.copy_page(0) # copy the 1st page and puts it at the end of the document
        doc.save("test-page-copied.pdf") # save the document

    .. note::

        **API reference**

        - :meth:`Document.copy_page`


----------

.. _The_Basics_Selecting_Pages:

选择页面
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Selecting Pages

.. tab:: 中文

    要选择页面，可以执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # 打开文档
        doc.select([0, 1]) # 选择文档的第1页和第2页
        doc.save("just-page-one-and-two.pdf") # 保存文档


    .. note::

        **进一步说明**

        使用 |PyMuPDF|，您可以轻松地复制、移动、删除或重新排列 |PDF| 的页面。直观的方法允许您按页面级别进行操作，如 :meth:`Document.copy_page` 方法。

        或者，您可以准备一个包含您想要的页面顺序的完整新页面布局，并按所需的顺序和次数重复每个页面。以下示例说明了使用 :meth:`Document.select` 可以做什么：

        .. code-block:: python

            doc.select([1, 1, 1, 5, 4, 9, 9, 9, 0, 2, 2, 2])


        现在让我们为双面打印（在不直接支持此功能的打印机上）准备一个 PDF：

        页数由 `len(doc)`（等于 `doc.page_count`）给出。以下分别表示偶数页和奇数页的页码：

        .. code-block:: python

            p_even = [p in range(doc.page_count) if p % 2 == 0]
            p_odd  = [p in range(doc.page_count) if p % 2 == 1]

        这个代码片段创建了相应的子文档，然后可以用于打印文档：

        .. code-block:: python

            doc.select(p_even) # 仅保留偶数页
            doc.save("even.pdf") # 保存“偶数”PDF
            doc.close() # 回收文件
            doc = pymupdf.open(doc.name) # 重新打开
            doc.select(p_odd) # 对奇数页执行相同操作
            doc.save("odd.pdf")


        有关更多信息，请查看此 Wiki `文章 <https://github.com/pymupdf/PyMuPDF/wiki/Rearranging-Pages-of-a-PDF>`_。

        以下示例将反转所有页面的顺序（**非常快速**：在 756 页的 :ref:`AdobeManual` 上不到一秒钟）：

        .. code-block:: python

            lastPage = doc.page_count - 1
            for i in range(lastPage):
                doc.move_page(lastPage, i) # 将当前最后一页移到前面


        这个代码片段将 PDF 自身复制，以便它将包含页面 *0, 1, ..., n, 0, 1, ..., n*（**非常快速且几乎不增加文件大小！**）：

        .. code-block:: python

            page_count = len(doc)
            for i in range(page_count):
                doc.copy_page(i) # 将此页面复制到最后一页后面


        **API参考**

        - :meth:`Document.select`


.. tab:: 英文


    To select pages, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open("test.pdf") # open a document
        doc.select([0, 1]) # select the 1st & 2nd page of the document
        doc.save("just-page-one-and-two.pdf") # save the document


    .. note::

        **Taking it further**

        With |PyMuPDF| you have all options to copy, move, delete or re-arrange the pages of a |PDF|. Intuitive methods exist that allow you to do this on a page-by-page level, like the :meth:`Document.copy_page` method.

        Or you alternatively prepare a complete new page layout in form of a :title:`Python` sequence, that contains the page numbers you want, in the sequence you want, and as many times as you want each page. The following may illustrate what can be done with :meth:`Document.select`

        .. code-block:: python

            doc.select([1, 1, 1, 5, 4, 9, 9, 9, 0, 2, 2, 2])


        Now let's prepare a PDF for double-sided printing (on a printer not directly supporting this):

        The number of pages is given by `len(doc)` (equal to `doc.page_count`). The following lists represent the even and the odd page numbers, respectively:

        .. code-block:: python

            p_even = [p in range(doc.page_count) if p % 2 == 0]
            p_odd  = [p in range(doc.page_count) if p % 2 == 1]

        This snippet creates the respective sub documents which can then be used to print the document:

        .. code-block:: python

            doc.select(p_even) # only the even pages left over
            doc.save("even.pdf") # save the "even" PDF
            doc.close() # recycle the file
            doc = pymupdf.open(doc.name) # re-open
            doc.select(p_odd) # and do the same with the odd pages
            doc.save("odd.pdf")


        For more information also have a look at this Wiki `article <https://github.com/pymupdf/PyMuPDF/wiki/Rearranging-Pages-of-a-PDF>`_.


        The following example will reverse the order of all pages (**extremely fast:** sub-second time for the 756 pages of the :ref:`AdobeManual`):

        .. code-block:: python

            lastPage = doc.page_count - 1
            for i in range(lastPage):
                doc.move_page(lastPage, i) # move current last page to the front



        This snippet duplicates the PDF with itself so that it will contain the pages *0, 1, ..., n, 0, 1, ..., n* **(extremely fast and without noticeably increasing the file size!)**:

        .. code-block:: python

            page_count = len(doc)
            for i in range(page_count):
                doc.copy_page(i) # copy this page to after last page



        **API reference**

        - :meth:`Document.select`

----------


.. _The_Basics_Adding_Blank_Pages:




添加空白页
~~~~~~~~~~~~~~~~~~~~~

Adding Blank Pages

.. tab:: 中文

    要添加一个空白页，可以执行以下操作：

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open(...) # 打开一个新的或现有的PDF文档
        page = doc.new_page(-1, # 插入点：文档末尾
                            width = 595, # 页面尺寸：A4纵向
                            height = 842)
        doc.save("doc-with-new-blank-page.pdf") # 保存文档


    .. note::

        **进一步说明**

        使用此方法创建页面时，可以选择其他预定义的纸张格式：

        .. code-block:: python

            w, h = pymupdf.paper_size("letter-l")  # 'Letter'横向
            page = doc.new_page(width = w, height = h)


        方便的函数 :meth:`paper_size` 支持选择超过40种行业标准的纸张格式。要查看它们，请检查字典 :attr:`paperSizes`。将所需的字典键传递给 :meth:`paper_size` 以获取纸张尺寸。支持大小写。如果在格式名称后附加 "-L"，则返回横向版本。

        这是一个创建包含一个空白页的 |PDF| 的三行代码。其文件大小为460字节：

        .. code-block:: python

            doc = pymupdf.open()
            doc.new_page()
            doc.save("A4.pdf")


        **API参考**

        - :meth:`Document.new_page`
        - :attr:`paperSizes`


.. tab:: 英文

    To add a blank page, do the following:

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open(...) # some new or existing PDF document
        page = doc.new_page(-1, # insertion point: end of document
                            width = 595, # page dimension: A4 portrait
                            height = 842)
        doc.save("doc-with-new-blank-page.pdf") # save the document


    .. note::

        **Taking it further**

        Use this to create the page with another pre-defined paper format:

        .. code-block:: python

            w, h = pymupdf.paper_size("letter-l")  # 'Letter' landscape
            page = doc.new_page(width = w, height = h)


        The convenience function :meth:`paper_size` knows over 40 industry standard paper formats to choose from. To see them, inspect dictionary :attr:`paperSizes`. Pass the desired dictionary key to :meth:`paper_size` to retrieve the paper dimensions. Upper and lower case is supported. If you append "-L" to the format name, the landscape version is returned.

        Here is a 3-liner that creates a |PDF|: with one empty page. Its file size is 460 bytes:

        .. code-block:: python

            doc = pymupdf.open()
            doc.new_page()
            doc.save("A4.pdf")


        **API reference**

        - :meth:`Document.new_page`
        - :attr:`paperSizes`


----------


.. _The_Basics_Inserting_Pages:

插入带有文本内容的页面
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Inserting Pages with Text Content

.. tab:: 中文

    使用 :meth:`Document.insert_page` 方法也可以插入一个新页面，并接受相同的 `width` 和 `height` 参数。此方法还允许在新页面中插入任意文本，并返回插入的行数。

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open(...)  # 打开一个新的或现有的PDF文档
        n = doc.insert_page(-1, # 默认插入位置
                            text = "The quick brown fox jumped over the lazy dog",
                            fontsize = 11,
                            width = 595,
                            height = 842,
                            fontname = "Helvetica", # 默认字体
                            fontfile = None, # 可选的字体文件名
                            color = (0, 0, 0)) # 文字颜色 (RGB)

    .. note::

        **进一步说明**

        `text` 参数可以是一个（序列）字符串（假设使用UTF-8编码）。插入将从 :ref:`Point` (50, 72) 开始，即页面顶部下方一英寸，距离页面左侧50点的位置。返回值为插入的文本行数。

        **API参考**

        - :meth:`Document.insert_page`


.. tab:: 英文

    Using the :meth:`Document.insert_page` method also inserts a new page and accepts the same `width` and `height` parameters. But it lets you also insert arbitrary text into the new page and returns the number of inserted lines.

    .. code-block:: python

        import pymupdf

        doc = pymupdf.open(...)  # some new or existing PDF document
        n = doc.insert_page(-1, # default insertion point
                            text = "The quick brown fox jumped over the lazy dog",
                            fontsize = 11,
                            width = 595,
                            height = 842,
                            fontname = "Helvetica", # default font
                            fontfile = None, # any font file name
                            color = (0, 0, 0)) # text color (RGB)

    .. note::

        **Taking it further**

        The text parameter can be a (sequence of) string (assuming UTF-8 encoding). Insertion will start at :ref:`Point` (50, 72), which is one inch below top of page and 50 points from the left. The number of inserted text lines is returned.

        **API reference**

        - :meth:`Document.insert_page`



----------



.. _The_Basics_Spliting_Single_Pages:

拆分单页
~~~~~~~~~~~~~~~~~~~~~~~~~~

Splitting Single Pages

.. tab:: 中文

    这涉及到将 |PDF| 页分割成任意的多个部分。例如，您可能有一个 |PDF| 文件，页面格式为 *Letter*，并希望以四倍放大因子打印：每一页分割成四个部分，每个部分将单独成为一个新的 |PDF| 页，格式仍为 *Letter*。

    .. code-block:: python

        import pymupdf

        src = pymupdf.open("test.pdf")
        doc = pymupdf.open()  # 创建一个空的输出PDF

        for spage in src:  # 遍历输入文档中的每一页
            r = spage.rect  # 输入页面的矩形区域
            d = pymupdf.Rect(spage.cropbox_position,  # 如果有裁剪框偏移，使用该偏移量
                        spage.cropbox_position)  # 从 (0, 0) 开始
            #--------------------------------------------------------------------------
            # 示例：将输入页面切割成 2 x 2 的部分
            #--------------------------------------------------------------------------
            r1 = r / 2  # 左上矩形区域
            r2 = r1 + (r1.width, 0, r1.width, 0)  # 右上矩形区域
            r3 = r1 + (0, r1.height, 0, r1.height)  # 左下矩形区域
            r4 = pymupdf.Rect(r1.br, r.br)  # 右下矩形区域
            rect_list = [r1, r2, r3, r4]  # 将它们放入列表中

            for rx in rect_list:  # 遍历矩形列表
                rx += d  # 加上裁剪框偏移
                page = doc.new_page(-1,  # 使用 rx 尺寸创建新的输出页面
                                width = rx.width,
                                height = rx.height)
                page.show_pdf_page(
                        page.rect,  # 填充整个新页面
                        src,  # 输入文档
                        spage.number,  # 输入页面的页码
                        clip = rx,  # 使用输入页面的哪部分
                    )

        # 完成后保存输出文件
        doc.save("poster-" + src.name,
                garbage=3,  # 去除重复对象
                deflate=True,  # 尽可能压缩文件
        )


    示例：

    .. image:: images/img-posterize.png

    .. note::

        **API参考**

        - :meth:`Page.cropbox_position`
        - :meth:`Page.show_pdf_page`


.. tab:: 英文

    This deals with splitting up pages of a |PDF| in arbitrary pieces. For example, you may have a |PDF| with *Letter* format pages which you want to print with a magnification factor of four: each page is split up in 4 pieces which each going to a separate |PDF| page in *Letter* format again.



    .. code-block:: python

        import pymupdf

        src = pymupdf.open("test.pdf")
        doc = pymupdf.open()  # empty output PDF

        for spage in src:  # for each page in input
            r = spage.rect  # input page rectangle
            d = pymupdf.Rect(spage.cropbox_position,  # CropBox displacement if not
                        spage.cropbox_position)  # starting at (0, 0)
            #--------------------------------------------------------------------------
            # example: cut input page into 2 x 2 parts
            #--------------------------------------------------------------------------
            r1 = r / 2  # top left rect
            r2 = r1 + (r1.width, 0, r1.width, 0)  # top right rect
            r3 = r1 + (0, r1.height, 0, r1.height)  # bottom left rect
            r4 = pymupdf.Rect(r1.br, r.br)  # bottom right rect
            rect_list = [r1, r2, r3, r4]  # put them in a list

            for rx in rect_list:  # run thru rect list
                rx += d  # add the CropBox displacement
                page = doc.new_page(-1,  # new output page with rx dimensions
                                width = rx.width,
                                height = rx.height)
                page.show_pdf_page(
                        page.rect,  # fill all new page with the image
                        src,  # input document
                        spage.number,  # input page number
                        clip = rx,  # which part to use of input page
                    )

        # that's it, save output file
        doc.save("poster-" + src.name,
                garbage=3,  # eliminate duplicate objects
                deflate=True,  # compress stuff where possible
        )


    Example:

    .. image:: images/img-posterize.png

    .. note::

        **API reference**

        - :meth:`Page.cropbox_position`
        - :meth:`Page.show_pdf_page`


--------------------------


.. _The_Basics_Combining_Single_Pages:


合并单页
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Combining Single Pages

.. tab:: 中文

    这涉及到将 |PDF| 页面合并形成一个新的 |PDF|，其中每一页合并了两个或四个原始页面（也叫“2-up”，“4-up”等）。这可以用于创建小册子或缩略图式的概览。

    .. code-block:: python

        import pymupdf

        src = pymupdf.open("test.pdf")
        doc = pymupdf.open()  # 创建一个空的输出PDF

        width, height = pymupdf.paper_size("a4")  # A4 竖版输出页面格式
        r = pymupdf.Rect(0, 0, width, height)

        # 定义每页的 4 个矩形区域
        r1 = r / 2  # 左上矩形区域
        r2 = r1 + (r1.width, 0, r1.width, 0)  # 右上矩形区域
        r3 = r1 + (0, r1.height, 0, r1.height)  # 左下矩形区域
        r4 = pymupdf.Rect(r1.br, r.br)  # 右下矩形区域

        # 将它们放入一个列表中
        r_tab = [r1, r2, r3, r4]

        # 现在将输入页面复制到输出文档
        for spage in src:
            if spage.number % 4 == 0:  # 创建新的输出页面
                page = doc.new_page(-1,
                            width = width,
                            height = height)
            # 将输入页面插入到正确的矩形区域
            page.show_pdf_page(r_tab[spage.number % 4],  # 选择输出矩形
                            src,  # 输入文档
                            spage.number)  # 输入页面的页码

        # 使用垃圾回收和压缩保存新文件
        doc.save("4up.pdf", garbage=3, deflate=True)


    示例：

    .. image:: images/img-4up.png


    .. note::

        **API参考**

        - :meth:`Page.cropbox_position`
        - :meth:`Page.show_pdf_page`


.. tab:: 英文

    This deals with joining |PDF| pages to form a new |PDF| with pages each combining two or four original ones (also called "2-up", "4-up", etc.). This could be used to create booklets or thumbnail-like overviews.


    .. code-block:: python

        import pymupdf

        src = pymupdf.open("test.pdf")
        doc = pymupdf.open()  # empty output PDF

        width, height = pymupdf.paper_size("a4")  # A4 portrait output page format
        r = pymupdf.Rect(0, 0, width, height)

        # define the 4 rectangles per page
        r1 = r / 2  # top left rect
        r2 = r1 + (r1.width, 0, r1.width, 0)  # top right
        r3 = r1 + (0, r1.height, 0, r1.height)  # bottom left
        r4 = pymupdf.Rect(r1.br, r.br)  # bottom right

        # put them in a list
        r_tab = [r1, r2, r3, r4]

        # now copy input pages to output
        for spage in src:
            if spage.number % 4 == 0:  # create new output page
                page = doc.new_page(-1,
                            width = width,
                            height = height)
            # insert input page into the correct rectangle
            page.show_pdf_page(r_tab[spage.number % 4],  # select output rect
                            src,  # input document
                            spage.number)  # input page number

        # by all means, save new file using garbage collection and compression
        doc.save("4up.pdf", garbage=3, deflate=True)


    Example:

    .. image:: images/img-4up.png


    .. note::

        **API reference**

        - :meth:`Page.cropbox_position`
        - :meth:`Page.show_pdf_page`

--------------------------


.. _The_Basics_Encryption_and_Decryption:


|PDF| 加密和解密
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|PDF| Encryption & Decryption

.. tab:: 中文

    从版本 1.16.0 开始，|PDF| 解密和加密（使用密码）得到了完全支持。您可以执行以下操作：

    * 检查文档是否受密码保护 /（仍然）加密 (:attr:`Document.needs_pass`, :attr:`Document.is_encrypted`)。
    * 获取对文档的访问授权 (:meth:`Document.authenticate`)。
    * 使用 :meth:`Document.save` 或 :meth:`Document.write` 设置 PDF 文件的加密详情：

        - 解密或加密内容
        - 设置密码
        - 设置加密方法
        - 设置权限详情

    .. note:: 一个 PDF 文档可能有两个不同的密码：

    * **所有者密码** 提供完全访问权限，包括更改密码、加密方法或权限详情。
    * **用户密码** 提供根据已设置的权限详情访问文档内容。如果存在，在查看器中打开 |PDF| 时需要提供该密码。

    方法 :meth:`Document.authenticate` 将自动根据使用的密码建立访问权限。

    以下代码片段创建一个新的 |PDF| 并使用不同的用户和所有者密码进行加密。权限被授予打印、复制和注释，但使用用户密码的人员无法进行更改。

    .. code-block:: python

        import pymupdf

        text = "some secret information" # 保持该数据为秘密
        perm = int(
            pymupdf.PDF_PERM_ACCESSIBILITY # 始终使用此选项
            | pymupdf.PDF_PERM_PRINT # 允许打印
            | pymupdf.PDF_PERM_COPY # 允许复制
            | pymupdf.PDF_PERM_ANNOTATE # 允许注释
        )
        owner_pass = "owner" # 所有者密码
        user_pass = "user" # 用户密码
        encrypt_meth = pymupdf.PDF_ENCRYPT_AES_256 # 最强加密算法
        doc = pymupdf.open() # 空的 PDF
        page = doc.new_page() # 空白页面
        page.insert_text((50, 72), text) # 插入数据
        doc.save(
            "secret.pdf",
            encryption=encrypt_meth, # 设置加密方法
            owner_pw=owner_pass, # 设置所有者密码
            user_pw=user_pass, # 设置用户密码
            permissions=perm, # 设置权限
        )

    .. note::

        **进一步说明**

        使用某些查看器（如 Nitro Reader 5）打开此文档将反映这些设置：

        .. image:: images/img-encrypting.*

        **解密** 将在保存时自动发生，如之前一样，如果没有提供加密参数。

        要 **保持 PDF 的加密方法**，请使用 `encryption=pymupdf.PDF_ENCRYPT_KEEP` 来保存。如果 `doc.can_save_incrementally() == True`，也可以进行增量保存。

        要 **更改加密方法**，请指定上述选项的完整范围（`encryption`、`owner_pw`、`user_pw`、`permissions`）。在这种情况下，**不可能进行增量保存**。

        **API 参考**

        - :meth:`Document.save`


.. tab:: 英文


    Starting with version 1.16.0, |PDF| decryption and encryption (using passwords) are fully supported. You can do the following:

    * Check whether a document is password protected / (still) encrypted (:attr:`Document.needs_pass`, :attr:`Document.is_encrypted`).
    * Gain access authorization to a document (:meth:`Document.authenticate`).
    * Set encryption details for PDF files using :meth:`Document.save` or :meth:`Document.write` and

        - decrypt or encrypt the content
        - set password(s)
        - set the encryption method
        - set permission details

    .. note:: A PDF document may have two different passwords:

    * The **owner password** provides full access rights, including changing passwords, encryption method, or permission detail.
    * The **user password** provides access to document content according to the established permission details. If present, opening the |PDF| in a viewer will require providing it.

    Method :meth:`Document.authenticate` will automatically establish access rights according to the password used.

    The following snippet creates a new |PDF| and encrypts it with separate user and owner passwords. Permissions are granted to print, copy and annotate, but no changes are allowed to someone authenticating with the user password.


    .. code-block:: python

        import pymupdf

        text = "some secret information" # keep this data secret
        perm = int(
            pymupdf.PDF_PERM_ACCESSIBILITY # always use this
            | pymupdf.PDF_PERM_PRINT # permit printing
            | pymupdf.PDF_PERM_COPY # permit copying
            | pymupdf.PDF_PERM_ANNOTATE # permit annotations
        )
        owner_pass = "owner" # owner password
        user_pass = "user" # user password
        encrypt_meth = pymupdf.PDF_ENCRYPT_AES_256 # strongest algorithm
        doc = pymupdf.open() # empty pdf
        page = doc.new_page() # empty page
        page.insert_text((50, 72), text) # insert the data
        doc.save(
            "secret.pdf",
            encryption=encrypt_meth, # set the encryption method
            owner_pw=owner_pass, # set the owner password
            user_pw=user_pass, # set the user password
            permissions=perm, # set permissions
        )

    .. note::

        **Taking it further**

        Opening this document with some viewer (Nitro Reader 5) reflects these settings:

        .. image:: images/img-encrypting.*

        **Decrypting** will automatically happen on save as before when no encryption parameters are provided.

        To **keep the encryption method** of a PDF save it using `encryption=pymupdf.PDF_ENCRYPT_KEEP`. If `doc.can_save_incrementally() == True`, an incremental save is also possible.

        To **change the encryption method** specify the full range of options above (`encryption`, `owner_pw`, `user_pw`, `permissions`). An incremental save is **not possible** in this case.

        **API reference**

        - :meth:`Document.save`

--------------------------

.. tab:: 中文



.. tab:: 英文



.. _The_Basics_Extracting_Tables:

从 :title:`页面` 中提取表格
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Extracting Tables from a :title:`Page`

.. tab:: 中文

    可以从任何文档 :ref:`Page` 中查找和提取表格。

    .. code-block:: python

        import pymupdf
        from pprint import pprint

        doc = pymupdf.open("test.pdf") # 打开文档
        page = doc[0] # 获取文档的第一页
        tabs = page.find_tables() # 定位并提取页面上的表格
        print(f"{len(tabs.tables)} found on {page}") # 显示找到的表格数量

        if tabs.tables:  # 至少找到一个表格？
        pprint(tabs[0].extract())  # 打印第一个表格的内容

    .. note::

        **API 参考**

        - :meth:`Page.find_tables`


    .. important::

        还有 `pdf2docx extract tables method`_，如果你更喜欢的话，它也可以提取表格。


.. tab:: 英文

    Tables can be found and extracted from any document :ref:`Page`.

    .. code-block:: python

        import pymupdf
        from pprint import pprint

        doc = pymupdf.open("test.pdf") # open document
        page = doc[0] # get the 1st page of the document
        tabs = page.find_tables() # locate and extract any tables on page
        print(f"{len(tabs.tables)} found on {page}") # display number of found tables

        if tabs.tables:  # at least one table found?
        pprint(tabs[0].extract())  # print content of first table

    .. note::

        **API reference**

        - :meth:`Page.find_tables`


    .. important::

        There is also the `pdf2docx extract tables method`_ which is capable of table extraction if you prefer.


--------------------------


.. _The_Basics_Get_Page_Links:

获取页面链接
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Getting Page Links

.. tab:: 中文

    可以从 :ref:`Page` 中提取链接，返回 :ref:`Link` 对象。

    .. code-block:: python

        import pymupdf

        for page in doc: # 遍历文档页面
            link = page.first_link  # 一个 `Link` 对象或 `None`

            while link: # 遍历页面上的链接
                # 对链接进行操作，然后：
                link = link.next # 获取下一个链接，最后一个链接的 `next` 为 `None`

    .. note::

        **API 参考**

        - :meth:`Page.first_link`


.. tab:: 英文

    Links can be extracted from a :ref:`Page` to return :ref:`Link` objects.


    .. code-block:: python

        import pymupdf

        for page in doc: # iterate the document pages
            link = page.first_link  # a `Link` object or `None`

            while link: # iterate over the links on page
                # do something with the link, then:
                link = link.next # get next link, last one has `None` in its `next`

    .. note::

        **API reference**

        - :meth:`Page.first_link`


-----------------------------


.. _The_Basics_Get_All_Annotations:

从文档中获取所有注释
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Getting All Annotations from a Document

.. tab:: 中文

    页面上的注释 (:ref:`Annot`) 可以通过 `page.annots()` 方法获取。

    .. code-block:: python

        import pymupdf

        for page in doc:
            for annot in page.annots():
                print(f'页面 {page.number} 上的注释，类型: {annot.type} 和矩形区域: {annot.rect}')


    .. note::

        **API 参考**

        - :meth:`Page.annots`


.. tab:: 英文

    Annotations (:ref:`Annot`) on pages can be retrieved with the `page.annots()` method.

    .. code-block:: python

        import pymupdf

        for page in doc:
            for annot in page.annots():
                print(f'Annotation on page: {page.number} with type: {annot.type} and rect: {annot.rect}')


    .. note::

        **API reference**

        - :meth:`Page.annots`


--------------------------



.. _The_Basics_Redacting:

从 **PDF** 中删除内容
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Redacting content from a **PDF**


.. tab:: 中文

    删除标记是特殊类型的注释，可以标记在文档页面上，表示页面中应安全删除的区域。在标记区域后，该区域将被标记为 *删除标记* ，一旦删除标记被*应用*，内容将被安全删除。

    例如，如果我们想删除文档中所有“Jane Doe”出现的实例，可以按如下方式操作：

    .. code-block:: python

        import pymupdf

        # 打开PDF文档
        doc = pymupdf.open('test.pdf')

        # 遍历文档中的每一页
        for page in doc:
            # 查找当前页中所有“Jane Doe”的实例
            instances = page.search_for("Jane Doe")

            # 删除当前页中每个“Jane Doe”的实例
            for inst in instances:
                page.add_redact_annot(inst)

            # 对当前页应用删除标记
            page.apply_redactions()

        # 保存修改后的文档
        doc.save('redacted_document.pdf')

        # 关闭文档
        doc.close()


    另一个例子是对页面的某个区域进行删除标记，但不删除该区域内的任何线条艺术（即矢量图形），通过设置参数标志，如下所示：

    .. code-block:: python

        import pymupdf

        # 打开PDF文档
        doc = pymupdf.open('test.pdf')

        # 获取第一页
        page = doc[0]

        # 添加一个要删除标记的区域
        rect = [0,0,200,200]

        # 添加删除标记注释，使用红色填充
        page.add_redact_annot(rect, fill=(1,0,0))

        # 对当前页应用删除标记，但忽略矢量图形
        page.apply_redactions(graphics=0)

        # 保存修改后的文档
        doc.save('redactied_document.pdf')

        # 关闭文档
        doc.close()


    .. warning::

        一旦保存了删除标记版本的文档，**PDF**中的删除内容将*无法恢复*。因此，文档中的删除区域将完全移除该区域的文本和图形内容。


    .. note::

        **进一步了解**

        有几个选项可以在页面上创建和应用删除标记，关于这些选项的完整API详细信息以及控制这些选项的参数，请参考API文档。

        **API参考**

        - :meth:`Page.add_redact_annot`

        - :meth:`Page.apply_redactions`


.. tab:: 英文

    Redactions are special types of annotations which can be marked onto a document page to denote an area on the page which should be securely removed. After marking an area with a rectangle then this area will be marked for *redaction*, once the redaction is *applied* then the content is securely removed.

    For example if we wanted to redact all instances of the name "Jane Doe" from a document we could do the following:

    .. code-block:: python

        import pymupdf

        # Open the PDF document
        doc = pymupdf.open('test.pdf')

        # Iterate over each page of the document
        for page in doc:
            # Find all instances of "Jane Doe" on the current page
            instances = page.search_for("Jane Doe")

            # Redact each instance of "Jane Doe" on the current page
            for inst in instances:
                page.add_redact_annot(inst)

            # Apply the redactions to the current page
            page.apply_redactions()

        # Save the modified document
        doc.save('redacted_document.pdf')

        # Close the document
        doc.close()


    Another example could be redacting an area of a page, but not to redact any line art (i.e. vector graphics) within the defined area, by setting a parameter flag as follows:


    .. code-block:: python

        import pymupdf

        # Open the PDF document
        doc = pymupdf.open('test.pdf')

        # Get the first page
        page = doc[0]

        # Add an area to redact
        rect = [0,0,200,200]

        # Add a redacction annotation which will have a red fill color
        page.add_redact_annot(rect, fill=(1,0,0))

        # Apply the redactions to the current page, but ignore vector graphics
        page.apply_redactions(graphics=0)

        # Save the modified document
        doc.save('redactied_document.pdf')

        # Close the document
        doc.close()


    .. warning::

        Once a redacted version of a document is saved then the redacted content in the **PDF** is *irretrievable*. Thus, a redacted area in a document removes text and graphics completely from that area.


    .. note::

        **Taking it further**

        The are a few options for creating and applying redactions to a page, for the full API details to understand the parameters to control these options refer to the API reference.

        **API reference**

        - :meth:`Page.add_redact_annot`

        - :meth:`Page.apply_redactions`


--------------------------



.. _The Basics_Coverting_PDF_Documents:

转换 PDF 文档
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Converting PDF Documents

.. tab:: 中文

    我们推荐使用 **PyMuPDF** 和 **python-docx** 库的 pdf2docx_ 库来提供从 **PDF** 到 **DOCX** 格式的简单文档转换。

.. tab:: 英文

    We recommend the pdf2docx_ library which uses **PyMuPDF** and the **python-docx** library to provide simple document conversion from **PDF** to **DOCX** format.




.. include:: footer.rst
