.. include:: header.rst



安装
=============

Installation


依赖
---------------------------------------------------------

Requirements

.. tab:: 中文

    以下所有示例均假设您在 Python 虚拟环境中运行。  
    详情请参见：https://docs.python.org/3/library/venv.html。  
    同时，我们假设 `pip` 已是最新版本。

    例如：

    * Windows::

        py -m venv pymupdf-venv
        .\pymupdf-venv\Scripts\activate
        python -m pip install --upgrade pip

    * Linux, MacOS::

        python -m venv pymupdf-venv
        . pymupdf-venv/bin/activate
        python -m pip install --upgrade pip


.. tab:: 英文

    All the examples below assume that you are running inside a Python virtual
    environment. See: https://docs.python.org/3/library/venv.html for details.
    We also assume that `pip` is up to date.

    For example:

    * Windows::

        py -m venv pymupdf-venv
        .\pymupdf-venv\Scripts\activate
        python -m pip install --upgrade pip

    * Linux, MacOS::

        python -m venv pymupdf-venv
        . pymupdf-venv/bin/activate
        python -m pip install --upgrade pip


安装
---------------------------------------------------------

Installation

.. tab:: 中文

  PyMuPDF 应使用 pip 安装::

    pip install --upgrade pymupdf

  如果您的平台有 Python wheel，这将从 Python wheel 安装。

.. tab:: 英文

  PyMuPDF should be installed using pip with::

    pip install --upgrade pymupdf

  This will install from a Python wheel if one is available for your platform.


在没有合适whell时的安装
---------------------------------------------------------

Installation when a suitable wheel is not available

.. tab:: 中文

    如果没有可用的 Python 轮子 (`wheel`)，`pip` 将自动使用 Python 源代码分发包 (`sdist`) 进行构建。

    **这需要安装 C/C++ 开发工具**：

    * 在 Windows 上：

      * 安装 Visual Studio 2019。如果未安装在标准位置，需要将环境变量 `PYMUPDF_SETUP_DEVENV` 设置为 `devenv.com` 二进制文件所在的目录。

      * 如果安装了其他版本的 Visual Studio，例如 Visual Studio 2022，可能会导致问题，因为 MuPDF 和 PyMuPDF 代码可能会使用不同版本的编译器进行编译。

    构建过程中将自动下载并构建 MuPDF。


.. tab:: 英文

    If a suitable Python wheel is not available, pip will automatically build from
    source using a Python sdist.

    **This requires C/C++ development tools to be installed**:

    * On Windows:

      *
        Install Visual Studio 2019. If not installed in a standard location, set
        environmental variable `PYMUPDF_SETUP_DEVENV` to the location of the
        `devenv.com` binary.

      *
        Having other installed versions of Visual Studio, for example Visual Studio
        2022, can cause problems because one can end up with MuPDF and PyMuPDF code
        being compiled with different compiler versions.

    The build will automatically download and build MuPDF.


.. _problems-after-installation:

安装后的问题
---------------------------------------------------------

Problems after installation

.. tab:: 中文

    * 在 Windows 上，可能遇到 Python 错误::
    
          ImportError: DLL load failed while importing _fitz
    
      这可能是由于 `MSVCP140.dll` 缺失造成的，通常与某些版本（2015-2017）的 `Microsoft Visual C++ Redistributables` 相关的错误有关。
    
      建议在 https://msdn.com 上搜索 `MSVCP140.dll`，查找如何重新安装的说明。例如，  
      https://learn.microsoft.com/cpp/windows/latest-supported-vc-redist 提供了最新支持版本的永久链接。
    
      详情请参见：https://github.com/pymupdf/PyMuPDF/issues/2678。
    
    *
      可能遇到 Python 错误::
    
          ModuleNotFoundError: No module named 'frontend'
      
      这通常发生在 PyMuPDF 的旧名称 `fitz` 被错误使用时（例如 `import fitz` 而非 `import pymupdf`），
      并且安装了一个无关的 Python 包 `fitz`（https://pypi.org/project/fitz/）。
    
      `fitz` 包似乎已不再维护（最新发布于 2017 年），但目前无法从 pypi.org 移除。
      它不仅无法独立运行，还会破坏 PyMuPDF 的旧名称 `fitz` 的使用。
    
      有几种方法可以避免此问题：
    
      * 使用 `import pymupdf` 替代 `import fitz`，并更新代码以匹配新的导入方式。
    
      * 或者卸载 `fitz` 包并重新安装 PyMuPDF::
      
            pip uninstall fitz
            pip install --force-reinstall pymupdf
    
      * 或者使用 `import pymupdf as fitz`，但此方法尚未经过充分测试。
    
    * 在 Apple Silicon (arm64) 上使用 Jupyter Lab 时，可能遇到 Python 错误::
    
          ImportError: /opt/conda/lib/python3.11/site-packages/pymupdf/libmupdf.so.24.4: undefined symbol: fz_pclm_write_options_usage
    
      这似乎是 Jupyter Lab 的问题，详情请参见：
      https://github.com/pymupdf/PyMuPDF/issues/3643#issuecomment-2210588778。


