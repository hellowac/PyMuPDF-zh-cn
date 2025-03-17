.. include:: header.rst

.. _RecipesJournalling:

=========================================
日记
=========================================

Journalling

.. tab:: 中文

    从版本 1.19.0 开始，MuPDF 支持在更新 PDF 文档时进行日志记录（journalling）。

    日志记录是一种日志机制，允许 **撤销** 或 **重新应用** 对 PDF 所做的更改。类似于现代数据库系统中的 LUW（"逻辑工作单元"），可以将一组更新组合成一个“操作”。在 MuPDF 的日志记录中，操作扮演着 LUW 的角色。

    .. note:: 与数据库系统中的 LUW 实现不同，MuPDF 的日志记录是 **按文档级别** 进行的。它不支持跨多个 PDF 文件的同时更新：如果需要此类功能，用户需要自行实现逻辑。

    * 日志记录必须通过文档方法 *启用*。日志记录可用于现有文档或新文档。日志记录 **只能在关闭文件时禁用**。
    * 一旦启用，每个更改都必须发生在一个 *操作* 中——否则会引发异常。操作通过文档方法启动和停止。在这两个调用之间发生的更新构成一个 LUW，因此可以整体回滚或重新应用，或者用 MuPDF 术语来说就是“撤销”或“重做”。
    * 可以随时查询日志记录状态：是否启用日志记录，已记录了多少个操作，是否可以进行“撤销”或“重做”，当前日志位置等。
    * 日志可以 **保存到** 或 **从** 文件中加载。这些是文档方法。
    * 加载日志文件时，会检查与文档的兼容性，并在成功后自动启用日志记录。
    * 对于 **现有的** 被记录日志的 PDF，提供了一种特殊的新保存方法：:meth:`Document.save_snapshot`。此方法执行一种特殊的增量保存，包括所有已经记录的更新。如果同时保存其日志（紧接着文档快照之后），则文档和日志将同步，以后可以一起使用以撤销或重做操作，或继续记录的更新——就像没有中断一样。
    * 快照 PDF 是一个有效的 PDF，在各方面都是可用的。然而，如果在没有使用其日志文件的情况下更改文档，则会发生去同步，日志变得不可用。
    * 快照文件的结构类似于增量更新。尽管如此，内部日志记录逻辑要求 **必须保存到一个新文件**。因此，用户应开发一种文件命名约定，以支持原始 PDF 和其快照集之间的可识别关系，例如 `original.pdf` 和它的快照集，像 `original-snap1.pdf` / `original-snap1.log`，`original-snap2.pdf` / `original-snap2.log` 等。


.. tab:: 英文

    Starting with version 1.19.0, journalling is possible when updating PDF documents.

    Journalling is a logging mechanism which permits either **reverting** or **re-applying** changes to a PDF. Similar to LUWs "Logical Units of Work" in modern database systems, one can group a set of updates into an "operation". In MuPDF journalling, an operation plays the role of a LUW.

    .. note:: In contrast to LUW implementations found in database systems, MuPDF journalling happens on a **per document level**. There is no support for simultaneous updates across multiple PDFs: one would have to establish one's own logic here.

    * Journalling must be *enabled* via a document method. Journalling is possible for existing or new documents. Journalling **can be disabled only** by closing the file.
    * Once enabled, every change must happen inside an *operation* -- otherwise an exception is raised. An operation is started and stopped via document methods. Updates happening between these two calls form an LUW and can thus collectively be rolled back or re-applied, or, in MuPDF terminology "undone" resp. "redone".
    * At any point, the journalling status can be queried: whether journalling is active, how many operations have been recorded, whether "undo" or "redo" is possible, the current position inside the journal, etc.
    * The journal can be **saved to** or **loaded from** a file. These are document methods.
    * When loading a journal file, compatibility with the document is checked and journalling is automatically enabled upon success.
    * For an **existing** PDF being journalled, a special new save method is available: :meth:`Document.save_snapshot`. This performs a special incremental save that includes all journalled updates so far. If its journal is saved at the same time (immediately after the document snapshot), then document and journal are in sync and can later on be used together to undo or redo operations or to continue journalled updates -- just as if there had been no interruption.
    * The snapshot PDF is a valid PDF in every aspect and fully usable. If the document is however changed in any way without using its journal file, then a desynchronization will take place and the journal is rendered unusable.
    * Snapshot files are structured like incremental updates. Nevertheless, the internal journalling logic requires, that saving **must happen to a new file**. So the user should develop a file naming convention to support recognizable relationships between an original PDF, like `original.pdf` and its snapshot sets, like `original-snap1.pdf` / `original-snap1.log`, `original-snap2.pdf` / `original-snap2.log`, etc.

