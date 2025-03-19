.. include:: header.rst

.. _IRect:

==========
IRect
==========

.. tab:: 中文

   IRect 是一个矩形边界框，非常类似于 :ref:`Rect`，不同之处在于所有角坐标都是整数。IRect 用于指定像素区域，例如在渲染时接收图像数据。除此之外，关于矩形的空性和有效性等考虑也适用于此类。方法和属性名称相同，并且在许多情况下通过重复使用相应的 :ref:`Rect` 对应项来实现。

   ============================== ==============================================
   **属性 / 方法**                **简短描述**
   ============================== ==============================================
   :meth:`IRect.contains`         检查是否包含另一个对象
   :meth:`IRect.get_area`         计算矩形面积
   :meth:`IRect.intersect`        与另一个矩形的公共部分
   :meth:`IRect.intersects`       检查是否存在非空交集
   :meth:`IRect.morph`            使用点和矩阵进行变换
   :meth:`IRect.torect`           转换为另一个矩形的矩阵
   :meth:`IRect.norm`             欧几里得范数
   :meth:`IRect.normalize`        使矩形有限
   :attr:`IRect.bottom_left`      左下角点，别名 *bl*
   :attr:`IRect.bottom_right`     右下角点，别名 *br*
   :attr:`IRect.height`           矩形的高度
   :attr:`IRect.is_empty`         矩形是否为空
   :attr:`IRect.is_infinite`      矩形是否是无限的
   :attr:`IRect.rect`             相应的 :ref:`Rect`
   :attr:`IRect.top_left`         左上角点，别名 *tl*
   :attr:`IRect.top_right`        右上角点，别名 *tr*
   :attr:`IRect.quad`             从矩形角点构造的 :ref:`Quad`
   :attr:`IRect.width`            矩形的宽度
   :attr:`IRect.x0`               左上角的 X 坐标
   :attr:`IRect.x1`               右下角的 X 坐标
   :attr:`IRect.y0`               左上角的 Y 坐标
   :attr:`IRect.y1`               右下角的 Y 坐标
   ============================== ==============================================

   **Class API**

   .. class:: IRect

      .. method:: __init__(self)

      .. method:: __init__(self, x0, y0, x1, y1)

      .. method:: __init__(self, irect)

      .. method:: __init__(self, sequence)

         重载构造函数。请参阅下面的示例以及 :ref:`Rect` 类的示例。

         如果指定了另一个 irect，将创建一个 **新副本**。

         如果指定了 sequence，它必须是一个包含 4 个数字的 Python 序列类型（参见 :ref:`SequenceTypes`）。非整数数字将被截断，非数字值将引发异常。

         其他参数表示整数坐标。

      .. method:: get_area([unit])

         计算矩形的面积，如果没有参数，则等于 *abs(IRect)*。像空矩形一样，无限矩形的面积也为零。

         :arg str unit: 指定所需的单位：分别为 "px"（像素，默认）、"in"（英寸）、"cm"（厘米）或 "mm"（毫米）。

         :rtype: float

      .. method:: intersect(ir)

         计算当前矩形与 *ir* 的交集（公共矩形区域），并将其替换当前矩形。如果任一矩形为空，则结果也为空。如果任一矩形是无限的，则取另一个矩形作为结果——如果两个矩形都是无限的，则结果也是无限的。

         :arg rect_like ir: 第二个矩形。

      .. method:: contains(x)

         检查 *x* 是否包含在矩形内。*x* 可以是 :data:`rect_like`、:data:`point_like` 或数字。如果 *x* 是一个空矩形，始终返回真。相反，如果矩形为空，则始终返回 *False*，如果 *x* 不是空矩形并且不是数字。如果 *x* 是数字，将检查其是否为四个组件之一。*x in irect* 和 *irect.contains(x)* 是等效的。

         :arg x: 要检查的对象。
         :type x: :ref:`IRect` 或 :ref:`Rect` 或 :ref:`Point` 或 int

         :rtype: bool

      .. method:: intersects(r)

         检查矩形与 :data:`rect_like` "r" 是否包含一个公共的非空 :ref:`IRect`。如果任一矩形是无限的或为空，则始终返回 *False*。

         :arg rect_like r: 要检查的矩形。

         :rtype: bool

      .. method:: torect(rect)

         * 新增于 v1.19.3
         
         计算将此矩形转换为给定矩形的矩阵。参见 :meth:`Rect.torect`。

         :arg rect_like rect: 目标矩形。不得为空或无限。
         :rtype: :ref:`Matrix`
         :returns: 一个矩阵 `mat`，使得 `self * mat = rect`。例如，可以用于在页面坐标和像素图坐标之间进行转换。

      .. method:: morph(fixpoint, matrix)

         * 新增于 v1.17.0
         
         在使用固定点应用矩阵后返回一个新的四边形（quad）。

         :arg point_like fixpoint: 固定点。
         :arg matrix_like matrix: 矩阵。
         :returns: 一个新的 :ref:`Quad`。这是同名四边形方法的封装。如果是无限的，将返回无限四边形。

      .. method:: norm()

         * 新增于 v1.16.0
         
         返回矩形的欧几里得范数，将矩形视为由四个数字组成的向量。

      .. method:: normalize()

         使矩形变为有限。通过调整矩形的角点来完成此操作。完成后，右下角将确实位于左上角的南东方向。更多细节请参见 :ref:`Rect`。

      .. attribute:: top_left

      .. attribute:: tl

         等同于 *Point(x0, y0)*。

         :type: :ref:`Point`

      .. attribute:: top_right

      .. attribute:: tr

         等同于 *Point(x1, y0)*。

         :type: :ref:`Point`

      .. attribute:: bottom_left

      .. attribute:: bl

         等同于 *Point(x0, y1)*。

         :type: :ref:`Point`

      .. attribute:: bottom_right

      .. attribute:: br

         等同于 *Point(x1, y1)*。

         :type: :ref:`Point`

      .. attribute:: rect

         与矩形具有相同坐标的 :ref:`Rect`，坐标为浮动点。

         :type: :ref:`Rect`

      .. attribute:: quad

         四边形 *Quad(irect.tl, irect.tr, irect.bl, irect.br)*。

         :type: :ref:`Quad`

      .. attribute:: width

         包含边界框的宽度。等于 *abs(x1 - x0)*。

         :type: int

      .. attribute:: height

         包含边界框的高度。等于 *abs(y1 - y0)*。

         :type: int

      .. attribute:: x0

         左上角的 X 坐标。

         :type: int

      .. attribute:: y0

         左上角的 Y 坐标。

         :type: int

      .. attribute:: x1

         右下角的 X 坐标。

         :type: int

      .. attribute:: y1

         右下角的 Y 坐标。

         :type: int

      .. attribute:: is_infinite

         如果矩形是无限的，则为 *True*，否则为 *False*。

         :type: bool

      .. attribute:: is_empty

         如果矩形是空的，则为 *True*，否则为 *False*。

         :type: bool


   .. note::

      * 该类遵循 Python 序列协议，因此组件也可以通过索引访问。另请参见 :ref:`SequenceTypes`。
      * 矩形可以与算术运算符一起使用 -- 参见 :ref:`Algebra` 章节。