.. tab:: 英文

    * On Windows, Python error::
    
          ImportError: DLL load failed while importing _fitz
    
      This has been occasionally seen if `MSVCP140.dll` is missing, and appears
      to be caused by a bug in some versions (2015-2017) of `Microsoft Visual C++
      Redistributables`.
    
      It is recommended to search for `MSVCP140.dll` in https://msdn.com
      to find instructions for how to reinstall it. For example
      https://learn.microsoft.com/cpp/windows/latest-supported-vc-redist has
      permalinks to the latest supported versions.
    
      See https://github.com/pymupdf/PyMuPDF/issues/2678 for more details.
    
    *
      Python error::
    
          ModuleNotFoundError: No module named 'frontend'
      
      This can happen if PyMuPDF's legacy name `fitz` is used (for example `import
      fitz` instead of `import pymupdf`), and an unrelated Python package called
      `fitz` (https://pypi.org/project/fitz/) is installed.
    
      The fitz package appears to be no longer maintained (the latest release is
      from 2017), but unfortunately it does not seem possible to remove it from
      pypi.org. It does not even work on its own, as well as breaking the use of
      PyMuPDF's legacy name.
    
      There are a few ways to avoid this problem:
    
      *
        Use `import pymupdf` instead of `import fitz`, and update one's code to
        match.
    
      * Or uninstall the `fitz` package and reinstall PyMuPDF::
      
            pip uninstall fitz
            pip install --force-reinstall pymupdf
    
      * Or use `import pymupdf as fitz`. However this has not been well tested.
    
    * With Jupyter labs on Apple Silicon (arm64), Python error::
    
          ImportError: /opt/conda/lib/python3.11/site-packages/pymupdf/libmupdf.so.24.4: undefined symbol: fz_pclm_write_options_usage
    
      This appears to be a problem in Jupyter labs; see:
      https://github.com/pymupdf/PyMuPDF/issues/3643#issuecomment-2210588778.


笔记
---------------------------------------------------------

Notes

.. tab:: 中文

    *
      以下平台提供预构建的 Wheels：
      
       * Windows 32 位 Intel。
       * Windows 64 位 Intel。
       * Linux 64 位 Intel。
       * Linux 64 位 ARM。
       * MacOS 64 位 Intel。
       * MacOS 64 位 ARM。
      
      详情如下：
      
      * 每个平台仅发布一个 Wheel 文件。
    
      *
        每个 Wheel 文件均使用当前最旧的受支持 Python 版本的稳定 ABI（目前为 3.9），
        因此可与所有更高版本的 Python 兼容，包括新的 Python 发行版本。
    
      *
        Wheels 已在所有当前被标记为“受支持”的 Python 版本上进行了测试，
        详情请见：https://devguide.python.org/versions/，目前支持 3.9、3.10、3.11、3.12 和 3.13。
    
    *
      Windows 通过 `Chocolatey <https://chocolatey.org/>`_ 安装的 Python **不支持** Wheels。  
      请改用 python.org 官网提供的 Windows 安装程序进行安装：
      http://www.python.org/downloads
    
    *
      对于 Linux-aarch64 平台，使用 `Musl libc <https://musl.libc.org/>`_（例如 `Alpine Linux <https://alpinelinux.org/>`_），
      不提供 Wheels，且已知从源码构建会失败。
    
    *
      **没有强制性外部依赖**，但某些可选功能需要额外安装组件：
    
      * `Pillow <https://pypi.org/project/Pillow/>`_ ：用于 :meth:`Pixmap.pil_save` 和 :meth:`Pixmap.pil_tobytes` 方法。
      * `fontTools <https://pypi.org/project/fonttools/>`_ ：用于 :meth:`Document.subset_fonts` 方法。
      * `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_ ：包含一组可用于文本输出方法的精美字体。
      * 
        `Tesseract-OCR <https://github.com/tesseract-ocr/tesseract>`_ ：用于图像和文档页面的光学字符识别（OCR）。  
        Tesseract 是一个独立的软件，而非 Python 包。要在 PyMuPDF 中启用 OCR 功能，  
        需要安装 Tesseract 并指定 `tessdata` 文件夹名称，具体请见下文。
    
      .. note:: 这些组件可以在安装 PyMuPDF 之前或之后随时添加。PyMuPDF 会在导入或使用相关功能时自动检测其是否可用。

