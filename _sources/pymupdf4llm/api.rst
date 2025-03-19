.. include:: ../header.rst



.. _pymupdf4llm-api:


API
===========================================================================

|PyMuPDF4LLM| API
--------------------------

The |PyMuPDF4LLM| API

.. tab:: 中文

    .. property:: version

        打印库的版本。

    .. method:: to_markdown(doc: pymupdf.Document | str, *, pages: list | range | None = None, hdr_info: Any = None, write_images: bool = False, embed_images: bool = False, dpi: int = 150, image_path="", image_format="png", image_size_limit=0.05, force_text=True, margins=(0, 50, 0, 50), page_chunks: bool = False, page_width: float = 612, page_height: float = None, table_strategy="lines_strict", graphics_limit: int = None, ignore_code: bool = False, extract_words: bool = False, show_progress: bool = True) -> str | list[dict]

        读取文件的页面，并以 |Markdown| 格式输出其文本。此过程的具体行为可以通过多个参数进行调整。请注意，**支持从 |Markdown| 文本构建页面块**。

        :arg Document,str doc: 需要处理的文件，可指定为文件路径字符串，或 |PyMuPDF| Document (通过 `pymupdf.open` 创建) 。若要使用 `pathlib.Path` 规范、Python 文件对象、内存中的文档等，**必须** 使用 |PyMuPDF| Document。

        :arg list pages: 可选参数，指定要输出的页面 (注意：使用 0-based 页码) 。如果省略，则处理所有页面。

        :arg hdr_info: 可选参数。如果要提供自定义的标题检测逻辑，可以使用此参数。它可以是一个可调用对象，或者是一个包含 `get_header_id` 方法的对象。该方法必须接受一个文本片段 (即 :meth:`~.extractDICT` 返回的 span 字典) ，以及一个关键字参数 `"page"` (其值为所属的 :ref:`Page <page>` 对象) 。它应返回一个字符串 `""` 或 1 到 6 个 `"#"` 号后跟一个空格。如果省略，将对整个文档进行扫描，以找到最常见的字体大小，并基于此推导标题级别。若要完全禁用此行为，可指定 `hdr_info=lambda s, page=None: ""` 或 `hdr_info=False`。

        :arg bool write_images: 当遇到图片或矢量图形时，将从相应页面区域创建图片，并存储在指定的文件夹中。同时，Markdown 中会生成指向这些图片的引用。此选项会导致该区域的文本不被包含在文本输出中 (但会出现在图片中) 。因此，如果文档中包含整页图片上的文本，请确保此参数设为 `False`。

        :arg bool embed_images: 类似于 `write_images`，但图片会被嵌入到 Markdown 文本中，以 base64 编码字符串的形式存储。如果使用该选项，则会忽略 `write_images` 和 `image_path`。此选项可能会显著增加 Markdown 文本的大小。

        :arg float image_size_limit: 该值必须为小于 1 的正数。当 `width / page.rect.width <= image_size_limit` 或 `height / page.rect.height <= image_size_limit` 时，图片将被忽略。例如，默认值 0.05 表示图片的宽度和高度必须大于页面宽度和高度的 5%，才能被考虑纳入。

        :arg int dpi: 指定图片的分辨率 (DPI) 。仅当 `write_images=True` 时有效。默认值为 150。

        :arg str image_path: 指定存储图片的文件夹。仅当 `write_images=True` 时有效。默认值为脚本目录的路径。

        :arg str image_format: 指定图片的格式 (扩展名) 。默认值为 `"png"`  (便携式网络图形格式) 。常见的替代格式包括 `"jpg"` 。所有可能的值详见 :ref:`支持的文件类型 <Supported_File_Types>`。

        :arg bool force_text: 生成文本输出，即使文本与图片/图形重叠。这些文本会在相应图片之后出现。如果 `write_images=True`，此参数可以设为 `False` 以避免重复输出图片上的文本。

        :arg float,list margins: 以浮点数或浮点数序列指定页面边距。只有位于边距内的对象才会被输出。

            * `margin=f` 等价于 `(f, f, f, f)`，表示 `(left, top, right, bottom)`。
            * `(top, bottom)` 等价于 `(0, top, 0, bottom)`。
            * 若要读取整个页面，请使用 `margins=0`。

        :arg bool page_chunks: 若设为 `True`，则输出将是 `Document.page_count` 个字典的列表 (每页一个字典) 。每个字典的结构如下：

            - **"metadata"** - 一个字典，包含文档的元数据 :attr:`Document.metadata`，并附加额外键 **"file_path"** (文件路径) 、**"page_count"** (文档页数) 和 **"page_number"** (基于 1 的页码) 。
            - **"toc_items"** - 该页面的目录项列表。每个项的格式为 `[lvl, title, pagenumber]`，其中 `lvl` 表示层级，`title` 为标题字符串，`pagenumber` 为基于 1 的页码。
            - **"tables"** - 该页面的表格列表。每个项是一个字典，包含键 `"bbox"` (表格在页面上的位置，格式为 `pymupdf.Rect` 元组) 、`"row_count"` (行数) 和 `"col_count"` (列数) 。
            - **"images"** - 该页面的图片列表，与 :meth:`Page.get_image_info` 方法返回值相同。
            - **"graphics"** - 该页面的矢量图形矩形列表，包含由 :meth:`Page.cluster_drawings` 方法返回的矢量图形边界框。
            - **"text"** - 以 |Markdown| 格式存储的页面内容。
            - **"words"** - 若 `extract_words=True`，则包含 `page.get_text("words")` 方法返回的单词列表。格式为 `(x0, y0, x1, y1, "wordstring", bno, lno, wno)`，其顺序与 Markdown 文本中的顺序一致，并且支持多栏文本和表格文本的正确顺序。

        :arg float page_width: 指定页面宽度。对于 PDF、XPS 等固定页面宽度的文档，此参数会被忽略。对于 **可重排** 文档 (如电子书、办公文档或纯文本文件) ，默认假设其宽度为信纸宽度 (612) ，高度为“无限大”，即整个文档被视为一页。

        :arg float page_height: 指定页面高度。仅当 `page_width` 适用时才相关。默认值 `None` 表示文档被视为单页，其宽度为 `page_width`，此时不会有 Markdown 页面分隔符 (除最后一页外) ，并且返回的 `page_chunks` 仅包含一个页面。

        :arg str table_strategy: 表格检测策略。默认值 `"lines_strict"` (忽略背景色) 。某些情况下，其他策略可能更有效，例如 `"lines"` (使用所有矢量图形对象进行检测) 。

        :arg int graphics_limit: 限制处理矢量图形元素的数量。某些科学文档或使用图形命令模拟文本的页面可能包含成千上万个矢量对象。矢量图形主要用于表格检测，分析此类页面可能导致运行时间过长。可通过 `graphics_limit=5000` 或更小的值排除问题页面。被忽略的页面将在输出文本中显示一条消息。

        :arg bool ignore_code: 若设为 `True`，则等宽字体文本不会被特殊处理，不会生成代码块。当 `extract_words=True` 时，该值默认为 `True`。

        :arg bool extract_words: 若设为 `True`，则 `page_chunks=True`，并为每个页面字典添加 `"words"` 键，其值为 `Page.get_text("words")` 返回的单词列表。

        :arg bool show_progress: 若设为 `True` (默认) ，则在转换为 Markdown 期间显示进度条，例如::

            处理 input.pdf...
            [====================                    ] (148/291)

        :returns: 返回选定页面文本的合并字符串，或包含页面信息的字典列表。

    .. method:: LlamaMarkdownReader(*args, **kwargs)

        使用 `LlamaIndex`_ 包创建一个 `pdf_markdown_reader.PDFMarkdownReader`。请注意，安装 **pymupdf4llm** 时 **不会自动安装** 此包。

        有关可能的参数详情，请参考 LlamaIndex 文档 [#f2]_。

        :raises: `NotImplementedError`: 请安装所需的 `LlamaIndex`_ 包。
        :returns: 一个 `pdf_markdown_reader.PDFMarkdownReader`，并输出消息 "Successfully imported LlamaIndex"。请注意，此方法执行需要几秒钟。有关 Markdown 读取器的使用详情，请参见下文。


    ----


    .. class:: pdf_markdown_reader.PDFMarkdownReader

        .. method:: load_data(file_path: Union[Path, str], extra_info: Optional[Dict] = None, **load_kwargs: Any) -> List[LlamaIndexDocument]

            这是当前提取 Markdown 数据时应使用的唯一方法。请务必忽略 `aload_data()` 和 `lazy_load_data()` 方法。其他方法（如 `use_doc_meta()` ）可能有意义，也可能没有。有关更多信息，请参考 LlamaIndex 文档 [#f2]_。

            在内部，该方法会执行 `to_markdown()` 。

            :returns: 一个 `LlamaIndexDocument` 文档列表——每个页面对应一个文档。


.. tab:: 英文


    .. property:: version
        :no-index:

        Prints the version of the library.

    .. method:: to_markdown(doc: pymupdf.Document | str, *, pages: list | range | None = None, hdr_info: Any = None, write_images: bool = False, embed_images: bool = False, dpi: int = 150, image_path="", image_format="png", image_size_limit=0.05, force_text=True, margins=(0, 50, 0, 50), page_chunks: bool = False, page_width: float = 612, page_height: float = None, table_strategy="lines_strict", graphics_limit: int = None, ignore_code: bool = False, extract_words: bool = False, show_progress: bool = True) -> str | list[dict]
        :no-index:

        Read the pages of the file and outputs the text of its pages in |Markdown| format. How this should happen in detail can be influenced by a number of parameters. Please note that there exists **support for building page chunks** from the |Markdown|  text.

        :arg Document,str doc: the file, to be specified either as a file path string, or as a |PyMuPDF| Document (created via `pymupdf.open`). In order to use `pathlib.Path` specifications, Python file-like objects, documents in memory etc. you **must** use a |PyMuPDF| Document.

        :arg list pages: optional, the pages to consider for output (caution: specify 0-based page numbers). If omitted all pages are processed.

        :arg hdr_info: optional. Use this if you want to provide your own header detection logic. This may be a callable or an object having a method named `get_header_id`. It must accept a text span (a span dictionary as contained in :meth:`~.extractDICT`) and a keyword parameter "page" (which is the owning :ref:`Page <page>` object). It must return a string "" or up to 6 "#" characters followed by 1 space. If omitted, a full document scan will be performed to find the most popular font sizes and derive header levels based on them. To completely avoid this behavior specify `hdr_info=lambda s, page=None: ""` or `hdr_info=False`.

        :arg bool write_images: when encountering images or vector graphics, images will be created from the respective page area and stored in the specified folder. Markdown references will be generated pointing to these images. Any text contained in these areas will not be included in the text output (but appear as part of the images). Therefore, if for instance your document has text written on full page images, make sure to set this parameter to `False`.

        :arg bool embed_images: like `write_images`, but images will be included in the markdown text as base64-encoded strings. Ignores `write_images` and `image_path` if used. This may drastically increase the size of your markdown text.

        :arg float image_size_limit: this must be a positive value less than 1. Images are ignored if `width / page.rect.width <= image_size_limit` or `height / page.rect.height <= image_size_limit`. For instance, the default value 0.05 means that to be considered for inclusion, an image's width and height must be larger than 5% of the page's width and height, respectively.

        :arg int dpi: specify the desired image resolution in dots per inch. Relevant only if `write_images=True`. Default value is 150.

        :arg str image_path: store images in this folder. Relevant if `write_images=True`. Default is the path of the script directory.

        :arg str image_format: specify the desired image format via its extension. Default is "png" (portable network graphics). Another popular format may be "jpg". Possible values are all :ref:`supported output formats <Supported_File_Types>`.

        :arg bool force_text: generate text output even when overlapping images / graphics. This text then appears after the respective image. If `write_images=True` this parameter may be `False` to suppress repetition of text on images.

        :arg float,list margins: a float or a sequence of 2 or 4 floats specifying page borders. Only objects inside the margins will be considered for output.

            * `margin=f` yields `(f, f, f, f)` for `(left, top, right, bottom)`.
            * `(top, bottom)` yields  `(0, top, 0, bottom)`.
            * To always read full pages, use `margins=0`.

        :arg bool page_chunks: if `True` the output will be a list of `Document.page_count` dictionaries (one per page). Each dictionary has the following structure:

            - **"metadata"** - a dictionary consisting of the document's metadata :attr:`Document.metadata`, enriched with additional keys **"file_path"** (the file name), **"page_count"** (number of pages in document), and **"page_number"** (1-based page number).

            - **"toc_items"** - a list of Table of Contents items pointing to this page. Each item of this list has the format `[lvl, title, pagenumber]`, where `lvl` is the hierarchy level, `title` a string and `pagenumber` as a 1-based page number.

            - **"tables"** - a list of tables on this page. Each item is a dictionary with keys "bbox", "row_count" and "col_count". Key "bbox" is a `pymupdf.Rect` in tuple format of the table's position on the page.

            - **"images"** - a list of images on the page. This a copy of page method :meth:`Page.get_image_info`.

            - **"graphics"** - a list of vector graphics rectangles on the page. This is a list of boundary boxes of clustered vector graphics as delivered by method :meth:`Page.cluster_drawings`.

            - **"text"** - page content as |Markdown| text.

            - **"words"** - if `extract_words=True` was used. This is a list of tuples `(x0, y0, x1, y1, "wordstring", bno, lno, wno)` as delivered by `page.get_text("words")`. The **sequence** of these tuples however is the same as produced in the markdown text string and thus honors multi-column text. This is also true for text in tables: words are extracted in the sequence of table row cells.

        :arg float page_width: specify a desired page width. This is ignored for documents with a fixed page width like PDF, XPS etc. **Reflowable** documents however, like e-books, office or text files have no fixed page dimensions and by default are assumed to have Letter format width (612) and an **"infinite"** page height. This means that the full document is treated as one large page.

        :arg float page_height: specify a desired page height. For relevance see the `page_width` parameter. If using the default `None`, the document will appear as one large page with a width of `page_width`. Consequently in this case, no markdown page separators will occur (except the final one), respectively only one page chunk will be returned.

        :arg str table_strategy: table detection strategy. Default is `"lines_strict"` which ignores background colors. In some occasions, other strategies may be more successful, for example `"lines"` which uses all vector graphics objects for detection.

        :arg int graphics_limit: use this to limit dealing with excess amounts of vector graphics elements. Typically, scientific documents or pages simulating text using graphics commands may contain tens of thousands of these objects. As vector graphics are used for table detection mainly, analyzing pages of this kind may result in excessive runtimes. You can exclude problematic pages via for instance `graphics_limit=5000` or even a smaller value if desired. The respective pages will then be ignored and be represented by one message line in the output text.

        :arg bool ignore_code: if `True` then mono-spaced text does not receive special formatting treatment. Code blocks will no longer be generated. This value is set to `True` if `extract_words=True` is used.

        :arg bool extract_words: a value of `True` enforces `page_chunks=True` and adds key "words" to each page dictionary. Its value is a list of words as delivered by PyMuPDF's `Page` method `get_text("words")`. The sequence of the words in this list is the same as the extracted text.

        :arg bool show_progress: a value of `True` (the default) displays a text-based progress bar as pages are being converted to Markdown. It will look similar to the following::

            Processing input.pdf...
            [====================                    ] (148/291)

        :returns: Either a string of the combined text of all selected document pages, or a list of dictionaries.

    .. method:: LlamaMarkdownReader(*args, **kwargs)
        :no-index:

        Create a `pdf_markdown_reader.PDFMarkdownReader` using the `LlamaIndex`_ package. Please note that this package will **not automatically be installed** when installing **pymupdf4llm**.

        For details on the possible arguments, please consult the LlamaIndex documentation [#f1]_.

        :raises: `NotImplementedError`: Please install required `LlamaIndex`_ package.
        :returns: a `pdf_markdown_reader.PDFMarkdownReader` and issues message "Successfully imported LlamaIndex". Please note that this method needs several seconds to execute. For details on using the markdown reader please see below.

    ----


    .. class:: pdf_markdown_reader.PDFMarkdownReader
        :no-index:

        .. method:: load_data(file_path: Union[Path, str], extra_info: Optional[Dict] = None, **load_kwargs: Any) -> List[LlamaIndexDocument]
            :no-index:

            This is the only method of the markdown reader you should currently use to extract markdown data. Please in any case ignore methods `aload_data()` and `lazy_load_data()`. Other methods like `use_doc_meta()` may or may not make sense. For more information, please consult the LlamaIndex documentation [#f1]_.

            Under the hood the method will execute `to_markdown()`.

            :returns: a list of `LlamaIndexDocument` documents - one for each page.


.. rubric:: Footnotes

.. [#f1] `LlamaIndex documentation <https://docs.llamaindex.ai/en/stable/>`_
.. [#f2] `LlamaIndex 文档 <https://docs.llamaindex.ai/en/stable/>`_




.. include:: ../footer.rst

.. _LlamaIndex: https://pypi.org/project/llama-index/



