.. include:: header.rst

.. _cooperation:

===============================================================
协同工作: DisplayList 和 TextPage
===============================================================

Working together: DisplayList and TextPage

.. tab:: 中文

        以下是有关如何一起使用这些类的一些说明。

        在某些情况下，当您回到此处解释的详细级别时，可能会实现性能改进。

.. tab:: 英文

        Here are some instructions on how to use these classes together.

        In some situations, performance improvements may be achievable, when you fall back to the detail level explained here.

创建 DisplayList
---------------------

Create a DisplayList

.. tab:: 中文

        :ref:`DisplayList` 表示一个解释过的文档页面。像素图创建、文本提取和文本搜索等方法 -- 在背后 -- 都是利用页面的显示列表来执行任务。如果页面需要多次渲染（例如由于缩放级别的变化），或者需要同时执行文本搜索和文本提取，节省开销的办法是只创建一次显示列表，然后用于所有其他任务。

        >>> dl = page.get_displaylist()              # 创建显示列表

        你也可以为多个页面创建显示列表“堆栈”（在一个列表中），这可以在文档打开时、空闲时间期间进行，或者在页面首次访问时存储它（例如，在 GUI 脚本中）。

        请注意，对于接下来的所有操作，只需要显示列表 -- 相应的 :ref:`Page` 对象可以已经被删除。


.. tab:: 英文

        A :ref:`DisplayList` represents an interpreted document page. Methods for pixmap creation, text extraction and text search are  -- behind the curtain -- all using the page's display list to perform their tasks. If a page must be rendered several times (e.g. because of changed zoom levels), or if text search and text extraction should both be performed, overhead can be saved, if the display list is created only once and then used for all other tasks.

        >>> dl = page.get_displaylist()              # create the display list

        You can also create display lists for many pages "on stack" (in a list), may be during document open, during idling times, or you store it when a page is visited for the first time (e.g. in GUI scripts).

        Note, that for everything what follows, only the display list is needed -- the corresponding :ref:`Page` object could have been deleted.

生成 Pixmap
------------------

Generate Pixmap

.. tab:: 中文

        以下代码从 :ref:`DisplayList` 创建一个 :ref:`Pixmap` 。参数与 :meth:`Page.get_pixmap` 相同。

        >>> pix = dl.get_pixmap()                    # 创建页面的 Pixmap

        该语句的执行时间可能比 :meth:`Page.get_pixmap` 短最多 50%。

.. tab:: 英文

        The following creates a Pixmap from a :ref:`DisplayList`. Parameters are the same as for :meth:`Page.get_pixmap`.

        >>> pix = dl.get_pixmap()                    # create the page's pixmap

        The execution time of this statement may be up to 50% shorter than that of :meth:`Page.get_pixmap`.

执行文本搜索
---------------------

Perform Text Search

.. tab:: 中文

        使用上面的显示列表，我们还可以进行文本搜索。

        为此，我们需要创建一个 :ref:`TextPage`。

        >>> tp = dl.get_textpage()                    # 从上面的显示列表创建 TextPage
        >>> rlist = tp.search("needle")              # 查找 "needle" 的位置
        >>> for r in rlist:                          # 处理找到的位置，例如：
                pix.invert_irect(r.irect)             # 在矩形区域内反转颜色

.. tab:: 英文

        With the display list from above, we can also search for text.

        For this we need to create a :ref:`TextPage`.

        >>> tp = dl.get_textpage()                    # display list from above
        >>> rlist = tp.search("needle")              # look up "needle" locations
        >>> for r in rlist:                          # work with the found locations, e.g.
                pix.invert_irect(r.irect)             # invert colors in the rectangles

提取文本
----------------

Extract Text

.. tab:: 中文

        使用上面的相同 :ref:`TextPage` 对象，我们现在可以立即使用所有或任意一个文本提取方法。

        .. note:: 
                
                上面我们创建了一个没有参数的文本页面，这会导致默认参数为 3（保留连字和空格），因此 **不会** 提取图像 -- 见下文。

        >>> txt  = tp.extractText()                  # 纯文本格式
        >>> json = tp.extractJSON()                  # JSON 格式
        >>> html = tp.extractHTML()                  # HTML 格式
        >>> xml  = tp.extractXML()                   # XML 格式
        >>> xml  = tp.extractXHTML()                 # XHTML 格式

.. tab:: 英文

        With the same :ref:`TextPage` object from above, we can now immediately use any or all of the 5 text extraction methods.

        .. note:: Above, we have created our text page without argument. This leads to a default argument of 3 (:data:`ligatures` and white-space are preserved), IAW images will **not** be extracted -- see below.

        >>> txt  = tp.extractText()                  # plain text format
        >>> json = tp.extractJSON()                  # json format
        >>> html = tp.extractHTML()                  # HTML format
        >>> xml  = tp.extractXML()                   # XML format
        >>> xml  = tp.extractXHTML()                 # XHTML format

进一步的性能改进
---------------------------------

Further Performance improvements

Pixmap
~~~~~~~

Pixmap

.. tab:: 中文

        如在 :ref:`Page` 章节中所解释的：

        如果您不需要透明度，在创建 Pixmap 时将 *alpha = 0* 。这样可以节省 25% 的内存（如果是 RGB，这是最常见的情况），并可能节省 5% 的执行时间（取决于 GUI 软件）。

.. tab:: 英文

        As explained in the :ref:`Page` chapter:

        If you do not need transparency set *alpha = 0* when creating pixmaps. This will save 25% memory (if RGB, the most common case) and possibly 5% execution time (depending on the GUI software).

TextPage
~~~~~~~~~

TextPage

.. tab:: 中文

        如果您不需要提取与页面文本一起的图像，可以设置以下选项：

        >>> flags = pymupdf.TEXT_PRESERVE_LIGATURES | pymupdf.TEXT_PRESERVE_WHITESPACE
        >>> tp = dl.get_textpage(flags)

        这样可以节省大约 25% 的整体执行时间，特别是在 HTML、XHTML 和 JSON 文本提取时，并且如果文档是图形导向的，将 **大幅** 减少存储空间（包括内存和磁盘空间）。

        但是，如果您确实需要图像，可以将标志设置为 7:

        >>> flags = pymupdf.TEXT_PRESERVE_LIGATURES | pymupdf.TEXT_PRESERVE_WHITESPACE | pymupdf.TEXT_PRESERVE_IMAGES

.. tab:: 英文

        If you do not need images extracted alongside the text of a page, you can set the following option:

        >>> flags = pymupdf.TEXT_PRESERVE_LIGATURES | pymupdf.TEXT_PRESERVE_WHITESPACE
        >>> tp = dl.get_textpage(flags)

        This will save ca. 25% overall execution time for the HTML, XHTML and JSON text extractions and **hugely** reduce the amount of storage (both, memory and disk space) if the document is graphics oriented.

        If you however do need images, use a value of 7 for flags:

        >>> flags = pymupdf.TEXT_PRESERVE_LIGATURES | pymupdf.TEXT_PRESERVE_WHITESPACE | pymupdf.TEXT_PRESERVE_IMAGES

.. include:: footer.rst
