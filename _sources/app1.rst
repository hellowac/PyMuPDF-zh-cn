.. include:: header.rst

.. _Appendix1:

======================================
附录 1：文本提取详细信息
======================================

Appendix 1: Details on Text Extraction

.. tab:: 中文

    本章介绍了 PyMuPDF 文本提取方法的背景。

    感兴趣的信息是

    * 它们提供什么？
    * 它们意味着什么 (处理时间/数据大小) ？

.. tab:: 英文

    This chapter provides background on the text extraction methods of PyMuPDF.

    Information of interest are

    * what do they provide?
    * what do they imply (processing time / data sizes)?

TextPage 的一般结构
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

General structure of a TextPage

.. tab:: 中文

    :ref:`TextPage` 是 (Py-) MuPDF 的一个类。通常，在使用 :ref:`Page` 的文本提取方法时，它会在幕后被创建并销毁。但它也可以直接使用，作为一个持久对象。尽管名称中包含“Text”，但文本页面中也可以包含图像::

        <page>
            <text block>
                <line>
                    <span>
                        <char>
            <image block>
                <img>

    一个 **文本页面(TextPage)** 由多个块 (block) 组成 (大致对应于段落) 。

    一个 **块(block)** 要么由文本行和字符组成，要么包含一幅图像。

    一 **行(line)** 由多个文本片段 (span) 组成。

    一个 **文本片段(span)** 由具有相同字体属性 (名称、大小、标志、颜色) 的相邻字符组成。

.. tab:: 英文

    :ref:`TextPage` is one of (Py-) MuPDF's classes. It is normally created (and destroyed again) behind the curtain, when :ref:`Page` text extraction methods are used, but it is also available directly and can be used as a persistent object. Other than its name suggests, images may optionally also be part of a text page::

        <page>
            <text block>
                <line>
                    <span>
                        <char>
            <image block>
                <img>

    A **text page** consists of blocks (= roughly paragraphs).

    A **block** consists of either lines and their characters, or an image.

    A **line** consists of spans.

    A **span** consists of adjacent characters with identical font properties: name, size, flags and color.

纯文本
~~~~~~~~~~

Plain Text

.. tab:: 中文

    函数 :meth:`TextPage.extractText` (或 `Page.get_text("text")`) 提取页面的 **纯文本**，并按照文档创建者指定的 **原始顺序** 返回。

    示例输出::

        >>> print(page.get_text("text"))
        Some text on first page.

    .. note:: 

        输出的文本顺序可能与常规的“自然”阅读顺序不同。然而，可以通过执行 `page.get_text("text", sort=True)` 来请求按照 **从左上到右下** 的顺序重新排序文本。


.. tab:: 英文

    Function :meth:`TextPage.extractText` (or *Page.get_text("text")*) extracts a page's plain **text in original order** as specified by the creator of the document.

    An example output::

        >>> print(page.get_text("text"))
        Some text on first page.

    .. note:: 
        
        The output may not equal an accustomed "natural" reading order. However, you can request a reordering following the scheme "top-left to bottom-right" by executing `page.get_text("text", sort=True)`.


块
~~~~~~~~~~

BLOCKS

.. tab:: 中文

    函数 :meth:`TextPage.extractBLOCKS` (或 `Page.get_text("blocks")`) 提取页面的文本块，并返回如下格式的列表::

        (x0, y0, x1, y1, "block 内的文本行", block_no, block_type)

    其中：

    - 前四个元素是该文本块的 **边界框(bbox)** 的浮点坐标值。
    - 文本块中的行通过换行符 (`\n`) 进行拼接。

    该方法运行速度极快，并且 **默认情况下还会提取图像的元数据信息**：每个图像都会作为一个文本块出现，其中包含一行文本，记录该图像的元数据信息。**图像本身不会被显示**。

    与前面提到的简单文本提取类似，`sort` 参数同样适用，可用于调整文本块的阅读顺序。

    示例输出::

        >>> print(page.get_text("blocks", sort=False))
        [(50.0, 88.17500305175781, 166.1709747314453, 103.28900146484375,
        'Some text on first page.', 0, 0)]


.. tab:: 英文

    Function :meth:`TextPage.extractBLOCKS` (or *Page.get_text("blocks")*) extracts a page's text blocks as a list of items like::

        (x0, y0, x1, y1, "lines in block", block_no, block_type)

    Where the first 4 items are the float coordinates of the block's bbox. The lines within each block are concatenated by a new-line character.

    This is a high-speed method, which by default also extracts image meta information: Each image appears as a block with one text line, which contains meta information. The image itself is not shown.

    As with simple text output above, the `sort` argument can be used as well to obtain a reading order.

    Example output::

        >>> print(page.get_text("blocks", sort=False))
        [(50.0, 88.17500305175781, 166.1709747314453, 103.28900146484375,
        'Some text on first page.', 0, 0)]