.. tab:: 英文


   IRect is a rectangular bounding box, very similar to :ref:`Rect`, except that all corner coordinates are integers. IRect is used to specify an area of pixels, e.g. to receive image data during rendering. Otherwise, e.g. considerations concerning emptiness and validity of rectangles also apply to this class. Methods and attributes have the same names, and in many cases are implemented by re-using the respective :ref:`Rect` counterparts.

   ============================== ==============================================
   **Attribute / Method**          **Short Description**
   ============================== ==============================================
   :meth:`IRect.contains`         checks containment of another object
   :meth:`IRect.get_area`         calculate rectangle area
   :meth:`IRect.intersect`        common part with another rectangle
   :meth:`IRect.intersects`       checks for non-empty intersection
   :meth:`IRect.morph`            transform with a point and a matrix
   :meth:`IRect.torect`           matrix that transforms to another rectangle
   :meth:`IRect.norm`             the Euclidean norm
   :meth:`IRect.normalize`        makes a rectangle finite
   :attr:`IRect.bottom_left`      bottom left point, synonym *bl*
   :attr:`IRect.bottom_right`     bottom right point, synonym *br*
   :attr:`IRect.height`           height of the rectangle
   :attr:`IRect.is_empty`         whether rectangle is empty
   :attr:`IRect.is_infinite`      whether rectangle is infinite
   :attr:`IRect.rect`             the :ref:`Rect` equivalent
   :attr:`IRect.top_left`         top left point, synonym *tl*
   :attr:`IRect.top_right`        top_right point, synonym *tr*
   :attr:`IRect.quad`             :ref:`Quad` made from rectangle corners
   :attr:`IRect.width`            width of the rectangle
   :attr:`IRect.x0`               X-coordinate of the top left corner
   :attr:`IRect.x1`               X-coordinate of the bottom right corner
   :attr:`IRect.y0`               Y-coordinate of the top left corner
   :attr:`IRect.y1`               Y-coordinate of the bottom right corner
   ============================== ==============================================

   **Class API**

   .. class:: IRect
      :no-index:

      .. method:: __init__(self)
         :no-index:

      .. method:: __init__(self, x0, y0, x1, y1)
         :no-index:

      .. method:: __init__(self, irect)
         :no-index:

      .. method:: __init__(self, sequence)
         :no-index:

         Overloaded constructors. Also see examples below and those for the :ref:`Rect` class.

         If another irect is specified, a **new copy** will be made.

         If sequence is specified, it must be a Python sequence type of 4 numbers (see :ref:`SequenceTypes`). Non-integer numbers will be truncated, non-numeric values will raise an exception.

         The other parameters mean integer coordinates.


      .. method:: get_area([unit])
         :no-index:

         Calculates the area of the rectangle and, with no parameter, equals *abs(IRect)*. Like an empty rectangle, the area of an infinite rectangle is also zero.

         :arg str unit: Specify required unit: respective squares of "px" (pixels, default), "in" (inches), "cm" (centimeters), or "mm" (millimeters).

         :rtype: float

      .. method:: intersect(ir)
         :no-index:

         The intersection (common rectangular area) of the current rectangle and *ir* is calculated and replaces the current rectangle. If either rectangle is empty, the result is also empty. If either rectangle is infinite, the other one is taken as the result -- and hence also infinite if both rectangles were infinite.

         :arg rect_like ir: Second rectangle.

      .. method:: contains(x)
         :no-index:

         Checks whether *x* is contained in the rectangle. It may be :data:`rect_like`, :data:`point_like` or a number. If *x* is an empty rectangle, this is always true. Conversely, if the rectangle is empty this is always *False*, if *x* is not an empty rectangle and not a number. If *x* is a number, it will be checked to be one of the four components. *x in irect* and *irect.contains(x)* are equivalent.

         :arg x: the object to check.
         :type x: :ref:`IRect` or :ref:`Rect` or :ref:`Point` or int

         :rtype: bool

      .. method:: intersects(r)
         :no-index:

         Checks whether the rectangle and the :data:`rect_like` "r" contain a common non-empty :ref:`IRect`. This will always be *False* if either is infinite or empty.

         :arg rect_like r: the rectangle to check.

         :rtype: bool

      .. method:: torect(rect)
         :no-index:

         * New in version 1.19.3
         
         Compute the matrix which transforms this rectangle to a given one. See :meth:`Rect.torect`.

         :arg rect_like rect: the target rectangle. Must not be empty or infinite.
         :rtype: :ref:`Matrix`
         :returns: a matrix `mat` such that `self * mat = rect`. Can for example be used to transform between the page and the pixmap coordinates.


      .. method:: morph(fixpoint, matrix)
         :no-index:

         * New in version 1.17.0
         
         Return a new quad after applying a matrix to it using a fixed point.

         :arg point_like fixpoint: the fixed point.
         :arg matrix_like matrix: the matrix.
         :returns: a new :ref:`Quad`. This a wrapper of the same-named quad method. If infinite, the infinite quad is returned.

      .. method:: norm()
         :no-index:

         * New in version 1.16.0
         
         Return the Euclidean norm of the rectangle treated as a vector of four numbers.

      .. method:: normalize()
         :no-index:

         Make the rectangle finite. This is done by shuffling rectangle corners. After this, the bottom right corner will indeed be south-eastern to the top left one. See :ref:`Rect` for a more details.

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

         Equals *Point(x1, y0)*.

         :type: :ref:`Point`

      .. attribute:: bottom_left
         :no-index:

      .. attribute:: bl
         :no-index:

         Equals *Point(x0, y1)*.

         :type: :ref:`Point`

      .. attribute:: bottom_right
         :no-index:

      .. attribute:: br
         :no-index:

         Equals *Point(x1, y1)*.

         :type: :ref:`Point`

      .. attribute:: rect
         :no-index:

         The :ref:`Rect` with the same coordinates as floats.

         :type: :ref:`Rect`

      .. attribute:: quad
         :no-index:

         The quadrilateral *Quad(irect.tl, irect.tr, irect.bl, irect.br)*.

         :type: :ref:`Quad`

      .. attribute:: width
         :no-index:

         Contains the width of the bounding box. Equals *abs(x1 - x0)*.

         :type: int

      .. attribute:: height
         :no-index:

         Contains the height of the bounding box. Equals *abs(y1 - y0)*.

         :type: int

      .. attribute:: x0
         :no-index:

         X-coordinate of the left corners.

         :type: int

      .. attribute:: y0
         :no-index:

         Y-coordinate of the top corners.

         :type: int

      .. attribute:: x1
         :no-index:

         X-coordinate of the right corners.

         :type: int

      .. attribute:: y1
         :no-index:

         Y-coordinate of the bottom corners.

         :type: int

      .. attribute:: is_infinite
         :no-index:

         *True* if rectangle is infinite, *False* otherwise.

         :type: bool

      .. attribute:: is_empty
         :no-index:

         *True* if rectangle is empty, *False* otherwise.

         :type: bool


   .. note::

      * This class adheres to the Python sequence protocol, so components can be accessed via their index, too. Also refer to :ref:`SequenceTypes`.
      * Rectangles can be used with arithmetic operators -- see chapter :ref:`Algebra`.

.. include:: footer.rst