.. tab:: 英文

    *
      Wheels are available for the following platforms:
      
       * Windows 32-bit Intel.
       * Windows 64-bit Intel.
       * Linux 64-bit Intel.
       * Linux 64-bit ARM.
       * MacOS 64-bit Intel.
       * MacOS 64-bit ARM.
      
      Details:
      
      * We release a single wheel for each of the above platforms.
      
      *
        Each wheel uses the Python Stable ABI of the current oldest supported
        Python version (currently 3.9), and so works with all later Python
        versions, including new Python releases.
      
      *
        Wheels are tested on all Python versions currently marked as "Supported" on
        https://devguide.python.org/versions/, currently 3.9, 3.10, 3.11, 3.12 and
        3.13.
    
    *
      Wheels are not available for Python installed with `Chocolatey
      <https://chocolatey.org/>`_ on Windows. Instead install Python
      using the Windows installer from the python.org website, see:
      http://www.python.org/downloads
    
    *
      Wheels are not available for Linux-aarch64 with `Musl libc
      <https://musl.libc.org/>`_ (For example `Alpine Linux
      <https://alpinelinux.org/>`_ on aarch64), and building from source is known
      to fail.
    
    * There are no **mandatory** external dependencies. However, some optional feature are available only if additional components are installed:
    
      * `Pillow <https://pypi.org/project/Pillow/>`_ is required for :meth:`Pixmap.pil_save` and :meth:`Pixmap.pil_tobytes`.
      * `fontTools <https://pypi.org/project/fonttools/>`_ is required for :meth:`Document.subset_fonts`.
      * `pymupdf-fonts <https://pypi.org/project/pymupdf-fonts/>`_ is a collection of nice fonts to be used for text output methods.
      * 
        `Tesseract-OCR <https://github.com/tesseract-ocr/tesseract>`_ for optical
        character recognition in images and document pages. Tesseract is separate
        software, not a Python package. To enable OCR functions in PyMuPDF,
        Tesseract must be installed and the `tessdata` folder name specified; see
        below.
    
      .. note:: You can install these additional components at any time -- before or after installing PyMuPDF. PyMuPDF will detect their presence during import or when the respective functions are being used.


从本地 PyMuPDF 源树构建并安装
---------------------------------------------------------

Build and install from a local PyMuPDF source tree

.. tab:: 中文

    初始化设置：
    
    * 按上述说明安装 C/C++ 开发工具。
    * 进入 Python 虚拟环境（venv）并更新 pip，如前所述。
    
    * 获取 PyMuPDF 源代码：
    
      * 克隆 PyMuPDF Git 仓库::
    
            git clone https://github.com/pymupdf/PyMuPDF.git
    
      *
        或从 https://github.com/pymupdf/PyMuPDF/releases 下载 `.zip` 或 `.tar.gz` 源代码发布包并解压。
    
    然后，可以通过以下两种方式构建 PyMuPDF：
    
    * 使用默认的 MuPDF 版本构建并安装 PyMuPDF::
    
          cd PyMuPDF && pip install .
    
      这将自动下载一个特定的 MuPDF 源代码版本，并将其构建到 PyMuPDF 中。
    
    * 或者使用本地 MuPDF 源代码树构建并安装 PyMuPDF：
    
      * 克隆 MuPDF Git 仓库::
    
          git clone --recursive https://git.ghostscript.com/mupdf.git
    
      *
        构建 PyMuPDF，并使用环境变量 `PYMUPDF_SETUP_MUPDF_BUILD` 指定本地 MuPDF 代码目录::
    
          cd PyMuPDF && PYMUPDF_SETUP_MUPDF_BUILD=../mupdf pip install .
    
    此外，可以在同一 PyMuPDF 源代码目录下为不同的 Python 版本构建 PyMuPDF：
    
    *
      PyMuPDF 将为运行 `pip` 的 Python 版本进行构建。要使用特定的 Python 版本运行 `pip`，请使用 `python -m pip` 而不是 `pip`。
    
      例如，在 Windows 上，可以使用以下方式构建不同版本的 PyMuPDF::
    
        cd PyMuPDF && py -3.9 -m pip install .
    
      或者::
    
        cd PyMuPDF && py -3.10-32 -m pip install .
    

