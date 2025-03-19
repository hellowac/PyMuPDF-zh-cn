.. include:: header.rst

.. _RecipesMultiprocessing:


.. |toggleStart| raw:: html

   <details>
   <summary><a>See code</a></summary>

.. |toggleEnd| raw:: html

   </details>

==============================
多进程
==============================

Multiprocessing

.. tab:: 中文

   |PyMuPDF| 不支持在多个线程上运行——这样做可能会导致不正确的行为，甚至使 Python 崩溃。

   但是，可以通过使用 Python 的 *multiprocessing* 模块来以多种方式进行处理。

   如果你希望加速大文档的按页处理，可以使用以下脚本作为起点。它的处理速度至少是相应顺序处理的两倍。

   |toggleStart|

   .. literalinclude:: samples/multiprocess-render.py
      :language: python

   |toggleEnd|

   这是一个更复杂的示例，涉及主进程（显示 GUI）与子进程（执行 |PyMuPDF| 文档访问）之间的进程间通信。


.. tab:: 英文


   |PyMuPDF| does not support running on multiple threads - doing so may cause incorrect behaviour or even crash Python itself.

   However, there is the option to use :title:`Python's` *multiprocessing* module in a variety of ways.

   If you are looking to speed up page-oriented processing for a large document, use this script as a starting point. It should be at least twice as fast as the corresponding sequential processing.


   |toggleStart|

   .. literalinclude:: samples/multiprocess-render.py
      :language: python

   |toggleEnd|


   Here is a more complex example involving inter-process communication between a main process (showing a GUI) and a child process doing |PyMuPDF| access to a document.


|toggleStart|

.. literalinclude:: samples/multiprocess-gui.py
   :language: python

|toggleEnd|


.. include:: footer.rst
