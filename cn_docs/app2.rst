.. include:: header.rst

.. _Appendix2:

================================================
附录 2：嵌入式文件的注意事项
================================================

Appendix 2: Considerations on Embedded Files

.. tab:: 中文

    本章提供了 PyMuPDF 中嵌入文件支持的一些背景信息。

.. tab:: 英文

    This chapter provides some background on embedded files support in PyMuPDF.

常规
----------

General

.. tab:: 中文

    从版本 1.4 开始，PDF 支持将任意文件作为嵌入文件流（"Embedded File Streams"）嵌入 PDF 文档文件中（参见 :ref:`AdobeManual` 第 103 页的 "7.11.4 嵌入文件流" 章节）。

    在许多方面，这与 ZIP 文件或 MS Windows 中的 OLE 技术概念相似。然而，PDF 嵌入文件并不支持像 ZIP 格式那样的目录结构。一个嵌入的文件本身也可以包含嵌入的文件。

    这一概念的优势在于，嵌入的文件受 PDF 文件的保护范围控制，享有 PDF 的权限/密码保护和完整性保障：所有 PDF 可能引用的或依赖的所有数据可以捆绑在一起，形成一个单一且一致的信息单元。

    除了嵌入文件外，PDF 1.7 还增加了对 **集合** （collections）的支持。这是一种存储和展示嵌入文件元信息的高级方式（即任意且可扩展的属性）。

.. tab:: 英文

    Starting with version 1.4, PDF supports embedding arbitrary files as part ("Embedded File Streams") of a PDF document file (see chapter "7.11.4
    Embedded File Streams", pp. 103 of the :ref:`AdobeManual`).

    In many aspects, this is comparable to concepts also found in ZIP files or the OLE technique in MS Windows. PDF embedded files do, however, *not* support directory structures as does the ZIP format. An embedded file can in turn contain embedded files itself.

    Advantages of this concept are that embedded files are under the PDF umbrella, benefitting from its permissions / password protection and integrity aspects: all data, which a PDF may reference or even may be dependent on, can be bundled into it and so form a single, consistent unit of information.

    In addition to embedded files, PDF 1.7 adds *collections* to its support range. This is an advanced way of storing and presenting meta information (i.e. arbitrary and extensible properties) of embedded files.

MuPDF 支持
--------------

MuPDF Support

.. tab:: 中文

    在 MuPDF 版本 1.11 中添加了对集合（portfolios）和 */EmbeddedFiles* 的初步支持，但在版本 1.15 中又取消了这一支持。

    因此，命令行工具 *mutool* 不再提供对嵌入文件的访问。

    PyMuPDF — 由于在 1.11.0 版本中实现了 */EmbeddedFiles* API — 从版本 1.16.0 开始不得不改变策略（我们从未发布过兼容 MuPDF v1.15.x 的 PyMuPDF 版本）。

    我们现在正在维护自己支持嵌入文件的代码基础。该代码仅使用基本的 MuPDF 字典和数组函数。


.. tab:: 英文

    After adding initial support for collections (portfolios) and */EmbeddedFiles* in MuPDF version 1.11, this support was dropped again in version 1.15.

    As a consequence, the cli utility *mutool* no longer offers access to embedded files.

    PyMuPDF -- having implemented an */EmbeddedFiles* API in response in its version 1.11.0 -- was therefore forced to change gears starting with its version 1.16.0 (we never published a MuPDF v1.15.x compatible PyMuPDF).

    We are now maintaining our own code basis supporting embedded files. This code makes use of basic MuPDF dictionary and array functions only.

PyMuPDF 支持
------------------

PyMuPDF Support

.. tab:: 中文

    我们继续支持嵌入文件的完整旧 API，只进行了少量的外观调整。

    此外，还新增了一个函数，用于返回 PDF 中注册的所有嵌入数据的名称列表： :meth:`Document.embfile_names` 。


.. tab:: 英文

    We continue to support the full old API with respect to embedded files -- with only minor, cosmetic changes.

    There even also is a new function, which delivers a list of all names under which embedded data are registered in a PDF, :meth:`Document.embfile_names`.

.. include:: footer.rst
