.. include:: header.rst

.. _Story:

================
Story
================

.. role:: htmlTag(emphasis)

.. tab:: 中文

    * 新增于 v1.21.0

    =========================================== =============================================================
    **方法 / 属性**                             **简短描述**
    =========================================== =============================================================
    :meth:`Story.reset`                         "重置"故事输出至其起始位置
    :meth:`Story.place`                         计算故事内容以适应提供的矩形区域
    :meth:`Story.draw`                          将计算出的内容写入当前页面
    :meth:`Story.element_positions`             回调函数，用于记录当前处理的故事内容
    :attr:`Story.body`                          故事的底层 :htmlTag:`body`
    :meth:`Story.write`                         将故事放置并绘制到 DocumentWriter
    :meth:`Story.write_stabilized`              迭代布局HTML内容到 DocumentWriter
    :meth:`Story.write_with_links`              类似于 `write()`，但也创建PDF链接
    :meth:`Story.write_stabilized_with_links`   类似于 `write_stabilized()`，但也创建PDF链接
    :meth:`Story.fit`                           查找包含故事 `self` 的最佳矩形
    :meth:`Story.fit_scale`                     
    :meth:`Story.fit_height`
    :meth:`Story.fit_width`
    =========================================== =============================================================

    **Class API**

    .. class:: Story

        .. method:: __init__(self, html=None, user_css=None, em=12, archive=None)

            创建一个 **story** （故事），可选地提供 HTML 和 CSS 源代码。
            HTML 将被解析，并作为 DOM（文档对象模型）存储在 Story 中。

            该结构可以被修改：可以使用 :ref:`Xml` 类的方法添加、复制、修改或删除内容（文本、图像）。

            完成后， **story** 可以写入任何设备；
            在典型的使用场景中，该设备可能由 :ref:`DocumentWriter` 提供，以创建新页面。

            以下是一些一般性说明：

            * :ref:`Story` 构造函数会解析并验证提供的 HTML 以创建 DOM。
            * PyMuPDF 提供了多种方法来操作 HTML 源代码，允许访问底层 DOM 的 *节点*。文档可以完全从头开始以编程方式构建，或者对现有 DOM 进行任意修改。有关该接口的详细信息，请参阅 :ref:`Xml` 类。
            * 如果不再需要对 DOM 进行更改，则 story 准备好进行布局，并可写入一系列设备（通常是由 :ref:`DocumentWriter` 提供的设备，以生成新页面）。
            * 下一步是放置 story 并将其写入输出。这可以直接完成，通过循环调用 `place()` 和 `draw()`，或者使用 `write()` 或 `write_stabilized()` 方法让它自动处理循环。选择哪种方式主要取决于个人偏好。
                
                * 如果选择手动方式，建议使用以下循环：
                
                1. 获取一个合适的设备进行写入；通常通过从 :ref:`DocumentWriter` 请求一个新的空白页面。
                2. 确定页面上的一个或多个矩形区域，这些区域将用于接收 **story** 数据。请注意，并非每个页面都需要使用相同的矩形区域集。
                3. 将每个矩形传递给 **story** 以进行放置，了解该矩形的填充情况，以及是否还有未适应的 story 数据。这一步可以重复多次，并调整矩形区域，直到获得满意的结果。 
                4. 在此阶段，可以通过调用 `element_positions()` 方法请求有关已放置项目的位置的详细信息。如果 `heading` 属性为非零整数（对应于 HTML 标签 :htmlTag:`h1` - :htmlTag:`h6`），或者 `id` 属性不为 `None` （对应于 HTML 标签 :htmlTag:`id`），或者 `href` 属性不为 `None` （对应于 HTML 标签 :htmlTag:`href`），则该元素被视为“感兴趣的元素”。这对于自动生成目录（Table of Contents）、索引或类似用途非常方便。
                5. 使用 `draw()` 方法将该矩形绘制到设备上。
                6. 如果最近一次 `place()` 调用表明所有 story 数据均已适应，则停止。
                7. 否则，循环回到前面：如果当前设备（页面）上还有更多的矩形需要填充，则返回到步骤 3；如果没有，则返回步骤 1 以获取新设备（页面）。

                * 另一种方式，如果使用 :ref:`DocumentWriter`，
                可以使用 `write()` 或 `write_stabilized()` 方法。
                这些方法会自动处理所有循环，
                只需提供控制行为的回调（主要是一个枚举矩形/页面的回调函数）。

            * **story** 的内容将如何分布到哪些矩形 / 页面，完全由 :ref:`Story` 对象控制，无法预知。
            * 图像可以包含在 **story** 中，并与周围文本一起放置。
            * 多个 story 可以独立地写入同一页面。例如，可以使用不同的 story 分别处理页眉、页脚、正文文本、注释框等。

            :arg str html: HTML 源代码。如果省略，将生成一个基本的最小结构（见下文）。
                如果提供，则无需是完整的 HTML 文档。
                内置解析器会容忍（大多数）HTML 语法错误，并接受 HTML 片段，如
                `"<b>Hello, <i>World!</i></b>"`。
            :arg str user_css: CSS 源代码。如果提供，则必须包含有效的 CSS 规则。
            :arg float em: 默认文本字体大小。
            :arg archive: 一个 :ref:`Archive` 对象，用于加载渲染资源。
                目前支持的资源类型包括图像和字体。
                如果省略，story 将不会查找任何此类数据，因此可能会导致输出不完整。

            .. note:: 除了实际的 archive 对象，也可以提供创建 :ref:`Archive` 的有效参数。
                例如，`story = pymupdf.Story(archive=pymupdf.Archive("myfolder"))` 可以简化为
                `story = pymupdf.Story(archive="myfolder")`。

        .. method:: place(where)

            计算 story 内容中可以适应提供矩形的部分。
            该方法会维护一个指针，指示 story 内容已经写入的位置，
            并在下一次调用时从该指针处继续。

            :arg rect_like where: 将当前内容布局到该矩形中。该矩形必须是页面 :ref:`MediaBox<Glossary_MediaBox>` 的子矩形。

            :rtype: tuple[bool, rect_like]
            :returns: 一个布尔值 `more` 和一个矩形 `filled`。
                如果 `more == 0`，则 story 的所有内容均已写入；
                否则，仍有内容等待写入后续矩形或页面。
                `filled` 是 `where` 中实际被填充的部分。

        .. method:: draw(dev, matrix=None)

            将 :meth:`Story.place` 计算出的内容写入页面。

            :arg dev: 由 `dev = writer.begin_page(mediabox)` 创建的 :ref:`Device`。
                该设备知道如何调用所有必要的 MuPDF 函数以写入内容。
            :arg matrix_like matrix: 写入页面时用于变换内容的矩阵，例如可用于旋转文本。
                默认值表示不进行变换（即使用 :ref:`Identity` 矩阵）。

        .. method:: element_positions(function, args=None)

            在 story 计算出 HTML 元素在当前页面的位置后，
            让它提供这些信息 - **必须在** :meth:`Story.place` **之后立即调用**。

            *Story* 将位置信息传递给 *function*，例如可用于生成目录（Table of Contents）。

            :arg callable function: 接受一个 :class:`ElementPosition` 对象的 Python 函数。
                该函数会被 Story 对象调用来处理位置信息。
                该函数 **必须** 是一个接受 **唯一** 一个参数的可调用对象。
            :arg dict args: 可选字典，包含应添加到 `ElementPosition` 实例中的 **额外信息**，
                例如当前输出页面编号。
                该字典的每个键必须是符合 Python 标识符规则的字符串。
                下面会详细说明完整的信息集。

        .. method:: reset()

            将 story 文档倒回到起始位置，以便重新开始输出。

        .. attribute:: body

            story DOM 的 :htmlTag:`body` 部分。
            该属性包含 :htmlTag:`body` 的 :ref:`Xml` 节点。
            所有 PDF 生产相关的内容都包含在 "<body>" 和 "</body>" 之间。

        .. method:: write(writer, rectfn, positionfn=None, pagefn=None)

            将 story 放置并绘制到 `DocumentWriter`，避免调用代码
            需要手动实现循环来调用 `Story.place()` 和 `Story.draw()`。

            :arg writer: `DocumentWriter` 实例，或 `None`。
            :arg rectfn: 一个可调用对象，接受 `(rect_num: int, filled: Rect)` 并返回 `(mediabox, rect, ctm)`：
                
                * mediabox: `None` 或者表示新页面的矩形。
                * rect: 下一块应放置内容的矩形区域。
                * ctm: `None` 或 `Matrix` 变换矩阵。

            :arg positionfn: `None`，或一个可调用对象，接受 `(position: ElementPosition)`：
                
                * position: 一个 `ElementPosition` 实例，具有额外的 `.page_num` 成员。
                该函数通常会在生成标题元素或具有 `id` 的元素时被多次调用。

            :arg pagefn: `None`，或一个可调用对象，接受 `(page_num, mediabox, dev, after)`：
                在每个页面的开始 (`after=0`) 和结束 (`after=1`) 处调用。

        .. staticmethod:: write_stabilized(writer, contentfn, rectfn, user_css=None, em=12, positionfn=None, pagefn=None, archive=None, add_header_ids=True)

            静态方法，对 HTML 内容进行迭代布局，并写入 `DocumentWriter`。

            例如，这允许在确保页码稳定的情况下添加目录（Table of Contents）部分。

            该方法会重复执行以下操作：

            1. 使用 `(contentfn(), user_css, em, archive)` 创建新的 `Story` 实例。
            2. 通过内部调用 `Story.write()` 进行布局，并使用 `None` 作为 writer 提取 `ElementPosition` 列表，该列表会传递给下一次 `contentfn()` 调用。
            3. 当 `contentfn()` 生成的 HTML 内容不再变化时，执行最终迭代，并使用 `writer` 进行最终写入。

            :arg writer:
                一个 `DocumentWriter`。
            :arg contentfn:
                一个函数，接受 `ElementPositions` 列表，并返回包含 HTML 内容的字符串。
                返回的 HTML 可以依赖于该列表，例如用于在文档开头生成目录。
            :arg rectfn:
                一个可调用对象，接受 `(rect_num: int, filled: Rect)` 并返回 `(mediabox, rect, ctm)`：

                * mediabox: `None` 或者新页面的矩形。
                * rect: 下一个应放置内容的矩形区域。
                * ctm: `Matrix` 变换矩阵。

            :arg pagefn:
                `None`，或一个可调用对象，接受 `(page_num, mediabox, dev, after)`；
                在每个页面的开始 (`after=0`) 和结束 (`after=1`) 处调用。

            :arg archive:
                资源存储 `Archive`，可用于加载图像和字体。

            :arg add_header_ids:
                若为 `True`，则对所有没有 `id` 的 HTML 标题标签（如 `<h1>`-`<h6>`）添加唯一的 `id`，
                以便自动生成目录。

            :returns:
                `None`。

        .. method:: write_with_links(rectfn, positionfn=None, pagefn=None)

            类似于 `write()`，但没有 `writer` 参数，
            并返回一个包含内部 HTML 链接的 PDF `Document`。

        .. staticmethod:: write_stabilized_with_links(contentfn, rectfn, user_css=None, em=12, positionfn=None, pagefn=None, archive=None, add_header_ids=True)

            类似于 `write_stabilized()`，但没有 `writer` 参数，
            而是返回一个包含内部 HTML 链接的 PDF `Document`。

        .. class:: Story.FitResult

            `Story.fit*()` 方法的返回结果。

            成员:

            `big_enough`:
                若 `True`，表示内容适应成功。

            `filled`:
                最后一次 `Story.place()` 调用填充的矩形。

            `more`:
                若 `False`，表示适应成功。

            `numcalls`:
                调用了 `self.place()` 的次数。

            `parameter`:
                适应成功的参数值，或最大的失败参数值。

            `rect`:
                由 `parameter` 生成的矩形。
                
        .. method:: fit(self, fn, pmin=None, pmax=None, delta=0.001, verbose=False)

            查找能包含 `self` 的最优矩形。

            返回一个 `Story.FitResult` 实例。

            成功时，`self.place()` 的最后一次调用会使用返回的矩形，因此可以直接调用 `self.draw()` 进行绘制。

            :arg fn:
                一个可调用对象，接受一个浮点数 `parameter` 并返回 `pymupdf.Rect()`。
                如果返回的矩形为空，则认为故事无法适应该矩形，并不会调用 `self.place()`。

                该函数必须保证 `self.place()` 在 `fn(parameter)` 增大时单调递增。
                这通常意味着宽度和高度随着 `parameter` 的增大而增大或保持不变。

            :arg pmin:
                要考虑的最小参数值；`None` 表示负无穷大。

            :arg pmax:
                要考虑的最大参数值；`None` 表示正无穷大。

            :arg delta:
                返回 `parameter` 的最大误差。

            :arg verbose:
                若为 `True`，则输出诊断信息。

        .. method:: fit_scale(self, rect, scale_min=0, scale_max=None, delta=0.001, verbose=False)

            查找范围 `scale_min..scale_max` 内的最小 `scale` 值，使得 `scale * rect`
            足够大，可以包含 `self`。

            返回一个 `Story.FitResult` 实例。

            :arg rect:
                原始矩形。

            :arg scale_min:
                需要考虑的最小缩放比例，必须大于等于 `0`。

            :arg scale_max:
                需要考虑的最大缩放比例，必须大于等于 `scale_min`，或 `None` 表示无限。

            :arg delta:
                返回的缩放比例的最大误差。

            :arg verbose:
                若为 `True`，则输出诊断信息。

        .. method:: fit_height(self, width, height_min=0, height_max=None, origin=(0, 0), delta=0.001, verbose=False)

            查找范围 `height_min..height_max` 内的最小高度，使得 `(width, height)` 的矩形足够大，
            可以包含 `self`。

            返回一个 `Story.FitResult` 实例。

            :arg width:
                矩形的宽度。

            :arg height_min:
                需要考虑的最小高度，必须大于等于 `0`。

            :arg height_max:
                需要考虑的最大高度，必须大于等于 `height_min`，或 `None` 表示无限。

            :arg origin:
                矩形的 `(x0, y0)` 坐标。

            :arg delta:
                返回的高度的最大误差。

            :arg verbose:
                若为 `True`，则输出诊断信息。

        .. method:: fit_width(self, height, width_min=0, width_max=None, origin=(0, 0), delta=0.001, verbose=False)

            查找范围 `width_min..width_max` 内的最小宽度，使得 `(width, height)` 的矩形足够大，
            可以包含 `self`。

            返回一个 `Story.FitResult` 实例。

            :arg height:
                矩形的高度。

            :arg width_min:
                需要考虑的最小宽度，必须大于等于 `0`。

            :arg width_max:
                需要考虑的最大宽度，必须大于等于 `width_min`，或 `None` 表示无限。

            :arg origin:
                矩形的 `(x0, y0)` 坐标。

            :arg delta:
                返回的宽度的最大误差。

            :arg verbose:
                若为 `True`，则输出诊断信息。




