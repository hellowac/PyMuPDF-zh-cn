.. include:: header.rst

.. _Module:

============================
命令行界面
============================

Command line interface

.. tab:: 中文

    * 1.16.8 版新增功能

    PyMuPDF 还可以从命令行使用，以执行实用功能。此功能应该会淘汰一些最基本的脚本。

    不可否认，它与 MuPDF CLI `mutool` 有一些功能重叠。另一方面，MuPDF 不再支持 PDF 嵌入文件，因此 PyMuPDF 在此提供了一些独特的功能。

.. tab:: 英文

    * New in version 1.16.8

    PyMuPDF can also be used from the command line to perform utility functions. This feature should obsolete writing some of the most basic scripts.

    Admittedly, there is some functional overlap with the MuPDF CLI `mutool`. On the other hand, PDF embedded files are no longer supported by MuPDF, so PyMuPDF is offering something unique here.

调用
-----------

Invocation

.. tab:: 中文

    命令行界面可以通过以下两种方式调用：

    * 使用已安装的 `pymupdf` 命令::

        pymupdf <command and parameters>

    * 或者使用 Python 的 `-m` 选项调用 PyMuPDF 的 `pymupdf` 模块::

        python -m pymupdf <command and parameters>


    .. highlight:: python

    通用说明：

    * 通过 `"-h"` 请求帮助，或使用 `"command -h"` 获取特定命令的帮助信息。
    * 在不引起歧义的情况下，参数可以缩写。
    * 若干命令支持 `-pages` 和 `-xrefs` 参数，用于筛选处理对象。请注意：

        - **页码** 在此工具中必须采用 **1-based** （从 1 开始）。
        - 有效的 :data:`xref` 号码从 1 开始。
        - 可以使用逗号分隔的列表来指定 *单个* 整数或整数 *范围* 。 **范围** 由两个整数组成，以连字符 `"-"` 连接。整数不得超过最大页码或最大 xref 号码。可以使用符号变量 `"N"` 表示最大值。整数或范围可以多次出现，顺序不限，且可以重叠。如果范围的第一个数字大于第二个数字，则会按相反顺序处理这些项目。

    * 如何在脚本中使用该模块::

        >>> import pymupdf.__main__
        >>> cmd = "clean input.pdf output.pdf -pages 1,N".split()  # 准备命令行参数
        >>> saved_parms = sys.argv[1:]  # 保存原始命令行参数
        >>> sys.argv[1:] = cmd  # 存储新命令行参数
        >>> pymupdf.__main__.main()  # 执行模块
        >>> sys.argv[1:] = saved_parms  # 还原原始命令行参数

    * 使用以下两行代码，并使用 `Nuitka <https://pypi.org/project/Nuitka/>`_ 以独立模式编译。这样可以生成一个 CLI 可执行文件，该文件可在所有兼容平台上运行，而无需安装 Python、PyMuPDF 或 MuPDF。

    ::

        from pymupdf.__main__ import main
        main()


.. tab:: 英文

    The command-line interface can be invoked in two ways.

    * Use the installed `pymupdf` command::

        pymupdf <command and parameters>

    * Or use Python's `-m` switch with PyMuPDF's `pymupdf` module::

        python -m pymupdf <command and parameters>


    .. highlight:: python

    General remarks:

    * Request help via `"-h"`, resp. command-specific help via `"command -h"`.
    * Parameters may be abbreviated where this does not introduce ambiguities.
    * Several commands support parameters `-pages` and `-xrefs`. They are intended for down-selection. Please note that:

        - **page numbers** for this utility must be given **1-based**.
        - valid :data:`xref` numbers start at 1.
        - Specify a comma-separated list of either *single* integers or integer *ranges*. A **range** is a pair of integers separated by one hyphen "-". Integers must not exceed the maximum page, resp. xref number. To specify that maximum, the symbolic variable "N" may be used. Integers or ranges may occur several times, in any sequence and may overlap. If in a range the first number is greater than the second one, the respective items will be processed in reversed order.

    * How to use the module inside your script::

        >>> import pymupdf.__main__
        >>> cmd = "clean input.pdf output.pdf -pages 1,N".split()  # prepare command line
        >>> saved_parms = sys.argv[1:]  # save original command line
        >>> sys.argv[1:] = cmd  # store new command line
        >>> pymupdf.__main__.()  # execute module
        >>> sys.argv[1:] = saved_parms  # restore original command line

    * Use the following 2-liner and compile it with `Nuitka <https://pypi.org/project/Nuitka/>`_ in standalone mode. This will give you a CLI executable with all the module's features, that can be used on all compatible platforms without Python, PyMuPDF or MuPDF being installed.

    ::

        from pymupdf.__main__ import main
        main()


清理和复制
----------------------

Cleaning and Copying

.. highlight:: text

