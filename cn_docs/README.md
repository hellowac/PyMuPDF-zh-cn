# PyMuPDF 文档

欢迎阅读 PyMuPDF 文档。本文档依赖 [Sphinx](https://www.sphinx-doc.org/en/master/) 从使用 [reStructuredText](https://en.wikipedia.org/wiki/ReStructuredText)（RST）编写的 markdown 文件发布 HTML 文档。

## Sphinx 版本

本 README 假设您已在系统上安装了 [Sphinx v5.0.2](https://www.sphinx-doc.org/en/master/usage/installation.html)。

## 更新文档

在 `docs` 目录中更新相关的 reStructuredText (`.rst`) 文件。这些文件对应相应的文档页面。

## 构建 HTML 文档

- 确保已安装 `furo` 主题：

`pip install furo`

Furo 主题，版权所有 (c) 2020 Pradyun Gedam <mail@pradyunsg.me>，感谢：

https://github.com/pradyunsg/furo/blob/main/LICENSE

- 在 `docs` 目录运行：

`sphinx-build -b html. build/html`

这将在 `build/html` 中创建 HTML 文档。

> 使用：`sphinx-build -a -b html. build/html` 构建全部内容，包括 `_static` 中的资源（如果您更新了 CSS，这一点很重要）。

### 使用 Sphinx 自动构建

如果您正在积极更新文档，更好的构建方式是运行：

`sphinx-autobuild. _build/html`

这将在本地主机上提供文档，并在您进行编辑时实时自动更新页面。

### 构建日语文档

- -在 `docs` 目录运行：

`sphinx-build -a -b html -D language=ja. _build/html/ja`

- 在 `main` 分支上进行更改并与主要的 `en` .rst 文件同步后，在 `docs` 目录运行：

`sphinx-build -b gettext. _build/gettext`

然后：

`sphinx-intl update -p _build/gettext -l ja`

这将更新相应的 `po` 文件以供进一步编辑。然后检查这些文件中的 "#, fuzzy" 条目，因为新内容可能存在于那里并需要编辑。

## 构建 PDF 文档

- 首先确保已安装 [rst2pdf](https://pypi.org/project/rst2pdf/)：

`python -m pip install rst2pdf`

- 然后运行：

`sphinx-build -b pdf. build/pdf`

这将在 `build/pdf` 中为所有文档生成单个 PDF。

---

有关详细信息，请参阅：[Using Sphinx](https://www.sphinx-doc.org/en/master/usage/index.html)