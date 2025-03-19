
.. include:: header.rst


PyMuPDF、LLM 和 RAG
============================

PyMuPDF, LLM & RAG

.. tab:: 中文

    整合 |PyMuPDF| 到您的 :title:`大型语言模型 (LLM)` 框架及整体 :title:`RAG (检索增强生成)` 解决方案中，是提供文档数据的最快速、最可靠的方法。

    目前已有一些知名的 :title:`LLM` 解决方案与 |PyMuPDF| 进行了集成 —— 这是一个快速发展的领域，因此如果您发现了更多相关案例，请告诉我们！

    如果您需要导出为 :title:`Markdown` 或从文件中获取 :title:`LlamaIndex` 文档：

    .. raw:: html

        <button id="pymupdf4llmButton" class="cta orange" style="text-transform: none;" onclick="window.location='pymupdf4llm/'">尝试 PyMuPDF4LLM</button>
        <p></p>


.. tab:: 英文


    Integrating |PyMuPDF| into your :title:`Large Language Model (LLM)` framework and overall :title:`RAG (Retrieval-Augmented Generation`) solution provides the fastest and most reliable way to deliver document data.

    There are a few well known :title:`LLM` solutions which have their own interfaces with |PyMuPDF| - it is a fast growing area, so please let us know if you discover any more!

    If you need to export to :title:`Markdown` or obtain a :title:`LlamaIndex` Document from a file:

    .. raw:: html

        <button id="pymupdf4llmButton" class="cta orange" style="text-transform: none;" onclick="window.location='pymupdf4llm/'">Try PyMuPDF4LLM</button>
        <p></p>


与 :title:`LangChain` 集成
-------------------------------------

Integration with :title:`LangChain`

.. tab:: 中文

    可以直接使用专门的加载器，与 :title:`LangChain` 进行集成，方法如下：

    .. code-block:: python

        from langchain_community.document_loaders import PyMuPDFLoader
        loader = PyMuPDFLoader("example.pdf")
        data = loader.load()

    完整详情请参阅 `LangChain 使用 PyMuPDF <https://python.langchain.com/docs/modules/data_connection/document_loaders/pdf/#using-pymupdf>`_ 。


.. tab:: 英文

    It is simple to integrate directly with :title:`LangChain` by using their dedicated loader as follows:


    .. code-block:: python

        from langchain_community.document_loaders import PyMuPDFLoader
        loader = PyMuPDFLoader("example.pdf")
        data = loader.load()


    See `LangChain Using PyMuPDF <https://python.langchain.com/docs/modules/data_connection/document_loaders/pdf/#using-pymupdf>`_ for full details.


与 :title:`LlamaIndex` 集成
---------------------------------------

Integration with :title:`LlamaIndex`

.. tab:: 中文

    使用专门的 `PyMuPDFReader` 组件，从 :title:`LlamaIndex` 🦙 进行文档加载管理。

    .. code-block:: python

        from llama_index.readers.file import PyMuPDFReader
        loader = PyMuPDFReader()
        documents = loader.load(file_path="example.pdf")

    详情请参阅 `从零开始构建 RAG <https://docs.llamaindex.ai/en/stable/examples/low_level/oss_ingestion_retrieval>`_ 。


.. tab:: 英文


    Use the dedicated `PyMuPDFReader` from :title:`LlamaIndex` 🦙 to manage your document loading.

    .. code-block:: python

        from llama_index.readers.file import PyMuPDFReader
        loader = PyMuPDFReader()
        documents = loader.load(file_path="example.pdf")

    See `Building RAG from Scratch <https://docs.llamaindex.ai/en/stable/examples/low_level/oss_ingestion_retrieval>`_ for more.


准备分块数据
-----------------------------

Preparing Data for Chunking

.. tab:: 中文

    数据分块（Chunking）对于向 :title:`LLM` 提供上下文至关重要。 现在 |PyMuPDF| 已支持 :title:`Markdown` 输出，这意味着 `三级分块 <https://medium.com/@anuragmishra_27746/five-levels-of-chunking-strategies-in-rag-notes-from-gregs-video-7b735895694d#b123>`_ 也得到了支持。


.. tab:: 英文

    Chunking (or splitting) data is essential to give context to your :title:`LLM` data and with :title:`Markdown` output now supported by |PyMuPDF| this means that `Level 3 chunking <https://medium.com/@anuragmishra_27746/five-levels-of-chunking-strategies-in-rag-notes-from-gregs-video-7b735895694d#b123>`_ is supported.



.. _rag_outputting_as_md:

输出为 :title:`Markdown`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Outputting as :title:`Markdown`