.. tab:: 中文

    此命令可优化 PDF 并将结果存储到新文件中。您还可以使用它进行加密、解密和创建子文档。它与 MuPDF 命令行工具 *"mutool clean"* 基本类似::

        pymupdf clean -h
        usage: pymupdf clean [-h] [-password PASSWORD]
                        [-encryption {keep,none,rc4-40,rc4-128,aes-128,aes-256}]
                        [-owner OWNER] [-user USER] [-garbage {0,1,2,3,4}]
                        [-compress] [-ascii] [-linear] [-permission PERMISSION]
                        [-sanitize] [-pretty] [-pages PAGES]
                        input output

        -------------- 优化 PDF 或创建子 PDF（若提供页码参数） --------------

        位置参数:
        input                 PDF 文件名
        output                输出 PDF 文件名

        可选参数:
        -h, --help            显示帮助信息并退出
        -password PASSWORD    密码
        -encryption {keep,none,rc4-40,rc4-128,aes-128,aes-256}
                            加密方式
        -owner OWNER          所有者密码
        -user USER            用户密码
        -garbage {0,1,2,3,4}  垃圾回收级别
        -compress             对输出进行压缩（deflate）
        -ascii                以 ASCII 编码二进制数据
        -linear               以快速网页显示格式存储
        -permission PERMISSION
                            权限级别（整数）
        -sanitize             清理 / 规范化内容
        -pretty               美化 PDF 结构
        -pages PAGES          输出选定的页面，格式示例: 1,5-7,50-N

    如果指定了 `"-pages"`，请注意， **仅会复制页面相关的对象** ，不会包含文档级别的元素，例如嵌入文件等。

    有关各参数的详细说明，请参考 :meth:`Document.save`。


.. tab:: 英文

    This command will optimize the PDF and store the result in a new file. You can use it also for encryption, decryption and creating sub documents. It is mostly similar to the MuPDF command line utility *"mutool clean"*::

        pymupdf clean -h
        usage: pymupdf clean [-h] [-password PASSWORD]
                        [-encryption {keep,none,rc4-40,rc4-128,aes-128,aes-256}]
                        [-owner OWNER] [-user USER] [-garbage {0,1,2,3,4}]
                        [-compress] [-ascii] [-linear] [-permission PERMISSION]
                        [-sanitize] [-pretty] [-pages PAGES]
                        input output

        -------------- optimize PDF or create sub-PDF if pages given --------------

        positional arguments:
        input                 PDF filename
        output                output PDF filename

        optional arguments:
        -h, --help            show this help message and exit
        -password PASSWORD    password
        -encryption {keep,none,rc4-40,rc4-128,aes-128,aes-256}
                            encryption method
        -owner OWNER          owner password
        -user USER            user password
        -garbage {0,1,2,3,4}  garbage collection level
        -compress             compress (deflate) output
        -ascii                ASCII encode binary data
        -linear               format for fast web display
        -permission PERMISSION
                            integer with permission levels
        -sanitize             sanitize / clean contents
        -pretty               prettify PDF structure
        -pages PAGES          output selected pages, format: 1,5-7,50-N

    If you specify "-pages", be aware that only page-related objects are copied, **no document-level items** like e.g. embedded files.

    Please consult :meth:`Document.save` for the parameter meanings.


提取字体和图像
----------------------------

Extracting Fonts and Images

.. tab:: 中文

    从选定的 PDF 页面提取字体或图像，并保存到指定目录::

        pymupdf extract -h
        usage: pymupdf extract [-h] [-images] [-fonts] [-output OUTPUT] [-password PASSWORD]
                            [-pages PAGES]
                            input

        --------------------- 将图像和字体提取到磁盘 --------------------

        位置参数:
        input                 PDF 文件名

        可选参数:
        -h, --help            显示帮助信息并退出
        -images               提取图像
        -fonts                提取字体
        -output OUTPUT        输出目录，默认为当前目录
        -password PASSWORD    密码
        -pages PAGES          仅处理选定页面，格式示例: 1,5-7,50-N

    **图像文件名** 采用以下命名规则： **"img-xref.ext"** ，其中 `"ext"` 是图像的文件扩展名， `"xref"` 是图像 PDF 对象的 :data:`xref` 号码。

    **字体文件名** 由字体名称和相应的文件扩展名组成。字体名称中的空格会被连字符 `"-"` 代替。

    输出目录必须 **已存在** 。

    .. note:: 
        
        除了不会创建输出目录之外，该功能在 **功能上等效于** 并取代了 `此脚本 <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/extract-images/extract-from-pages.py>`_。