示例会话 1
~~~~~~~~~~~~~~~~~~

Example Session 1

.. tab:: 中文

    描述：

    * 创建一个新的PDF并启用日志记录。然后添加一个页面和一些文本行，每个操作作为单独的日志条目。
    * 在日志中导航，撤销和重做这些更新，并显示状态和文件结果：

    示例::

        >>> import pymupdf
        >>> doc = pymupdf.open()
        >>> doc.journal_enable()

        >>> # 尝试没有操作的更新：
        >>> page = doc.new_page()
        mupdf: No journalling operation started
        ... 省略的行
        RuntimeError: No journalling operation started

        >>> doc.journal_start_op("op1")
        >>> page = doc.new_page()
        >>> doc.journal_stop_op()

        >>> doc.journal_start_op("op2")
        >>> page.insert_text((100, 100), "Line 1")
        >>> doc.journal_stop_op()

        >>> doc.journal_start_op("op3")
        >>> page.insert_text((100, 120), "Line 2")
        >>> doc.journal_stop_op()

        >>> doc.journal_start_op("op4")
        >>> page.insert_text((100, 140), "Line 3")
        >>> doc.journal_stop_op()

        >>> # 显示在日志中的位置
        >>> doc.journal_position()
        (4, 4)
        >>> # 记录了4个操作 - 当前处于底部
        >>> # 我们可以做什么？
        >>> doc.journal_can_do()
        {'undo': True, 'redo': False}
        >>> # 目前只能撤销。打印页面内容：
        >>> print(page.get_text())
        Line 1
        Line 2
        Line 3

        >>> # 撤销最后一次插入：
        >>> doc.journal_undo()
        >>> # 再次显示合并状态：
        >>> doc.journal_position(); doc.journal_can_do()
        (3, 4)
        {'undo': True, 'redo': True}
        >>> print(page.get_text())
        Line 1
        Line 2

        >>> # 我们现在的位置是倒数第二
        >>> # 最后的文本插入已被撤销
        >>> # 但我们也可以重做/向前移动：
        >>> doc.journal_redo()
        >>> # 我们的合并状态：
        >>> doc.journal_position(); doc.journal_can_do()
        (4, 4)
        {'undo': True, 'redo': False}
        >>> print(page.get_text())
        Line 1
        Line 2
        Line 3
        >>> # Line 3 又出现了！


.. tab:: 英文

    Description:

    * Make a new PDF and enable journalling. Then add a page and some text lines -- each as a separate operation.
    * Navigate within the journal, undoing and redoing these updates and displaying status and file results::

        >>> import pymupdf
        >>> doc=pymupdf.open()
        >>> doc.journal_enable()

        >>> # try update without an operation:
        >>> page = doc.new_page()
        mupdf: No journalling operation started
        ... omitted lines
        RuntimeError: No journalling operation started

        >>> doc.journal_start_op("op1")
        >>> page = doc.new_page()
        >>> doc.journal_stop_op()

        >>> doc.journal_start_op("op2")
        >>> page.insert_text((100,100), "Line 1")
        >>> doc.journal_stop_op()

        >>> doc.journal_start_op("op3")
        >>> page.insert_text((100,120), "Line 2")
        >>> doc.journal_stop_op()

        >>> doc.journal_start_op("op4")
        >>> page.insert_text((100,140), "Line 3")
        >>> doc.journal_stop_op()

        >>> # show position in journal
        >>> doc.journal_position()
        (4, 4)
        >>> # 4 operations recorded - positioned at bottom
        >>> # what can we do?
        >>> doc.journal_can_do()
        {'undo': True, 'redo': False}
        >>> # currently only undos are possible. Print page content:
        >>> print(page.get_text())
        Line 1
        Line 2
        Line 3

        >>> # undo last insert:
        >>> doc.journal_undo()
        >>> # show combined status again:
        >>> doc.journal_position();doc.journal_can_do()
        (3, 4)
        {'undo': True, 'redo': True}
        >>> print(page.get_text())
        Line 1
        Line 2

        >>> # our position is now second to last
        >>> # last text insertion was reverted
        >>> # but we can redo / move forward as well:
        >>> doc.journal_redo()
        >>> # our combined status:
        >>> doc.journal_position();doc.journal_can_do()
        (4, 4)
        {'undo': True, 'redo': False}
        >>> print(page.get_text())
        Line 1
        Line 2
        Line 3
        >>> # line 3 has appeared again!


