.. include:: header.rst

.. _Algebra:

几何对象的运算符代数
======================================

Operator Algebra for Geometry Objects

.. highlight:: python

.. tab:: 中文

    类的实例：:ref:`Point`、 :ref:`IRect`、 :ref:`Rect`、 :ref:`Quad` 和 :ref:`Matrix` 被统称为“几何”对象。

    它们都是 Python 序列的特例，更多背景请参见 :ref:`SequenceTypes`。

    我们为这些类定义了操作符，使得它们在加法、减法、乘法、除法等方面几乎可以像普通数字一样进行操作。

    本章节概述了这些操作的可能性。

.. tab:: 英文

    Instances of classes :ref:`Point`, :ref:`IRect`, :ref:`Rect`, :ref:`Quad` and :ref:`Matrix` are collectively also called "geometry" objects.

    They all are special cases of Python sequences, see :ref:`SequenceTypes` for more background.

    We have defined operators for these classes that allow dealing with them (almost) like ordinary numbers in terms of addition, subtraction, multiplication, division, and some others.

    This chapter is a synopsis of what is possible. For details, please refer to the respective class documentation.

一般说明
-----------------

General Remarks

.. tab:: 中文

    1. 操作符可以是 **二元** 的（即涉及两个对象）或 **一元** 的。

    2. **二元** 操作的结果类型可以是 **左操作数类的新对象** ，一个布尔值，或者（对于点积）一个浮动数值。

    3. **一元** 操作的结果是 **相同类的新对象** ，一个布尔值或一个浮动数值。

    4. 二元操作符 `+`, `-`, `*`, `/` 对所有类都已定义。它们 *大致* 执行你所期待的操作 —— **不过，第二个操作数...**

        - 总是可以是一个数字，它会对第一个操作数的每个组件执行相应的操作，
        - 总是可以是一个与第一个操作数相同长度的数值序列（2、4或6个元素）——我们称这样的序列为 :data:`point_like`、 :data:`rect_like`、 :data:`quad_like` 或 :data:`matrix_like` ，分别对应点、矩形、四边形或矩阵。

    5. 矩形支持 **附加的二元** 操作： **交集** （操作符 `"&"`）， **并集** （操作符 `"|"`）以及 **包含性** 检查。

    6. 二元操作符完全支持就地操作。因此，如果“°”是一个二元操作符，那么表达式 `a °= b` 总是有效，并且等价于 `a = a ° b` 。因此，要小心， **不要** 对两个点执行 `p1 *= p2`，因为这会使得 "p1" 成为一个 **浮动数值** 。


.. tab:: 英文

    1. Operators can be either **binary** (i.e. involving two objects) or **unary**.

    2. The resulting type of **binary** operations is either a **new object of the left operand's class,** a bool or (for dot products) a float.

    3. The result of **unary** operations is either a **new object** of the same class, a bool or a float.

    4. The binary operators `+, -, *, /` are defined for all classes. They *roughly* do what you would expect -- **except, that the second operand ...**

        - may always be a number which then performs the operation on every component of the first one,
        - may always be a numeric sequence of the same length (2, 4 or 6) -- we call such sequences :data:`point_like`, :data:`rect_like`, :data:`quad_like` or :data:`matrix_like`, respectively.

    5. Rectangles support **additional binary** operations: **intersection** (operator `"&"`), **union** (operator `"|"`) and **containment** checking.

    6. Binary operators fully support in-place operations. So if "°" is a binary operator then the expression `a °= b` is always valid and the same as `a = a ° b`. Therefore, be careful and do **not** do `p1 *= p2` for two points, because thereafter "p1" is a **float**.


一元运算
------------------

Unary Operations

.. tab:: 中文

    =========== ===================================================================
    操作        结果  
    =========== ===================================================================
     bool(OBJ)  如果 OBJ 的所有分量都为零，则为假                
     abs(OBJ)   矩形面积——对于其他类型，等于norm(OBJ)
     norm(OBJ)  分量平方的平方根（欧几里得范数）
     +OBJ       OBJ 的新副本
     -OBJ       带有否定组件的 OBJ 新副本
     ~m         矩阵“m”的逆，如果不可逆则为零矩阵
    =========== ===================================================================

.. tab:: 英文

    =========== ===================================================================
    Oper.       Result
    =========== ===================================================================
     bool(OBJ)  is false exactly if all components of OBJ are zero
     abs(OBJ)   the rectangle area -- equal to norm(OBJ) for the other types
     norm(OBJ)  square root of the component squares (Euclidean norm)
     +OBJ       new copy of OBJ
     -OBJ       new copy of OBJ with negated components
     ~m         inverse of matrix "m", or the null matrix if not invertible
    =========== ===================================================================


