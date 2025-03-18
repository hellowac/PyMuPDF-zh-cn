.. include:: header.rst

.. _Matrix:

==========
Matrix
==========

.. tab:: 中文

   矩阵是一个用于图像变换的行优先 3x3 矩阵（符合 :ref:`AdobeManual` 中定义的相应概念）。通过矩阵，你可以以多种方式操控页面的渲染图像：（页面的）某些部分可以被旋转、缩放、翻转、剪切和平移，方法是设置六个浮动值中的一些或全部。

   由于所有点或像素都位于二维空间中，因此该矩阵的一个列向量是一个常数单位向量，只有剩下的六个元素用于操作。这六个元素通常表示为 *[a, b, c, d, e, f]*。它们在矩阵中的位置如下所示：

   .. image:: images/img-matrix.*

   请注意：

      * 以下方法只是便利函数 -- 它们所做的一切，也可以通过直接操作六个数值来实现。
      * 所有操作可以组合使用 -- 你可以在一次操作中构建一个同时进行旋转、剪切、缩放和平移等变换的矩阵。如果你选择这样做，请查看下面的 **备注** 或 :ref:`AdobeManual`。

   ================================ ==============================================
   **方法 / 属性**                  **描述**
   ================================ ==============================================
   :meth:`Matrix.prerotate`         执行旋转
   :meth:`Matrix.prescale`          执行缩放
   :meth:`Matrix.preshear`          执行剪切（倾斜）
   :meth:`Matrix.pretranslate`      执行平移（位移）
   :meth:`Matrix.concat`            执行矩阵乘法
   :meth:`Matrix.invert`            计算逆矩阵
   :meth:`Matrix.norm`              欧几里得范数
   :attr:`Matrix.a`                 X 方向缩放因子
   :attr:`Matrix.b`                 Y 方向剪切效果
   :attr:`Matrix.c`                 X 方向剪切效果
   :attr:`Matrix.d`                 Y 方向缩放因子
   :attr:`Matrix.e`                 水平位移
   :attr:`Matrix.f`                 垂直位移
   :attr:`Matrix.is_rectilinear`    如果矩形角仍然是矩形角则为 True
   ================================ ==============================================

   **类 API**

   .. class:: Matrix

      .. method:: __init__(self)

      .. method:: __init__(self, zoom-x, zoom-y)

      .. method:: __init__(self, shear-x, shear-y, 1)

      .. method:: __init__(self, a, b, c, d, e, f)

      .. method:: __init__(self, matrix)

      .. method:: __init__(self, degree)

      .. method:: __init__(self, sequence)

         重载构造函数。

         如果没有参数，将创建零矩阵 *Matrix(0.0, 0.0, 0.0, 0.0, 0.0, 0.0)*。

         *zoom-** 和 *shear-** 分别指定缩放或剪切值（浮动），并分别创建缩放或剪切矩阵。

         对于“matrix”，会创建另一个矩阵的 **新副本** 。

         浮动值“degree”指定创建一个逆时针旋转矩阵。

         “sequence”必须是一个包含 6 个浮动条目的 Python 序列对象（见 :ref:`SequenceTypes`）。

         *pymupdf.Matrix(1, 1)* 和 *pymupdf.Matrix(pymupdf.Identity)* 创建可修改版本的 :ref:`Identity` 矩阵，其形态为 *[1, 0, 0, 1, 0, 0]*。

      .. method:: norm()

         * 版本 1.16.0 新增
         
         返回矩阵的欧几里得范数作为向量。

      .. method:: prerotate(deg)

         修改矩阵以执行逆时针旋转，旋转角度为正 *deg* 度，反之为顺时针。单位矩阵的矩阵元素将按以下方式更改：

         *[1, 0, 0, 1, 0, 0] -> [cos(deg), sin(deg), -sin(deg), cos(deg), 0, 0]*。

         :arg float deg: 旋转角度（以 Pi = 180 度为基础使用常规符号）。

      .. method:: prescale(sx, sy)

         修改矩阵以按缩放因子 sx 和 sy 进行缩放。仅影响属性 *a* 到 *d*： *[a, b, c, d, e, f] -> [a*sx, b*sx, c*sy, d*sy, e, f]*。

         :arg float sx: X 方向的缩放因子。效果见属性 *a* 的描述。

         :arg float sy: Y 方向的缩放因子。效果见属性 *d* 的描述。

      .. method:: preshear(sx, sy)

         修改矩阵以执行剪切操作，即将矩形转换为平行四边形（菱形）。仅影响属性 *a* 到 *d*： *[a, b, c, d, e, f] -> [c*sy, d*sy, a*sx, b*sx, e, f]*。

         :arg float sx: X 方向的剪切效果。见属性 *c*。

         :arg float sy: Y 方向的剪切效果。见属性 *b*。

      .. method:: pretranslate(tx, ty)

         修改矩阵以沿 x 和/或 y 轴执行位移 / 平移操作。仅影响属性 *e* 和 *f*： *[a, b, c, d, e, f] -> [a, b, c, d, tx*a + ty*c, tx*b + ty*d]*。

         :arg float tx: X 方向的平移效果。见属性 *e*。

         :arg float ty: Y 方向的平移效果。见属性 *f*。

      .. method:: concat(m1, m2)

         计算矩阵积 *m1 * m2* 并将结果存储在当前矩阵中。*m1* 或 *m2* 中的任何一个可以是当前矩阵。请注意，矩阵乘法是非交换的。所以 *m1* 和 *m2* 的顺序很重要。

         :arg m1: 第一个（左）矩阵。
         :type m1: :ref:`Matrix`

         :arg m2: 第二个（右）矩阵。
         :type m2: :ref:`Matrix`

      .. method:: invert(m = None)

         计算 *m* 的矩阵逆，并将结果存储在当前矩阵中。如果 *m* 不可逆（“退化”），则返回 *1*。在这种情况下，当前矩阵 **不会改变** 。如果 *m* 可逆，则返回 *0*，并且当前矩阵被替换为 *m* 的逆矩阵。

         :arg m: 要反转的矩阵。如果未提供，则使用当前矩阵。
         :type m: :ref:`Matrix`

         :rtype: int

      .. attribute:: a

         X 方向的缩放因子 **(宽度)**。例如，值为 0.5 时， **宽度** 会缩小一倍。如果 a < 0，则会发生左右翻转。

         :type: float

      .. attribute:: b

         引起剪切效果：每个 `Point(x, y)` 将变为 `Point(x, y - b*x)`。因此，水平线将“倾斜”。

         :type: float

      .. attribute:: c

         引起剪切效果：每个 `Point(x, y)` 将变为 `Point(x - c*y, y)`。因此，垂直线将“倾斜”。

         :type: float

      .. attribute:: d

         Y 方向的缩放因子 **(高度)**。例如，值为 1.5 时， **高度** 会拉伸 50%。如果 d < 0，则会发生上下翻转。

         :type: float

      .. attribute:: e

         引起水平位移效果：每个 *Point(x, y)* 将变为 *Point(x + e, y)*。正（负）值的 *e* 将向右（左）位移。

         :type: float

      .. attribute:: f

         引起垂直位移效果：每个 *Point(x, y)* 将变为 *Point(x, y - f)*。正（负）值的 *f* 将向下（上）位移。

         :type: float

      .. attribute:: is_rectilinear

         直线性意味着没有剪切，并且任何旋转都是 90 度的整数倍。通常用于确认（轴对齐的）矩形在变换前后仍然是轴对齐矩形。

         :type: bool

   .. note::

      * 本类遵循 Python 序列协议，因此可以通过索引访问组件。参见 :ref:`SequenceTypes`。
      * 矩阵可以像普通数字一样与算术运算符一起使用：它们可以加、减、乘或除 -- 见 :ref:`Algebra` 章节。
      * 矩阵乘法是 **非交换的** -- 改变乘法因子的顺序通常会改变结果。因此，变换的结果可能会变得不明确。