.. tab:: 英文

    Initial setup:
    
    * Install C/C++ development tools as described above.
    * Enter a Python venv and update pip, as described above.
    
    * Get a PyMuPDF source tree:
    
      * Clone the PyMuPDF git repository::
    
            git clone https://github.com/pymupdf/PyMuPDF.git
    
      *
        Or download and extract a `.zip` or `.tar.gz` source release from
        https://github.com/pymupdf/PyMuPDF/releases.
    
    Then one can build PyMuPDF in two ways:
    
    * Build and install PyMuPDF with default MuPDF version::
    
          cd PyMuPDF && pip install .
    
      This will automatically download a specific hard-coded MuPDF source
      release, and build it into PyMuPDF.
    
    * Or build and install PyMuPDF using a local MuPDF source tree:
    
      * Clone the MuPDF git repository::
    
          git clone --recursive https://git.ghostscript.com/mupdf.git
    
      *
        Build PyMuPDF, specifying the location of the local MuPDF tree with the
        environmental variables `PYMUPDF_SETUP_MUPDF_BUILD`::
    
          cd PyMuPDF && PYMUPDF_SETUP_MUPDF_BUILD=../mupdf pip install .
    
    Also, one can build for different Python versions in the same PyMuPDF tree:
    
    *
      PyMuPDF will build for the version of Python that is being used to run
      `pip`. To run `pip` with a specific Python version, use `python -m pip`
      instead of `pip`.
    
      So for example on Windows one can build different versions with::
    
        cd PyMuPDF && py -3.9 -m pip install .
    
      or::
    
        cd PyMuPDF && py -3.10-32 -m pip install .


运行测试
---------------------------------------------------------

Running tests

.. tab:: 中文

    有了 PyMuPDF 树，就可以运行 PyMuPDF 的 `pytest` 测试套件::

      pip install pytest fontTools
      pytest PyMuPDF/tests

.. tab:: 英文

    Having a PyMuPDF tree available allows one to run PyMuPDF's `pytest` test suite::

      pip install pytest fontTools
      pytest PyMuPDF/tests



关于使用非默认 MuPDF 的注意事项
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Notes about using a non-default MuPDF

.. tab:: 中文

    通过设置环境变量 `PYMUPDF_SETUP_MUPDF_BUILD` 使用非默认版本的 MuPDF 可能会导致各种问题，因此通常不建议使用：

    * 如果 MuPDF 的主版本号与 PyMuPDF 默认使用的版本不同，PyMuPDF 可能无法构建，因为 MuPDF 的 API 在不同的主版本之间可能会发生变化。

    * PyMuPDF 的运行时行为可能会发生变化，因为 MuPDF 在不同的小版本之间的运行时行为可能会不同。这也可能会导致一些 PyMuPDF 测试失败。

    * 如果 MuPDF 是使用其默认配置构建的，而不是 PyMuPDF 的自定义配置（例如，如果 MuPDF 是系统安装），那么 `tests/test_textbox.py:test_textbox3()` 可能会失败。可以通过在 `pytest` 命令行中添加 `-k 'not test_textbox3'` 来跳过此特定测试。