.. tab:: 英文

    Extract fonts or images from selected PDF pages to a desired directory::

        pymupdf extract -h
        usage: pymupdf extract [-h] [-images] [-fonts] [-output OUTPUT] [-password PASSWORD]
                            [-pages PAGES]
                            input

        --------------------- extract images and fonts to disk --------------------

        positional arguments:
        input                 PDF filename

        optional arguments:
        -h, --help            show this help message and exit
        -images               extract images
        -fonts                extract fonts
        -output OUTPUT        output directory, defaults to current
        -password PASSWORD    password
        -pages PAGES          only consider these pages, format: 1,5-7,50-N

    **Image filenames** are built according to the naming scheme: **"img-xref.ext"**, where "ext" is the extension associated with the image and "xref" the :data:`xref` of the image PDF object.

    **Font filenames** consist of the fontname and the associated extension. Any spaces in the fontname are replaced with hyphens "-".

    The output directory must already exist.

    .. note:: 
        
        Except for output directory creation, this feature is **functionally equivalent** to and obsoletes `this script <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/extract-images/extract-from-pages.py>`_.


合并 PDF 文档
-----------------------

Joining PDF Documents

.. tab:: 中文

    要合并多个 PDF 文件，请使用以下命令::

        pymupdf join -h
        usage: pymupdf join [-h] -output OUTPUT [input [input ...]]

        ---------------------------- 合并 PDF 文档 ---------------------------

        位置参数:
        input           输入文件名

        可选参数:
        -h, --help      显示帮助信息并退出
        -output OUTPUT  输出文件名

        每个输入文件的格式: 'filename[,password[,pages]]'

    .. note::

        1. 每个输入文件必须按 **"filename,password,pages"** 形式提供。`password` 和 `pages` 为可选项。
        2. 如果使用 `"pages"` 参数，则 **必须提供密码**。若 PDF 无需密码，请使用两个逗号 `",,"` 作为占位符。
        3. **"pages"** 参数的格式与本节开头说明的格式相同。
        4. 每个输入文件在使用后会立即关闭。因此，您可以将其中一个输入文件作为输出文件名，从而覆盖原文件。

    示例: 设要合并以下文件

    1. **file1.pdf:** 所有页面，逆序排列，无密码
    2. **file2.pdf:** 最后一页、第一页，密码: `"secret"`
    3. **file3.pdf:** 第 5 页至最后一页，无密码

    并将结果存储为 **output.pdf**，可使用以下命令:

    *pymupdf join -o output.pdf file1.pdf,,N-1 file2.pdf,secret,N,1 file3.pdf,,5-N*


.. tab:: 英文

    To join several PDF files specify::

        pymupdf join -h
        usage: pymupdf join [-h] -output OUTPUT [input [input ...]]

        ---------------------------- join PDF documents ---------------------------

        positional arguments:
        input           input filenames

        optional arguments:
        -h, --help      show this help message and exit
        -output OUTPUT  output filename

        specify each input as 'filename[,password[,pages]]'


    .. note::

        1. Each input must be entered as **"filename,password,pages"**. Password and pages are optional.
        2. The password entry **is required** if the "pages" entry is used. If the PDF needs no password, specify two commas.
        3. The **"pages"** format is the same as explained at the top of this section.
        4. Each input file is immediately closed after use. Therefore you can use one of them as output filename, and thus overwrite it.


    Example: To join the following files

    1. **file1.pdf:** all pages, back to front, no password
    2. **file2.pdf:** last page, first page, password: "secret"
    3. **file3.pdf:** pages 5 to last, no password

    and store the result as **output.pdf** enter this command:

    *pymupdf join -o output.pdf file1.pdf,,N-1 file2.pdf,secret,N,1 file3.pdf,,5-N*


低级信息
----------------------

Low Level Information

.. tab:: 中文

    显示 PDF 内部信息。此命令与 *"mutool show"* 具有相似功能::

        pymupdf show -h
        usage: pymupdf show [-h] [-password PASSWORD] [-catalog] [-trailer] [-metadata]
                        [-xrefs XREFS] [-pages PAGES]
                        input

        ------------------------- 显示 PDF 信息 -------------------------

        位置参数:
        input               PDF 文件名

        可选参数:
        -h, --help          显示帮助信息并退出
        -password PASSWORD  密码
        -catalog            显示 PDF 目录（Catalog）
        -trailer            显示 PDF 尾部信息（Trailer）
        -metadata           显示 PDF 元数据（Metadata）
        -xrefs XREFS        显示选定对象，格式示例: 1,5-7,N
        -pages PAGES        显示选定页面，格式示例: 1,5-7,50-N

    示例::

        pymupdf show x.pdf
        PDF 受密码保护

        pymupdf show x.pdf -pass hugo
        认证失败

        pymupdf show x.pdf -pass jorjmckie
        认证为所有者
        文件 'x.pdf'，页数: 1，对象数: 19，大小: 58 MB，PDF 版本: 1.4，加密方式: Standard V5 R6 256-bit AES
        文档包含 15 个嵌入文件。

        pymupdf show FDA-1572_508_R6_FINAL.pdf -tr -m
        'FDA-1572_508_R6_FINAL.pdf'，页数: 2，对象数: 1645，大小: 1.4 MB，PDF 版本: 1.6，加密方式: Standard V4 R4 128-bit AES
        文档包含 740 个根表单字段，并已签名

        ------------------------------- PDF 元数据 ------------------------------
            格式: PDF 1.6
            标题: FORM FDA 1572
            作者: PSC Publishing Services
            主题: Statement of Investigator
            关键字: None
            创建者: PScript5.dll Version 5.2.2
            生成器: Acrobat Distiller 9.0.0 (Windows)
        创建日期: D:20130522104413-04'00'
        修改日期: D:20190718154905-07'00'
        加密方式: Standard V4 R4 128-bit AES

        ------------------------------- PDF 尾部信息 -------------------------------
        <<
        /DecodeParms <<
            /Columns 5
            /Predictor 12
        >>
        /Encrypt 1389 0 R
        /Filter /FlateDecode
        /ID [ <9252E9E39183F2A0B0C51BE557B8A8FC> <85227BE9B84B724E8F678E1529BA8351> ]
        /Index [ 1388 258 ]
        /Info 1387 0 R
        /Length 253
        /Prev 1510559
        /Root 1390 0 R
        /Size 1646
        /Type /XRef
        /W [ 1 3 1 ]
        >>


