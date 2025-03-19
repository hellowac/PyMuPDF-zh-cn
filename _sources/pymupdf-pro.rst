
.. include:: header.rst



.. _pymupdf-pro

PyMuPDF Pro
=============

.. tab:: 中文

    |PyMuPDF Pro| 是一组 |PyMuPDF| 的 *商业扩展*。

    通过 **Office** 文档支持和 **RAG/LLM** 集成增强 |PyMuPDF| 功能。

    - 支持 Office 文档处理，包括 ``doc``、 ``docx``、 ``hwp``、 ``hwpx``、 ``ppt``、 ``pptx``、 ``xls``、 ``xlsx`` 等。
    - 支持文本和表格提取、 文档转换等。
    - 包括 |PyMuPDF4LLM| 的商业版本。

    要咨询如何获得商业许可，请使用 `此联系页面 <https://artifex.com/contact/>`_。

    .. note::

        |PyMuPDF Pro| 的许可版本还为您提供了 |PyMuPDF4LLM| 的许可版本。如果您有兴趣使用 |PyMuPDF4LLM| 包，您应该单独安装它。

.. tab:: 英文

    |PyMuPDF Pro| is a set of *commercial extensions* for |PyMuPDF|.

    Enhance |PyMuPDF| capability with **Office** document support & **RAG/LLM** integrations.

    - Enables Office document handling, including ``doc``, ``docx``, ``hwp``, ``hwpx``, ``ppt``, ``pptx``, ``xls``, ``xlsx``, and others.
    - Supports text and table extraction, document conversion and more.
    - Includes the commercial version of |PyMuPDF4LLM|.

    To enquire about obtaining a commercial license, then `use this contact page <https://artifex.com/contact/>`_.


    .. note::

        A licensed version of |PyMuPDF Pro| also gives you a licensed version of |PyMuPDF4LLM|. If you are interested in using the |PyMuPDF4LLM| package you should install it separately.


平台支持
--------------------

Platform support

.. tab:: 中文

    仅适用于以下平台：

    - Windows x86_64.
    - Linux x86_64 (glibc).
    - MacOS x86_64.
    - MacOS arm64.

.. tab:: 英文

    Available for these platforms only:

    - Windows x86_64.
    - Linux x86_64 (glibc).
    - MacOS x86_64.
    - MacOS arm64.


官方文件支持
----------------------

Office file support

.. tab:: 中文

    除了 `PyMuPDF 支持的标准文件类型 <Supported_File_Types>` 之外， |PyMuPDF Pro| 支持：

    .. list-table::
       :header-rows: 1
    
       * - **DOC/DOCX**
         - **XLS/XLSX**
         - **PPT/PPTX**
         - **HWP/HWPX**
       * - .. image:: images/icons/icon-docx.svg
              :width: 40
              :height: 40
         - .. image:: images/icons/icon-xlsx.svg
              :width: 40
              :height: 40
         - .. image:: images/icons/icon-pptx.svg
              :width: 40
              :height: 40
         - .. image:: images/icons/icon-hangul.svg
              :width: 40
              :height: 40

.. tab:: 英文

    In addition to the `standard file types supported by PyMuPDF <Supported_File_Types>`, |PyMuPDF Pro| supports:

    .. list-table::
       :header-rows: 1
    
       * - **DOC/DOCX**
         - **XLS/XLSX**
         - **PPT/PPTX**
         - **HWP/HWPX**
       * - .. image:: images/icons/icon-docx.svg
              :width: 40
              :height: 40
         - .. image:: images/icons/icon-xlsx.svg
              :width: 40
              :height: 40
         - .. image:: images/icons/icon-pptx.svg
              :width: 40
              :height: 40
         - .. image:: images/icons/icon-hangul.svg
              :width: 40
              :height: 40
    


使用
--------------

安装
~~~~~~~~~~~~~~~~~~

Installation

.. tab:: 中文

    使用pip安装:

    .. code-block:: bash

        pip install pymupdfpro



.. tab:: 英文

    Install via pip with:

    .. code-block:: bash

        pip install pymupdfpro


加载 **Office** 文档
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Loading an **Office** document

.. tab:: 中文

    导入 |PyMuPDF Pro| 后，您可以直接引用 **Office** 文档，例如：

    .. code-block:: python

        import pymupdf.pro
        pymupdf.pro.unlock()
        # PyMuPDF 现在已扩展为包含 PyMuPDF Pro 功能，但带有一些限制。
        doc = pymupdf.open("my-office-doc.xls")

    .. note::

        所有标准的 |PyMuPDF| 功能都会按预期提供， |PyMuPDF Pro| 处理扩展的 **Office** 文件类型。

    从此，您可以像通常一样处理文档页面，但需遵守 `限制 <PyMuPDFPro_Restrictions>`。


.. tab:: 英文

    Import |PyMuPDF Pro| and you can then reference **Office** documents directly, e.g.:

    .. code-block:: python

        import pymupdf.pro
        pymupdf.pro.unlock()
        # PyMuPDF has now been extended with PyMuPDF Pro features, with some restrictions.
        doc = pymupdf.open("my-office-doc.xls")

    .. note::

        All standard |PyMuPDF| functionality is exposed as expected - |PyMuPDF Pro| handles the extended **Office** file types


    From then on you can work with document pages just as you would do normally, but with respect to the `restrictions <PyMuPDFPro_Restrictions>`.


