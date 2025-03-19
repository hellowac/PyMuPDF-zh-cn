.. include:: header.rst

.. _Rect:

==========
Rect
==========

.. tab:: 中文

   *Rect* 表示由四个浮动点数 x0, y0, x1, y1 定义的矩形。它们被视为两个对角线上的点的坐标。前两个数字被视为“左上”角 P\ :sub:`(x0,y0)`，而 P\ :sub:`(x1,y1)` 为“右下”角。然而，这两个属性不一定与它们直观的含义相符 —— 请继续阅读。

   以下说明对于 :ref:`IRect` 对象同样适用：

   * 在 (Py-) MuPDF **(和 PDF)** 的意义上，矩形总是具有与 x 轴和 y 轴平行的边界。一般的正交四边形 **不是矩形** —— 这与数学定义不同。
   * 构造点可以（几乎！——见下文）位于平面中的任何位置 —— 它们不必不同，例如“左上”角也不一定是几何学上的“西北”点。
   * 单位为点，其中 72 个点等于 1 英寸。
   * 对于任何给定的四个数字，可以通过四种不同的方式定义几何上“相同”的矩形：
      1. Rect(P\ :sub:`(x0,y0)`, P\ :sub:`(x1,y1)`)
      2. Rect(P\ :sub:`(x1,y1)`, P\ :sub:`(x0,y0)`)
      3. Rect(P\ :sub:`(x0,y1)`, P\ :sub:`(x1,y0)`)
      4. Rect(P\ :sub:`(x1,y0)`, P\ :sub:`(x0,y1)`)

   **(在 v1.19.0 版本中更改)** 因此，做出如下分类：

   * 如果 `x0 <= x1` 且 `y0 <= y1`，则矩形称为 **有效** （即右下角点位于左上角点的“东南”方向），否则为 **无效** 。在上述四种方式中， **仅第一种** 是有效的。请注意，在 MuPDF 的坐标系统中，y 轴是从 **上到下** 定向的。无效矩形在早期版本中称为无限矩形。

   * 如果 `x0 >= x1` 或 `y0 >= y1`，则矩形称为 **空矩形**。这意味着 **无效矩形始终为空矩形**。并且，如果 `x0 > x1` （或 `y0 > y1` ），则 `width` （或 `height` ） **设置为零**。在早期版本中，矩形仅在宽度或高度为零时才为空。

   * 矩形坐标 **不能超出** 数值范围 `FZ_MIN_INF_RECT = -2147483648` 至 `FZ_MAX_INF_RECT = 2147483520`。这两个值被选择是因为它们是能够通过 C 浮动转换来存活的最小/最大的 32 位整数。在早期版本中，坐标值没有限制。

   * 有 **唯一的一个“无限”矩形**，由 `x0 = y0 = FZ_MIN_INF_RECT` 和 `x1 = y1 = FZ_MAX_INF_RECT` 定义。它包含所有其他矩形。它主要用于技术目的 —— 例如，当一个函数调用应忽略一个形式上要求的矩形参数时。这个矩形不是空的。

   * **矩形是（半）开区间的**：右边和底边（包括相应的角点）不被认为是矩形的一部分。这意味着，只有左上角 `(x0, y0)` 能属于矩形 —— 其他三个角点永远不会属于矩形。空矩形根本不包含任何角点。

   .. image:: images/img-rect-contains.*
      :scale: 30
      :align: center

   * 以下是更改概览。

   ================= =================================== ==================================================
   概念              版本 < 1.19.0                       版本 1.19.*    
   ================= =================================== ==================================================
   空矩形            x0 = x1 或 y0 = y1                  x0 >= x1 或 y0 >= y1 -- 包括无效矩形          
   有效矩形          不适用                              x0 <= x1 且 y0 <= y1 
   无限矩形          所有 x0 > x1 或 y1 > y0 的矩形      **仅有一个无限矩形 / irect!**         
   坐标值            所有数字                            `FZ_MIN_INF_RECT <= number <= FZ_MAX_INF_RECT`
   边界, 角点        是矩形的一部分                      右边和底边的角点和边界 **不在矩形内**
   ================= =================================== ==================================================

   * 有新的顶级函数定义了无限矩形和标准空矩形以及四边形，请参见 :meth:`INFINITE_RECT` 及其相关函数。


   ============================= =======================================================
   **方法 / 属性**               **简要描述**
   ============================= =======================================================
   :meth:`Rect.contains`         检查点或矩形是否包含在矩形内
   :meth:`Rect.get_area`         计算矩形面积
   :meth:`Rect.include_point`    扩展矩形以包含一个点
   :meth:`Rect.include_rect`     扩展矩形以包含另一个矩形
   :meth:`Rect.intersect`        计算与另一个矩形的公共部分
   :meth:`Rect.intersects`       检查是否存在非空交集
   :meth:`Rect.morph`            使用点和矩阵变换矩形
   :meth:`Rect.torect`           转换为另一个矩形的矩阵
   :meth:`Rect.norm`             欧几里得范数
   :meth:`Rect.normalize`        使矩形有效
   :meth:`Rect.round`            创建最小的 :ref:`Irect` 包含矩形
   :meth:`Rect.transform`        使用矩阵变换矩形
   :attr:`Rect.bottom_left`      左下角点，别名 *bl*
   :attr:`Rect.bottom_right`     右下角点，别名 *br*
   :attr:`Rect.height`           矩形高度
   :attr:`Rect.irect`            等于方法 *round()* 的结果
   :attr:`Rect.is_empty`         矩形是否为空
   :attr:`Rect.is_valid`         矩形是否有效
   :attr:`Rect.is_infinite`      矩形是否是无限矩形
   :attr:`Rect.top_left`         左上角点，别名 *tl*
   :attr:`Rect.top_right`        右上角点，别名 *tr*
   :attr:`Rect.quad`             由矩形角点构成的 :ref:`Quad`
   :attr:`Rect.width`            矩形宽度
   :attr:`Rect.x0`               左角点的 x 坐标
   :attr:`Rect.x1`               右角点的 x 坐标
   :attr:`Rect.y0`               上角点的 y 坐标
   :attr:`Rect.y1`               下角点的 y 坐标
   ============================= =======================================================

   **类 API**

   .. class:: Rect

      .. method:: __init__(self)

      .. method:: __init__(self, x0, y0, x1, y1)

      .. method:: __init__(self, top_left, bottom_right)

      .. method:: __init__(self, top_left, x1, y1)

      .. method:: __init__(self, x0, y0, bottom_right)

      .. method:: __init__(self, rect)

      .. method:: __init__(self, sequence)

         重载构造函数：*top_left*、*bottom_right* 代表 :data:`point_like` 对象，"sequence" 是一个包含 4 个数字的 Python 序列类型（见 :ref:`SequenceTypes`），"rect" 表示另一个 :data:`rect_like` 对象，而其他参数代表坐标。

         如果指定了 "rect"，则构造函数会创建它的 **新副本**。

         如果没有参数，则创建空矩形 *Rect(0.0, 0.0, 0.0, 0.0)*。

      .. method:: round()

         创建最小的包含 :ref:`IRect`。这 **不是** 简单地四舍五入矩形的边：左上角向上和向左四舍五入，而右下角向下和向右四舍五入。

         >>> pymupdf.Rect(0.5, -0.01, 123.88, 455.123456).round()
         IRect(0, -1, 124, 456)

         1. 如果矩形 **为空**，结果也为空。
         2. **可能的悖论：** 即使矩形 **不** 为空，结果也可能为空！在这种情况下，结果显然不包含该矩形。因为 MuPDF 的算法允许有一个小的公差（1e-3）。示例：

         >>> r = pymupdf.Rect(100, 100, 200, 100.001)
         >>> r.is_empty  # 矩形 **不是** 空的
         False
         >>> r.round()  # 但是它的 irect 是空的！
         pymupdf.IRect(100, 100, 200, 100)
         >>> r.round().is_empty
         True

         :rtype: :ref:`IRect`

      .. method:: transform(m)

         使用矩阵变换矩形并 **替换原始矩形**。如果矩形为空或无限，则不进行任何操作。

         :arg m: 变换矩阵。
         :type m: :ref:`Matrix`

         :rtype: *Rect*
         :returns: 包含变换后的矩形的最小矩形。

      .. method:: intersect(r)

         计算当前矩形与另一个矩形 `r` 的交集。如果没有交集，则返回空矩形。

         :arg r: 另一个矩形。
         :type r: :ref:`Rect`

         :rtype: *Rect*
         :returns: 当前矩形与给定矩形的交集。

      .. method:: include_rect(r)

         扩展矩形以包含给定的矩形 `r`，返回包含两个矩形的最小矩形。

         :arg r: 另一个矩形。
         :type r: :ref:`Rect`

         :rtype: *Rect*
         :returns: 扩展后的矩形。

      .. method:: norm()

         计算矩形的欧几里得范数。

         :rtype: *float*
         :returns: 欧几里得范数，即矩形的对角线长度。

      .. method:: contains(p)

         检查点 `p` 是否包含在矩形内。

         :arg p: 一个点。
         :type p: :ref:`Point`
         :rtype: *bool*
         :returns: 如果点在矩形内，则返回 `True`，否则返回 `False`。

      .. method:: is_infinite()

         检查矩形是否是“无限矩形”。

         :rtype: *bool*
         :returns: 如果矩形是无限的，则返回 `True`，否则返回 `False`。

      .. method:: is_empty()

         检查矩形是否为空。

         :rtype: *bool*
         :returns: 如果矩形为空，则返回 `True`，否则返回 `False`。

      .. method:: is_valid()

         检查矩形是否有效（即其左上角坐标小于右下角坐标）。

         :rtype: *bool*
         :returns: 如果矩形有效，则返回 `True`，否则返回 `False`。