.. tab:: 英文

    Display PDF internal information. Again, there are similarities to *"mutool show"*::

        pymupdf show -h
        usage: pymupdf show [-h] [-password PASSWORD] [-catalog] [-trailer] [-metadata]
                        [-xrefs XREFS] [-pages PAGES]
                        input

        ------------------------- display PDF information -------------------------

        positional arguments:
        input               PDF filename

        optional arguments:
        -h, --help          show this help message and exit
        -password PASSWORD  password
        -catalog            show PDF catalog
        -trailer            show PDF trailer
        -metadata           show PDF metadata
        -xrefs XREFS        show selected objects, format: 1,5-7,N
        -pages PAGES        show selected pages, format: 1,5-7,50-N

    Examples::

        pymupdf show x.pdf
        PDF is password protected

        pymupdf show x.pdf -pass hugo
        authentication unsuccessful

        pymupdf show x.pdf -pass jorjmckie
        authenticated as owner
        file 'x.pdf', pages: 1, objects: 19, 58 MB, PDF 1.4, encryption: Standard V5 R6 256-bit AES
        Document contains 15 embedded files.

        pymupdf show FDA-1572_508_R6_FINAL.pdf -tr -m
        'FDA-1572_508_R6_FINAL.pdf', pages: 2, objects: 1645, 1.4 MB, PDF 1.6, encryption: Standard V4 R4 128-bit AES
        document contains 740 root form fields and is signed

        ------------------------------- PDF metadata ------------------------------
            format: PDF 1.6
                title: FORM FDA 1572
            author: PSC Publishing Services
            subject: Statement of Investigator
            keywords: None
            creator: PScript5.dll Version 5.2.2
            producer: Acrobat Distiller 9.0.0 (Windows)
        creationDate: D:20130522104413-04'00'
            modDate: D:20190718154905-07'00'
        encryption: Standard V4 R4 128-bit AES

        ------------------------------- PDF trailer -------------------------------
        <<
        /DecodeParms <<
            /Columns 5
            /Predictor 12
        >>
        /Encrypt 1389 0 R
        /Filter /FlateDecode
        /ID [ <9252E9E39183F2A0B0C51BE557B8A8FC> <85227BE9B84B724E8F678E1529BA8351> ]
        /Index [ 1388 258 ]
        /Info 1387 0 R
        /Length 253
        /Prev 1510559
        /Root 1390 0 R
        /Size 1646
        /Type /XRef
        /W [ 1 3 1 ]
        >>

嵌入文件命令
------------------------

Embedded Files Commands

.. tab:: 中文

    以下命令处理嵌入式文件 - 这是 MuPDF 在 v1.14 之后完全删除的功能，因此也从其所有命令行工具中删除。

.. tab:: 英文

    The following commands deal with embedded files -- which is a feature completely removed from MuPDF after v1.14, and hence from all its command line tools.

信息
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Information

.. tab:: 中文

    显示嵌入文件的名称（长格式或短格式）::

        pymupdf embed-info -h
        usage: pymupdf embed-info [-h] [-name NAME] [-detail] [-password PASSWORD] input

        --------------------------- 列出嵌入文件 ---------------------------

        位置参数:
        input               PDF 文件名

        可选参数:
        -h, --help          显示帮助信息并退出
        -name NAME          如果提供此参数，仅报告该文件
        -detail             显示详细信息
        -password PASSWORD  密码

    示例::

        pymupdf embed-info some.pdf
        'some.pdf' 包含以下 15 个嵌入文件。

        20110813_180956_0002.jpg
        20110813_181009_0003.jpg
        20110813_181012_0004.jpg
        20110813_181131_0005.jpg
        20110813_181144_0006.jpg
        20110813_181306_0007.jpg
        20110813_181307_0008.jpg
        20110813_181314_0009.jpg
        20110813_181315_0010.jpg
        20110813_181324_0011.jpg
        20110813_181339_0012.jpg
        20110813_181913_0013.jpg
        insta-20110813_180944_0001.jpg
        markiert-20110813_180944_0001.jpg
        neue.datei

    详细输出示例如下::

            name: neue.datei
        filename: text-tester.pdf
       ufilename: text-tester.pdf
            desc: nur zum Testen!
            size: 4639
          length: 1566