二元运算
------------------

Binary Operations

.. tab:: 中文

    这些是像 `a ° b` 这样的表达式，其中“°”是任何操作符 `+`, `-`, `*`, `/`。二元操作也可以是形如 `a == b` 和 `b in a` 的表达式。

    如果“b”是一个数字，那么相应的操作将对“a”的每个组件执行。否则，如果“b”**不是一个数字**，那么将发生以下情况：


    ========= ===========================================================================
    操作符    结果  
    ========= ===========================================================================
    a+b, a-b  组件-wise 执行，“b”必须是“a-like”。
    a*m, a/m  "“a”可以是一个点、矩形或矩阵，而“m”是一个
              :data:`matrix_like`。*“a/m”* 被处理为 *“a*~m”* （参见下文关于不可逆矩阵的说
              明）。如果“a”是 **点** 或 **矩形** ，
              则执行 *“a.transform(m)”* 。如果“a”是矩阵，则进行矩阵连接。
    a*b       返回 **点积** ，对于点“a”和点-like 的“b”。
    a&b       **交集矩形：** “a”必须是矩形，“b”是 :data:`rect_like`。返回两个操作数中 **包含的最大矩形** 。
    a|b       **并集矩形：** “a”必须是矩形，且“b”可以是
              :data:`point_like` 或 :data:`rect_like`。
              返回两个操作数 **包含的最小矩形** 。
    b in a    如果“b”是数字，则返回 `b in tuple(a)`。
              如果“b”是 :data:`point_like`、:data:`rect_like` 或 :data:`quad_like`，
              则“a”必须是矩形，返回 `a.contains(b)`。
    a == b    如果 *bool(a-b)* 为 *False* ，则返回 *True* （“b”可以是“a-like”）。
    ========= ===========================================================================

    .. note:: 请注意与常规算术运算的一个重要区别：

                矩阵乘法是**不可交换的**，即通常情况下我们有 `m*n != n*m`，对于两个矩阵。此外，某些非零矩阵是没有逆矩阵的，例如 `m = Matrix(1, 0, 1, 0, 1, 0)`。如果你尝试除以这些矩阵，你将会遇到 `ZeroDivisionError` 异常，使用操作符 *"/"*，例如表达式 `pymupdf.Identity / m`。但是，如果你使用 `pymupdf.Identity * ~m`，结果将是 `pymupdf.Matrix()`（零矩阵）。

                诚然，这表示一个不一致，我们正在考虑删除这个行为。暂时，你可以选择避免异常并检查是否 ~m 是零矩阵，或者通过使用 `pymupdf.Identity / m` 接受潜在的 *ZeroDivisionError* 异常。

    .. note::

      * 根据这些约定，所有常规的代数规则都适用。例如，可以在**相同类的对象间**任意使用括号：如果 r1, r2 是矩形，m1, m2 是矩阵，你可以这样做 `(r1 + r2) * m1 * m2`。
      * 对于相同类的所有对象，`a + b + c == (a + b) + c == a + (b + c)` 是成立的。
      * 对于矩阵加法，以下等式成立：`(m1 + m2) * m3 == m1 * m3 + m2 * m3`（分配律）。
      * **但应用矩阵的顺序是很重要的：** 如果 r 是一个矩形，m1 和 m2 是矩阵，那么——**小心！:**

        - `r * m1 * m2 == (r * m1) * m2 != r * (m1 * m2)`。


