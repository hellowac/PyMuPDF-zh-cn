.. include:: header.rst

.. _Deprecated:

================
Deprecated Names
================

.. tab:: 中文

    PyMuPDF 方法与属性的命名约定最初采用的是“camelCase”。自 2013 年左右创建以来，PyMuPDF 的功能大幅增加，同时类、方法和属性的数量也相应增长。在许多情况下，这导致了名称不够直观、不够逻辑化，甚至显得冗长难记。

    几版之前，我决定改变策略，转向“snake_case”命名标准。这是一个重大的调整，需要逐步实施。现在（1.18.14 版），我认为这一转换已经完成。

    下表列出了已废弃名称及其新版本的对应关系。例如，:ref:`Document` 类中的 `pageCount` 属性已更名为 `page_count`。此外，也存在一些不那么直观的更改，例如 :ref:`Pixmap` 类中的 `getPNGdata` 方法被重命名为 `tobytes`。

    类名（仍然使用驼峰命名）以及包范围的常量（大多数为全大写）保持不变。

    旧名称将在 MuPDF 1.19.0 版本之前继续作为废弃别名可用，并将在后续版本（预计为 1.20.0，但具体取决于上游决定）中**被移除**。

    从 1.19.0 版本开始，我们将在 `sys.stderr` 输出弃用警告，例如当使用别名方法时，将显示类似以下内容的消息： `Deprecation: 'newPage' removed from class 'Document' after v1.19.0 - use 'new_page'.` 。 但使用已废弃的属性不会触发此类警告。

    从现在起，所有已废弃的对象（方法和属性）仍会显示原始文档字符串的副本，但会**添加**弃用消息作为前缀，例如::

        >>> print(pymupdf.Document.pageCount.__doc__)
        *** Deprecated and removed in version following 1.19.0 - use 'page_count'. ***
        Number of pages.
        >>> print(pymupdf.Document.newPage.__doc__)
        *** Deprecated and removed in version following 1.19.0 - use 'new_page'. ***
        Create and return a new page object.

            Args:
                pno: (int) 在此页面之前插入。默认值：最后一页之后。
                width: (float) 页面宽度（单位：点）。默认值：595（ISO A4 宽度）。
                height: (float) 页面高度（单位：点）。默认值：842（ISO A4 高度）。
            Returns:
                一个 Page 对象。

    此外，提供了一个实用脚本 `alias-changer.py <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/alias-changer.py>`_，可用于批量重命名脚本中的方法和属性。该脚本接受单个文件或文件夹作为参数。如果提供的是文件夹，则会更改其中的所有 Python 文件及其子文件夹中的 Python 文件。可选地，还可以创建脚本的备份。


.. tab:: 英文

    The original naming convention for methods and properties has been "camelCase". Since its creation around 2013, a tremendous increase of functionality has happened in PyMuPDF -- and with it a corresponding increase in classes, methods and properties. In too many cases, this has led to non-intuitive, illogical and ugly names, difficult to memorize or guess.

    A few versions ago, I therefore decided to shift gears and switch to a "snake_cased" naming standard.
    This was a major effort, which needed a step-wise approach. I think am done with it now (version 1.18.14).

    The following list maps deprecated names to their new versions. For example, property `pageCount` became `page_count` in the :ref:`Document` class. There also are less obvious name changes, e.g. method `getPNGdata` was renamed to `tobytes` in the :ref:`Pixmap` class.

    Names of classes (camel case) and package-wide constants (the majority is upper case) remain untouched.

    Old names will remain available as deprecated aliases through MuPDF version 1.19.0 and **be removed** in the version that follows it - probably version 1.20.0, but this depends on upstream decisions (MuPDF).

    Starting with version 1.19.0, we will issue deprecation warnings on `sys.stderr` like `Deprecation: 'newPage' removed from class 'Document' after v1.19.0 - use 'new_page'.` when aliased methods are being used. Using a deprecated property will not cause this type of warning.

    Starting immediately, all deprecated objects (methods and properties) will show a copy of the original's docstring, **prefixed** with the deprecation message, for example::

        >>> print(pymupdf.Document.pageCount.__doc__)
        *** Deprecated and removed in version following 1.19.0 - use 'page_count'. ***
        Number of pages.
        >>> print(pymupdf.Document.newPage.__doc__)
        *** Deprecated and removed in version following 1.19.0 - use 'new_page'. ***
        Create and return a new page object.

            Args:
                pno: (int) insert before this page. Default: after last page.
                width: (float) page width in points. Default: 595 (ISO A4 width).
                height: (float) page height in points. Default 842 (ISO A4 height).
            Returns:
                A Page object.
            

    There is a utility script `alias-changer.py <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/alias-changer.py>`_ which can be used to do mass-renames in your scripts. It accepts either a single file or a folder as argument. If a folder is supplied, all its Python files and those of its subfolders are changed. Optionally, backups of the scripts can be taken.


.. include:: footer.rst