.. tab:: 英文

    Show the embedded file names (long or short format)::

        pymupdf embed-info -h
        usage: pymupdf embed-info [-h] [-name NAME] [-detail] [-password PASSWORD] input

        --------------------------- list embedded files ---------------------------

        positional arguments:
        input               PDF filename

        optional arguments:
        -h, --help          show this help message and exit
        -name NAME          if given, report only this one
        -detail             show detail information
        -password PASSWORD  password

    Example::

        pymupdf embed-info some.pdf
        'some.pdf' contains the following 15 embedded files.

        20110813_180956_0002.jpg
        20110813_181009_0003.jpg
        20110813_181012_0004.jpg
        20110813_181131_0005.jpg
        20110813_181144_0006.jpg
        20110813_181306_0007.jpg
        20110813_181307_0008.jpg
        20110813_181314_0009.jpg
        20110813_181315_0010.jpg
        20110813_181324_0011.jpg
        20110813_181339_0012.jpg
        20110813_181913_0013.jpg
        insta-20110813_180944_0001.jpg
        markiert-20110813_180944_0001.jpg
        neue.datei

    Detailed output would look like this per entry::

            name: neue.datei
        filename: text-tester.pdf
       ufilename: text-tester.pdf
            desc: nur zum Testen!
            size: 4639
          length: 1566

提取
~~~~~~~~~~~~~~~~~~~~~~~~~

Extraction

.. tab:: 中文

    提取嵌入文件的方法如下::

        pymupdf embed-extract -h
        usage: pymupdf embed-extract [-h] -name NAME [-password PASSWORD] [-output OUTPUT]
                                input

        ---------------------- 将嵌入文件提取到磁盘 ----------------------

        位置参数:
        input                 PDF 文件名

        可选参数:
        -h, --help            显示帮助信息并退出
        -name NAME            条目名称
        -password PASSWORD    密码
        -output OUTPUT        输出文件名，默认为存储的名称

    详细信息请参考 :meth:`Document.embfile_get`。示例（参见上一节）::

        pymupdf embed-extract some.pdf -name neue.datei
        已将条目 'neue.datei' 保存为 'text-tester.pdf'


.. tab:: 英文

    Extract an embedded file like this::

        pymupdf embed-extract -h
        usage: pymupdf embed-extract [-h] -name NAME [-password PASSWORD] [-output OUTPUT]
                                input

        ---------------------- extract embedded file to disk ----------------------

        positional arguments:
        input                 PDF filename

        optional arguments:
        -h, --help            show this help message and exit
        -name NAME            name of entry
        -password PASSWORD    password
        -output OUTPUT        output filename, default is stored name

    For details consult :meth:`Document.embfile_get`. Example (refer to previous section)::

        pymupdf embed-extract some.pdf -name neue.datei
        Saved entry 'neue.datei' as 'text-tester.pdf'

删除
~~~~~~~~~~~~~~~~~~~~~~~~

Deletion

.. tab:: 中文

    删除嵌入文件的方法如下::

        pymupdf embed-del -h
        usage: pymupdf embed-del [-h] [-password PASSWORD] [-output OUTPUT] -name NAME input

        --------------------------- 删除嵌入文件 ---------------------------

        位置参数:
        input                 PDF 文件名

        可选参数:
        -h, --help            显示帮助信息并退出
        -password PASSWORD    密码
        -output OUTPUT        输出 PDF 文件名，若未指定则进行增量保存
        -name NAME            要删除的条目名称

    详细信息请参考 :meth:`Document.embfile_del`。


.. tab:: 英文

    Delete an embedded file like this::

        pymupdf embed-del -h
        usage: pymupdf embed-del [-h] [-password PASSWORD] [-output OUTPUT] -name NAME input

        --------------------------- delete embedded file --------------------------

        positional arguments:
        input                 PDF filename

        optional arguments:
        -h, --help            show this help message and exit
        -password PASSWORD    password
        -output OUTPUT        output PDF filename, incremental save if none
        -name NAME            name of entry to delete

    For details consult :meth:`Document.embfile_del`.

插入
~~~~~~~~~~~~~~~~~~~~~~~~

Insertion

