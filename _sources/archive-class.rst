.. include:: header.rst

.. _Archive:

================
Archive
================

.. tab:: 中文

   *新增于 v1.21.0*

   此类表示对文件夹和容器文件（如 ZIP 和 TAR 归档文件）的泛化。归档文件允许将任意集合的文件夹、ZIP / TAR 文件以及单个二进制数据元素视为一个层次化的文件夹树进行访问。

   在 PyMuPDF 中，归档文件目前仅被 :ref:`Story` 对象使用，以指定字体、图像和其他资源的查找位置。

   ================================ ===================================================
   **方法 / 属性**                   **简要描述**
   ================================ ===================================================
   :meth:`Archive.add`              向归档添加新数据
   :meth:`Archive.has_entry`        检查指定名称是否为归档成员
   :meth:`Archive.read_entry`       读取指定名称的数据
   :attr:`Archive.entry_list`       归档项的列表（list[dict]）
   ================================ ===================================================

.. tab:: 英文

   * New in v1.21.0

   This class represents a generalization of file folders and container files like ZIP and TAR archives. Archives allow accessing arbitrary collections of file folders, ZIP / TAR files and single binary data elements as if they all were part of one hierarchical tree of folders.

   In PyMuPDF, archives are currently only used by :ref:`Story` objects to specify where to look for fonts, images and other resources.

   ================================ ===================================================
   **Method / Attribute**           **Short Description**
   ================================ ===================================================
   :meth:`Archive.add`              add new data to the archive
   :meth:`Archive.has_entry`        check if given name is a member
   :meth:`Archive.read_entry`       read the data given by the name
   :attr:`Archive.entry_list`       list[dict] of archive items
   ================================ ===================================================

**Class API**

.. tab:: 中文

   .. class:: Archive

      .. method:: __init__(self [, content [, path]])

         创建一个新的归档文件。如果没有参数，则创建一个空的归档文件。

         如果提供了 `content`，它可以是以下之一：

         * 另一个 Archive：将该归档作为新归档的子归档。
         * 字符串：这必须是一个本地文件夹或文件的名称。也支持 `pathlib.Path` 对象。
            - **文件夹** 会被转换为子归档，以便可以通过文件夹内的文件名访问。
            - **文件** 会以 `"rb"` 模式读取，文件内容（二进制数据，`bytes` 对象）会作为一个单一成员的子归档。在这种情况下，`path` 参数是 **必需的**，应指定该项的成员名称。
         * `zipfile.ZipFile` 或 `tarfile.TarFile` 对象：将其作为子归档添加。
         * Python 二进制对象（如 `bytes`, `bytearray`, `io.BytesIO`）：将其添加为单成员子归档。在这种情况下，`path` 参数是 **必需的**，并应指定该项的成员名称。
         * 元组 `(data, name)`：将其作为一个单成员子归档，成员名称为 `name`。`data` 可以是 Python 二进制对象或本地文件名（在这种情况下将使用文件的二进制内容）。如果需要指定 `path`，则使用这种格式。
         * Python 序列：这是一个便利的格式，可以指定上述任何组合。

         如果提供了 `path`，它必须是一个字符串。
         
         * 如果 `content` 是二进制数据或文件名，则此参数是 **必需的**，必须是数据的成员名称。
         * 否则此参数是可选的。它可以用来模拟文件夹名称或挂载点，在该点下可以找到子归档的元素。例如，`Archive((data, "name"), "path")` 表示 `data` 将通过元素名称 `"path/name"` 找到。对其他子归档也是类似的：要检索 ZIP 子归档的成员，必须在成员名前加上 `"path/"`。此参数的主要目的是区分重复的名称。

         .. note:: 如果归档中存在重复的条目名称，总是会返回最后一个具有该名称的条目。在创建归档或向归档追加更多数据时（请参见 :meth:`Archive.add`），不会进行重复检查。使用 `path` 参数可以防止此问题发生。

      .. method:: add(content [,path])

         添加一个子归档。参数含义与上述完全相同，当然，`content` 参数在此不可省略。

      .. method:: has_entry(name)

         检查在任何子归档中是否存在指定条目。

         :arg str name: 条目的完整名称，包括任何 `path` 前缀，该前缀表示条目所在的子归档。

         :returns: `True` 或 `False`。

      .. method:: read_entry(name)

         检索指定条目的数据。

         :arg str name: 条目的完整名称，包括任何 `path` 前缀，该前缀表示条目所在的子归档。

         :returns: 条目的二进制数据（`bytes`）。如果找不到条目，将引发异常。

      .. attribute:: entry_list

         归档的子归档列表。每个列表项是一个字典，包含以下键：

         * `entries` -- 该子归档中的（顶级）条目名称列表。
         * `fmt` -- 子归档的格式。可能是以下之一："dir"（文件夹）、"zip"（ZIP 归档）、"tar"（TAR 归档）或 "tree"（单一二进制条目或文件内容）。
         * `path` -- 添加此子归档时使用的 `path` 参数值。

         **示例：**

         >>> from pprint import pprint
         >>> import pymupdf
         >>> dir1 = "fitz-32"  # 文件夹名称
         >>> dir2 = "fitz-64"  # 文件夹名称
         >>> img = ("nur-ruhig.jpg", "img")  # 图像文件
         >>> members = (dir1, img, dir2)  # 我们希望一次性添加这些
         >>> arch = pymupdf.Archive()
         >>> arch.add(members, path="mypath")
         >>> pprint(arch.entry_list)
         [{'entries': ['310', '37', '38', '39'], 'fmt': 'dir', 'path': 'mypath'},
         {'entries': ['img'], 'fmt': 'tree', 'path': 'mypath'},
         {'entries': ['310', '311', '37', '38', '39', 'pypy'],
         'fmt': 'dir',
         'path': 'mypath'}]


