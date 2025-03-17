.. include:: header.rst

.. _RecipesStories:

.. role:: htmlTag(emphasis)

.. |toggleStart| raw:: html

   <details>
   <summary><a>See recipe</a></summary>

.. |toggleEnd| raw:: html

   </details>

==============================
故事
==============================

Stories

.. tab:: 中文

    本文档展示了一些 :ref:`故事（Stories）<WorkingWithStories>` 的典型用例。

    正如 :ref:`教程 <WorkingWithStories>` 所述，故事可以使用最多三种输入来源创建：HTML、CSS 和存档（Archives）。这些输入都是可选的，并且可以通过编程方式提供。

    以下示例将展示这些输入的不同组合方式。

    .. note::

        许多示例的源代码已包含在 `docs` 文件夹中。

.. tab:: 英文


    This document showcases some typical use cases for :ref:`Stories<WorkingWithStories>`.

    As mentioned in the :ref:`tutorial<WorkingWithStories>`, stories may be created using up to three input sources: HTML, CSS and Archives -- all of which are optional and which, respectively, can be provided programmatically.

    The following examples will showcase combinations for using these inputs.

    .. note::

        Many of these recipe's source code are included as examples in the `docs` folder.


.. _RecipesStories_A:

如何添加一行带格式的文本
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Add a Line of Text with Some Formatting