.. tab:: 中文

    使用以下命令添加嵌入文件::

        pymupdf embed-add -h
        usage: pymupdf embed-add [-h] [-password PASSWORD] [-output OUTPUT] -name NAME -path
                            PATH [-desc DESC]
                            input

        ---------------------------- 添加嵌入文件 ----------------------------

        位置参数:
        input                 PDF 文件名

        可选参数:
        -h, --help            显示帮助信息并退出
        -password PASSWORD    密码
        -output OUTPUT        输出 PDF 文件名，若未指定则进行增量保存
        -name NAME            新条目的名称
        -path PATH            新条目的数据路径
        -desc DESC            新条目的描述

    *"NAME"* **不能** 在 PDF 中已经存在。详细信息请参考 :meth:`Document.embfile_add` 。


.. tab:: 英文

    Add a new embedded file using this command::

        pymupdf embed-add -h
        usage: pymupdf embed-add [-h] [-password PASSWORD] [-output OUTPUT] -name NAME -path
                            PATH [-desc DESC]
                            input

        ---------------------------- add embedded file ----------------------------

        positional arguments:
        input                 PDF filename

        optional arguments:
        -h, --help            show this help message and exit
        -password PASSWORD    password
        -output OUTPUT        output PDF filename, incremental save if none
        -name NAME            name of new entry
        -path PATH            path to data for new entry
        -desc DESC            description of new entry

    *"NAME"* **must not** already exist in the PDF. For details consult :meth:`Document.embfile_add`.

更新
~~~~~~~~~~~~~~~~~~~~~~~

Updates

.. tab:: 中文

    使用以下命令更新现有的嵌入文件::

        pymupdf embed-upd -h
        usage: pymupdf embed-upd [-h] -name NAME [-password PASSWORD] [-output OUTPUT]
                            [-path PATH] [-filename FILENAME] [-ufilename UFILENAME]
                            [-desc DESC]
                            input

        --------------------------- 更新嵌入文件 --------------------------

        位置参数:
        input                 PDF 文件名

        可选参数:
        -h, --help            显示帮助信息并退出
        -name NAME            条目的名称
        -password PASSWORD    密码
        -output OUTPUT        输出 PDF 文件名，若未指定则进行增量保存
        -path PATH            新数据的路径
        -filename FILENAME    存储在条目中的新文件名
        -ufilename UFILENAME  存储在条目中的新 Unicode 文件名
        -desc DESC            存储在条目中的新描述

    除 `-name` 外，所有参数均为可选项。

    使用此方法可以更改文件的元信息——只需省略 *"PATH"* 。详细信息请参考 :meth:`Document.embfile_upd`。


.. tab:: 英文

    Update an existing embedded file using this command::

        pymupdf embed-upd -h
        usage: pymupdf embed-upd [-h] -name NAME [-password PASSWORD] [-output OUTPUT]
                            [-path PATH] [-filename FILENAME] [-ufilename UFILENAME]
                            [-desc DESC]
                            input

        --------------------------- update embedded file --------------------------

        positional arguments:
        input                 PDF filename

        optional arguments:
        -h, --help            show this help message and exit
        -name NAME            name of entry
        -password PASSWORD    password
        -output OUTPUT        Output PDF filename, incremental save if none
        -path PATH            path to new data for entry
        -filename FILENAME    new filename to store in entry
        -ufilename UFILENAME  new unicode filename to store in entry
        -desc DESC            new description to store in entry

        except '-name' all parameters are optional

    Use this method to change meta-information of the file -- just omit the *"PATH"*. For details consult :meth:`Document.embfile_upd`.


复制
~~~~~~~~~~~~~~~~~~~~~~~

Copying

.. tab:: 中文

    在 PDF 之间复制嵌入文件的方法如下::

        pymupdf embed-copy -h
        usage: pymupdf embed-copy [-h] [-password PASSWORD] [-output OUTPUT] -source
                            SOURCE [-pwdsource PWDSOURCE]
                            [-name [NAME [NAME ...]]]
                            input

        --------------------- 在 PDF 之间复制嵌入文件 --------------------

        位置参数:
        input                 接收嵌入文件的 PDF

        可选参数:
        -h, --help            显示帮助信息并退出
        -password PASSWORD    输入文件的密码
        -output OUTPUT        输出 PDF 文件，若未指定则对 'input' 进行增量保存
        -source SOURCE        从此 PDF 复制嵌入文件
        -pwdsource PWDSOURCE  'source' PDF 的密码
        -name [NAME [NAME ...]]
                            限制复制为这些条目