.. tab:: 英文

   Matrix is a row-major 3x3 matrix used by image transformations in MuPDF (which complies with the respective concepts laid down in the :ref:`AdobeManual`). With matrices you can manipulate the rendered image of a page in a variety of ways: (parts of) the page can be rotated, zoomed, flipped, sheared and shifted by setting some or all of just six float values.


   Since all points or pixels live in a two-dimensional space, one column vector of that matrix is a constant unit vector, and only the remaining six elements are used for manipulations. These six elements are usually represented by *[a, b, c, d, e, f]*. Here is how they are positioned in the matrix:

   .. image:: images/img-matrix.*


   Please note:

      * the below methods are just convenience functions -- everything they do, can also be achieved by directly manipulating the six numerical values
      * all manipulations can be combined -- you can construct a matrix that rotates **and** shears **and** scales **and** shifts, etc. in one go. If you however choose to do this, do have a look at the **remarks** further down or at the :ref:`AdobeManual`.

   ================================ ==============================================
   **Method / Attribute**             **Description**
   ================================ ==============================================
   :meth:`Matrix.prerotate`         perform a rotation
   :meth:`Matrix.prescale`          perform a scaling
   :meth:`Matrix.preshear`          perform a shearing (skewing)
   :meth:`Matrix.pretranslate`      perform a translation (shifting)
   :meth:`Matrix.concat`            perform a matrix multiplication
   :meth:`Matrix.invert`            calculate the inverted matrix
   :meth:`Matrix.norm`              the Euclidean norm
   :attr:`Matrix.a`                 zoom factor X direction
   :attr:`Matrix.b`                 shearing effect Y direction
   :attr:`Matrix.c`                 shearing effect X direction
   :attr:`Matrix.d`                 zoom factor Y direction
   :attr:`Matrix.e`                 horizontal shift
   :attr:`Matrix.f`                 vertical shift
   :attr:`Matrix.is_rectilinear`    true if rect corners will remain rect corners
   ================================ ==============================================

   **Class API**

   .. class:: Matrix
      :no-index:

      .. method:: __init__(self)
         :no-index:

      .. method:: __init__(self, zoom-x, zoom-y)
         :no-index:

      .. method:: __init__(self, shear-x, shear-y, 1)
         :no-index:

      .. method:: __init__(self, a, b, c, d, e, f)
         :no-index:

      .. method:: __init__(self, matrix)
         :no-index:

      .. method:: __init__(self, degree)
         :no-index:

      .. method:: __init__(self, sequence)
         :no-index:

         Overloaded constructors.

         Without parameters, the zero matrix *Matrix(0.0, 0.0, 0.0, 0.0, 0.0, 0.0)* will be created.

         *zoom-** and *shear-** specify zoom or shear values (float) and create a zoom or shear matrix, respectively.

         For "matrix" a **new copy** of another matrix will be made.

         Float value "degree" specifies the creation of a rotation matrix which rotates anti-clockwise.

         A "sequence" must be any Python sequence object with exactly 6 float entries (see :ref:`SequenceTypes`).

         *pymupdf.Matrix(1, 1)* and *pymupdf.Matrix(pymupdf.Identity)* create modifiable versions of the :ref:`Identity` matrix, which looks like *[1, 0, 0, 1, 0, 0]*.

      .. method:: norm()
         :no-index:

         * New in version 1.16.0
         
         Return the Euclidean norm of the matrix as a vector.

      .. method:: prerotate(deg)
         :no-index:

         Modify the matrix to perform a counter-clockwise rotation for positive *deg* degrees, else clockwise. The matrix elements of an identity matrix will change in the following way:

         *[1, 0, 0, 1, 0, 0] -> [cos(deg), sin(deg), -sin(deg), cos(deg), 0, 0]*.

         :arg float deg: The rotation angle in degrees (use conventional notation based on Pi = 180 degrees).

      .. method:: prescale(sx, sy)
         :no-index:

         Modify the matrix to scale by the zoom factors sx and sy. Has effects on attributes *a* thru *d* only: *[a, b, c, d, e, f] -> [a*sx, b*sx, c*sy, d*sy, e, f]*.

         :arg float sx: Zoom factor in X direction. For the effect see description of attribute *a*.

         :arg float sy: Zoom factor in Y direction. For the effect see description of attribute *d*.

      .. method:: preshear(sx, sy)
         :no-index:

         Modify the matrix to perform a shearing, i.e. transformation of rectangles into parallelograms (rhomboids). Has effects on attributes *a* thru *d* only: *[a, b, c, d, e, f] -> [c*sy, d*sy, a*sx, b*sx, e, f]*.

         :arg float sx: Shearing effect in X direction. See attribute *c*.

         :arg float sy: Shearing effect in Y direction. See attribute *b*.

      .. method:: pretranslate(tx, ty)
         :no-index:

         Modify the matrix to perform a shifting / translation operation along the x and / or y axis. Has effects on attributes *e* and *f* only: *[a, b, c, d, e, f] -> [a, b, c, d, tx*a + ty*c, tx*b + ty*d]*.

         :arg float tx: Translation effect in X direction. See attribute *e*.

         :arg float ty: Translation effect in Y direction. See attribute *f*.

      .. method:: concat(m1, m2)
         :no-index:

         Calculate the matrix product *m1 * m2* and store the result in the current matrix. Any of *m1* or *m2* may be the current matrix. Be aware that matrix multiplication is not commutative. So the sequence of *m1*, *m2* is important.

         :arg m1: First (left) matrix.
         :type m1: :ref:`Matrix`

         :arg m2: Second (right) matrix.
         :type m2: :ref:`Matrix`

      .. method:: invert(m = None)
         :no-index:

         Calculate the matrix inverse of *m* and store the result in the current matrix. Returns *1* if *m* is not invertible ("degenerate"). In this case the current matrix **will not change**. Returns *0* if *m* is invertible, and the current matrix is replaced with the inverted *m*.

         :arg m: Matrix to be inverted. If not provided, the current matrix will be used.
         :type m: :ref:`Matrix`

         :rtype: int

      .. attribute:: a
         :no-index:

         Scaling in X-direction **(width)**. For example, a value of 0.5 performs a shrink of the **width** by a factor of 2. If a < 0, a left-right flip will (additionally) occur.

         :type: float

      .. attribute:: b
         :no-index:

         Causes a shearing effect: each `Point(x, y)` will become `Point(x, y - b*x)`. Therefore, horizontal lines will be "tilt".

         :type: float

      .. attribute:: c
         :no-index:

         Causes a shearing effect: each `Point(x, y)` will become `Point(x - c*y, y)`. Therefore, vertical lines will be "tilt".

         :type: float

      .. attribute:: d
         :no-index:

         Scaling in Y-direction **(height)**. For example, a value of 1.5 performs a stretch of the **height** by 50%. If d < 0, an up-down flip will (additionally) occur.

         :type: float

      .. attribute:: e
         :no-index:

         Causes a horizontal shift effect: Each *Point(x, y)* will become *Point(x + e, y)*. Positive (negative) values of *e* will shift right (left).

         :type: float

      .. attribute:: f
         :no-index:

         Causes a vertical shift effect: Each *Point(x, y)* will become *Point(x, y - f)*. Positive (negative) values of *f* will shift down (up).

         :type: float

      .. attribute:: is_rectilinear
         :no-index:

         Rectilinear means that no shearing is present and that any rotations are integer multiples of 90 degrees. Usually this is used to confirm that (axis-aligned) rectangles before the transformation are still axis-aligned rectangles afterwards.

         :type: bool

   .. note::

      * This class adheres to the Python sequence protocol, so components can be accessed via their index, too. Also refer to :ref:`SequenceTypes`.
      * Matrices can be used with arithmetic operators almost like ordinary numbers: they can be added, subtracted, multiplied or divided -- see chapter :ref:`Algebra`.
      * Matrix multiplication is **not commutative** -- changing the sequence of the multiplicands will change the result in general. So it can quickly become unclear which result a transformation will yield.


