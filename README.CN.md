# PyMuPDF

**PyMuPDF** 是一个用于数据提取、分析、转换和处理 [PDF（及其他）文档](https://pymupdf.readthedocs.io/en/latest/the-basics.html#supported-file-types) 的高性能 **Python** 库。

# 社区

加入我们的 **Discord** 社区：[#pymupdf](https://discord.gg/TSpYGBW4eq)

# 安装

**PyMuPDF** 需要 **Python 3.9 或更高版本**，可以使用 **pip** 进行安装：

`pip install PyMuPDF`

它没有强制要求的外部依赖项。不过，只有在安装了额外软件包的情况下，一些[可选功能](#pymupdf-可选功能)才会可用。

您也可以在访问 [PyMuPDF.io](https://pymupdf.io/#examples) 时无需安装即可试用。

# 使用方法

基本用法如下：

```python
import pymupdf # 导入 pymupdf 库
doc = pymupdf.open("example.pdf") # 打开一个文档
for page in doc: # 遍历文档页面
  text = page.get_text() # 获取以 UTF-8 编码的纯文本
```

# 文档

完整的文档可以在 [pymupdf.readthedocs.io](https://pymupdf.readthedocs.io) 上找到。

# <a id="pymupdf-可选功能"></a>可选功能

• [fontTools](https://pypi.org/project/fonttools/) 用于创建字体子集。
• [pymupdf-fonts](https://pypi.org/project/pymupdf-fonts/) 包含一些适用于您的文本输出的精美字体。
• [Tesseract-OCR](https://github.com/tesseract-ocr/tesseract) 用于图像和文档页面中的光学字符识别。

# 关于

**PyMuPDF** 为 [MuPDF](https://mupdf.com/) 添加了 **Python** 绑定和抽象层。MuPDF 是一款轻量级的 **PDF**、**XPS** 和 **电子书** 查看器、渲染器和工具包。**PyMuPDF** 和 **MuPDF** 均由 [Artifex Software, Inc](https://artifex.com) 维护和开发。

**PyMuPDF** 最初由 [Jorj X. McKie](mailto:jorj.x.mckie@outlook.de) 编写。

# 许可证和版权

**PyMuPDF** 可根据 [开源 AGPL](https://www.gnu.org/licenses/agpl-3.0.html) 和商业许可协议使用。如果您确定无法满足 **AGPL** 的要求，请联系 [Artifex](https://artifex.com/contact/pymupdf-inquiry.php) 以获取有关商业许可证的更多信息 。
