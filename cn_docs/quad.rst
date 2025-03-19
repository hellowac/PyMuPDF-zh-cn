.. include:: header.rst

.. _Quad:

==========
Quad
==========

.. tab:: 中文

   表示平面中的四边形（也叫“矩形”或“四边形”），由四个 :ref:`Point` 对象组成，分别是 ul、ur、ll、lr（分别称为左上角、右上角、左下角、右下角）。

   四边形可以 **通过** 文本搜索方法获得（如： :meth:`Page.search_for`），并且 **用于** 定义文本标记注释（参见例如： :meth:`Page.add_squiggly_annot` 等），以及在多个绘图方法中使用（如： :meth:`Page.draw_quad` / :meth:`Shape.draw_quad` ， :meth:`Page.draw_oval` / :meth:`Shape.draw_quad`）。

   .. note::

      * 如果矩形的角点经过 **旋转** 、 **缩放** 或 **平移** 的 :ref:`Matrix` 变换，则结果四边形是 **矩形** （即与矩形全等），即所有角点仍然形成 90 度角。属性 :attr:`Quad.is_rectangular` 用于检查四边形是否可以视为这种变换的结果。

      * 不是所有矩阵都适用：例如，剪切矩阵会产生平行四边形，而不可逆矩阵则会生成“退化”四边形，如三角形或直线。

      * 属性 :attr:`Quad.rect` 获取最小的包含矩形。反之，矩形现在具有属性 :attr:`Rect.quad`，即 :attr:`IRect.quad`，用于获取其相应的四边形版本。

   ============================= =======================================================
   **方法 / 属性**               **简短描述**
   ============================= =======================================================
   :meth:`Quad.transform`        使用矩阵进行变换
   :meth:`Quad.morph`            使用点和矩阵进行变换
   :attr:`Quad.ul`               左上点
   :attr:`Quad.ur`               右上点
   :attr:`Quad.ll`               左下点
   :attr:`Quad.lr`               右下点
   :attr:`Quad.is_convex`        如果四边形是凸集，则为真
   :attr:`Quad.is_empty`         如果四边形为空集，则为真
   :attr:`Quad.is_rectangular`   如果四边形全等于矩形，则为真
   :attr:`Quad.rect`             包含四边形的最小 :ref:`Rect`
   :attr:`Quad.width`            最长宽度值
   :attr:`Quad.height`           最长高度值
   ============================= =======================================================

   **类 API**

   .. class:: Quad

      .. method:: __init__(self)

      .. method:: __init__(self, ul, ur, ll, lr)

      .. method:: __init__(self, quad)

      .. method:: __init__(self, sequence)

         重载构造函数："ul"、"ur"、"ll"、"lr" 代表 :data:`point_like` 对象（四个角点），"sequence" 是包含四个 :data:`point_like` 对象的 Python 序列。

         如果指定了 "quad"，构造函数会创建一个 **新的副本**。

         如果没有参数，则会创建一个由 4 个 *Point(0, 0)* 组成的四边形。

      .. method:: transform(matrix)

         通过矩阵变换修改四边形，变换其每个角点。

         :arg matrix_like matrix: 要应用的矩阵。

      .. method:: morph(fixpoint, matrix)

         *(新版本 1.17.0 引入)* 使用类似矩阵的对象，通过指定固定点对四边形进行变换。

         :arg point_like fixpoint: 固定点。
         :arg matrix_like matrix: 要应用的矩阵。
         :returns: 一个新的四边形（如果是无限四边形则不进行操作）。

      .. attribute:: rect

         包含四边形的最小矩形，由下图中的蓝色区域表示。

         .. image:: images/img-quads.*

         :type: :ref:`Rect`

      .. attribute:: ul

         左上点。

         :type: :ref:`Point`

      .. attribute:: ur

         右上点。

         :type: :ref:`Point`

      .. attribute:: ll

         左下点。

         :type: :ref:`Point`

      .. attribute:: lr

         右下点。

         :type: :ref:`Point`

      .. attribute:: is_convex

         * 新版本 1.16.1 引入

         检查四边形的任意两点之间的连接线上的所有点是否也属于四边形。

            .. image:: images/img-convexity.*
               :scale: 30

         :type: bool

      .. attribute:: is_empty

         如果包围的区域为零，则为真，这意味着至少三个角点在同一条线上。如果为假，四边形可能仍然是退化的，或者根本不像四边形（如三角形、平行四边形、梯形等）。

         :type: bool

      .. attribute:: is_rectangular

         如果所有角度为 90 度，则为真。这意味着四边形是 **凸的且不为空**。

         :type: bool

      .. attribute:: width

         上边和下边的最大长度。

         :type: float

      .. attribute:: height

         左边和右边的最大长度。

         :type: float