.. tab:: 英文

   *Rect* represents a rectangle defined by four floating point numbers x0, y0, x1, y1. They are treated as being coordinates of two diagonally opposite points. The first two numbers are regarded as the "top left" corner P\ :sub:`(x0,y0)` and P\ :sub:`(x1,y1)` as the "bottom right" one. However, these two properties need not coincide with their intuitive meanings -- read on.

   The following remarks are also valid for :ref:`IRect` objects:

   * A rectangle in the sense of (Py-) MuPDF **(and PDF)** always has **borders parallel to the x- resp. y-axis**. A general orthogonal tetragon **is not a rectangle** -- in contrast to the mathematical definition.
   * The constructing points can be (almost! -- see below) anywhere in the plane -- they need not even be different, and e.g. "top left" need not be the geometrical "north-western" point.
   * Units are in points, where 72 points is 1 inch.
   * For any given quadruple of numbers, the geometrically "same" rectangle can be defined in four different ways:
      1. Rect(P\ :sub:`(x0,y0)`, P\ :sub:`(x1,y1)`\ )
      2. Rect(P\ :sub:`(x1,y1)`, P\ :sub:`(x0,y0)`\ )
      3. Rect(P\ :sub:`(x0,y1)`, P\ :sub:`(x1,y0)`\ )
      4. Rect(P\ :sub:`(x1,y0)`, P\ :sub:`(x0,y1)`\ )

   **(Changed in v1.19.0)** Hence some classification:

   * A rectangle is called **valid** if `x0 <= x1` and `y0 <= y1` (i.e. the bottom right point is "south-eastern" to the top left one), otherwise **invalid**. Of the four alternatives above, **only the first** is valid. Please take into account, that in MuPDF's coordinate system, the y-axis is oriented from **top to bottom**. Invalid rectangles have been called infinite in earlier versions.

   * A rectangle is called **empty** if `x0 >= x1` or `y0 >= y1`. This implies, that **invalid rectangles are also always empty.** And `width` (resp. `height`) is **set to zero** if `x0 > x1` (resp. `y0 > y1`). In previous versions, a rectangle was empty only if one of width or height was zero.

   * Rectangle coordinates **cannot be outside** the number range from `FZ_MIN_INF_RECT = -2147483648` to `FZ_MAX_INF_RECT = 2147483520`. Both values have been chosen, because they are the smallest / largest 32bit integers that survive C float conversion roundtrips. In previous versions there was no limit for coordinate values.

   * There is **exactly one "infinite" rectangle**, defined by `x0 = y0 = FZ_MIN_INF_RECT` and `x1 = y1 = FZ_MAX_INF_RECT`. It contains every other rectangle. It is mainly used for technical purposes -- e.g. when a function call should ignore a formally required rectangle argument. This rectangle is not empty.

   * **Rectangles are (semi-) open:** The right and the bottom edges (including the resp. corners) are not considered part of the rectangle. This implies, that only the top-left corner `(x0, y0)` can ever belong to the rectangle - the other three corners never do. An empty rectangle contains no corners at all.

   .. image:: images/img-rect-contains.*
      :scale: 30
      :align: center

   * Here is an overview of the changes.

   ================= =================================== ==================================================
   Notion            Versions < 1.19.0                   Versions 1.19.*
   ================= =================================== ==================================================
   empty             x0 = x1 or y0 = y1                  x0 >= x1 or y0 >= y1 -- includes invalid rects
   valid             n/a                                 x0 <= x1 and y0 <= y1
   infinite          all rects where x0 > x1 or y1 > y0  **exactly one infinite rect / irect!**
   coordinate values all numbers                         `FZ_MIN_INF_RECT <= number <= FZ_MAX_INF_RECT`
   borders, corners  are parts of the rectangle          right and bottom corners and edges **are outside**
   ================= =================================== ==================================================

   * There are new top level functions defining infinite and standard empty rectangles and quads, see :meth:`INFINITE_RECT` and friends.


   ============================= =======================================================
   **Methods / Attributes**      **Short Description**
   ============================= =======================================================
   :meth:`Rect.contains`         checks containment of point_likes and rect_likes
   :meth:`Rect.get_area`         calculate rectangle area
   :meth:`Rect.include_point`    enlarge rectangle to also contain a point
   :meth:`Rect.include_rect`     enlarge rectangle to also contain another one
   :meth:`Rect.intersect`        common part with another rectangle
   :meth:`Rect.intersects`       checks for non-empty intersections
   :meth:`Rect.morph`            transform with a point and a matrix
   :meth:`Rect.torect`           the matrix that transforms to another rectangle
   :meth:`Rect.norm`             the Euclidean norm
   :meth:`Rect.normalize`        makes a rectangle valid
   :meth:`Rect.round`            create smallest :ref:`Irect` containing rectangle
   :meth:`Rect.transform`        transform rectangle with a matrix
   :attr:`Rect.bottom_left`      bottom left point, synonym *bl*
   :attr:`Rect.bottom_right`     bottom right point, synonym *br*
   :attr:`Rect.height`           rectangle height
   :attr:`Rect.irect`            equals result of method *round()*
   :attr:`Rect.is_empty`         whether rectangle is empty
   :attr:`Rect.is_valid`         whether rectangle is valid
   :attr:`Rect.is_infinite`      whether rectangle is infinite
   :attr:`Rect.top_left`         top left point, synonym *tl*
   :attr:`Rect.top_right`        top_right point, synonym *tr*
   :attr:`Rect.quad`             :ref:`Quad` made from rectangle corners
   :attr:`Rect.width`            rectangle width
   :attr:`Rect.x0`               left corners' x coordinate
   :attr:`Rect.x1`               right corners' x -coordinate
   :attr:`Rect.y0`               top corners' y coordinate
   :attr:`Rect.y1`               bottom corners' y coordinate
   ============================= =======================================================

   **Class API**

   .. class:: Rect
      :no-index:

      .. method:: __init__(self)
         :no-index:

      .. method:: __init__(self, x0, y0, x1, y1)
         :no-index:

      .. method:: __init__(self, top_left, bottom_right)
         :no-index:

      .. method:: __init__(self, top_left, x1, y1)
         :no-index:

      .. method:: __init__(self, x0, y0, bottom_right)
         :no-index:

      .. method:: __init__(self, rect)
         :no-index:

      .. method:: __init__(self, sequence)
         :no-index:

         Overloaded constructors: *top_left*, *bottom_right* stand for :data:`point_like` objects, "sequence" is a Python sequence type of 4 numbers (see :ref:`SequenceTypes`), "rect" means another :data:`rect_like`, while the other parameters mean coordinates.

         If "rect" is specified, the constructor creates a **new copy** of it.

         Without parameters, the empty rectangle *Rect(0.0, 0.0, 0.0, 0.0)* is created.

      .. method:: round()
         :no-index:

         Creates the smallest containing :ref:`IRect`. This is **not** the same as simply rounding the rectangle's edges: The top left corner is rounded upwards and to the left while the bottom right corner is rounded downwards and to the right.

         >>> pymupdf.Rect(0.5, -0.01, 123.88, 455.123456).round()
         IRect(0, -1, 124, 456)

         1. If the rectangle is **empty**, the result is also empty.
         2. **Possible paradox:** The result may be empty, **even if** the rectangle is **not** empty! In such cases, the result obviously does **not** contain the rectangle. This is because MuPDF's algorithm allows for a small tolerance (1e-3). Example:

         >>> r = pymupdf.Rect(100, 100, 200, 100.001)
         >>> r.is_empty  # rect is NOT empty
         False
         >>> r.round()  # but its irect IS empty!
         pymupdf.IRect(100, 100, 200, 100)
         >>> r.round().is_empty
         True

         :rtype: :ref:`IRect`

      .. method:: transform(m)
         :no-index:

         Transforms the rectangle with a matrix and **replaces the original**. If the rectangle is empty or infinite, this is a no-operation.

         :arg m: The matrix for the transformation.
         :type m: :ref:`Matrix`

         :rtype: *Rect*
         :returns: the smallest rectangle that contains the transformed original.

      .. method:: intersect(r)
         :no-index:

         The intersection (common rectangular area, largest rectangle contained in both) of the current rectangle and *r* is calculated and **replaces the current** rectangle. If either rectangle is empty, the result is also empty. If *r* is infinite, this is a no-operation. If the rectangles are (mathematically) disjoint sets, then the result is invalid. If the result is valid but empty, then the rectangles touch each other in a corner or (part of) a side.

         :arg r: Second rectangle
         :type r: :ref:`Rect`

      .. method:: include_rect(r)
         :no-index:

         The smallest rectangle containing the current one and *r* is calculated and **replaces the current** one. If either rectangle is infinite, the result is also infinite. If one is empty, the other one will be taken as the result.

         :arg r: Second rectangle
         :type r: :ref:`Rect`

      .. method:: include_point(p)
         :no-index:

         The smallest rectangle containing the current one and point *p* is calculated and **replaces the current** one. **The infinite rectangle remains unchanged.** To create a rectangle containing a series of points, start with (the empty) *pymupdf.Rect(p1, p1)* and successively include the remaining points.

         :arg p: Point to include.
         :type p: :ref:`Point`


      .. method:: get_area([unit])
         :no-index:

         Calculate the area of the rectangle and, with no parameter, equals *abs(rect)*. Like an empty rectangle, the area of an infinite rectangle is also zero. So, at least one of *pymupdf.Rect(p1, p2)* and *pymupdf.Rect(p2, p1)* has a zero area.

         :arg str unit: Specify required unit: respective squares of *px* (pixels, default), *in* (inches), *cm* (centimeters), or *mm* (millimeters).
         :rtype: float

      .. method:: contains(x)
         :no-index:

         Checks whether *x* is contained in the rectangle. It may be an *IRect*, *Rect*, *Point* or number. If *x* is an empty rectangle, this is always true. If the rectangle is empty this is always *False* for all non-empty rectangles and for all points. `x in rect` and `rect.contains(x)` are equivalent.

         :arg x: the object to check.
         :type x: :data:`rect_like` or :data:`point_like`.

         :rtype: bool

      .. method:: intersects(r)
         :no-index:

         Checks whether the rectangle and a :data:`rect_like` "r" contain a common non-empty :ref:`Rect`. This will always be *False* if either is infinite or empty.

         :arg rect_like r: the rectangle to check.

         :rtype: bool

      .. method:: torect(rect)
         :no-index:

         * New in version 1.19.3
         
         Compute the matrix which transforms this rectangle to a given one.

         :arg rect_like rect: the target rectangle. Must not be empty or infinite.
         :rtype: :ref:`Matrix`
         :returns: a matrix `mat` such that `self * mat = rect`. Can for example be used to transform between the page and the pixmap coordinates. See an example use here :ref:`RecipesImages_P`.

      .. method:: morph(fixpoint, matrix)
         :no-index:

         * New in version 1.17.0
         
         Return a new quad after applying a matrix to the rectangle using the fixed point `fixpoint`.

         :arg point_like fixpoint: the fixed point.
         :arg matrix_like matrix: the matrix.
         :returns: a new :ref:`Quad`. This a wrapper for the same-named quad method. If infinite, the infinite quad is returned.

      .. method:: norm()
         :no-index:

         * New in version 1.16.0
         
         Return the Euclidean norm of the rectangle treated as a vector of four numbers.

      .. method:: normalize()
         :no-index:

         **Replace** the rectangle with its valid version. This is done by shuffling the rectangle corners. After completion of this method, the bottom right corner will indeed be south-eastern to the top left one (but may still be empty).

      .. attribute:: irect
         :no-index:

         Equals result of method *round()*.

      .. attribute:: top_left
         :no-index:

      .. attribute:: tl
         :no-index:

         Equals *Point(x0, y0)*.

         :type: :ref:`Point`

      .. attribute:: top_right
         :no-index:

      .. attribute:: tr
         :no-index:

         Equals `Point(x1, y0)`.

         :type: :ref:`Point`

      .. attribute:: bottom_left
         :no-index:

      .. attribute:: bl
         :no-index:

         Equals `Point(x0, y1)`.

         :type: :ref:`Point`

      .. attribute:: bottom_right
         :no-index:

      .. attribute:: br
         :no-index:

         Equals `Point(x1, y1)`.

         :type: :ref:`Point`

      .. attribute:: quad
         :no-index:

         The quadrilateral `Quad(rect.tl, rect.tr, rect.bl, rect.br)`.

         :type: :ref:`Quad`

      .. attribute:: width
         :no-index:

         Width of the rectangle. Equals `max(x1 - x0, 0)`.

         :rtype: float

      .. attribute:: height
         :no-index:

         Height of the rectangle. Equals `max(y1 - y0, 0)`.

         :rtype: float

      .. attribute:: x0
         :no-index:

         X-coordinate of the left corners.

         :type: float

      .. attribute:: y0
         :no-index:

         Y-coordinate of the top corners.

         :type: float

      .. attribute:: x1
         :no-index:

         X-coordinate of the right corners.

         :type: float

      .. attribute:: y1
         :no-index:

         Y-coordinate of the bottom corners.

         :type: float

      .. attribute:: is_infinite
         :no-index:

         `True` if this is the infinite rectangle.

         :type: bool

      .. attribute:: is_empty
         :no-index:

         `True` if rectangle is empty.

         :type: bool

      .. attribute:: is_valid
         :no-index:

         `True` if rectangle is valid.

         :type: bool

   .. note::

      * This class adheres to the Python sequence protocol, so components can be accessed via their index, too. Also refer to :ref:`SequenceTypes`.
      * Rectangles can be used with arithmetic operators -- see chapter :ref:`Algebra`.

.. include:: footer.rst