.. tab:: 中文

    这里是不可避免的 "Hello World" 示例。我们将展示两种变体：

    1. 使用现有的 HTML 源 [#f2]_，它可以来自任何地方。
    2. 使用 Python API 进行创建。

    -----

    **使用现有 HTML 源的变体** [#f2]_ —— 在本例中，该 HTML 源被定义为脚本中的一个常量::

        import pymupdf

        HTML = """
        <p style="font-family: sans-serif;color: blue">Hello World!</p>
        """

        MEDIABOX = pymupdf.paper_rect("letter")  # 输出页面格式：Letter
        WHERE = MEDIABOX + (36, 36, -36, -36)  # 保留 0.5 英寸的边距

        story = pymupdf.Story(html=HTML)  # 从 HTML 创建故事
        writer = pymupdf.DocumentWriter("output.pdf")  # 创建写入器
        
        more = 1  # 当 more 设为 0 时，表示输入结束

        while more:  # 循环输出故事
            device = writer.begin_page(MEDIABOX)  # 创建新页面
            more, _ = story.place(WHERE)  # 在允许的矩形区域内布局
            story.draw(device)  # 在页面上绘制内容
            writer.end_page()  # 结束页面
        
        writer.close()  # 关闭输出文件

    .. note::
        
        上述效果（无衬线字体和蓝色文本）也可以通过单独的 CSS 资源实现，如下所示::

            import pymupdf

            CSS = """
            body {
                font-family: sans-serif;
                color: blue;
            }
            """

            HTML = """
            <p>Hello World!</p>
            """

            # 这样创建故事：
            story = pymupdf.Story(html=HTML, user_css=CSS)

    -----

    **Python API 变体** —— 所有内容均通过编程方式创建::

        import pymupdf
        
        MEDIABOX = pymupdf.paper_rect("letter")
        WHERE = MEDIABOX + (36, 36, -36, -36)

        story = pymupdf.Story()  # 创建一个空的故事
        body = story.body  # 访问其 DOM 的 body
        with body.add_paragraph() as para:  # 存储所需内容
            para.set_font("sans-serif").set_color("blue").add_text("Hello World!")

        writer = pymupdf.DocumentWriter("output.pdf")
        
        more = 1

        while more:
            device = writer.begin_page(MEDIABOX)
            more, _ = story.place(WHERE)
            story.draw(device)
            writer.end_page()

        writer.close()

    两种变体都会生成相同的输出 PDF。

.. tab:: 英文

    Here is the inevitable "Hello World" example. We will show two variants:

    1. Create using existing HTML source [#f1]_, that may come from anywhere.
    2. Create using the Python API.

    -----

    Variant using an existing HTML source [#f1]_ -- which in this case is defined as a constant in the script::

        import pymupdf

        HTML = """
        <p style="font-family: sans-serif;color: blue">Hello World!</p>
        """

        MEDIABOX = pymupdf.paper_rect("letter")  # output page format: Letter
        WHERE = MEDIABOX + (36, 36, -36, -36)  # leave borders of 0.5 inches

        story = pymupdf.Story(html=HTML)  # create story from HTML
        writer = pymupdf.DocumentWriter("output.pdf")  # create the writer
        
        more = 1  # will indicate end of input once it is set to 0

        while more:  # loop outputting the story
            device = writer.begin_page(MEDIABOX)  # make new page
            more, _ = story.place(WHERE)  # layout into allowed rectangle
            story.draw(device)  # write on page
            writer.end_page()  # finish page
        
        writer.close()  # close output file

    .. note::
        
        The above effect (sans-serif and blue text) could have been achieved by using a separate CSS source like so::

            import pymupdf

            CSS = """
            body {
                font-family: sans-serif;
                color: blue;
            }
            """

            HTML = """
            <p>Hello World!</p>
            """

            # the story would then be created like this:
            story = pymupdf.Story(html=HTML, user_css=CSS)

    -----

    The Python API variant -- everything is created programmatically::

        import pymupdf
        
        MEDIABOX = pymupdf.paper_rect("letter")
        WHERE = MEDIABOX + (36, 36, -36, -36)

        story = pymupdf.Story()  # create an empty story
        body = story.body  # access the body of its DOM
        with body.add_paragraph() as para:  # store desired content
            para.set_font("sans-serif").set_color("blue").add_text("Hello World!")

        writer = pymupdf.DocumentWriter("output.pdf")
        
        more = 1

        while more:
            device = writer.begin_page(MEDIABOX)
            more, _ = story.place(WHERE)
            story.draw(device)
            writer.end_page()

        writer.close()

    Both variants will produce the same output PDF.

-----

.. _RecipesStories_B:


如何使用图像
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to use Images

.. tab:: 中文

    图片可以在提供的 HTML 源代码中引用，也可以通过 Python API 存储对所需图片的引用。无论哪种方式，都需要使用 :ref:`Archive`，它指向图片所在的位置。

    .. note:: 嵌入 HTML 源代码中的二进制内容的图片 **不支持** 在 stories 中使用。

    我们扩展了上面的 "Hello World" 示例，并在文本后面显示我们的星球图片。假设图片的名称为 "world.jpg" 且位于脚本文件夹中，那么下面是修改后的 Python API 变体::

        import pymupdf

        MEDIABOX = pymupdf.paper_rect("letter")
        WHERE = MEDIABOX + (36, 36, -36, -36)

        # 创建故事，让它查看脚本文件夹中的资源
        story = pymupdf.Story(archive=".")
        body = story.body  # 访问其 DOM 的 body

        with body.add_paragraph() as para:
            # 存储所需内容
            para.set_font("sans-serif").set_color("blue").add_text("Hello World!")

        # 另一个段落用于我们的图片：
        with body.add_paragraph() as para:
            # 在另一个段落中存储图片
            para.add_image("world.jpg")

        writer = pymupdf.DocumentWriter("output.pdf")

        more = 1

        while more:
            device = writer.begin_page(MEDIABOX)
            more, _ = story.place(WHERE)
            story.draw(device)
            writer.end_page()

        writer.close()


.. tab:: 英文

    Images can be referenced in the provided HTML source, or the reference to a desired image can also be stored via the Python API. In any case, this requires using an :ref:`Archive`, which refers to the place where the image can be found.

    .. note:: Images with the binary content embedded in the HTML source are **not supported** by stories.

    We extend our "Hello World" example from above and display an image of our planet right after the text. Assuming the image has the name "world.jpg" and is present in the script's folder, then this is the modified version of the above Python API variant::

        import pymupdf

        MEDIABOX = pymupdf.paper_rect("letter")
        WHERE = MEDIABOX + (36, 36, -36, -36)

        # create story, let it look at script folder for resources
        story = pymupdf.Story(archive=".")
        body = story.body  # access the body of its DOM

        with body.add_paragraph() as para:
            # store desired content
            para.set_font("sans-serif").set_color("blue").add_text("Hello World!")

        # another paragraph for our image:
        with body.add_paragraph() as para:
            # store image in another paragraph
            para.add_image("world.jpg")

        writer = pymupdf.DocumentWriter("output.pdf")

        more = 1

        while more:
            device = writer.begin_page(MEDIABOX)
            more, _ = story.place(WHERE)
            story.draw(device)
            writer.end_page()

        writer.close()


-----


.. _RecipesStories_C:


如何读取故事的外部 HTML 和 CSS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Read External HTML and CSS for a Story

.. tab:: 中文

    这些情况相对简单。

    作为一般建议，HTML 和 CSS 源文件应该 **以二进制文件形式读取**，并在使用它们创建故事之前进行解码。Python 的 `pathlib.Path` 提供了方便的方式来实现这一点::

        import pathlib
        import pymupdf

        htmlpath = pathlib.Path("myhtml.html")
        csspath = pathlib.Path("mycss.css")

        HTML = htmlpath.read_bytes().decode()
        CSS = csspath.read_bytes().decode()

        story = pymupdf.Story(html=HTML, user_css=CSS)


.. tab:: 英文


    These cases are fairly straightforward.

    As a general recommendation, HTML and CSS sources should be **read as binary files** and decoded before using them in a story. The Python `pathlib.Path` provides convenient ways to do this::

        import pathlib
        import pymupdf

        htmlpath = pathlib.Path("myhtml.html")
        csspath = pathlib.Path("mycss.css")

        HTML = htmlpath.read_bytes().decode()
        CSS = csspath.read_bytes().decode()

        story = pymupdf.Story(html=HTML, user_css=CSS)


-----


.. _RecipesStories_D:


如何使用故事模板输出数据库内容
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Output Database Content with Story Templates

.. tab:: 中文

    这个脚本演示了如何使用 **HTML 模板** 报告 SQL 数据库内容。

    示例 SQL 数据库包含两个表：

    1. 表 "films" 包含每部电影的一行数据，字段包括 **"title"**、 **"director"**  和（上映年份） **"year"**。
    2. 表 "actors" 包含每位演员和电影标题的一行数据，字段包括（演员） **"name"** 和 （电影） **"title"** 。

    故事 DOM 由一个电影的模板组成，报告电影数据以及一个演员名单。

    **文件：**

    * `docs/samples/filmfestival-sql.py`
    * `docs/samples/filmfestival-sql.db`


.. tab:: 英文


    This script demonstrates how to report SQL database content using an **HTML template**.

    The example SQL database contains two tables:

    1. Table "films" contains one row per film with the fields **"title"**, **"director"** and (release) **"year"**.
    2. Table "actors" contains one row per actor and film title (fields (actor) **"name"** and (film) **"title"**).

    The story DOM consists of a template for one film, which reports film data together with a list of casted actors.

    **Files:**

    * `docs/samples/filmfestival-sql.py`
    * `docs/samples/filmfestival-sql.db`


|toggleStart|

.. literalinclude:: samples/filmfestival-sql.py

|toggleEnd|


-----


.. _RecipesStories_E:

如何与现有 PDF 集成
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Integrate with Existing PDFs

.. tab:: 中文

    由于 :ref:`DocumentWriter` 只能写入新文件，因此故事无法放置在现有页面上。这个脚本演示了如何规避这一限制。

    基本思路是让 :ref:`DocumentWriter` 将输出写入内存中的 PDF。一旦故事完成，我们重新打开这个内存中的 PDF，并通过 :meth:`Page.show_pdf_page` 方法将其页面放置到 **现有** 页面上的所需位置。

    **文件：**

    * `docs/samples/showpdf-page.py`


.. tab:: 英文

    Because a :ref:`DocumentWriter` can only write to a new file, stories cannot be placed on existing pages. This script demonstrates a circumvention of this restriction.

    The basic idea is letting :ref:`DocumentWriter` output to a PDF in memory. Once the story has finished, we re-open this memory PDF and put its pages to desired locations on **existing** pages via method :meth:`Page.show_pdf_page`.

    **Files:**

    * `docs/samples/showpdf-page.py`

|toggleStart|

.. literalinclude:: samples/showpdf-page.py

|toggleEnd|


-----


.. _RecipesStories_F:

如何从包 `pymupdf-fonts`_ 制作多列布局和访问字体
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Make Multi-Columned Layouts and Access Fonts from Package `pymupdf-fonts`_

.. tab:: 中文

    这个脚本输出了一篇文章（来自 Wikipedia），该文章包含文本和多张图片，并使用了 2 列页面布局。

    此外，脚本使用了 `pymupdf-fonts`_ 包中的两个 "Ubuntu" 字体系列，而不是默认的 Base-14 字体。

    另一个特性是，所有数据——图片和文章 HTML——都共同存储在一个 ZIP 文件中。

    **文件：**

    * `docs/samples/quickfox.py`
    * `docs/samples/quickfox.zip`


.. tab:: 英文


    This script outputs an article (taken from Wikipedia) that contains text and multiple images and uses a 2-column page layout.

    In addition, two "Ubuntu" font families from package `pymupdf-fonts`_ are used instead of defaulting to Base-14 fonts.

    Yet another feature used here is that all data -- the images and the article HTML -- are jointly stored in a ZIP file.


    **Files:**

    * `docs/samples/quickfox.py`
    * `docs/samples/quickfox.zip`


|toggleStart|

.. literalinclude:: samples/quickfox.py

|toggleEnd|


-----


.. _RecipesStories_G:

如何制作环绕预定义“禁区”布局的布局
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Make a Layout which Wraps Around a Predefined "no go area" Layout

.. tab:: 中文

    这是一个使用 PyMuPDF 的 Story 类输出文本为 PDF 的演示脚本，具有双栏页面布局。

    该脚本演示了以下特性：

    * 在现有的（"目标"）PDF 上布局文本和图片。
    * 基于一些全局参数，识别每页上的区域，这些区域可以用于接收由 Story 布局的文本。
    * 这些全局参数不会存储在目标 PDF 中，因此必须以某种方式提供：

    - 每页的边距宽度。
    - 用于文本的字体大小。这个值决定了提供的文本是否能够适应目标 PDF 固定页面上的空白区域。无法预测。如果目标 PDF 没有足够的页面，脚本会抛出异常，并在某些页面没有至少一些文本时打印警告消息。在这两种情况下，可以修改 FONTSIZE 值（浮动值）。
    - 使用双栏页面布局来布局文本。

    * 布局会创建一个临时的（内存中的）PDF。生成的页面内容（文本）会覆盖目标页面的相应位置。如果文本需要的页数超过目标 PDF 可用的页数，则会抛出异常。如果不是所有目标页面都接收到至少一些文本，将打印警告信息。
    * 脚本读取当前文件夹中的 "image-no-go.pdf"。这是 "目标" PDF，它包含 2 页，每页上有 2 张图片（来自原始文章），这些图片被放置在创建广泛测试覆盖的地方。否则，页面是空的。
    * 脚本生成 "quickfox-image-no-go.pdf"，其中包含原始页面和图片位置，但原始文章的文本已布局在它们周围。

    **文件：**

    * `docs/samples/quickfox-image-no-go.py`
    * `docs/samples/quickfox-image-no-go.pdf`
    * `docs/samples/quickfox.zip`


.. tab:: 英文

    This is a demo script using PyMuPDF's Story class to output text as a PDF with
    a two-column page layout.

    The script demonstrates the following features:

    * Layout text around images of an existing ("target") PDF.
    * Based on a few global parameters, areas on each page are identified, that
    can be used to receive text layouted by a Story.
    * These global parameters are not stored anywhere in the target PDF and
    must therefore be provided in some way:

    - The width of the border(s) on each page.
    - The fontsize to use for text. This value determines whether the provided
        text will fit in the empty spaces of the (fixed) pages of target PDF. It
        cannot be predicted in any way. The script ends with an exception if
        target PDF has not enough pages, and prints a warning message if not all
        pages receive at least some text. In both cases, the FONTSIZE value
        can be changed (a float value).
    - Use of a 2-column page layout for the text.
    * The layout creates a temporary (memory) PDF. Its produced page content
    (the text) is used to overlay the corresponding target page. If text
    requires more pages than are available in target PDF, an exception is raised.
    If not all target pages receive at least some text, a warning is printed.
    * The script reads "image-no-go.pdf" in its own folder. This is the "target" PDF.
    It contains 2 pages with each 2 images (from the original article), which are
    positioned at places that create a broad overall test coverage. Otherwise the
    pages are empty.
    * The script produces "quickfox-image-no-go.pdf" which contains the original pages
    and image positions, but with the original article text laid out around them.

    **Files:**

    * `docs/samples/quickfox-image-no-go.py`
    * `docs/samples/quickfox-image-no-go.pdf`
    * `docs/samples/quickfox.zip`


|toggleStart|

.. literalinclude:: samples/quickfox-image-no-go.py

|toggleEnd|


-----


.. _RecipesStories_H:

如何输出 HTML 表格
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Output an HTML Table

.. tab:: 中文

    支持输出 HTML 表格，如下所示：

    * 支持平面表格布局（"行 x 列"），不支持 "colspan" / "rowspan" 属性。
    * 表头标签 :htmlTag:`th` 支持 "scope" 属性，其值可以是 "row" 或 "col"。适用的文本默认加粗。
    * 列宽度基于列内容自动计算，不能直接设置。
    * 表格**单元格可以包含图片**，这些图片会被考虑在列宽计算中。
    * 行高基于行内容自动计算 - 必要时会生成多行行。
    * 表格行中的多行始终会保持在同一页（或 "where" 矩形区域）内，而不会被拆分。
    * 表头行仅在第一页 / "where" 矩形区域显示。
    * 在 HTML 表格元素中直接给出的 "style" 属性会被忽略。表格及其元素的样式必须在 CSS 源中或在 :htmlTag:`style` 标签内定义。
    * 不支持并会忽略 :htmlTag:`tr` 元素的样式。因此，不支持表格的网格或交替行背景颜色。不过，以下示例脚本展示了如何解决此限制。

    **文件：**

    * `docs/samples/table01.py` 该脚本反映了基本特性。

    |toggleStart|

    .. literalinclude:: samples/table01.py

    |toggleEnd|

    * `docs/samples/national-capitals.py` 高级脚本使用简单的附加代码扩展表格输出选项：

        - 多页输出模拟 **重复标题行**
        - 交替表格行背景颜色
        - 表格行和列由网格线分隔
        - 表格行动态生成/填充来自 SQL 数据库的数据

.. tab:: 英文

    Outputting HTML tables is supported as follows:

    * Flat table layouts are supported ("rows x columns"), no support of the "colspan" / "rowspan" attributes.
    * Table header tag :htmlTag:`th` supports attribute "scope" with values "row" or "col". Applicable text will be bold by default.
    * Column widths are computed automatically based on column content. They cannot be directly set.
    * Table **cells may contain images** which will be considered in the column width calculation magic.
    * Row heights are computed automatically based on row content - leading to multi-line rows where needed.
    * The potentially multiple lines of a table row will always be kept together on one page (respectively "where" rectangle) and not be split.
    * Table header rows are only **shown on the first page / "where" rectangle.**
    * The "style" attribute is ignored when given directly in HTML table elements. Styling for a table and its elements must happen separately, in CSS source or within the :htmlTag:`style` tag.
    * Styling for :htmlTag:`tr` elements is not supported and ignored. Therefore, a table-wide grid or alternating row background colors are not supported. One of the following example scripts however shows an easy way to deal with this limitation.

    **Files:**

    * `docs/samples/table01.py` This script reflects basic features.

    |toggleStart|

    .. literalinclude:: samples/table01.py

    |toggleEnd|

    * `docs/samples/national-capitals.py` Advanced script extending table output options using simple additional code:

        - Multi-page output simulating **repeating header rows**
        - Alternating table row background colors
        - Table rows and columns delimited by gridlines
        - Table rows dynamically generated / filled with data from an SQL database

|toggleStart|

.. literalinclude:: samples/national-capitals.py

|toggleEnd|


-----


.. _RecipesStories_I:

如何创建简单的网格布局
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Create a Simple Grid Layout

.. tab:: 中文

    通过在通过 :ref:`make_table<Functions_make_table>` 函数创建的网格内创建 :ref:`Story` 对象序列，开发人员可以根据需要创建网格布局。

    **文件：**

    * `docs/samples/simple-grid.py`

.. tab:: 英文

    By creating a sequence of :ref:`Story` objects within a grid created via the :ref:`make_table<Functions_make_table>` function a developer can create grid layouts as required.

    **Files:**

    * `docs/samples/simple-grid.py`

|toggleStart|

.. literalinclude:: samples/simple-grid.py

|toggleEnd|


-----


.. _RecipesStories_J:

如何生成目录
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Generate a Table of Contents

.. tab:: 中文

    此脚本列出了脚本目录中所有 Python 脚本的源代码。

    **文件：**

    * `docs/samples/code-printer.py`

    |toggleStart|

    .. literalinclude:: samples/code-printer.py

    |toggleEnd|

    它具有以下功能：

    * 使用专门的 :ref:`Story` 在文档开头的单独编号页面上自动生成目录 (TOC)。

    * 每页使用 3 个单独的 :ref:`Story` 对象：页眉故事、页脚故事和用于打印 Python 源的故事。

        - 页面 **页脚会自动更改** 以显示当前 Python 文件的名称。

    * 使用 :meth:`Story.element_positions` 收集目录数据并动态调整页脚。这是故事输出过程和脚本之间 **双向通信** 的示例。

    * 带有 Python 源的主 PDF 正在由其 :ref:`DocumentWriter` 写入内存。然后使用另一个 :ref:`Story` / :ref:`DocumentWriter` 对为目录页面创建（内存）PDF。最后，将这两个 PDF 合并并将结果存储到磁盘。

.. tab:: 英文


    This script lists the source code of all Python scripts that live in the script's directory.

    **Files:**

    * `docs/samples/code-printer.py`

    |toggleStart|

    .. literalinclude:: samples/code-printer.py

    |toggleEnd|


    It features the following capabilities:

    * Automatic generation of a Table of Contents (TOC) on separately numbered pages at the start of the document - using a specialized :ref:`Story`.

    * Use of 3 separate :ref:`Story` objects per page: header story, footer story and the story for printing the Python sources.

        - The page **footer is automatically changed** to show the name of the current Python file.

    * Use of :meth:`Story.element_positions` to collect the data for the TOC and for the dynamic adjustment of page footers. This is an example of a **bidirectional communication** between the story output process and the script.

    * The main PDF with the Python sources is being written to memory by its :ref:`DocumentWriter`. Another :ref:`Story` / :ref:`DocumentWriter` pair is then used to create a (memory) PDF for the TOC pages. Finally, both these PDFs are joined and the result stored to disk.


-----


.. _RecipesStories_K:

如何显示 JSON 数据列表
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Display a List from JSON Data

.. tab:: 中文

    此示例采用一些 JSON 数据输入，用于填充 :ref:`Story`。它还包含一些可视文本格式，并展示了如何添加链接。

    **文件：**

    * `docs/samples/json-example.py`

.. tab:: 英文

    This example takes some JSON data input which it uses to populate a :ref:`Story`. It also contains some visual text formatting and shows how to add links.

    **Files:**

    * `docs/samples/json-example.py`

|toggleStart|

.. literalinclude:: samples/json-example.py

|toggleEnd|



-----


.. _RecipesStories_L:

使用替代的 :meth:`Story.write*()` 函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Using the Alternative :meth:`Story.write*()` functions

.. tab:: 中文

    :meth:`Story.write*()` 函数提供了一种使用
    :ref:`Story` 功能的另一种方式，无需调用代码来实现
    一个调用 :meth:`Story.place()` 和 :meth:`Story.draw()` 等的循环，但
    必须至少提供一个 `rectfn()` 回调。

.. tab:: 英文


    The :meth:`Story.write*()` functions provide a different way to use the
    :ref:`Story` functionality, removing the need for calling code to implement
    a loop that calls :meth:`Story.place()` and :meth:`Story.draw()` etc, at the
    expense of having to provide at least a `rectfn()` callback.


.. _RecipesStories_L_a:

如何使用 :meth:`Story.write()` 进行基本布局
-------------------------------------------------

How to do Basic Layout with :meth:`Story.write()`

.. tab:: 中文

    此脚本将其自身源代码的多个副本布局为每页四个矩形。

    **文件：**

    * `docs/samples/story-write.py`

.. tab:: 英文

    This script lays out multiple copies of its own source code, into four
    rectangles per page.

    **Files:**

    * `docs/samples/story-write.py`

|toggleStart|

.. literalinclude:: samples/story-write.py

|toggleEnd|


-----


.. _RecipesStories_L_b:

如何使用 :meth:`Story.write_stabilized()` 对目录进行迭代布局
----------------------------------------------------------------------------------------

How to do Iterative Layout for a Table of Contents with :meth:`Story.write_stabilized()`

.. tab:: 中文

    此脚本动态创建 html 内容，并根据具有非零 `.heading` 值的 :ref:`ElementPosition` 项添加内容部分。

    内容部分位于文档的开头，因此对内容的修改可能会更改文档其余部分的页码，进而导致内容部分的页码不正确。

    因此，脚本使用 :meth:`Story.write_stabilized()` 反复布局，直到一切稳定。

    **文件：**

    * `docs/samples/story-write-stabilized.py`

.. tab:: 英文


    This script creates html content dynamically, adding a contents section based
    on :ref:`ElementPosition` items that have non-zero `.heading` values.

    The contents section is at the start of the document, so modifications to the
    contents can change page numbers in the rest of the document, which in turn can
    cause page numbers in the contents section to be incorrect.

    So the script uses :meth:`Story.write_stabilized()` to repeatedly lay things
    out until things are stable.


    **Files:**

    * `docs/samples/story-write-stabilized.py`

|toggleStart|

.. literalinclude:: samples/story-write-stabilized.py

|toggleEnd|



-----

.. _RecipesStories_L_c:

如何使用 :meth:`Story.write_stabilized_links()` 进行迭代布局和创建 PDF 链接
-------------------------------------------------------------------------------------------

How to do Iterative Layout and Create PDF Links with :meth:`Story.write_stabilized_links()`

.. tab:: 中文

    此脚本与上面 “如何使用 :meth:`Story.write_stabilized()` ” 中描述的脚本类似，不同之处在于生成的 PDF 还包含与原始 html 中的内部链接相对应的链接。

    这是通过使用 :meth:`Story.write_stabilized_links()` 完成的；这与 :meth:`Story.write_stabilized()` 略有不同：

    * 它不接受 :ref:`DocumentWriter` `writer` 参数。
    * 它返回 PDF :ref:`Document` 实例。

    [原因有点复杂；例如 :ref:`DocumentWriter` 不一定是 PDF 编写器，因此在 PDF 特定的 API 中实际上不起作用。]

    **文件：**

    * `docs/samples/story-write-stabilized-links.py`

.. tab:: 英文


    This script is similar to the one described in "How to use
    :meth:`Story.write_stabilized()`" above, except that the generated PDF also
    contains links that correspond to the internal links in the original html.

    This is done by using :meth:`Story.write_stabilized_links()`; this is slightly
    different from :meth:`Story.write_stabilized()`:

    * It does not take a :ref:`DocumentWriter` `writer` arg.
    * It returns a PDF :ref:`Document` instance.

    [The reasons for this are a little involved; for example a
    :ref:`DocumentWriter` is not necessarily a PDF writer, so doesn't really work
    in a PDF-specific API.]


    **Files:**

    * `docs/samples/story-write-stabilized-links.py`

|toggleStart|

.. literalinclude:: samples/story-write-stabilized-links.py

|toggleEnd|



-----


.. rubric:: Footnotes

.. [#f1] HTML & CSS support

    .. note::

        At the time of writing the HTML engine for Stories is fairly basic and supports a subset of CSS2 attributes.

    Some important CSS support to consider:

    - The only available layout is relative layout.
    - `background` is unavailable, use `background-color` instead.
    - `float` is unavailable.

.. [#f2] HTML & CSS support

    .. note::

    在编写 Stories 的 HTML 引擎时，它相当基础，并且支持 CSS2 属性的子集。

    需要考虑的一些重要 CSS 支持：

    - 唯一可用的布局是相对布局。
    - `background` 不可用，请改用 `background-color`。
    - `float` 不可用。


.. include:: footer.rst

.. External Links:

.. _pymupdf-fonts: https://github.com/pymupdf/pymupdf-fonts