.. tab:: 英文

    * New in v1.21.0

    =========================================== =============================================================
    **Method / Attribute**                      **Short Description**
    =========================================== =============================================================
    :meth:`Story.reset`                         "rewind" story output to its beginning
    :meth:`Story.place`                         compute story content to fit in provided rectangle
    :meth:`Story.draw`                          write the computed content to current page
    :meth:`Story.element_positions`             callback function logging currently processed story content
    :attr:`Story.body`                          the story's underlying :htmlTag:`body`
    :meth:`Story.write`                         places and draws Story to a DocumentWriter
    :meth:`Story.write_stabilized`              iterative layout of html content to a DocumentWriter
    :meth:`Story.write_with_links`              like `write()` but also creates PDF links
    :meth:`Story.write_stabilized_with_links`   like `write_stabilized()` but also creates PDF links
    :meth:`Story.fit`                           Finds optimal rect that contains the story `self`.
    :meth:`Story.fit_scale`                     
    :meth:`Story.fit_height`
    :meth:`Story.fit_width`
    =========================================== =============================================================

    **Class API**

    .. class:: Story
        :no-index:

        .. method:: __init__(self, html=None, user_css=None, em=12, archive=None)
            :no-index:

            Create a **story**, optionally providing HTML and CSS source.
            The HTML is parsed, and held within the Story as a DOM (Document Object Model).

            This structure may be modified: content (text, images) may be added,
            copied, modified or removed by using methods of the :ref:`Xml` class.

            When finished, the **story** can be written to any device;
            in typical usage the device may be provided by a :ref:`DocumentWriter` to make new pages.

            Here are some general remarks:

            * The :ref:`Story` constructor parses and validates the provided HTML to create the DOM.
            * PyMuPDF provides a number of ways to manipulate the HTML source by
                providing access to the *nodes* of the underlying DOM.
                Documents can be completely built from ground up programmatically,
                or the existing DOM can be modified pretty arbitrarily.
                For details of this interface, please see the :ref:`Xml` class.
            * If no (or no more) changes to the DOM are required,
                the story is ready to be laid out and to be fed to a series of devices
                (typically devices provided by a :ref:`DocumentWriter` to produce new pages).
            * The next step is to place the story and write it out.
                This can either be done directly, by looping around calling `place()` and `draw()`,
                or alternatively,
                the looping can handled for you using the `write()` or `write_stabilised()` methods.
                Which method you choose is largely a matter of taste.
                
                * To work in the first of these styles, the following loop should be used:
                
                1. Obtain a suitable device to write to;
                    typically by requesting a new,
                    empty page from a :ref:`DocumentWriter`.
                2. Determine one or more rectangles on the page,
                    that should receive **story** data.
                    Note that not every page needs to have the same set of rectangles.
                3. Pass each rectangle to the **story** to place it,
                    learning what part of that rectangle has been filled,
                    and whether there is more story data that did not fit.
                    This step can be repeated several times with adjusted rectangles
                    until the caller is happy with the results. 
                4. Optionally, at this point,
                    we can request details of where interesting items have been placed,
                    by calling the `element_positions()` method.
                    Items are deemed to be interesting if their integer `heading` attribute is a non-zero
                    (corresponding to HTML tags :htmlTag:`h1` - :htmlTag:`h6`),
                    if their `id` attribute is not `None` (corresponding to HTML tag :htmlTag:`id`),
                    or if their `href` attribute is not `None` (responding to HTML tag :htmlTag:`href`).
                    This can conveniently be used for automatic generation of a Table of Contents,
                    an index of images or the like.
                5. Next, draw that rectangle out to the device with the `draw()` method.
                6. If the most recent call to `place()` indicated that all the story data had fitted,
                    stop now.
                7. Otherwise, we can loop back.
                    If there are more rectangles to be placed on the current device (page),
                    we jump back to step 3 - if not, we jump back to step 1 to get a new device.
                * Alternatively, in the case where you are using a :ref:`DocumentWriter`,
                the `write()` or `write_stabilized()` methods can be used.
                These handle all the looping for you,
                in exchange for being provided with callbacks that control the behaviour
                (notably a callback that enumerates the rectangles/pages to use).
            * Which part of the **story** will land on which rectangle / which page,
                is fully under control of the :ref:`Story` object and cannot be predicted.
            * Images may be part of a **story**. They will be placed together with any surrounding text.
            * Multiple stories may - independently from each other - write to the same page.
                For example, one may have separate stories for page header,
                page footer, regular text, comment boxes, etc.

            :arg str html: HTML source code. If omitted, a basic minimum is generated (see below).
                If provided, not a complete HTML document is needed.
                The in-built source parser will forgive (many / most)
                HTML syntax errors and also accepts HTML fragments like
                `"<b>Hello, <i>World!</i></b>"`.
            :arg str user_css: CSS source code. If provided, must contain valid CSS specifications.
            :arg float em: the default text font size.
            :arg archive: an :ref:`Archive` from which to load resources for rendering. Currently supported resource types are images and text fonts. If omitted, the story will not try to look up any such data and may thus produce incomplete output.
            
            .. note:: Instead of an actual archive, valid arguments for **creating** an :ref:`Archive` can also be provided -- in which case an archive will temporarily be constructed. So, instead of `story = pymupdf.Story(archive=pymupdf.Archive("myfolder"))`, one can also shorter write `story = pymupdf.Story(archive="myfolder")`.

        .. method:: place(where)
            :no-index:

            Calculate that part of the story's content, that will fit in the provided rectangle. The method maintains a pointer which part of the story's content has already been written and upon the next invocation resumes from that pointer's position.

            :arg rect_like where: layout the current part of the content to fit into this rectangle. This must be a sub-rectangle of the page's :ref:`MediaBox<Glossary_MediaBox>`.

            :rtype: tuple[bool, rect_like]
            :returns: a bool (int) `more` and a rectangle `filled`. If `more == 0`, all content of the story has been written, otherwise more is waiting to be written to subsequent rectangles / pages. Rectangle `filled` is the part of `where` that has actually been filled.

        .. method:: draw(dev, matrix=None)
            :no-index:

            Write the content part prepared by :meth:`Story.place` to the page.

            :arg dev: the :ref:`Device` created by `dev = writer.begin_page(mediabox)`. The device knows how to call all MuPDF functions needed to write the content.
            :arg matrix_like matrix: a matrix for transforming content when writing to the page. An example may be writing rotated text. The default means no transformation (i.e. the :ref:`Identity` matrix).

        .. method:: element_positions(function, args=None)
            :no-index:

            Let the Story provide positioning information about certain HTML elements once their place on the current page has been computed - i.e. invoke this method **directly after** :meth:`Story.place`.

            *Story* will pass position information to *function*. This information can for example be used to generate a Table of Contents.

            :arg callable function: a Python function accepting an :class:`ElementPosition` object. It will be invoked by the Story object to process positioning information. The function **must** be a callable accepting exactly one argument.
            :arg dict args: an optional dictionary with any **additional** information that should be added to the :class:`ElementPosition` instance passed to `function`.
            Like for example the current output page number.
            Every key in this dictionary must be a string that conforms to the rules for a valid Python identifier.
            The complete set of information is explained below.


        .. method:: reset()
            :no-index:

            Rewind the story's document to the beginning for starting over its output.

        .. attribute:: body
            :no-index:

            The :htmlTag:`body` part of the story's DOM. This attribute contains the :ref:`Xml` node of :htmlTag:`body`. All relevant content for PDF production is contained between "<body>" and "</body>".

        .. method:: write(writer, rectfn, positionfn=None, pagefn=None)
            :no-index:

            Places and draws Story to a `DocumentWriter`. Avoids the need for
            calling code to implement a loop that calls `Story.place()` and
            `Story.draw()` etc, at the expense of having to provide at least the
            `rectfn()` callback.
        
            :arg writer: a `DocumentWriter` or None.
            :arg rectfn: a callable taking `(rect_num: int, filled: Rect)` and
                returning `(mediabox, rect, ctm)`:
                
                * mediabox: None or rect for new page.
                * rect: The next rect into which content should be placed.
                * ctm: None or a `Matrix`.
            :arg positionfn: None, or a callable taking `(position: ElementPosition)`:
                
                * position:
                    An `ElementPosition` with an extra `.page_num` member.
                Typically called multiple times as we generate elements that
                are headings or have an id.
            :arg pagefn:
                None, or a callable taking `(page_num, mediabox, dev, after)`;
                called at start (`after=0`) and end (`after=1`) of each page.

        .. staticmethod:: write_stabilized(writer, contentfn, rectfn, user_css=None, em=12, positionfn=None, pagefn=None, archive=None, add_header_ids=True)
            :no-index:
        
            Static method that does iterative layout of html content to a
            `DocumentWriter`.

            For example this allows one to add a table of contents section
            while ensuring that page numbers are patched up until stable.

            Repeatedly creates a new `Story` from `(contentfn(),
            user_css, em, archive)` and lays it out with internal call
            to `Story.write()`; uses a None writer and extracts the list
            of `ElementPosition`'s which is passed to the next call of
            `contentfn()`.

            When the html from `contentfn()` becomes unchanged, we do a
            final iteration using `writer`.

            :arg writer:
                A `DocumentWriter`.
            :arg contentfn:
                A function taking a list of `ElementPositions` and
                returning a string containing html. The returned html
                can depend on the list of positions, for example with a
                table of contents near the start.
            :arg rectfn:
                A callable taking `(rect_num: int, filled: Rect)` and
                returning `(mediabox, rect, ctm)`:

                * mediabox: None or rect for new page.
                * rect: The next rect into which content should be placed.
                * ctm: A `Matrix`.
            :arg pagefn:
                None, or a callable taking `(page_num, medibox,
                dev, after)`; called at start (`after=0`) and end
                (`after=1`) of each page.
            :arg archive:
            :arg add_header_ids:
                If true, we add unique ids to all header tags that
                don't already have an id. This can help automatic
                generation of tables of contents.
            Returns:
                None.
            
        .. method:: write_with_links(rectfn, positionfn=None, pagefn=None)
            :no-index:

            Similar to `write()` except that we don't have a `writer` arg
            and we return a PDF `Document` in which links have been created
            for each internal html link.

        .. staticmethod:: write_stabilized_with_links(contentfn, rectfn, user_css=None, em=12, positionfn=None, pagefn=None, archive=None, add_header_ids=True)
            :no-index:

            Similar to `write_stabilized()` except that we don't have a `writer`
            arg and instead return a PDF `Document` in which links have been
            created for each internal html link.
            
        .. class:: Story.FitResult
            :no-index:
            
            The result from a `Story.fit*()` method.
            
            Members:
            
            `big_enough`:
                `True` if the fit succeeded.
            `filled`:
                From the last call to `Story.place()`.
            `more`:
                `False` if the fit succeeded.
            `numcalls`:
                Number of calls made to `self.place()`.
            `parameter`:
                The successful parameter value, or the largest failing value.
            `rect`:
                The rect created from `parameter`.
                
        .. method:: fit(self, fn, pmin=None, pmax=None, delta=0.001, verbose=False)
            :no-index:

            Finds optimal rect that contains the story `self`.
            
            Returns a `Story.FitResult` instance.
                
            On success, the last call to `self.place()` will have been with the
            returned rectangle, so `self.draw()` can be used directly.
            
            :arg fn:
                A callable taking a floating point `parameter` and returning a
                `pymupdf.Rect()`. If the rect is empty, we assume the story will
                not fit and do not call `self.place()`.

                Must guarantee that `self.place()` behaves monotonically when
                given rect `fn(parameter`) as `parameter` increases. This
                usually means that both width and height increase or stay
                unchanged as `parameter` increases.
            :arg pmin:
                Minimum parameter to consider; `None` for -infinity.
            :arg pmax:
                Maximum parameter to consider; `None` for +infinity.
            :arg delta:
                Maximum error in returned `parameter`.
            :arg verbose:
                If true we output diagnostics.

        .. method:: fit_scale(self, rect, scale_min=0, scale_max=None, delta=0.001, verbose=False)
            :no-index:

            Finds smallest value `scale` in range `scale_min..scale_max` where
            `scale * rect` is large enough to contain the story `self`.

            Returns a `Story.FitResult` instance.

            :arg width:
                width of rect.
            :arg height:
                height of rect.
            :arg scale_min:
                Minimum scale to consider; must be >= 0.
            :arg scale_max:
                Maximum scale to consider, must be >= scale_min or `None` for
                infinite.
            :arg delta:
                Maximum error in returned scale.
            :arg verbose:
                If true we output diagnostics.

        .. method:: fit_height(self, width, height_min=0, height_max=None, origin=(0, 0), delta=0.001, verbose=False)
            :no-index:

            Finds smallest height in range `height_min..height_max` where a rect
            with size `(width, height)` is large enough to contain the story
            `self`.

            Returns a `Story.FitResult` instance.

            :arg width:
                width of rect.
            :arg height_min:
                Minimum height to consider; must be >= 0.
            :arg height_max:
                Maximum height to consider, must be >= height_min or `None` for
                infinite.
            :arg origin:
                `(x0, y0)` of rect.
            :arg delta:
                Maximum error in returned height.
            :arg verbose:
                If true we output diagnostics.

        .. method:: fit_width(self, height, width_min=0, width_max=None, origin=(0, 0), delta=0.001, verbose=False)
            :no-index:

            Finds smallest width in range `width_min..width_max` where a rect with size
            `(width, height)` is large enough to contain the story `self`.

            Returns a `Story.FitResult` instance.

            :arg height:
                height of rect.
            :arg width_min:
                Minimum width to consider; must be >= 0.
            :arg width_max:
                Maximum width to consider, must be >= width_min or `None` for
                infinite.
            :arg origin:
                `(x0, y0)` of rect.
            :arg delta:
                Maximum error in returned width.
            :arg verbose:
                If true we output diagnostics.