示例会话 2
~~~~~~~~~~~~~~~~~~

Example Session 2

.. tab:: 中文

    描述：

    * 与之前的例子类似，但在撤销了一些操作后，我们现在添加了一个不同的更新操作。这将导致：

        - 被撤销的日志条目被永久移除
        - 新的更新操作将成为新的最后一条日志条目。

    示例：

        >>> doc = pymupdf.open()
        >>> doc.journal_enable()
        >>> doc.journal_start_op("插入页面")
        >>> page = doc.new_page()
        >>> doc.journal_stop_op()
        >>> for i in range(5):
        >>>     doc.journal_start_op("insert-%i" % i)
        >>>     page.insert_text((100, 100 + 20*i), "text line %i" % i)
        >>>     doc.journal_stop_op()

        >>> # 合并状态信息：
        >>> doc.journal_position(); doc.journal_can_do()
        (6, 6)
        {'undo': True, 'redo': False}

        >>> for i in range(3):  # 撤销最后三次操作
        >>>     doc.journal_undo()
        >>> doc.journal_position(); doc.journal_can_do()
        (3, 6)
        {'undo': True, 'redo': True}

        >>> # 现在做一个不同的更新：
        >>> doc.journal_start_op("绘制一条线")
        >>> page.draw_line((100, 150), (300, 150))
        Point(300.0, 150.0)
        >>> doc.journal_stop_op()
        >>> doc.journal_position(); doc.journal_can_do()
        (4, 4)
        {'undo': True, 'redo': False}

        >>> # 这已经改变了日志：
        >>> # 之前的最后三次文本插入操作被移除，
        >>> # 现在我们只有4个操作：绘制线条是新的最后一条操作


.. tab:: 英文

    Description:

    * Similar to previous, but after undoing some operations, we now add a different update. This will cause:

        - permanent removal of the undone journal entries
        - the new update operation will become the new last entry.


        >>> doc=pymupdf.open()
        >>> doc.journal_enable()
        >>> doc.journal_start_op("Page insert")
        >>> page=doc.new_page()
        >>> doc.journal_stop_op()
        >>> for i in range(5):
                doc.journal_start_op("insert-%i" % i)
                page.insert_text((100, 100 + 20*i), "text line %i" %i)
                doc.journal_stop_op()

        >>> # combined status info:
        >>> doc.journal_position();doc.journal_can_do()
        (6, 6)
        {'undo': True, 'redo': False}

        >>> for i in range(3):  # revert last three operations
                doc.journal_undo()
        >>> doc.journal_position();doc.journal_can_do()
        (3, 6)
        {'undo': True, 'redo': True}

        >>> # now do a different update:
        >>> doc.journal_start_op("Draw some line")
        >>> page.draw_line((100,150), (300,150))
        Point(300.0, 150.0)
        >>> doc.journal_stop_op()
        >>> doc.journal_position();doc.journal_can_do()
        (4, 4)
        {'undo': True, 'redo': False}

        >>> # this has changed the journal:
        >>> # previous last 3 text line operations were removed, and
        >>> # we have only 4 operations: drawing the line is the new last one

.. include:: footer.rst
