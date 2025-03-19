
.. include:: ../header.rst

.. _pymupdf4llm


PyMuPDF4LLM
===========================================================================

.. tab:: 中文

    |PyMuPDF4LLM| 旨在让您更轻松地以 **LLM** 和 **RAG** 环境所需的格式提取 **PDF** 内容。它支持 :ref:`Markdown 提取 <extracting_as_md>` 以及 :ref:`LlamaIndex 文档输出 <extracting_as_llamaindex>`。

    .. important::

        您可以通过 :ref:`将 PyMuPDF Pro 与 PyMuPDF4LLM 结合使用 <using_pymupdf4llm_withpymupdfpro>` 扩展支持的文件类型，以包括 **Office** 文档格式 (DOC/DOCX、XLS/XLSX、PPT/PPTX、HWP/HWPX)。

.. tab:: 英文

    |PyMuPDF4LLM| is aimed to make it easier to extract **PDF** content in the format you need for **LLM** & **RAG** environments. It supports :ref:`Markdown extraction <extracting_as_md>` as well as :ref:`LlamaIndex document output <extracting_as_llamaindex>`.

    .. important::

        You can extend the supported file types to also include **Office** document formats (DOC/DOCX, XLS/XLSX, PPT/PPTX, HWP/HWPX) by :ref:`using PyMuPDF Pro with PyMuPDF4LLM <using_pymupdf4llm_withpymupdfpro>`.

功能
-------------------------------

Features

.. tab:: 中文

    - 支持多列页面
    - 支持图像和矢量图形提取（以及在 MD 文本中包含参考资料）
    - 支持页面分块输出。
    - 直接支持输出为 :ref:`LlamaIndex Documents <extracting_as_llamaindex>`。

.. tab:: 英文

    - Support for multi-column pages
    - Support for image and vector graphics extraction (and inclusion of references in the MD text)
    - Support for page chunking output.
    - Direct support for output as :ref:`LlamaIndex Documents <extracting_as_llamaindex>`.


功能
--------------------

Functionality

.. tab:: 中文

    - 此包使用 |PyMuPDF| 将文件页面转换为 **Markdown** 格式的文本。

    - 检测标准文本和表格，将其按正确的阅读顺序引入，然后一起转换为与 **GitHub** 兼容的 **Markdown** 文本。

    - 通过字体大小识别标题行，并在其前适当添加一个或多个 `#` 标签。

    - 检测粗体、斜体、等宽文本和代码块并进行相应格式化。有序和无序列表也类似。

    - 默认情况下，处理所有文档页面。如果需要，可以通过提供基于 `0` 的页码列表来指定页面子集。

.. tab:: 英文

    - This package converts the pages of a file to text in **Markdown** format using |PyMuPDF|.

    - Standard text and tables are detected, brought in the right reading sequence and then together converted to **GitHub**-compatible **Markdown** text.

    - Header lines are identified via the font size and appropriately prefixed with one or more `#` tags.

    - Bold, italic, mono-spaced text and code blocks are detected and formatted accordingly. Similar applies to ordered and unordered lists.

    - By default, all document pages are processed. If desired, a subset of pages can be specified by providing a list of `0`-based page numbers.


安装
----------------

Installation

.. tab:: 中文

    通过 **pip** 安装包：

.. tab:: 英文


    Install the package via **pip** with:


.. code-block:: bash

    pip install pymupdf4llm


.. _extracting_as_md:

将文件提取为 **Markdown**
--------------------------------------------------------------

Extracting a file as **Markdown**

.. tab:: 中文

    要在 **Markdown** 中检索文档内容，只需安装该包，然后使用几行 **Python** 代码即可获得结果。

    然后在您的 **Python** 脚本中执行以下操作：

    .. code-block:: python

        import pymupdf4llm
        md_text = pymupdf4llm.to_markdown("input.pdf")

    .. note::

        除了上述文件名字符串外，还可以提供 :ref:`PyMuPDF Document <Document>`。第二个参数可能是基于 `0` 的页码列表，例如 `[0,1]` 只会选择文档的第一页和第二页。

    如果您想存储 **Markdown** 文件，例如存储为 UTF8 编码文件，请执行以下操作：

.. tab:: 英文

    To retrieve your document content in **Markdown** simply install the package and then use a couple of lines of **Python** code to get results.



    Then in your **Python** script do:


    .. code-block:: python

        import pymupdf4llm
        md_text = pymupdf4llm.to_markdown("input.pdf")


    .. note::

        Instead of the filename string as above, one can also provide a :ref:`PyMuPDF Document <Document>`. A second parameter may be a list of `0`-based page numbers, e.g. `[0,1]` would just select the first and second pages of the document.


    If you want to store your **Markdown** file, e.g. store as a UTF8-encoded file, then do:


.. code-block:: python

    import pathlib
    pathlib.Path("output.md").write_bytes(md_text.encode())



.. _extracting_as_llamaindex:

将文件提取为 **LlamaIndex** 文档
--------------------------------------------------------------

Extracting a file as a **LlamaIndex** document

.. tab:: 中文

    |PyMuPDF4LLM| 支持直接转换为 **LLamaIndex** 文档。首先将文档转换为 **Markdown** 格式，然后返回 **LlamaIndex** 文档，如下所示：

.. tab:: 英文

    |PyMuPDF4LLM| supports direct conversion to a **LLamaIndex** document. A document is first converted into **Markdown** format and then a **LlamaIndex** document is returned as follows:



.. code-block:: python

    import pymupdf4llm
    llama_reader = pymupdf4llm.LlamaMarkdownReader()
    llama_docs = llama_reader.load_data("input.pdf")


.. _using_pymupdf4llm_withpymupdfpro:

和 |PyMuPDF Pro| 一起使用
---------------------------

Using with |PyMuPDF Pro|

.. tab:: 中文

    对于 **Office** 文档支持， |PyMuPDF4LLM| 可与 |PyMuPDF Pro| 无缝协作。假设您已安装 :doc:`../pymupdf-pro`，您将能够按预期使用 **Office** 文档：

    .. code-block:: python

        import pymupdf4llm
        import pymupdf.pro
        pymupdf.pro.unlock()
        md_text = pymupdf4llm.to_markdown("sample.doc")

    如您所见， |PyMuPDF Pro| 功能将在 |PyMuPDF4LLM| 上下文中可用！

.. tab:: 英文


    For **Office** document support, |PyMuPDF4LLM| works seamlessly with |PyMuPDF Pro|. Assuming you have :doc:`../pymupdf-pro` installed you will be able to work with **Office** documents as expected:


    .. code-block:: python

        import pymupdf4llm
        import pymupdf.pro
        pymupdf.pro.unlock()
        md_text = pymupdf4llm.to_markdown("sample.doc")


    As you can see |PyMuPDF Pro| functionality will be available within the |PyMuPDF4LLM| context!



API
-------

API

.. tab:: 中文

    见 :ref:`PyMuPDF4LLM API <pymupdf4llm-api>`.

.. tab:: 英文

    See :ref:`the PyMuPDF4LLM API <pymupdf4llm-api>`.

更多资源
-------------------

Further Resources

示例代码
~~~~~~~~~~~~~~~

Sample code

.. tab:: 中文

    - `使用 PyMuPDF 的命令行 RAG 聊天机器人 <https://github.com/pymupdf/RAG/tree/main/examples/country-capitals>`_
    - `使用 Langchain 和 PyMuPDF 的浏览器应用程序示例 <https://github.com/pymupdf/RAG/tree/main/examples/GUI>`_

.. tab:: 英文

    - `Command line RAG Chatbot with PyMuPDF <https://github.com/pymupdf/RAG/tree/main/examples/country-capitals>`_
    - `Example of a Browser Application using Langchain and PyMuPDF <https://github.com/pymupdf/RAG/tree/main/examples/GUI>`_


博客
~~~~~~~~~~~~~~

Blogs

.. tab:: 中文

    - `RAG/LLM 和 PDF: 增强文本提取 <https://artifex.com/blog/rag-llm-and-pdf-enhanced-text-extraction>`_
    - `使用 ChatGPT 和 PyMuPDF 创建 RAG 聊天机器人 <https://artifex.com/blog/creating-a-rag-chatbot-with-chatgpt-and-pymupdf>`_
    - `使用 ChatGPT API 和 PyMuPDF 构建 RAG 聊天机器人 GUI <https://artifex.com/blog/building-a-rag-chatbot-gui-with-the-chatgpt-api-and-pymupdf>`_
    - `RAG/LLM 和 PDF: 使用 PyMuPDF 转换为 Markdown 文本 <https://artifex.com/blog/rag-llm-and-pdf-conversion-to-markdown-text-with-pymupdf>`_

.. tab:: 英文

    - `RAG/LLM and PDF: Enhanced Text Extraction <https://artifex.com/blog/rag-llm-and-pdf-enhanced-text-extraction>`_
    - `Creating a RAG Chatbot with ChatGPT and PyMuPDF <https://artifex.com/blog/creating-a-rag-chatbot-with-chatgpt-and-pymupdf>`_
    - `Building a RAG Chatbot GUI with the ChatGPT API and PyMuPDF <https://artifex.com/blog/building-a-rag-chatbot-gui-with-the-chatgpt-api-and-pymupdf>`_
    - `RAG/LLM and PDF: Conversion to Markdown Text with PyMuPDF <https://artifex.com/blog/rag-llm-and-pdf-conversion-to-markdown-text-with-pymupdf>`_




.. include:: ../footer.rst