.. tab:: 英文

    Copy embedded files between PDFs::

        pymupdf embed-copy -h
        usage: pymupdf embed-copy [-h] [-password PASSWORD] [-output OUTPUT] -source
                            SOURCE [-pwdsource PWDSOURCE]
                            [-name [NAME [NAME ...]]]
                            input

        --------------------- copy embedded files between PDFs --------------------

        positional arguments:
        input                 PDF to receive embedded files

        optional arguments:
        -h, --help            show this help message and exit
        -password PASSWORD    password of input
        -output OUTPUT        output PDF, incremental save to 'input' if omitted
        -source SOURCE        copy embedded files from here
        -pwdsource PWDSOURCE  password of 'source' PDF
        -name [NAME [NAME ...]]
                            restrict copy to these entries


文本提取
----------------

Text Extraction 

.. tab:: 中文

    * 新增功能：v1.18.16版本

    从任意支持的文档中提取文本并保存为文本文件。目前，有三种输出格式模式可供选择：简单模式、块排序模式和物理布局复现模式。

    * **简单模式**：文本按其在文档页面中的显示顺序复制，不进行任何排列或重新排序。
    * **块排序模式**：按垂直和水平方向的坐标对文本块进行排序，这通常能为基本文本页面建立“自然”阅读顺序。
    * **布局模式**：尽量复现输入页面的原始外观。您可以预期类似以下的结果（由命令 `pymupdf gettext -pages 1 demo1.pdf` 生成）：

    .. image:: images/img-layout-text.*
        :scale: 60

    .. note:: "gettext" 命令提供类似于 XPDF 软件的 CLI 工具 `pdftotext` 的功能，尤其是在 "layout" 模式下，这与该工具的 `-layout` 和 `-table` 选项结合使用非常相似。

    每页输出文件后将写入一个换页符字符 `hex(12)`，即使输入页面没有任何文本。此行为可以通过选项进行控制。

    .. note:: 对于 "layout" 模式，**仅支持水平的从左到右、从上到下的文本**，其他方向的文本将被忽略。在此模式下，若文本的 :data:`fontsize` 太小，则该文本也会被忽略。

    相比之下，"simple" 和 "blocks" 模式会输出 **所有文本** ，无论其字体大小或方向如何。

    命令::

        pymupdf gettext -h
        usage: pymupdf gettext [-h] [-password PASSWORD] [-mode {simple,blocks,layout}] [-pages PAGES] [-noligatures]
                            [-convert-white] [-extra-spaces] [-noformfeed] [-skip-empty] [-output OUTPUT] [-grid GRID]
                            [-fontsize FONTSIZE]
                            input

        ----------------- 在各种格式模式下提取文本 ----------------

        位置参数:
        input                 输入文档文件名

        可选参数:
        -h, --help            显示帮助信息并退出
        -password PASSWORD    输入文档的密码
        -mode {simple,blocks,layout}
                                模式：简单、块排序或布局（默认为布局）
        -pages PAGES          选择页面，格式为：1,5-7,50-N
        -noligatures          展开连字字符（默认为 False）
        -convert-white        将空白字符转换为空格（默认为 False）
        -extra-spaces         用空格填充字符间的空隙（默认为 False）
        -noformfeed           写入换行符，而非换页符（默认为 False）
        -skip-empty           跳过没有文本的页面（默认为 False）
        -output OUTPUT        将文本存储到此文件（默认为输入文件名 .txt）
        -grid GRID            如果行之间的垂直坐标差距小于此值，则合并为同一行（默认为 2）
        -fontsize FONTSIZE    仅包括较大字体的文本（默认为 3）

    .. note:: 
        
        命令选项可以缩写，只要不引起歧义。如下命令效果相同：

        * `... -output text.txt -noligatures -noformfeed -convert-white -grid 3 -extra-spaces ...`
        * `... -o text.txt -nol -nof -c -g 3 -e ...`

        输出文件名默认为输入文件名，并将其扩展名替换为 `.txt`。与其他命令一样，您可以按照上述格式选择页面范围 **（请注意：从 1 开始计数！）**。

    * **mode:** (str) 选择格式模式 -- 默认为 "layout"。
    * **noligatures:** (bool) 相当于 **不** :data:`TEXT_PRESERVE_LIGATURES`。如果指定，连字（如 "fi"）将被分解为其组成字符（即 "f" 和 "i"）。默认保留连字。
    * **convert-white:** (bool) 相当于 **不** :data:`TEXT_PRESERVE_WHITESPACE`。如果指定，所有空白字符（如制表符）将被替换为一个或多个空格。默认保留空白字符。
    * **extra-spaces:** (bool) 相当于 **不** :data:`TEXT_INHIBIT_SPACES`。如果指定，大间隙将填充为一个或多个空格。默认关闭。
    * **noformfeed:** (bool) 用换行符 `\n` 替代 `hex(12)` （换页符），在输出页面的结尾添加换行符。
    * **skip-empty:** (bool) 跳过没有文本的页面。
    * **grid:** 行的垂直坐标差小于此值（以点为单位）时，将合并为同一输出行。仅对 "layout" 模式相关。**请谨慎使用：** 3 或默认值 2 在大多数情况下已足够。如果 **设置过大**，原本应为不同的行可能会合并，从而导致输出错误或不完整。如果 **设置过小**，某些输入行因字体属性略有不同而可能生成不必要的分离行。
    * **fontsize:** 仅包括字体大小大于此值的文本（默认为 3）。仅对 "layout" 模式相关。