例子
-------------

Examples

.. tab:: 中文

   以下是一些示例，展示了一些可以实现的效果。所有图片都显示了一些文本，这些文本在某个矩阵的控制下相对于一个固定的参考点（红点）插入。

   1. :ref:`Identity` 矩阵不执行任何操作。

   .. image:: images/img-matrix-0.*
      :scale: 66

   2. 缩放矩阵 `Matrix(2, 0.5)` 在水平方向上按因子 2 拉伸，在垂直方向上按因子 0.5 收缩。

   .. image:: images/img-matrix-1.*
      :scale: 66

   3. 属性 :attr:`Matrix.e` 和 :attr:`Matrix.f` 分别在水平和垂直方向上平移。以下示例中，水平平移 10，垂直平移 20。

   .. image:: images/img-matrix-2.*
      :scale: 66

   4. 负的 :attr:`Matrix.a` 会导致左右翻转。

   .. image:: images/img-matrix-3.*
      :scale: 66

   5. 负的 :attr:`Matrix.d` 会导致上下翻转。

   .. image:: images/img-matrix-4.*
      :scale: 66

   6. 属性 :attr:`Matrix.b` 沿 x 轴向上/向下倾斜。

   .. image:: images/img-matrix-5.*
      :scale: 66

   7. 属性 :attr:`Matrix.c` 沿 y 轴向左/向右倾斜。

   .. image:: images/img-matrix-6.*
      :scale: 66

   8. 矩阵 `Matrix(beta)` 对正角度 `beta` 进行逆时针旋转。

   .. image:: images/img-matrix-7.*
      :scale: 66

   9. 展示对矩形的效果::

         import pymupdf
         
         # 仅定义和临时 PDF
         RED = (1, 0, 0)
         BLUE = (0, 0, 1)
         GREEN = (0, 1, 0)
         doc = pymupdf.open()
         page = doc.new_page()
         
         # 矩形
         r1 = pymupdf.Rect(100, 100, 200, 200)
         
         # 在 x 方向缩小 50%，在 y 方向放大 50%
         mat1 = pymupdf.Matrix(0.5, 1.5)
         
         # 在两个方向上平移 50
         mat2 = pymupdf.Matrix(1, 0, 0, 1, 50, 50)
         
         # 绘制相应的矩形
         page.draw_rect(r1, color=RED)  # 原始矩形
         page.draw_rect(r1 * mat1, color=GREEN)  # 缩放后的矩形
         page.draw_rect(r1 * mat2, color=BLUE)  # 平移后的矩形
         doc.ez_save("matrix-effects.pdf")