.. tab:: 英文

    These are expressions like `a ° b` where "°" is any of the operators `+, -, *, /`. Also binary operations are expressions of the form `a == b` and `b in a`.

    If "b" is a number, then the respective operation is executed for each component of "a". Otherwise, if "b" is **not a number,** then the following happens:


    ========= ===========================================================================
    Oper.     Result
    ========= ===========================================================================
    a+b, a-b  component-wise execution, "b" must be "a-like".
    a*m, a/m  "a" can be a point, rectangle or matrix and "m" is a
              :data:`matrix_like`. *"a/m"* is treated as *"a*~m"* (see note below
              for non-invertible matrices). If "a" is a **point** or a **rectangle**,
              then *"a.transform(m)"* is executed. If "a" is a matrix, then
              matrix concatenation takes place.
    a*b       returns the **vector dot product** for a point "a" and point-like "b".
    a&b       **intersection rectangle:** "a" must be a rectangle and
              "b" :data:`rect_like`. Delivers the **largest rectangle**
              contained in both operands.
    a|b       **union rectangle:** "a" must be a rectangle, and "b" may be
              :data:`point_like` or :data:`rect_like`.
              Delivers the **smallest rectangle** containing both operands.
    b in a    if "b" is a number, then `b in tuple(a)` is returned.
              If "b" is :data:`point_like`, :data:`rect_like` or :data:`quad_like`,
              then "a" must be a rectangle, and `a.contains(b)` is returned.
    a == b    *True* if *bool(a-b)* is *False* ("b" may be "a-like").
    ========= ===========================================================================


    .. note:: Please note an important difference to usual arithmetic:

            Matrix multiplication is **not commutative**, i.e. in general we have `m*n != n*m` for two matrices. Also, there are non-zero matrices which have no inverse, for example `m = Matrix(1, 0, 1, 0, 1, 0)`. If you try to divide by any of these, you will receive a `ZeroDivisionError` exception using operator *"/"*, e.g. for the expression `pymupdf.Identity / m`. But if you formulate `pymupdf.Identity * ~m`, the result will be `pymupdf.Matrix()` (the null matrix).

            Admittedly, this represents an inconsistency, and we are considering to remove it. For the time being, you can choose to avoid an exception and check whether ~m is the null matrix, or accept a potential *ZeroDivisionError* by using `pymupdf.Identity / m`.

    .. note::

      * With these conventions, all the usual algebra rules apply. For example, arbitrarily using brackets **(among objects of the same class!)** is possible: if r1, r2 are rectangles and m1, m2 are matrices, you can do this `(r1 + r2) * m1 * m2`.
      * For all objects of the same class, `a + b + c == (a + b) + c == a + (b + c)` is true.
      * For matrices in addition the following is true: `(m1 + m2) * m3 == m1 * m3 + m2 * m3` (distributivity property).
      * **But the sequence of applying matrices is important:** If r is a rectangle and m1, m2 are matrices, then -- **caution!:**
      
        - `r * m1 * m2 == (r * m1) * m2 != r * (m1 * m2)`.

一些示例
--------------

Some Examples

数字操作
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Manipulation with numbers

.. tab:: 中文

    对于常规算术操作，数字始终允许作为第二操作数。此外，你还可以编写 `"x in OBJ"`，其中 `x` 是一个数字。这实现为 `"x in tuple(OBJ)"`：

      >>> pymupdf.Rect(1, 2, 3, 4) + 5
      pymupdf.Rect(6.0, 7.0, 8.0, 9.0)
      >>> 3 in pymupdf.Rect(1, 2, 3, 4)
      True
      >>> 

    以下将创建文档页面矩形的左上角四分之一区域：

      >>> page.rect
      Rect(0.0, 0.0, 595.0, 842.0)
      >>> page.rect / 2
      Rect(0.0, 0.0, 297.5, 421.0)
      >>> 

    以下将提供连接两个点 **p1** 和 **p2** 的线段的 **中点** ：

      >>> p1 = pymupdf.Point(1, 2)
      >>> p2 = pymupdf.Point(4711, 3141)
      >>> mp = (p1 + p2) / 2
      >>> mp
      Point(2356.0, 1571.5)
      >>> 

    计算两个点的**向量点积**。你可以计算它们之间的**角度余弦**并检查正交性。

      >>> p1 = pymupdf.Point(1, 0)
      >>> p2 = pymupdf.Point(1, 1)
      >>> dot = p1 * p2
      >>> dot
      1.0

      >>> # 计算 p1 和 p2 之间的角度余弦：
      >>> cosine = dot / (abs(p1) * abs(p2))
      >>> cosine  # 45 度角的余弦
      0.7071067811865475

      >>> math.cos(math.radians(45))  # 验证：
      0.7071067811865476

      >>> # 检查正交性
      >>> p3 = pymupdf.Point(0, 1)
      >>> # p1 和 p3 正交，因此，按预期：
      >>> p1 * p3  
      0.0