.. tab:: 英文

    * New in v1.18.16

    Extract text from arbitrary :ref:`supported documents<Supported_File_Types>` to a textfile. Currently, there are three output formatting modes available: simple, block sorting and reproduction of physical layout.

    * **Simple** text extraction reproduces all text as it appears in the document pages -- no effort is made to rearrange in any particular reading order.
    * **Block sorting** sorts text blocks (as identified by MuPDF) by ascending vertical, then horizontal coordinates. This should be sufficient to establish a "natural" reading order for basic pages of text.
    * **Layout** strives to reproduce the original appearance of the input pages. You can expect results like this (produced by the command `pymupdf gettext -pages 1 demo1.pdf`):

    .. image:: images/img-layout-text.*
        :scale: 60

    .. note:: The "gettext" command offers a functionality similar to the CLI tool `pdftotext` by XPDF software, http://www.foolabs.com/xpdf/ -- this is especially true for "layout" mode, which combines that tool's `-layout` and `-table` options.



    After each page of the output file, a formfeed character, `hex(12)` is written -- even if the input page has no text at all. This behavior can be controlled via options.

    .. note:: For "layout" mode, **only horizontal, left-to-right, top-to bottom** text is supported, other text is ignored. In this mode, text is also ignored, if its :data:`fontsize` is too small.

    "Simple" and "blocks" mode in contrast output **all text** for any text size or orientation.

    Command::

        pymupdf gettext -h
        usage: pymupdf gettext [-h] [-password PASSWORD] [-mode {simple,blocks,layout}] [-pages PAGES] [-noligatures]
                            [-convert-white] [-extra-spaces] [-noformfeed] [-skip-empty] [-output OUTPUT] [-grid GRID]
                            [-fontsize FONTSIZE]
                            input

        ----------------- extract text in various formatting modes ----------------

        positional arguments:
        input                 input document filename

        optional arguments:
        -h, --help            show this help message and exit
        -password PASSWORD    password for input document
        -mode {simple,blocks,layout}
                                mode: simple, block sort, or layout (default)
        -pages PAGES          select pages, format: 1,5-7,50-N
        -noligatures          expand ligature characters (default False)
        -convert-white        convert whitespace characters to space (default False)
        -extra-spaces         fill gaps with spaces (default False)
        -noformfeed           write linefeeds, no formfeeds (default False)
        -skip-empty           suppress pages with no text (default False)
        -output OUTPUT        store text in this file (default inputfilename.txt)
        -grid GRID            merge lines if closer than this (default 2)
        -fontsize FONTSIZE    only include text with a larger :data:`fontsize` (default 3)

    .. note:: 
        
        Command options may be abbreviated as long as no ambiguities are introduced. So the following do the same:

        * `... -output text.txt -noligatures -noformfeed -convert-white -grid 3 -extra-spaces ...`
        * `... -o text.txt -nol -nof -c -g 3 -e ...`

        The output filename defaults to the input with its extension replaced by `.txt`. As with other commands, you can select page ranges **(caution: 1-based!)** in `mutool` format, as indicated above.

    * **mode:** (str) select a formatting mode -- default is "layout".
    * **noligatures:** (bool) corresponds to **not** :data:`TEXT_PRESERVE_LIGATURES`. If specified, ligatures (present in advanced fonts: glyphs combining multiple characters like "fi") are split up into their components (i.e. "f", "i"). Default is passing them through.
    * **convert-white:** corresponds to **not** :data:`TEXT_PRESERVE_WHITESPACE`. If specified, all white space characters (like tabs) are replaced with one or more spaces. Default is passing them through.
    * **extra-spaces:**  (bool) corresponds to **not** :data:`TEXT_INHIBIT_SPACES`. If specified, large gaps between adjacent characters will be filled with one or more spaces. Default is off.
    * **noformfeed:**  (bool) instead of `hex(12)` (formfeed), write linebreaks ``\n`` at end of output pages.
    * **skip-empty:**  (bool) skip pages with no text.
    * **grid:** lines with a vertical coordinate difference of no more than this value (in points) will be merged into the same output line. Only relevant for "layout" mode. **Use with care:** 3 or the default 2 should be adequate in most cases. If **too large**, lines that are *intended* to be different in the original may be merged and will result in garbled and / or incomplete output. If **too low**, artifact separate output lines may be generated for some spans in the input line, just because they are coded in a different font with slightly deviating properties.
    * **fontsize:** include text with :data:`fontsize` larger than this value only (default 3). Only relevant for "layout" option.


.. highlight:: python

.. include:: footer.rst
