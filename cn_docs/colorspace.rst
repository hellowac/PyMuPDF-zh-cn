.. include:: header.rst

.. _Colorspace:

================
Colorspace
================

.. tab:: 中文

   表示 :ref:`Pixmap` 的颜色空间。 

   **Class API**

   .. class:: Colorspace

      .. method:: __init__(self, n)

         构造函数

         :arg int n: 一个标识颜色空间的数字。可能的值有 :data:`CS_RGB`，:data:`CS_GRAY` 和 :data:`CS_CMYK`。

      .. attribute:: name

         标识颜色空间的名称。例如：*pymupdf.csCMYK.name = 'DeviceCMYK'*。

         :type: str

      .. attribute:: n

         定义一个像素颜色所需的字节数。例如：*pymupdf.csCMYK.n == 4*。

         :type: int


      **预定义的颜色空间**

      为了节省打字工作，存在用于三种可用情况的预定义颜色空间对象。

      * :data:`csRGB`  = *pymupdf.Colorspace(pymupdf.CS_RGB)*
      * :data:`csGRAY` = *pymupdf.Colorspace(pymupdf.CS_GRAY)*
      * :data:`csCMYK` = *pymupdf.Colorspace(pymupdf.CS_CMYK)*


.. tab:: 英文

   Represents the color space of a :ref:`Pixmap`.


   **Class API**

   .. class:: Colorspace
      :no-index:

      .. method:: __init__(self, n)
         :no-index:

         Constructor

         :arg int n: A number identifying the colorspace. Possible values are :data:`CS_RGB`, :data:`CS_GRAY` and :data:`CS_CMYK`.

      .. attribute:: name
         :no-index:

         The name identifying the colorspace. Example: *pymupdf.csCMYK.name = 'DeviceCMYK'*.

         :type: str

      .. attribute:: n
         :no-index:

         The number of bytes required to define the color of one pixel. Example: *pymupdf.csCMYK.n == 4*.

         :type: int


      **Predefined Colorspaces**

      For saving some typing effort, there exist predefined colorspace objects for the three available cases.

      * :data:`csRGB`  = *pymupdf.Colorspace(pymupdf.CS_RGB)*
      * :data:`csGRAY` = *pymupdf.Colorspace(pymupdf.CS_GRAY)*
      * :data:`csCMYK` = *pymupdf.Colorspace(pymupdf.CS_CMYK)*

.. include:: footer.rst