元素定位回调函数
--------------------------------------

Element Positioning CallBack function

.. tab:: 中文

    回调函数可用于记录关于故事输出的信息。该函数对信息的访问是只读的：它无法影响故事的输出。

    使用此方法执行故事的典型循环如下所示::

        HTML = """
        <html>
            <head></head>
            <body>
                <h1>Header level 1</h1>
                <h2>Header level 2</h2>
                <p>Hello MuPDF!</p>
            </body>
        </html>
        """
        MEDIABOX = pymupdf.paper_rect("letter")  # 页面大小
        WHERE = MEDIABOX + (36, 36, -36, -36)  # 留出0.5英寸的边框
        story = pymupdf.Story(html=HTML)  # 创建故事
        writer = pymupdf.DocumentWriter("test.pdf")  # 创建写入器
        pno = 0  # 当前页码
        more = 1  # 完成时设置为0
        while more:  # 循环，直到所有故事内容处理完
            dev = writer.begin_page(MEDIABOX)  # 创建设备以在页面上写入
            more, filled = story.place(WHERE)  # 计算页面上的内容位置
            story.element_positions(recorder, {"page": pno})  # 另外提供页面号码
            story.draw(dev)
            writer.end_page()
            pno += 1  # 增加页码
        writer.close()  # 关闭输出文件

        def recorder(elpos):
            pass


