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

            Calculate that part of the story's content, that will fit in the provided rectangle. The method maintains a pointer which part of the story's content has already been written and upon the next invocation resumes from that pointer's position.

            :arg rect_like where: layout the current part of the content to fit into this rectangle. This must be a sub-rectangle of the page's :ref:`MediaBox<Glossary_MediaBox>`.

            :rtype: tuple[bool, rect_like]
            :returns: a bool (int) `more` and a rectangle `filled`. If `more == 0`, all content of the story has been written, otherwise more is waiting to be written to subsequent rectangles / pages. Rectangle `filled` is the part of `where` that has actually been filled.

        .. method:: draw(dev, matrix=None)

            Write the content part prepared by :meth:`Story.place` to the page.

            :arg dev: the :ref:`Device` created by `dev = writer.begin_page(mediabox)`. The device knows how to call all MuPDF functions needed to write the content.
            :arg matrix_like matrix: a matrix for transforming content when writing to the page. An example may be writing rotated text. The default means no transformation (i.e. the :ref:`Identity` matrix).

        .. method:: element_positions(function, args=None)

            Let the Story provide positioning information about certain HTML elements once their place on the current page has been computed - i.e. invoke this method **directly after** :meth:`Story.place`.

            *Story* will pass position information to *function*. This information can for example be used to generate a Table of Contents.

            :arg callable function: a Python function accepting an :class:`ElementPosition` object. It will be invoked by the Story object to process positioning information. The function **must** be a callable accepting exactly one argument.
            :arg dict args: an optional dictionary with any **additional** information that should be added to the :class:`ElementPosition` instance passed to `function`.
            Like for example the current output page number.
            Every key in this dictionary must be a string that conforms to the rules for a valid Python identifier.
            The complete set of information is explained below.


        .. method:: reset()

            Rewind the story's document to the beginning for starting over its output.

        .. attribute:: body

            The :htmlTag:`body` part of the story's DOM. This attribute contains the :ref:`Xml` node of :htmlTag:`body`. All relevant content for PDF production is contained between "<body>" and "</body>".

        .. method:: write(writer, rectfn, positionfn=None, pagefn=None)

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

            Similar to `write()` except that we don't have a `writer` arg
            and we return a PDF `Document` in which links have been created
            for each internal html link.

        .. staticmethod:: write_stabilized_with_links(contentfn, rectfn, user_css=None, em=12, positionfn=None, pagefn=None, archive=None, add_header_ids=True)

            Similar to `write_stabilized()` except that we don't have a `writer`
            arg and instead return a PDF `Document` in which links have been
            created for each internal html link.
            
        .. class:: Story.FitResult
            
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