单词
~~~~~~~~~~

WORDS

.. tab:: 中文

    函数 :meth:`TextPage.extractWORDS` (或 `Page.get_text("words")`) 提取页面中的 **单词**，并返回如下格式的列表::

        (x0, y0, x1, y1, "word", block_no, line_no, word_no)

    其中：

    - 前四个元素是该单词的 **边界框(bbox)** 的浮点坐标值。
    - 最后三个整数表示该单词所在的 **文本块编号、行号和单词编号**。

    该方法运行速度极快。与前述方法类似， `sort=True` 参数可用于重新排序单词，以确保按照 **阅读顺序** 提取内容。

    示例输出::

        >>> for word in page.get_text("words", sort=False):
                print(word)
        (50.0, 88.17500305175781, 78.73200225830078, 103.28900146484375,
        'Some', 0, 0, 0)
        (81.79000091552734, 88.17500305175781, 99.5219955444336, 103.28900146484375,
        'text', 0, 0, 1)
        (102.57999420166016, 88.17500305175781, 114.8119888305664, 103.28900146484375,
        'on', 0, 0, 2)
        (117.86998748779297, 88.17500305175781, 135.5909881591797, 103.28900146484375,
        'first', 0, 0, 3)
        (138.64898681640625, 88.17500305175781, 166.1709747314453, 103.28900146484375,
        'page.', 0, 0, 4)


.. tab:: 英文

    Function :meth:`TextPage.extractWORDS` (or *Page.get_text("words")*) extracts a page's text **words** as a list of items like::

        (x0, y0, x1, y1, "word", block_no, line_no, word_no)

    Where the first 4 items are the float coordinates of the words's bbox. The last three integers provide some more information on the word's whereabouts.

    This is a high-speed method. As with the previous methods, argument `sort=True` will reorder the words.

    Example output::

        >>> for word in page.get_text("words", sort=False):
                print(word)
        (50.0, 88.17500305175781, 78.73200225830078, 103.28900146484375,
        'Some', 0, 0, 0)
        (81.79000091552734, 88.17500305175781, 99.5219955444336, 103.28900146484375,
        'text', 0, 0, 1)
        (102.57999420166016, 88.17500305175781, 114.8119888305664, 103.28900146484375,
        'on', 0, 0, 2)
        (117.86998748779297, 88.17500305175781, 135.5909881591797, 103.28900146484375,
        'first', 0, 0, 3)
        (138.64898681640625, 88.17500305175781, 166.1709747314453, 103.28900146484375,
        'page.', 0, 0, 4)

HTML
~~~~

HTML

.. tab:: 中文

    :meth:`TextPage.extractHTML` (或 `Page.get_text("html")`) 的输出完整反映了 *TextPage* 的结构 —— 类似于 `DICT` / `JSON` 格式的输出。  
    其中包括 **图像、字体信息** 以及 **文本位置**。如果将其包裹在 **HTML 头部和尾部代码** 之中，就可以直接在浏览器中显示。

    例如，以下代码展示了 `page.get_text("html")` 的输出::

        >>> for line in page.get_text("html").splitlines():
                print(line)

        <div id="page0" style="position:relative;width:300pt;height:350pt;
        background-color:white">
        <p style="position:absolute;white-space:pre;margin:0;padding:0;top:88pt;
        left:50pt"><span style="font-family:Helvetica,sans-serif;
        font-size:11pt">Some text on first page.</span></p>
        </div>