将 **Office** 文档转换为 **PDF**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Converting an **Office** document to **PDF**

.. tab:: 中文

    以下代码片段可以将您的 **Office** 文档转换为 |PDF| 格式：

.. tab:: 英文

    The following code snippet can convert your **Office** document to |PDF| format:

.. code-block:: python

    import pymupdf.pro
    pymupdf.pro.unlock()

    doc = pymupdf.open("my-office-doc.xlsx")

    pdfdata = doc.convert_to_pdf()
    with open('output.pdf', 'wb') as f:
        f.write(pdfdata)



.. _PyMuPDFPro_Restrictions:

限制
~~~~~~~~~~~~~~~~~~~~

Restrictions

.. tab:: 中文

    如果没有许可证密钥， |PyMuPDF Pro| 的功能将受到以下限制：

    **任何文档的前 3 页都只能使用。**

    要解锁完整功能，您应该 `获取试用密钥 <https://pymupdf.io/try-pro/>`_ 。

.. tab:: 英文


    |PyMuPDF Pro| functionality is restricted without a license key as follows:

        **Only the first 3 pages of any document will be available.**

    To unlock full functionality you should `obtain a trial key <https://pymupdf.io/try-pro/>`_.


.. _PyMuPDFPro_TrialKeys:

试用密钥
-----------------------

Trial keys

.. tab:: 中文

    要获取许可证密钥， `请填写此页面 <https://pymupdf.io/try-pro/>`_ 上的表格。然后，试用密钥将通过电子邮件发送到您提交的地址。

.. tab:: 英文

    To obtain a license key `please fill out the form on this page <https://pymupdf.io/try-pro/>`_. You will then have the trial key emailled to the address you submitted.


使用密钥
~~~~~~~~~~~~~~~~

Using a key

.. tab:: 中文

    使用如下密钥初始化 |PyMuPDF Pro| :

    .. code-block:: python

        import pymupdf.pro
        pymupdf.pro.unlock(my_key)
        # PyMuPDF 现在已经扩展了 PyMuPDF Pro 的功能。

    这将允许您在有限的时间内评估产品。如果您想在此时间之后使用 |PyMuPDF Pro|，则应 `咨询如何获得商业许可证 <https://artifex.com/products/pymupdf-pro/>`_ 。

.. tab:: 英文


    Initialize |PyMuPDF Pro| with a key as follows:

    .. code-block:: python

        import pymupdf.pro
        pymupdf.pro.unlock(my_key)
        # PyMuPDF has now been extended with PyMuPDF Pro features.

    This will allow you to evaluate the product for a limited time. If you want to use |PyMuPDF Pro| after this time you should then `enquire about obtaining a commercial license <https://artifex.com/products/pymupdf-pro/>`_.


字体
-----------------------

Fonts

.. tab:: 中文

    默认情况下， `pymupdf.pro.unlock()` 会搜索所有已安装的字体目录。

    可以使用仅限关键字参数来控制此行为：

    * `fontpath`：指定的字体目录，可以是列表/元组或 `os.sep` 分隔的字符串。
      如果为 None（默认值），则使用 `os.environ['PYMUPDFPRO_FONT_PATH']` （如果已设置）。
    * `fontpath_auto`：是否追加系统字体目录。
      如果为 None（默认值），则当 `os.environ['PYMUPDFPRO_FONT_PATH_AUTO']` 为 '1' 时默认为 True。
      如果为 True，则会追加所有系统字体目录。

    函数 `pymupdf.pro.get_fontpath()` 返回 `unlock()` 使用的所有字体目录的元组。


    .. raw:: html

        <button id="findOutAboutPyMuPDFPro" class="cta orange" onclick="window.location='https://pymupdf.io/try-pro/?utm_source=rtd-pymupdf&utm_medium=rtd&utm_content=cta-button'">准备尝试 PyMuPDF Pro?</button>


.. tab:: 英文

    By default `pymupdf.pro.unlock()` searches for all installed font directories.

    This can be controlled with keyword-only args:

    * `fontpath`: specific font directories, either as a list/tuple or `os.sep`-separated string.
      If None (the default), we use `os.environ['PYMUPDFPRO_FONT_PATH']` if set.
    * `fontpath_auto`: Whether to append system font directories.
      If None (the default) we use true if `os.environ['PYMUPDFPRO_FONT_PATH_AUTO']` is '1'.
      If true we append all system font directories.

    Function `pymupdf.pro.get_fontpath()` returns a tuple of all font directories used by `unlock()`.


    .. raw:: html

        <button id="findOutAboutPyMuPDFPro" class="cta orange" onclick="window.location='https://pymupdf.io/try-pro/?utm_source=rtd-pymupdf&utm_medium=rtd&utm_content=cta-button'">Ready to try PyMuPDF Pro?</button>





.. include:: footer.rst
