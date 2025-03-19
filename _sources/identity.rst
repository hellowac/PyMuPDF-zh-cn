.. include:: header.rst

.. _Identity:

============
Identity
============

.. tab:: 中文

    Identity 是一个 :ref:`Matrix`，它不执行任何操作——用于需要矩阵的语法，但不进行实际变换的情况。其形式为 *pymupdf.Matrix(1, 0, 0, 1, 0, 0)*。

    Identity 是一个常量，一个“不可变”对象。因此，所有矩阵属性都是只读的，并且其方法被禁用。

    如果你需要一个 **可变** 的单位矩阵作为起始点，可以使用以下语句之一::

        >>> m = pymupdf.Matrix(1, 0, 0, 1, 0, 0)  # 指定数值
        >>> m = pymupdf.Matrix(1, 1)              # 使用因子为1的缩放
        >>> m = pymupdf.Matrix(0)                 # 使用零度的旋转
        >>> m = pymupdf.Matrix(pymupdf.Identity)  # 复制 Identity


.. tab:: 英文

    Identity is a :ref:`Matrix` that performs no action -- to be used whenever the syntax requires a matrix, but no actual transformation should take place. It has the form *pymupdf.Matrix(1, 0, 0, 1, 0, 0)*.

    Identity is a constant, an "immutable" object. So, all of its matrix properties are read-only and its methods are disabled.

    If you need a **mutable** identity matrix as a starting point, use one of the following statements::

        >>> m = pymupdf.Matrix(1, 0, 0, 1, 0, 0)  # specify the values
        >>> m = pymupdf.Matrix(1, 1)              # use scaling by factor 1
        >>> m = pymupdf.Matrix(0)                 # use rotation by zero degrees
        >>> m = pymupdf.Matrix(pymupdf.Identity)     # make a copy of Identity

.. include:: footer.rst