.. tab:: 英文

    For the usual arithmetic operations, numbers are always allowed as second operand. In addition, you can formulate `"x in OBJ"`, where x is a number. It is implemented as `"x in tuple(OBJ)"`::

      >>> pymupdf.Rect(1, 2, 3, 4) + 5
      pymupdf.Rect(6.0, 7.0, 8.0, 9.0)
      >>> 3 in pymupdf.Rect(1, 2, 3, 4)
      True
      >>> 

    The following will create the upper left quarter of a document page rectangle::

      >>> page.rect
      Rect(0.0, 0.0, 595.0, 842.0)
      >>> page.rect / 2
      Rect(0.0, 0.0, 297.5, 421.0)
      >>> 

    The following will deliver the **middle point of a line** that connects two points **p1** and **p2**::

      >>> p1 = pymupdf.Point(1, 2)
      >>> p2 = pymupdf.Point(4711, 3141)
      >>> mp = (p1 + p2) / 2
      >>> mp
      Point(2356.0, 1571.5)
      >>> 

    Compute the **vector dot product** of two points. You can compute the **cosine of angles** and check orthogonality.

      >>> p1 = pymupdf.Point(1, 0)
      >>> p2 = pymupdf.Point(1, 1)
      >>> dot = p1 * p2
      >>> dot
      1.0

      >>> # compute the cosine of the angle between p1 and p2:
      >>> cosine = dot / (abs(p1) * abs(p2))
      >>> cosine  # cosine of 45 degrees
      0.7071067811865475

      >>> math.cos(mat.radians(45))  # verify:
      0.7071067811865476

      >>> # check orhogonality
      >>> p3 = pymupdf.Point(0, 1)
      >>> # p1 and p3 are orthogonal so, as expected:
      >>> p1 * p3  
      0.0


使用“类似”对象进行操作
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Manipulation with "like" Objects

.. tab:: 中文

    二元操作的第二个操作数始终可以是“像”左操作数的对象。在这里，“像”意味着“一个与左操作数具有相同长度的数字序列”。以下是一些示例：

      >>> p1 + p2
      Point(4712.0, 3143.0)
      >>> p1 + (4711, 3141)
      Point(4712.0, 3143.0)
      >>> p1 += (4711, 3141)
      >>> p1
      Point(4712.0, 3143.0)
      >>> 

    要将一个矩形向右移动 5 像素，可以执行以下操作：

      >>> pymupdf.Rect(100, 100, 200, 200) + (5, 0, 5, 0)  # 将 x 坐标加 5
      Rect(105.0, 100.0, 205.0, 200.0)
      >>> 

    点、矩形和矩阵可以通过矩阵进行 *变换* 。在 PyMuPDF 中，我们将此操作视为一种 **“乘法”** （或相应的 **“除法”** ），其中第二个操作数可以是“像”矩阵。在这个上下文中，除法表示“与倒矩阵的乘法”：

      >>> m = pymupdf.Matrix(1, 2, 3, 4, 5, 6)
      >>> n = pymupdf.Matrix(6, 5, 4, 3, 2, 1)
      >>> p = pymupdf.Point(1, 2)
      >>> p * m
      Point(12.0, 16.0)
      >>> p * (1, 2, 3, 4, 5, 6)
      Point(12.0, 16.0)
      >>> p / m
      Point(2.0, -2.0)
      >>> p / (1, 2, 3, 4, 5, 6)
      Point(2.0, -2.0)
      >>> 
      >>> m * n  # 矩阵乘法
      Matrix(14.0, 11.0, 34.0, 27.0, 56.0, 44.0)
      >>> m / n  # 矩阵除法
      Matrix(2.5, -3.5, 3.5, -4.5, 5.5, -7.5)
      >>> 
      >>> m / m  # 结果等于单位矩阵
      Matrix(1.0, 0.0, 0.0, 1.0, 0.0, 0.0)
      >>> 
      >>> # 看看这个不可逆矩阵：
      >>> m = pymupdf.Matrix(1, 0, 1, 0, 1, 0)
      >>> ~m
      Matrix(0.0, 0.0, 0.0, 0.0, 0.0, 0.0)
      >>> # 我们尝试两种方式进行除法：
      >>> p = pymupdf.Point(1, 2)
      >>> p * ~m  # 结果是点 (0, 0)：
      Point(0.0, 0.0)
      >>> p / m  # 但是这会引发异常：
      Traceback (most recent call last):
        File "<pyshell#6>", line 1, in <module>
          p / m
        File "... /site-packages/fitz/pymupdf.py", line 869, in __truediv__
          raise ZeroDivisionError("matrix not invertible")
      ZeroDivisionError: matrix not invertible
      >>>


    作为特例，矩形支持额外的二元操作：

    * **交集** -- 矩形相交的区域，操作符 *"&"*
    * **包含** -- 扩展以包含一个点或矩形，操作符 *"|"*
    * **包含检查** -- 检查一个点或矩形是否在内部

    以下是创建包含给定点的最小矩形的示例：

      >>> # 首先定义一些点
      >>> points = []
      >>> for i in range(10):
              for j in range(10):
                  points.append((i, j))
      >>>
      >>> # 现在创建一个包含所有这 100 个点的矩形
      >>> # 从一个空矩形开始
      >>> r = pymupdf.Rect(points[0], points[0])
      >>> for p in points[1:]:  # 逐一包含剩余的点
              r |= p
      >>> r  # 这是预期的结果：
      Rect(0.0, 0.0, 9.0, 9.0)
      >>> (4, 5) in r  # 这个点在矩形内部
      True
      >>> # 这个矩形也在里面
      >>> (4, 4, 5, 5) in r
      True
      >>>


