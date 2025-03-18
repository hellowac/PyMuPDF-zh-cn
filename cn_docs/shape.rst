.. include:: header.rst

.. _Shape:

Shape
================

|pdf_only_class|

.. tab:: 中文

   该类允许在 PDF 页面上创建相互连接的图形元素。其方法的名称和含义与相应的 :ref:`Page` 方法相同。  

   实际上，每个 :ref:`Page` 的绘图方法只是以下操作的便利封装：
   1. 调用一个 `Shape` 类的绘图方法，  
   2. 调用 :meth:`Shape.finish` 方法，  
   3. 调用 :meth:`Shape.commit` 方法。  

   对于页面文本插入，仅调用 :meth:`Shape.commit` 方法即可。如果在页面上执行多个绘图和文本操作，应优先考虑使用 `Shape` 对象。  

   可以连续执行多个绘图方法，每个方法都会为同一个绘图贡献元素。一旦绘图完成，必须调用 :meth:`Shape.finish` 方法，以应用颜色、虚线、线宽、变形等属性。  

   该类的 **绘图** 方法（包括 :meth:`Shape.insert_textbox`）会记录它们覆盖的区域，并存储在一个矩形框中（:attr:`Shape.rect`）。例如，该属性可以用于设置 :attr:`Page.cropbox_position`。  

   **文本插入** 方法 :meth:`Shape.insert_text` 和 :meth:`Shape.insert_textbox` 会隐式执行 "finish" 操作，因此只需调用 :meth:`Shape.commit` 使其生效。因此，这些方法包含控制颜色等属性的参数。  

   ================================ =====================================================
   **方法 / 属性**                    **描述**
   ================================ =====================================================
   :meth:`Shape.commit`             更新页面内容
   :meth:`Shape.draw_bezier`        绘制三次贝塞尔曲线
   :meth:`Shape.draw_circle`        绘制一个以点为中心的圆
   :meth:`Shape.draw_curve`         使用一个辅助点绘制三次贝塞尔曲线
   :meth:`Shape.draw_line`          绘制一条直线
   :meth:`Shape.draw_oval`          绘制椭圆
   :meth:`Shape.draw_polyline`      连接一系列点，绘制折线
   :meth:`Shape.draw_quad`          绘制四边形
   :meth:`Shape.draw_rect`          绘制矩形
   :meth:`Shape.draw_sector`        绘制圆扇形或饼状区域
   :meth:`Shape.draw_squiggle`      绘制波浪线
   :meth:`Shape.draw_zigzag`        绘制锯齿线
   :meth:`Shape.finish`             结束一组绘图命令
   :meth:`Shape.insert_text`        插入文本行
   :meth:`Shape.insert_textbox`     将文本适配到矩形区域内
   :attr:`Shape.doc`                存储当前页面所属的文档
   :attr:`Shape.draw_cont`          记录自上次 :meth:`Shape.finish` 以来的绘图命令
   :attr:`Shape.height`             存储页面高度
   :attr:`Shape.lastPoint`          记录当前绘图点
   :attr:`Shape.page`               存储当前 `Shape` 所属的页面
   :attr:`Shape.rect`               记录围绕所有绘图元素的矩形区域
   :attr:`Shape.text_cont`          记录所有已插入的文本内容
   :attr:`Shape.totalcont`          记录所有待存储到 :data:`contents` 的字符串
   :attr:`Shape.width`              存储页面宽度
   ================================ =====================================================

   **Class API**

   .. class:: Shape

      .. method:: __init__(self, page)

         创建一个新的绘图。在导入 PyMuPDF 时，*pymupdf.Page* 对象会被赋予一个便捷方法 *new_shape()*，用于构造 *Shape* 对象。在实例化过程中，会检查是否为 PDF 页面，否则会引发异常。

         :arg page: 一个已存在的 PDF 文档页面。
         :type page: :ref:`Page`

      .. method:: draw_line(p1, p2)

         从 :data:`point_like` 对象 *p1* 到 *p2* 画一条直线。

         :arg point_like p1: 起点

         :arg point_like p2: 终点

         :rtype: :ref:`Point`
         :returns: 终点 *p2*。

      .. index::
         pair: 波宽; draw_squiggle

      .. method:: draw_squiggle(p1, p2, breadth=2)

         从 :data:`point_like` 对象 *p1* 到 *p2* 画一条波浪线（起伏的、波状的）。始终会绘制整数个完整波浪周期，每个周期的长度为 *4 * breadth*。*breadth* 参数将根据需要进行调整，以满足此条件。绘制的波浪线在离开 *p1* 时始终向“左”转，并在连接 *p2* 时始终从“右”侧进入。

         :arg point_like p1: 起点

         :arg point_like p2: 终点

         :arg float breadth: 每个波的振幅。条件 *2 * breadth < abs(p2 - p1)* 必须成立，以确保至少能容纳一个完整的波形。请参阅下图，该图显示了两个点通过一个完整周期连接的情况。

         :rtype: :ref:`Point`
         :returns: 终点 *p2*。

         .. image:: images/img-breadth.*

         下面是一个示例，绘制了三条相连的波浪线，形成一个封闭的填充三角形。小箭头指示了绘制方向。

            >>> import pymupdf
            >>> doc = pymupdf.open()
            >>> page = doc.new_page()
            >>> r = pymupdf.Rect(100, 100, 300, 200)
            >>> shape = page.new_shape()
            >>> shape.draw_squiggle(r.tl, r.tr)
            >>> shape.draw_squiggle(r.tr, r.br)
            >>> shape.draw_squiggle(r.br, r.tl)
            >>> shape.finish(color=(0, 0, 1), fill=(1, 1, 0))
            >>> shape.commit()
            >>> doc.save("x.pdf")

         .. image:: images/img-squiggly.*

         .. note:: 绘制的波浪线 **不是** 三角函数（正弦/余弦）曲线。如果需要这样的曲线，请查看 `draw.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/draw-sines/draw.py>`_。

      .. index::
         pair: breadth; draw_zigzag

      .. method:: draw_zigzag(p1, p2, breadth=2)

         从 :data:`point_like` 对象 *p1* 到 *p2* 画一条锯齿线。除此之外，该方法的工作方式与 :meth:`Shape.draw_squiggle` 完全相同。

         :arg point_like p1: 起点

         :arg point_like p2: 终点

         :arg float breadth: 运动的振幅。条件 *2 * breadth < abs(p2 - p1)* 必须成立，以确保至少能容纳一个完整周期。

         :rtype: :ref:`Point`
         :returns: 终点 *p2*。

      .. method:: draw_polyline(points)

         在序列 *points* 中的点之间绘制多条相连的直线。通过使最后一个点与第一个点相等，可以用于创建任意多边形。

         :arg sequence points: 一个包含 :data:`point_like` 对象的序列。其长度必须至少为 2（如果仅有 2 个点，则等同于 *draw_line()*）。

         :rtype: :ref:`Point`
         :returns: *points[-1]* —— 传入序列中的最后一个点。

      .. method:: draw_bezier(p1, p2, p3, p4)

         从 *p1* 到 *p4* 绘制一条标准的三次贝塞尔曲线，使用 *p2* 和 *p3* 作为控制点。

         所有参数均为 :data:`point_like` 对象。

         :rtype: :ref:`Point`
         :returns: 终点 *p4*。

         .. note:: 这些点不一定要不同 —— 试着让其中一些点相等，看看会发生什么！

         示例：

         .. image:: images/img-drawBezier.*

      .. method:: draw_oval(tetra)

         在指定的四边形（四边形区域）内绘制一个“椭圆”。如果该区域是正方形，则绘制一个标准圆形；如果是矩形，则绘制椭圆。而对于一般的四边形，则可能得到各种不同的形状。

         该绘制过程从 `bottom-left -> top-left` 线段的中点开始，并沿逆时针方向完成。

         :arg rect_like,quad_like tetra: :data:`rect_like` 或 :data:`quad_like`。

            *自版本 1.14.5 变更:*  现在支持四边形（quad）。

         :rtype: :ref:`Point`
         :returns: 线段 `rect.bl -> rect.tl` 或 `quad.ll -> quad.ul` 的中点。请查看以下示例，或查阅 PyMuPDF-Utilities 仓库中的 *quad-show?.py* 脚本。

         .. image:: images/img-drawquad.*
            :scale: 50

      .. method:: draw_circle(center, radius)

         根据给定的圆心和半径绘制一个圆。绘制从 `center - (radius, 0)` 这一点开始，并沿 **逆时针** 方向完成。该点位于包围正方形左侧边的中点。

         这是 `draw_sector(center, start, 360, fullSector=False)` 的简写形式。要以 **顺时针** 方向绘制相同的圆，请使用 `-360` 作为角度参数。

         :arg point_like center: 圆心。

         :arg float radius: 圆的半径，必须为正数。

         :rtype: :ref:`Point`
         :returns: `Point(center.x - radius, center.y)`。

            .. image:: images/img-drawcircle.*
               :scale: 60

      .. method:: draw_curve(p1, p2, p3)

         该方法是 *draw_bezier()* 的特例：从 *p1* 到 *p3* 绘制一条三次贝塞尔曲线。在 `p1 -> p2` 和 `p3 -> p2` 这两条线上分别生成一个控制点，因此两个控制点始终位于 `p1 -> p3` 线的同一侧。这保证了曲线的曲率不会改变符号。如果两条线与 *p2* 交叉的角度为 90 度，则所得曲线是一个四分之一椭圆（如果两段长度相等，则是四分之一圆）。

         所有参数均为 :data:`point_like`。

         :rtype: :ref:`Point`
         :returns: 终点 *p3*。下图展示了一个填充的四分之一椭圆区域，黄色部分的方向为 **顺时针**：

            .. image:: images/img-drawCurve.png
               :align: center

      .. index::
         pair: fullSector; draw_sector

      .. method:: draw_sector(center, point, angle, fullSector=True)

         绘制一个扇形（圆弧部分可以选择是否连接到圆心，使其成为“饼状”）。

         :arg point_like center: 圆心。

         :arg point_like point: 扇形弧线段的一个端点，另一个端点由 *angle* 计算得出。

         :arg float angle: 扇形的角度（以度为单位）。用于计算弧线的另一端点。根据符号，弧线将以逆时针（正值）或顺时针（负值）方向绘制。

         :arg bool fullSector: 是否将弧线的两端连接到圆心。如果指定了填充颜色，则整个“饼状”区域将被填充颜色，否则仅填充弧形区域。

         :rtype: :ref:`Point`
         :returns: 弧线的另一端点。此返回值可用作后续调用的起点，以创建逻辑上相连的饼状图。例如：

            .. image:: images/img-drawSector1.*

            .. image:: images/img-drawSector2.*

      .. method:: draw_rect(rect, *, radius=None)

         * v1.22.0 版本变更: 添加了 *radius* 参数。

         绘制一个矩形。绘制从左上角开始，并沿 **逆时针** 方向完成。

         :arg rect_like rect: 矩形在页面上的位置。
         :arg multiple radius: 绘制圆角矩形。如果不为 `None`，则指定曲率半径，作为矩形边长的百分比。该值必须是一个浮点数，或者包含两个浮点数的元组，满足 `0 < radius <= 0.5`，其中 `0.5` 表示相应边长的 50%。如果为单个浮点数，则曲率半径计算为 `radius * min(width, height)`，角的轮廓将绘制为四分之一圆。如果提供 `(rx, ry)` 形式的元组，则水平和垂直方向的曲率将不同。值 `radius=(0.5, 0.5)` 将绘制一个椭圆。

         :rtype: :ref:`Point`
         :returns: 矩形的左上角。

      .. method:: draw_quad(quad)

         绘制一个四边形。绘制从左上角 (:attr:`Quad.ul`) 开始，并沿 **逆时针** 方向完成。该方法是 :meth:`Shape.draw_polyline` 的简写，参数为 `(ul, ll, lr, ur, ul)`。

         :arg quad_like quad: 四边形在页面上的位置。

         :rtype: :ref:`Point`
         :returns: :attr:`Quad.ul`。

      .. index::
         pair: closePath; finish
         pair: color; finish
         pair: dashes; finish
         pair: even_odd; finish
         pair: fill; finish
         pair: lineCap; finish
         pair: lineJoin; finish
         pair: morph; finish
         pair: width; finish
         pair: stroke_opacity; finish
         pair: fill_opacity; finish
         pair: oc; finish

      .. method:: finish(width=1, color=(0,), fill=None, lineCap=0, lineJoin=0, dashes=None, closePath=True, even_odd=False, morph=(fixpoint, matrix), stroke_opacity=1, fill_opacity=1, oc=0)

         结束一组 *draw*() 方法的绘制，并对其应用 :ref:`CommonParms`。

         **该方法不会影响** :meth:`Shape.insert_text` 和 :meth:`Shape.insert_textbox`。

         该方法还支持 **对组合绘图进行形变 (morphing)**，使用 :ref:`Point` *fixpoint* 作为固定点，并应用 :ref:`matrix` *matrix* 进行变换。

         :arg sequence morph: 通过对某个任意 :ref:`Point` *fixpoint* 应用 :ref:`Matrix` *matrix* 来变换文本或组合图形。此操作会保持 *fixpoint* 位置不变。默认情况下不进行变换 (*None*)。矩阵的前四个分量可以包含任意值，但必须满足 *matrix.e == matrix.f == 0*。这意味着可以进行缩放、剪切、旋转、翻转等操作，但不能进行平移。

         :arg float stroke_opacity: *(v1.18.1 新增)* 设置描边颜色的透明度。值范围 < 0 或 > 1 的数值将被忽略，默认值为 1（不透明）。
         :arg float fill_opacity: *(v1.18.1 新增)* 设置填充颜色的透明度，默认值为 1（不透明）。

         :arg bool even_odd: 请求填充时使用 **"奇偶规则"**。默认值为 *False*，即使用 **"非零环绕数规则"**。这些规则用于确定填充颜色在重叠区域的应用方式。只有在复杂形状中，这些规则才会产生不同的填充效果。详细说明请参考 :ref:`AdobeManual` 第 137 页及后续部分。以下示例演示了两种规则的不同效果：

         :arg int oc: *(v1.18.4 新增)* 指定此绘图的 :data:`xref` 号，使其可按条件显示，适用于 :data:`OCG` 或 :data:`OCMD`。

         .. image:: images/img-even-odd.*

         .. note:: 对于形状中的每个像素，系统会执行以下步骤：

            1. **"奇偶规则"** 计算包含该像素的区域数量。如果该数量为 **奇数**，则像素被视为 **形状内部**，如果为 **偶数**，则像素被视为 **形状外部**。

            2. **默认规则 "非零环绕数"** 还会检查每个区域的 *"方向"*：如果区域是 **逆时针** 绘制的，则计数 **加 1**；如果是 **顺时针** 绘制的，则计数 **减 1**。如果最终结果为零，则像素被视为 **外部**，否则为 **形状内部**。

            上图中的四个形状中，上面两行的三个圆形均采用标准方式（逆时针）绘制（可参考箭头方向）。下面两行的形状中，左上角的圆形是 **顺时针** 绘制的。可以看到，在右列（奇偶规则）中，区域的方向不影响填充结果。

      .. index::
         pair: border_width; insert_text
         pair: color; insert_text
         pair: encoding; insert_text
         pair: fill; insert_text
         pair: fontfile; insert_text
         pair: fontname; insert_text
         pair: fontsize; insert_text
         pair: lineheight; insert_text
         pair: morph; insert_text
         pair: render_mode; insert_text
         pair: miter_limit; insert_text
         pair: rotate; insert_text
         pair: stroke_opacity; insert_text
         pair: fill_opacity; insert_text
         pair: oc; insert_text

      .. method:: insert_text(point, text, *, fontsize=11, fontname="helv", fontfile=None, set_simple=False, encoding=TEXT_ENCODING_LATIN, color=None, lineheight=None, fill=None, render_mode=0, miter_limit=1, border_width=1, rotate=0, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         在 ``point`` 位置插入文本行。

         :arg point_like point: 文本首字符的 **左下角** 位置（像素）。需要注意该参数如何与 *rotate* 参数协同工作。请参考下图，红色小点表示在四种不同旋转角度下 *point* 的位置：

            .. image:: images/img-inserttext.*
               :scale: 33

         :arg str/sequence text: 要插入的文本，可以是字符串或序列类型。如果字符串包含换行符 ``\n``，或提供了序列类型，则会插入多行文本。不会自动处理超出页面宽度的文本，但插入的行数会受页面的 **"垂直"** 空间限制（根据 *rotate* 参数确定的阅读方向）。多余的文本将被丢弃，但返回值会包含实际插入的行数。

         :arg float lineheight: 设定行高的调整系数，覆盖字体默认的行高计算方式。如果不为 `None`，则行高将设定为 `fontsize * lineheight`。
         :arg float stroke_opacity: *(v1.18.1 新增)* 设定描边颜色（字符 **边框**）的透明度，仅支持 `0 <= value <= 1`。默认值为 1（完全不透明）。
         :arg float fill_opacity: *(v1.18.1 新增)* 设定填充颜色（文本颜色）的透明度。默认值为 1（完全不透明）。此参数控制文本颜色的透明度，而 *stroke_opacity* 仅影响字符边框。

         :arg int rotate: 设定文本的旋转角度。仅支持 90° 的整数倍：
         
            - `0` （默认）：文本从 **左到右** 水平显示。
            - `90`：逆时针旋转，文本 **向上** 显示。
            - `180`：文本上下颠倒，从 **右到左** 显示。
            - `270`（或 `-90`）：顺时针旋转，文本 **向下** 显示。

            在所有情况下，*point* 都表示首字符矩形的 **左下角** 坐标。如果存在多行文本，则它们按照 *rotate* 参数确定的阅读方向排列。例如，在 `rotate = 180` 时，第 2 行将出现在 **第 1 行的上方**。

         :arg int oc: *(v1.18.4 新增)* 指定文本的 :data:`xref` 号，以使其可按条件显示，适用于 :data:`OCG` 或 :data:`OCMD`。

         :rtype: int
         :returns: 实际插入的行数。

         其他参数的详细说明请参见 :ref:`CommonParms`。

      .. index::
         pair: align; insert_textbox
         pair: border_width; insert_textbox
         pair: color; insert_textbox
         pair: encoding; insert_textbox
         pair: expandtabs; insert_textbox
         pair: fill; insert_textbox
         pair: fontfile; insert_textbox
         pair: fontname; insert_textbox
         pair: fontsize; insert_textbox
         pair: lineheight; insert_textbox
         pair: morph; insert_textbox
         pair: render_mode; insert_textbox
         pair: miter_limit; insert_textbox
         pair: rotate; insert_textbox
         pair: oc; insert_textbox

      .. method:: insert_textbox(rect, buffer, *, fontsize=11, fontname="helv", fontfile=None, set_simple=False, encoding=TEXT_ENCODING_LATIN, color=None, fill=None, render_mode=0, miter_limit=1, border_width=1, expandtabs=8, align=TEXT_ALIGN_LEFT, rotate=0, lineheight=None, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)

         **仅适用于 PDF**：在指定矩形区域内插入文本。文本将被分割为单词和行，并填充到可用空间中。文本填充的起始角由 `rotate` 参数决定。换行符和多个空格会被保留。

         :arg rect_like rect: 指定文本框的矩形区域。必须是有限且非空的区域。

         :arg str/sequence buffer: 要插入的文本，必须是字符串或字符串序列。字符串序列中的换行符也会被保留。

         :arg int align: 设置文本的对齐方式。默认值为 `0` （左对齐）。还支持居中、右对齐和两端对齐（`TEXT_ALIGN_JUSTIFY`）。需要注意，两端对齐仅适用于 "简单"（单字节）字体，包括 :ref:`Base-14-Fonts`。

         :arg float lineheight: 设定行高调整系数，覆盖字体默认的行高计算方式。如果不为 `None`，行高将设定为 `fontsize * lineheight`。

         :arg int expandtabs: 控制制表符 ``\t`` 的处理方式。每一行都会使用 `string.expandtabs()` 方法进行转换。

         :arg float stroke_opacity: *(v1.18.1 新增)* 设置描边颜色（字符 **边框**）的透明度，仅接受 `0 <= value <= 1` 的值。默认值为 `1`（完全不透明）。
         :arg float fill_opacity: *(v1.18.1 新增)* 设置填充颜色（文本颜色）的透明度，默认值为 `1`（完全不透明）。此参数控制文本颜色的透明度，而 `stroke_opacity` 仅影响字符边框。

         :arg int rotate: 设置文本在矩形区域内的旋转角度。该值必须是 `90` 的整数倍，支持 `0`、 `90`、 `180` 和 `270` （或 `-90` ）：
         
            - `0` （默认）：从左上角开始填充文本。
            - `90`：从 **左下角** 开始填充文本。
            - `180`：从 **右下角** 开始填充文本。
            - `270` （或 `-90` ）：从 **右上角** 开始填充文本。

            旋转参数会覆盖 `morph` 变换效果。请参考以下示例，其中一个示例展示了文本先向左旋转 `90` 度，之后整个矩形围绕左下角顺时针旋转。

         :arg int oc: *(v1.18.4 新增)* 指定文本的 :data:`xref` 号，以使其可按条件显示，适用于 :data:`OCG` 或 :data:`OCMD`。

         :rtype: float
         :returns:
            **如果返回值为正或零**：执行成功，返回未使用的矩形行高（单位：像素）。该值可用于优化矩形、调整后续元素位置等。

            **如果返回值为负**：未能执行，返回值表示插入文本所需的额外空间。建议增大矩形区域、减少字体大小或减少文本量。

         .. image:: images/img-rotate.*

         .. image:: images/img-rot+morph.*

         其他参数的详细说明请参见 :ref:`CommonParms`。

      .. index::
         pair: overlay; commit

      .. method:: commit(overlay=True)

         将累积的绘图和文本插入更新到页面的 :data:`contents`。如果文本与绘图重叠，文本将覆盖在绘图之上。

         .. warning:: **请勿忘记执行此方法：**
         
               如果未提交 (`commit`) 形状，它将被忽略，页面内容不会发生变化！

         该方法会重置以下属性：:attr:`Shape.rect` 、:attr:`lastPoint` 、:attr:`draw_cont` 、:attr:`text_cont` 和 :attr:`totalcont`。之后，该形状对象仍可用于 **同一页面**。

         :arg bool overlay: 是否将内容放置在前景（默认）或背景中。仅当页面已有非空的 :data:`contents` 时才相关。

      **---------- 属性 ----------**

      .. attribute:: doc

         **仅供参考**：所属文档对象。

         :type: :ref:`Document`

      .. attribute:: page

         **仅供参考**：所属页面对象。

         :type: :ref:`Page`

      .. attribute:: height

         页面高度的副本。

         :type: float

      .. attribute:: width

         页面宽度的副本。

         :type: float

      .. attribute:: draw_cont

         **绘图方法** 的累积命令缓冲区。每次执行 `finish` 方法后，该缓冲区的内容会追加到 :attr:`Shape.totalcont`。

         :type: str

      .. attribute:: text_cont

         **文本插入** 的累积缓冲区。所有文本插入都会存储在此缓冲区中，并在执行 :meth:`Shape.commit` 时追加到 :attr:`totalcont`，确保文本不会被同一 `Shape` 内的绘图覆盖。

         :type: str

      .. attribute:: rect

         包围绘图的矩形。此属性供您使用，可以随时更改。当创建或提交形状时，默认值为 *None*。每次调用 *draw* 方法和 :meth:`Shape.insert_textbox` 时，都会更新该属性（即 **根据需要扩大** 矩形）。然而，**变换（Morphing）** 操作（如 :meth:`Shape.finish`、:meth:`Shape.insert_textbox`）会被忽略。

         该属性的典型用法是在创建形状时，将 :attr:`Page.cropbox_position` 设置为该值，尤其是当您为后续或外部使用创建形状时。如果您没有手动操作该属性，它应反映包含所有绘图的矩形。

         如果您使用了变换并需要包含变换对象的矩形，可以使用以下代码：

            >>> # 假设 ...
            >>> morph = (point, matrix)
            >>> # ... 重新计算形状矩形如下：
            >>> shape.rect = (shape.rect - pymupdf.Rect(point, point)) * ~matrix + pymupdf.Rect(point, point)

         :type: :ref:`Rect`

      .. attribute:: totalcont

         累积的绘图和文本插入命令缓冲区。此缓冲区将在 :meth:`Shape.commit` 中使用。

         :type: str

      .. attribute:: lastPoint

         **仅供参考**：当前绘图路径的点。它在 *Shape* 创建时以及每次执行 *finish()* 和 *commit()* 后为 *None*。

         :type: :ref:`Point`

.. tab:: 英文

   This class allows creating interconnected graphical elements on a PDF page. Its methods have the same meaning and name as the corresponding :ref:`Page` methods.

   In fact, each :ref:`Page` draw method is just a convenience wrapper for (1) one shape draw method, (2) the :meth:`Shape.finish` method, and (3) the :meth:`Shape.commit` method. For page text insertion, only the :meth:`Shape.commit` method is invoked. If many draw and text operations are executed for a page, you should always consider using a Shape object.

   Several draw methods can be executed in a row and each one of them will contribute to one drawing. Once the drawing is complete, the :meth:`Shape.finish` method must be invoked to apply color, dashing, width, morphing and other attributes.

   **Draw** methods of this class (and :meth:`Shape.insert_textbox`) are logging the area they are covering in a rectangle (:attr:`Shape.rect`). This property can for instance be used to set :attr:`Page.cropbox_position`.

   **Text insertions** :meth:`Shape.insert_text` and :meth:`Shape.insert_textbox` implicitly execute a "finish" and therefore only require :meth:`Shape.commit` to become effective. As a consequence, both include parameters for controlling properties like colors, etc.

   ================================ =====================================================
   **Method / Attribute**             **Description**
   ================================ =====================================================
   :meth:`Shape.commit`             update the page's contents
   :meth:`Shape.draw_bezier`        draw a cubic Bezier curve
   :meth:`Shape.draw_circle`        draw a circle around a point
   :meth:`Shape.draw_curve`         draw a cubic Bezier using one helper point
   :meth:`Shape.draw_line`          draw a line
   :meth:`Shape.draw_oval`          draw an ellipse
   :meth:`Shape.draw_polyline`      connect a sequence of points
   :meth:`Shape.draw_quad`          draw a quadrilateral
   :meth:`Shape.draw_rect`          draw a rectangle
   :meth:`Shape.draw_sector`        draw a circular sector or piece of pie
   :meth:`Shape.draw_squiggle`      draw a squiggly line
   :meth:`Shape.draw_zigzag`        draw a zigzag line
   :meth:`Shape.finish`             finish a set of draw commands
   :meth:`Shape.insert_text`        insert text lines
   :meth:`Shape.insert_textbox`     fit text into a rectangle
   :attr:`Shape.doc`                stores the page's document
   :attr:`Shape.draw_cont`          draw commands since last :meth:`Shape.finish`
   :attr:`Shape.height`             stores the page's height
   :attr:`Shape.lastPoint`          stores the current point
   :attr:`Shape.page`               stores the owning page
   :attr:`Shape.rect`               rectangle surrounding drawings
   :attr:`Shape.text_cont`          accumulated text insertions
   :attr:`Shape.totalcont`          accumulated string to be stored in :data:`contents`
   :attr:`Shape.width`              stores the page's width
   ================================ =====================================================

   **Class API**

   .. class:: Shape
      :no-index:

      .. method:: __init__(self, page)
         :no-index:

         Create a new drawing. During importing PyMuPDF, the *pymupdf.Page* object is being given the convenience method *new_shape()* to construct a *Shape* object. During instantiation, a check will be made whether we do have a PDF page. An exception is otherwise raised.

         :arg page: an existing page of a PDF document.
         :type page: :ref:`Page`

      .. method:: draw_line(p1, p2)
         :no-index:

         Draw a line from :data:`point_like` objects *p1* to *p2*.

         :arg point_like p1: starting point

         :arg point_like p2: end point

         :rtype: :ref:`Point`
         :returns: the end point, *p2*.

      .. index::
         pair: breadth; draw_squiggle

      .. method:: draw_squiggle(p1, p2, breadth=2)
         :no-index:

         Draw a squiggly (wavy, undulated) line from :data:`point_like` objects *p1* to *p2*. An integer number of full wave periods will always be drawn, one period having a length of *4 * breadth*. The breadth parameter will be adjusted as necessary to meet this condition. The drawn line will always turn "left" when leaving *p1* and always join *p2* from the "right".

         :arg point_like p1: starting point

         :arg point_like p2: end point

         :arg float breadth: the amplitude of each wave. The condition *2 * breadth < abs(p2 - p1)* must be true to fit in at least one wave. See the following picture, which shows two points connected by one full period.

         :rtype: :ref:`Point`
         :returns: the end point, *p2*.

         .. image:: images/img-breadth.*

         Here is an example of three connected lines, forming a closed, filled triangle. Little arrows indicate the stroking direction.

            >>> import pymupdf
            >>> doc=pymupdf.open()
            >>> page=doc.new_page()
            >>> r = pymupdf.Rect(100, 100, 300, 200)
            >>> shape=page.new_shape()
            >>> shape.draw_squiggle(r.tl, r.tr)
            >>> shape.draw_squiggle(r.tr, r.br)
            >>> shape.draw_squiggle(r.br, r.tl)
            >>> shape.finish(color=(0, 0, 1), fill=(1, 1, 0))
            >>> shape.commit()
            >>> doc.save("x.pdf")

         .. image:: images/img-squiggly.*

         .. note:: Waves drawn are **not** trigonometric (sine / cosine). If you need that, have a look at `draw.py <https://github.com/pymupdf/PyMuPDF-Utilities/blob/master/examples/draw-sines/draw.py>`_.

      .. method:: draw_zigzag(p1, p2, breadth=2)
         :no-index:

         Draw a zigzag line from :data:`point_like` objects *p1* to *p2*. Otherwise works exactly like :meth:`Shape.draw_squiggle`.

         :arg point_like p1: starting point

         :arg point_like p2: end point

         :arg float breadth: the amplitude of the movement. The condition *2 * breadth < abs(p2 - p1)* must be true to fit in at least one period.

         :rtype: :ref:`Point`
         :returns: the end point, *p2*.

      .. method:: draw_polyline(points)
         :no-index:

         Draw several connected lines between points contained in the sequence *points*. This can be used for creating arbitrary polygons by setting the last item equal to the first one.

         :arg sequence points: a sequence of :data:`point_like` objects. Its length must at least be 2 (in which case it is equivalent to *draw_line()*).

         :rtype: :ref:`Point`
         :returns: *points[-1]* -- the last point in the argument sequence.

      .. method:: draw_bezier(p1, p2, p3, p4)
         :no-index:

         Draw a standard cubic Bézier curve from *p1* to *p4*, using *p2* and *p3* as control points.

         All arguments are :data:`point_like` objects.

         :rtype: :ref:`Point`
         :returns: the end point, *p4*.

         .. note:: The points do not need to be different -- experiment a bit with some of them being equal!

         Example:

         .. image:: images/img-drawBezier.*

      .. method:: draw_oval(tetra)
         :no-index:

         Draw an "ellipse" inside the given tetragon (quadrilateral). If it is a square, a regular circle is drawn, a general rectangle will result in an ellipse. If a quadrilateral is used instead, a plethora of shapes can be the result.

         The drawing starts and ends at the middle point of the line `bottom-left -> top-left` corners in an anti-clockwise movement.

         :arg rect_like,quad_like tetra: :data:`rect_like` or :data:`quad_like`.

            *Changed in version 1.14.5:*  Quads are now also supported.

         :rtype: :ref:`Point`
         :returns: the middle point of line `rect.bl -> rect.tl`, or resp. `quad.ll -> quad.ul`. Look at just a few examples here, or at the *quad-show?.py* scripts in the PyMuPDF-Utilities repository.

         .. image:: images/img-drawquad.*
            :scale: 50

      .. method:: draw_circle(center, radius)
         :no-index:

         Draw a circle given its center and radius. The drawing starts and ends at point `center - (radius, 0)` in an **anti-clockwise** movement. This point is the middle of the enclosing square's left side.

         This is a shortcut for `draw_sector(center, start, 360, fullSector=False)`. To draw the same circle in a **clockwise** movement, use `-360` as degrees.

         :arg point_like center: the center of the circle.

         :arg float radius: the radius of the circle. Must be positive.

         :rtype: :ref:`Point`
         :returns: `Point(center.x - radius, center.y)`.

            .. image:: images/img-drawcircle.*
               :scale: 60

      .. method:: draw_curve(p1, p2, p3)
         :no-index:

         A special case of *draw_bezier()*: Draw a cubic Bezier curve from *p1* to *p3*. On each of the two lines `p1 -> p2` and `p3 -> p2` one control point is generated. Both control points will therefore be on the same side of the line `p1 -> p3`. This guaranties that the curve's curvature does not change its sign. If the lines to p2 intersect with an angle of 90 degrees, then the resulting curve is a quarter ellipse (resp. quarter circle, if of same length).

         All arguments are :data:`point_like`.

         :rtype: :ref:`Point`
         :returns: the end point, *p3*. The following is a filled quarter ellipse segment. The yellow area is oriented **clockwise:**

            .. image:: images/img-drawCurve.png
               :align: center

      .. method:: draw_sector(center, point, angle, fullSector=True)
         :no-index:

         Draw a circular sector, optionally connecting the arc to the circle's center (like a piece of pie).

         :arg point_like center: the center of the circle.

         :arg point_like point: one of the two end points of the pie's arc segment. The other one is calculated from the *angle*.

         :arg float angle: the angle of the sector in degrees. Used to calculate the other end point of the arc. Depending on its sign, the arc is drawn anti-clockwise (positive) or clockwise.

         :arg bool fullSector: whether to draw connecting lines from the ends of the arc to the circle center. If a fill color is specified, the full "pie" is colored, otherwise just the sector.

         :rtype: :ref:`Point`
         :returns: the other end point of the arc. Can be used as starting point for a following invocation to create logically connected pies charts. Examples:

            .. image:: images/img-drawSector1.*

            .. image:: images/img-drawSector2.*


      .. method:: draw_rect(rect, *, radius=None)
         :no-index:

         * Changed in v1.22.0: Added parameter *radius*.

         Draw a rectangle. The drawing starts and ends at the top-left corner in an anti-clockwise movement.

         :arg rect_like rect: where to put the rectangle on the page.
         :arg multiple radius: draw rounded rectangle corners. If not `None`, specifies the radius of the curvature as a percentage of a rectangle side length. This must one or (a tuple of) two floats `0 < radius <= 0.5`, where 0.5 corresponds to 50% of the respective side. If a float, the radius of the curvature is computed as `radius * min(width, height)`, drawing the corner's perimeter as a quarter circle. If a tuple `(rx, ry)` is given, then the curvature is asymmetric with respect to the horizontal and vertical directions. A value of `radius=(0.5, 0.5)` draws an ellipse.

         :rtype: :ref:`Point`
         :returns: top-left corner of the rectangle.

      .. method:: draw_quad(quad)
         :no-index:

         Draw a quadrilateral. The drawing starts and ends at the top-left corner (:attr:`Quad.ul`) in an anti-clockwise movement. It is a shortcut of :meth:`Shape.draw_polyline` with the argument `(ul, ll, lr, ur, ul)`.

         :arg quad_like quad: where to put the tetragon on the page.

         :rtype: :ref:`Point`
         :returns: :attr:`Quad.ul`.

      .. method:: finish(width=1, color=(0,), fill=None, lineCap=0, lineJoin=0, dashes=None, closePath=True, even_odd=False, morph=(fixpoint, matrix), stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         Finish a set of *draw*()* methods by applying :ref:`CommonParms` to all of them.
         
         It has **no effect on** :meth:`Shape.insert_text` and :meth:`Shape.insert_textbox`.

         The method also supports **morphing the compound drawing** using :ref:`Point` *fixpoint* and :ref:`matrix` *matrix*.

         :arg sequence morph: morph the text or the compound drawing around some arbitrary :ref:`Point` *fixpoint* by applying :ref:`Matrix` *matrix* to it. This implies that *fixpoint* is a **fixed point** of this operation: it will not change its position. Default is no morphing (*None*). The matrix can contain any values in its first 4 components, *matrix.e == matrix.f == 0* must be true, however. This means that any combination of scaling, shearing, rotating, flipping, etc. is possible, but translations are not.

         :arg float stroke_opacity: *(new in v1.18.1)* set transparency for stroke colors. Value < 0 or > 1 will be ignored. Default is 1 (intransparent).
         :arg float fill_opacity: *(new in v1.18.1)* set transparency for fill colors. Default is 1 (intransparent).

         :arg bool even_odd: request the **"even-odd rule"** for filling operations. Default is *False*, so that the **"nonzero winding number rule"** is used. These rules are alternative methods to apply the fill color where areas overlap. Only with fairly complex shapes a different behavior is to be expected with these rules. For an in-depth explanation, see :ref:`AdobeManual`, pp. 137 ff. Here is an example to demonstrate the difference.

         :arg int oc: *(new in v1.18.4)* the :data:`xref` number of an :data:`OCG` or :data:`OCMD` to make this drawing conditionally displayable.

         .. image:: images/img-even-odd.*

         .. note:: For each pixel in a shape, the following will happen:

            1. Rule **"even-odd"** counts, how many areas contain the pixel. If this count is **odd,** the pixel is regarded **inside** the shape, if it is **even**, the pixel is **outside**.

            2. The default rule **"nonzero winding"** in addition looks at the *"orientation"* of each area containing the pixel: it **adds 1** if an area is drawn anti-clockwise and it **subtracts 1** for clockwise areas. If the result is zero, the pixel is regarded **outside,** pixels with a non-zero count are **inside** the shape.

            Of the four shapes in above image, the top two each show three circles drawn in standard manner (anti-clockwise, look at the arrows). The lower two shapes contain one (the top-left) circle drawn clockwise. As can be seen, area orientation is irrelevant for the right column (even-odd rule).

      .. method:: insert_text(point, text, *, fontsize=11, fontname="helv", fontfile=None, set_simple=False, encoding=TEXT_ENCODING_LATIN, color=None, lineheight=None, fill=None, render_mode=0, miter_limit=1, border_width=1, rotate=0, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         Insert text lines starting at ``point``.

         :arg point_like point: the bottom-left position of the first character of *text* in pixels. It is important to understand, how this works in conjunction with the *rotate* parameter. Please have a look at the following picture. The small red dots indicate the positions of *point* in each of the four possible cases.

            .. image:: images/img-inserttext.*
               :scale: 33

         :arg str/sequence text: the text to be inserted. May be specified as either a string type or as a sequence type. For sequences, or strings containing line breaks ``\n``, several lines will be inserted. No care will be taken if lines are too wide, but the number of inserted lines will be limited by "vertical" space on the page (in the sense of reading direction as established by the *rotate* parameter). Any rest of *text* is discarded -- the return code however contains the number of inserted lines.

         :arg float lineheight: a factor to override the line height calculated from font properties. If not `None`, a line height of `fontsize * lineheight` will be used.
         :arg float stroke_opacity: *(new in v1.18.1)* set transparency for stroke colors (the **border line** of a character). Only  `0 <= value <= 1` will be considered. Default is 1 (intransparent).
         :arg float fill_opacity: *(new in v1.18.1)* set transparency for fill colors. Default is 1 (intransparent). Use this value to control transparency of the text color. Stroke opacity **only** affects the border line of characters.

         :arg int rotate: determines whether to rotate the text. Acceptable values are multiples of 90 degrees. Default is 0 (no rotation), meaning horizontal text lines oriented from left to right. 180 means text is shown upside down from **right to left**. 90 means anti-clockwise rotation, text running **upwards**. 270 (or -90) means clockwise rotation, text running **downwards**. In any case, *point* specifies the bottom-left coordinates of the first character's rectangle. Multiple lines, if present, always follow the reading direction established by this parameter. So line 2 is located **above** line 1 in case of `rotate = 180`, etc.

         :arg int oc: *(new in v1.18.4)* the :data:`xref` number of an :data:`OCG` or :data:`OCMD` to make this text conditionally displayable.

         :rtype: int
         :returns: number of lines inserted.

         For a description of the other parameters see :ref:`CommonParms`.

      .. method:: insert_textbox(rect, buffer, *, fontsize=11, fontname="helv", fontfile=None, set_simple=False, encoding=TEXT_ENCODING_LATIN, color=None, fill=None, render_mode=0, miter_limit=1, border_width=1, expandtabs=8, align=TEXT_ALIGN_LEFT, rotate=0, lineheight=None, morph=None, stroke_opacity=1, fill_opacity=1, oc=0)
         :no-index:

         PDF only: Insert text into the specified rectangle. The text will be split into lines and words and then filled into the available space, starting from one of the four rectangle corners, which depends on `rotate`. Line feeds and multiple space will be respected.

         :arg rect_like rect: the area to use. It must be finite and not empty.

         :arg str/sequence buffer: the text to be inserted. Must be specified as a string or a sequence of strings. Line breaks are respected also when occurring in a sequence entry.

         :arg int align: align each text line. Default is 0 (left). Centered, right and justified are the other supported options, see :ref:`TextAlign`. Please note that the effect of parameter value *TEXT_ALIGN_JUSTIFY* is only achievable with "simple" (single-byte) fonts (including the :ref:`Base-14-Fonts`).

         :arg float lineheight: a factor to override the line height calculated from font properties. If not `None`, a line height of `fontsize * lineheight` will be used.

            :arg int expandtabs: controls handling of tab characters ``\t`` using the `string.expandtabs()` method **per each line**.

         :arg float stroke_opacity: *(new in v1.18.1)* set transparency for stroke colors. Negative values and values > 1 will be ignored. Default is 1 (intransparent).
         :arg float fill_opacity: *(new in v1.18.1)* set transparency for fill colors. Default is 1 (intransparent). Use this value to control transparency of the text color. Stroke opacity **only** affects the border line of characters.

         :arg int rotate: requests text to be rotated in the rectangle. This value must be a multiple of 90 degrees. Default is 0 (no rotation). Effectively, the four values `0`, `90`, `180` and `270` (= `-90`) are processed, each causing the text to start in a different rectangle corner. Bottom-left is `90`, bottom-right is `180`, and `-90 / 270` is top-right. See the example how text is filled in a rectangle. This argument takes precedence over morphing. See the second example, which shows text first rotated left by `90` degrees and then the whole rectangle rotated clockwise around is lower left corner.

         :arg int oc: *(new in v1.18.4)* the :data:`xref` number of an :data:`OCG` or :data:`OCMD` to make this text conditionally displayable.

         :rtype: float
         :returns:
            **If positive or zero**: successful execution. The value returned is the unused rectangle line space in pixels. This may safely be ignored -- or be used to optimize the rectangle, position subsequent items, etc.

            **If negative**: no execution. The value returned is the space deficit to store text lines. Enlarge rectangle, decrease *fontsize*, decrease text amount, etc.

         .. image:: images/img-rotate.*

         .. image:: images/img-rot+morph.*

         For a description of the other parameters see :ref:`CommonParms`.

      .. method:: commit(overlay=True)
         :no-index:

         Update the page's :data:`contents` with the accumulated drawings, followed by any text insertions. If text overlaps drawings, it will be written on top of the drawings.
         
         .. warning:: **Do not forget to execute this method:**
         
               If a shape is **not committed, it will be ignored and the page will not be changed!**

         The method will reset attributes :attr:`Shape.rect`, :attr:`lastPoint`, :attr:`draw_cont`, :attr:`text_cont` and :attr:`totalcont`. Afterwards, the shape object can be reused for the **same page**.

         :arg bool overlay: determine whether to put content in foreground (default) or background. Relevant only, if the page already has a non-empty :data:`contents` object.

      **---------- Attributes ----------**

      .. attribute:: doc
         :no-index:

         For reference only: the page's document.

         :type: :ref:`Document`

      .. attribute:: page
         :no-index:

         For reference only: the owning page.

         :type: :ref:`Page`

      .. attribute:: height
         :no-index:

         Copy of the page's height

         :type: float

      .. attribute:: width
         :no-index:

         Copy of the page's width.

         :type: float

      .. attribute:: draw_cont
         :no-index:

         Accumulated command buffer for **draw methods** since last finish. Every finish method will append its commands to :attr:`Shape.totalcont`.

         :type: str

      .. attribute:: text_cont
         :no-index:

         Accumulated text buffer. All **text insertions** go here. This buffer will be appended to :attr:`totalcont` :meth:`Shape.commit`, so that text will never be covered by drawings in the same Shape.

         :type: str

      .. attribute:: rect
         :no-index:

         Rectangle surrounding drawings. This attribute is at your disposal and may be changed at any time. Its value is set to *None* when a shape is created or committed. Every *draw** method, and :meth:`Shape.insert_textbox` update this property (i.e. **enlarge** the rectangle as needed). **Morphing** operations, however (:meth:`Shape.finish`, :meth:`Shape.insert_textbox`) are ignored.

         A typical use of this attribute would be setting :attr:`Page.cropbox_position` to this value, when you are creating shapes for later or external use. If you have not manipulated the attribute yourself, it should reflect a rectangle that contains all drawings so far.

         If you have used morphing and need a rectangle containing the morphed objects, use the following code::

            >>> # assuming ...
            >>> morph = (point, matrix)
            >>> # ... recalculate the shape rectangle like so:
            >>> shape.rect = (shape.rect - pymupdf.Rect(point, point)) * ~matrix + pymupdf.Rect(point, point)

         :type: :ref:`Rect`

      .. attribute:: totalcont
         :no-index:

         Total accumulated command buffer for draws and text insertions. This will be used by :meth:`Shape.commit`.

         :type: str

      .. attribute:: lastPoint
         :no-index:

         For reference only: the current point of the drawing path. It is *None* at *Shape* creation and after each *finish()* and *commit()*.

         :type: :ref:`Point`

用法
------

Usage

.. tab:: 中文

   图形对象的构造方式是通过 `shape = page.new_shape()`。在此之后，可以根据需要进行任意数量的绘制、完成和文本插入方法调用。每个绘制序列必须在提交之前完成。整体的代码模式如下所示：

   >>> shape = page.new_shape()
   >>> shape.draw1(...)
   >>> shape.draw2(...)
   >>> ...
   >>> shape.finish(width=..., color=..., fill=..., morph=...)
   >>> shape.draw3(...)
   >>> shape.draw4(...)
   >>> ...
   >>> shape.finish(width=..., color=..., fill=..., morph=...)
   >>> ...
   >>> shape.insert_text*
   >>> ...
   >>> shape.commit()
   >>> ....

   .. note::

      1. 每个 `finish()` 方法会将之前的绘制操作合并为一个逻辑图形，赋予它统一的颜色、线宽、形态等。如果指定了 `closePath`，它还会将最后一个绘制的终点与第一个绘制的起点连接起来。

      2. 为了成功创建复合图形，确保每个 `draw` 方法的起点是上一个绘制的终点。在上面的伪代码中，`draw2` 方法应该使用 `draw1` 返回的 `Point` 作为起点。如果没有这样做，将自动开始一个新的路径，并且 `finish()` 方法可能无法按预期工作（但不会报错）。

      3. 文本插入可以在 `commit` 之前的任何位置进行，它们不会影响 `Shape.draw_cont` 或 `Shape.lastPoint`。文本将直接追加到 `Shape.totalcont` 中，而绘制操作则由 `Shape.finish` 方法追加。

      4. 每个 `commit` 方法会将所有文本插入和图形放置在页面的前景或背景中，从而提供对图形层次的控制。

      5. **只有** `commit` **会更新** 页面内容，其他方法基本上是字符串操作。

.. tab:: 英文

   A drawing object is constructed by *shape = page.new_shape()*. After this, as many draw, finish and text insertions methods as required may follow. Each sequence of draws must be finished before the drawing is committed. The overall coding pattern looks like this::

      >>> shape = page.new_shape()
      >>> shape.draw1(...)
      >>> shape.draw2(...)
      >>> ...
      >>> shape.finish(width=..., color=..., fill=..., morph=...)
      >>> shape.draw3(...)
      >>> shape.draw4(...)
      >>> ...
      >>> shape.finish(width=..., color=..., fill=..., morph=...)
      >>> ...
      >>> shape.insert_text*
      >>> ...
      >>> shape.commit()
      >>> ....

   .. note::

      1. Each *finish()* combines the preceding draws into one logical shape, giving it common colors, line width, morphing, etc. If *closePath* is specified, it will also connect the end point of the last draw with the starting point of the first one.

      2. To successfully create compound graphics, let each draw method use the end point of the previous one as its starting point. In the above pseudo code, *draw2* should hence use the returned :ref:`Point` of *draw1* as its starting point. Failing to do so, would automatically start a new path and *finish()* may not work as expected (but it won't complain either).

      3. Text insertions may occur anywhere before the commit (they neither touch :attr:`Shape.draw_cont` nor :attr:`Shape.lastPoint`). They are appended to *Shape.totalcont* directly, whereas draws will be appended by *Shape.finish*.

      4. Each *commit* takes all text insertions and shapes and places them in foreground or background on the page -- thus providing a way to control graphical layers.

      5. **Only** *commit* **will update** the page's contents, the other methods are basically string manipulations.

例子
---------

Examples

.. highlight:: python

.. tab:: 中文

   1. 创建一个由不同颜色的饼状图构成的完整圆形
   
      ::

         shape = page.new_shape()  # 开始一个新形状
         cols = (...)  # 一个RGB颜色三元组的序列
         pieces = len(cols)  # 需要绘制的部分数量
         beta = 360. / pieces  # 每个饼块的角度
         center = pymupdf.Point(...)  # 饼的中心
         p0 = pymupdf.Point(...)  # 起始点
         for i in range(pieces):
            p0 = shape.draw_sector(center, p0, beta,
                                    fullSector=True)  # 绘制饼块
            # 现在填充它，但不要连接弧的端点
            shape.finish(fill=cols[i], closePath=False)
         shape.commit()  # 更新页面

      这是五种颜色的示例：

      .. image:: images/img-cake.*

   2. 创建一个规则的n边多边形（黄色填充，红色边框）。我们仅使用 *draw_sector()* 来计算圆周上的点，并在绘制多边形之前清空绘图命令缓冲区
   
      ::

         shape = page.new_shape()  # 开始一个新形状
         beta = -360.0 / n  # 角度，顺时针绘制
         center = pymupdf.Point(...)  # 圆心
         p0 = pymupdf.Point(...)  # 从这里开始（第一个边）
         points = [p0]  # 存储多边形的边
         for i in range(n):  # 计算边
            p0 = shape.draw_sector(center, p0, beta)
            points.append(p0)
         shape.draw_cont = ""  # 不绘制圆的扇形
         shape.draw_polyline(points)  # 绘制多边形
         shape.finish(color=(1,0,0), fill=(1,1,0), closePath=False)
         shape.commit()

      这是 n = 7 时的多边形：

      .. image:: images/img-7edges.*

.. tab:: 英文

   1. Create a full circle of pieces of pie in different colors::

         shape = page.new_shape()  # start a new shape
         cols = (...)  # a sequence of RGB color triples
         pieces = len(cols)  # number of pieces to draw
         beta = 360. / pieces  # angle of each piece of pie
         center = pymupdf.Point(...)  # center of the pie
         p0 = pymupdf.Point(...)  # starting point
         for i in range(pieces):
            p0 = shape.draw_sector(center, p0, beta,
                                 fullSector=True) # draw piece
            # now fill it but do not connect ends of the arc
            shape.finish(fill=cols[i], closePath=False)
         shape.commit()  # update the page

      Here is an example for 5 colors:

      .. image:: images/img-cake.*

   2. Create a regular n-edged polygon (fill yellow, red border). We use *draw_sector()* only to calculate the points on the circumference, and empty the draw command buffer again before drawing the polygon::

         shape = page.new_shape() # start a new shape
         beta = -360.0 / n  # our angle, drawn clockwise
         center = pymupdf.Point(...)  # center of circle
         p0 = pymupdf.Point(...)  # start here (1st edge)
         points = [p0]  # store polygon edges
         for i in range(n):  # calculate the edges
            p0 = shape.draw_sector(center, p0, beta)
            points.append(p0)
         shape.draw_cont = ""  # do not draw the circle sectors
         shape.draw_polyline(points)  # draw the polygon
         shape.finish(color=(1,0,0), fill=(1,1,0), closePath=False)
         shape.commit()

      Here is the polygon for n = 7:

      .. image:: images/img-7edges.*

.. _CommonParms:

公共参数
-------------------

Common Parameters

.. tab:: 中文

   **fontname** (*str*)

      通常有三种选择：

      1. 使用标准的 :ref:`Base-14-Fonts` 中的一种字体。在这种情况下， **必须不** 指定 *fontfile*，如果省略该参数，则使用 *"Helvetica"*。
      2. 选择页面中已使用的字体。然后指定其 **引用** 名称，并在前面加上斜杠 "/"，例如下面的示例。
      3. 指定计算机系统中存在的字体文件。在这种情况下，为此参数选择一个任意但新的名称（不带 "/" 前缀）。

      如果插入的文本应该重用页面中的某个字体，可以使用 :meth:`Page.get_fonts` 中显示的引用名称，如下所示：

      假设字体列表中有项 *[1024, 0, 'Type1', 'NimbusMonL-Bold', 'R366']*，那么指定 *fontname = "/R366", fontfile = None* 来使用字体 *NimbusMonL-Bold*。

   ----

   **fontfile** (*str*)

      计算机上现有字体的文件路径。如果指定了 *fontfile* ，确保使用的 *fontname* **不出现在** 上述列表中。这个新字体将在 *doc.save()* 时嵌入到 PDF 中。与新图像类似，字体文件只会嵌入一次。使用一个 MD5 码表来确保这一点。

   ----

   **set_simple** (*bool*)

      从文件安装的字体默认为 **Type0** 字体。如果只想使用 1 字节字符，将此设置为 true。此设置无法恢复。后续的更改将被忽略。

   ----

   **fontsize** (*float*)

      文本的字体大小，参见： :data:`fontsize`。

   ----

   **dashes** (*str*)

      使线条以虚线方式绘制。一般格式是 `"[n m] p"`，由最多 3 个浮动值组成，表示像素长度。 ``n`` 是虚线的长度， ``m`` （可选）是随后的间隙长度， ``p`` （“相位” - **必需**，即使是 0！）指定在开始绘制虚线之前应跳过多少像素。如果省略 ``m``，它默认为 ``n``。

      使用 `"[] 0"` 或 *None* 或 `""` 绘制连续的线条（无虚线）。示例如下：

      * 指定 `"[3 4] 0"` 意味着虚线长度为 3，间隙长度为 4 像素，依次排列。
      * `"[3 3] 0"` 和 `"[3] 0"` 的效果相同。

      有关如何实现复杂虚线效果的详细信息，请参见 :ref:`AdobeManual`，第 217 页。

   ----

   **color / fill** (*list, tuple*)

      描边和填充颜色可以用 0 到 1 之间的浮点数元组或列表指定。这些序列的长度必须为 1（灰度 GRAY）、3（RGB）或 4（CMYK）。对于灰度色彩空间，除了 *(float,)* 或 *[float]* 这种较繁琐的格式外，也可以直接使用单个浮点数。可以使用默认值（Accept），或者使用 `None` 以忽略该参数。

      为了简化颜色指定，可以使用 *pymupdf.utils* 中的 *getColor()* 方法，通过颜色名称获取预定义的 RGB 颜色三元组。它接受一个字符串作为颜色名称，并返回相应的 RGB 颜色三元组。该方法支持 540 多种颜色名称，详见 :ref:`ColorDatabase`。

      请注意，术语 *color* 在与填充颜色一起使用时通常指的是“描边”颜色。

      如果将颜色参数默认设为 `None`，那么不会生成相应的颜色选择命令。如果 *fill* 和 *color* 都为 `None`，那么绘制内容将不包含任何颜色指定。但仍然会被“描边”，导致 PDF 默认颜色“黑色”被 Adobe Acrobat 及其他查看器使用。

   ----

   **width** (*float*)

      形状元素（如果适用）的描边（“边框”）宽度，默认值为 1。参数 *width*、*color* 和 *fill* 之间的关系如下：

      * 如果 `fill=None`，形状元素始终会带有边框，即使 `color=None` （此时默认使用黑色）或 `width=0` （此时默认使用 1）。
      * 只有在指定填充颜色（可以是白色）时，才可以绘制无边框的形状。要实现这一点，需要指定 `width=0`，此时 ``color`` 参数会被忽略。

   ----

   **stroke_opacity / fill_opacity** (*floats*)

      这两个值的取值范围均为 [0, 1]。负值或大于 1 的值在大多数情况下会被忽略。这两个参数用于设置透明度，其中 0.5 代表 50% 透明度，0 代表完全透明，1 代表不透明。例如，对于矩形，*stroke_opacity* 适用于边框，*fill_opacity* 适用于内部填充。

      对于文本插入（:meth:`Shape.insert_text` 和 :meth:`Shape.insert_textbox`），使用 *fill_opacity* 控制文本透明度。乍看之下这可能令人困惑，但结合 `render_mode` 可以理解：`fill_opacity` 适用于黄色部分，`stroke_opacity` 适用于蓝色部分。

   ----

   **border_width** (*float*)

      设置文本插入的边框宽度。新增于 v1.14.9，仅在 *render_mode* 取值大于 0 时有效。

   ----

   **render_mode** (*int*)

      *自 v1.14.9 版本新增*：取值范围 `range(8)`，用于控制文本外观（适用于 :meth:`Shape.insert_text` 和 :meth:`Shape.insert_textbox`）。详见 :ref:`AdobeManual` 第 246 页。本版本新增了填充色与描边色的区分。

      * 默认值 0：仅使用填充颜色绘制文本。为保持向后兼容性，也可以使用 *color* 参数代替。
      * 渲染模式 1：仅绘制字符（字形）的边框，边框厚度由 *border_width* 参数指定。颜色使用 *color* 参数指定，*fill* 参数被忽略。
      * 渲染模式 2：字形既填充又描边，使用 *fill* 和 *color* 参数，以及指定的边框宽度。可以使用该模式模拟 **加粗文本**，方法是为 *fill* 和 *color* 选择相同的颜色，并设置适当的 *border_width*。
      * 渲染模式 3：字形既不填充也不描边，即文本不可见。

      下列示例使用 `border_width=0.3`，字体大小 `fontsize=15`，描边颜色为蓝色，填充颜色为黄色。

      .. image:: images/img-rendermode.*

   ----

   **miter_limit** (*float*)

      一个浮点数，指定可接受的最大“斜接比”（miter quotient），计算方式为 `miter-length / line-width`。该参数用于文本输出方法，仅在非零渲染模式下相关 —— 也就是说，当字符带有描边（stroked）时才生效。

      当某个字符的描边线在一个锐角（≤ 90°）处相交，并且线宽较大时，可能会出现“尖刺”现象，导致不美观的视觉效果，如下所示。更多背景信息可参考 :ref:`AdobeManual` 第 126 页。

      例如，当连接点以 90° 角度相交时，斜接长度（miter length）为 ``sqrt(2) * line-width``，斜接比（miter quotient）为 ``sqrt(2)``。

      如果 ``miter_limit`` 被超过，则所有超过该比值的连接处都会被截断，呈现为斜切（“butt”）的外观。

      * 默认值为 1（以及任何更小的值），这会确保所有连接点都呈现为斜切（butt）。
      * 设为 ``None`` 时，则使用 PDF 的默认值。

      **示例：**

      出现尖刺的文本（``miter_limit=None``）：

      .. image:: images/spikes-yes.*

      抑制尖刺的文本（``miter_limit=1``）：

      .. image:: images/spikes-no.*

   ----

   **overlay** (*bool*)

      控制元素是出现在前景（默认）还是背景。

   ----

   **morph** (*sequence*)

      使形状（由 *draw()* 方法创建）或文本（由 *insert_textbox()* / *insert_text()* 方法插入）发生“变形”（morphing）。如果不为 *None*，则必须是一个 *(fixpoint, matrix)* 对，其中：

      * *fixpoint* 是一个 :ref:`Point`，作为变换的固定点。
      * *matrix* 是一个 :ref:`Matrix`，可以是任意变换矩阵，但不能是平移变换，即必须满足 *matrix.e == matrix.f == 0*。

      例如：
      
      * 如果 *matrix* 是旋转或缩放变换，则 *fixpoint* 是变换的中心。
      * 如果 *matrix* 是左右或上下翻转变换，则镜像轴分别是经过 *fixpoint* 的垂直线或水平线。

      .. note::  
         一些方法（如 :meth:`Shape.insert_text` 或 :meth:`Shape.draw_rect`）会检查插入的元素是否能够完全适应页面。但是对于变形操作的结果，系统不会进行此类检查，因此完全由开发者自行负责。

   ----

   **lineCap** (*int*) *(已弃用: "roundCap")*

      控制线条端点的外观。默认值为 0，表示线条在指定坐标处以锐角结束。其他选项包括：

      * 0：线条在端点处直接终止，形成锐角。
      * 1：在线条端点处添加一个半圆，半圆的中心为端点，直径等于线宽。
      * 2：在线条端点处添加一个半方形，边长等于线宽，中心为端点。

      *在 v1.14.15 版本修改*

   ----

   **lineJoin** (*int*)

      *自 v1.14.15 版本新增*：控制线条连接处的外观：

      * 0：尖角连接（sharp edge）。
      * 1：圆角连接（rounded join）。
      * 2：斜切连接（cut-off edge，"butt"）。

   ----

   **closePath** (*bool*)

      使绘制的终点自动与起点相连（使用一条直线闭合路径）。

.. tab:: 英文


   **fontname** (*str*)

      In general, there are three options:

      1. Use one of the standard :ref:`Base-14-Fonts`. In this case, *fontfile* **must not** be specified and *"Helvetica"* is used if this parameter is omitted, too.
      2. Choose a font already in use by the page. Then specify its **reference** name prefixed with a slash "/", see example below.
      3. Specify a font file present on your system. In this case choose an arbitrary, but new name for this parameter (without "/" prefix).

      If inserted text should re-use one of the page's fonts, use its reference name appearing in :meth:`Page.get_fonts` like so:

      Suppose the font list has the item *[1024, 0, 'Type1', 'NimbusMonL-Bold', 'R366']*, then specify *fontname = "/R366", fontfile = None* to use font *NimbusMonL-Bold*.

   ----

   **fontfile** (*str*)

      File path of a font existing on your computer. If you specify *fontfile*, make sure you use a *fontname* **not occurring** in the above list. This new font will be embedded in the PDF upon *doc.save()*. Similar to new images, a font file will be embedded only once. A table of MD5 codes for the binary font contents is used to ensure this.

   ----

   **set_simple** (*bool*)

      Fonts installed from files are installed as **Type0** fonts by default. If you want to use 1-byte characters only, set this to true. This setting cannot be reverted. Subsequent changes are ignored.

   ----

   **fontsize** (*float*)

      Font size of text, see: :data:`fontsize`.

   ----

   **dashes** (*str*)

      Causes lines to be drawn dashed. The general format is `"[n m] p"` of (up to) 3 floats denoting pixel lengths. ``n`` is the dash length, ``m`` (optional) is the subsequent gap length, and ``p`` (the "phase" - **required**, even if 0!) specifies how many pixels should be skipped before the dashing starts. If ``m`` is omitted, it defaults to ``n``.
   
      A continuous line (no dashes) is drawn with `"[] 0"` or *None* or `""`. Examples:
      
      * Specifying `"[3 4] 0"` means dashes of 3 and gaps of 4 pixels following each other.
      * `"[3 3] 0"` and `"[3] 0"` do the same thing.
      
      For (the rather complex) details on how to achieve sophisticated dashing effects, see :ref:`AdobeManual`, page 217.

   ----

   **color / fill** (*list, tuple*)

      Stroke and fill colors can be specified as tuples or list of of floats from 0 to 1. These sequences must have a length of 1 (GRAY), 3 (RGB) or 4 (CMYK). For GRAY colorspace, a single float instead of the unwieldy *(float,)* or *[float]* is also accepted. Accept (default) or use `None` to not use the parameter.

      To simplify color specification, method *getColor()* in *pymupdf.utils* may be used to get predefined RGB color triples by name. It accepts a string as the name of the color and returns the corresponding triple. The method knows over 540 color names -- see section :ref:`ColorDatabase`.

      Please note that the term *color* usually means "stroke" color when used in conjunction with fill color.

      If letting default a color parameter to `None`, then no resp. color selection command will be generated. If *fill* and *color* are both `None`, then the drawing will contain no color specification. But it will still be "stroked", which causes PDF's default color "black" be used by Adobe Acrobat and all other viewers.

   ----

   **width** (*float*)

      The stroke ("border") width of the elements in a shape (if applicable). The default value is 1. The values width, color and fill have the following relationship / dependency:

      * If `fill=None` shape elements will always be drawn with a border - even if `color=None` (in which case black is taken) or `width=0` (in which case 1 is taken).
      * Shapes without border can only be achieved if a fill color is specified (which may be white of course). To achieve this, specify `width=0`. In this case, the ``color`` parameter is ignored.

   ----

   **stroke_opacity / fill_opacity** (*floats*)

      Both values are floats in range [0, 1]. Negative values or values > 1 will ignored (in most cases). Both set the transparency such that a value 0.5 corresponds to 50% transparency, 0 means invisible and 1 means intransparent. For e.g. a rectangle the stroke opacity applies to its border and fill opacity to its interior.

      For text insertions (:meth:`Shape.insert_text` and :meth:`Shape.insert_textbox`), use *fill_opacity* for the text. At first sight this seems surprising, but it becomes obvious when you look further down to `render_mode`: `fill_opacity` applies to the yellow and `stroke_opacity` applies to the blue color.

   ----

   **border_width** (*float*)

      Set the border width for text insertions. New in v1.14.9. Relevant only if the render mode argument is used with a value greater zero.

   ----

   **render_mode** (*int*)

      *New in version 1.14.9:* Integer in `range(8)` which controls the text appearance (:meth:`Shape.insert_text` and :meth:`Shape.insert_textbox`). See page 246 in :ref:`AdobeManual`. New in v1.14.9. These methods now also differentiate between fill and stroke colors.

      * For default 0, only the text fill color is used to paint the text. For backward compatibility, using the *color* parameter instead also works.
      * For render mode 1, only the border of each glyph (i.e. text character) is drawn with a thickness as set in argument *border_width*. The color chosen in the *color* argument is taken for this, the *fill* parameter is ignored.
      * For render mode 2, the glyphs are filled and stroked, using both color parameters and the specified border width. You can use this value to simulate **bold text** without using another font: choose the same value for *fill* and *color* and an appropriate value for *border_width*.
      * For render mode 3, the glyphs are neither stroked nor filled: the text becomes invisible.

      The following examples use border_width=0.3, together with a fontsize of 15. Stroke color is blue and fill color is some yellow.

      .. image:: images/img-rendermode.*

   ----

   **miter_limit** (*float*)

      A float specifying the maximum acceptable value of the quotient `miter-length / line-width` ("miter quotient"). Used in text output methods. This is only relevant for non-zero render mode values -- then, characters are written with border lines (i.e. "stroked").
      
      If two lines stroking some character meet at a sharp (<= 90°) angle and the line width is large enough, then "spikes" may become visible -- causing an ugly appearance as shown below. For more background, see page 126 of the :ref:`AdobeManual`.
         
      For instance, when joins meet at 90°, then the miter length is ``sqrt(2) * line-width``, so the miter quotient is ``sqrt(2)``.
      
      If ``miter_limit`` is exceeded, then all joins with a larger qotient will appear as beveled ("butt" appearance).
         
      The default value 1 (and any smaller value) will ensure that all joins are rendered as a butt. A value of ``None`` will use the PDF default value.
      
      Example text showing spikes (``miter_limit=None``):

      .. image:: images/spikes-yes.*

      Example text suppressing spikes (``miter_limit=1``):

      .. image:: images/spikes-no.*

   ----

   **overlay** (*bool*)

      Causes the item to appear in foreground (default) or background.

   ----

   **morph** (*sequence*)

      Causes "morphing" of either a shape, created by the *draw*()* methods, or the text inserted by page methods *insert_textbox()* / *insert_text()*. If not *None*, it must be a pair *(fixpoint, matrix)*, where *fixpoint* is a :ref:`Point` and *matrix* is a :ref:`Matrix`. The matrix can be anything except translations, i.e. *matrix.e == matrix.f == 0* must be true. The point is used as a fixed point for the matrix operation. For example, if *matrix* is a rotation or scaling, then *fixpoint* is its center. Similarly, if *matrix* is a left-right or up-down flip, then the mirroring axis will be the vertical, respectively horizontal line going through *fixpoint*, etc.

      .. note:: Several methods contain checks whether the to be inserted items will actually fit into the page (like :meth:`Shape.insert_text`, or :meth:`Shape.draw_rect`). For the result of a morphing operation there is however no such guaranty: this is entirely the programmer's responsibility.

   ----

   **lineCap (deprecated: "roundCap")** (*int*)

      Controls the look of line ends. The default value 0 lets each line end at exactly the given coordinate in a sharp edge. A value of 1 adds a semi-circle to the ends, whose center is the end point and whose diameter is the line width. Value 2 adds a semi-square with an edge length of line width and a center of the line end.

      *Changed in version 1.14.15*

   ----

   **lineJoin** (*int*)

      *New in version 1.14.15:* Controls the way how line connections look like. This may be either as a sharp edge (0), a rounded join (1), or a cut-off edge (2, "butt").

   ----

   **closePath** (*bool*)

      Causes the end point of a drawing to be automatically connected with the starting point (by a straight line).

.. include:: footer.rst
