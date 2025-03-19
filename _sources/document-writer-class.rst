.. include:: header.rst

.. _DocumentWriter:

================
DocumentWriter
================

|pdf_only_class|

.. tab:: 中文

   * v1.21.0 中新增

   该类表示一个实用工具，可输出各种 :ref:`PyMuPDF 支持的文档类型<Supported_File_Types>`。

   在 |PyMuPDF| 中，该类仅用于输出 PDF 文档，其页面由 :ref:`Story` DOM 填充。

   未来可能会使用 DocumentWriter_ 处理其他文档类型。

   ================================= ===================================================
   **方法 / 属性**                    **简要描述**
   ================================= ===================================================
   :meth:`DocumentWriter.begin_page` 开始一个新的输出页面
   :meth:`DocumentWriter.end_page`   结束当前输出页面
   :meth:`DocumentWriter.close`      刷新待处理的输出并关闭文件
   ================================= ===================================================

   **类 API**

   .. class:: DocumentWriter

      .. method:: __init__(self, path, options=None)

         创建一个文档写入对象，传入一个 Python 文件指针或文件路径。还可以传递用于保存文件的选项。

         此类还可作为 Python 上下文管理器使用。

         :arg path: 输出文件。可以是字符串文件名，也可以是任意 Python 文件指针。
         
            .. note:: 
               
               通过使用 `io.BytesIO()` 对象作为文件指针，文档写入器可以在内存中创建 PDF。随后，该 PDF 可以重新打开用于输入并进一步操作。该技术在 :ref:`Stories 配方<RecipesStories>` 中的多个示例脚本中有所应用。

         :arg str options: 指定输出 PDF 的保存选项。常见选项包括 "compress" 或 "clean"。更多可能的选项可参考 `mutool convert` CLI 工具的帮助输出。

      .. method:: begin_page(mediabox)

         启动一个给定尺寸的新输出页面。

         :arg rect_like mediabox: 指定页面大小的矩形。调用此方法后，输出操作可以向页面写入内容。

      .. method:: end_page()

         结束当前页面。该方法会刷新所有待处理的数据，并将页面追加到输出文档中。

      .. method:: close()

         关闭输出文件。调用此方法后，所有待处理的数据都会被写入。

      具体用法示例，请参考 :ref:`Story` 章节。


.. tab:: 英文

   * New in v1.21.0

   This class represents a utility which can output various :ref:`document types supported by PyMuPDF<Supported_File_Types>`.

   In |PyMuPDF| only used for outputting PDF documents whose pages are populated by :ref:`Story` DOMs.

   Using DocumentWriter_ also for other document types might happen in the future.

   ================================= ===================================================
   **Method / Attribute**            **Short Description**
   ================================= ===================================================
   :meth:`DocumentWriter.begin_page` start a new output page
   :meth:`DocumentWriter.end_page`   finish the current output page
   :meth:`DocumentWriter.close`      flush pending output and close the file
   ================================= ===================================================

   **Class API**

   .. class:: DocumentWriter
      :no-index:

      .. method:: __init__(self, path, options=None)
         :no-index:

         Create a document writer object, passing a Python file pointer or a file path. Options to use when saving the file may also be passed.

         This class can also be used as a Python context manager.

         :arg path: the output file. This may be a string file name, or any Python file pointer.
         
            .. note:: By using a `io.BytesIO()` object as file pointer, a document writer can create a PDF in memory. Subsequently, this PDF can be re-opened for input and be further manipulated. This technique is used by several example scripts in :ref:`Stories recipes<RecipesStories>`.

         :arg str options: specify saving options for the output PDF. Typical are "compress" or "clean". More possible values may be taken from help output of the `mutool convert` CLI utility.

      .. method:: begin_page(mediabox)
         :no-index:

         Start a new output page of a given dimension.

         :arg rect_like mediabox: a rectangle specifying the page size. After this method, output operations may write content to the page.

      .. method:: end_page()
         :no-index:

         Finish a page. This flushes any pending data and appends the page to the output document.

      .. method:: close()
         :no-index:

         Close the output file. This method is required for writing any pending data.

      For usage examples consult the section of :ref:`Story`.

.. include:: footer.rst
