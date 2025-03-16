.. include:: header.rst

.. _HowToOpenAFile:

==============================
打开文件
==============================

Opening Files


.. _Supported_File_Types:

支持的文件类型
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Supported File Types

.. tab:: 中文

    |PyMuPDF| 可以打开除 |PDF| 之外的其他文件。

    支持以下文件类型：

.. tab:: 英文

    |PyMuPDF| can open files other than just |PDF|.

    The following file types are supported:

.. include:: supported-files-table.rst



如何打开文件
~~~~~~~~~~~~~~~~~~~~~

How to Open a File

.. tab:: 中文

    要打开文件，可以按以下方式操作：

    .. code-block:: python

        doc = pymupdf.open("a.pdf")

    .. note:: 
        
        上述代码创建了一个 :ref:`Document`。指令 `doc = pymupdf.Document("a.pdf")` 完全相同。因此，`open` 只是一个方便的别名，您可以在相关章节找到其完整的 API 文档。


.. tab:: 英文

    To open a file, do the following:

    .. code-block:: python

        doc = pymupdf.open("a.pdf")


    .. note:: 
        
        The above creates a :ref:`Document`. The instruction `doc = pymupdf.Document("a.pdf")` does exactly the same. So, `open` is just a convenient alias  and you can find its full API documented in that chapter. 


使用 :index:`错误的文件扩展名 <pair: wrong; file extension>` 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Opening with :index:`a Wrong File Extension <pair: wrong; file extension>`

.. tab:: 中文

    如果您的文档文件扩展名不正确，您仍然可以正确地打开它。

    假设 *"some.file"* 实际上是一个 **XPS** 文件。可以按照以下方式打开它：

    .. code-block:: python

        doc = pymupdf.open("some.file", filetype="xps")

    .. note::

        |PyMuPDF| 本身不会尝试从文件内容中确定文件类型。**您** 需要以某种方式提供文件类型信息——可以通过文件扩展名隐式地提供，或者如上所示通过 `filetype` 参数显式提供。有一些纯 :title:`Python` 包，如 `filetype <https://pypi.org/project/filetype/>`_，可以帮助您完成这项工作。有关详细信息，请参考 :ref:`Document` 章节。

        如果 |PyMuPDF| 遇到一个未知或缺失扩展名的文件，它将尝试将其作为 |PDF| 打开。因此，在这种情况下无需额外的预防措施。同样，对于内存中的文档，您只需指定 `doc=pymupdf.open(stream=mem_area)` 即可将其作为 |PDF| 文档打开。

        如果您尝试打开一个不受支持的文件，|PyMuPDF| 会抛出文件数据错误。


.. tab:: 英文

    If you have a document with a wrong file extension for its type, you can still correctly open it.

    Assume that *"some.file"* is actually an **XPS**. Open it like so:

    .. code-block:: python

        doc = pymupdf.open("some.file", filetype="xps")



    .. note::

        |PyMuPDF| itself does not try to determine the file type from the file contents. **You** are responsible for supplying the file type information in some way -- either implicitly, via the file extension, or explicitly as shown with the `filetype` parameter. There are pure :title:`Python` packages like `filetype <https://pypi.org/project/filetype/>`_ that help you doing this. Also consult the :ref:`Document` chapter for a full description.

        If |PyMuPDF| encounters a file with an unknown / missing extension, it will try to open it as a |PDF|. So in these cases there is no need for additional precautions. Similarly, for memory documents, you can just specify `doc=pymupdf.open(stream=mem_area)` to open it as a |PDF| document.

        If you attempt to open an unsupported file then |PyMuPDF| will throw a file data error.




----------



打开远程文件
~~~~~~~~~~~~~~~~~~~~~~~~~~

Opening Remote Files

.. tab:: 中文

    对于服务器上的远程文件（即非本地文件），您需要将文件数据*流式传输*到 |PyMuPDF|。

    例如，按如下方式使用 `requests <https://requests.readthedocs.io/en/latest/>`_ 库：

.. tab:: 英文


    For remote files on a server (i.e. non-local files), you will need to *stream* the file data to |PyMuPDF|.

    For example use the `requests <https://requests.readthedocs.io/en/latest/>`_ library as follows:

.. code-block:: python

    import pymupdf
    import requests

    r = requests.get('https://mupdf.com/docs/mupdf_explored.pdf')
    data = r.content
    doc = pymupdf.Document(stream=data)



从云服务打开文件
""""""""""""""""""""""""""""""""""""""

Opening Files from Cloud Services

.. tab:: 中文

    有关处理典型云服务上保存的文件的更多示例，请参阅这些 `云交互代码片段 <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/cloud-interactions>`_ 。

.. tab:: 英文

    For further examples which deal with files held on typical cloud services please see these `Cloud Interactions code snippets <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/cloud-interactions>`_.



----------



以文本形式打开文件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Opening Files as Text

.. tab:: 中文

    |PyMuPDF| 具有打开任何纯文本文件作为文档的能力。要实现这一点，您需要在调用 `pymupdf.open` 函数时提供 `filetype` 参数，并将其设为 `"txt"`。

    .. code-block:: python

        doc = pymupdf.open("my_program.py", filetype="txt")

    通过这种方式，您可以打开各种文件类型，并执行典型的 **非 PDF** 特定功能，如文本搜索、文本提取和页面渲染。显然，一旦您渲染了 `txt` 内容，就可以轻松地将其保存为 |PDF|，或与其他 |PDF| 文件合并。


.. tab:: 英文


    |PyMuPDF| has the capability to open any plain text file as a document. In order to do this you should provide the `filetype` parameter for the `pymupdf.open` function as `"txt"`.

    .. code-block:: python

        doc = pymupdf.open("my_program.py", filetype="txt")


    In this way you are able to open a variety of file types and perform the typical **non-PDF** specific features like text searching, text extracting and page rendering. Obviously, once you have rendered your `txt` content, then saving as |PDF| or merging with other |PDF| files is no problem.


例子
""""""""""""""""""

Examples


打开 ``C#`` 文件
...........................

Opening a `C#` file

.. code-block:: python

    doc = pymupdf.open("MyClass.cs", filetype="txt")


打开 ``XML`` 文件
...........................

Opening an ``XML`` file
.. code-block:: python

    doc = pymupdf.open("my_data.xml", filetype="txt")


打开 ``JSON`` 文件
...........................

Opening a `JSON` file
.. code-block:: python

    doc = pymupdf.open("more_of_my_data.json", filetype="txt")


.. tab:: 中文

    等等！

    正如您所想象的，许多基于文本的文件格式都可以由 |PyMuPDF| *非常简单地打开*和 *解释*。这可以使对各种以前不可用的文件的数据分析和提取突然成为可能。

.. tab:: 英文

    And so on!

    As you can imagine many text based file formats can be *very simply opened* and *interpreted* by |PyMuPDF|. This can make data analysis and extraction for a wide range of previously unavailable files suddenly possible.









.. include:: footer.rst
