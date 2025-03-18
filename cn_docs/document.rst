.. include:: header.rst

.. _Document:

================
Document
================

.. highlight:: python

.. tab:: 中文

  该类表示一个文档。它可以从文件或内存中构造。

  该类的别名为 *open*，即 `pymupdf.Document(...)` 和 `pymupdf.open(...)` 的功能完全相同。

  有关 **嵌入文件** 的详细信息，请参阅附录 3。

  .. note::

    从 v1.17.0 开始，**仅针对 EPUB 文件** 引入了一种新的页面寻址机制。此类文档在内部按章节组织，因此最有效的页面查找方式是使用所谓的 "location"（位置）。该位置是一个元组 *(chapter, pno)*，其中：

    - *chapter* 表示章节编号
    - *pno* 表示该章节中的页面编号

    这两个编号均从 0 开始。

    尽管仍然可以使用页面的（绝对）编号进行定位，但这样可能会导致完整的 EPUB 文档在定位前必须完全布局。如果文档非常大，这可能会对性能产生显著影响。而使用 *(chapter, pno)* 进行定位可以避免这一问题。

    为了保持一致的 API，PyMuPDF 支持对 **所有文件类型** 使用页面 *location* 语法——对于不具备此功能的文档，其内容仅包含一个章节。:meth:`Document.load_page` 以及等效的索引访问现在也支持 *location* 参数。

    此外，还提供了一些方法用于在页面编号和位置之间进行转换，获取章节数、每个章节的页面数，以及计算文档的下一页、上一页和最后一页的位置。

  ======================================= ==========================================================
  **方法 / 属性**                         **简要说明**
  ======================================= ==========================================================
  :meth:`Document.add_layer`              仅限 PDF：创建新的可选内容配置
  :meth:`Document.add_ocg`                仅限 PDF：添加新的可选内容组
  :meth:`Document.authenticate`           访问加密文档
  :meth:`Document.bake`                   仅限 PDF：使注释 / 表单字段成为永久内容
  :meth:`Document.can_save_incrementally` 检查是否可以增量保存
  :meth:`Document.chapter_page_count`     章节中的页数
  :meth:`Document.close`                  关闭文档
  :meth:`Document.convert_to_pdf`         将文档转换为 PDF 并存储到内存
  :meth:`Document.copy_page`              仅限 PDF：复制页面引用
  :meth:`Document.del_toc_item`           仅限 PDF：删除单个目录项
  :meth:`Document.delete_page`            仅限 PDF：删除页面
  :meth:`Document.delete_pages`           仅限 PDF：删除多个页面
  :meth:`Document.embfile_add`            仅限 PDF：从缓冲区添加新的嵌入文件
  :meth:`Document.embfile_count`          仅限 PDF：获取嵌入文件的数量
  :meth:`Document.embfile_del`            仅限 PDF：删除嵌入文件条目
  :meth:`Document.embfile_get`            仅限 PDF：提取嵌入文件的缓冲区
  :meth:`Document.embfile_info`           仅限 PDF：获取嵌入文件的元数据
  :meth:`Document.embfile_names`          仅限 PDF：列出嵌入文件
  :meth:`Document.embfile_upd`            仅限 PDF：更新嵌入文件
  :meth:`Document.extract_font`           仅限 PDF：通过 :data:`xref` 提取字体
  :meth:`Document.extract_image`          仅限 PDF：通过 :data:`xref` 提取嵌入图像
  :meth:`Document.ez_save`                仅限 PDF：使用不同默认值调用 :meth:`Document.save`
  :meth:`Document.find_bookmark`          获取文档布局后的书签页位置
  :meth:`Document.fullcopy_page`          仅限 PDF：复制整个页面
  :meth:`Document.get_layer`              仅限 PDF：获取开启、关闭、RBGroups 中的 OCGs
  :meth:`Document.get_layers`             仅限 PDF：获取所有可选内容配置
  :meth:`Document.get_oc`                 仅限 PDF：获取图像 / 表单 xobject 的 OCG / OCMD :data:`xref`
  :meth:`Document.get_ocgs`               仅限 PDF：获取所有可选内容组信息
  :meth:`Document.get_ocmd`               仅限 PDF：检索 :data:`OCMD` 定义
  :meth:`Document.get_page_fonts`         仅限 PDF：获取页面中引用的字体列表
  :meth:`Document.get_page_images`        仅限 PDF：获取页面中引用的图像列表
  :meth:`Document.get_page_labels`        仅限 PDF：获取页面标签定义列表
  :meth:`Document.get_page_numbers`       仅限 PDF：获取具有特定标签的页码
  :meth:`Document.get_page_pixmap`        通过页码创建页面的 pixmap
  :meth:`Document.get_page_text`          通过页码提取页面文本
  :meth:`Document.get_page_xobjects`      仅限 PDF：获取页面引用的 XObjects 列表
  :meth:`Document.get_sigflags`           仅限 PDF：确定签名状态
  :meth:`Document.get_toc`                提取目录（TOC）
  :meth:`Document.get_xml_metadata`       仅限 PDF：读取 XML 元数据
  :meth:`Document.has_annots`             仅限 PDF：检查 PDF 是否包含注释
  :meth:`Document.has_links`              仅限 PDF：检查 PDF 是否包含链接
  :meth:`Document.insert_page`            仅限 PDF：插入新页面
  :meth:`Document.insert_pdf`             仅限 PDF：从另一个 PDF 插入页面
  :meth:`Document.insert_file`            仅限 PDF：从任意文档插入页面
  :meth:`Document.journal_can_do`         仅限 PDF：检查支持的日志操作
  :meth:`Document.journal_enable`         仅限 PDF：启用日志记录
  :meth:`Document.journal_load`           仅限 PDF：从文件加载日志
  :meth:`Document.journal_op_name`        仅限 PDF：获取日志步骤名称
  :meth:`Document.journal_position`       仅限 PDF：获取日志状态
  :meth:`Document.journal_redo`           仅限 PDF：重做当前操作
  :meth:`Document.journal_save`           仅限 PDF：保存日志到文件
  :meth:`Document.journal_start_op`       仅限 PDF：开始新的日志操作并命名
  :meth:`Document.journal_stop_op`        仅限 PDF：结束当前日志操作
  :meth:`Document.journal_undo`           仅限 PDF：撤销当前操作
  :meth:`Document.layer_ui_configs`       仅限 PDF：获取可选内容的 UI 配置
  :meth:`Document.layout`                 重新分页文档（如果支持）
  :meth:`Document.load_page`              读取页面
  :meth:`Document.make_bookmark`          在可重排文档中创建页面指针
  :meth:`Document.move_page`              仅限 PDF：移动页面到文档中不同位置
  :meth:`Document.need_appearances`       仅限 PDF：获取 / 设置 `/NeedAppearances` 属性
  :meth:`Document.new_page`               仅限 PDF：插入新的空白页面
  :meth:`Document.next_location`          返回下一页的 (chapter, pno)
  :meth:`Document.outline_xref`           仅限 PDF：获取目录项的 :data:`xref`
  :meth:`Document.page_cropbox`           仅限 PDF：获取未旋转的页面矩形
  :meth:`Document.page_xref`              仅限 PDF：获取页面的 :data:`xref`
  :meth:`Document.pages`                  迭代页面范围
  :meth:`Document.pdf_catalog`            仅限 PDF：获取 PDF 目录的 :data:`xref`
  :meth:`Document.pdf_trailer`            仅限 PDF：获取 PDF 预告信息
  :meth:`Document.prev_location`          返回前一页的 (chapter, pno)
  :meth:`Document.reload_page`            仅限 PDF：重新加载页面
  :meth:`Document.resolve_names`          仅限 PDF：将目标名称转换为 Python 字典
  :meth:`Document.save`                   仅限 PDF：保存文档
  :meth:`Document.saveIncr`               仅限 PDF：增量保存文档
  :meth:`Document.scrub`                  仅限 PDF：删除敏感数据
  :meth:`Document.search_page_for`        在页面中搜索字符串
  :meth:`Document.select`                 仅限 PDF：选择页面子集
  :meth:`Document.set_layer_ui_config`    仅限 PDF：临时设置 OCG 可见性
  :meth:`Document.set_layer`              仅限 PDF：批量更改 OCG 状态
  :meth:`Document.set_markinfo`           仅限 PDF：设置 MarkInfo 值
  :meth:`Document.set_metadata`           仅限 PDF：设置元数据
  :meth:`Document.set_oc`                 仅限 PDF：将 OCG/OCMD 附加到图像 / 表单 xobject
  :meth:`Document.set_ocmd`               仅限 PDF：创建或更新 :data:`OCMD`
  :meth:`Document.set_page_labels`        仅限 PDF：添加 / 更新页面标签定义
  :meth:`Document.set_pagemode`           仅限 PDF：设置 PageMode
  :meth:`Document.set_pagelayout`         仅限 PDF：设置 PageLayout
  :meth:`Document.set_toc_item`           仅限 PDF：修改单个目录项
  :meth:`Document.set_toc`                仅限 PDF：设置目录（TOC）
  :meth:`Document.set_xml_metadata`       仅限 PDF：创建或更新 XML 元数据
  :meth:`Document.subset_fonts`           仅限 PDF：创建字体子集
  :meth:`Document.switch_layer`           仅限 PDF：激活 OC 配置
  :meth:`Document.tobytes`                仅限 PDF：将文档写入内存
  ======================================= ==========================================================

  **Class API**

  .. class:: Document

    .. index::
      pair: filename; open
      pair: stream; open
      pair: filetype; open
      pair: rect; open
      pair: width; open
      pair: height; open
      pair: fontsize; open
      pair: open; Document
      pair: filename; Document
      pair: stream; Document
      pair: filetype; Document
      pair: rect; Document
      pair: fontsize; Document

    .. method:: __init__(self, filename=None, stream=None, *, filetype=None, rect=None, width=0, height=0, fontsize=11)

      * v1.14.13 版本更改：支持 `io.BytesIO` 作为内存文档。
      * v1.19.6 版本更改：提供更清晰、更简短且更一致的异常消息。如果未指定文件类型，则默认假定为 "pdf"。空文件或空内存区域将始终导致异常。

      创建一个 *Document* 对象。

      * 使用默认参数时，将创建一个 **新的空 PDF** 文档。
      * 如果提供了 *stream*，则从内存创建文档。如果该文档不是 PDF，则必须通过 *filename* 或 *filetype* 指定其类型。
      * 如果 *stream* 为 `None`，则从 *filename* 指定的文件创建文档。其类型将根据文件扩展名推断。该推断可以通过 *filetype* 覆盖。

      :arg str,pathlib filename: 一个 UTF-8 字符串或 *pathlib* 对象，表示文件路径。文档类型由文件名扩展名推断。如果未提供，或者扩展名不匹配 :ref:`支持的类型 <Supported_File_Types>`，则默认假定为 PDF。对于内存文档，可以使用此参数代替 `filetype`，详见下文。

      :arg bytes,bytearray,BytesIO stream: 一个包含支持的文档数据的内存区域。如果不是 PDF，则必须通过 `filename` 或 `filetype` 指定其类型。

      :arg str filetype: 指定文档类型的字符串。它可以是类似于文件名的格式（例如 `"x.pdf"`），此时 MuPDF 会使用扩展名来确定类型，或者 MIME 类型（如 *application/pdf*）。直接使用 `"pdf"` 或 `".pdf"` 也是可以的。对于 PDF 文档，此参数可以省略，否则必须匹配 :ref:`支持的文档类型 <Supported_File_Types>`。

      :arg rect_like rect: 指定所需页面尺寸的矩形。此参数仅对具有可变页面布局（"可重排" 文档，例如电子书或 HTML）的文档有意义，其他情况下会被忽略。如果指定，该矩形必须是非空的有限矩形，且左上角坐标为 (0, 0)。与 *fontsize* 参数结合，每个页面将根据此布局计算，并由此决定总页数。

      :arg float width: 可以与 *height* 一起用作 *rect* 的替代方案，以指定页面布局信息。

      :arg float height: 可以与 *width* 一起用作 *rect* 的替代方案，以指定页面布局信息。

      :arg float fontsize: 可重排文档类型的默认 :data:`fontsize`。如果未指定 *rect*，或者 *width* 和 *height*，则该参数被忽略。否则，将用于计算页面布局。

      :raises TypeError: 如果任一参数的类型不符合要求。
      :raises FileNotFoundError: 如果无法找到文件/路径。该异常被重新实现为 `RuntimeError` 的子类。
      :raises EmptyFileError: 如果文件/路径为空，或内存中的 `bytes` 对象长度为零。该异常是 `FileDataError` 和 `RuntimeError` 的子类。
      :raises ValueError: 如果显式指定了未知的文件类型。
      :raises FileDataError: 如果文档的结构对于给定的类型无效，或者不是文件（例如是一个文件夹）。该异常是 `RuntimeError` 的子类。

      :return: 返回一个文档对象。如果文档无法创建，则按照上述顺序引发异常。请注意，PyMuPDF 特定的异常 `FileNotFoundError`、`EmptyFileError` 和 `FileDataError` 在检查 `RuntimeError` 时也会被拦截。

        如果遇到问题，可以使用以下方式查看内部消息存储的详细信息：
        `print(pymupdf.TOOLS.mupdf_warnings())`
        该调用会清空消息存储，但也可以防止清空，详见 :meth:`Tools.mupdf_warnings`。

      .. note:: 并非所有文档类型都会在打开时立即检查格式的有效性。例如，栅格图像可能会在访问内容时才引发异常。其他类型（尤其是非二进制内容）有时即使内容格式无效也可以打开（甚至可以 **访问** 部分内容）：

        * HTM, HTML, XHTML：**始终** 可以打开，`metadata["format"]` 分别为 `"HTML5"` 和 `"XHTML"`。
        * XML, FB2：**始终** 可以打开，`metadata["format"]` 为 `"FictionBook2"`。

      以下是可能的使用方式，注意：`open` 是 `Document` 的同义词::

          >>> # 从文件打开
          >>> doc = pymupdf.open("some.xps")
          >>> # 处理错误的扩展名
          >>> doc = pymupdf.open("some.file", filetype="xps")
          >>>
          >>> # 从内存打开，如果不是 PDF，必须提供 filetype
          >>> doc = pymupdf.open("xps", mem_area)
          >>> doc = pymupdf.open(None, mem_area, "xps")
          >>> doc = pymupdf.open(stream=mem_area, filetype="xps")
          >>>
          >>> # 创建一个新的空 PDF
          >>> doc = pymupdf.open()
          >>> doc = pymupdf.open(None)
          >>> doc = pymupdf.open("")

      .. note:: 如果栅格图像的文件扩展名错误（但仍然是受支持的格式），**不会有问题**。MuPDF 在实际访问文件 **内容** 时会确定正确的图像类型，并能正常处理。因此，即使 `file.jpg` 实际上是 PNG 图像，`pymupdf.open("file.jpg")` 仍然可以工作。

      `Document` 类也可以用作 **上下文管理器**。退出时，文档会自动关闭。

          >>> import pymupdf
          >>> with pymupdf.open(...) as doc:
                  for page in doc: print("page %i" % page.number)
          page 0
          page 1
          page 2
          page 3
          >>> doc.is_closed
          True
          >>>

    .. method:: get_oc(xref)

      * 新增于 v1.18.4

      返回附加到图像或表单 xobject 的 :data:`OCG` 或 :data:`OCMD` 的交叉引用号。

      :arg int xref: 图像或表单 xobject 的 :data:`xref`。有效的交叉引用号可通过 :meth:`Document.get_page_images` 或 :meth:`Document.get_page_xobjects` 获取。对于无效的编号，将引发异常。
      :rtype: int
      :returns: 可选内容对象的交叉引用号，如果没有则返回 0。

    .. method:: set_oc(xref, ocxref)

      * 新增于 v1.18.4

      如果 *xref* 代表一个图像或表单 xobject，则设置或移除可选内容对象的交叉引用号 *ocxref*。

      :arg int xref: 图像或表单 xobject 的 :data:`xref` [#f12]_。有效的交叉引用号可通过 :meth:`Document.get_page_images` 或 :meth:`Document.get_page_xobjects` 获取。对于无效的编号，将引发异常。
      :arg int ocxref: :data:`OCG` / :data:`OCMD` 的 :data:`xref` 号。如果不为 0，则无效的引用会引发异常。如果为 0，则移除任何 OC 引用。

    .. method:: get_layers()

      * 新增于 v1.18.3

      显示可选的图层配置。始终存在一个标准配置，但不会包含在返回结果中。

        >>> for item in doc.get_layers(): print(item)
        {'number': 0, 'name': 'my-config', 'creator': ''}
        >>> # 在 add_ocg 中使用 'number' 作为配置标识符

    .. method:: add_layer(name, creator=None, on=None)

      * 新增于 v1.18.3

      添加一个可选内容配置。图层用于管理可选内容组的开 / 关状态，并允许在同一文档的不同视图之间快速切换可见性。

      :arg str name: 任意名称。
      :arg str creator: (可选) 创建此图层的软件。
      :arg sequ on: 需要在此图层激活时设置为 ON 的 OCG :data:`xref` 编号序列。所有未列出的 OCG 将被设置为 OFF。

    .. method:: switch_layer(number, as_default=False)

      * 新增于 v1.18.3

      切换到由可选图层的配置编号定义的文档视图。该操作是临时的，除非设定为默认配置。

      :arg int number: 由 :meth:`Document.layer_configs` 返回的配置编号。
      :arg bool as_default: 是否将此配置设为默认。

      激活 OCG 的开 / 关状态，该状态由指定图层定义。如果 *as_default=True*，则所有图层（包括标准图层）将被合并，并将结果写回标准图层，同时 **所有可选图层都会被删除**。

    .. method:: add_ocg(name, config=-1, on=True, intent="View", usage="Artwork")

      * 新增于 v1.18.3

      添加一个可选内容组。OCG 是确定对象可见性的最重要单位。对于 PDF，如果希望支持可选内容，至少需要存在一个 OCG。

      :arg str name: 任意名称，将显示在支持的 PDF 查看器中。
      :arg int config: 图层配置编号。默认值 -1 表示标准配置。
      :arg bool on: 指向该 OCG 的对象的标准可见性状态。
      :arg str,list intent: 一个字符串或字符串列表，声明可见性意图。PDF 标准提供了两个选项："View" 和 "Design"。默认值为 "View"。**拼写必须正确**。
      :arg str usage: 影响 OCG 可见性的另一个因素。此值将成为 OCG 的 `/Usage` 键的一部分。PDF 标准提供了两个选项："Artwork" 和 "Technical"，默认值为 "Artwork"。仅在必要时更改。

      :returns: 创建的 OCG 的 :data:`xref`。可用于支持对象的 `oc` 参数。

      .. note:: 可以创建多个具有相同参数的 OCG，这不会导致问题。使用 :meth:`Document.save` 的垃圾回收选项 3 可删除任何重复项。

    .. method:: set_ocmd(xref=0, ocgs=None, policy="AnyOn", ve=None)

      * 新增于 v1.18.4

      创建或更新一个 :data:`OCMD`，即 **可选内容成员字典 (Optional Content Membership Dictionary)**。

      :arg int xref: 要更新的 OCMD 的 :data:`xref`，如果为 0，则创建一个新的 OCMD。
      :arg list ocgs: 现有 :data:`OCG` PDF 对象的 :data:`xref` 号序列。
      :arg str policy: 可选值为 "AnyOn"（默认）、"AnyOff"、"AllOn"、"AllOff"（可混合大小写）。
      :arg list ve: “可见性表达式”，由任意嵌套的列表组成（详细说明见下文）。如果需要更复杂的条件，可以使用此参数替代 *ocgs* / *policy* 组合。
      :rtype: int
      :returns: OCMD 的 :data:`xref`，可作为支持对象的 `oc=xref` 参数，也可用于 :meth:`Document.set_oc` 或 :meth:`Annot.set_oc`。

      .. note::

        与 OCG 类似，OCMD 具有 ON 或 OFF 的可见性状态，并可与 OCG 相同方式使用。但 OCMD 通过 **布尔表达式** 计算一个或多个 OCG 的状态，以决定自身的可见性。如果表达式计算结果为 true，则 OCMD 处于 ON 状态；若为 false，则处于 OFF 状态。

        OCMD 可通过以下两种方式设定可见性：

        1. 使用 *ocgs* 和 *policy* 组合：
          
          *policy* 的值含义如下：
          
          - **AnyOn** ——（默认）至少有一个 OCG 处于 ON 状态，则 OCMD 也为 ON。
          - **AnyOff** —— 只要有一个 OCG 处于 OFF 状态，则 OCMD 为 ON。
          - **AllOn** —— 仅当所有 OCG 均为 ON 时，OCMD 才为 ON。
          - **AllOff** —— 仅当所有 OCG 均为 OFF 时，OCMD 才为 ON。

          假设希望两个 PDF 对象交替显示（一个 ON，另一个必须 OFF）：
          
          解决方案：对象 1 使用 **OCG**，对象 2 使用 **OCMD**。通过 `set_ocmd(ocgs=[xref], policy="AllOff")` 创建 OCMD，其中 *xref* 为 OCG 的交叉引用号。

        2. 使用 **可见性表达式** *ve*：
          
          该参数是一个至少包含两个元素的列表，**第一个元素** 必须是逻辑关键字之一："and"、"or" 或 "not"。**第二个及后续元素** 必须是整数或嵌套列表：
          
          - 每个列表必须以逻辑关键字开头。
          - 若关键字为 **"not"**，列表必须包含恰好两个元素。
          - 若关键字为 **"and"** 或 **"or"**，则后续可以有任意数量的元素。
          - 逻辑关键字后面的元素可以是整数（OCG 的 xref 号）或嵌套列表（必须符合上述规则）。

          **示例：**
          
          - `set_ocmd(ve=["or", 4, ["not", 5], ["and", 6, 7]])`  
            逻辑含义：**“4 为 ON，或 5 为 OFF，或 6 和 7 均为 ON”**。
          - `set_ocmd(ve=["not", xref])`  
            其效果与示例 1 中的 OCMD 相同。

          更多详细信息和示例可参考 :ref:`AdobeManual` 第 224 页，以及 `此处 <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/optional-content>`_ 的示例脚本。

          可见性表达式 (`/VE`) 属于 PDF 规范 1.6 版的一部分，因此并非所有 PDF 查看器都支持此功能。对于不支持的查看器，将采用某种标准处理方式。

    .. method:: get_ocmd(xref)

      * 新增于 v1.18.4

      获取 :data:`OCMD` 的定义。

      :arg int xref: OCMD 的 :data:`xref`。
      :rtype: dict
      :returns: 包含以下键的字典：*xref*、*ocgs*、*policy* 和 *ve*。

    .. method:: get_layer(config=-1)

      * 新增于 v1.18.3

      按状态列出指定配置中的可选内容组。返回包含 OCG 交叉引用号列表的字典，这些 OCG 出现在 `/ON`、`/OFF` 或单选按钮组 (`/RBGroups`) 数组中。

      :arg int config: 配置层编号（默认值为标准配置层）。

      >>> pprint(doc.get_layer())
      {'off': [8, 9, 10], 'on': [5, 6, 7], 'rbgroups': [[7, 10]]}
      >>>

    .. method:: set_layer(config, *, on=None, off=None, basestate=None, rbgroups=None, locked=None)

      * 新增于 v1.18.3

      * v1.22.5 变更：支持 *locked* OCGs 列表。

      批量更改可选内容组（OCG）的状态。此方法 **永久** 设置 OCG 的状态。

      :arg int config: 目标配置层，选择 -1 代表默认配置层。
      :arg list on: 需要设置为 ON 的 OCG 的 :data:`xref` 号列表。会替换先前的值。空列表表示不再将任何 OCG 设为 ON。若使用 `basestate="ON"`，则应指定此参数。
      :arg list off: 需要设置为 OFF 的 OCG 的 :data:`xref` 号列表。会替换先前的值。空列表表示不再将任何 OCG 设为 OFF。若使用 `basestate="OFF"`，则应指定此参数。
      :arg str basestate: 未在 *on* 或 *off* 中提及的 OCG 的状态。可能的取值为 "ON"、"OFF" 或 "Unchanged"（不变）。大小写均可。
      :arg list rbgroups: 一个列表，包含多个子列表。会替换先前的值。每个子列表包含两个或多个 OCG 的 xref 号。处于同一子列表中的 OCG 会像单选按钮一样处理：设定其中一个为 ON，会自动将同组中的其他 OCG 设为 OFF。
      :arg list locked: OCG xref 号列表，其中的 OCG 不能通过用户界面更改状态。

      若参数值为 `None`，则不会更改对应的 PDF 数组。

        >>> doc.set_layer(-1, basestate="OFF")  # 仅更改基础状态
        >>> pprint(doc.get_layer())
        {'basestate': 'OFF', 'off': [8, 9, 10], 'on': [5, 6, 7], 'rbgroups': [[7, 10]]}

    .. method:: get_ocgs()

      * 新增于 v1.18.3

      获取所有可选内容组 (OCG) 的详细信息。返回一个字典，其中键为 OCG 的 :data:`xref` 号，值为 OCG 的属性字典：

        >>> pprint(doc.get_ocgs())
        {13: {'on': True,
              'intent': ['View', 'Design'],
              'name': 'Circle',
              'usage': 'Artwork'},
        14: {'on': True,
              'intent': ['View', 'Design'],
              'name': 'Square',
              'usage': 'Artwork'},
        15: {'on': False, 'intent': ['View'], 'name': 'Square', 'usage': 'Artwork'}}
        >>>

    .. method:: layer_ui_configs()

      * 新增于 v1.18.3

      显示支持 PDF 查看器的用户界面可修改的可选内容可见性状态。

          * 仅报告当前所选层配置中包含的项目。

          * 字典键的含义如下：

            - *depth:* 项目在 `/Order` 数组中的嵌套级别
            - *locked:* 若为 true，则无法通过用户界面更改
            - *number:* 运行序列号
            - *on:* 项目状态
            - *text:* 该 OCG 的文本字符串或名称字段
            - *type:* 可能的值包括 "label"（由文本字符串设定）、"checkbox"（由单个 OCG 设定）或 "radiobox"（由一组相关 OCG 设定）

    .. method:: set_layer_ui_config(number, action=0)

      * 新增于 v1.18.3

      修改内容组的可见性状态。其作用类似于支持 PDF 查看器提供的用户界面功能。

        请注意，可见性 **不是** OCG 本身存储的属性，甚至不一定包含在 PDF 文档中。当前可见性是通过支持的 PDF 软件用户界面 **临时** 设定的。本方法提供了相同类型的功能。

        若需 **永久** 更改可见性，请使用 :meth:`Document.set_layer`。

      :arg int,str number: 可选项的序列号（在 :meth:`Document.layer_configs` 列表中的索引）或该项的 *text* 字段值。
      :arg int action:  
          - `PDF_OC_ON` = 设为 ON（默认值）
          - `PDF_OC_TOGGLE` = 切换 ON/OFF
          - `PDF_OC_OFF` = 设为 OFF

    .. method:: authenticate(password)

      使用字符串 *password* 解密文档。若解密成功，即可访问文档数据。对于 PDF 文档，“所有者”与“用户”具有不同的权限，因此可能存在不同的密码。该方法会根据提供的密码自动建立适当的访问权限（所有者或用户）。

      :arg str password: 所有者或用户密码。

      :rtype: int
      :returns: 若成功，则返回正值；若失败（密码不匹配），则返回 0。若返回正值，则 :attr:`Document.is_encrypted` 设为 *False*。 **正值** 的含义如下：

        * 1 => 认证成功，但 PDF 既无所有者密码，也无用户密码。
        * 2 => 认证成功，使用的是 **用户** 密码。
        * 4 => 认证成功，使用的是 **所有者** 密码。
        * 6 => 认证成功，且所有者密码与用户密码相同（可能较为罕见）。

        .. note::

          文档可能受所有者密码保护，但 **未** 受用户密码保护。可通过 `doc.authenticate("") == 2` 进行检测。这种情况下，文档可在 **无需认证** 的情况下打开和阅读，但具体操作可能受 :attr:`Document.permissions` 限制。然而，PyMuPDF（与 MuPDF 一样） **忽略** 这些限制。因此，与普通 PDF 查看器不同，即使 `PDF_PERM_COPY`、`PDF_PERM_MODIFY`、`PDF_PERM_ANNOTATE` 等权限被禁用，仍可提取文本、添加或修改内容。请确保您的应用符合相关法律要求。

    .. method:: get_page_numbers(label, only_one=False)

      * 新增于 v1.18.6

      仅适用于 PDF：返回具有指定标签的页面编号列表——请注意，在 PDF 中，页面标签可能并不唯一。因此，该方法会 **顺序搜索所有页面**，以比较其标签。

      .. note:: 实现细节 —— 该方法 **不会加载页面** 进行搜索。

      :arg str label: 要查找的标签，例如 "vii"（罗马数字 7）。
      :arg bool only_one: 若为 True，则在找到第一个匹配项后停止。适用于已知标签唯一或文档页面较多的情况。默认行为是检查所有页面。
      :rtype: list
      :returns: 包含所有匹配页面编号的列表。若未找到匹配项，或文档未定义页面标签，则返回空列表。

    .. method:: get_page_labels()

      * 新增于 v1.18.7

      仅适用于 PDF：提取页面标签定义列表。通常用于修改后传递给 :meth:`Document.set_page_labels` 进行更新。

      :returns: 以字典形式表示的页面标签定义列表，格式与 :meth:`Document.set_page_labels` 相同。

    .. method:: set_page_labels(labels)

      * 新增于 v1.18.6

      仅适用于 PDF：新增或更新 PDF 的页面标签定义。

      :arg list labels: 由字典组成的列表，每个字典定义一个标签规则，并指定起始页面编号（从 0 开始）。每个字典最多包含 4 个键，格式如下：
      
          `{'startpage': int, 'prefix': str, 'style': str, 'firstpagenum': int}`，其中：

          - `startpage`: (int) 规则适用的第一个页面（从 0 开始）。**必须提供**。该规则适用于所有后续页面，直到文档结束，或被下一个规则覆盖。
          - `prefix`: (str) 标签前缀，例如 "A-"。默认为 ""。
          - `style`: (str) 编号样式，可选值:

            - `"D"`：十进制数字
            - `"r"/"R"`：罗马数字（小写 / 大写）
            - `"a"/"A"`：字母编号（小写 / 大写）。示例："a" 至 "z"，然后 "aa" 至 "zz"，以此类推。
            - 默认为 ""，即无编号，仅使用 `prefix` 作为标签。若 `prefix` 也为空，则该范围内的页面标签将为空字符串 ""。
          
          - `firstpagenum`: (int) 编号的起始值。默认为 1，若小于 1，则忽略。

      例如::

        [{'startpage': 6, 'prefix': 'A-', 'style': 'D', 'firstpagenum': 10},
        {'startpage': 10, 'prefix': '', 'style': 'D', 'firstpagenum': 1}]

      以上规则会生成如下页面标签："A-10", "A-11", "A-12", "A-13", "1", "2", "3", ... 对应的页面为 6, 7, 8, 9, 10, 11, 12，直到文档结束。页面 0 至 5 的标签为空字符串 ""。

    .. method:: make_bookmark(loc)

      * 新增于 v1.17.3

      在可重排（reflowable）文档中返回一个页面指针。重新布局文档后，可以使用该方法的返回值来查找该页面的新位置。

      .. note:: **不要与目录 (TOC) 条目混淆**。

      :arg list,tuple loc: 页面位置。必须是有效的 *(chapter, pno)*。

      :rtype: pointer
      :returns: 一个长整数，表示指针格式。可用于在文档重新布局后查找新位置。请勿修改或重新分配此值。

    .. method:: find_bookmark(bookmark)

      * 新增于 v1.17.3

      重新布局文档后，返回书签对应的新页面位置。

      :arg pointer bookmark: 由 :meth:`Document.make_bookmark` 创建的书签。

      :rtype: tuple
      :returns: 该页面的新位置 *(chapter, pno)*。

    .. method:: chapter_page_count(chapter)

      * 新增于 v1.17.0

      返回指定章节的页面数量。

      :arg int chapter: 以 0 为基准的章节编号。

      :rtype: int
      :returns: 该章节的页面数。仅适用于支持章节结构的文档类型（目前适用于 EPUB）。

    .. method:: next_location(page_id)

      * 新增于 v1.17.0

      返回下一页的位置。

      :arg tuple page_id: 当前页面的标识，必须是 *(chapter, pno)* 形式的元组，指向现有页面。

      :returns: 下一页的位置，即 *(chapter, pno + 1)* 或 *(chapter + 1, 0)*， **如果当前页面为最后一页，则返回空元组 ()** 。仅适用于支持章节结构的文档类型（目前适用于 EPUB）。

    .. method:: prev_location(page_id)

      * 新增于 v1.17.0

      返回前一页的位置。

      :arg tuple page_id: 当前页面的标识，必须是 *(chapter, pno)* 形式的元组，指向现有页面。

      :returns: 前一页的位置，即 *(chapter, pno - 1)* 或前一章节的最后一页， **如果当前页面为第一页，则返回空元组 ()**。仅适用于支持章节结构的文档类型（目前适用于 EPUB）。

    .. method:: load_page(page_id=0)

      * 变更于 v1.17.0: 对于支持“章节结构”的文档类型（如 EPUB），页面现在也可以通过章节号和相对页码来加载，而不仅仅是绝对页码。这在处理大型文档时 **显著提高了访问速度**。

      创建一个 :ref:`Page` 对象，以便进行后续处理（如渲染、文本搜索等）。

      :arg int,tuple page_id: *(变更于 v1.17.0)*

          可以是 0 基准的页面编号，也可以是 *(chapter, pno)* 形式的元组。

          - **整数类型**: 允许 `-∞ < page_id < page_count`。如果 `page_id` 为负数，则会自动加上 :attr:`page_count`。例如，要加载最后一页，可以使用 *doc.load_page(-1)*，此时 `page.number = doc.page_count - 1`。
          - **元组类型**: `chapter` 必须在 :attr:`Document.chapter_count` 范围内，`pno` 必须在该章节的 :meth:`Document.chapter_page_count` 范围内。两者均从 0 开始计数。使用此格式时，:attr:`Page.number` 将等于提供的元组值。

      :rtype: :ref:`Page`

    .. note::

      文档也遵循 Python 序列协议，页面编号作为索引：*doc.load_page(n) == doc[n]*。

      仅对 **绝对页面编号**，像 *"for page in doc: ..."* 和 *"for page in reversed(doc): ..."* 这样的表达式将依次返回文档的页面。参见 :meth:`Document.pages`，它允许像切片一样处理页面。

      你也可以使用基于章节的页面标识符的索引表示法：使用 *page = doc[(5, 2)]* 加载第六章的第三页。

      为了保持一致的 API，对于不支持章节结构的文档类型（如 PDF），:attr:`Document.chapter_count` 为 1，页面也可以通过元组 *(0, pno)* 来加载。请参阅此 [#f10]_ 脚注，了解性能改进的说明。

    .. method:: reload_page(page)

      * 在 v1.16.10 中新增

      仅适用于 PDF：在完成并更新所有待处理更改后，提供页面的新副本。

      :arg page: 页面对象。
      :type page: :ref:`Page`

      :rtype: :ref:`Page`

      :returns: 同一页面的新副本。所有待处理更新（例如注释或小部件）将被最终确定，并将加载页面的新副本。

        .. note:: 在典型的使用情况下，应该在添加或更改注释/小部件后获取页面 :ref:`Pixmap`。为了强制所有更改反映在页面结构中，此方法重新加载一个新副本，同时保持对象层次结构“文档 -> 页面 -> 注释/小部件”不变。

    .. method:: resolve_names()

      仅适用于 PDF：将目标名称转换为 Python 字典。

      :returns:
          一个字典，包含以下布局：

          * *key*: (str) 名称。
          * *value*: (dict)，具有以下布局：
              * "page": 目标页面编号（从 0 开始）。如果未找到页面编号，则为 -1。
              * "to": (x, y) 页面上的目标点。当前为 PDF 坐标，
                即点 (0,0) 是页面的左下角。
              * "zoom": (float) 缩放因子。
              * "dest": (str) 仅在目标位置未提供为 "/XYZ" 或未找到页面编号时出现。
          示例::

              {
                  '__bookmark_1': {'page': 0, 'to': (0.0, 541.0), 'zoom': 0.0},
                  '__bookmark_2': {'page': 0, 'to': (0.0, 481.45), 'zoom': 0.0},
              }

          或::

              {
                  '21154a7c20684ceb91f9c9adc3b677c40': {'page': -1, 'dest': '/XYZ 15.75 1486 0'},
                  ...
              }

      所有在目录中通过键 "/Dests" 和 "/Names/Dests" 找到的名称都会被包括。

      * 在 v1.23.6 中新增

    .. method:: page_cropbox(pno)

      * 在 v1.17.7 中新增

      仅适用于 PDF：返回未旋转的页面矩形—— **无需加载页面** （通过 :meth:`Document.load_page`）。此方法用于需要最佳性能的内部用途。

      :arg int pno: 从 0 开始的页面编号。

      :returns: :ref:`Rect` 页面，如 :meth:`Page.rect`，但忽略任何旋转。

    .. method:: page_xref(pno)

      * 在 v1.17.7 中新增

      仅适用于 PDF：返回页面的 :data:`xref` —— **无需加载页面** （通过 :meth:`Document.load_page`）。此方法用于需要最佳性能的内部用途。

      :arg int pno: 从 0 开始的页面编号。

      :returns: 页面 :data:`xref`，如 :attr:`Page.xref`。

    .. method:: pages(start=None, [stop=None, [step=None]])

      * 在 v1.16.4 中新增

      一个用于页面范围的生成器。参数的含义与内置函数 *range()* 相同。用于类似 *"for page in doc.pages(start, stop, step): ..."* 的表达式。

      :arg int start: 从此页面编号开始迭代。默认值为零，允许的值为 `-∞ < start < page_count`。当为负数时，:attr:`page_count` 会 **在** 开始迭代之前加上。
      :arg int stop: 在此页面编号停止迭代。默认值为 :attr:`page_count`，允许的值为 `-∞ < stop <= page_count`。较大的值会 **默默地替换** 为默认值。负值会循环按相反顺序返回页面。与内置的 *range()* 一样，这是 **不** 返回的第一个页面。
      :arg int step: 步进值。如果 start < stop，则默认为 1；如果 start > stop，则默认为 -1。不允许为零。

      :returns: 文档页面的生成器迭代器。以下是一些示例：

          * "doc.pages()" 返回所有页面。
          * "doc.pages(4, 9, 2)" 返回页面 4、6、8。
          * "doc.pages(0, None, 2)" 返回所有偶数页。
          * "doc.pages(-2)" 返回最后两页。
          * "doc.pages(-1, -1)" 返回所有页面，按相反顺序。
          * "doc.pages(-1, -10)" 总是以相反顺序返回 10 页，从最后一页开始——如果文档少于 10 页，则**重复**返回。例如，对于一个 4 页的文档，返回的页面编号依次为：3, 2, 1, 0, 3, 2, 1, 0, 3, 2, 1, 0, 3。

    .. index::
      pair: from_page; Document.convert_to_pdf
      pair: to_page; Document.convert_to_pdf
      pair: rotate; Document.convert_to_pdf

    .. method:: convert_to_pdf(from_page=-1, to_page=-1, rotate=0)

      创建当前文档的 PDF 版本并将其写入内存。 **所有文档类型** 都受支持。参数与 :meth:`insert_pdf` 中的含义相同。实际上，您可以限制转换为页面子集、指定页面旋转角度，并逆转页面顺序。

      :arg int from_page: 要复制的第一页（从 0 开始）。默认是第一页。

      :arg int to_page: 要复制的最后一页（从 0 开始）。默认是最后一页。

      :arg int rotate: 旋转角度。默认是 0（不旋转）。应为 *n * 90*，其中 n 是整数（未检查）。

      :rtype: bytes
      :returns: 一个包含 PDF 文件图像的 Python *bytes* 对象。它是通过内部使用 `tobytes(garbage=4, deflate=True)` 创建的。参见 :meth:`tobytes`。您可以直接将其输出到磁盘或将其作为 PDF 打开。以下是一些示例::

        >>> # 将 XPS 文件转换为 PDF
        >>> xps = pymupdf.open("some.xps")
        >>> pdfbytes = xps.convert_to_pdf()
        >>>
        >>> # 任选其一 -->
        >>> pdf = pymupdf.open("pdf", pdfbytes)
        >>> pdf.save("some.pdf")
        >>>
        >>> # 或者这样 -->
        >>> pdfout = open("some.pdf", "wb")
        >>> pdfout.tobytes(pdfbytes)
        >>> pdfout.close()

        >>> # 将图像文件复制到 PDF 页面
        >>> # 每个页面将具有图像尺寸
        >>> doc = pymupdf.open()                     # 新建 PDF
        >>> imglist = [ ... 图像文件名 ...] # 例如一个目录列表
        >>> for img in imglist:
                imgdoc=pymupdf.open(img)           # 打开图像作为文档
                pdfbytes=imgdoc.convert_to_pdf()  # 创建一个 1 页的 PDF
                imgpdf=pymupdf.open("pdf", pdfbytes)
                doc.insert_pdf(imgpdf)             # 插入图像 PDF
        >>> doc.save("allmyimages.pdf")

      .. note:: 该方法使用与 *mutool convert* CLI 相同的逻辑。在大多数情况下，这非常有效——但是，需要注意以下限制。

        * 图像文件：完美，无检测到问题。然而，图像透明度会被忽略。如果您需要透明度（如水印），请使用 :meth:`Page.insert_image`。否则，推荐使用此方法，因为它性能更好。
        * XPS：外观非常好。链接正常，书签（大纲）丢失，但可以轻松恢复 [#f9]_。
        * EPUB, CBZ, FB2：与 XPS 相似。
        * SVG：中等。大致可与 `svglib <https://github.com/deeplook/svglib>`_ 相比。

    .. method:: get_toc(simple=True)

      从文档的目录链创建目录（TOC）。

      :arg bool simple: 指示是否需要简单或详细的目录。如果 *False*，列表中的每个条目还将包含一个字典，包含每个大纲条目的 :ref:`linkDest` 详细信息。

      :rtype: list

      :returns: 一个列表的列表。每个条目的形式为 *[lvl, title, page, dest]*。其条目有以下含义：

        * *lvl* -- 层级（正整数）。第一个条目总是 1。行中的条目要么 **相等** ，要么 **增加** 1，或者 **减少** 任意数目。
        * *title* -- 标题（字符串）。
        * *page* -- 基于 1 的源页面编号（整数）。如果没有目标或在文档之外，则为 `-1`。
        * *dest* -- （字典）仅在 *simple=False* 时包含。包含以下 TOC 项目的详细信息：

          - kind: 目标类型，见 :ref:`linkDest Kinds`。
          - file: 如果类型为 :data:`LINK_GOTOR` 或 :data:`LINK_LAUNCH`，则为文件名。
          - page: 目标页面，基于 0，只有 :data:`LINK_GOTOR` 或 :data:`LINK_GOTO`。
          - to: 目标页面上的位置（:ref:`Point`）。
          - zoom: 目标页面的缩放因子（浮动）。
          - xref: 项目的 :data:`xref` （如果没有 PDF，则为 0）。
          - color: 项目在 PDF 中的 RGB 颜色格式 `(红色，绿色，蓝色)`，或者省略（如果没有 PDF，则始终省略）。
          - bold: 如果项目文本是粗体，则为 true，或者省略。仅限 PDF。
          - italic: 如果项目文本是斜体，则为 true，或者省略。仅限 PDF。
          - collapse: 如果子项被折叠，则为 true，或者省略。仅限 PDF。
          - nameddest: 如果 kind=4，目标名称。仅限 PDF。（在 1.23.7 中新增）

    .. method:: xref_get_keys(xref)

      * 在 v1.18.7 中新增

      仅限 PDF：返回由其 xref 编号提供的 PDF 字典对象的字典键。

      :arg int xref: 该 :data:`xref`。 *(在 v1.18.10 中更改)* 使用 `-1` 来访问特殊字典“PDF trailer”。

      :returns: 一个包含对象 :data:`xref` 中字典键的元组。示例：

        >>> from pprint import pprint
        >>> import pymupdf
        >>> doc=pymupdf.open("pymupdf.pdf")
        >>> xref = doc.page_xref(0)  # 页面 0 的 xref
        >>> pprint(doc.xref_get_keys(xref))  # 页面主键
        ('Type', 'Contents', 'Resources', 'MediaBox', 'Parent')
        >>> pprint(doc.xref_get_keys(-1))  # trailer 的主键
        ('Type', 'Index', 'Size', 'W', 'Root', 'Info', 'ID', 'Length', 'Filter')
        >>>

    .. method:: xref_get_key(xref, key)

      * 在 v1.18.7 中新增

      仅限 PDF：返回给定 xref 的 PDF 字典键的类型和值。

      :arg int xref: 该 :data:`xref`。*在 v1.18.10 中更改:* 使用 `-1` 来访问特殊字典“PDF trailer”。

      :arg str key: 所需的 PDF 键。必须 **完全** 匹配（区分大小写） :meth:`Document.xref_get_keys` 中包含的键。

      :rtype: tuple

      :returns: 一个元组 (type, value) 的字符串，其中 type 是以下之一：“xref”，“array”，“dict”，“int”，“float”，“null”，“bool”，“name”，“string” 或 “unknown”（应避免发生）。独立于 "type" ，键的值 **始终** 格式化为字符串——见下例——并且（几乎总是）忠实地反映 PDF 中存储的内容。在大多数情况下，值字符串的格式还可以提供有关键类型的线索：

      * "name" 总是以 "/" 斜杠开头。
      * "xref" 总是以 " 0 R" 结尾。
      * "array" 总是用 "[...]" 括起来。
      * "dict" 总是用 "<<...>>" 括起来。
      * "bool" 或 "null" 总是等于 "true"、"false" 或 "null"。
      * "float" 和 "int" 以其字符串格式表示——因此不能总是区分。
      * "string" 被转换为 UTF-8，因此可能与 PDF 中存储的内容有所不同。例如，PDF 键 "Author" 在文件中的值可能是 "<FEFF004A006F0072006A00200058002E0020004D0063004B00690065>"，但该方法将返回 `('string', 'Jorj X. McKie')`。

        >>> for key in doc.xref_get_keys(xref):
                print(key, "=" , doc.xref_get_key(xref, key))
        Type = ('name', '/Page')
        Contents = ('xref', '1297 0 R')
        Resources = ('xref', '1296 0 R')
        MediaBox = ('array', '[0 0 612 792]')
        Parent = ('xref', '1301 0 R')
        >>> #
        >>> # 现在对于 PDF trailer 做同样的事情。
        >>> # 它没有 xref，因此必须使用 -1。
        >>> #
        >>> for key in doc.xref_get_keys(-1):
                print(key, "=", doc.xref_get_key(-1, key))
        Type = ('name', '/XRef')
        Index = ('array', '[0 8802]')
        Size = ('int', '8802')
        W = ('array', '[1 3 1]')
        Root = ('xref', '8799 0 R')
        Info = ('xref', '8800 0 R')
        ID = ('array', '[<DC9D56A6277EFFD82084E64F9441E18C><DC9D56A6277EFFD82084E64F9441E18C>]')
        Length = ('int', '21111')
        Filter = ('name', '/FlateDecode')
        >>>

    .. method:: xref_set_key(xref, key, value)

      * 在 v1.18.7 中新增，v1.18.13 中更改
      * 在 v1.19.4 中更改：如果设置为 "null"，则“物理”地移除一个键。

      仅限 PDF：为通过其 xref 提供的 :data:`字典` 对象设置（添加、更新、删除）PDF 键的值。

      .. caution:: 这是一个专家功能：如果您不知道自己在做什么，存在将（部分）PDF 渲染为不可用的高风险。请参考 :ref:`AdobeManual` 中关于对象规范格式（第18页）以及诸如页面对象等特殊字典类型的结构。

      :arg int xref: 该 :data:`xref`。*在 v1.18.13 中更改：* 若要更新 PDF trailer，需指定 -1。
      :arg str key: 所需的 PDF 键（不带前导的 "/"）。不能为空。任何有效的 PDF 键——无论是已经存在于对象中（将被覆盖）还是新的。可以使用 PDF 路径表示法，如 `"Resources/ExtGState"`——这会将键 `"/ExtGState"` 的值设置为 `"/Resources"` 的子对象。
      :arg str value: 键的值。它必须是一个非空字符串，并且根据所需的 PDF 对象类型，必须遵守以下规则。存在某些语法检查，但 **没有类型检查** ，也没有检查其在 PDF 上是否合理，即 **没有语义检查** 。大小写非常重要！

      * **xref** -- 必须作为 `"nnn 0 R"` 提供，其中 nnn 为有效的 :data:`xref` 编号。后缀 "`0 R`" 必须存在，以便 PDF 应用程序识别为 xref。
      * **array** -- 类似 `"[a b c d e f]"` 的字符串。方括号是必需的。数组项必须至少用一个空格分隔（而不是像 Python 中的逗号）。空数组 `"[]"` 是可能的，并且*等同于*删除该键。数组项可以是任何 PDF 对象，如字典、xref、其他数组等。与 Python 中一样，数组项可以是不同类型的。
      * **dict** -- 类似 `"<< ... >>"` 的字符串。方括号是必需的，且必须包含有效的 PDF 字典定义。空字典 `"<<>>"` 是可能的，并且*等同于*删除该键。
      * **int** -- 以字符串格式表示的整数。
      * **float** -- 以字符串格式表示的浮点数。PDF 不允许使用科学记数法（带有指数）。
      * **null** -- 字符串 `"null"`。这是 PDF 等效于 Python 的 `None`，并导致该键被忽略——但不一定被移除，或者在保存时通过垃圾回收移除。*在 v1.19.4 中更改：* 如果键不是路径层次（即不包含斜杠 "/"），则将完全移除它。
      * **bool** -- 字符串 `"true"` 或 `"false"`。
      * **name** -- 有效的 PDF 名称，前导斜杠，例如：`"/PageLayout"`。请参见 :ref:`AdobeManual` 第16页。
      * **string** -- 有效的 PDF 字符串。**所有 PDF 字符串必须被括在括号中**。空字符串表示为 `"()"`。根据内容，可能使用以下括号：

        - "(...)" 用于仅包含 ASCII 的文本。保留的 PDF 字符必须使用反斜杠转义，非 ASCII 字符必须使用 3 位的反斜杠转义八进制表示——包括前导零。例如：12 = 0x0C 必须编码为 `\014`。
        - "<...>" 用于十六进制编码文本。每个字符必须由两个十六进制数字表示（大小写均可）。

        - 如果不确定， **强烈推荐** 使用 :meth:`get_pdf_str`！此函数会自动生成正确的括号、转义和整体格式。例如：

          >>> # 由于 € 符号，以下结果为 UTF-16BE BOM
          >>> pymupdf.get_pdf_str("Pay in $ or €.")
          '<feff00500061007900200069006e002000240020006f0072002020ac002e>'
          >>> # 对括号和非 ASCII 字符进行转义
          >>> pymupdf.get_pdf_str("Prices in EUR (USD also accepted). Areas are in m².")
          '(Prices in EUR \\(USD also accepted\\). Areas are in m\\262.)'


    .. method:: get_page_pixmap(pno: int, *, matrix: matrix_like = Identity, dpi=None, colorspace: Colorspace = csRGB, clip: rect_like = None, alpha: bool = False, annots: bool = True)

      从页面 *pno* （基于零）创建一个位图。调用 :meth:`Page.get_pixmap`。

      所有参数除了 `pno` 都是 *仅限关键字* 。

      :arg int pno: 页面编号，基于零，`-∞ < pno < page_count`。

      :rtype: :ref:`Pixmap`

    .. method:: get_page_xobjects(pno)

      * 在 v1.16.13 中新增
      * 在 v1.18.11 中更改

      仅限 PDF：返回页面引用的所有 XObjects 的列表。

      :arg int pno: 页面编号，基于零，`-∞ < pno < page_count`。

      :rtype: list
      :returns: 一个包含（非图像）XObjects 的列表。这些对象通常表示嵌入的（而非复制的）其他 PDF 页面。例如， :meth:`Page.show_pdf_page` 将创建这种类型的对象。该列表中的一项具有以下布局：`(xref, name, invoker, bbox)`，其中：

        * **xref** (*int*) 是 XObject 的 :data:`xref`。
        * **name** (*str*) 是引用 XObject 的符号名称。
        * **invoker** (*int*) 是调用 XObject 的 :data:`xref`，如果页面直接调用它，则为零。
        * **bbox** (:ref:`Rect`) 是 XObject 在页面上的边界框位置 **未经过变换的坐标** 。要获取实际的、非旋转的页面坐标，可以与页面的变换矩阵 :attr:`Page.transformation_matrix` 相乘。*在 v1.18.11 中更改：* bbox 现在以 :ref:`Rect` 格式返回。

    .. method:: get_page_images(pno, full=False)

      仅限 PDF：返回页面直接或间接引用的所有图像的列表。

      :arg int pno: 页面编号，基于零，`-∞ < pno < page_count`。
      :arg bool full: 是否还包括引用者的 :data:`xref`（如果是页面，则为零）。

      :rtype: list

      :returns: 一个包含 **该页面引用的** 图像的列表。每项的格式如下：

          `(xref, smask, width, height, bpc, colorspace, alt_colorspace, name, filter, referencer)`

          其中：

            * **xref** (*int*) 是图像对象编号
            * **smask** (*int*) 是其软掩码图像的对象编号
            * **width** (*int*) 是图像宽度
            * **height** (*int*) 是图像高度
            * **bpc** (*int*) 表示每个组件的位数（通常为 8）
            * **colorspace** (*str*) 是表示颜色空间的字符串（如 **DeviceRGB**）
            * **alt_colorspace** (*str*) 是根据 **colorspace** 的值可能的任何替代颜色空间
            * **name** (*str*) 是图像的符号名称
            * **filter** (*str*) 是图像的解码过滤器（参考 :ref:`AdobeManual`，第22页）。
            * **referencer** (*int*) 是引用者的 :data:`xref`。如果由页面直接引用，则为零。只有在 *full=True* 时才会存在。

      .. note:: 
        
        一般来说，这不是 **实际显示的** 图像列表。此方法仅解析多个 PDF 对象，以收集对嵌入图像的引用。它并不分析页面的 :data:`contents`，其中定义了所有实际的图像显示命令。要获取此信息，请使用 :meth:`Page.get_image_info`。还请参阅 :ref:`textpagedict` 部分中的讨论。


    .. method:: get_page_fonts(pno, full=False)

      仅限 PDF：返回页面直接或间接引用的所有字体的列表。

      :arg int pno: 页面编号，基于零，`-∞ < pno < page_count`。
      :arg bool full: 是否还包括引用者的 :data:`xref`。如果 *True*，返回的每个条目将多一个项。使用此选项可以知道页面是否直接引用了该字体。如果是，最后一个条目为 0。如果字体是通过页面的 `/XObject` 引用的，您将在此看到其 :data:`xref`。

      :rtype: list

      :returns: 一个包含页面引用的字体的列表。每个条目格式如下：

      **(xref, ext, type, basefont, name, encoding, referencer)**，

      其中：

          * **xref** (*int*) 是字体对象编号（如果 PDF 直接使用内置字体，则可能为零）
          * **ext** (*str*) 字体文件扩展名（例如 "ttf"，参见 :ref:`FontExtensions`）
          * **type** (*str*) 字体类型（如 "Type1" 或 "TrueType" 等）
          * **basefont** (*str*) 基本字体名称，
          * **name** (*str*) 是引用字体的符号名称
          * **encoding** (*str*) 如果字体的字符编码不同于其内置编码，则为字体的字符编码（参考 :ref:`AdobeManual`，第254页）
          * **referencer** (*int* 可选) 引用者的 :data:`xref`。如果由页面直接引用，则为零，否则是 XObject 的 xref。仅在 *full=True* 时存在。

      示例::

          >>> pprint(doc.get_page_fonts(0, full=False))
          [(12, 'ttf', 'TrueType', 'FNUUTH+Calibri-Bold', 'R8', ''),
          (13, 'ttf', 'TrueType', 'DOKBTG+Calibri', 'R10', ''),
          (14, 'ttf', 'TrueType', 'NOHSJV+Calibri-Light', 'R12', ''),
          (15, 'ttf', 'TrueType', 'NZNDCL+CourierNewPSMT', 'R14', ''),
          (16, 'ttf', 'Type0', 'MNCSJY+SymbolMT', 'R17', 'Identity-H'),
          (17, 'cff', 'Type1', 'UAEUYH+Helvetica', 'R20', 'WinAnsiEncoding'),
          (18, 'ttf', 'Type0', 'ECPLRU+Calibri', 'R23', 'Identity-H'),
          (19, 'ttf', 'Type0', 'TONAYT+CourierNewPSMT', 'R27', 'Identity-H')]

      .. note::
          * 该列表没有重复项： :data:`xref` 、 *name* 和 *referencer* 的组合是唯一的。
          * 一般来说，这是页面实际使用字体的超集。PDF 创建者可能指定了一些全局字体列表，而每个页面只使用其中的一部分。

    .. method:: get_page_text(pno, output="text", flags=3, textpage=None, sort=False)

      提取给定页面编号 *pno* （基于零）的页面文本。调用 :meth:`Page.get_text`。

      :arg int pno: 页面编号，基于零，任何值 `-∞ < pno < page_count`。

      对于其他参数，请参阅页面方法。

      :rtype: str

    .. index::
      pair: fontsize; Document.layout
      pair: rect; Document.layout
      pair: width; Document.layout
      pair: height; Document.layout


    .. method:: layout(rect=None, width=0, height=0, fontsize=11)

      根据给定的页面尺寸和字体大小重新分页（“重新流动”）文档。这只影响一些文档类型，如电子书和 HTML。如果不支持则会被忽略。受支持的文档在属性 :attr:`is_reflowable` 中为 *True*。

      :arg rect_like rect: 所需的页面尺寸。必须是有限的，非空且起始点为 (0, 0)。
      :arg float width: 与 *height* 一起使用，作为 *rect* 的替代。
      :arg float height: 与 *width* 一起使用，作为 *rect* 的替代。
      :arg float fontsize: 所需的默认字体大小。


    .. method:: select(s)

      仅限 PDF：仅保留文档中页码出现在列表中的页面。空的序列或超出 `range(doc.page_count)` 的元素将引发 *ValueError*。有关更多详细信息，请参见本章节底部的备注。

      :arg sequence s: 要包含的页面编号序列（基于零）。不在序列中的页面将被删除（从内存中）并且直到重新打开文档才会变得可用。**页面编号可以多次出现且无特定顺序：** 结果文档将完全按照指定的序列反映。

      .. note::

          * 序列中的页面编号无需唯一或按特定顺序排列。这使得该方法成为一个多功能的工具，例如，只选择偶数页或奇数页，或满足其他标准等。

          * 从技术层面讲，该方法始终会创建一个新的 :data:`pagetree`。

          * 处理少量页面时，使用方法 :meth:`copy_page`、:meth:`move_page`、:meth:`delete_page` 会更容易。事实上，当文档包含很多页面时，它们的效率 **更高** ——至少提高一个数量级。


    .. method:: set_metadata(m)

      仅限 PDF：根据 *m* （一个 Python 字典）设置或更新文档的元数据。

      :arg dict m: 一个与 *metadata* 相同键的字典（见下文）。所有键都是可选的。PDF 的格式和加密方法不能设置或更改，并将被忽略。如果任何值不应包含数据，请不要指定其键或将值设置为 `None`。如果使用 *{}*，所有元数据将被清除并设置为字符串 *"none"*。如果您只想选择性地更改某些值，可以修改 *doc.metadata* 的副本并将其作为参数使用。可以指定任意的 UTF-8 编码的 Unicode 值。

      *(在 v1.18.4 中更改)* 现在空值或 "none" 不再写入，而是完全省略。

      
    .. method:: get_xml_metadata()

      仅限 PDF：获取文档的 XML 元数据。

      :rtype: str
      :returns: 文档的 XML 元数据。如果没有或不是 PDF，则返回空字符串。


    .. method:: set_xml_metadata(xml)

      仅限 PDF：设置或更新文档的 XML 元数据。

      :arg str xml: 新的 XML 元数据。应为 XML 语法，但此方法不进行检查，接受任何字符串。


    .. method:: set_pagelayout(value)

      * 新增于 v1.22.2

      仅限 PDF：设置 `/PageLayout`。

      :arg str value: 以下字符串之一 "SinglePage", "OneColumn", "TwoColumnLeft", "TwoColumnRight", "TwoPageLeft", "TwoPageRight"。支持小写。

      
    .. method:: set_pagemode(value)

      * 新增于 v1.22.2

      仅限 PDF：设置 `/PageMode`。

      :arg str value: 以下字符串之一 "UseNone", "UseOutlines", "UseThumbs", "FullScreen", "UseOC", "UseAttachments"。支持小写。

    .. method:: set_markinfo(value)

      * 新增于 v1.22.2

      仅限 PDF：设置 `/MarkInfo` 值。

      :arg dict value: 一个类似于 `{"Marked": False, "UserProperties": False, "Suspects": False}` 的字典。此字典包含有关 Tagged PDF 约定使用情况的信息。有关详细信息，请参见 `PDF 规范 <https://opensource.adobe.com/dc-acrobat-sdk-docs/standards/pdfstandards/pdf/PDF32000_2008.pdf>`_。


    .. method:: set_toc(toc, collapse=1)

      仅限 PDF：用提供的参数替换 **完整的当前大纲** 树（目录）。成功执行后，可以像平常一样通过 :meth:`Document.get_toc` 或 :attr:`Document.outline` 访问新的大纲树。与其他输出导向的方法一样，只有通过 :meth:`save` （支持增量保存）时，修改才会成为永久性。此方法内部包含以下两个步骤。示例如下所示。

      - 第 1 步：删除所有现有的书签。

      - 第 2 步：从 *toc* 中包含的条目创建新的目录。

      :arg sequence toc:

          一个包含 **所有书签条目** 的列表/元组，构成新的目录。可以接受 :meth:`get_toc` 的输出变体。如果要完全删除目录，指定一个空的序列或 `None`。每个条目必须是具有以下格式的列表。

          * [lvl, title, page [, dest]] 其中

            - **lvl** 是项的层级（int > 0）， **必须为 1** 对于第一个项，且最大为前一个项的层级加 1。

            - **title** （str）是要显示的标题。假定它是 UTF-8 编码的（仅对多字节字符集相关）。

            - **page** （int）是目标页面编号 **（注意：基于 1）**。如果是正数，则必须在有效范围内。如果没有目标或目标是外部的，则设置为 -1。

            - **dest** （可选）是字典或数字。如果是数字，它将被解释为此条目应指向页面的高度（以点为单位）。使用字典（例如 `get_toc(False)` 的输出）来详细控制书签的属性，详细信息见 :meth:`Document.get_toc`。

      :arg int collapse: *(新增于 v1.16.9)* 控制大纲条目初始显示折叠的层级。默认值为 1，这意味着仅显示第 1 层级，更高层级需要使用 PDF 查看器展开。要展开所有条目，指定一个较大的整数、0 或 `None`。

      :rtype: int
      :returns: 插入或删除的条目数量。

      在 v1.23.8 中更改：目标 'to' 坐标现在应与 `get_toc()` 返回的坐标系统相同（内部会使用 `page.cropbox` 和 `page.rotation_matrix` 进行转换）。因此，例如 `set_toc(get_toc())` 现在会返回未更改的目标 'to' 坐标。


    .. method:: outline_xref(idx)

      * 新增于 v1.17.7

      仅限 PDF：返回大纲项的 :data:`xref`。主要用于内部用途。

      :arg int idx: 在 :meth:`Document.get_toc` 返回的列表中的项的索引。

      :returns: :data:`xref`。


    .. method:: del_toc_item(idx)

      * 新增于 v1.17.7
      * 在 v1.18.14 中更改：不再移除该项的文本，而是将其显示为灰色。

      仅限 PDF：删除此目录项。这是一个高速方法，它 **禁用** 该项，但保持整体目录结构不变。物理上，该项仍存在于目录树中，但显示为灰色，并且不再指向任何目标。

      这也意味着，您可以在需要时通过 :meth:`Document.set_toc_item` 将该项重新分配到新的目标。

      :arg int idx: 在 :meth:`Document.get_toc` 返回的列表中的项的索引。


    .. method:: set_toc_item(idx, dest_dict=None, kind=None, pno=None, uri=None, title=None, to=None, filename=None, zoom=0)

      * 新增于 v1.17.7
      * 在 v1.18.6 中更改

      仅限 PDF：更改由索引标识的 TOC 项目。可以更改项目的 **标题**、 **目标**、 **外观** （颜色、加粗、斜体）或折叠子项，或者完全删除该项目。

      如果只需要对选定的条目进行特定修改，并且想避免替换整个目录，则使用此方法。特别是在处理大型目录时，这非常有用。

      :arg int idx: 在 :meth:`Document.get_toc` 创建的列表中的条目的索引。
      :arg dict dest_dict: 新的目标。一个字典，类似于 `doc.get_toc(False)` 中条目的最后一个条目。建议使用此作为模板。当给定时，**所有其他参数将被忽略**，仅保留标题。
      :arg int kind: 链接类型，参见 :ref:`linkDest Kinds`。如果是 :data:`LINK_NONE`，则所有其他参数将被忽略，目录项将被移除——与 :meth:`Document.del_toc_item` 相同。如果为 None，则仅修改标题，忽略其余参数。其他所有值将使用随后的参数创建一个新的目标字典。
      :arg int pno: 基于 1 的页面编号，即 1 <= pno <= doc.page_count。对于 LINK_GOTO 是必需的。
      :arg str uri: URL 文本。对于 LINK_URI 是必需的。
      :arg str title: 所需的新标题。如果不更改，则为 None。
      :arg point_like to: （可选）指向目标页面上的坐标。对于 LINK_GOTO 有效。如果省略，则选择页面顶部附近的一个点。
      :arg str filename: 对于 LINK_GOTOR 和 LINK_LAUNCH 是必需的。
      :arg float zoom: 显示目标页面时使用的缩放因子。

      **示例用法：** 更改 SWIG 手册的 TOC，如下所示：

      折叠所有低于顶级的内容，并将关于 Python 支持的章节显示为红色、加粗和斜体::

        >>> import pymupdf
        >>> doc=pymupdf.open("SWIGDocumentation.pdf")
        >>> toc = doc.get_toc(False)  # 我们需要详细的 TOC
        >>> # 获取第 1 层级的索引和标题列表
        >>> lvl1 = [(i, item[1]) for i, item in enumerate(toc) if item[0] == 1]
        >>> for i, title in lvl1:
                d = toc[i][3]  # 获取目标字典
                d["collapse"] = True  # 折叠其下的条目
                if "Python" in title:  # 显示 'Python' 章节
                    d["color"] = (1, 0, 0)  # 红色，
                    d["bold"] = True  # 加粗并且
                    d["italic"] = True  # 斜体
                doc.set_toc_item(i, dest_dict=d)  # 更新此 TOC 项
        >>> doc.save("NEWSWIG.pdf",garbage=3,deflate=True)

      在上面的示例中，我们仅更改了文件中 1240 个 TOC 条目中的 42 个。

    .. method:: bake(*, annots=True, widgets=True)

      仅限 PDF：将注释和/或小部件转换为页面的永久部分。此方法将 **更改** PDF 文件。如果 `widgets` 为 `True`，则文档将不再是 "表单 PDF"。

      所有页面看起来相同，但不再包含注释或字段。可见部分将根据需要转换为标准文本、矢量图形或图像。

      因此，该方法可能是使用 :meth:`Document.convert_to_pdf` 进行 PDF 到 PDF 转换的一个有效 **替代方案**。

      请考虑到注释是复杂的对象，可能包含其可视外观下方的更多数据。例如，"Text" 和 "FileAttachment" 注释。当将注释/小部件 "烘焙" 进 PDF 时，所有底层信息（附加文件、评论、相关的 PopUp 注释等）将丢失，并在下次垃圾回收时被移除。

      例如，当源页面应该在目标中完全相同但不支持注释或小部件时，可以使用此功能，例如在 :meth:`Page.show_pdf_page` 中。

      :arg bool annots: 是否转换注释。
      :arg bool widgets: 是否转换字段/小部件。执行后，文档将不再是 "表单 PDF"。

    .. method:: can_save_incrementally()

      * 新增于 v1.16.0

      检查文档是否可以增量保存。使用此方法可以在不遇到异常的情况下选择正确的选项。

    .. method:: scrub(attached_files=True, clean_pages=True, embedded_files=True, hidden_text=True, javascript=True, metadata=True, redactions=True, redact_images=0, remove_links=True, reset_fields=True, reset_responses=True, thumbnails=True, xml_metadata=True)

      * 新增于 v1.16.14

      仅限 PDF：从 PDF 中移除潜在的敏感数据。此功能灵感来自于 Adobe Acrobat 产品中的类似 "Sanitize" 功能。该过程可以通过多个选项进行配置。

      :arg bool attached_files: 搜索 'FileAttachment' 注释并移除文件内容。
      :arg bool clean_pages: 移除页面绘制源中的所有评论。如果此选项设置为 *False*，则也会对 *hidden_text* 和 *redactions* 进行清理。
      :arg bool embedded_files: 移除嵌入的文件。
      :arg bool hidden_text: 移除 OCR 识别的文本和不可见文本 [#f14]_。
      :arg bool javascript: 移除 JavaScript 源代码。
      :arg bool metadata: 移除 PDF 标准元数据。
      :arg bool redactions: 应用删除标记注释。
      :arg int redact_images: 处理图像时如何应用删除标记。可以选择 0（忽略）、1（遮盖重叠部分）或 2（移除）。
      :arg bool remove_links: 移除所有链接。
      :arg bool reset_fields: 重置所有表单字段为默认值。
      :arg bool reset_responses: 移除所有注释中的响应。
      :arg bool thumbnails: 移除页面的缩略图。
      :arg bool xml_metadata: 移除 XML 元数据。

    .. method:: save(outfile, garbage=0, clean=False, deflate=False, deflate_images=False, deflate_fonts=False, incremental=False, ascii=False, expand=0, linear=False, pretty=False, no_new_id=False, encryption=PDF_ENCRYPT_NONE, permissions=-1, owner_pw=None, user_pw=None, use_objstms=0)

      * 更改于 v1.18.7
      * 更改于 v1.19.0
      * 更改于 v1.24.1

      仅限 PDF：保存文档的 **当前状态**。

      :arg str, Path, fp outfile: 要保存的文件路径，`pathlib.Path` 或文件对象。文件对象必须通过 `open(...)` 或 `io.BytesIO()` 事先创建。选择 `io.BytesIO()` 类似于下面的 :meth:`Document.tobytes`，其等同于内部创建的 `io.BytesIO()` 的 `getvalue()` 输出。

      :arg int garbage: 执行垃圾回收。正值将排除 "增量" 保存。

        * 0 = 无
        * 1 = 移除未使用（未引用）对象。
        * 2 = 在执行第 1 步的基础上，压缩 :data:`xref` 表。
        * 3 = 在执行第 2 步的基础上，合并重复对象。
        * 4 = 在执行第 3 步的基础上，检查 :data:`stream` 对象的重复性。这可能较慢，因为这些数据通常较大。

      :arg bool clean: 清理并消毒内容流 [#f8]_。对应于 "mutool clean -sc"。

      :arg bool deflate: 压缩（deflate）未压缩的流。
      :arg bool deflate_images: *(v1.18.3 新增)* 压缩（deflate）未压缩的图像流 [#f11]_。
      :arg bool deflate_fonts: *(v1.18.3 新增)* 压缩（deflate）未压缩的字体文件流 [#f11]_。

      :arg bool incremental: 仅保存对 PDF 的更改。排除 "garbage" 和 "linear"。此选项仅在 *outfile* 为字符串或 `pathlib.Path` 且等于 :attr:`Document.name` 时可用。对于解密或修复后的文件以及其他某些情况，不能使用此选项。为了确保，请检查 :meth:`Document.can_save_incrementally`。如果返回 false，则需要保存到新文件。

      :arg bool ascii: 将二进制数据转换为 ASCII。

      :arg int expand: 解压对象。生成可以被其他程序更好读取的版本，但文件可能会更大。

        * 0 = 无
        * 1 = 图像
        * 2 = 字体
        * 255 = 所有

      :arg bool linear: 保存线性化版本的文档。此选项为改进互联网访问性能创建文件格式。排除 "incremental" 和 "use_objstms"。

      :arg bool pretty: 美化文档源，增加可读性。PDF 对象将重新格式化，以便更像 :meth:`Document.xref_object` 的默认输出。

      :arg bool no_new_id: 抑制更新文件的 `/ID` 字段。如果文件根本没有该字段，也抑制创建新字段。默认为 `False`，即每次保存都会导致文件标识的更新。

      :arg int permissions: *(v1.16.0 新增)* 设置所需的权限级别。请参阅 :ref:`PermissionCodes` 了解可能的值。默认是授予所有权限。

      :arg int encryption: *(v1.16.0 新增)* 设置所需的加密方法。请参阅 :ref:`EncryptionMethods` 了解可能的值。

      :arg str owner_pw: *(v1.16.0 新增)* 设置文档的所有者密码。*(v1.18.3 更改)* 如果未提供，则使用用户密码（如果提供）。字符串长度不得超过 40 个字符。

      :arg str user_pw: *(v1.16.0 新增)* 设置文档的用户密码。字符串长度不得超过 40 个字符。

      :arg int use_objstms: *(v1.24.0 新增)* 压缩选项，将合适的 PDF 对象定义转换为存储在其他对象的 :data:`stream` 数据中。根据 `deflate` 参数的值，转换后的对象定义将被压缩——这可以显著减少文件大小。

      .. warning:: 该方法不会检查文件是否已经存在，因此不会提示确认，并会覆盖文件。作为程序员，你需要负责处理这个问题。

      .. note::

        **文件大小减少**

        1. 使用保存选项如 `garbage=3|4, deflate=True, use_objstms=1`。不要更改默认值 `expand=False|0, clean=False|0, incremental=False|0, linear=False|0`。
        这是一种 "无损" 的文件大小减小。该方法有一个便利版本，默认设置了这些值： :meth:`Document.ez_save`，请参阅下面。

        2. "有损" 文件大小减小本质上必须放弃某些图像方面的内容，例如 (a) 移除所有图像 (b) 将图像替换为其灰度版本 (c) 降低图像分辨率。可以在 `PyMuPDF Utilities "replace-image" 文件夹 <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/examples/replace-image>`_ 找到示例。

    .. method:: ez_save(*args, **kwargs)

      * 新增于 v1.18.11

      仅限 PDF：与 :meth:`Document.save` 相同，但默认选项为 `deflate=True, garbage=3, use_objstms=1`。

    .. method:: saveIncr()

      PDF only: saves the document incrementally. This is a convenience abbreviation for *doc.save(doc.name, incremental=True, encryption=PDF_ENCRYPT_KEEP)*.

    .. note::

        如果文档包含经过验证的签名，保存为新文件可能会导致签名无效，此时可能需要增量保存。

    .. method:: tobytes(garbage=0, clean=False, deflate=False, deflate_images=False, deflate_fonts=False, ascii=False, expand=0, linear=False, pretty=False, no_new_id=False, encryption=PDF_ENCRYPT_NONE, permissions=-1, owner_pw=None, user_pw=None, use_objstms=0)

      * 更改于 v1.18.7
      * 更改于 v1.19.0
      * 更改于 v1.24.1

      仅限 PDF：将 **文档的当前内容** 写入字节对象，而不是文件。显然，您应该注意内存要求。参数的含义与 :meth:`save` 中的完全相同。章节 :ref:`FAQ` 包含一个示例，演示如何将此方法用作 `pdfrw <https://pypi.python.org/pypi/pdfrw/0.3>`_ 的预处理器。

      *(v1.16.0 更改)* 增强的加密支持。

      :rtype: bytes
      :returns: 包含完整文档的字节对象。

    .. method:: search_page_for(pno, text, quads=False)

      在页面编号 "pno" 上搜索 "text"。与对应的 :meth:`Page.search_for` 完全相同。任何整数 `-∞ < pno < page_count` 都是可接受的。

    .. index::
      pair: append; Document.insert_pdf
      pair: join; Document.insert_pdf
      pair: merge; Document.insert_pdf
      pair: from_page; Document.insert_pdf
      pair: to_page; Document.insert_pdf
      pair: start_at; Document.insert_pdf
      pair: rotate; Document.insert_pdf
      pair: links; Document.insert_pdf
      pair: annots; Document.insert_pdf
      pair: widgets; Document.insert_pdf
      pair: join_duplicates; Document.insert_pdf
      pair: show_progress; Document.insert_pdf

    .. method:: insert_pdf(docsrc, from_page=-1, to_page=-1, start_at=-1, rotate=-1, links=True, annots=True, widgets=True, join_duplicates=False, show_progress=0, final=1)

      仅限 PDF：将 PDF 文档 *docsrc* 的页面范围 **[from_page, to_page]** （包括两者）复制到当前文档中。插入将从页面编号 *start_at* 开始。值为 -1 表示使用默认值。所有复制的页面将按照指定的旋转角度进行旋转。链接、注释和小部件可以在目标中排除，具体如下。所有页面编号为 0 基础。

      :arg docsrc: 已打开的 PDF *Document*，必须与当前文档不同，但可以引用相同的底层文件。
      :type docsrc: *Document*

      :arg int from_page: *docsrc* 中的第一页编号。默认值为零。

      :arg int to_page: *docsrc* 中的最后一页编号。默认为最后一页。

      :arg int start_at: 第一个复制的页面，将在目标中成为页面编号 *start_at*。默认 -1 表示将页面范围追加到最后。如果为零，则页面范围将插入到当前第一页之前。

      :arg int rotate: 所有复制的页面将按提供的值旋转（度数，90 的整数倍）。

      :arg bool links: 选择是否应包括复制中的（内部和外部）链接。默认值为 `True`。 *命名* 链接 ( :data:`LINK_NAMED` ) 和指向复制页面范围之外的内部链接 **始终会被排除**。

      :arg bool annots: 选择是否应包括复制中的注释。

      :arg bool widgets: 选择是否应包括复制中的注释。如果为 `True` 且源页面中至少有一个表单字段，则目标 PDF 将变成一个表单 PDF（如果尚未是）。

      :arg bool join_duplicates: *(新功能，版本 1.25.5)* 选择如何处理源页面中的重复根字段名称。如果 `widgets=False`，则此参数被忽略。

        默认值为 ``False``，这将为目标中已存在的重复源根字段添加统一的字符串。例如，如果 "name" 已经存在于目标中，源小部件的名称将更改为 "name [text]"，并附加适当选择的字符串 "text"。

        如果 ``True``，源和目标中重复名称的根字段将被转换为 "父" 对象的 "Kids"（即列表中的所有子小部件）。这会有效地将这些小部件变成 "相同" 小部件的实例：例如，如果其中一个小部件被更改，则所有实例都会自动继承该更改，无论它们显示在哪一页。

      :arg int show_progress: *(新功能，版本 1.17.7)* 指定大于零的间隔大小，以便在 `sys.stdout` 上查看进度消息。每个间隔后，将打印类似 "Inserted 30 of 47 pages." 的消息。

      :arg int final: *(新功能，版本 1.18.0)* 控制是否在此方法后 **丢弃** 已复制对象的列表，默认为 *True*。除了从同一源 PDF 进行的多次插入的最后一次外，将其设置为 0 以节省目标文件的大小并显著加快执行速度。

    .. note::

      1. 这是一个基于页面的方法。因此，源文档的文档级信息大多数将被忽略。例如，包括可选内容、嵌入文件、`StructureElem`、目录、页面标签、元数据、命名目标（和其他命名条目）等。

      2. 如果 `from_page > to_page`，页面将 **按相反顺序复制**。如果 `0 <= from_page == to_page`，则只复制一页。

      3. `docsrc` 的 TOC 条目 **不会被复制**。但是，恢复结果文档的目录是很容易的。请查看下面的示例以及程序 `join.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/join-documents/join.py>`_，它可以同时合并 PDF 文档并拼接相应部分的目录。

    .. index::
      pair: append; Document.insert_file
      pair: join; Document.insert_file
      pair: merge; Document.insert_file
      pair: from_page; Document.insert_file
      pair: to_page; Document.insert_file
      pair: start_at; Document.insert_file
      pair: rotate; Document.insert_file
      pair: links; Document.insert_file
      pair: annots; Document.insert_file
      pair: show_progress; Document.insert_file

    .. method:: insert_file(infile, from_page=-1, to_page=-1, start_at=-1, rotate=-1, links=True, annots=True, show_progress=0, final=1)

      * 新功能，版本 v1.22.0

      仅限 PDF：将任意受支持的文档添加到当前 PDF 中。打开 "infile" 作为文档，将其转换为 PDF 然后调用 :meth:`Document.insert_pdf` 方法。参数与该方法相同。此功能提供了一种简单的方法，可以将图像作为完整页面附加到输出 PDF 中。

      :arg multiple infile: 要插入的输入文档。可以是创建 :ref:`Document` 或 :ref:`Pixmap` 时有效的文件名规格。

    .. index::
      pair: width; Document.new_page
      pair: height; Document.new_page

    .. method:: new_page(pno=-1, width=595, height=842)

      仅限 PDF：插入一个空白页面。

      :arg int pno: 新页面应插入的页面编号。必须在 *1 < pno <= page_count* 范围内。特殊值 -1 和 *doc.page_count* 表示插入到最后一页之后。

      :arg float width: 页面宽度。
      :arg float height: 页面高度。

      :rtype: :ref:`Page`
      :returns: 创建的页面对象。

    .. index::
      pair: fontsize; Document.insert_page
      pair: width; Document.insert_page
      pair: height; Document.insert_page
      pair: fontname; Document.insert_page
      pair: fontfile; Document.insert_page
      pair: color; Document.insert_page

    .. method:: insert_page(pno, text=None, fontsize=11, width=595, height=842, fontname="helv", fontfile=None, color=None)

      仅限 PDF：插入一个新页面并插入一些文本。此便捷函数结合了 :meth:`Document.new_page` 和（部分） :meth:`Page.insert_text` 方法。

      :arg int pno: 页面编号（0 基数），表示新页面插入的位置。必须在 `range(-1, doc.page_count + 1)` 范围内。特殊值 -1 和 `doc.page_count` 表示插入到最后一页之后。

          变更于 v1.14.12
            现在是一个位置参数

      对于其他参数，请参阅上述方法。

      :rtype: int
      :returns: :meth:`Page.insert_text` 的结果（成功插入的行数）。

    .. method:: delete_page(pno=-1)

      仅限 PDF：删除指定的页面，页面编号为 `-∞ < pno < page_count - 1`。

      * 变更于 v1.18.14：支持 Python 的 `del` 语句。

      :arg int pno: 要删除的页面。负数从文档末尾开始计数（与索引类似）。默认值为最后一页。

    .. method:: delete_pages(*args, **kwds)

      * 变更于 v1.18.13：更灵活地指定要删除的页面。
      * 变更于 v1.18.14：支持 Python 的 `del` 语句。

      仅限 PDF：删除多个页面，给定 0 基数的页面编号。

      **格式 1**：使用关键字参数。表示旧格式。删除一个连续的页面范围。
        * "from_page"：要删除的第一页。如果省略，则为零。
        * "to_page"：要删除的最后一页。如果省略，则为文档的最后一页。不能小于 "from_page"。

      **格式 2**：两个页面编号作为位置参数。与格式 1 相同处理。

      **格式 3**：一个位置整数参数。等同于 :meth:`Page.delete_page`。

      **格式 4**：一个位置参数，类型为 *list*、*tuple* 或 *range()*，包含要删除的页面编号。该序列中的项目可以按任何顺序排列，并且可以包含重复项。

      **格式 5**：*(新功能，v1.18.14)* 现在可以使用 Python 的 `del` 语句和索引/切片表示法。

      .. note::

        *(变更于 v1.14.17，优化于 v1.17.7)* 为了保持有效的 PDF 结构，此方法和 :meth:`delete_page` 还会停用指向已删除页面的目录项。此处的“停用”意味着书签将不指向任何位置，并且支持的 PDF 查看器将显示为灰色标题。总体目录结构保持不变。

        它还会移除所有指向已删除页面的**链接**，此操作可能对页面较多的文档有较长的响应时间。

        以下示例将删除第 500 页至第 519 页：

        * `doc.delete_pages(500, 519)`
        * `doc.delete_pages(from_page=500, to_page=519)`
        * `doc.delete_pages((500, 501, 502, ... , 519))`
        * `doc.delete_pages(range(500, 520))`
        * `del doc[500:520]`
        * `del doc[(500, 501, 502, ... , 519)]`
        * `del doc[range(500, 520)]`

        对于 :ref:`AdobeManual`，上述操作大约需要 0.6 秒，因为剩余的 1290 页必须清除无效链接。

        一般来说，此方法的性能依赖于剩余页面的数量，而不是删除页面的数量：例如，**删除所有页面**，只保留那 20 页，将需要更少的时间。

    .. method:: copy_page(pno, to=-1)

      仅限 PDF：在文档中复制页面引用。

      :arg int pno: 要复制的页面。必须在 `0 <= pno < page_count` 范围内。

      :arg int to: 要复制到的页面编号。默认将插入到最后一页之后。

      .. note:: 仅会创建页面对象的 **新引用** ，而不是新的页面对象，所有复制的页面将具有相同的属性值，包括 :attr:`Page.xref`。这意味着对其中一个副本的任何更改将出现在所有副本中。

    .. method:: fullcopy_page(pno, to=-1)

      * 新功能，v1.14.17

      仅限 PDF：完全复制（重复）一个页面。

      :arg int pno: 要复制的页面。必须在 `0 <= pno < page_count` 范围内。

      :arg int to: 要复制到的页面编号。默认将插入到最后一页之后。

      .. note::

          * 与 :meth:`copy_page` 不同，此方法创建一个新的页面对象（带有新的 :data:`xref`），可以独立于原始页面进行更改。

          * 任何弹出窗口和 "IRT"（“响应于”）注释将 **不被复制** ，以避免出现潜在的不正确情况。

    .. method:: move_page(pno, to=-1)

      仅限 PDF：在文档中移动（复制并删除原始页面）一个页面。

      :arg int pno: 要移动的页面。必须在 `0 <= pno < page_count` 范围内。

      :arg int to: 要插入的页面编号。默认将移动到最后一页之后。

    .. method:: need_appearances(value=None)

      * 新功能，v1.17.4

      仅限 PDF：获取或设置表单 PDF 的 */NeedAppearances* 属性。引用：“（可选）一个标志，指定是否为文档中的所有小部件注释构建外观流和外观字典... 默认值：false。” 这可能有助于控制某些读取器/查看器的行为。

      :arg bool value: 设置此属性的值。如果省略或为 `None`，则查询当前值。

      :rtype: bool
      :returns:
        * None: 不是表单 PDF，或未定义该属性。
        * True / False: 属性的值（无论是刚设置的还是查询时已有的）。如果不是表单 PDF，则无效。

    .. method:: get_sigflags()

      仅限 PDF：返回文档是否包含签名字段。这是一个可选的 PDF 属性：如果不存在（返回值为 -1），则无法得出结论——PDF 创建者可能只是没有使用它。

      :rtype: int
      :returns:
        * -1: 不是表单 PDF / 没有记录的签名字段 / 未找到 *SigFlags*。
        * 1: 至少存在一个签名字段。
        * 3: 包含可能会被使无效的签名，如果文件以一种修改其先前内容的方式保存（写入），而不是增量更新。

    .. method:: embfile_add(name, buffer, filename=None, ufilename=None, desc=None)

      * 变更于 v1.14.16：位置参数“name”和“buffer”的顺序已更改，以符合其他函数的调用模式。

      仅限 PDF：嵌入一个新文件。所有字符串参数（除了名称）都可以是 Unicode（在以前的版本中，只有 ASCII 正常工作）。文件内容将在有益的情况下进行压缩。

      :arg str name: 条目标识符，**不能已经存在**。
      :arg bytes,bytearray,BytesIO buffer: 文件内容。

        *(变更于 v1.14.13)* 现在也支持 *io.BytesIO*。

      :arg str filename: 可选文件名。仅文档说明，如果为 `None`，则设置为 *name*。
      :arg str ufilename: 可选的 Unicode 文件名。仅文档说明，如果为 `None`，则设置为 *filename*。
      :arg str desc: 可选的描述。仅文档说明，如果为 `None`，则设置为 *name*。

      :rtype: int
      :returns: *(变更于 v1.18.13)* 该方法现在返回插入文件的 :data:`xref`。此外，文件对象将自动赋予 PDF 键 `/CreationDate` 和 `/ModDate`，基于当前日期和时间。

    .. method:: embfile_count()

      * 变更于 v1.14.16：现在这是一个方法。在以前的版本中，这是一个属性。

      仅限 PDF：返回嵌入文件的数量。

    .. method:: embfile_get(item)

      仅限 PDF：通过条目编号或名称检索嵌入文件的内容。如果文档不是 PDF，或无法找到条目，则会引发异常。

      :arg int,str item: 条目的索引或名称。整数必须在 `range(embfile_count())` 范围内。

      :rtype: bytes

    .. method:: embfile_del(item)

      * 变更于 v1.14.16：现在也可以通过索引删除条目。

      仅限 PDF：从 `/EmbeddedFiles` 中删除一个条目。与往常一样，只有在将文档保存为新文件并使用适当的垃圾回收选项时，才会实际删除嵌入的文件内容（并恢复文件空间）。

      :arg int/str item: 条目的索引或名称。

      .. warning:: 
        
        当指定条目名称时，此函数将仅 **删除第一个** 具有该名称的条目。请注意，未使用 PyMuPDF 创建的 PDF 可能包含重复的名称。因此，您可能需要采取适当的预防措施。

    .. method:: embfile_info(item)

      * 变更于 v1.18.13

      仅限 PDF：通过条目的编号或名称检索嵌入文件的信息。

      :arg int/str item: 条目的索引或名称。整数必须在 `range(embfile_count())` 范围内。

      :rtype: dict
      :returns: 包含以下键的字典：

          * ``name`` -- (*str*) 存储此条目的名称
          * ``filename`` -- (*str*) 文件名
          * ``ufilename`` -- (*unicode*) 文件名
          * ``description`` -- (*str*) 描述
          * ``size`` -- (*int*) 原始文件大小
          * ``length`` -- (*int*) 压缩后的文件长度
          * ``creationDate`` -- (*str*) 项目创建日期时间，PDF 格式
          * ``modDate`` -- (*str*) 最后修改日期时间，PDF 格式
          * ``collection`` -- (*int*) 关联的 PDF 组合项的 :data:`xref`，如果有的话，否则为零。
          * ``checksum`` -- (*str*) 存储文件内容的哈希码，作为十六进制字符串。根据 PDF 规范，它应该是 MD5，但可以看到其他哈希算法。

    .. method:: embfile_names()

      仅限 PDF：返回嵌入文件名称的列表。名称的顺序等于文档中物理顺序。

      :rtype: list

    .. method:: embfile_upd(item, buffer=None, filename=None, ufilename=None, desc=None)

      仅限 PDF：根据条目的编号或名称更改嵌入的文件。所有参数都是可选的，默认值会导致无操作。

      :arg int/str item: 条目的索引或名称。整数必须在 `range(embfile_count())` 范围内。
      :arg bytes,bytearray,BytesIO buffer: 新的文件内容。

        *(变更于 v1.14.13)* 现在也支持 *io.BytesIO*。

      :arg str filename: 新的文件名。
      :arg str ufilename: 新的 Unicode 文件名。
      :arg str desc: 新的描述。

      *(变更于 v1.18.13)* 该方法现在返回文件对象的 :data:`xref`。

      :rtype: int
      :returns: 文件对象的 xref。自动地，PDF 键 `/ModDate` 将会更新为当前日期时间。

    .. method:: close()

      释放与文档相关的对象和空间分配。如果是从文件创建的，也会关闭 *filename* （将控制权交还给操作系统）。显式关闭文档等同于删除它，`del doc`，或将其赋值为其他对象，如 `doc = None`。

    .. method:: xref_object(xref, compressed=False, ascii=False)

      * 新功能，v1.16.8
      * 变更于 v1.18.10

      仅限 PDF：返回 PDF 对象的定义源。

      :arg int xref: 对象的 :data:`xref`。*变更于 v1.18.10:* 值为 `-1` 时返回 PDF trailer 源。
      :arg bool compressed: 是否生成没有换行符或空格的紧凑输出。
      :arg bool ascii: 是否将二进制数据进行 ASCII 编码。

      :rtype: str
      :returns: 对象的定义源。

    .. method:: pdf_catalog()

      * 新功能，v1.16.8

      仅限 PDF：返回 PDF 目录（或根）对象的 :data:`xref` 编号。使用该编号与 :meth:`Document.xref_object` 来查看其源。

    .. method:: pdf_trailer(compressed=False)

      * 新功能，v1.16.8

      仅限 PDF：返回 PDF 的 trailer 源，通常位于 PDF 文件的末尾。这相当于 :meth:`Document.xref_object`，并且 *xref* 参数为 -1。

    .. method:: xref_stream(xref)

      * 新功能，v1.16.8

      仅限 PDF：返回 :data:`xref` 流对象的 **解压缩** 内容。

      :arg int xref: :data:`xref` 编号。

      :rtype: bytes
      :returns: 对象的（解压缩的）流内容。

    .. method:: xref_stream_raw(xref)

      * 新功能，v1.16.8

      仅限 PDF：返回 :data:`xref` 流对象的 **未修改** （尤其是 **未解压缩**）内容。与 :meth:`Document.xref_stream` 基本相同。

      :rtype: bytes
      :returns: 对象的（原始，未修改的）流内容。

    .. method:: update_object(xref, obj_str, page=None)

      * 新功能，v1.16.8

      仅限 PDF：用提供的字符串替换 :data:`xref` 对象的定义。该 xref 也可以是新的，在这种情况下，此指令将完成对象定义。如果提供了页面对象，页面的链接和注释将在之后重新加载。

      :arg int xref: :data:`xref` 编号。

      :arg str obj_str: 包含有效 PDF 对象定义的字符串。

      :arg page: 页面对象。如果提供，将指示该页面的注释应该刷新（重新加载），以反映链接和/或注释所做的更改。
      :type page: :ref:`Page`

      :rtype: int
      :returns: 如果成功则返回零，否则将引发异常。

    .. method:: update_stream(xref, data, new=False, compress=True)

      * 新功能，v1.16.8
      * 变更于 v1.19.2：添加了参数 "compress"
      * 变更于 v1.19.6：弃用参数 "new"。现在确认对象是一个 PDF 字典对象。

      替换由 *xref* 标识的对象的流，该对象必须是 PDF 字典。如果该对象不是 :data:`stream`，它将被转换为流。该函数会自动执行压缩操作（"deflate"），如果有益的话。

      :arg int xref: :data:`xref` 编号。

      :arg bytes|bytearray|BytesIO stream: 流的新内容。

        *(变更于 v1.14.13:)* 现在也支持 *io.BytesIO* 对象。

      :arg bool new: *弃用*，将被忽略。将在 v1.20.0 之后移除。
      :arg bool compress: 是否压缩插入的流。如果 `True` （默认），则流将使用 `/FlateDecode` 压缩（如果有益），否则流将按原样插入。

      :raises ValueError: 如果 *xref* 不代表 PDF :data:`dict`。空字典 ``<<>>`` 是被接受的。所以，如果您刚创建了 xref 并想给它一个流，请先执行 `doc.update_object(xref, "<<>>")`，然后使用此方法插入流数据。

      此方法主要（但不完全）用于操作包含 PDF 操作符语法的流（请参阅 :ref:`AdobeManual` 第 643 页），例如页面内容流。

      如果更新内容流，请考虑使用保存参数 *clean=True* 来确保 PDF 操作符源和对象结构之间的一致性。

      示例：假设您不再希望页面上出现某个图像。这可以通过删除相应内容源中的引用来实现——事实上：图像在重新加载页面后将消失。但页面的 :data:`resources` 对象仍会显示该图像被页面引用。此保存选项将清理任何此类不匹配。

    .. method:: Document.xref_copy(source, target, *, keep=None)

      * 新功能，v1.19.5

      仅限 PDF：使 *target* xref 成为 *source* 的精确副本。如果 *source* 是 :data:`stream`，那么这些数据也会被复制。

      :arg int source: 源 :data:`xref`。必须是现有的 **字典** 对象。
      :arg int target: 目标 xref。必须是现有的 **字典** 对象。如果该 xref 刚刚被创建，请确保将其初始化为具有最小规格的 PDF 字典 ``<<>>``。
      :arg list keep: 可选的 *target* 顶级键的列表，这些键在准备复制过程中不应被删除。

      .. note::

          * 此方法与 Python 的 *dict* 方法 `copy()` 非常相似。
          * 两个 xref 编号必须代表现有的字典。
          * 在从 *source* 复制数据之前，所有 *target* 字典键都会被删除。您可以在 *keep* 列表中指定例外。如果 *source* 具有相同名称的键，其值仍会替换目标。
          * 如果 *source* 是 :data:`stream` 对象，那么这些数据也会被复制，并且 *target* 将被转换为流对象。
          * 一个典型的用例是替换或移除现有图像，而不使用删除注释。示例脚本可以在 `这里 <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/examples/replace-image>`_ 中看到。

    .. method:: Document.extract_image(xref)

      仅限 PDF：提取存储在文档中的图像数据和元信息。输出可以直接用于存储为图像文件、作为 PIL 输入、创建 :ref:`Pixmap` 等。此方法尽可能避免使用 pixmap 来呈现图像，以其原始格式（例如 JPEG）展示图像。

      :arg int xref: 图像对象的 :data:`xref`。如果不在 `range(1, doc.xref_length())` 范围内，或该对象不是图像或发生其他错误，则返回 `None`，且不会引发异常。

      :rtype: dict
      :returns: 包含以下键的字典：

        * *ext* (*str*) 图像类型（例如 *'jpeg'*），可用作图像文件扩展名。
        * *smask* (*int*) 蒙版 (/SMask) 图像的 :data:`xref` 编号，或为零。
        * *width* (*int*) 图像宽度。
        * *height* (*int*) 图像高度。
        * *colorspace* (*int*) 图像的 *colorspace.n* 编号。
        * *cs-name* (*str*) 图像的 *colorspace.name*。
        * *xres* (*int*) x 方向的分辨率。请参见 :data:`resolution`。
        * *yres* (*int*) y 方向的分辨率。请参见 :data:`resolution`。
        * *image* (*bytes*) 图像数据，可作为图像文件内容使用。

      >>> d = doc.extract_image(1373)
      >>> d
      {'ext': 'png', 'smask': 2934, 'width': 5, 'height': 629, 'colorspace': 3, 'xres': 96,
      'yres': 96, 'cs-name': 'DeviceRGB',
      'image': b'\x89PNG\r\n\x1a\n\x00\x00\x00\rIHDR\x00\x00\x00\x05\ ...'}
      >>> imgout = open(f"image.{d['ext']}", "wb")
      >>> imgout.write(d["image"])
      102
      >>> imgout.close()

      .. note:: 
        
        此方法与 *pix = pymupdf.Pixmap(doc, xref)*，然后调用 *pix.tobytes()* 有功能重叠。主要区别在于，`extract_image` **(1)** 不总是提供 PNG 图像格式， **(2)** 对非 PNG 图像 **非常** 快速，**(3)** 通常会导致提取的图像占用更少的磁盘存储，**(4)** 在错误情况下返回 `None` （不会生成异常）。以下是同一 PDF 中的示例图像。

        * xref 1268 是 PNG -- 可比的执行时间和相同的输出::

            In [23]: %timeit pix = pymupdf.Pixmap(doc, 1268);pix.tobytes()
            10.8 ms ± 52.4 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
            In [24]: len(pix.tobytes())
            Out[24]: 21462

            In [25]: %timeit img = doc.extract_image(1268)
            10.8 ms ± 86 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
            In [26]: len(img["image"])
            Out[26]: 21462

        * xref 1186 是 JPEG -- :meth:`Document.extract_image` **速度更快**，并生成 **更小的** 输出（2.48 MB vs. 0.35 MB）::

            In [27]: %timeit pix = pymupdf.Pixmap(doc, 1186);pix.tobytes()
            341 ms ± 2.86 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
            In [28]: len(pix.tobytes())
            Out[28]: 2599433

            In [29]: %timeit img = doc.extract_image(1186)
            15.7 µs ± 116 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
            In [30]: len(img["image"])
            Out[30]: 371177

    .. method:: Document.extract_font(xref, info_only=False, named=None)

      * v1.19.4 变更：如果 `named == True`，返回一个字典。

      仅限 PDF：返回嵌入字体文件的数据和相应的文件扩展名。可用于将字体存储为外部文件。该方法不会引发异常（除了检查 PDF 和有效的 :data:`xref`）。

      :arg int xref: 要提取的字体的 PDF 对象编号。
      :arg bool info_only: 仅返回字体信息，不返回缓冲区。用于仅获取信息的目的，避免分配大型缓冲区区域。
      :arg bool named: 如果为 True，返回包含以下键的字典：'name'（字体基本名称），'ext'（字体文件扩展名），'type'（字体类型），'content'（字体文件内容）。

      :rtype: tuple,dict
      :returns: 一个元组 `(basename, ext, type, content)`，其中 *ext* 是 3 字节的建议文件扩展名 (*str*)，*basename* 是字体的名称 (*str*)，*type* 是字体的类型（例如 "Type1"），*content* 是包含字体文件内容的字节对象（或 *b""*）。对于可能的扩展值及其含义，请参阅 :ref:`FontExtensions`。错误时返回详细信息：

            * `("", "", "", b"")` -- 无效的 xref 或 xref 不是（有效的）字体对象。
            * `(basename, "n/a", "Type1", b"")` -- *basename* 未嵌入，因此无法提取。这适用于例如 :ref:`Base-14-Fonts` 和 Type 3 字体。

      示例：

      >>> # 将字体存储为外部文件
      >>> name, ext, _, content = doc.extract_font(4711)
      >>> # 假设 content 不是 None：
      >>> ofile = open(name + "." + ext, "wb")
      >>> ofile.write(content)
      >>> ofile.close()

      .. warning:: `basename` 会保持从 PDF 返回，可能包含一些字符（如空格），这些字符可能不适合您的操作系统的文件名。请采取适当的措施。

      .. note::
        * 返回的 *basename* 通常 **不是** 原始文件名，但可能与之有某些相似之处。
        * 如果参数 `named == True`，则返回一个包含以下键的字典：`{'name': 'T1', 'ext': 'n/a', 'type': 'Type3', 'content': b''}`。

    .. method:: xref_xml_metadata()

      * v1.16.8 新增

      仅限 PDF：返回文档 XML 元数据的 :data:`xref`。


    .. method:: has_links()

    .. method:: has_annots()

      * v1.18.7 新增

      仅限 PDF：检查文档中是否存在链接或注释。

      :returns: *True* / *False*。与字段不同，字段存储在 PDF 文档的中央位置，而链接或注释的存在只能通过解析每个页面来检测。这些方法经过优化，可高效执行，并且如果在某个页面找到 *True*，则会立即返回。但如果 PDF 包含数千页，并且未找到链接或注释，则可能需要较长时间 [#f13]_ 。


    .. method:: subset_fonts(verbose=False, fallback=False)

      仅限 PDF：分析文档中的字体是否可用于文本，并检查是否可以缩小字体大小。如果字体支持且可以减少大小，则会用仅包含部分字符的字体版本替换原字体。

      请在保存文档之前立即使用此方法。

      :arg bool verbose: 是否向标准输出打印进度信息。目前仅在 `fallback` 为 `True` 时有效。
      :arg bool fallback: 是否使用已弃用的算法，该算法依赖于 `fontTools <https://pypi.org/project/fonttools/>`_ （必须安装此包）。如果使用推荐值 `False` （默认），则会使用 MuPDF 的原生函数 —— **速度快得多** ，并且支持更多种类的字体。在此情况下，无需安装 `fontTools`。

      在创建包含大字体（如亚洲字符集）的新 PDF 时，此方法的优化效果最明显。例如，使用 :ref:`Story` 类或方法 :meth:`Page.insert_htmlbox` 时，可能会自动包含多个字体，而程序员对此可能并不知情。

      在这些情况下，实际使用的 Unicode 字符通常远少于字体可用的字形数量。使用此方法可以轻松减少嵌入字体的大小，从几 MB 级别减少到几十 KB。

      创建字体子集后，PDF 可能会遗留大量未使用的 PDF 对象（“幽灵对象”）。因此，在保存文件时，请务必进行压缩和垃圾回收。建议使用 :meth:`Document.ez_save`。

      |history_begin|

      * v1.18.7 新增
      * v1.18.9 变更
      * v1.24.2 变更，改为使用 MuPDF 的原生函数。

      |history_end|


    .. method:: journal_enable()

      * v1.19.0 新增

      仅限 PDF：启用日志记录。在开始记录操作之前，请先调用此方法。


    .. method:: journal_start_op(name)

      * v1.19.0 新增

      仅限 PDF：启动一个日志操作，并使用字符串 `"name"` 作为标识。对于启用了日志记录的 PDF，如果未启动操作，则更新将失败。


    .. method:: journal_stop_op()

      * v1.19.0 新增

      仅限 PDF：停止当前操作。从开始到停止之间的所有更新都属于同一个工作单元，并将在撤销/重做时一同应用。


    .. method:: journal_position()

      * v1.19.0 新增

      仅限 PDF：返回当前操作编号及总操作数。

      :returns: 一个元组 `(step, steps)`，其中 **step** 是当前操作编号，**steps** 是日志中的总操作数。如果 **step** 为 0，表示当前位于日志顶部；如果 **step** 等于 **steps**，表示当前位于日志底部。  
      在执行撤销或重做以外的任何更新时，当前操作之后的所有日志条目都会被删除，新更新将成为日志中的最新条目。被删除的日志条目对应的更新将被永久丢失。

    .. method:: journal_op_name(step)

      * v1.19.0 新增

      仅限 PDF：返回编号为 *step* 的操作名称。


    .. method:: journal_can_do()

      * v1.19.0 新增

      仅限 PDF：检查是否可以从当前日志位置执行前进（"redo"）或后退（"undo"）操作。

      :returns: 一个字典 `{"undo": bool, "redo": bool}`。如果值为 `True`，则相应的方法可用。


    .. method:: journal_undo()

      * v1.19.0 新增

      仅限 PDF：撤销（undo）日志中的当前步骤。此操作会向日志的顶部移动。


    .. method:: journal_redo()

      * v1.19.0 新增

      仅限 PDF：重做（redo）日志中的当前步骤。此操作会向日志的底部移动。


    .. method:: journal_save(filename)

      * v1.19.0 新增

      仅限 PDF：将日志保存到文件。

      :arg str,fp filename: 可以是字符串格式的文件名，也可以是以 `"wb"` 打开的文件对象（或 `io.BytesIO()` 对象）。


    .. method:: journal_load(filename)

      * v1.19.0 新增

      仅限 PDF：从文件加载日志，并为文档启用日志记录。如果日志记录已启用，则会引发异常。

      :arg str,fp filename: 日志文件的名称（字符串），或以 `"rb"` 打开的文件对象（或 `io.BytesIO()` 对象）。


    .. method:: save_snapshot()

      * v1.19.0 新增

      仅限 PDF：保存文档的“快照”。这是一种特殊的增量保存格式的 PDF 文档，兼容日志记录，因此不提供任何保存选项。新建文档无法保存快照。

      这是一个正常的 PDF 文档，没有任何使用限制。如果未进行任何更改，它可以与日志一起使用，以执行撤销 / 重做操作或继续更新。


    .. attribute:: outline

      包含文档的首个 :ref:`Outline` 条目（或 `None`）。可作为遍历所有大纲项的起点。对于加密但未经身份验证的文档，访问此属性将引发 *AttributeError*。

      :type: :ref:`Outline`


    .. attribute:: is_closed

      如果文档仍然打开，则值为 *False*。如果文档已关闭，则大多数其他属性和方法都将被删除 / 禁用。此外，所有引用此文档的 :ref:`Page` 对象（即通过 :meth:`Document.load_page` 创建的页面）及其相关对象也将无法使用。  
      出于参考目的，:attr:`Document.name` 仍然存在，并包含原始文档的文件名（如果适用）。

      :type: bool


    .. attribute:: is_dirty

      如果这是一个 PDF 文档且包含未保存的更改，则值为 *True*，否则为 *False*。

      :type: bool


    .. attribute:: is_pdf

      如果这是一个 PDF 文档，则值为 *True*，否则为 *False*。

      :type: bool


    .. attribute:: is_form_pdf

      如果文档不是 PDF 或者没有表单字段，则值为 *False*。否则，返回根表单字段（无父级字段）的数量。

      *(v1.16.4 变更)* 现在返回总的（根）表单字段数量。

      :type: bool,int

    .. attribute:: is_reflowable

      如果文档具有可变页面布局（如电子书或 HTML），则值为 *True*。在这种情况下，可以在文档创建（打开）时或通过 :meth:`layout` 方法设置所需的页面尺寸。

      :type: bool


    .. attribute:: is_repaired

      * v1.18.2 新增

      如果 PDF 在打开时因结构问题被修复，则值为 *True*。对于非 PDF 文档，始终为 *False*。  
      如果该值为 *True*，则详细信息存储在 `TOOLS.mupdf_warnings()` 中，并且 :meth:`Document.can_save_incrementally` 将返回 *False*。

      :type: bool


    .. attribute:: is_fast_webaccess

      * v1.22.2 新增

      如果 PDF 采用线性化格式，则值为 *True*。对于非 PDF 文档，值为 *False*。

      :type: bool


    .. attribute:: markinfo

      * v1.22.2 新增

      一个字典，指示 `/MarkInfo` 值。如果未指定，则返回空字典。对于非 PDF 文档，返回 `None`。

      :type: dict


    .. attribute:: pagemode

      * v1.22.2 新增

      一个字符串，包含 `/PageMode` 值。如果未指定，则返回默认值 `"UseNone"`。对于非 PDF 文档，返回 `None`。

      :type: str


    .. attribute:: pagelayout

      * v1.22.2 新增

      一个字符串，包含 `/PageLayout` 值。如果未指定，则返回默认值 `"SinglePage"`。对于非 PDF 文档，返回 `None`。

      :type: str


    .. attribute:: version_count

      * v1.22.2 新增

      一个整数，表示文档中存在的版本数量。如果不是 PDF，则返回 0，否则返回增量保存次数加 1。

      :type: int


    .. attribute:: needs_pass

      指示文档是否受密码保护以限制访问。该指示器在 **即使文档已通过身份验证** 后仍保持不变。如果值为 *True*，则无法进行增量保存。

      :type: bool


    .. attribute:: is_encrypted

      该指示器初始值等于 :attr:`Document.needs_pass`。成功身份验证后，该值会被设置为 *False* 以反映当前状态。

      :type: bool


    .. attribute:: permissions

      * v1.16.0 变更：该属性现在是一个包含位指示器的整数，之前是一个字典。

      包含访问文档的权限。该值是一个整数，其中各个位表示不同的权限。例如，如果 *doc.permissions & pymupdf.PDF_PERM_MODIFY > 0*，则可以修改文档。  
      详见 :ref:`PermissionCodes`。

      :type: int

    .. attribute:: metadata

      包含文档的元数据，格式为 Python 字典。如果 *is_encrypted=True* 且 *needs_pass=True*，则返回 `None`。  
      键包括 *format*、*encryption*、*title*、*author*、*subject*、*keywords*、*creator*、*producer*、*creationDate*、*modDate*、*trapped*，其值均为字符串或 `None`。

      对于 PDF 文档，除 *format* 和 *encryption* 之外，键名对应于 PDF 的标准元数据键，例如 */Creator*、*/Producer*、*/CreationDate*、*/ModDate*、*/Title*、*/Author*、*/Subject*、*/Trapped* 和 */Keywords*。

      - *format* 表示文档格式（如 `'PDF-1.6'`、`'XPS'`、`'EPUB'`）。
      - *encryption* 指示加密方式，可能为 `None`（未加密）或加密算法名称（如 `'Standard V4 R4 128-bit RC4'`）。即使 *needs_pass=False*，文档仍可能受到部分权限限制，可通过 :attr:`Document.permissions` 检查具体权限。

      - 如果日期字段包含有效数据（这并不总是保证的），则采用 PDF 专用时间戳格式 `"D:<TS><TZ>"`，其中：
      
        - `<TS>` 是 12 位 ISO 格式时间戳 *YYYYMMDDhhmmss* （ *YYYY* - 年， *MM* - 月， *DD* - 日， *hh* - 小时， *mm* - 分钟， *ss* - 秒）。
        - `<TZ>` 是时区值（相对 GMT 的时间偏移量），格式为 `+hh'mm'` 或 `-hh'mm'`。

      - 例如，巴拉圭的时间戳可能为 `"D:20150415131602-04'00'"`，表示 2015 年 4 月 15 日 13:16:02（当地时间：亚松森）。

      :type: dict


    .. attribute:: name

      包含 *Document* 创建时的 *文件名* 或 *文件类型*。

      :type: str


    .. attribute:: page_count

      文档的总页数。如果文档没有页面，则返回 0。可以使用 `len(doc)` 获取相同的结果。

      :type: int


    .. attribute:: chapter_count

      * v1.17.0 新增

      文档中的章节数。对于支持章节的文档格式（目前仅 EPUB）有效，始终至少为 1。其他格式返回 1。

      :type: int


    .. attribute:: last_location

      * v1.17.0 新增

      记录文档的最后位置，格式为 `(chapter, pno)`。仅适用于支持章节的格式（目前仅 EPUB）。  
      其他格式返回 `(0, page_count - 1)`，如果文档无页面，则返回 `(0, -1)`。

      :type: tuple(int, int)


    .. attribute:: FormFonts

      包含在 */AcroForm* 对象中定义的表单字段字体名称的列表。如果不是 PDF，则返回 `None`。

      :type: list


    .. NOTE:: 

      对于修改 PDF 结构的方法（如 :meth:`insert_pdf`、:meth:`select`、:meth:`copy_page`、:meth:`delete_page` 等），请注意，程序中的对象或属性可能会失效或变成孤立对象。例如:

      - :ref:`Page` 对象及其子对象（链接、注释、小部件）可能会变得无效。  
      - 存储旧页面计数的变量可能需要更新。  
      - 目录结构等数据可能需要重新生成。  

      请务必保持相关变量的更新，或删除无效对象。详见 :ref:`ReferenialIntegrity`。


.. tab:: 英文

  This class represents a document. It can be constructed from a file or from memory.

  There exists the alias *open* for this class, i.e. `pymupdf.Document(...)` and `pymupdf.open(...)` do exactly the same thing.

  For details on **embedded files** refer to Appendix 3.

  .. note::

    Starting with v1.17.0, a new page addressing mechanism for **EPUB files only** is supported. This document type is internally organized in chapters such that pages can most efficiently be found by their so-called "location". The location is a tuple *(chapter, pno)* consisting of the chapter number and the page number **in that chapter**. Both numbers are zero-based.

    While it is still possible to locate a page via its (absolute) number, doing so may mean that the complete EPUB document must be laid out before the page can be addressed. This may have a significant performance impact if the document is very large. Using the page's *(chapter, pno)* prevents this from happening.

    To maintain a consistent API, PyMuPDF supports the page *location* syntax for **all file types** -- documents without this feature simply have just one chapter. :meth:`Document.load_page` and the equivalent index access now also support a *location* argument.

    There are a number of methods for converting between page numbers and locations, for determining the chapter count, the page count per chapter, for computing the next and the previous locations, and the last page location of a document.

  ======================================= ==========================================================
  **Method / Attribute**                  **Short Description**
  ======================================= ==========================================================
  :meth:`Document.add_layer`              PDF only: make new optional content configuration
  :meth:`Document.add_ocg`                PDF only: add new optional content group
  :meth:`Document.authenticate`           gain access to an encrypted document
  :meth:`Document.bake`                   PDF only: make annotations / fields permanent content
  :meth:`Document.can_save_incrementally` check if incremental save is possible
  :meth:`Document.chapter_page_count`     number of pages in chapter
  :meth:`Document.close`                  close the document
  :meth:`Document.convert_to_pdf`         write a PDF version to memory
  :meth:`Document.copy_page`              PDF only: copy a page reference
  :meth:`Document.del_toc_item`           PDF only: remove a single TOC item
  :meth:`Document.delete_page`            PDF only: delete a page
  :meth:`Document.delete_pages`           PDF only: delete multiple pages
  :meth:`Document.embfile_add`            PDF only: add a new embedded file from buffer
  :meth:`Document.embfile_count`          PDF only: number of embedded files
  :meth:`Document.embfile_del`            PDF only: delete an embedded file entry
  :meth:`Document.embfile_get`            PDF only: extract an embedded file buffer
  :meth:`Document.embfile_info`           PDF only: metadata of an embedded file
  :meth:`Document.embfile_names`          PDF only: list of embedded files
  :meth:`Document.embfile_upd`            PDF only: change an embedded file
  :meth:`Document.extract_font`           PDF only: extract a font by :data:`xref`
  :meth:`Document.extract_image`          PDF only: extract an embedded image by :data:`xref`
  :meth:`Document.ez_save`                PDF only: :meth:`Document.save` with different defaults
  :meth:`Document.find_bookmark`          retrieve page location after laid out document
  :meth:`Document.fullcopy_page`          PDF only: duplicate a page
  :meth:`Document.get_layer`              PDF only: lists of OCGs in ON, OFF, RBGroups
  :meth:`Document.get_layers`             PDF only: list of optional content configurations
  :meth:`Document.get_oc`                 PDF only: get OCG /OCMD xref of image / form xobject
  :meth:`Document.get_ocgs`               PDF only: info on all optional content groups
  :meth:`Document.get_ocmd`               PDF only: retrieve definition of an :data:`OCMD`
  :meth:`Document.get_page_fonts`         PDF only: list of fonts referenced by a page
  :meth:`Document.get_page_images`        PDF only: list of images referenced by a page
  :meth:`Document.get_page_labels`        PDF only: list of page label definitions
  :meth:`Document.get_page_numbers`       PDF only: get page numbers having a given label
  :meth:`Document.get_page_pixmap`        create a pixmap of a page by page number
  :meth:`Document.get_page_text`          extract the text of a page by page number
  :meth:`Document.get_page_xobjects`      PDF only: list of XObjects referenced by a page
  :meth:`Document.get_sigflags`           PDF only: determine signature state
  :meth:`Document.get_toc`                extract the table of contents
  :meth:`Document.get_xml_metadata`       PDF only: read the XML metadata
  :meth:`Document.has_annots`             PDF only: check if PDF contains any annots
  :meth:`Document.has_links`              PDF only: check if PDF contains any links
  :meth:`Document.insert_page`            PDF only: insert a new page
  :meth:`Document.insert_pdf`             PDF only: insert pages from another PDF
  :meth:`Document.insert_file`            PDF only: insert pages from arbitrary document
  :meth:`Document.journal_can_do`         PDF only: which journal actions are possible
  :meth:`Document.journal_enable`         PDF only: enables journalling for the document
  :meth:`Document.journal_load`           PDF only: load journal from a file
  :meth:`Document.journal_op_name`        PDF only: return name of a journalling step
  :meth:`Document.journal_position`       PDF only: return journalling status
  :meth:`Document.journal_redo`           PDF only: redo current operation
  :meth:`Document.journal_save`           PDF only: save journal to a file
  :meth:`Document.journal_start_op`       PDF only: start an "operation" giving it a name
  :meth:`Document.journal_stop_op`        PDF only: end current operation
  :meth:`Document.journal_undo`           PDF only: undo current operation
  :meth:`Document.layer_ui_configs`       PDF only: list of optional content intents
  :meth:`Document.layout`                 re-paginate the document (if supported)
  :meth:`Document.load_page`              read a page
  :meth:`Document.make_bookmark`          create a page pointer in reflowable documents
  :meth:`Document.move_page`              PDF only: move a page to different location in doc
  :meth:`Document.need_appearances`       PDF only: get/set `/NeedAppearances` property
  :meth:`Document.new_page`               PDF only: insert a new empty page
  :meth:`Document.next_location`          return (chapter, pno) of following page
  :meth:`Document.outline_xref`           PDF only: :data:`xref` a TOC item
  :meth:`Document.page_cropbox`           PDF only: the unrotated page rectangle
  :meth:`Document.page_xref`              PDF only: :data:`xref` of a page number
  :meth:`Document.pages`                  iterator over a page range
  :meth:`Document.pdf_catalog`            PDF only: :data:`xref` of catalog (root)
  :meth:`Document.pdf_trailer`            PDF only: trailer source
  :meth:`Document.prev_location`          return (chapter, pno) of preceding page
  :meth:`Document.reload_page`            PDF only: provide a new copy of a page
  :meth:`Document.resolve_names`          PDF only: Convert destination names into a Python dict
  :meth:`Document.save`                   PDF only: save the document
  :meth:`Document.saveIncr`               PDF only: save the document incrementally
  :meth:`Document.scrub`                  PDF only: remove sensitive data
  :meth:`Document.search_page_for`        search for a string on a page
  :meth:`Document.select`                 PDF only: select a subset of pages
  :meth:`Document.set_layer_ui_config`    PDF only: set OCG visibility temporarily
  :meth:`Document.set_layer`              PDF only: mass changing OCG states
  :meth:`Document.set_markinfo`           PDF only: set the MarkInfo values
  :meth:`Document.set_metadata`           PDF only: set the metadata
  :meth:`Document.set_oc`                 PDF only: attach OCG/OCMD to image / form xobject
  :meth:`Document.set_ocmd`               PDF only: create or update an :data:`OCMD`
  :meth:`Document.set_page_labels`        PDF only: add/update page label definitions
  :meth:`Document.set_pagemode`           PDF only: set the PageMode
  :meth:`Document.set_pagelayout`         PDF only: set the PageLayout
  :meth:`Document.set_toc_item`           PDF only: change a single TOC item
  :meth:`Document.set_toc`                PDF only: set the table of contents (TOC)
  :meth:`Document.set_xml_metadata`       PDF only: create or update document XML metadata
  :meth:`Document.subset_fonts`           PDF only: create font subsets
  :meth:`Document.switch_layer`           PDF only: activate OC configuration
  :meth:`Document.tobytes`                PDF only: writes document to memory
  :meth:`Document.xref_copy`              PDF only: copy a PDF dictionary to another :data:`xref`
  :meth:`Document.xref_get_key`           PDF only: get the value of a dictionary key
  :meth:`Document.xref_get_keys`          PDF only: list the keys of object at :data:`xref`
  :meth:`Document.xref_object`            PDF only: get the definition source of :data:`xref`
  :meth:`Document.xref_set_key`           PDF only: set the value of a dictionary key
  :meth:`Document.xref_stream_raw`        PDF only: raw stream source at :data:`xref`
  :meth:`Document.xref_xml_metadata`      PDF only: :data:`xref` of XML metadata
  :attr:`Document.chapter_count`          number of chapters
  :attr:`Document.FormFonts`              PDF only: list of global widget fonts
  :attr:`Document.is_closed`              has document been closed?
  :attr:`Document.is_dirty`               PDF only: has document been changed yet?
  :attr:`Document.is_encrypted`           document (still) encrypted?
  :attr:`Document.is_fast_webaccess`      is PDF linearized?
  :attr:`Document.is_form_pdf`            is this a Form PDF?
  :attr:`Document.is_pdf`                 is this a PDF?
  :attr:`Document.is_reflowable`          is this a reflowable document?
  :attr:`Document.is_repaired`            PDF only: has this PDF been repaired during open?
  :attr:`Document.last_location`          (chapter, pno) of last page
  :attr:`Document.metadata`               metadata
  :attr:`Document.markinfo`               PDF MarkInfo value
  :attr:`Document.name`                   filename of document
  :attr:`Document.needs_pass`             require password to access data?
  :attr:`Document.outline`                first `Outline` item
  :attr:`Document.page_count`             number of pages
  :attr:`Document.permissions`            permissions to access the document
  :attr:`Document.pagemode`               PDF PageMode value
  :attr:`Document.pagelayout`             PDF PageLayout value
  :attr:`Document.version_count`          PDF count of versions
  ======================================= ==========================================================

  **Class API**

  .. class:: Document
    :no-index:

    .. method:: __init__(self, filename=None, stream=None, *, filetype=None, rect=None, width=0, height=0, fontsize=11)
      :no-index:

      * Changed in v1.14.13: support `io.BytesIO` for memory documents.
      * Changed in v1.19.6: Clearer, shorter and more consistent exception messages. File type "pdf" is always assumed if not specified. Empty files and memory areas will always lead to exceptions.

      Creates a *Document* object.

      * With default parameters, a **new empty PDF** document will be created.
      * If *stream* is given, then the document is created from memory and, if not a PDF, either *filename* or *filetype* must indicate its type.
      * If *stream* is `None`, then a document is created from the file given by *filename*. Its type is inferred from the extension. This can be overruled by *filetype.*

      :arg str,pathlib filename: A UTF-8 string or *pathlib* object containing a file path. The document type is inferred from the filename extension. If not present or not matching :ref:`a supported type<Supported_File_Types>`, a PDF document is assumed. For memory documents, this argument may be used instead of `filetype`, see below.

      :arg bytes,bytearray,BytesIO stream: A memory area containing a supported document. If not a PDF, its type **must** be specified by either `filename` or `filetype`.

      :arg str filetype: A string specifying the type of document. This may be anything looking like a filename (e.g. "x.pdf"), in which case MuPDF uses the extension to determine the type, or a mime type like *application/pdf*. Just using strings like "pdf"  or ".pdf" will also work. May be omitted for PDF documents, otherwise must match :ref:`a supported document type<Supported_File_Types>`.

      :arg rect_like rect: a rectangle specifying the desired page size. This parameter is only meaningful for documents with a variable page layout ("reflowable" documents), like e-books or HTML, and ignored otherwise. If specified, it must be a non-empty, finite rectangle with top-left coordinates (0, 0). Together with parameter *fontsize*, each page will be accordingly laid out and hence also determine the number of pages.

      :arg float width: may used together with *height* as an alternative to *rect* to specify layout information.

      :arg float height: may used together with *width* as an alternative to *rect* to specify layout information.

      :arg float fontsize: the default :data:`fontsize` for reflowable document types. This parameter is ignored if none of the parameters *rect* or *width* and *height* are specified. Will be used to calculate the page layout.

      :raises TypeError: if the *type* of any parameter does not conform.
      :raises FileNotFoundError: if the file / path cannot be found. Re-implemented as subclass of `RuntimeError`.
      :raises EmptyFileError: if the file / path is empty or the `bytes` object in memory has zero length. A subclass of `FileDataError` and `RuntimeError`.
      :raises ValueError: if an unknown file type is explicitly specified.
      :raises FileDataError: if the document has an invalid structure for the given type -- or is no file at all (but e.g. a folder). A subclass of `RuntimeError`.

      :return: A document object. If the document cannot be created, an exception is raised in the above sequence. Note that PyMuPDF-specific exceptions, `FileNotFoundError`, `EmptyFileError` and `FileDataError` are intercepted if you check for `RuntimeError`.

        In case of problems you can see more detail in the internal messages store: `print(pymupdf.TOOLS.mupdf_warnings())` (which will be emptied by this call, but you can also prevent this -- consult :meth:`Tools.mupdf_warnings`).

      .. note:: Not all document types are checked for valid formats already at open time. Raster images for example will raise exceptions only later, when trying to access the content. Other types (notably with non-binary content) may also be opened (and sometimes **accessed**) successfully -- sometimes even when having invalid content for the format:

        * HTM, HTML, XHTML: **always** opened, `metadata["format"]` is "HTML5", resp. "XHTML".
        * XML, FB2: **always** opened, `metadata["format"]` is "FictionBook2".

      Overview of possible forms, note: `open` is a synonym of `Document`::

          >>> # from a file
          >>> doc = pymupdf.open("some.xps")
          >>> # handle wrong extension
          >>> doc = pymupdf.open("some.file", filetype="xps")
          >>>
          >>> # from memory, filetype is required if not a PDF
          >>> doc = pymupdf.open("xps", mem_area)
          >>> doc = pymupdf.open(None, mem_area, "xps")
          >>> doc = pymupdf.open(stream=mem_area, filetype="xps")
          >>>
          >>> # new empty PDF
          >>> doc = pymupdf.open()
          >>> doc = pymupdf.open(None)
          >>> doc = pymupdf.open("")

      .. note:: Raster images with a wrong (but supported) file extension **are no problem**. MuPDF will determine the correct image type when file **content** is actually accessed and will process it without complaint. So `pymupdf.open("file.jpg")` will work even for a PNG image.

      The Document class can be also be used as a **context manager**. On exit, the document will automatically be closed.

          >>> import pymupdf
          >>> with pymupdf.open(...) as doc:
                  for page in doc: print("page %i" % page.number)
          page 0
          page 1
          page 2
          page 3
          >>> doc.is_closed
          True
          >>>


    .. method:: get_oc(xref)
      :no-index:

      * New in v1.18.4

      Return the cross reference number of an :data:`OCG` or :data:`OCMD` attached to an image or form xobject.

      :arg int xref: the :data:`xref` of an image or form xobject. Valid such cross reference numbers are returned by :meth:`Document.get_page_images`, resp. :meth:`Document.get_page_xobjects`. For invalid numbers, an exception is raised.
      :rtype: int
      :returns: the cross reference number of an optional contents object or zero if there is none.

    .. method:: set_oc(xref, ocxref)
      :no-index:

      * New in v1.18.4

      If *xref* represents an image or form xobject, set or remove the cross reference number *ocxref* of an optional contents object.

      :arg int xref: the :data:`xref` of an image or form xobject [#f5]_. Valid such cross reference numbers are returned by :meth:`Document.get_page_images`, resp. :meth:`Document.get_page_xobjects`. For invalid numbers, an exception is raised.
      :arg int ocxref: the :data:`xref` number of an :data:`OCG` / :data:`OCMD`. If not zero, an invalid reference raises an exception. If zero, any OC reference is removed.


    .. method:: get_layers()
      :no-index:

      * New in v1.18.3

      Show optional layer configurations. There always is a standard one, which is not included in the response.

        >>> for item in doc.get_layers(): print(item)
        {'number': 0, 'name': 'my-config', 'creator': ''}
        >>> # use 'number' as config identifier in add_ocg

    .. method:: add_layer(name, creator=None, on=None)
      :no-index:

      * New in v1.18.3

      Add an optional content configuration. Layers serve as a collection of ON / OFF states for optional content groups and allow fast visibility switches between different views on the same document.

      :arg str name: arbitrary name.
      :arg str creator: (optional) creating software.
      :arg sequ on: a sequence of OCG :data:`xref` numbers which should be set to ON when this layer gets activated. All OCGs not listed here will be set to OFF.


    .. method:: switch_layer(number, as_default=False)
      :no-index:

      * New in v1.18.3

      Switch to a document view as defined by the optional layer's configuration number. This is temporary, except if established as default.

      :arg int number: config number as returned by :meth:`Document.layer_configs`.
      :arg bool as_default: make this the default configuration.

      Activates the ON / OFF states of OCGs as defined in the identified layer. If *as_default=True*, then additionally all layers, including the standard one, are merged and the result is written back to the standard layer, and **all optional layers are deleted**.


    .. method:: add_ocg(name, config=-1, on=True, intent="View", usage="Artwork")
      :no-index:

      * New in v1.18.3

      Add an optional content group. An OCG is the most important unit of information to determine object visibility. For a PDF, in order to be regarded as having optional content, at least one OCG must exist.

      :arg str name: arbitrary name. Will show up in supporting PDF viewers.
      :arg int config: layer configuration number. Default -1 is the standard configuration.
      :arg bool on: standard visibility status for objects pointing to this OCG.
      :arg str,list intent: a string or list of strings declaring the visibility intents. There are two PDF standard values to choose from: "View" and "Design". Default is "View". Correct **spelling is important**.
      :arg str usage: another influencer for OCG visibility. This will become part of the OCG's `/Usage` key. There are two PDF standard values to choose from: "Artwork" and "Technical". Default is "Artwork". Please only change when required.

      :returns: :data:`xref` of the created OCG. Use as entry for `oc` parameter in supporting objects.

      .. note:: Multiple OCGs with identical parameters may be created. This will not cause problems. Garbage option 3 of :meth:`Document.save` will get rid of any duplicates.


    .. method:: set_ocmd(xref=0, ocgs=None, policy="AnyOn", ve=None)
      :no-index:

      * New in v1.18.4

      Create or update an :data:`OCMD`, **Optional Content Membership Dictionary.**

      :arg int xref: :data:`xref` of the OCMD to be updated, or 0 for a new OCMD.
      :arg list ocgs: a sequence of :data:`xref` numbers of existing :data:`OCG` PDF objects.
      :arg str policy: one of "AnyOn" (default), "AnyOff", "AllOn", "AllOff" (mixed or lower case).
      :arg list ve: a "visibility expression". This is a list of arbitrarily nested other lists -- see explanation below. Use as an alternative to the combination *ocgs* / *policy* if you need to formulate more complex conditions.
      :rtype: int
      :returns: :data:`xref` of the OCMD. Use as `oc=xref` parameter in supporting objects, and respectively in :meth:`Document.set_oc` or :meth:`Annot.set_oc`.

      .. note::

        Like an OCG, an OCMD has a visibility state ON or OFF, and it can be used like an OCG. In contrast to an OCG, the OCMD state is determined by evaluating the state of one or more OCGs via special forms of **boolean expressions.** If the expression evaluates to true, the OCMD state is ON and OFF for false.

        There are two ways to formulate OCMD visibility:

        1. Use the combination of *ocgs* and *policy*: The *policy* value is interpreted as follows:

          - AnyOn -- (default) true if at least one OCG is ON.
          - AnyOff -- true if at least one OCG is OFF.
          - AllOn -- true if all OCGs are ON.
          - AllOff -- true if all OCGs are OFF.

          Suppose you want two PDF objects be displayed exactly one at a time (if one is ON, then the other one must be OFF):

          Solution: use an **OCG** for object 1 and an **OCMD** for object 2. Create the OCMD via `set_ocmd(ocgs=[xref], policy="AllOff")`, with the :data:`xref` of the OCG.

        2. Use the **visibility expression** *ve*: This is a list of two or more items. The **first item** is a logical keyword: one of the strings **"and"**, **"or"**, or **"not"**. The **second** and all subsequent items must either be an integer or another list. An integer must be the :data:`xref` number of an OCG. A list must again have at least two items starting with one of the boolean keywords. This syntax is a bit awkward, but quite powerful:

          - Each list must start with a logical keyword.
          - If the keyword is a **"not"**, then the list must have exactly two items. If it is **"and"** or **"or"**, any number of other items may follow.
          - Items following the logical keyword may be either integers or again a list. An *integer* must be the xref of an OCG. A *list* must conform to the previous rules.

          **Examples:**

          - `set_ocmd(ve=["or", 4, ["not", 5], ["and", 6, 7]])`. This delivers ON if the following is true: **"4 is ON, or 5 is OFF, or 6 and 7 are both ON"**.
          - `set_ocmd(ve=["not", xref])`. This has the same effect as the OCMD example created under 1.

          For more details and examples see page 224 of :ref:`AdobeManual`. Also do have a look at example scripts `here <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/optional-content>`_.

          Visibility expressions, `/VE`, are part of PDF specification version 1.6. So not all PDF viewers / readers may already support this feature and hence will react in some standard way for those cases.


    .. method:: get_ocmd(xref)
      :no-index:

      * New in v1.18.4

      Retrieve the definition of an :data:`OCMD`.

      :arg int xref: the :data:`xref` of the OCMD.
      :rtype: dict
      :returns: a dictionary with the keys *xref*, *ocgs*, *policy* and *ve*.


    .. method:: get_layer(config=-1)
      :no-index:

      * New in v1.18.3

      List of optional content groups by status in the specified configuration. This is a dictionary with lists of cross reference numbers for OCGs that occur in the arrays `/ON`, `/OFF` or in some radio button group (`/RBGroups`).

      :arg int config: the configuration layer (default is the standard config layer).

      >>> pprint(doc.get_layer())
      {'off': [8, 9, 10], 'on': [5, 6, 7], 'rbgroups': [[7, 10]]}
      >>>

    .. method:: set_layer(config, *, on=None, off=None, basestate=None, rbgroups=None, locked=None)
      :no-index:

      * New in v1.18.3

      * Changed in v1.22.5: Support list of *locked* OCGs.

      Mass status changes of optional content groups. **Permanently** sets the status of OCGs.

      :arg int config: desired configuration layer, choose -1 for the default one.
      :arg list on: list of :data:`xref` of OCGs to set ON. Replaces previous values. An empty list will cause no OCG being set to ON anymore. Should be specified if `basestate="ON"` is used.
      :arg list off: list of :data:`xref` of OCGs to set OFF. Replaces previous values. An empty list will cause no OCG being set to OFF anymore. Should be specified if `basestate="OFF"` is used.
      :arg str basestate: state of OCGs that are not mentioned in *on* or *off*. Possible values are "ON", "OFF" or "Unchanged". Upper / lower case possible.
      :arg list rbgroups: a list of lists. Replaces previous values. Each sublist should contain two or more OCG xrefs. OCGs in the same sublist are handled like buttons in a radio button group: setting one to ON automatically sets all other group members to OFF.
      :arg list locked: a list of OCG xref number that cannot be changed by the user interface.

      Values `None` will not change the corresponding PDF array.

        >>> doc.set_layer(-1, basestate="OFF")  # only changes the base state
        >>> pprint(doc.get_layer())
        {'basestate': 'OFF', 'off': [8, 9, 10], 'on': [5, 6, 7], 'rbgroups': [[7, 10]]}


    .. method:: get_ocgs()
      :no-index:

      * New in v1.18.3

      Details of all optional content groups. This is a dictionary of dictionaries like this (key is the OCG's :data:`xref`):

        >>> pprint(doc.get_ocgs())
        {13: {'on': True,
              'intent': ['View', 'Design'],
              'name': 'Circle',
              'usage': 'Artwork'},
        14: {'on': True,
              'intent': ['View', 'Design'],
              'name': 'Square',
              'usage': 'Artwork'},
        15: {'on': False, 'intent': ['View'], 'name': 'Square', 'usage': 'Artwork'}}
        >>>

    .. method:: layer_ui_configs()
      :no-index:

      * New in v1.18.3

      Show the visibility status of optional content that is modifiable by the user interface of supporting PDF viewers.

          * Only reports items contained in the currently selected layer configuration.

          * The meaning of the dictionary keys is as follows:
            - *depth:* item's nesting level in the `/Order` array
            - *locked:* true if cannot be changed via user interfaces
            - *number:* running sequence number
            - *on:* item state
            - *text:* text string or name field of the originating OCG
            - *type:* one of "label" (set by a text string), "checkbox" (set by a single OCG) or "radiobox" (set by a set of connected OCGs)

    .. method:: set_layer_ui_config(number, action=0)
      :no-index:

      * New in v1.18.3

      Modify OC visibility status of content groups. This is analog to what supporting PDF viewers would offer.

        Please note that visibility is **not** a property stored with the OCG. It is not even information necessarily present in the PDF document at all. Instead, the current visibility is **temporarily** set using the user interface of some supporting PDF consumer software. The same type of functionality is offered by this method.

        To make **permanent** changes, use :meth:`Document.set_layer`.

      :arg int,str number: either the sequence number of the item in list :meth:`Document.layer_configs` or the "text" of one of these items.
      :arg int action: `PDF_OC_ON` = set on (default), `PDF_OC_TOGGLE` = toggle on/off, `PDF_OC_OFF` = set off.


    .. method:: authenticate(password)
      :no-index:

      Decrypts the document with the string *password*. If successful, document data can be accessed. For PDF documents, the "owner" and the "user" have different privileges, and hence different passwords may exist for these authorization levels. The method will automatically establish the appropriate (owner or user) access rights for the provided password.

      :arg str password: owner or user password.

      :rtype: int
      :returns: a positive value if successful, zero otherwise (the string does not match either password). If positive, the indicator :attr:`Document.is_encrypted` is set to *False*. **Positive** return codes carry the following information detail:

        * 1 => authenticated, but the PDF has neither owner nor user passwords.
        * 2 => authenticated with the **user** password.
        * 4 => authenticated with the **owner** password.
        * 6 => authenticated and both passwords are equal -- probably a rare situation.

        .. note::

          The document may be protected by an owner, but **not** by a user password. Detect this situation via `doc.authenticate("") == 2`. This allows opening and reading the document without authentication, but, depending on the :attr:`Document.permissions` value, other actions may be prohibited. PyMuPDF (like MuPDF) in this case **ignores those restrictions**. So, -- in contrast to any PDF viewers -- you can for example extract text and add or modify content, even if the respective permission flags `PDF_PERM_COPY`, `PDF_PERM_MODIFY`, `PDF_PERM_ANNOTATE`, etc. are set off! It is your responsibility building a legally compliant application where applicable.

    .. method:: get_page_numbers(label, only_one=False)
      :no-index:

      * New in v 1.18.6

      PDF only: Return a list of page numbers that have the specified label -- note that labels may not be unique in a PDF. This implies a sequential search through **all page numbers** to compare their labels.

      .. note:: Implementation detail -- pages are **not loaded** for this purpose.

      :arg str label: the label to look for, e.g. "vii" (Roman number 7).
      :arg bool only_one: stop after first hit. Useful e.g. if labelling is known to be unique, or there are many pages, etc. The default will check every page number.
      :rtype: list
      :returns: list of page numbers that have this label. Empty if none found, no labels defined, etc.


    .. method:: get_page_labels()
      :no-index:

      * New in v1.18.7

      PDF only: Extract the list of page label definitions. Typically used for modifications before feeding it into :meth:`Document.set_page_labels`.

      :returns: a list of dictionaries as defined in :meth:`Document.set_page_labels`.

    .. method:: set_page_labels(labels)
      :no-index:

      * New in v1.18.6

      PDF only: Add or update the page label definitions of the PDF.

      :arg list labels: a list of dictionaries. Each dictionary defines a label building rule and a 0-based "start" page number. That start page is the first for which the label definition is valid. Each dictionary has up to 4 items and looks like `{'startpage': int, 'prefix': str, 'style': str, 'firstpagenum': int}` and has the following items.

          - `startpage`: (int) the first page number (0-based) to apply the label rule. This key **must be present**. The rule is applied to all subsequent pages until either end of document or superseded by the rule with the next larger page number.
          - `prefix`: (str) an arbitrary string to start the label with, e.g. "A-". Default is "".
          - `style`: (str) the numbering style. Available are "D" (decimal), "r"/"R" (Roman numbers, lower / upper case), and "a"/"A" (lower / upper case alphabetical numbering: "a" through "z", then "aa" through "zz", etc.). Default is "". If "", no numbering will take place and the pages in that range will receive the same label consisting of the `prefix` value. If prefix is also omitted, then the label will be "".
          - `firstpagenum`: (int) start numbering with this value. Default is 1, smaller values are ignored.

      For example::

        [{'startpage': 6, 'prefix': 'A-', 'style': 'D', 'firstpagenum': 10},
        {'startpage': 10, 'prefix': '', 'style': 'D', 'firstpagenum': 1}]

      will generate the labels "A-10", "A-11", "A-12", "A-13", "1", "2", "3", ... for pages 6, 7 and so on until end of document. Pages 0 through 5 will have the label "".


    .. method:: make_bookmark(loc)
      :no-index:

      * New in v.1.17.3

      Return a page pointer in a reflowable document. After re-layouting the document, the result of this method can be used to find the new location of the page.

      .. note:: Do not confuse with items of a table of contents, TOC.

      :arg list,tuple loc: page location. Must be a valid *(chapter, pno)*.

      :rtype: pointer
      :returns: a long integer in pointer format. To be used for finding the new location of the page after re-layouting the document. Do not touch or re-assign.


    .. method:: find_bookmark(bookmark)
      :no-index:

      * New in v.1.17.3

      Return the new page location after re-layouting the document.

      :arg pointer bookmark: created by :meth:`Document.make_bookmark`.

      :rtype: tuple
      :returns: the new (chapter, pno) of the page.


    .. method:: chapter_page_count(chapter)
      :no-index:

      * New in v.1.17.0

      Return the number of pages of a chapter.

      :arg int chapter: the 0-based chapter number.

      :rtype: int
      :returns: number of pages in chapter. Relevant only for document types with chapter support (EPUB currently).


    .. method:: next_location(page_id)
      :no-index:

      * New in v.1.17.0

      Return the location of the following page.

      :arg tuple page_id: the current page id. This must be a tuple *(chapter, pno)* identifying an existing page.

      :returns: The tuple of the following page, i.e. either *(chapter, pno + 1)* or *(chapter + 1, 0)*, **or** the empty tuple *()* if the argument was the last page. Relevant only for document types with chapter support (EPUB currently).


    .. method:: prev_location(page_id)
      :no-index:

      * New in v.1.17.0

      Return the locator of the preceding page.

      :arg tuple page_id: the current page id. This must be a tuple *(chapter, pno)* identifying an existing page.

      :returns: The tuple of the preceding page, i.e. either *(chapter, pno - 1)* or the last page of the preceding chapter, **or** the empty tuple *()* if the argument was the first page. Relevant only for document types with chapter support (EPUB currently).


    .. method:: load_page(page_id=0)
      :no-index:

      * Changed in v1.17.0: For document types supporting a so-called "chapter structure" (like EPUB), pages can also be loaded via the combination of chapter number and relative page number, instead of the absolute page number. This should **significantly speed up access** for large documents.

      Create a :ref:`Page` object for further processing (like rendering, text searching, etc.).

      :arg int,tuple page_id: *(Changed in v1.17.0)*

          Either a 0-based page number, or a tuple *(chapter, pno)*. For an **integer**, any `-∞ < page_id < page_count` is acceptable. While page_id is negative, :attr:`page_count` will be added to it. For example: to load the last page, you can use *doc.load_page(-1)*. After this you have page.number = doc.page_count - 1.

          For a tuple, *chapter* must be in range :attr:`Document.chapter_count`, and *pno* must be in range :meth:`Document.chapter_page_count` of that chapter. Both values are 0-based. Using this notation, :attr:`Page.number` will equal the given tuple. Relevant only for document types with chapter support (EPUB currently).

      :rtype: :ref:`Page`

    .. note::

      Documents also follow the Python sequence protocol with page numbers as indices: *doc.load_page(n) == doc[n]*.

      For **absolute page numbers** only, expressions like *"for page in doc: ..."* and *"for page in reversed(doc): ..."* will successively yield the document's pages. Refer to :meth:`Document.pages` which allows processing pages as with slicing.

      You can also use index notation with the new chapter-based page identification: use *page = doc[(5, 2)]* to load the third page of the sixth chapter.

      To maintain a consistent API, for document types not supporting a chapter structure (like PDFs), :attr:`Document.chapter_count` is 1, and pages can also be loaded via tuples *(0, pno)*. See this [#f3]_ footnote for comments on performance improvements.

    .. method:: reload_page(page)
      :no-index:

      * New in v1.16.10

      PDF only: Provide a new copy of a page after finishing and updating all pending changes.

      :arg page: page object.
      :type page: :ref:`Page`

      :rtype: :ref:`Page`

      :returns: a new copy of the same page. All pending updates (e.g. to annotations or widgets) will be finalized and a fresh copy of the page will be loaded.

        .. note:: In a typical use case, a page :ref:`Pixmap` should be taken after annotations / widgets have been added or changed. To force all those changes being reflected in the page structure, this method re-instates a fresh copy while keeping the object hierarchy "document -> page -> annotations/widgets" intact.


    .. method:: resolve_names()
      :no-index:

      PDF only: Convert destination names into a Python dict.

      :returns:
          A dictionary with the following layout:

          * *key*: (str) the name.
          * *value*: (dict) with the following layout:
              * "page":  target page number (0-based). If no page number found -1.
              * "to": (x, y) target point on page. Currently in PDF coordinates,
                i.e. point (0,0) is the bottom-left of the page.
              * "zoom": (float) the zoom factor.
              * "dest": (str) only present if the target location on the page has
                not been provided as "/XYZ" or if no page number was found.
          Examples::

              {
                  '__bookmark_1': {'page': 0, 'to': (0.0, 541.0), 'zoom': 0.0},
                  '__bookmark_2': {'page': 0, 'to': (0.0, 481.45), 'zoom': 0.0},
              }

          or::

              {
                  '21154a7c20684ceb91f9c9adc3b677c40': {'page': -1, 'dest': '/XYZ 15.75 1486 0'},
                  ...
              }

      All names found in the catalog under keys "/Dests" and "/Names/Dests" are
      included.

      * New in v1.23.6


    .. method:: page_cropbox(pno)
      :no-index:

      * New in v1.17.7

      PDF only: Return the unrotated page rectangle -- **without loading the page** (via :meth:`Document.load_page`). This is meant for internal purpose requiring best possible performance.

      :arg int pno: 0-based page number.

      :returns: :ref:`Rect` of the page like :meth:`Page.rect`, but ignoring any rotation.

    .. method:: page_xref(pno)
      :no-index:

      * New in v1.17.7

      PDF only: Return the :data:`xref` of the page -- **without loading the page** (via :meth:`Document.load_page`). This is meant for internal purpose requiring best possible performance.

      :arg int pno: 0-based page number.

      :returns: :data:`xref` of the page like :attr:`Page.xref`.

    .. method:: pages(start=None, [stop=None, [step=None]])
      :no-index:

      * New in v1.16.4

      A generator for a range of pages. Parameters have the same meaning as in the built-in function *range()*. Intended for expressions of the form *"for page in doc.pages(start, stop, step): ..."*.

      :arg int start: start iteration with this page number. Default is zero, allowed values are `-∞ < start < page_count`. While this is negative, :attr:`page_count` is added **before** starting the iteration.
      :arg int stop: stop iteration at this page number. Default is :attr:`page_count`, possible are `-∞ < stop <= page_count`. Larger values are **silently replaced** by the default. Negative values will cyclically emit the pages in reversed order. As with the built-in *range()*, this is the first page **not** returned.
      :arg int step: stepping value. Defaults are 1 if start < stop and -1 if start > stop. Zero is not allowed.

      :returns: a generator iterator over the document's pages. Some examples:

          * "doc.pages()" emits all pages.
          * "doc.pages(4, 9, 2)" emits pages 4, 6, 8.
          * "doc.pages(0, None, 2)" emits all pages with even numbers.
          * "doc.pages(-2)" emits the last two pages.
          * "doc.pages(-1, -1)" emits all pages in reversed order.
          * "doc.pages(-1, -10)" always emits 10 pages in reversed order, starting with the last page -- **repeatedly** if the document has less than 10 pages. So for a 4-page document the following page numbers are emitted: 3, 2, 1, 0, 3, 2, 1, 0, 3, 2, 1, 0, 3.

    .. method:: convert_to_pdf(from_page=-1, to_page=-1, rotate=0)
      :no-index:

      Create a PDF version of the current document and write it to memory. **All document types** are supported. The parameters have the same meaning as in :meth:`insert_pdf`. In essence, you can restrict the conversion to a page subset, specify page rotation, and revert page sequence.

      :arg int from_page: first page to copy (0-based). Default is first page.

      :arg int to_page: last page to copy (0-based). Default is last page.

      :arg int rotate: rotation angle. Default is 0 (no rotation). Should be *n * 90* with an integer n (not checked).

      :rtype: bytes
      :returns: a Python *bytes* object containing a PDF file image. It is created by internally using `tobytes(garbage=4, deflate=True)`. See :meth:`tobytes`. You can output it directly to disk or open it as a PDF. Here are some examples::

          >>> # convert an XPS file to PDF
          >>> xps = pymupdf.open("some.xps")
          >>> pdfbytes = xps.convert_to_pdf()
          >>>
          >>> # either do this -->
          >>> pdf = pymupdf.open("pdf", pdfbytes)
          >>> pdf.save("some.pdf")
          >>>
          >>> # or this -->
          >>> pdfout = open("some.pdf", "wb")
          >>> pdfout.tobytes(pdfbytes)
          >>> pdfout.close()

          >>> # copy image files to PDF pages
          >>> # each page will have image dimensions
          >>> doc = pymupdf.open()                     # new PDF
          >>> imglist = [ ... image file names ...] # e.g. a directory listing
          >>> for img in imglist:
                  imgdoc=pymupdf.open(img)           # open image as a document
                  pdfbytes=imgdoc.convert_to_pdf()  # make a 1-page PDF of it
                  imgpdf=pymupdf.open("pdf", pdfbytes)
                  doc.insert_pdf(imgpdf)             # insert the image PDF
          >>> doc.save("allmyimages.pdf")

      .. note:: The method uses the same logic as the *mutool convert* CLI. This works very well in most cases -- however, beware of the following limitations.

        * Image files: perfect, no issues detected. However, image transparency is ignored. If you need that (like for a watermark), use :meth:`Page.insert_image` instead. Otherwise, this method is recommended for its much better performance.
        * XPS: appearance very good. Links work fine, outlines (bookmarks) are lost, but can easily be recovered [#f2]_.
        * EPUB, CBZ, FB2: similar to XPS.
        * SVG: medium. Roughly comparable to `svglib <https://github.com/deeplook/svglib>`_.

    .. method:: get_toc(simple=True)
      :no-index:

      Creates a table of contents (TOC) out of the document's outline chain.

      :arg bool simple: Indicates whether a simple or a detailed TOC is required. If *False*, each item of the list also contains a dictionary with :ref:`linkDest` details for each outline entry.

      :rtype: list

      :returns: a list of lists. Each entry has the form *[lvl, title, page, dest]*. Its entries have the following meanings:

        * *lvl* -- hierarchy level (positive *int*). The first entry is always 1. Entries in a row are either **equal**, **increase** by 1, or **decrease** by any number.
        * *title* -- title (*str*)
        * *page* -- 1-based source page number (*int*). `-1` if no destination or outside document.
        * *dest* -- (*dict*) included only if *simple=False*. Contains details of the TOC item as follows:

          - kind: destination kind, see :ref:`linkDest Kinds`.
          - file: filename if kind is :data:`LINK_GOTOR` or :data:`LINK_LAUNCH`.
          - page: target page, 0-based, :data:`LINK_GOTOR` or :data:`LINK_GOTO` only.
          - to: position on target page (:ref:`Point`).
          - zoom: (float) zoom factor on target page.
          - xref: :data:`xref` of the item (0 if no PDF).
          - color: item color in PDF RGB format `(red, green, blue)`, or omitted (always omitted if no PDF).
          - bold: true if bold item text or omitted. PDF only.
          - italic: true if italic item text, or omitted. PDF only.
          - collapse: true if sub-items are folded, or omitted. PDF only.
          - nameddest: target name if kind=4. PDF only. (New in 1.23.7.)


    .. method:: xref_get_keys(xref)
      :no-index:

      * New in v1.18.7

      PDF only: Return the PDF dictionary keys of the :data:`dictionary` object provided by its xref number.

      :arg int xref: the :data:`xref`. *(Changed in v1.18.10)* Use `-1` to access the special dictionary "PDF trailer".

      :returns: a tuple of dictionary keys present in object :data:`xref`. Examples:

        >>> from pprint import pprint
        >>> import pymupdf
        >>> doc=pymupdf.open("pymupdf.pdf")
        >>> xref = doc.page_xref(0)  # xref of page 0
        >>> pprint(doc.xref_get_keys(xref))  # primary level keys of a page
        ('Type', 'Contents', 'Resources', 'MediaBox', 'Parent')
        >>> pprint(doc.xref_get_keys(-1))  # primary level keys of the trailer
        ('Type', 'Index', 'Size', 'W', 'Root', 'Info', 'ID', 'Length', 'Filter')
        >>>


    .. method:: xref_get_key(xref, key)
      :no-index:

      * New in v1.18.7

      PDF only: Return type and value of a PDF dictionary key of a :data:`dictionary` object given by its xref.

      :arg int xref: the :data:`xref`. *Changed in v1.18.10:* Use `-1` to access the special dictionary "PDF trailer".

      :arg str key: the desired PDF key. Must **exactly** match (case-sensitive) one of the keys contained in :meth:`Document.xref_get_keys`.

      :rtype: tuple

      :returns: A tuple (type, value) of strings, where type is one of "xref", "array", "dict", "int", "float", "null", "bool", "name", "string" or "unknown" (should not occur). Independent of "type", the value of the key is **always** formatted as a string -- see the following example -- and (almost always) a faithful reflection of what is stored in the PDF. In most cases, the format of the value string also gives a clue about the key type:

      * A "name" always starts with a "/" slash.
      * An "xref" always ends with " 0 R".
      * An "array" is always enclosed in "[...]" brackets.
      * A "dict" is always enclosed in "<<...>>" brackets.
      * A "bool", resp. "null" always equal either "true", "false", resp. "null".
      * "float" and "int" are represented by their string format -- and are thus not always distinguishable.
      * A "string" is converted to UTF-8 and may therefore deviate from what is stored in the PDF. For example, the PDF key "Author" may have a value of "<FEFF004A006F0072006A00200058002E0020004D0063004B00690065>" in the file, but the method will return `('string', 'Jorj X. McKie')`.

        >>> for key in doc.xref_get_keys(xref):
                print(key, "=" , doc.xref_get_key(xref, key))
        Type = ('name', '/Page')
        Contents = ('xref', '1297 0 R')
        Resources = ('xref', '1296 0 R')
        MediaBox = ('array', '[0 0 612 792]')
        Parent = ('xref', '1301 0 R')
        >>> #
        >>> # Now same thing for the PDF trailer.
        >>> # It has no xref, so -1 must be used instead.
        >>> #
        >>> for key in doc.xref_get_keys(-1):
                print(key, "=", doc.xref_get_key(-1, key))
        Type = ('name', '/XRef')
        Index = ('array', '[0 8802]')
        Size = ('int', '8802')
        W = ('array', '[1 3 1]')
        Root = ('xref', '8799 0 R')
        Info = ('xref', '8800 0 R')
        ID = ('array', '[<DC9D56A6277EFFD82084E64F9441E18C><DC9D56A6277EFFD82084E64F9441E18C>]')
        Length = ('int', '21111')
        Filter = ('name', '/FlateDecode')
        >>>


    .. method:: xref_set_key(xref, key, value)
      :no-index:

      * New in v1.18.7, changed in v 1.18.13
      * Changed in v1.19.4: remove a key "physically" if set to "null".

      PDF only: Set (add, update, delete) the value of a PDF key for the :data:`dictionary` object given by its xref.

      .. caution:: This is an expert function: if you do not know what you are doing, there is a high risk to render (parts of) the PDF unusable. Please do consult :ref:`AdobeManual` about object specification formats (page 18) and the structure of special dictionary types like page objects.

      :arg int xref: the :data:`xref`. *Changed in v1.18.13:* To update the PDF trailer, specify -1.
      :arg str key: the desired PDF key (without leading "/"). Must not be empty. Any valid PDF key -- whether already present in the object (which will be overwritten) -- or new. It is possible to use PDF path notation like `"Resources/ExtGState"` -- which sets the value for key `"/ExtGState"` as a sub-object of `"/Resources"`.
      :arg str value: the value for the key. It must be a non-empty string and, depending on the desired PDF object type, the following rules must be observed. There is some syntax checking, but **no type checking** and no checking if it makes sense PDF-wise, i.e. **no semantics checking**. Upper / lower case is important!

      * **xref** -- must be provided as `"nnn 0 R"` with a valid :data:`xref` number nnn of the PDF. The suffix "`0 R`" is required to be recognizable as an xref by PDF applications.
      * **array** -- a string like `"[a b c d e f]"`. The brackets are required. Array items must be separated by at least one space (not commas like in Python). An empty array `"[]"` is possible and *equivalent* to removing the key. Array items may be any PDF objects, like dictionaries, xrefs, other arrays, etc. Like in Python, array items may be of different types.
      * **dict** -- a string like `"<< ... >>"`. The brackets are required and must enclose a valid PDF dictionary definition. The empty dictionary `"<<>>"` is possible and *equivalent* to removing the key.
      * **int** -- an integer formatted **as a string**.
      * **float** -- a float formatted **as a string**. Scientific notation (with exponents) is **not allowed by PDF**.
      * **null** -- the string `"null"`. This is the PDF equivalent to Python's `None` and causes the key to be ignored -- however not necessarily removed, resp. removed on saves with garbage collection. *Changed in v1.19.4:* If the key is no path hierarchy (i.e. contains no slash "/"), then it will be completely removed.
      * **bool** -- one of the strings `"true"` or `"false"`.
      * **name** -- a valid PDF name with a leading slash like this: `"/PageLayout"`. See page 16 of the :ref:`AdobeManual`.
      * **string** -- a valid PDF string. **All PDF strings must be enclosed by brackets**. Denote the empty string as `"()"`. Depending on its content, the possible brackets are

        - "(...)" for ASCII-only text. Reserved PDF characters must be backslash-escaped and non-ASCII characters must be provided as 3-digit backslash-escaped octals -- including leading zeros. Example: 12 = 0x0C must be encoded as `\014`.
        - "<...>" for hex-encoded text. Every character must be represented by two hex-digits (lower or upper case).

        - If in doubt, we **strongly recommend** to use :meth:`get_pdf_str`! This function automatically generates the right brackets, escapes, and overall format. It will for example do conversions like these:

          >>> # because of the € symbol, the following yields UTF-16BE BOM
          >>> pymupdf.get_pdf_str("Pay in $ or €.")
          '<feff00500061007900200069006e002000240020006f0072002020ac002e>'
          >>> # escapes for brackets and non-ASCII
          >>> pymupdf.get_pdf_str("Prices in EUR (USD also accepted). Areas are in m².")
          '(Prices in EUR \\(USD also accepted\\). Areas are in m\\262.)'


    .. method:: get_page_pixmap(pno: int, *, matrix: matrix_like = Identity, dpi=None, colorspace: Colorspace = csRGB, clip: rect_like = None, alpha: bool = False, annots: bool = True)
      :no-index:

      Creates a pixmap from page *pno* (zero-based). Invokes :meth:`Page.get_pixmap`.

      All parameters except `pno` are *keyword-only.*

      :arg int pno: page number, 0-based in `-∞ < pno < page_count`.

      :rtype: :ref:`Pixmap`

    .. method:: get_page_xobjects(pno)
      :no-index:

      * New in v1.16.13
      * Changed in v1.18.11

      PDF only: Return a list of all XObjects referenced by a page.

      :arg int pno: page number, 0-based, `-∞ < pno < page_count`.

      :rtype: list
      :returns: a list of (non-image) XObjects. These objects typically represent pages *embedded* (not copied) from other PDFs. For example, :meth:`Page.show_pdf_page` will create this type of object. An item of this list has the following layout: `(xref, name, invoker, bbox)`, where

        * **xref** (*int*) is the XObject's :data:`xref`.
        * **name** (*str*) is the symbolic name to reference the XObject.
        * **invoker** (*int*) the :data:`xref` of the invoking XObject or zero if the page directly invokes it.
        * **bbox** (:ref:`Rect`) the boundary box of the XObject's location on the page **in untransformed coordinates**. To get actual, non-rotated page coordinates, multiply with the page's transformation matrix :attr:`Page.transformation_matrix`. *Changed in v.18.11:* the bbox is now formatted as :ref:`Rect`.


    .. method:: get_page_images(pno, full=False)
      :no-index:

      PDF only: Return a list of all images (directly or indirectly) referenced by the page.

      :arg int pno: page number, 0-based, `-∞ < pno < page_count`.
      :arg bool full: whether to also include the referencer's :data:`xref` (which is zero if this is the page).

      :rtype: list

      :returns: a list of images **referenced** by this page. Each item looks like

          `(xref, smask, width, height, bpc, colorspace, alt_colorspace, name, filter, referencer)`

          Where

            * **xref** (*int*) is the image object number
            * **smask** (*int*) is the object number of its soft-mask image
            * **width** (*int*) is the image width
            * **height** (*int*) is the image height
            * **bpc** (*int*) denotes the number of bits per component (normally 8)
            * **colorspace** (*str*) a string naming the colorspace (like **DeviceRGB**)
            * **alt_colorspace** (*str*) is any alternate colorspace depending on the value of **colorspace**
            * **name** (*str*) is the symbolic name by which the image is referenced
            * **filter** (*str*) is the decode filter of the image (:ref:`AdobeManual`, pp. 22).
            * **referencer** (*int*) the :data:`xref` of the referencer. Zero if directly referenced by the page. Only present if *full=True*.

      .. note:: In general, this is not the list of images that are **actually displayed**. This method only parses several PDF objects to collect references to embedded images. It does not analyse the page's :data:`contents`, where all the actual image display commands are defined. To get this information, please use :meth:`Page.get_image_info`. Also have a look at the discussion in section :ref:`textpagedict`.


    .. method:: get_page_fonts(pno, full=False)
      :no-index:

      PDF only: Return a list of all fonts (directly or indirectly) referenced by the page.

      :arg int pno: page number, 0-based, `-∞ < pno < page_count`.
      :arg bool full: whether to also include the referencer's :data:`xref`. If *True*, the returned items are one entry longer. Use this option if you need to know, whether the page directly references the font. In this case the last entry is 0. If the font is referenced by an `/XObject` of the page, you will find its :data:`xref` here.

      :rtype: list

      :returns: a list of fonts referenced by this page. Each entry looks like

      **(xref, ext, type, basefont, name, encoding, referencer)**,

      where

          * **xref** (*int*) is the font object number (may be zero if the PDF uses one of the builtin fonts directly)
          * **ext** (*str*) font file extension (e.g. "ttf", see :ref:`FontExtensions`)
          * **type** (*str*) is the font type (like "Type1" or "TrueType" etc.)
          * **basefont** (*str*) is the base font name,
          * **name** (*str*) is the symbolic name, by which the font is referenced
          * **encoding** (*str*) the font's character encoding if different from its built-in encoding (:ref:`AdobeManual`, p. 254):
          * **referencer** (*int* optional) the :data:`xref` of the referencer. Zero if directly referenced by the page, otherwise the xref of an XObject. Only present if *full=True*.

      Example::

          >>> pprint(doc.get_page_fonts(0, full=False))
          [(12, 'ttf', 'TrueType', 'FNUUTH+Calibri-Bold', 'R8', ''),
          (13, 'ttf', 'TrueType', 'DOKBTG+Calibri', 'R10', ''),
          (14, 'ttf', 'TrueType', 'NOHSJV+Calibri-Light', 'R12', ''),
          (15, 'ttf', 'TrueType', 'NZNDCL+CourierNewPSMT', 'R14', ''),
          (16, 'ttf', 'Type0', 'MNCSJY+SymbolMT', 'R17', 'Identity-H'),
          (17, 'cff', 'Type1', 'UAEUYH+Helvetica', 'R20', 'WinAnsiEncoding'),
          (18, 'ttf', 'Type0', 'ECPLRU+Calibri', 'R23', 'Identity-H'),
          (19, 'ttf', 'Type0', 'TONAYT+CourierNewPSMT', 'R27', 'Identity-H')]

      .. note::
          * This list has no duplicate entries: the combination of :data:`xref`, *name* and *referencer* is unique.
          * In general, this is a superset of the fonts actually in use by this page. The PDF creator may e.g. have specified some global list, of which each page only makes partial use.

    .. method:: get_page_text(pno, output="text", flags=3, textpage=None, sort=False)
      :no-index:

      Extracts the text of a page given its page number *pno* (zero-based). Invokes :meth:`Page.get_text`.

      :arg int pno: page number, 0-based, any value `-∞ < pno < page_count`.

      For other parameter refer to the page method.

      :rtype: str

    .. method:: layout(rect=None, width=0, height=0, fontsize=11)
      :no-index:

      Re-paginate ("reflow") the document based on the given page dimension and fontsize. This only affects some document types like e-books and HTML. Ignored if not supported. Supported documents have *True* in property :attr:`is_reflowable`.

      :arg rect_like rect: desired page size. Must be finite, not empty and start at point (0, 0).
      :arg float width: use it together with *height* as alternative to *rect*.
      :arg float height: use it together with *width* as alternative to *rect*.
      :arg float fontsize: the desired default fontsize.

    .. method:: select(s)
      :no-index:

      PDF only: Keeps only those pages of the document whose numbers occur in the list. Empty sequences or elements outside `range(doc.page_count)` will cause a *ValueError*. For more details see remarks at the bottom or this chapter.

      :arg sequence s: The sequence (see :ref:`SequenceTypes`) of page numbers (zero-based) to be included. Pages not in the sequence will be deleted (from memory) and become unavailable until the document is reopened. **Page numbers can occur multiple times and in any order:** the resulting document will reflect the sequence exactly as specified.

      .. note::

          * Page numbers in the sequence need not be unique nor be in any particular order. This makes the method a versatile utility to e.g. select only the even or the odd pages or meeting some other criteria and so forth.

          * On a technical level, the method will always create a new :data:`pagetree`.

          * When dealing with only a few pages, methods :meth:`copy_page`, :meth:`move_page`, :meth:`delete_page` are easier to use. In fact, they are also **much faster** -- by at least one order of magnitude when the document has many pages.


    .. method:: set_metadata(m)
      :no-index:

      PDF only: Sets or updates the metadata of the document as specified in *m*, a Python dictionary.

      :arg dict m: A dictionary with the same keys as *metadata* (see below). All keys are optional. A PDF's format and encryption method cannot be set or changed and will be ignored. If any value should not contain data, do not specify its key or set the value to `None`. If you use *{}* all metadata information will be cleared to the string *"none"*. If you want to selectively change only some values, modify a copy of *doc.metadata* and use it as the argument. Arbitrary unicode values are possible if specified as UTF-8-encoded.

      *(Changed in v1.18.4)* Empty values or "none" are no longer written, but completely omitted.

    .. method:: get_xml_metadata()
      :no-index:

      PDF only: Get the document XML metadata.

      :rtype: str
      :returns: XML metadata of the document. Empty string if not present or not a PDF.

    .. method:: set_xml_metadata(xml)
      :no-index:

      PDF only: Sets or updates XML metadata of the document.

      :arg str xml: the new XML metadata. Should be XML syntax, however no checking is done by this method and any string is accepted.


    .. method:: set_pagelayout(value)
      :no-index:

      * New in v1.22.2

      PDF only: Set the `/PageLayout`.

      :arg str value: one of the strings "SinglePage", "OneColumn", "TwoColumnLeft", "TwoColumnRight", "TwoPageLeft", "TwoPageRight". Lower case is supported.


    .. method:: set_pagemode(value)
      :no-index:

      * New in v1.22.2

      PDF only: Set the `/PageMode`.

      :arg str value: one of the strings "UseNone", "UseOutlines", "UseThumbs", "FullScreen", "UseOC", "UseAttachments". Lower case is supported.


    .. method:: set_markinfo(value)
      :no-index:

      * New in v1.22.2

      PDF only: Set the `/MarkInfo` values.

      :arg dict value: a dictionary like this one: `{"Marked": False, "UserProperties": False, "Suspects": False}`. This dictionary contains information about the usage of Tagged PDF conventions. For details please see the `PDF specifications <https://opensource.adobe.com/dc-acrobat-sdk-docs/standards/pdfstandards/pdf/PDF32000_2008.pdf>`_.


    .. method:: set_toc(toc, collapse=1)
      :no-index:

      PDF only: Replaces the **complete current outline** tree (table of contents) with the one provided as the argument. After successful execution, the new outline tree can be accessed as usual via :meth:`Document.get_toc` or via :attr:`Document.outline`. Like with other output-oriented methods, changes become permanent only via :meth:`save` (incremental save supported). Internally, this method consists of the following two steps. For a demonstration see example below.

      - Step 1 deletes all existing bookmarks.

      - Step 2 creates a new TOC from the entries contained in *toc*.

      :arg sequence toc:

          A list / tuple with **all bookmark entries** that should form the new table of contents. Output variants of :meth:`get_toc` are acceptable. To completely remove the table of contents specify an empty sequence or None. Each item must be a list with the following format.

          * [lvl, title, page [, dest]] where

            - **lvl** is the hierarchy level (int > 0) of the item, which **must be 1** for the first item and at most 1 larger than the previous one.

            - **title** (str) is the title to be displayed. It is assumed to be UTF-8-encoded (relevant for multibyte code points only).

            - **page** (int) is the target page number **(attention: 1-based)**. Must be in valid range if positive. Set it to -1 if there is no target, or the target is external.

            - **dest** (optional) is a dictionary or a number. If a number, it will be interpreted as the desired height (in points) this entry should point to on the page. Use a dictionary (like the one given as output by `get_toc(False)`) for a detailed control of the bookmark's properties, see :meth:`Document.get_toc` for a description.

      :arg int collapse: *(new in v1.16.9)* controls the hierarchy level beyond which outline entries should initially show up collapsed. The default 1 will hence only display level 1, higher levels must be unfolded using the PDF viewer. To unfold everything, specify either a large integer, 0 or None.

      :rtype: int
      :returns: the number of inserted, resp. deleted items.

      Changed in v1.23.8: Destination 'to' coordinates should now be in the
      same coordinate system as those returned by `get_toc()` (internally they
      are now transformed with `page.cropbox` and `page.rotation_matrix`). So
      for example `set_toc(get_toc())` now gives unchanged destination 'to'
      coordinates.

    .. method:: outline_xref(idx)
      :no-index:

      * New in v1.17.7

      PDF only: Return the :data:`xref` of the outline item. This is mainly used for internal purposes.

      :arg int idx: index of the item in list :meth:`Document.get_toc`.

      :returns: :data:`xref`.

    .. method:: del_toc_item(idx)
      :no-index:

      * New in v1.17.7
      * Changed in v1.18.14: no longer remove the item's text, but show it grayed-out.

      PDF only: Remove this TOC item. This is a high-speed method, which **disables** the respective item, but leaves the overall TOC structure intact. Physically, the item still exists in the TOC tree, but is shown grayed-out and will no longer point to any destination.

      This also implies that you can reassign the item to a new destination using :meth:`Document.set_toc_item`, when required.

      :arg int idx: the index of the item in list :meth:`Document.get_toc`.


    .. method:: set_toc_item(idx, dest_dict=None, kind=None, pno=None, uri=None, title=None, to=None, filename=None, zoom=0)
      :no-index:

      * New in v1.17.7
      * Changed in v1.18.6

      PDF only: Changes the TOC item identified by its index. Change the item **title**, **destination**, **appearance** (color, bold, italic) or collapsing sub-items -- or to remove the item altogether.

      Use this method if you need specific changes for selected entries only and want to avoid replacing the complete TOC. This is beneficial especially when dealing with large table of contents.

      :arg int idx: the index of the entry in the list created by :meth:`Document.get_toc`.
      :arg dict dest_dict: the new destination. A dictionary like the last entry of an item in `doc.get_toc(False)`. Using this as a template is recommended. When given, **all other parameters are ignored** -- except title.
      :arg int kind: the link kind, see :ref:`linkDest Kinds`. If :data:`LINK_NONE`, then all remaining parameter will be ignored, and the TOC item will be removed -- same as :meth:`Document.del_toc_item`. If None, then only the title is modified and the remaining parameters are ignored. All other values will lead to making a new destination dictionary using the subsequent arguments.
      :arg int pno: the 1-based page number, i.e. a value 1 <= pno <= doc.page_count. Required for LINK_GOTO.
      :arg str uri: the URL text. Required for LINK_URI.
      :arg str title: the desired new title. None if no change.
      :arg point_like to: (optional) points to a coordinate on the target page. Relevant for LINK_GOTO. If omitted, a point near the page's top is chosen.
      :arg str filename: required for LINK_GOTOR and LINK_LAUNCH.
      :arg float zoom: use this zoom factor when showing the target page.

      **Example use:** Change the TOC of the SWIG manual to achieve this:

      Collapse everything below top level and show the chapter on Python support in red, bold and italic::

        >>> import pymupdf
        >>> doc=pymupdf.open("SWIGDocumentation.pdf")
        >>> toc = doc.get_toc(False)  # we need the detailed TOC
        >>> # list of level 1 indices and their titles
        >>> lvl1 = [(i, item[1]) for i, item in enumerate(toc) if item[0] == 1]
        >>> for i, title in lvl1:
                d = toc[i][3]  # get the destination dict
                d["collapse"] = True  # collapse items underneath
                if "Python" in title:  # show the 'Python' chapter
                    d["color"] = (1, 0, 0)  # in red,
                    d["bold"] = True  # bold and
                    d["italic"] = True  # italic
                doc.set_toc_item(i, dest_dict=d)  # update this toc item
        >>> doc.save("NEWSWIG.pdf",garbage=3,deflate=True)

      In the previous example, we have changed only 42 of the 1240 TOC items of the file.

    .. method:: bake(*, annots=True, widgets=True)
      :no-index:

      PDF only: Convert annotations and / or widgets to become permanent parts of the pages. The PDF **will be changed** by this method. If `widgets` is `True`, the document will also no longer be a "Form PDF".
      
      All pages will look the same, but will no longer have annotations, respectively fields. The visible parts will be converted to standard text, vector graphics or images as required.

      The method may thus be a viable **alternative for PDF-to-PDF conversions** using :meth:`Document.convert_to_pdf`.

      Please consider that annotations are complex objects and may consist of more data "underneath" their visual appearance. Examples are "Text" and "FileAttachment" annotations. When "baking in" annotations / widgets with this method, all this underlying information (attached files, comments, associated PopUp annotations, etc.) will be lost and be removed on next garbage collection.

      Use this feature for instance for :meth:`Page.show_pdf_page` (which supports neither annotations nor widgets) when the source pages should look exactly the same in the target.


      :arg bool annots: convert annotations.
      :arg bool widgets: convert fields / widgets. After execution, the document will no longer be a "Form PDF".


    .. method:: can_save_incrementally()
      :no-index:

      * New in v1.16.0

      Check whether the document can be saved incrementally. Use it to choose the right option without encountering exceptions.

    .. method:: scrub(attached_files=True, clean_pages=True, embedded_files=True, hidden_text=True, javascript=True, metadata=True, redactions=True, redact_images=0, remove_links=True, reset_fields=True, reset_responses=True, thumbnails=True, xml_metadata=True)
      :no-index:

      * New in v1.16.14

      PDF only: Remove potentially sensitive data from the PDF. This function is inspired by the similar "Sanitize" function in Adobe Acrobat products. The process is configurable by a number of options.

      :arg bool attached_files: Search for 'FileAttachment' annotations and remove the file content.
      :arg bool clean_pages: Remove any comments from page painting sources. If this option is set to *False*, then this is also done for *hidden_text* and *redactions*.
      :arg bool embedded_files: Remove embedded files.
      :arg bool hidden_text: Remove OCRed text and invisible text [#f7]_.
      :arg bool javascript: Remove JavaScript sources.
      :arg bool metadata: Remove PDF standard metadata.
      :arg bool redactions: Apply redaction annotations.
      :arg int redact_images: how to handle images if applying redactions. One of 0 (ignore), 1 (blank out overlaps) or 2 (remove).
      :arg bool remove_links: Remove all links.
      :arg bool reset_fields: Reset all form fields to their defaults.
      :arg bool reset_responses: Remove all responses from all annotations.
      :arg bool thumbnails: Remove thumbnail images from pages.
      :arg bool xml_metadata: Remove XML metadata.


    .. method:: save(outfile, garbage=0, clean=False, deflate=False, deflate_images=False, deflate_fonts=False, incremental=False, ascii=False, expand=0, linear=False, pretty=False, no_new_id=False, encryption=PDF_ENCRYPT_NONE, permissions=-1, owner_pw=None, user_pw=None, use_objstms=0)
      :no-index:

      * Changed in v1.18.7
      * Changed in v1.19.0
      * Changed in v1.24.1

      PDF only: Saves the document in its **current state**.

      :arg str,Path,fp outfile: The file path, `pathlib.Path` or file object to save to. A file object must have been created before via `open(...)` or `io.BytesIO()`. Choosing `io.BytesIO()` is similar to :meth:`Document.tobytes` below, which equals the `getvalue()` output of an internally created `io.BytesIO()`.

      :arg int garbage: Do garbage collection. Positive values exclude "incremental".

      * 0 = none
      * 1 = remove unused (unreferenced) objects.
      * 2 = in addition to 1, compact the :data:`xref` table.
      * 3 = in addition to 2, merge duplicate objects.
      * 4 = in addition to 3, check :data:`stream` objects for duplication. This may be slow because such data are typically large.

      :arg bool clean: Clean and sanitize content streams [#f1]_. Corresponds to "mutool clean -sc".

      :arg bool deflate: Deflate (compress) uncompressed streams.
      :arg bool deflate_images: *(new in v1.18.3)* Deflate (compress) uncompressed image streams [#f4]_.
      :arg bool deflate_fonts: *(new in v1.18.3)* Deflate (compress) uncompressed fontfile streams [#f4]_.

      :arg bool incremental: Only save changes to the PDF. Excludes "garbage" and "linear". Can only be used if *outfile* is a string or a `pathlib.Path` and equal to :attr:`Document.name`. Cannot be used for files that are decrypted or repaired and also in some other cases. To be sure, check :meth:`Document.can_save_incrementally`. If this is false, saving to a new file is required.

      :arg bool ascii: convert binary data to ASCII.

      :arg int expand: Decompress objects. Generates versions that can be better read by some other programs and will lead to larger files.

      * 0 = none
      * 1 = images
      * 2 = fonts
      * 255 = all

      :arg bool linear: Save a linearised version of the document. This option creates a file format for improved performance for Internet access. Excludes "incremental" and "use_objstms".

      :arg bool pretty: Prettify the document source for better readability. PDF objects will be reformatted to look like the default output of :meth:`Document.xref_object`.

      :arg bool no_new_id: Suppress the update of the file's `/ID` field. If the file happens to have no such field at all, also suppress creation of a new one. Default is `False`, so every save will lead to an updated file identification.

      :arg int permissions: *(new in v1.16.0)* Set the desired permission levels. See :ref:`PermissionCodes` for possible values. Default is granting all.

      :arg int encryption: *(new in v1.16.0)* set the desired encryption method. See :ref:`EncryptionMethods` for possible values.

      :arg str owner_pw: *(new in v1.16.0)* set the document's owner password. *(Changed in v1.18.3)* If not provided, the user password is taken if provided. The string length must not exceed 40 characters.

      :arg str user_pw: *(new in v1.16.0)* set the document's user password. The string length must not exceed 40 characters.

      :arg int use_objstms: *(new in v1.24.0)* compression option that converts eligible PDF object definitions to information that is stored in some other object's :data:`stream` data. Depending on the `deflate` parameter value, the converted object definitions will be compressed -- which can lead to very significant file size reductions.

      .. warning:: The method does not check, whether a file of that name already exists, will hence not ask for confirmation, and overwrite the file. It is your responsibility as a programmer to handle this.

      .. note::

        **File size reduction**

        1. Use the save options like `garbage=3|4, deflate=True, use_objstms=True|1`. Do not touch the default values `expand=False|0, clean=False|0, incremental=False|0, linear=False|0`.
        This is a "lossless" file size reduction. There is a convenience version of this method with these values set by default, :meth:`Document.ez_save` -- please see below. 

        2. "Lossy" file size reduction in essence must give up something with respect to images, like (a) remove all images (b) replace images by their grayscale versions (c) reduce image resolutions. Find examples in the `PyMuPDF Utilities "replace-image" folder <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/examples/replace-image>`_.

    .. method:: ez_save(*args, **kwargs)
      :no-index:

      * New in v1.18.11

      PDF only: The same as :meth:`Document.save` but with changed defaults `deflate=True, garbage=3, use_objstms=1`.

    .. method:: saveIncr()
      :no-index:

      PDF only: saves the document incrementally. This is a convenience abbreviation for *doc.save(doc.name, incremental=True, encryption=PDF_ENCRYPT_KEEP)*.

    .. note::

        Saving incrementally may be required if the document contains verified signatures which would be invalidated by saving to a new file.


    .. method:: tobytes(garbage=0, clean=False, deflate=False, deflate_images=False, deflate_fonts=False, ascii=False, expand=0, linear=False, pretty=False, no_new_id=False, encryption=PDF_ENCRYPT_NONE, permissions=-1, owner_pw=None, user_pw=None, use_objstms=0)
      :no-index:

      * Changed in v1.18.7
      * Changed in v1.19.0
      * Changed in v1.24.1

      PDF only: Writes the **current content of the document** to a bytes object instead of to a file. Obviously, you should be wary about memory requirements. The meanings of the parameters exactly equal those in :meth:`save`. Chapter :ref:`FAQ` contains an example for using this method as a pre-processor to `pdfrw <https://pypi.python.org/pypi/pdfrw/0.3>`_.

      *(Changed in v1.16.0)* for extended encryption support.

      :rtype: bytes
      :returns: a bytes object containing the complete document.

    .. method:: search_page_for(pno, text, quads=False)
      :no-index:

      Search for "text" on page number "pno". Works exactly like the corresponding :meth:`Page.search_for`. Any integer `-∞ < pno < page_count` is acceptable.

    .. method:: insert_pdf(docsrc, from_page=-1, to_page=-1, start_at=-1, rotate=-1, links=True, annots=True, widgets=True, join_duplicates=False, show_progress=0, final=1)
      :no-index:

      PDF only: Copy the page range **[from_page, to_page]** (including both) of PDF document *docsrc* into the current one. Inserts will start with page number *start_at*. Value -1 indicates default values. All pages thus copied will be rotated as specified. Links, annotations and widgets can be excluded in the target, see below. All page numbers are 0-based.

      :arg docsrc: An opened PDF *Document* which must not be the current document. However, it may refer to the same underlying file.
      :type docsrc: *Document*

      :arg int from_page: First page number in *docsrc*. Default is zero.

      :arg int to_page: Last page number in *docsrc* to copy. Defaults to last page.

      :arg int start_at: First copied page, will become page number *start_at* in the target. Default -1 appends the page range to the end. If zero, the page range will be inserted before current first page.

      :arg int rotate: All copied pages will be rotated by the provided value (degrees, integer multiple of 90).

      :arg bool links: Choose whether (internal and external) links should be included in the copy. Default is `True`. *Named* links (:data:`LINK_NAMED`) and internal links to outside the copied page range are **always excluded**. 
      
      :arg bool annots: choose whether annotations should be included in the copy.
      
      :arg bool widgets: choose whether annotations should be included in the copy. If `True` and at least one of the source pages contains form fields, the target PDF will be turned into a Form PDF (if not already being one).
      
      :arg bool join_duplicates: *(New in version 1.25.5)* Choose how to handle duplicate root field names in the source pages. This parameter is ignored if `widgets=False`.
      
        Default is ``False`` which will add unifying strings to the name of those source root fields which have a duplicate in the target. For instance, if "name" already occurs in the target, the source widget's name will be changed to "name [text]" with a suitably chosen string "text".

        If ``True``, root fields with duplicate names in source and target will be converted to so-called "Kids" of a "Parent" object (which lists all kid widgets in a PDF array). This will effectively turn those kids into instances of the "same" widget: if e.g. one of the kids is changed, then all its instances will automatically inherit this change -- no matter on which page they happen to be displayed.
      
      :arg int show_progress: *(new in v1.17.7)* specify an interval size greater zero to see progress messages on `sys.stdout`. After each interval, a message like `Inserted 30 of 47 pages.` will be printed.
      
      :arg int final: *(new in v1.18.0)* controls whether the list of already copied objects should be **dropped** after this method, default *True*. Set it to 0 except for the last one of multiple insertions from the same source PDF. This saves target file size and speeds up execution considerably.

    .. note::

      1. This is a page-based method. Document-level information of source documents is therefore mostly ignored. Examples include Optional Content, Embedded Files, `StructureElem`, table of contents, page labels, metadata, named destinations (and other named entries) and some more.

      2. If `from_page > to_page`, pages will be **copied in reverse order**. If `0 <= from_page == to_page`, then one page will be copied.

      3. `docsrc` TOC entries **will not be copied**. It is easy however, to recover a table of contents for the resulting document. Look at the examples below and at program `join.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/join-documents/join.py>`_ in the *examples* directory: it can join PDF documents and at the same time piece together respective parts of the tables of contents.

    .. method:: insert_file(infile, from_page=-1, to_page=-1, start_at=-1, rotate=-1, links=True, annots=True, show_progress=0, final=1)
      :no-index:

      * New in v1.22.0

      PDF only: Add an arbitrary supported document to the current PDF. Opens "infile" as a document, converts it to a PDF and then invokes :meth:`Document.insert_pdf`. Parameters are the same as for that method. Among other things, this features an easy way to append images as full pages to an output PDF.

      :arg multiple infile: the input document to insert. May be a filename specification as is valid for creating a :ref:`Document` or a :ref:`Pixmap`.

    .. method:: new_page(pno=-1, width=595, height=842)
      :no-index:

      PDF only: Insert an empty page.

      :arg int pno: page number in front of which the new page should be inserted. Must be in *1 < pno <= page_count*. Special values -1 and *doc.page_count* insert **after** the last page.

      :arg float width: page width.
      :arg float height: page height.

      :rtype: :ref:`Page`
      :returns: the created page object.

    .. method:: insert_page(pno, text=None, fontsize=11, width=595, height=842, fontname="helv", fontfile=None, color=None)
      :no-index:

      PDF only: Insert a new page and insert some text. Convenience function which combines :meth:`Document.new_page` and (parts of) :meth:`Page.insert_text`.

      :arg int pno: page number (0-based) **in front of which** to insert. Must be in `range(-1, doc.page_count + 1)`. Special values -1 and `doc.page_count` insert **after** the last page.

          Changed in v1.14.12
            This is now a positional parameter

      For the other parameters, please consult the aforementioned methods.

      :rtype: int
      :returns: the result of :meth:`Page.insert_text` (number of successfully inserted lines).

    .. method:: delete_page(pno=-1)
      :no-index:

      PDF only: Delete a page given by its 0-based number in `-∞ < pno < page_count - 1`.

      * Changed in v1.18.14: support Python's `del` statement.

      :arg int pno: the page to be deleted. Negative number count backwards from the end of the document (like with indices). Default is the last page.

    .. method:: delete_pages(*args, **kwds)
      :no-index:

      * Changed in v1.18.13: more flexibility specifying pages to delete.
      * Changed in v1.18.14: support Python's `del` statement.

      PDF only: Delete multiple pages given as 0-based numbers.

      **Format 1:** Use keywords. Represents the old format. A contiguous range of pages is removed.
        * "from_page": first page to delete. Zero if omitted.
        * "to_page": last page to delete. Last page in document if omitted. Must not be less then "from_page".

      **Format 2:** Two page numbers as positional parameters. Handled like Format 1.

      **Format 3:** One positional integer parameter. Equivalent to :meth:`Page.delete_page`.

      **Format 4:** One positional parameter of type *list*, *tuple* or *range()* of page numbers. The items of this sequence may be in any order and may contain duplicates.

      **Format 5:** *(New in v1.18.14)* Using the Python `del` statement and index / slice notation is now possible.

      .. note::

        *(Changed in v1.14.17, optimized in v1.17.7)* In an effort to maintain a valid PDF structure, this method and :meth:`delete_page` will also deactivate items in the table of contents which point to deleted pages. "Deactivation" here means, that the bookmark will point to nowhere and the title will be shown grayed-out by supporting PDF viewers. The overall TOC structure is left intact.

        It will also remove any **links on remaining pages** which point to a deleted one. This action may have an extended response time for documents with many pages.

        Following examples will all delete pages 500 through 519:

        * `doc.delete_pages(500, 519)`
        * `doc.delete_pages(from_page=500, to_page=519)`
        * `doc.delete_pages((500, 501, 502, ... , 519))`
        * `doc.delete_pages(range(500, 520))`
        * `del doc[500:520]`
        * `del doc[(500, 501, 502, ... , 519)]`
        * `del doc[range(500, 520)]`

        For the :ref:`AdobeManual` the above takes about 0.6 seconds, because the remaining 1290 pages must be cleaned from invalid links.

        In general, the performance of this method is dependent on the number of remaining pages -- **not** on the number of deleted pages: in the above example, **deleting all pages except** those 20, will need much less time.


    .. method:: copy_page(pno, to=-1)
      :no-index:

      PDF only: Copy a page reference within the document.

      :arg int pno: the page to be copied. Must be in range `0 <= pno < page_count`.

      :arg int to: the page number in front of which to copy. The default inserts **after** the last page.

      .. note:: Only a new **reference** to the page object will be created -- not a new page object, all copied pages will have identical attribute values, including the :attr:`Page.xref`. This implies that any changes to one of these copies will appear on all of them.

    .. method:: fullcopy_page(pno, to=-1)
      :no-index:

      * New in v1.14.17

      PDF only: Make a full copy (duplicate) of a page.

      :arg int pno: the page to be duplicated. Must be in range `0 <= pno < page_count`.

      :arg int to: the page number in front of which to copy. The default inserts **after** the last page.

      .. note::

          * In contrast to :meth:`copy_page`, this method creates a new page object (with a new :data:`xref`), which can be changed independently from the original.

          * Any Popup and "IRT" ("in response to") annotations are **not copied** to avoid potentially incorrect situations.

    .. method:: move_page(pno, to=-1)
      :no-index:

      PDF only: Move (copy and then delete original) a page within the document.

      :arg int pno: the page to be moved. Must be in range `0 <= pno < page_count`.

      :arg int to: the page number in front of which to insert the moved page. The default moves **after** the last page.


    .. method:: need_appearances(value=None)
      :no-index:

      * New in v1.17.4

      PDF only: Get or set the */NeedAppearances* property of Form PDFs. Quote: *"(Optional) A flag specifying whether to construct appearance streams and appearance dictionaries for all widget annotations in the document ... Default value: false."* This may help controlling the behavior of some readers / viewers.

      :arg bool value: set the property to this value. If omitted or `None`, inquire the current value.

      :rtype: bool
      :returns:
        * None: not a Form PDF, or property not defined.
        * True / False: the value of the property (either just set or existing for inquiries). Has no effect if no Form PDF.

    .. method:: get_sigflags()
      :no-index:

      PDF only: Return whether the document contains signature fields. This is an optional PDF property: if not present (return value -1), no conclusions can be drawn -- the PDF creator may just not have bothered using it.

      :rtype: int
      :returns:
        * -1: not a Form PDF / no signature fields recorded / no *SigFlags* found.
        * 1: at least one signature field exists.
        * 3:  contains signatures that may be invalidated if the file is saved (written) in a way that alters its previous contents, as opposed to an incremental update.

    .. method:: embfile_add(name, buffer, filename=None, ufilename=None, desc=None)
      :no-index:

      * Changed in v1.14.16: The sequence of positional parameters "name" and "buffer" has been changed to comply with the call pattern of other functions.

      PDF only: Embed a new file. All string parameters except the name may be unicode (in previous versions, only ASCII worked correctly). File contents will be compressed (where beneficial).

      :arg str name: entry identifier, **must not already exist**.
      :arg bytes,bytearray,BytesIO buffer: file contents.

        *(Changed in v1.14.13)* *io.BytesIO* is now also supported.

      :arg str filename: optional filename. Documentation only, will be set to *name* if `None`.
      :arg str ufilename: optional unicode filename. Documentation only, will be set to *filename* if `None`.
      :arg str desc: optional description. Documentation only, will be set to *name* if `None`.

      :rtype: int
      :returns: *(Changed in v1.18.13)* The method now returns the :data:`xref` of the inserted file. In addition, the file object now will be automatically given the PDF keys `/CreationDate` and `/ModDate` based on the current date-time.


    .. method:: embfile_count()
      :no-index:

      * Changed in v1.14.16: This is now a method. In previous versions, this was a property.

      PDF only: Return the number of embedded files.

    .. method:: embfile_get(item)
      :no-index:

      PDF only: Retrieve the content of embedded file by its entry number or name. If the document is not a PDF, or entry cannot be found, an exception is raised.

      :arg int,str item: index or name of entry. An integer must be in `range(embfile_count())`.

      :rtype: bytes

    .. method:: embfile_del(item)
      :no-index:

      * Changed in v1.14.16: Items can now be deleted by index, too.

      PDF only: Remove an entry from `/EmbeddedFiles`. As always, physical deletion of the embedded file content (and file space regain) will occur only when the document is saved to a new file with a suitable garbage option.

      :arg int/str item: index or name of entry.

      .. warning:: When specifying an entry name, this function will only **delete the first item** with that name. Be aware that PDFs not created with PyMuPDF may contain duplicate names. So you may want to take appropriate precautions.

    .. method:: embfile_info(item)
      :no-index:

      * Changed in v1.18.13

      PDF only: Retrieve information of an embedded file given by its number or by its name.

      :arg int/str item: index or name of entry. An integer must be in `range(embfile_count())`.

      :rtype: dict
      :returns: a dictionary with the following keys:

          * ``name`` -- (*str*) name under which this entry is stored
          * ``filename`` -- (*str*) filename
          * ``ufilename`` -- (*unicode*) filename
          * ``description`` -- (*str*) description
          * ``size`` -- (*int*) original file size
          * ``length`` -- (*int*) compressed file length
          * ``creationDate`` -- (*str*) date-time of item creation in PDF format
          * ``modDate`` -- (*str*) date-time of last change in PDF format
          * ``collection`` -- (*int*) :data:`xref` of the associated PDF portfolio item if any, else zero.
          * ``checksum`` -- (*str*) a hashcode of the stored file content as a hexadecimal string. Should be MD5 according to PDF specifications, but be prepared to see other hashing algorithms.

    .. method:: embfile_names()
      :no-index:

      PDF only: Return a list of embedded file names. The sequence of the names equals the physical sequence in the document.

      :rtype: list

    .. method:: embfile_upd(item, buffer=None, filename=None, ufilename=None, desc=None)
      :no-index:

      PDF only: Change an embedded file given its entry number or name. All parameters are optional. Letting them default leads to a no-operation.

      :arg int/str item: index or name of entry. An integer must be in `range(embfile_count())`.
      :arg bytes,bytearray,BytesIO buffer: the new file content.

        *(Changed in v1.14.13)* *io.BytesIO* is now also supported.

      :arg str filename: the new filename.
      :arg str ufilename: the new unicode filename.
      :arg str desc: the new description.

      *(Changed in v1.18.13)*  The method now returns the :data:`xref` of the file object.

      :rtype: int
      :returns: xref of the file object. Automatically, its `/ModDate` PDF key will be updated with the current date-time.


    .. method:: close()
      :no-index:

      Release objects and space allocations associated with the document. If created from a file, also closes *filename* (releasing control to the OS). Explicitly closing a document is equivalent to deleting it, `del doc`, or assigning it to something else like `doc = None`.

    .. method:: xref_object(xref, compressed=False, ascii=False)
      :no-index:

      * New in v1.16.8
      * Changed in v1.18.10

      PDF only: Return the definition source of a PDF object.

      :arg int xref: the object's :data:`xref`. *Changed in v1.18.10:* A value of `-1` returns the PDF trailer source.
      :arg bool compressed: whether to generate a compact output with no line breaks or spaces.
      :arg bool ascii: whether to ASCII-encode binary data.

      :rtype: str
      :returns: The object definition source.

    .. method:: pdf_catalog()
      :no-index:

      * New in v1.16.8

      PDF only: Return the :data:`xref` number of the PDF catalog (or root) object. Use that number with :meth:`Document.xref_object` to see its source.


    .. method:: pdf_trailer(compressed=False)
      :no-index:

      * New in v1.16.8

      PDF only: Return the trailer source of the PDF,  which is usually located at the PDF file's end. This is :meth:`Document.xref_object` with an *xref* argument of -1.


    .. method:: xref_stream(xref)
      :no-index:

      * New in v1.16.8

      PDF only: Return the **decompressed** contents of the :data:`xref` stream object.

      :arg int xref: :data:`xref` number.

      :rtype: bytes
      :returns: the (decompressed) stream of the object.

    .. method:: xref_stream_raw(xref)
      :no-index:

      * New in v1.16.8

      PDF only: Return the **unmodified** (esp. **not decompressed**) contents of the :data:`xref` stream object. Otherwise equal to :meth:`Document.xref_stream`.

      :rtype: bytes
      :returns: the (original, unmodified) stream of the object.

    .. method:: update_object(xref, obj_str, page=None)
      :no-index:

      * New in v1.16.8

      PDF only: Replace object definition of :data:`xref` with the provided string. The xref may also be new, in which case this instruction completes the object definition. If a page object is also given, its links and annotations will be reloaded afterwards.

      :arg int xref: :data:`xref` number.

      :arg str obj_str: a string containing a valid PDF object definition.

      :arg page: a page object. If provided, indicates, that annotations of this page should be refreshed (reloaded) to reflect changes incurred with links and / or annotations.
      :type page: :ref:`Page`

      :rtype: int
      :returns: zero if successful, otherwise an exception will be raised.


    .. method:: update_stream(xref, data, new=False, compress=True)
      :no-index:

      * New in v.1.16.8
      * Changed in v1.19.2: added parameter "compress"
      * Changed in v1.19.6: deprecated parameter "new". Now confirms that the object is a PDF dictionary object.

      Replace the stream of an object identified by *xref*, which must be a PDF dictionary. If the object is no :data:`stream`, it will be turned into one. The function automatically performs a compress operation ("deflate") where beneficial.

      :arg int xref: :data:`xref` number.

      :arg bytes|bytearray|BytesIO stream: the new content of the stream.

        *(Changed in v1.14.13:)* *io.BytesIO* objects are now also supported.

      :arg bool new: *deprecated* and ignored. Will be removed some time after v1.20.0.
      :arg bool compress: whether to compress the inserted stream. If `True` (default), the stream will be inserted using `/FlateDecode` compression (if beneficial), otherwise the stream will inserted as is.

      :raises ValueError: if *xref* does not represent a PDF :data:`dict`. An empty dictionary ``<<>>`` is accepted. So if you just created the xref and want to give it a stream, first execute `doc.update_object(xref, "<<>>")`, and then insert the stream data with this method.

      The method is primarily (but not exclusively) intended to manipulate streams containing PDF operator syntax (see pp. 643 of the :ref:`AdobeManual`) as it is the case for e.g. page content streams.

      If you update a contents stream, consider using save parameter *clean=True* to ensure consistency between PDF operator source and the object structure.

      Example: Let us assume that you no longer want a certain image appear on a page. This can be achieved by deleting the respective reference in its contents source(s) -- and indeed: the image will be gone after reloading the page. But the page's :data:`resources` object would still show the image as being referenced by the page. This save option will clean up any such mismatches.


    .. method:: Document.xref_copy(source, target, *, keep=None)
      :no-index:

      * New in v1.19.5

      PDF Only: Make *target* xref an exact copy of *source*. If *source* is a :data:`stream`, then these data are also copied.

      :arg int source: the source :data:`xref`. It must be an existing **dictionary** object.
      :arg int target: the target xref. Must be an existing **dictionary** object. If the xref has just been created, make sure to initialize it as a PDF dictionary with the minimum specification ``<<>>``.
      :arg list keep: an optional list of top-level keys in *target*, that should not be removed in preparation of the copy process.

      .. note::

          * This method has much in common with Python's *dict* method `copy()`.
          * Both xref numbers must represent existing dictionaries.
          * Before data is copied from *source*, all *target* dictionary keys are deleted. You can specify exceptions from this in the *keep* list. If *source* however has a same-named key, its value will still replace the target.
          * If *source* is a :data:`stream` object, then these data will also be copied over, and *target* will be converted to a stream object.
          * A typical use case is to replace or remove an existing image without using redaction annotations. Example scripts can be seen `here <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/examples/replace-image>`_.

    .. method:: Document.extract_image(xref)
      :no-index:

      PDF Only: Extract data and meta information of an image stored in the document. The output can directly be used to be stored as an image file, as input for PIL, :ref:`Pixmap` creation, etc. This method avoids using pixmaps wherever possible to present the image in its original format (e.g. as JPEG).

      :arg int xref: :data:`xref` of an image object. If this is not in `range(1, doc.xref_length())`, or the object is no image or other errors occur, `None` is returned and no exception is raised.

      :rtype: dict
      :returns: a dictionary with the following keys

        * *ext* (*str*) image type (e.g. *'jpeg'*), usable as image file extension
        * *smask* (*int*) :data:`xref` number of a stencil (/SMask) image or zero
        * *width* (*int*) image width
        * *height* (*int*) image height
        * *colorspace* (*int*) the image's *colorspace.n* number.
        * *cs-name* (*str*) the image's *colorspace.name*.
        * *xres* (*int*) resolution in x direction. Please also see :data:`resolution`.
        * *yres* (*int*) resolution in y direction. Please also see :data:`resolution`.
        * *image* (*bytes*) image data, usable as image file content

      >>> d = doc.extract_image(1373)
      >>> d
      {'ext': 'png', 'smask': 2934, 'width': 5, 'height': 629, 'colorspace': 3, 'xres': 96,
      'yres': 96, 'cs-name': 'DeviceRGB',
      'image': b'\x89PNG\r\n\x1a\n\x00\x00\x00\rIHDR\x00\x00\x00\x05\ ...'}
      >>> imgout = open(f"image.{d['ext']}", "wb")
      >>> imgout.write(d["image"])
      102
      >>> imgout.close()

      .. note:: There is a functional overlap with *pix = pymupdf.Pixmap(doc, xref)*, followed by a *pix.tobytes()*. Main differences are that extract_image, **(1)** does not always deliver PNG image formats, **(2)** is **very** much faster with non-PNG images, **(3)** usually results in much less disk storage for extracted images, **(4)** returns `None` in error cases (generates no exception). Look at the following example images within the same PDF.

        * xref 1268 is a PNG -- Comparable execution time and identical output::

            In [23]: %timeit pix = pymupdf.Pixmap(doc, 1268);pix.tobytes()
            10.8 ms ± 52.4 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
            In [24]: len(pix.tobytes())
            Out[24]: 21462

            In [25]: %timeit img = doc.extract_image(1268)
            10.8 ms ± 86 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
            In [26]: len(img["image"])
            Out[26]: 21462

        * xref 1186 is a JPEG -- :meth:`Document.extract_image` is **many times faster** and produces a **much smaller** output (2.48 MB vs. 0.35 MB)::

            In [27]: %timeit pix = pymupdf.Pixmap(doc, 1186);pix.tobytes()
            341 ms ± 2.86 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
            In [28]: len(pix.tobytes())
            Out[28]: 2599433

            In [29]: %timeit img = doc.extract_image(1186)
            15.7 µs ± 116 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
            In [30]: len(img["image"])
            Out[30]: 371177


    .. method:: Document.extract_font(xref, info_only=False, named=None)
      :no-index:

      * Changed in v1.19.4: return a dictionary if `named == True`.

      PDF Only: Return an embedded font file's data and appropriate file extension. This can be used to store the font as an external file. The method does not throw exceptions (other than via checking for PDF and valid :data:`xref`).

      :arg int xref: PDF object number of the font to extract.
      :arg bool info_only: only return font information, not the buffer. To be used for information-only purposes, avoids allocation of large buffer areas.
      :arg bool named: If true, a dictionary with the following keys is returned: 'name' (font base name), 'ext' (font file extension), 'type' (font type), 'content' (font file content).

      :rtype: tuple,dict
      :returns: a tuple `(basename, ext, type, content)`, where *ext* is a 3-byte suggested file extension (*str*), *basename* is the font's name (*str*), *type* is the font's type (e.g. "Type1") and *content* is a bytes object containing the font file's content (or *b""*). For possible extension values and their meaning see :ref:`FontExtensions`. Return details on error:

            * `("", "", "", b"")` -- invalid xref or xref is not a (valid) font object.
            * `(basename, "n/a", "Type1", b"")` -- *basename* is not embedded and thus cannot be extracted. This is the case for e.g. the :ref:`Base-14-Fonts` and Type 3 fonts.

      Example:

      >>> # store font as an external file
      >>> name, ext, _, content = doc.extract_font(4711)
      >>> # assuming content is not None:
      >>> ofile = open(name + "." + ext, "wb")
      >>> ofile.write(content)
      >>> ofile.close()

      .. warning:: The basename is returned unchanged from the PDF. So it may contain characters (such as blanks) which may disqualify it as a filename for your operating system. Take appropriate action.

      .. note::
        * The returned *basename* in general is **not** the original file name, but it probably has some similarity.
        * If parameter `named == True`, a dictionary with the following keys is returned: `{'name': 'T1', 'ext': 'n/a', 'type': 'Type3', 'content': b''}`.


    .. method:: xref_xml_metadata()
      :no-index:

      * New in v1.16.8

      PDF only: Return the :data:`xref` of the document's XML metadata.


    .. method:: has_links()
      :no-index:

    .. method:: has_annots()
      :no-index:

      * New in v1.18.7

      PDF only: Check whether there are links, resp. annotations anywhere in the document.

      :returns: *True* / *False*. As opposed to fields, which are also stored in a central place of a PDF document, the existence of links / annotations can only be detected by parsing each page. These methods are tuned to do this efficiently and will immediately return, if the answer is *True* for a page. For PDFs with many thousand pages however, an answer may take some time [#f6]_ if no link, resp. no annotation is found.


    .. method:: subset_fonts(verbose=False, fallback=False)
      :no-index:

      PDF only: Investigate eligible fonts for their use by text in the document. If a font is supported and a size reduction is possible, that font is replaced by a version with a subset of its characters.

      Use this method immediately before saving the document.

      :arg bool verbose: write various progress information to sysout. This currently only has an effect if `fallback` is `True`.
      :arg bool fallback: if `True` use the deprecated algorithm that makes use of package `fontTools <https://pypi.org/project/fonttools/>`_ (which hence must be installed). If using the recommended value `False` (default), MuPDF's native function is used -- which is **very much faster** and can subset a broader range of font types. Package fontTools is not required then.

      The greatest benefit can be achieved when creating new PDFs using large fonts like is typical for Asian scripts. When using the :ref:`Story` class or method :meth:`Page.insert_htmlbox`, multiple fonts may automatically be included -- without the programmer becoming aware of it.
      
      In all these cases, the set of actually used unicodes mostly is very small compared to the number of glyphs available in the used fonts. Using this method can easily reduce the embedded font binaries by two orders of magnitude -- from several megabytes down to a low two-digit kilobyte amount.

      Creating font subsets leaves behind a large number of large, now unused PDF objects ("ghosts"). Therefore, make sure to compress and garbage-collect when saving the file. We recommend to use :meth:`Document.ez_save`.

      |history_begin|

      * New in v1.18.7
      * Changed in v1.18.9
      * Changed in v1.24.2 use native function of MuPDF.

      |history_end|


    .. method:: journal_enable()
      :no-index:

      * New in v1.19.0

      PDF only: Enable journalling. Use this before you start logging operations.

    .. method:: journal_start_op(name)
      :no-index:

      * New in v1.19.0

      PDF only: Start journalling an *"operation"* identified by a string "name". Updates will fail for a journal-enabled PDF, if no operation has been started.


    .. method:: journal_stop_op()
      :no-index:

      * New in v1.19.0

      PDF only: Stop the current operation. The updates between start and stop of an operation belong to the same unit of work and will be undone / redone together.


    .. method:: journal_position()
      :no-index:

      * New in v1.19.0

      PDF only: Return the numbers of the current operation and the total operation count.

      :returns: a tuple `(step, steps)` containing the current operation number and the total number of operations in the journal. If **step** is 0, we are at the top of the journal. If **step** equals **steps**, we are at the bottom. Updating the PDF with anything other than undo or redo will automatically remove all journal entries after the current one and the new update will become the new last entry in the journal. The updates corresponding to the removed journal entries will be permanently lost.


    .. method:: journal_op_name(step)
      :no-index:

      * New in v1.19.0

      PDF only: Return the name of operation number *step.*


    .. method:: journal_can_do()
      :no-index:

      * New in v1.19.0

      PDF only: Show whether forward ("redo") and / or backward ("undo") executions are possible from the current journal position.

      :returns: a dictionary `{"undo": bool, "redo": bool}`. The respective method is available if its value is `True`.


    .. method:: journal_undo()
      :no-index:

      * New in v1.19.0

      PDF only: Revert (undo) the current step in the journal. This moves towards the journal's top.


    .. method:: journal_redo()
      :no-index:

      * New in v1.19.0

      PDF only: Re-apply (redo) the current step in the journal. This moves towards the journal's bottom.


    .. method:: journal_save(filename)
      :no-index:

      * New in v1.19.0

      PDF only: Save the journal to a file.

      :arg str,fp filename: either a filename as string or a file object opened as "wb" (or an `io.BytesIO()` object).


    .. method:: journal_load(filename)
      :no-index:

      * New in v1.19.0

      PDF only: Load journal from a file. Enables journalling for the document. If journalling is already enabled, an exception is raised.

      :arg str,fp filename: the filename (str) of the journal or a file object opened as "rb" (or an `io.BytesIO()` object).


    .. method:: save_snapshot()
      :no-index:

      * New in v1.19.0

      PDF only: Saves a "snapshot" of the document. This is a PDF document with a special, incremental-save format compatible with journalling -- therefore no save options are available. Saving a snapshot is not possible for new documents.

      This is a normal PDF document with no usage restrictions whatsoever. If it is not being changed in any way, it can be used together with its journal to undo / redo operations or continue updating.


    .. attribute:: outline
      :no-index:

      Contains the first :ref:`Outline` entry of the document (or `None`). Can be used as a starting point to walk through all outline items. Accessing this property for encrypted, not authenticated documents will raise an *AttributeError*.

      :type: :ref:`Outline`

    .. attribute:: is_closed
      :no-index:

      *False* if document is still open. If closed, most other attributes and methods will have been deleted / disabled. In addition, :ref:`Page` objects referring to this document (i.e. created with :meth:`Document.load_page`) and their dependent objects will no longer be usable. For reference purposes, :attr:`Document.name` still exists and will contain the filename of the original document (if applicable).

      :type: bool

    .. attribute:: is_dirty
      :no-index:

      *True* if this is a PDF document and contains unsaved changes, else *False*.

      :type: bool

    .. attribute:: is_pdf
      :no-index:

      *True* if this is a PDF document, else *False*.

      :type: bool

    .. attribute:: is_form_pdf
      :no-index:

      *False* if this is not a PDF or has no form fields, otherwise the number of root form fields (fields with no ancestors).

      *(Changed in v1.16.4)* Returns the total number of (root) form fields.

      :type: bool,int

    .. attribute:: is_reflowable
      :no-index:

      *True* if document has a variable page layout (like e-books or HTML). In this case you can set the desired page dimensions during document creation (open) or via method :meth:`layout`.

      :type: bool

    .. attribute:: is_repaired
      :no-index:

      * New in v1.18.2

      *True* if PDF has been repaired during open (because of major structure issues). Always *False* for non-PDF documents. If true, more details have been stored in `TOOLS.mupdf_warnings()`, and :meth:`Document.can_save_incrementally` will return *False*.

      :type: bool

    .. attribute:: is_fast_webaccess
      :no-index:

      * New in v1.22.2

      *True* if PDF is in linearized format. *False* for non-PDF documents.

      :type: bool

    .. attribute:: markinfo
      :no-index:

      * New in v1.22.2

      A dictionary indicating the `/MarkInfo` value. If not specified, the empty dictionary is returned. If not a PDF, `None` is returned.

      :type: dict

    .. attribute:: pagemode
      :no-index:

      * New in v1.22.2

      A string containing the `/PageMode` value. If not specified, the default "UseNone" is returned. If not a PDF, `None` is returned.

      :type: str

    .. attribute:: pagelayout
      :no-index:

      * New in v1.22.2

      A string containing the `/PageLayout` value. If not specified, the default "SinglePage" is returned. If not a PDF, `None` is returned.

      :type: str

    .. attribute:: version_count
      :no-index:

      * New in v1.22.2

      An integer counting the number of versions present in the document. Zero if not a PDF, otherwise the number of incremental saves plus one.

      :type: int

    .. attribute:: needs_pass
      :no-index:

      Indicates whether the document is password-protected against access. This indicator remains unchanged -- **even after the document has been authenticated**. Precludes incremental saves if true.

      :type: bool

    .. attribute:: is_encrypted
      :no-index:

      This indicator initially equals :attr:`Document.needs_pass`. After successful authentication, it is set to *False* to reflect the situation.

      :type: bool

    .. attribute:: permissions
      :no-index:

      * Changed in v1.16.0: This is now an integer comprised of bit indicators. Was a dictionary previously.

      Contains the permissions to access the document. This is an integer containing bool values in respective bit positions. For example, if *doc.permissions & pymupdf.PDF_PERM_MODIFY > 0*, you may change the document. See :ref:`PermissionCodes` for details.

      :type: int

    .. attribute:: metadata
      :no-index:

      Contains the document's meta data as a Python dictionary or `None` (if *is_encrypted=True* and *needPass=True*). Keys are *format*, *encryption*, *title*, *author*, *subject*, *keywords*, *creator*, *producer*, *creationDate*, *modDate*, *trapped*. All item values are strings or `None`.

      Except *format* and *encryption*, for PDF documents, the key names correspond in an obvious way to the PDF keys */Creator*, */Producer*, */CreationDate*, */ModDate*, */Title*, */Author*, */Subject*, */Trapped* and */Keywords* respectively.

      - *format* contains the document format (e.g. 'PDF-1.6', 'XPS', 'EPUB').

      - *encryption* either contains `None` (no encryption), or a string naming an encryption method (e.g. *'Standard V4 R4 128-bit RC4'*). Note that an encryption method may be specified **even if** *needs_pass=False*. In such cases not all permissions will probably have been granted. Check :attr:`Document.permissions` for details.

      - If the date fields contain valid data (which need not be the case at all!), they are strings in the PDF-specific timestamp format "D:<TS><TZ>", where

          - <TS> is the 12 character ISO timestamp *YYYYMMDDhhmmss* (*YYYY* - year, *MM* - month, *DD* - day, *hh* - hour, *mm* - minute, *ss* - second), and

          - <TZ> is a time zone value (time interval relative to GMT) containing a sign ('+' or '-'), the hour (*hh*), and the minute (*'mm'*, note the apostrophes!).

      - A Paraguayan value might hence look like *D:20150415131602-04'00'*, which corresponds to the timestamp April 15, 2015, at 1:16:02 pm local time Asuncion.

      :type: dict

    .. Attribute:: name
      :no-index:

      Contains the *filename* or *filetype* value with which *Document* was created.

      :type: str

    .. Attribute:: page_count
      :no-index:

      Contains the number of pages of the document. May return 0 for documents with no pages. Function `len(doc)` will also deliver this result.

      :type: int

    .. Attribute:: chapter_count
      :no-index:

      * New in v1.17.0

      Contains the number of chapters in the document. Always at least 1. Relevant only for document types with chapter support (EPUB currently). Other documents will return 1.

      :type: int

    .. Attribute:: last_location
      :no-index:

      * New in v1.17.0

      Contains (chapter, pno) of the document's last page. Relevant only for document types with chapter support (EPUB currently). Other documents will return `(0, page_count - 1)` and `(0, -1)` if it has no pages.

      :type: int

    .. Attribute:: FormFonts
      :no-index:

      A list of form field font names defined in the */AcroForm* object. `None` if not a PDF.

      :type: list

  .. NOTE:: 
    
    For methods that change the structure of a PDF (:meth:`insert_pdf`, :meth:`select`, :meth:`copy_page`, :meth:`delete_page` and others), be aware that objects or properties in your program may have been invalidated or orphaned. Examples are :ref:`Page` objects and their children (links, annotations, widgets), variables holding old page counts, tables of content and the like. Remember to keep such variables up to date or delete orphaned objects. Also refer to :ref:`ReferenialIntegrity`.

:meth:`set_metadata` 例子
-------------------------------

:meth:`set_metadata` Example

.. tab:: 中文

  清除元数据。如果出于隐私或数据保护考虑执行此操作，请确保将文档保存为新文件，并设置 *garbage > 0*。这样，旧的 */Info* 对象也会被物理删除。在这种情况下，您可能还希望清除由多个 PDF 编辑器插入的任何 XML 元数据：

  >>> import pymupdf
  >>> doc = pymupdf.open("pymupdf.pdf")
  >>> doc.metadata             # 查看当前的元数据
  {'producer': 'rst2pdf, reportlab', 'format': 'PDF 1.4', 'encryption': None, 'author':
  'Jorj X. McKie', 'modDate': "D:20160611145816-04'00'", 'keywords': 'PDF, XPS, EPUB, CBZ',
  'title': 'The PyMuPDF Documentation', 'creationDate': "D:20160611145816-04'00'",
  'creator': 'sphinx', 'subject': 'PyMuPDF 1.9.1'}
  >>> doc.set_metadata({})      # 清除所有字段
  >>> doc.metadata             # 再次查看，验证修改结果
  {'producer': 'none', 'format': 'PDF 1.4', 'encryption': None, 'author': 'none',
  'modDate': 'none', 'keywords': 'none', 'title': 'none', 'creationDate': 'none',
  'creator': 'none', 'subject': 'none'}
  >>> doc.del_xml_metadata()    # 清除任何 XML 元数据
  >>> doc.save("anonymous.pdf", garbage = 4)       # 保存匿名化后的文档

.. tab:: 英文

  Clear metadata information. If you do this out of privacy / data protection concerns, make sure you save the document as a new file with *garbage > 0*. Only then the old */Info* object will also be physically removed from the file. In this case, you may also want to clear any XML metadata inserted by several PDF editors:

  >>> import pymupdf
  >>> doc=pymupdf.open("pymupdf.pdf")
  >>> doc.metadata             # look at what we currently have
  {'producer': 'rst2pdf, reportlab', 'format': 'PDF 1.4', 'encryption': None, 'author':
  'Jorj X. McKie', 'modDate': "D:20160611145816-04'00'", 'keywords': 'PDF, XPS, EPUB, CBZ',
  'title': 'The PyMuPDF Documentation', 'creationDate': "D:20160611145816-04'00'",
  'creator': 'sphinx', 'subject': 'PyMuPDF 1.9.1'}
  >>> doc.set_metadata({})      # clear all fields
  >>> doc.metadata             # look again to show what happened
  {'producer': 'none', 'format': 'PDF 1.4', 'encryption': None, 'author': 'none',
  'modDate': 'none', 'keywords': 'none', 'title': 'none', 'creationDate': 'none',
  'creator': 'none', 'subject': 'none'}
  >>> doc.del_xml_metadata()    # clear any XML metadata
  >>> doc.save("anonymous.pdf", garbage = 4)       # save anonymized doc

:meth:`set_toc` 演示
----------------------------------

:meth:`set_toc` Demonstration

.. tab:: 中文

  这展示了如何修改或添加目录。也可以查看 `import.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/import-toc/import.py>`_ 和 `export.py <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/examples/export-toc/export.py>`_ 示例目录中的示例。

  >>> import pymupdf
  >>> doc = pymupdf.open("test.pdf")
  >>> toc = doc.get_toc()
  >>> for t in toc: print(t)                           # 显示当前目录
  [1, 'The PyMuPDF Documentation', 1]
  [2, 'Introduction', 1]
  [3, 'Note on the Name fitz', 1]
  [3, 'License', 1]
  >>> toc[1][1] += " modified by set_toc"               # 修改内容
  >>> doc.set_toc(toc)                                  # 替换目录树
  3                                                    # 插入了 3 个书签
  >>> for t in doc.get_toc(): print(t)                  # 验证修改是否成功
  [1, 'The PyMuPDF Documentation', 1]
  [2, 'Introduction modified by set_toc', 1]            # <<< 这里已更改
  [3, 'Note on the Name fitz', 1]
  [3, 'License', 1]


.. tab:: 英文

  This shows how to modify or add a table of contents. Also have a look at `import.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/import-toc/import.py>`_ and `export.py <https://github.com/pymupdf/PyMuPDF-Utilities/tree/master/examples/export-toc/export.py>`_ in the examples directory.

  >>> import pymupdf
  >>> doc = pymupdf.open("test.pdf")
  >>> toc = doc.get_toc()
  >>> for t in toc: print(t)                           # show what we have
  [1, 'The PyMuPDF Documentation', 1]
  [2, 'Introduction', 1]
  [3, 'Note on the Name fitz', 1]
  [3, 'License', 1]
  >>> toc[1][1] += " modified by set_toc"               # modify something
  >>> doc.set_toc(toc)                                  # replace outline tree
  3                                                    # number of bookmarks inserted
  >>> for t in doc.get_toc(): print(t)                  # demonstrate it worked
  [1, 'The PyMuPDF Documentation', 1]
  [2, 'Introduction modified by set_toc', 1]            # <<< this has changed
  [3, 'Note on the Name fitz', 1]
  [3, 'License', 1]

:meth:`insert_pdf` 例子
----------------------------

:meth:`insert_pdf` Examples

.. tab:: 中文

  **(1) 合并两个文档并包括它们的目录（TOC）：**

  >>> doc1 = pymupdf.open("file1.pdf")          # 必须是 PDF 文件
  >>> doc2 = pymupdf.open("file2.pdf")          # 必须是 PDF 文件
  >>> pages1 = len(doc1)                        # 保存 doc1 的页面数
  >>> toc1 = doc1.get_toc(False)                # 保存 TOC 1
  >>> toc2 = doc2.get_toc(False)                # 保存 TOC 2
  >>> doc1.insert_pdf(doc2)                     # 将 doc2 插入到 doc1 的末尾
  >>> for t in toc2:                            # 增加 toc2 页码
          t[2] += pages1                        # 增加原 doc1 的页面数
  >>> doc1.set_toc(toc1 + toc2)                # 现在结果包含总的 TOC

  显然，在更一般的情况下可以找到类似的方式。只需确保目录中的层级顺序不会增加超过一级。在 *toc2* 部分前后插入虚拟书签可以修复此类情况。一个现成的 GUI（wxPython）解决方案可以在 `join.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/join-documents/join.py>`_ 示例目录中找到。

  **(2) 更多示例：**

  >>> # 插入 doc2 的 5 页，其中 doc2 的第 21 页变为 doc1 的第 15 页
  >>> doc1.insert_pdf(doc2, from_page=21, to_page=25, start_at=15)
  >>>
  >>> # 相同示例，但页面被旋转并逆序复制
  >>> doc1.insert_pdf(doc2, from_page=25, to_page=21, start_at=15, rotate=90)
  >>>
  >>> # 将复制的页面插入到 doc1 前面
  >>> doc1.insert_pdf(doc2, from_page=21, to_page=25, start_at=0)


.. tab:: 英文

  **(1) Concatenate two documents including their TOCs:**

  >>> doc1 = pymupdf.open("file1.pdf")          # must be a PDF
  >>> doc2 = pymupdf.open("file2.pdf")          # must be a PDF
  >>> pages1 = len(doc1)                     # save doc1's page count
  >>> toc1 = doc1.get_toc(False)     # save TOC 1
  >>> toc2 = doc2.get_toc(False)     # save TOC 2
  >>> doc1.insert_pdf(doc2)                   # doc2 at end of doc1
  >>> for t in toc2:                         # increase toc2 page numbers
          t[2] += pages1                     # by old len(doc1)
  >>> doc1.set_toc(toc1 + toc2)               # now result has total TOC

  Obviously, similar ways can be found in more general situations. Just make sure that hierarchy levels in a row do not increase by more than one. Inserting dummy bookmarks before and after *toc2* segments would heal such cases. A ready-to-use GUI (wxPython) solution can be found in script `join.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/join-documents/join.py>`_ of the examples directory.

  **(2) More examples:**

  >>> # insert 5 pages of doc2, where its page 21 becomes page 15 in doc1
  >>> doc1.insert_pdf(doc2, from_page=21, to_page=25, start_at=15)

  >>> # same example, but pages are rotated and copied in reverse order
  >>> doc1.insert_pdf(doc2, from_page=25, to_page=21, start_at=15, rotate=90)

  >>> # put copied pages in front of doc1
  >>> doc1.insert_pdf(doc2, from_page=21, to_page=25, start_at=0)

其他例子
----------------

Other Examples

.. tab:: 中文

  **从 PDF 中提取所有页面引用的图像并保存为单独的 PNG 文件**::

    for i in range(doc.page_count):
        imglist = doc.get_page_images(i)
        for img in imglist:
            xref = img[0]                  # xref 编号
            pix = pymupdf.Pixmap(doc, xref)   # 从图像创建 pixmap
            if pix.n - pix.alpha < 4:      # 可以保存为 PNG 格式
                pix.save("p%s-%s.png" % (i, xref))
            else:                          # CMYK: 必须先转换
                pix0 = pymupdf.Pixmap(pymupdf.csRGB, pix)
                pix0.save("p%s-%s.png" % (i, xref))
                pix0 = None                # 释放 Pixmap 资源
            pix = None                     # 释放 Pixmap 资源

  **旋转 PDF 的所有页面：**

  >>> for page in doc: page.set_rotation(90)

.. tab:: 英文

  **Extract all page-referenced images of a PDF into separate PNG files**::

  for i in range(doc.page_count):
      imglist = doc.get_page_images(i)
      for img in imglist:
          xref = img[0]                  # xref number
          pix = pymupdf.Pixmap(doc, xref)   # make pixmap from image
          if pix.n - pix.alpha < 4:      # can be saved as PNG
              pix.save("p%s-%s.png" % (i, xref))
          else:                          # CMYK: must convert first
              pix0 = pymupdf.Pixmap(pymupdf.csRGB, pix)
              pix0.save("p%s-%s.png" % (i, xref))
              pix0 = None                # free Pixmap resources
          pix = None                     # free Pixmap resources

  **Rotate all pages of a PDF:**

  >>> for page in doc: page.set_rotation(90)

.. rubric:: Footnotes

.. [#f1] Content streams describe what (e.g. text or images) appears where and how on a page. PDF uses a specialized mini language similar to PostScript to do this (pp. 643 in :ref:`AdobeManual`), which gets interpreted when a page is loaded.

.. [#f2] However, you **can** use :meth:`Document.get_toc` and :meth:`Page.get_links` (which are available for all document types) and copy this information over to the output PDF. See demo `convert.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/convert-document/convert.py>`_.

.. [#f3] For applicable (EPUB) document types, loading a page via its absolute number may result in layouting a large part of the document, before the page can be accessed. To avoid this performance impact, prefer chapter-based access. Use convenience methods and attributes :meth:`Document.next_location`, :meth:`Document.prev_location` and :attr:`Document.last_location` for maintaining a high level of coding efficiency.

.. [#f4] These parameters cause separate handling of stream categories: use it together with `expand` to restrict decompression to streams other than images / fontfiles.

.. [#f5] Examples for "Form XObjects" are created by :meth:`Page.show_pdf_page`.

.. [#f6] For a *False* the **complete document** must be scanned. Both methods **do not load pages,** but only scan object definitions. This makes them at least 10 times faster than application-level loops (where total response time roughly equals the time for loading all pages). For the :ref:`AdobeManual` (756 pages) and the Pandas documentation (over 3070 pages) -- both have no annotations -- the method needs about 11 ms for the answer *False*. So response times will probably become significant only well beyond this order of magnitude.

.. [#f7] This only works under certain conditions. For example, if there is normal text covered by some image on top of it, then this is undetectable and the respective text is **not** removed. Similar is true for white text on white background, and so on.

.. [#f8] 
   内容流（Content Streams）描述了页面上出现的内容（如文本或图像）、位置及呈现方式。PDF 采用了一种类似于 PostScript 的专用小型语言来实现这一点（详见 :ref:`AdobeManual` 第 643 页）。该语言在页面加载时被解释执行。

.. [#f9] 
   但您 **可以** 使用 :meth:`Document.get_toc` 和 :meth:`Page.get_links` （适用于所有文档类型），并将这些信息复制到输出 PDF 中。参见示例代码：  
   `convert.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/convert-document/convert.py>`_。

.. [#f10] 
   对于适用的文档类型（如 EPUB），如果通过绝对页码加载页面，可能会导致大量文档布局计算，从而影响性能。因此，建议优先使用基于章节的访问方式。  
   可使用 :meth:`Document.next_location`、:meth:`Document.prev_location` 和 :attr:`Document.last_location` 等便捷方法和属性，提高代码执行效率。

.. [#f11] 
   这些参数可用于对不同类别的流进行单独处理：可与 `expand` 结合使用，以限制对非图像 / 字体文件流的解压缩。

.. [#f12] 
   由 :meth:`Page.show_pdf_page` 方法创建的示例包括 "Form XObjects"。

.. [#f13] 
   如果返回 *False*，则必须扫描 **整个文档**。这两种方法 **不会加载页面**，而只是扫描对象定义，因此它们比应用级循环快至少 10 倍（应用级循环的总响应时间通常等于加载所有页面的时间）。  
   例如，在 :ref:`AdobeManual` （756 页）和 Pandas 文档（超过 3070 页）这类无批注文档中，该方法的响应时间约为 11 毫秒。因此，响应时间通常仅在处理规模远超此量级时才会变得显著。

.. [#f14] 
   该方法仅在特定条件下有效。例如，如果某段正常文本被某个图像覆盖，则无法检测到该文本，因此 **不会** 被移除。同样，白色文本位于白色背景上等情况也难以识别。


.. include:: footer.rst