.. tab:: 英文

    :meth:`TextPage.extractHTML` (or *Page.get_text("html")* output fully reflects the structure of the page's *TextPage* -- much like DICT / JSON below. This includes images, font information and text positions. If wrapped in HTML header and trailer code, it can readily be displayed by an internet browser. Our above example::

        >>> for line in page.get_text("html").splitlines():
                print(line)

        <div id="page0" style="position:relative;width:300pt;height:350pt;
        background-color:white">
        <p style="position:absolute;white-space:pre;margin:0;padding:0;top:88pt;
        left:50pt"><span style="font-family:Helvetica,sans-serif;
        font-size:11pt">Some text on first page.</span></p>
        </div>


.. _HTMLQuality:

控制 HTML 输出的质量
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Controlling Quality of HTML Output

.. tab:: 中文

    虽然 MuPDF v1.12.0 版本在 **HTML 输出** 方面有了很大改进，但仍然存在一些 **bug**，主要涉及以下两个方面：

    1. **字体支持问题**
    2. **图片定位问题**

    **字体问题：**
    
    * HTML 文本包含对原始文档字体的引用。如果浏览器无法识别这些字体 (大概率如此) ，它会用其他字体替代，导致排版异常。
    * 不同的浏览器对此的处理方式不同。例如，在 **Windows 系统** 上，**MS Edge** 的显示效果较好，而 **Firefox** 可能会很混乱。

    **图片定位问题：**

    * 对于结构复杂的 PDF，图片可能 **未正确定位或缩放**。
    * 这通常发生在 **旋转页面** 以及 **不同 bbox 定义不一致**  (如 `MediaBox != CropBox`) 的情况下。
    * 目前尚未找到有效的解决方案，**已经向 MuPDF 提交了 bug 反馈**。

    ---

    **解决方案：**

    对于 **字体问题**，可以使用一个简单的 Python 脚本来扫描 HTML 文件，并将 **未知字体** 替换为标准的 `Base-14` 字体：
    * **衬线字体(serif)**  → **"Times"**
    * **非衬线字体(sans-serif)**  → **"Helvetica"**
    * **等宽字体(monospace)**  → **"Courier"**

    以下示例代码完成该操作::

        import sys
        filename = sys.argv[1]
        otext = open(filename).read()                 # 读取 HTML 文件
        pos1 = 0                                      # 搜索起始位置
        font_serif = "font-family:Times"              # 替换为 Times
        font_sans  = "font-family:Helvetica"          # 替换为 Helvetica
        font_mono  = "font-family:Courier"            # 替换为 Courier
        found_one  = False                            # 标记是否进行了替换

        while True:
            pos0 = otext.find("font-family:", pos1)   # 查找字体定义
            if pos0 < 0:                              # 未找到，退出循环
                break
            pos1 = otext.find(";", pos0)              # 找到字体定义的结束位置
            test = otext[pos0 : pos1]                 # 获取完整的字体定义
            testn = ""                                # 新的字体定义
            if test.endswith(",serif"):               # 如果是衬线字体
                testn = font_serif                    # 替换为 Times
            elif test.endswith(",sans-serif"):        # 如果是非衬线字体
                testn = font_sans                     # 替换为 Helvetica
            elif test.endswith(",monospace"):         # 如果是等宽字体
                testn = font_mono                     # 替换为 Courier

            if testn != "":                           # 发现可替换字体
                otext = otext.replace(test, testn)    # 替换 HTML 内容
                found_one = True
                pos1 = 0                              # 重新开始搜索

        if found_one:
            ofile = open(filename + ".html", "w")
            ofile.write(otext)                        # 保存新的 HTML 文件
            ofile.close()
        else:
            print("Warning: could not find any font specs!")

    这样，转换后的 HTML 文件可以在大多数浏览器中正确显示。


.. tab:: 英文

    While HTML output has improved a lot in MuPDF v1.12.0, it is not yet bug-free: we have found problems in the areas **font support** and **image positioning**.

    * HTML text contains references to the fonts used of the original document. If these are not known to the browser (a fat chance!), it will replace them with others; the results will probably look awkward. This issue varies greatly by browser -- on my Windows machine, MS Edge worked just fine, whereas Firefox looked horrible.

    * For PDFs with a complex structure, images may not be positioned and / or sized correctly. This seems to be the case for rotated pages and pages, where the various possible page bbox variants do not coincide (e.g. *MediaBox != CropBox*). We do not know yet, how to address this -- we filed a bug at MuPDF's site.

    To address the font issue, you can use a simple utility script to scan through the HTML file and replace font references. Here is a little example that replaces all fonts with one of the :ref:`Base-14-Fonts`: serifed fonts will become "Times", non-serifed "Helvetica" and monospaced will become "Courier". Their respective variations for "bold", "italic", etc. are hopefully done correctly by your browser::

        import sys
        filename = sys.argv[1]
        otext = open(filename).read()                 # original html text string
        pos1 = 0                                      # search start poition
        font_serif = "font-family:Times"              # enter ...
        font_sans  = "font-family:Helvetica"          # ... your choices ...
        font_mono  = "font-family:Courier"            # ... here
        found_one  = False                            # true if search successful

        while True:
            pos0 = otext.find("font-family:", pos1)   # start of a font spec
            if pos0 < 0:                              # none found - we are done
                break
            pos1 = otext.find(";", pos0)              # end of font spec
            test = otext[pos0 : pos1]                 # complete font spec string
            testn = ""                                # the new font spec string
            if test.endswith(",serif"):               # font with serifs?
                testn = font_serif                    # use Times instead
            elif test.endswith(",sans-serif"):        # sans serifs font?
                testn = font_sans                     # use Helvetica
            elif test.endswith(",monospace"):         # monospaced font?
                testn = font_mono                     # becomes Courier

            if testn != "":                           # any of the above found?
                otext = otext.replace(test, testn)    # change the source
                found_one = True
                pos1 = 0                              # start over

        if found_one:
            ofile = open(filename + ".html", "w")
            ofile.write(otext)
            ofile.close()
        else:
            print("Warning: could not find any font specs!")



DICT (或 JSON) 
~~~~~~~~~~~~~~~~

DICT (or JSON)

.. tab:: 中文

    :meth:`TextPage.extractDICT` (或 *Page.get_text("dict", sort=False)*) 方法的输出 **完整反映** 了 *TextPage* 的结构，并且 **提供详细的图片内容和位置信息** (*bbox*，以像素单位表示的边界框) 。

    与 **JSON 输出** 的不同之处：
    - 在 **DICT 输出** 中，图片以 *bytes* 格式存储。
    - 在 **JSON 输出** 中，图片以 **Base64 编码字符串** 形式存储。

    如果想要可视化 **字典结构**，请参考 :ref:`textpagedict`。

    ---

    以下是一个 **Page.get_text("dict")** 提取的 **字典结构** 示例::

        {
            "width": 300.0,   # 页面宽度
            "height": 350.0,  # 页面高度
            "blocks": [{      # 文字块列表
                "type": 0,    # 0 代表文本块，1 代表图片块
                "bbox": (50.0, 88.175, 166.171, 103.289),  # 文本块的边界框
                "lines": ({   # 文本行列表
                    "wmode": 0,  # 书写模式 (0 = 水平书写，1 = 垂直书写) 
                    "dir": (1.0, 0.0),  # 方向向量 (水平文本：1.0, 0.0) 
                    "bbox": (50.0, 88.175, 166.171, 103.289),  # 该行的边界框
                    "spans": ({  # 文本跨度列表 (即一段字体属性相同的文本) 
                        "size": 11.0,  # 字体大小
                        "flags": 0,  # 字体样式 (0 = 正常，1 = 斜体，2 = 粗体，4 = 下划线) 
                        "font": "Helvetica",  # 字体名称
                        "color": 0,  # 颜色 (RGB 值) 
                        "origin": (50.0, 100.0),  # 文字起始位置
                        "text": "Some text on first page.",  # 具体文本内容
                        "bbox": (50.0, 88.175, 166.171, 103.289)  # 文字的边界框
                    })
                }]
            }]
        }

    ---

    **说明：**

    - `blocks`：包含多个 **文本块** (或者 **图片块**) 。
    - `type`：区分块的类型：
    - `0` 表示 **文本块** (text block) 。
    - `1` 表示 **图片块** (image block) 。
    - `lines`：每个 **文本块** 由多行 (`lines`) 组成。
    - `spans`：每行由多个 **文本跨度(spans)** 组成，每个跨度代表 **相同字体属性** 的一组字符。

    这种结构 **不仅提供了文本内容**，还能精确描述 **文本的坐标、字体、颜色和大小**，适用于 **OCR 处理、文本再排版、PDF 数据分析等应用**。


.. tab:: 英文

    :meth:`TextPage.extractDICT` (or *Page.get_text("dict", sort=False)*) output fully reflects the structure of a *TextPage* and provides image content and position detail (*bbox* -- boundary boxes in pixel units) for every block, line and span. Images are stored as *bytes* for DICT output and base64 encoded strings for JSON output.

    For a visualization of the dictionary structure have a look at :ref:`textpagedict`.

    Here is how this looks like::

        {
            "width": 300.0,
            "height": 350.0,
            "blocks": [{
                "type": 0,
                "bbox": (50.0, 88.17500305175781, 166.1709747314453, 103.28900146484375),
                "lines": ({
                    "wmode": 0,
                    "dir": (1.0, 0.0),
                    "bbox": (50.0, 88.17500305175781, 166.1709747314453, 103.28900146484375),
                    "spans": ({
                        "size": 11.0,
                        "flags": 0,
                        "font": "Helvetica",
                        "color": 0,
                        "origin": (50.0, 100.0),
                        "text": "Some text on first page.",
                        "bbox": (50.0, 88.17500305175781, 166.1709747314453, 103.28900146484375)
                    })
                }]
            }]
        }

RAWDICT (或 RAWJSON) 
~~~~~~~~~~~~~~~~~~~~~

RAWDICT (or RAWJSON)

.. tab:: 中文

    :meth:`TextPage.extractRAWDICT` (或 *Page.get_text("rawdict", sort=False)*) 是 **DICT 模式的超集**，提供了 **更细粒度的信息**。

    其结构与 `extractDICT` 输出 **几乎相同**，但 **将 "text" 字段 (字符串) 替换为 "chars" 列表**。  
    列表中的每个元素是一个 **字符字典**，包含每个字符的 **位置、边界框和具体字符内容** 。

    假设 `extractDICT` 输出的部分如下::

        "spans": [{
            "size": 11.0,
            "flags": 0,
            "font": "Helvetica",
            "color": 0,
            "origin": (50.0, 100.0),
            "text": "Some text on first page.",
            "bbox": (50.0, 88.175, 166.171, 103.289)
        }]

    在 `extractRAWDICT` 模式下，相应部分将变为::

        "spans": [{
            "size": 11.0,
            "flags": 0,
            "font": "Helvetica",
            "color": 0,
            "origin": (50.0, 100.0),
            "chars": [{
                "origin": (50.0, 100.0),
                "bbox": (50.0, 88.175, 57.337, 103.289),
                "c": "S"
            }, {
                "origin": (57.337, 100.0),
                "bbox": (57.337, 88.175, 63.453, 103.289),
                "c": "o"
            }, {
                "origin": (63.453, 100.0),
                "bbox": (63.453, 88.175, 72.616, 103.289),
                "c": "m"
            }, {
                "origin": (72.616, 100.0),
                "bbox": (72.616, 88.175, 78.732, 103.289),
                "c": "e"
            }, {
                "origin": (78.732, 100.0),
                "bbox": (78.732, 88.175, 81.790, 103.289),
                "c": " "
            }, 
            <...中间字符省略...>, 
            {
                "origin": (163.113, 100.0),
                "bbox": (163.113, 88.175, 166.171, 103.289),
                "c": "."
            }]
        }]

    ---

    **说明：**

    - `"text"` (完整文本) 字段被 `"chars"` (字符列表) 替代。
    - 每个字符的 **边界框 (bbox)、起始坐标 (origin)、字符内容 ("c")** 都被单独存储。
    - 适用于 **需要精确字符级分析** 的应用，例如：
    - OCR (光学字符识别) 后处理
    - 字符级坐标对齐
    - 精细文本分析
    - 复杂的 PDF 文字排版重建

    与 `extractDICT` 相比，`extractRAWDICT` 提供了 **更细粒度的信息**，但 **数据量更大，处理速度较慢**，因此 **适用于更精确的文本处理任务**。


.. tab:: 英文

    :meth:`TextPage.extractRAWDICT` (or *Page.get_text("rawdict", sort=False)*) is an **information superset of DICT** and takes the detail level one step deeper. It looks exactly like the above, except that the *"text"* items (*string*) in the spans are replaced by the list *"chars"*. Each *"chars"* entry is a character *dict*. For example, here is what you would see in place of item *"text": "Text in black color."* above::

        "chars": [{
            "origin": (50.0, 100.0),
            "bbox": (50.0, 88.17500305175781, 57.336997985839844, 103.28900146484375),
            "c": "S"
        }, {
            "origin": (57.33700180053711, 100.0),
            "bbox": (57.33700180053711, 88.17500305175781, 63.4530029296875, 103.28900146484375),
            "c": "o"
        }, {
            "origin": (63.4530029296875, 100.0),
            "bbox": (63.4530029296875, 88.17500305175781, 72.61600494384766, 103.28900146484375),
            "c": "m"
        }, {
            "origin": (72.61600494384766, 100.0),
            "bbox": (72.61600494384766, 88.17500305175781, 78.73200225830078, 103.28900146484375),
            "c": "e"
        }, {
            "origin": (78.73200225830078, 100.0),
            "bbox": (78.73200225830078, 88.17500305175781, 81.79000091552734, 103.28900146484375),
            "c": " "
        < ... deleted ... >
        }, {
            "origin": (163.11297607421875, 100.0),
            "bbox": (163.11297607421875, 88.17500305175781, 166.1709747314453, 103.28900146484375),
            "c": "."
        }],


XML
~~~

XML

.. tab:: 中文

    :meth:`TextPage.extractXML` (或 *Page.get_text("xml")*) 提供与 `extractRAWDICT` 相同的细粒度信息，但以 **XML 格式** 组织数据，并且 **仅包含文本(不包含图像)**。

    ---

    执行以下代码::

        >>> for line in page.get_text("xml").splitlines():
            print(line)

    可能的输出如下::

        <page id="page0" width="300" height="350">
        <block bbox="50 88.175 166.17098 103.289">
        <line bbox="50 88.175 166.17098 103.289" wmode="0" dir="1 0">
        <font name="Helvetica" size="11">
        <char quad="50 88.175 57.336999 88.175 50 103.289 57.336999 103.289" 
            x="50" y="100" color="#000000" c="S"/>
        <char quad="57.337 88.175 63.453004 88.175 57.337 103.289 63.453004 103.289" 
            x="57.337" y="100" color="#000000" c="o"/>
        <char quad="63.453004 88.175 72.616008 88.175 63.453004 103.289 72.616008 103.289" 
            x="63.453004" y="100" color="#000000" c="m"/>
        <char quad="72.616008 88.175 78.732 88.175 72.616008 103.289 78.732 103.289" 
            x="72.616008" y="100" color="#000000" c="e"/>
        <char quad="78.732 88.175 81.79 88.175 78.732 103.289 81.79 103.289" 
            x="78.732" y="100" color="#000000" c=" "/>
        
        ... (部分字符省略) ...

        <char quad="163.11298 88.175 166.17098 88.175 163.11298 103.289 166.17098 103.289" 
            x="163.11298" y="100" color="#000000" c="."/>
        </font>
        </line>
        </block>
        </page>

    ---

    **说明：**

    - **XML 结构**：
    - `<page>`: 页面级别
    - `<block>`: 文本块 (段落) 
    - `<line>`: 文字行
    - `<font>`: 该行的字体信息
    - `<char>`: 每个字符的详细信息，包括：
        - **quad**: 四边形坐标 (用于文本变形或旋转) 
        - **x, y**: 字符的起始坐标
        - **color**: 颜色信息
        - **c**: 具体字符内容

    - **特点**：
    - 仅包含文本信息 (不包含图像) 
    - 适用于 **结构化解析**
    - 可与 **XML 解析库** (如 `lxml` ) **结合使用**

    ---

    **解析 XML 输出：**

    你可以使用 Python `lxml` 库解析此输出::

        from lxml import etree
        
        xml_text = page.get_text("xml")
        root = etree.fromstring(xml_text)
        
        for char in root.xpath("//char"):
            print(f"Character: {char.get('c')}, Position: ({char.get('x')}, {char.get('y')})")

    ---

    **适用场景：**

    - **PDF 文字结构解析**
    - **文档格式化**
    - **字体分析**
    - **字符级定位**

    与 `extractRAWDICT` 相比， `extractXML` **提供相同的信息，但采用 XML 结构**，便于与标准 XML 解析工具集成。


.. tab:: 英文

    The :meth:`TextPage.extractXML` (or *Page.get_text("xml")*) version extracts text (no images) with the detail level of RAWDICT::

        >>> for line in page.get_text("xml").splitlines():
            print(line)

        <page id="page0" width="300" height="350">
        <block bbox="50 88.175 166.17098 103.289">
        <line bbox="50 88.175 166.17098 103.289" wmode="0" dir="1 0">
        <font name="Helvetica" size="11">
        <char quad="50 88.175 57.336999 88.175 50 103.289 57.336999 103.289" x="50"
        y="100" color="#000000" c="S"/>
        <char quad="57.337 88.175 63.453004 88.175 57.337 103.289 63.453004 103.289" x="57.337"
        y="100" color="#000000" c="o"/>
        <char quad="63.453004 88.175 72.616008 88.175 63.453004 103.289 72.616008 103.289" x="63.453004"
        y="100" color="#000000" c="m"/>
        <char quad="72.616008 88.175 78.732 88.175 72.616008 103.289 78.732 103.289" x="72.616008"
        y="100" color="#000000" c="e"/>
        <char quad="78.732 88.175 81.79 88.175 78.732 103.289 81.79 103.289" x="78.732"
        y="100" color="#000000" c=" "/>

        ... deleted ...

        <char quad="163.11298 88.175 166.17098 88.175 163.11298 103.289 166.17098 103.289" x="163.11298"
        y="100" color="#000000" c="."/>
        </font>
        </line>
        </block>
        </page>

    .. note:: 
        
        We have successfully tested `lxml <https://pypi.org/project/lxml/>`_ to interpret this output.

XHTML
~~~~~

XHTML

.. tab:: 中文

    :meth:`TextPage.extractXHTML` (或 *Page.get_text("xhtml")*) 是一种 **TEXT 提取的 HTML 变体**，其输出格式类似 HTML，但 **仅包含基本文本和图片**，即“语义化”输出。::

        <div id="page0">
        <p>Some text on first page.</p>
        </div>

.. tab:: 英文

    :meth:`TextPage.extractXHTML` (or *Page.get_text("xhtml")*) is a variation of TEXT but in HTML format, containing the bare text and images ("semantic" output)::

        <div id="page0">
        <p>Some text on first page.</p>
        </div>

.. _text_extraction_flags:

文本提取标志默认值
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Text Extraction Flags Defaults

.. tab:: 中文

    * 新功能 (版本 1.16.2) ：`meth:Page.get_text` 方法支持一个关键字参数 *flags* (*int*)，用于控制提取数据的数量和质量。以下表格展示了每种提取方式的默认设置 (未提供 flags 或 flags 设置为 None) 。如果您指定了一个不同于 *None* 的 flags 值，请注意必须设置 **所有期望的** 选项。各个选项的位设置说明可以参见 :ref:`TextPreserve`。

    * 新功能 (版本 1.19.6) ：以下默认组合现在作为 Python 常量提供：:data:`TEXTFLAGS_TEXT` ，:data:`TEXTFLAGS_WORDS` ，:data:`TEXTFLAGS_BLOCKS` ，:data:`TEXTFLAGS_DICT` ，:data:`TEXTFLAGS_RAWDICT` ，:data:`TEXTFLAGS_HTML` ，:data:`TEXTFLAGS_XHTML` ，:data:`TEXTFLAGS_XML` 和 :data:`TEXTFLAGS_SEARCH` 。您现在可以轻松修改默认的 flag，例如：

        - **包含** 图像在 "blocks" 输出中：
        
        `flags = TEXTFLAGS_BLOCKS | TEXT_PRESERVE_IMAGES`
        
        - **排除** 图像从 "dict" 输出中：
        
        `flags = TEXTFLAGS_DICT & ~TEXT_PRESERVE_IMAGES`
        
        - 在文本搜索中设置 **关闭断字**：
        
        `flags = TEXTFLAGS_SEARCH & ~TEXT_DEHYPHENATE`

    ========================= ==== ==== ===== === ==== ======= ===== ====== ======
    指示器                    text html xhtml xml dict rawdict words blocks search
    ========================= ==== ==== ===== === ==== ======= ===== ====== ======
    保留连字                  1    1    1     1   1    1       1     1       0
    保留空白                  1    1    1     1   1    1       1     1       1
    保留图像                  n/a  1    1     n/a 1    1       n/a   0       0
    抑制空格                  0    0    0     0   0    0       0     0       0
    断字处理                  0    0    0     0   0    0       0     0       1
    裁剪到媒体框              1    1    1     1   1    1       1     1       1
    使用 CID 而非 U+FFFD      1    1    1     1   1    1       1     1       0
    ========================= ==== ==== ===== === ==== ======= ===== ====== ======

    - **search**：表示文本搜索功能。
    - **"json"** 与 **"dict"** 处理方式完全相同，因此被省略。
    - **"rawjson"** 与 **"rawdict"** 处理方式完全相同，因此被省略。
    - **"n/a"**：表示该项为 0，设置此位对输出没有影响 (但可能会对性能产生负面影响) 。
    - 如果您对输出方式默认包含的图像不感兴趣，可以关闭相应的标志位：这样您将体验到更好的性能和更低的空间需求。

    展示 `TEXT_INHIBIT_SPACES` 的效果::

        >>> print(page.get_text("text"))
        H a l l o !
        Mo r e  t e x t
        i s  f o l l o w i n g
        i n  E n g l i s h
        . . .  l e t ' s  s e e
        w h a t  h a p p e n s .
        >>> print(page.get_text("text", flags=pymupdf.TEXT_INHIBIT_SPACES))
        Hallo!
        More text
        is following
        in English
        ... let's see
        what happens.
        >>>


.. tab:: 英文

    * New in version 1.16.2: Method :meth:`Page.get_text` supports a keyword parameter *flags* *(int)* to control the amount and the quality of extracted data. The following table shows the defaults settings (flags parameter omitted or None) for each extraction variant. If you specify flags with a value other than *None*, be aware that you must set **all desired** options. A description of the respective bit settings can be found in :ref:`TextPreserve`.

    * New in v1.19.6: The default combinations in the following table are now available as Python constants: :data:`TEXTFLAGS_TEXT`, :data:`TEXTFLAGS_WORDS`, :data:`TEXTFLAGS_BLOCKS`, :data:`TEXTFLAGS_DICT`, :data:`TEXTFLAGS_RAWDICT`, :data:`TEXTFLAGS_HTML`, :data:`TEXTFLAGS_XHTML`, :data:`TEXTFLAGS_XML`, and :data:`TEXTFLAGS_SEARCH`. You can now easily modify a default flag, e.g.

        - **include** images in a "blocks" output:
        
        `flags = TEXTFLAGS_BLOCKS | TEXT_PRESERVE_IMAGES`
        
        - **exclude** images from a "dict" output:
        
        `flags = TEXTFLAGS_DICT & ~TEXT_PRESERVE_IMAGES`
        
        - set **dehyphenation off** in text searches:
        
        `flags = TEXTFLAGS_SEARCH & ~TEXT_DEHYPHENATE`


    ========================= ==== ==== ===== === ==== ======= ===== ====== ======
    Indicator                 text html xhtml xml dict rawdict words blocks search
    ========================= ==== ==== ===== === ==== ======= ===== ====== ======
    preserve ligatures        1    1    1     1   1    1       1     1       0
    preserve whitespace       1    1    1     1   1    1       1     1       1
    preserve images           n/a  1    1     n/a 1    1       n/a   0       0
    inhibit spaces            0    0    0     0   0    0       0     0       0
    dehyphenate               0    0    0     0   0    0       0     0       1
    clip to mediabox          1    1    1     1   1    1       1     1       1
    use CID instead of U+FFFD 1    1    1     1   1    1       1     1       0
    ========================= ==== ==== ===== === ==== ======= ===== ====== ======

    * **search** refers to the text search function.
    * **"json"** is handled exactly like **"dict"** and is hence left out.
    * **"rawjson"** is handled exactly like **"rawdict"** and is hence left out.
    * An "n/a" specification means a value of 0 and setting this bit never has any effect on the output (but an adverse effect on performance).
    * If you are not interested in images when using an output variant which includes them by default, then by all means set the respective bit off: You will experience a better performance and much lower space requirements.

    To show the effect of `TEXT_INHIBIT_SPACES` have a look at this example::

        >>> print(page.get_text("text"))
        H a l l o !
        Mo r e  t e x t
        i s  f o l l o w i n g
        i n  E n g l i s h
        . . .  l e t ' s  s e e
        w h a t  h a p p e n s .
        >>> print(page.get_text("text", flags=pymupdf.TEXT_INHIBIT_SPACES))
        Hallo!
        More text
        is following
        in English
        ... let's see
        what happens.
        >>>


性能
~~~~~~~~~~~~

Performance

.. tab:: 中文

    文本提取方法在提供的信息量和资源需求、运行时间方面有显著差异。通常来说，更多的信息意味着需要更多的处理，并且会生成更大的数据量。

    .. note::

        特别是图像对性能的影响 **非常显著** 。确保在不需要图像时通过 *flags* 参数将它们排除。在默认标志设置下，处理下面提到的 2,700 页文档需要 160 秒，而排除所有图像后，仅需不到 50% 的时间 (77 秒) 。

    首先，所有方法相较于市面上的其他产品来说都是 **非常快速** 的。在处理速度方面，我们不知道有更快的 (免费的) 工具。即使是最详细的方法，RAWDICT，也能在不到 5 秒的时间内处理完《AdobeManual》的 1,310 页 (简单文本处理时间不到 2 秒) 。

    下表显示了平均相对速度 ("RSpeed"，基准值 1.00 为 TEXT) ，基于大约 1,400 页文本密集型和 1,300 页图像密集型页面。

    ======= ====== ===================================================================== ==========
    方法    RSpeed 备注                                                                   无图像时 
    ======= ====== ===================================================================== ==========
    TEXT     1.00  无图像，  **纯** 文本，带换行符                                        1.00
    BLOCKS   1.00  **仅** 图像边界框 (bbox), **block** 级文本与边界框，带换行符           1.00
    WORDS    1.02  无图像, **word** 级文本与边界框                                        1.02
    XML      2.72  无图像, **char** 级文本,布局和字体细节                                 2.72
    XHTML    3.32  **base64** 图像, **span** 级文本, 无布局信息                           1.00
    HTML     3.54  **base64** 图像, **span** 级文本, 带布局和字体细节                     1.01
    DICT     3.93  **binary** 图像, **span** 级文本, 带布局和字体细节                     1.04
    RAWDICT  4.50  **binary** 图像, **char** 级文本, 带布局和字体细节                     1.68
    ======= ====== ===================================================================== ==========

    如表所示：当排除图像提取 (最后一列) 时，相对速度变化显著：除了 RAWDICT 和 XML，其他方法几乎同样快速，而 RAWDICT 比 **现在最慢的 XML** 快 40% 的执行时间。

    更多性能信息请参见 **附录 1** 。

.. tab:: 英文

    The text extraction methods differ significantly both: in terms of information they supply, and in terms of resource requirements and runtimes. Generally, more information of course means, that more processing is required and a higher data volume is generated.

    .. note:: Especially images have a **very significant** impact. Make sure to exclude them (via the *flags* parameter) whenever you do not need them. To process the below mentioned 2'700 total pages with default flags settings required 160 seconds across all extraction methods. When all images where excluded, less than 50% of that time (77 seconds) were needed.

    To begin with, all methods are **very fast** in relation to other products out there in the market. In terms of processing speed, we are not aware of a faster (free) tool. Even the most detailed method, RAWDICT, processes all 1'310 pages of the :ref:`AdobeManual` in less than 5 seconds (simple text needs less than 2 seconds here).

    The following table shows average relative speeds ("RSpeed", baseline 1.00 is TEXT), taken across ca. 1400 text-heavy and 1300 image-heavy pages.

    ======= ====== ===================================================================== ==========
    Method  RSpeed Comments                                                               no images
    ======= ====== ===================================================================== ==========
    TEXT     1.00  no images, **plain** text, line breaks                                 1.00
    BLOCKS   1.00  image bboxes (only), **block** level text with bboxes, line breaks     1.00
    WORDS    1.02  no images, **word** level text with bboxes                             1.02
    XML      2.72  no images, **char** level text, layout and font details                2.72
    XHTML    3.32  **base64** images, **span** level text, no layout info                 1.00
    HTML     3.54  **base64** images, **span** level text, layout and font details        1.01
    DICT     3.93  **binary** images, **span** level text, layout and font details        1.04
    RAWDICT  4.50  **binary** images, **char** level text, layout and font details        1.68
    ======= ====== ===================================================================== ==========

    As mentioned: when excluding image extraction (last column), the relative speeds are changing drastically: except RAWDICT and XML, the other methods are almost equally fast, and RAWDICT requires 40% less execution time than the **now slowest XML**.

    Look at chapter **Appendix 1** for more performance information.

.. include:: footer.rst