.. tab:: 英文

    The callback function can be used to log information about story output. The function's access to the information is read-only: it has no way to influence the story's output.

    A typical loop for executing a story with using this method would look like this::

        HTML = """
        <html>
            <head></head>
            <body>
                <h1>Header level 1</h1>
                <h2>Header level 2</h2>
                <p>Hello MuPDF!</p>
            </body>
        </html>
        """
        MEDIABOX = pymupdf.paper_rect("letter")  # size of a page
        WHERE = MEDIABOX + (36, 36, -36, -36)  # leave borders of 0.5 inches
        story =  pymupdf.Story(html=HTML)  # make the story
        writer = pymupdf.DocumentWriter("test.pdf")  # make the writer
        pno = 0 # current page number
        more = 1  # will be set to 0 when done
        while more:  # loop until all story content is processed
            dev = writer.begin_page(MEDIABOX)  # make a device to write on the page
            more, filled = story.place(WHERE)  # compute content positions on page
            story.element_positions(recorder, {"page": pno})  # provide page number in addition
            story.draw(dev)
            writer.end_page()
            pno += 1  # increase page number
        writer.close()  # close output file

        def recorder(elpos):
            pass


ElementPosition 类的属性
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Attributes of the ElementPosition class

.. tab:: 中文

    必须向 :meth:`Story.element_positions` 提供的函数传递一个参数。它是一个具有以下属性的对象：

    传递给 `recorder` 函数的参数是一个具有以下属性的对象：

    * `elpos.depth` (int) -- 该元素在盒子结构中的深度。

    * `elpos.heading` (int) -- 标题级别，0 表示没有标题，1-6 对应于 :htmlTag:`h1` - :htmlTag:`h6`。

    * `elpos.href` (str) -- `href` 属性的值，如果未定义，则为 None。

    * `elpos.id` (str) -- `id` 属性的值，如果未定义，则为 None。

    * `elpos.rect` (tuple) -- 元素在页面上的位置。

    * `elpos.text` (str) -- 元素的即时文本。

    * `elpos.open_close` (int bit field) -- 位 0 设置：打开元素，位 1 设置：关闭元素。与可能包含其他元素的元素相关，因此这些元素可能在创建/打开后不能立即关闭。

    * `elpos.rect_num` (int) -- 到目前为止故事中填充的矩形数量。

    * `elpos.page_num` (int) -- 页码；仅在使用 `pymupdf.Story.write*()` 函数时存在。


.. tab:: 英文

    Exactly one parameter must be passed to the function provided by :meth:`Story.element_positions`. It is an object with the following attributes:

    The parameter passed to the `recorder` function is an object with the following attributes:

    * `elpos.depth` (int) -- depth of this element in the box structure.

    * `elpos.heading` (int) -- the header level, 0 if no header, 1-6 for :htmlTag:`h1` - :htmlTag:`h6`.

    * `elpos.href` (str) -- value of the `href` attribute, or None if not defined.

    * `elpos.id` (str) -- value of the `id` attribute, or None if not defined.

    * `elpos.rect` (tuple) -- element position on page.

    * `elpos.text` (str) -- immediate text of the element.

    * `elpos.open_close` (int bit field) -- bit 0 set: opens element, bit 1 set: closes element. Relevant for elements that may contain other elements and thus may not immediately be closed after being created / opened.

    * `elpos.rect_num` (int) -- count of rectangles filled by the story so far.

    * `elpos.page_num` (int) -- page number; only present when using `pymupdf.Story.write*()` functions.

.. include:: footer.rst