.. tab:: 英文

   Here are examples that illustrate some of the achievable effects. All pictures show some text, inserted under control of some matrix and relative to a fixed reference point (the red dot).

   1. The :ref:`Identity` matrix performs no operation.

   .. image:: images/img-matrix-0.*
      :scale: 66

   2. The scaling matrix `Matrix(2, 0.5)` stretches by a factor of 2 in horizontal, and shrinks by factor 0.5 in vertical direction.

   .. image:: images/img-matrix-1.*
      :scale: 66

   3. Attributes :attr:`Matrix.e` and :attr:`Matrix.f` shift horizontally and, respectively vertically. In the following 10 to the right and 20 down.

   .. image:: images/img-matrix-2.*
      :scale: 66

   4. A negative :attr:`Matrix.a` causes a left-right flip.

   .. image:: images/img-matrix-3.*
      :scale: 66

   5. A negative :attr:`Matrix.d` causes an up-down flip.

   .. image:: images/img-matrix-4.*
      :scale: 66

   6. Attribute :attr:`Matrix.b` tilts upwards / downwards along the x-axis.

   .. image:: images/img-matrix-5.*
      :scale: 66

   7. Attribute :attr:`Matrix.c` tilts left / right along the y-axis.

   .. image:: images/img-matrix-6.*
      :scale: 66

   8. Matrix `Matrix(beta)` performs counterclockwise rotations for positive angles `beta`.

   .. image:: images/img-matrix-7.*
      :scale: 66

   9. Show some effects on a rectangle::

         import pymupdf
         
         # just definitions and a temp PDF
         RED = (1, 0, 0)
         BLUE = (0, 0, 1)
         GREEN = (0, 1, 0)
         doc = pymupdf.open()
         page = doc.new_page()
         
         # rectangle
         r1 = pymupdf.Rect(100, 100, 200, 200)
         
         # scales down by 50% in x- and up by 50% in y-direction
         mat1 = pymupdf.Matrix(0.5, 1.5)
         
         # shifts by 50 in both directions
         mat2 = pymupdf.Matrix(1, 0, 0, 1, 50, 50)
         
         # draw corresponding rectangles
         page.draw_rect(r1, color=RED)  # original
         page.draw_rect(r1 * mat1, color=GREEN)  # scaled
         page.draw_rect(r1 * mat2, color=BLUE)  # shifted
         doc.ez_save("matrix-effects.pdf")


.. image:: images/img-matrix-9.*
   :scale: 66

.. include:: footer.rst