.. tab:: 英文

    The second operand of a binary operation can always be "like" the left operand. "Like" in this context means "a sequence of numbers of the same length". With the above examples::

      >>> p1 + p2
      Point(4712.0, 3143.0)
      >>> p1 + (4711, 3141)
      Point(4712.0, 3143.0)
      >>> p1 += (4711, 3141)
      >>> p1
      Point(4712.0, 3143.0)
      >>> 

    To shift a rectangle for 5 pixels to the right, do this::

      >>> pymupdf.Rect(100, 100, 200, 200) + (5, 0, 5, 0)  # add 5 to the x coordinates
      Rect(105.0, 100.0, 205.0, 200.0)
      >>>

    Points, rectangles and matrices can be *transformed* with matrices. In PyMuPDF, we treat this like a **"multiplication"** (or resp. **"division"**), where the second operand may be "like" a matrix. Division in this context means "multiplication with the inverted matrix"::

      >>> m = pymupdf.Matrix(1, 2, 3, 4, 5, 6)
      >>> n = pymupdf.Matrix(6, 5, 4, 3, 2, 1)
      >>> p = pymupdf.Point(1, 2)
      >>> p * m
      Point(12.0, 16.0)
      >>> p * (1, 2, 3, 4, 5, 6)
      Point(12.0, 16.0)
      >>> p / m
      Point(2.0, -2.0)
      >>> p / (1, 2, 3, 4, 5, 6)
      Point(2.0, -2.0)
      >>>
      >>> m * n  # matrix multiplication
      Matrix(14.0, 11.0, 34.0, 27.0, 56.0, 44.0)
      >>> m / n  # matrix division
      Matrix(2.5, -3.5, 3.5, -4.5, 5.5, -7.5)
      >>>
      >>> m / m  # result is equal to the Identity matrix
      Matrix(1.0, 0.0, 0.0, 1.0, 0.0, 0.0)
      >>>
      >>> # look at this non-invertible matrix:
      >>> m = pymupdf.Matrix(1, 0, 1, 0, 1, 0)
      >>> ~m
      Matrix(0.0, 0.0, 0.0, 0.0, 0.0, 0.0)
      >>> # we try dividing by it in two ways:
      >>> p = pymupdf.Point(1, 2)
      >>> p * ~m  # this delivers point (0, 0):
      Point(0.0, 0.0)
      >>> p / m  # but this is an exception:
      Traceback (most recent call last):
        File "<pyshell#6>", line 1, in <module>
          p / m
        File "... /site-packages/fitz/pymupdf.py", line 869, in __truediv__
          raise ZeroDivisionError("matrix not invertible")
      ZeroDivisionError: matrix not invertible
      >>>


    As a specialty, rectangles support additional binary operations:

    * **intersection** -- the common area of rectangle-likes, operator *"&"*
    * **inclusion** -- enlarge to include a point-like or rect-like, operator *"|"*
    * **containment** check -- whether a point-like or rect-like is inside

    Here is an example for creating the smallest rectangle enclosing given points::

      >>> # first define some point-likes
      >>> points = []
      >>> for i in range(10):
              for j in range(10):
                  points.append((i, j))
      >>>
      >>> # now create a rectangle containing all these 100 points
      >>> # start with an empty rectangle
      >>> r = pymupdf.Rect(points[0], points[0])
      >>> for p in points[1:]:  # and include remaining points one by one
              r |= p
      >>> r  # here is the to be expected result:
      Rect(0.0, 0.0, 9.0, 9.0)
      >>> (4, 5) in r  # this point-like lies inside the rectangle
      True
      >>> # and this rect-like is also inside
      >>> (4, 4, 5, 5) in r
      True
      >>>

.. include:: footer.rst