.. tab:: 英文

    Using a non-default build of MuPDF by setting environmental variable
    `PYMUPDF_SETUP_MUPDF_BUILD` can cause various things to go wrong and so is
    not generally supported:

    * If MuPDF's major version number differs from what PyMuPDF uses by default,
      PyMuPDF can fail to build, because MuPDF's API can change between major
      versions.

    * Runtime behaviour of PyMuPDF can change because MuPDF's runtime behaviour
      changes between different minor releases. This can also break some PyMuPDF
      tests.

    * If MuPDF was built with its default config instead of PyMuPDF's customised
      config (for example if MuPDF is a system install), it is possible that
      `tests/test_textbox.py:test_textbox3()` will fail. One can skip this
      particular test by adding `-k 'not test_textbox3'` to the `pytest`
  command line.


打包
---------

Packaging

.. tab:: 中文

    见 :doc:`packaging`.

.. tab:: 英文

    See :doc:`packaging`.


与 Pyodide 一起使用
------------------

Using with Pyodide

.. tab:: 中文

    见 :doc:`pyodide`.

.. tab:: 英文

    See :doc:`pyodide`.


.. _installation_ocr:

启用集成 OCR 支持
---------------------------------------------------------

Enabling Integrated OCR Support

.. tab:: 中文

    如果您不打算使用此功能，请跳过此步骤。否则，它对于两种安装方式都必需：**通过轮子（wheels）和源代码（sources）安装**。

    PyMuPDF 已经包含了支持 OCR 功能的所有逻辑。但它还需要 `Tesseract 的语言支持数据 <https://github.com/tesseract-ocr/tessdata>`_。

    如果没有明确指定，PyMuPDF 会尝试查找已安装的 Tesseract 的 tessdata，但这不应依赖于此。

    否则，PyMuPDF 要求明确指定 Tesseract 的语言支持文件夹，无论是通过 PyMuPDF OCR 函数的 `tessdata` 参数，还是通过 `os.environ["TESSDATA_PREFIX"]`。

    因此，要使 OCR 功能正常工作，请确保完成以下检查：

    1. 找到 Tesseract 的语言支持文件夹。通常您会在以下位置找到它：

      * Windows: `C:/Program Files/Tesseract-OCR/tessdata`
      * Unix 系统: `/usr/share/tesseract-ocr/4.00/tessdata`

    2. 在调用 PyMuPDF OCR 函数时指定语言支持文件夹：
      
      * 设置 `tessdata` 参数。
      * 或在 Python 中设置 `os.environ["TESSDATA_PREFIX"]`。
      * 或在运行 Python 之前设置环境变量 `TESSDATA_PREFIX`，例如：
      
        * Windows: `setx TESSDATA_PREFIX "C:/Program Files/Tesseract-OCR/tessdata"`
        * Unix 系统: `declare -x TESSDATA_PREFIX=/usr/share/tesseract-ocr/4.00/tessdata`


.. tab:: 英文

    If you do not intend to use this feature, skip this step. Otherwise, it is required for both installation paths: **from wheels and from sources.**

    PyMuPDF will already contain all the logic to support OCR functions. But it additionally does need `Tesseract’s language support data <https://github.com/tesseract-ocr/tessdata>`_.

    If not specified explicitly, PyMuPDF will attempt to find the installed
    Tesseract's tessdata, but this should probably not be relied upon.

    Otherwise PyMuPDF requires that Tesseract's language support folder is
    specified explicitly either in PyMuPDF OCR functions' `tessdata` arguments or
    `os.environ["TESSDATA_PREFIX"]`.

    So for a working OCR functionality, make sure to complete this checklist:

    1. Locate Tesseract's language support folder. Typically you will find it here:

      * Windows: `C:/Program Files/Tesseract-OCR/tessdata`
      * Unix systems: `/usr/share/tesseract-ocr/4.00/tessdata`

    2. Specify the language support folder when calling PyMuPDF OCR functions:
      
      * Set the `tessdata` argument.
      * Or set `os.environ["TESSDATA_PREFIX"]` from within Python.
      * Or set environment variable `TESSDATA_PREFIX` before running Python, for example:
      
        * Windows: `setx TESSDATA_PREFIX "C:/Program Files/Tesseract-OCR/tessdata"`
        * Unix systems: `declare -x TESSDATA_PREFIX=/usr/share/tesseract-ocr/4.00/tessdata`

.. include:: footer.rst
