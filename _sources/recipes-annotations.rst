.. include:: header.rst

.. _RecipesAnnotations:

==============================
注释
==============================

annotations

.. _RecipesAnnotations_A:

如何添加和修改注释
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Add and Modify Annotations

.. tab:: 中文

   在 |PyMuPDF| 中，可以通过 :ref:`Page` 方法添加新的注释。一旦注释存在，就可以使用 :ref:`Annot` 类的方法对其进行大范围修改。

   注释 **只能** 插入到 |PDF| 页面中——其他文档类型不支持注释插入。

   与许多其他工具不同，初始插入注释时仅需设置最少的属性。我们将诸如作者、创建日期或主题等属性的设置留给程序员自行处理。

   为了概览这些功能，请查看以下脚本，该脚本在 PDF 页面上填充了大部分可用的注释。请在接下来的章节中查看更多特殊情况：

   .. literalinclude:: samples/new-annots.py
      :language: python

   运行此脚本应会产生以下输出：

   .. image:: images/img-annots.*
      :scale: 80


.. tab:: 英文


   In |PyMuPDF|, new annotations can be added via :ref:`Page` methods. Once an annotation exists, it can be modified to a large extent using methods of the :ref:`Annot` class.

   Annotations can **only** be inserted in |PDF| pages - other document types do not support annotation insertion.

   In contrast to many other tools, initial insert of annotations happens with a minimum number of properties. We leave it to the programmer to e.g. set attributes like author, creation date or subject.

   As an overview for these capabilities, look at the following script that fills a PDF page with most of the available annotations. Look in the next sections for more special situations:

   .. literalinclude:: samples/new-annots.py
      :language: python


   This script should lead to the following output:

   .. image:: images/img-annots.*
      :scale: 80

------------------------------

.. _RecipesAnnotations_B:

如何使用 FreeText
~~~~~~~~~~~~~~~~~~~~~

How to Use FreeText

.. tab:: 中文

   此脚本展示了处理 'FreeText' 注释的几种基本方法：

   .. literalinclude:: samples/annotations-freetext1.py

   运行结果如下所示：

   .. image:: images/img-freetext1.*
      :scale: 80

   下面是一个使用富文本和标注线的示例：

   .. literalinclude:: samples/annotations-freetext2.py

   运行结果如下所示：

   .. image:: images/img-freetext2.*
      :scale: 80


.. tab:: 英文

   This script shows a couple of basic ways to deal with 'FreeText' annotations:

   .. literalinclude:: samples/annotations-freetext1.py

   The result looks like this:

   .. image:: images/img-freetext1.*
      :scale: 80

   Here is an example for using rich text and call-out lines:

   .. literalinclude:: samples/annotations-freetext2.py

   The result looks like this:

   .. image:: images/img-freetext2.*
      :scale: 80


------------------------------



.. _RecipesAnnotations_C:

如何使用墨迹注释
~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Use Ink Annotations

.. tab:: 中文

   墨迹（Ink）注释用于包含手写涂鸦。一个典型示例是包含名字和姓氏的签名图像。从技术上讲，墨迹注释实现为 **点列表的列表**。每个点列表被视为一条连续的线段，连接其中的各个点。不同的点列表表示注释中的独立线段。

   以下脚本创建了一个墨迹注释，其中包含两条数学曲线（正弦和余弦函数图）作为线段：

   .. literalinclude:: samples/annotations-ink.py

   运行结果如下所示：

   .. image:: images/img-inkannot.*
      :scale: 50


.. tab:: 英文

   Ink annotations are used to contain freehand scribbling. A typical example may be an image of your signature consisting of first name and last name. Technically an ink annotation is implemented as a **list of lists of points**. Each point list is regarded as a continuous line connecting the points. Different point lists represent independent line segments of the annotation.

   The following script creates an ink annotation with two mathematical curves (sine and cosine function graphs) as line segments:

   .. literalinclude:: samples/annotations-ink.py

   This is the result:

   .. image:: images/img-inkannot.*
      :scale: 50

.. include:: footer.rst