.. tab:: 英文

   Represents a four-sided mathematical shape (also called "quadrilateral" or "tetragon") in the plane, defined as a sequence of four :ref:`Point` objects ul, ur, ll, lr (conveniently called upper left, upper right, lower left, lower right).

   Quads can **be obtained** as results of text search methods (:meth:`Page.search_for`), and they **are used** to define text marker annotations (see e.g. :meth:`Page.add_squiggly_annot` and friends), and in several draw methods (like :meth:`Page.draw_quad` / :meth:`Shape.draw_quad`, :meth:`Page.draw_oval`/ :meth:`Shape.draw_quad`).

   .. note::

      * If the corners of a rectangle are transformed with a **rotation**, **scale** or **translation** :ref:`Matrix`, then the resulting quad is **rectangular** (= congruent to a rectangle), i.e. all of its corners again enclose angles of 90 degrees. Property :attr:`Quad.is_rectangular` checks whether a quad can be thought of being the result of such an operation.

      * This is not true for all matrices: e.g. shear matrices produce parallelograms, and non-invertible matrices deliver "degenerate" tetragons like triangles or lines.

      * Attribute :attr:`Quad.rect` obtains the enveloping rectangle. Vice versa, rectangles now have attributes :attr:`Rect.quad`, resp. :attr:`IRect.quad` to obtain their respective tetragon versions.


   ============================= =======================================================
   **Methods / Attributes**      **Short Description**
   ============================= =======================================================
   :meth:`Quad.transform`        transform with a matrix
   :meth:`Quad.morph`            transform with a point and matrix
   :attr:`Quad.ul`               upper left point
   :attr:`Quad.ur`               upper right point
   :attr:`Quad.ll`               lower left point
   :attr:`Quad.lr`               lower right point
   :attr:`Quad.is_convex`        true if quad is a convex set
   :attr:`Quad.is_empty`         true if quad is an empty set
   :attr:`Quad.is_rectangular`   true if quad is congruent to a rectangle
   :attr:`Quad.rect`             smallest containing :ref:`Rect`
   :attr:`Quad.width`            the longest width value
   :attr:`Quad.height`           the longest height value
   ============================= =======================================================

   **Class API**

   .. class:: Quad
      :no-index:

      .. method:: __init__(self)
         :no-index:

      .. method:: __init__(self, ul, ur, ll, lr)
         :no-index:

      .. method:: __init__(self, quad)
         :no-index:

      .. method:: __init__(self, sequence)
         :no-index:

         Overloaded constructors: "ul", "ur", "ll", "lr" stand for :data:`point_like` objects (the four corners), "sequence" is a Python sequence with four :data:`point_like` objects.

         If "quad" is specified, the constructor creates a **new copy** of it.

         Without parameters, a quad consisting of 4 copies of *Point(0, 0)* is created.


      .. method:: transform(matrix)
         :no-index:

         Modify the quadrilateral by transforming each of its corners with a matrix.

         :arg matrix_like matrix: the matrix.

      .. method:: morph(fixpoint, matrix)
         :no-index:

         *(New in version 1.17.0)* "Morph" the quad with a matrix-like using a point-like as fixed point.

         :arg point_like fixpoint: the point.
         :arg matrix_like matrix: the matrix.
         :returns: a new quad (no operation if this is the infinite quad).


      .. attribute:: rect
         :no-index:

         The smallest rectangle containing the quad, represented by the blue area in the following picture.

         .. image:: images/img-quads.*

         :type: :ref:`Rect`

      .. attribute:: ul
         :no-index:

         Upper left point.

         :type: :ref:`Point`

      .. attribute:: ur
         :no-index:

         Upper right point.

         :type: :ref:`Point`

      .. attribute:: ll
         :no-index:

         Lower left point.

         :type: :ref:`Point`

      .. attribute:: lr
         :no-index:

         Lower right point.

         :type: :ref:`Point`

      .. attribute:: is_convex
         :no-index:

         * New in version 1.16.1

         Checks if for any two points of the quad, all points on their connecting line also belong to the quad.

            .. image:: images/img-convexity.*
               :scale: 30

         :type: bool

      .. attribute:: is_empty
         :no-index:

         True if enclosed area is zero, which means that at least three of the four corners are on the same line. If this is false, the quad may still be degenerate or not look like a tetragon at all (triangles, parallelograms, trapezoids, ...).

         :type: bool

      .. attribute:: is_rectangular
         :no-index:

         True if all corner angles are 90 degrees. This implies that the quad is **convex and not empty**.

         :type: bool

      .. attribute:: width
         :no-index:

         The maximum length of the top and the bottom side.

         :type: float

      .. attribute:: height
         :no-index:

         The maximum length of the left and the right side.

         :type: float

评论
------

Remark

.. tab:: 中文

   此类遵循序列协议，因此也可以通过其索引来处理组件。另请参阅 :ref:`SequenceTypes`。

.. tab:: 英文

   This class adheres to the sequence protocol, so components can be dealt with via their indices, too. Also refer to :ref:`SequenceTypes`.

代数和包含检查
-------------------------------

Algebra and Containment Checks

.. tab:: 中文

   从 v1.19.6 开始，四边形（quads）可以像其他几何对象一样用于代数表达式 -- 相关的限制已被取消。特别地，现在所有以下的包含关系检查组合都可以使用：

   `{Point | IRect | Rect | Quad} in {IRect | Rect | Quad}`

   请注意以下有趣的细节：

   对于矩形，只有其左上角的点属于它。自 v1.19.0 起，矩形被定义为“开区间”，即其底边和右边不属于矩形 — 包括相应的角点。但对于四边形，概念中没有类似于“开区间”的定义，因此我们有以下令人惊讶的结论：

      >>> rect.br in rect
      False
      >>> # 但：
      >>> rect.br in rect.quad
      True

.. tab:: 英文

   Starting with v1.19.6, quads can be used in algebraic expressions like the other geometry object -- the respective restrictions have been lifted. In particular, all the following combinations of containment checking are now possible:

   `{Point | IRect | Rect | Quad} in {IRect | Rect | Quad}`

   Please note the following interesting detail:

   For a rectangle, only its top-left point belongs to it. Since v1.19.0, rectangles are defined to be "open", such that its bottom and its right edge do not belong to it -- including the respective corners. But for quads there exists no such notion like "openness", so we have the following somewhat surprising implication:

      >>> rect.br in rect
      False
      >>> # but:
      >>> rect.br in rect.quad
      True

.. include:: footer.rst
