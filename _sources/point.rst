.. include:: header.rst

.. _Point:

================
Point
================

.. tab:: 中文

   *Point* 表示平面中的一个点，由其 x 和 y 坐标定义。

   ============================ ============================================
   **属性 / 方法**              **描述**
   ============================ ============================================
   :meth:`Point.distance_to`    计算到点或矩形的距离
   :meth:`Point.norm`           欧几里得范数
   :meth:`Point.transform`      使用矩阵转换点
   :attr:`Point.abs_unit`       与单位相同，但坐标为正
   :attr:`Point.unit`           点的坐标除以 *abs(point)*
   :attr:`Point.x`              x 坐标
   :attr:`Point.y`              y 坐标
   ============================ ============================================

   **类 API**

   .. class:: Point

      .. method:: __init__(self)

      .. method:: __init__(self, x, y)

      .. method:: __init__(self, point)

      .. method:: __init__(self, sequence)

         重载构造函数。

         如果没有参数，则创建 *Point(0, 0)*。

         如果指定了另一个点，则会创建 **新副本**，"sequence" 是包含 2 个数字的 Python 序列（参见 :ref:`SequenceTypes`）。

         :arg float x: 点的 x 坐标

         :arg float y: 点的 y 坐标

      .. method:: distance_to(x [, unit])

         计算到 *x* 的距离，*x* 可以是 :data:`point_like` 或 :data:`rect_like`。距离以像素（默认）、英寸、厘米或毫米为单位。

      :arg point_like,rect_like x: 计算距离的目标。

      :arg str unit: 测量的单位。可以是 "px"、"in"、"cm"、"mm" 之一。

      :rtype: float
      :returns: 到 *x* 的距离。如果 *x* 是 :data:`rect_like`，则距离

            * 是连接矩形任意一边的最短线段的长度
            * 计算的是 **有限版本** 的距离
            * 如果矩形 **包含** 该点，则距离为零

      .. method:: norm()

         * 新版本 1.16.0 引入

         返回点的欧几里得范数（长度）作为一个向量。等同于函数 *abs()* 的结果。

      .. method:: transform(m)

         将矩阵应用于点，并用结果替代原点。

      :arg matrix_like m: 要应用的矩阵。

      :rtype: :ref:`Point`

      .. attribute:: unit

         将每个坐标除以 *norm(point)* （点到 (0,0) 的距离）后的结果。该向量的长度为 1，方向与点相同。其 x 和 y 值分别等于该向量与 x 轴夹角的余弦和正弦值。

         .. image:: images/img-point-unit.*

         :type: :ref:`Point`

      .. attribute:: abs_unit

         与 :attr:`unit` 相同，但坐标的值替换为其绝对值。

         :type: :ref:`Point`

      .. attribute:: x

         x 坐标

         :type: float

      .. attribute:: y

         y 坐标

         :type: float

   .. note::

      * 该类遵循 Python 序列协议，因此可以通过索引访问各个组件。另见 :ref:`SequenceTypes`。
      * 矩形可以与算术运算符一起使用 -- 参见 :ref:`Algebra` 章节。

.. tab:: 英文

   *Point* represents a point in the plane, defined by its x and y coordinates.

   ============================ ============================================
   **Attribute / Method**       **Description**
   ============================ ============================================
   :meth:`Point.distance_to`    calculate distance to point or rect
   :meth:`Point.norm`           the Euclidean norm
   :meth:`Point.transform`      transform point with a matrix
   :attr:`Point.abs_unit`       same as unit, but positive coordinates
   :attr:`Point.unit`           point coordinates divided by *abs(point)*
   :attr:`Point.x`              the X-coordinate
   :attr:`Point.y`              the Y-coordinate
   ============================ ============================================

   **Class API**

   .. class:: Point
      :no-index:

      .. method:: __init__(self)
         :no-index:

      .. method:: __init__(self, x, y)
         :no-index:

      .. method:: __init__(self, point)
         :no-index:

      .. method:: __init__(self, sequence)
         :no-index:

         Overloaded constructors.

         Without parameters, *Point(0, 0)* will be created.

         With another point specified, a **new copy** will be created, "sequence" is a Python sequence of 2 numbers (see :ref:`SequenceTypes`).

         :arg float x: x coordinate of the point

         :arg float y: y coordinate of the point

      .. method:: distance_to(x [, unit])
         :no-index:

         Calculate the distance to *x*, which may be :data:`point_like` or :data:`rect_like`. The distance is given in units of either pixels (default), inches, centimeters or millimeters.

         :arg point_like,rect_like x: to which to compute the distance.

         :arg str unit: the unit to be measured in. One of "px", "in", "cm", "mm".

         :rtype: float
         :returns: the distance to *x*. If this is :data:`rect_like`, then the distance

             * is the length of the shortest line connecting to one of the rectangle sides
             * is calculated to the **finite version** of it
             * is zero if it **contains** the point

      .. method:: norm()
         :no-index:

         * New in version 1.16.0
         
         Return the Euclidean norm (the length) of the point as a vector. Equals result of function *abs()*.

      .. method:: transform(m)
         :no-index:

         Apply a matrix to the point and replace it with the result.

         :arg matrix_like m: The matrix to be applied.

         :rtype: :ref:`Point`

      .. attribute:: unit
         :no-index:

         Result of dividing each coordinate by *norm(point)*, the distance of the point to (0,0). This is a vector of length 1 pointing in the same direction as the point does. Its x, resp. y values are equal to the cosine, resp. sine of the angle this vector (and the point itself) has with the x axis.

         .. image:: images/img-point-unit.*

         :type: :ref:`Point`

      .. attribute:: abs_unit
         :no-index:

         Same as :attr:`unit` above, replacing the coordinates with their absolute values.

         :type: :ref:`Point`

      .. attribute:: x
         :no-index:

         The x coordinate

         :type: float

      .. attribute:: y
         :no-index:

         The y coordinate

         :type: float

   .. note::

      * This class adheres to the Python sequence protocol, so components can be accessed via their index, too. Also refer to :ref:`SequenceTypes`.
      * Rectangles can be used with arithmetic operators -- see chapter :ref:`Algebra`.

.. include:: footer.rst