.. tab:: 英文

   .. class:: Archive

      .. method:: __init__(self [, content [, path]])

         Creates a new archive. Without parameters, an empty archive is created.

         If provided, `content` may be one of the following:

         * another Archive: the archive is being made a sub-archive of the new one.

         * a string: this must be the name of a local folder or file. `pathlib.Path` objects are also supported.

            - A **folder** will be converted to a sub-archive, so its files (and any sub-folders) can be accessed by their names.
            - A **file** will be read with mode `"rb"` and these binary data (a `bytes` object) be treated as a single-member sub-archive. In this case, the `path` parameter is **mandatory** and should be the member name under which this item can be found / retrieved.

         * a `zipfile.ZipFile` or `tarfile.TarFile` object: Will be added as a sub-archive.

         * a Python binary object (`bytes`, `bytearray`, `io.BytesIO`): this will add a single-member sub-archive. In this case, the `path` parameter is **mandatory** and should be the member name under which this item can be found / retrieved.

         * a tuple `(data, name)`: This will add a single-member sub-archive with the member name ``name``. ``data`` may be a Python binary object or a local file name (in which case its binary file content is used). Use this format if you need to specify `path`.

         * a Python sequence: This is a convenience format to specify any combination of the above.

         If provided, `path` must be a string.
         
         * If `content` is either binary data or a file name, this parameter is mandatory and must be the name under which the data can be found.

         * Otherwise this parameter is optional. It can be used to simulate a folder name or a mount point, under which this sub-archive's elements can be found. For example this specification `Archive((data, "name"), "path")` means that `data` will be found using the element name `"path/name"`. Similar is true for other sub-archives: to retrieve members of a ZIP sub-archive, their names must be prefixed with `"path/"`. The main purpose of this parameter probably is to differentiate between duplicate names.

         .. note:: If duplicate entry names exist in the archive, always the last entry with that name will be found / retrieved. During archive creation, or appending more data to an archive (see :meth:`Archive.add`) no check for duplicates will be made. Use the `path` parameter to prevent this from happening.

      .. method:: add(content [,path])

         Append a sub-archive. The meaning of the parameters are exactly the same as explained above. Of course, parameter `content` is not optional here.

      .. method:: has_entry(name)

         Checks whether an entry exists in any of the sub-archives.

         :arg str name: The fully qualified name of the entry. So must include any `path` prefix under which the entry's sub-archive has been added.

         :returns: `True` or `False`.

      .. method:: read_entry(name)

         Retrieve the data of an entry.

         :arg str name: The fully qualified name of the entry. So must include any `path` prefix under which the entry's sub-archive has been added.

         :returns: The binary data (`bytes`) of the entry. If not found, an exception is raised.

      .. attribute:: entry_list

         A list of the archive's sub-archives. Each list item is a dictionary with the following keys:

         * `entries` -- a list of (top-level) entry names in this sub-archive.
         * `fmt` -- the format of the sub-archive. This is one of the strings "dir" (file folder), "zip" (ZIP archive), "tar" (TAR archive), or "tree" for single binary entries or file content.
         * `path` -- the value of the `path` parameter under which this sub-archive was added.

         **Example:**

         >>> from pprint import pprint
         >>> import pymupdf
         >>> dir1 = "fitz-32"  # a folder name
         >>> dir2 = "fitz-64"  # a folder name
         >>> img = ("nur-ruhig.jpg", "img")  # an image file
         >>> members = (dir1, img, dir2)  # we want to append these in one go
         >>> arch = pymupdf.Archive()
         >>> arch.add(members, path="mypath")
         >>> pprint(arch.entry_list)
         [{'entries': ['310', '37', '38', '39'], 'fmt': 'dir', 'path': 'mypath'},
         {'entries': ['img'], 'fmt': 'tree', 'path': 'mypath'},
         {'entries': ['310', '311', '37', '38', '39', 'pypy'],
         'fmt': 'dir',
         'path': 'mypath'}]
         >>> 

.. include:: footer.rst