.. tab:: 中文

    为了将文档导出为 :title:`Markdown` 格式，您需要使用一个单独的辅助工具。软件包 :doc:`pymupdf4llm/index` 是 |PyMuPDF| 函数的高级封装，它可以为每一页输出标准文本和表格文本，并以 Markdown 格式整合整个文档的内容：

    .. code-block:: python

        # 将文档转换为 Markdown
        import pymupdf4llm
        md_text = pymupdf4llm.to_markdown("input.pdf")

        # 以 UTF-8 编码将文本写入文件
        import pathlib
        pathlib.Path("output.md").write_bytes(md_text.encode())

    更多信息请参考： :doc:`pymupdf4llm/index`。


.. tab:: 英文

    In order to export your document in :title:`Markdown` format you will need a separate helper. Package :doc:`pymupdf4llm/index` is a high-level wrapper of |PyMuPDF| functions which for each page outputs standard and table text in an integrated Markdown-formatted string across all document pages:


    .. code-block:: python

        # convert the document to markdown
        import pymupdf4llm
        md_text = pymupdf4llm.to_markdown("input.pdf")

        # Write the text to some file in UTF8-encoding
        import pathlib
        pathlib.Path("output.md").write_bytes(md_text.encode())


    For further information please refer to: :doc:`pymupdf4llm/index`.


如何使用 :title:`Markdown` 输出
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to use :title:`Markdown` output

.. tab:: 中文

    一旦您的数据转换为 :title:`Markdown` 格式，便可以对其进行分块（chunking）并提供给 :title:`LLM`。例如，如果使用 :title:`LangChain`，可以执行以下操作：

    .. code-block:: python

        import pymupdf4llm
        from langchain.text_splitter import MarkdownTextSplitter

        # 获取 Markdown 格式的文本
        md_text = pymupdf4llm.to_markdown("input.pdf")  # 获取所有页面的 Markdown 内容

        splitter = MarkdownTextSplitter(chunk_size=40, chunk_overlap=0)

        splitter.create_documents([md_text])

    更多信息请参考 `5 Levels of Text Splitting <https://github.com/FullStackRetrieval-com/RetrievalTutorials/blob/main/tutorials/LevelsOfTextSplitting/5_Levels_Of_Text_Splitting.ipynb>`_。


.. tab:: 英文

    Once you have your data in :title:`Markdown` format you are ready to chunk/split it and supply it to your :title:`LLM`, for example, if this is :title:`LangChain` then do the following:

    .. code-block:: python

        import pymupdf4llm
        from langchain.text_splitter import MarkdownTextSplitter

        # Get the MD text
        md_text = pymupdf4llm.to_markdown("input.pdf")  # get markdown for all pages

        splitter = MarkdownTextSplitter(chunk_size=40, chunk_overlap=0)

        splitter.create_documents([md_text])



    For more see `5 Levels of Text Splitting <https://github.com/FullStackRetrieval-com/RetrievalTutorials/blob/main/tutorials/LevelsOfTextSplitting/5_Levels_Of_Text_Splitting.ipynb>`_


相关博客
--------------------

Related Blogs

.. tab:: 中文

    要了解更多关于 |PyMuPDF|、:title:`LLM` 和 :title:`RAG` 的信息，请查看我们的博客，其中包含实现方案和教程。


.. tab:: 英文

    To find out more about |PyMuPDF|, :title:`LLM` & :title:`RAG` check out our blogs for implementations & tutorials.


提取文本的方法
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Methodologies to Extract Text

.. tab:: 中文

    - `增强的文本提取 <https://artifex.com/blog/rag-llm-and-pdf-enhanced-text-extraction>`_
    - `使用 PyMuPDF 将 PDF 转换为 Markdown 文本 <https://artifex.com/blog/rag-llm-and-pdf-conversion-to-markdown-text-with-pymupdf>`_


.. tab:: 英文

    - `Enhanced Text Extraction <https://artifex.com/blog/rag-llm-and-pdf-enhanced-text-extraction>`_
    - `Conversion to Markdown Text with PyMuPDF <https://artifex.com/blog/rag-llm-and-pdf-conversion-to-markdown-text-with-pymupdf>`_



创建聊天机器人来讨论您的文档
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a Chatbot to discuss your documents

.. tab:: 中文

    - `创建一个简单的命令行聊天机器人 <https://artifex.com/blog/creating-a-rag-chatbot-with-chatgpt-and-pymupdf>`_
    - `构建一个聊天机器人 GUI <https://artifex.com/blog/building-a-rag-chatbot-gui-with-the-chatgpt-api-and-pymupdf>`_


.. tab:: 英文

    - `Make a simple command line Chatbot <https://artifex.com/blog/creating-a-rag-chatbot-with-chatgpt-and-pymupdf>`_
    - `Make a Chatbot GUI <https://artifex.com/blog/building-a-rag-chatbot-gui-with-the-chatgpt-api-and-pymupdf>`_


.. include:: footer.rst