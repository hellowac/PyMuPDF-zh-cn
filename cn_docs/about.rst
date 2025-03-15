.. include:: header.rst

.. _About:



.. _About_Features:

功能比较
-----------------------------------------------

Features Comparison

.. _About_Feature_Matrix:

功能矩阵
~~~~~~~~~~~~~~~~~~~

Feature Matrix

.. tab:: 中文

   下表说明了|PyMuPDF|与其他典型解决方案的比较。
   
.. tab:: 英文

   The following table illustrates how |PyMuPDF| compares with other typical solutions.


.. include:: about-feature-matrix.rst

----

.. image:: images/icons/icon-docx.svg
          :width: 40
          :height: 40

.. image:: images/icons/icon-xlsx.svg
          :width: 40
          :height: 40

.. image:: images/icons/icon-pptx.svg
          :width: 40
          :height: 40


.. image:: images/icons/icon-hangul.svg
          :width: 40
          :height: 40


.. tab:: 中文

   .. note::

      关于 **Office** 文档类型 (DOCX、XLXS、PPTX) 和 **Hangul** 文档 (HWPX) 的说明。这些文档可以加载到 |PyMuPDF| 中，您将收到一个 :ref:`Document <Document>` 对象。

      有一些注意事项：


         - 我们将输入转换为 **HTML** 来布局内容。
         - 因此，原来的页面分离已经消失。

      保存结果时，不能期望其忠实地再现原始布局。

      因此输入文件大多采用对文本提取有用的形式。
   
.. tab:: 英文

   .. note::

      A note about **Office** document types (DOCX, XLXS, PPTX) and **Hangul** documents (HWPX). These documents can be loaded into |PyMuPDF| and you will receive a :ref:`Document <Document>` object.

      There are some caveats:


         - we convert the input to **HTML** to layout the content.
         - because of this the original page separation has gone.

      When saving out the result any faithful representation of the original layout cannot be expected.

      Therefore input files are mostly in a form that's useful for text extraction.


----

.. _About_Performance:

性能
-----------------------------------------------

Performance

.. tab:: 中文

   为了对 |PyMuPDF| 针对一系列任务的性能进行基准测试，我们使用了一组固定的 :ref:`8 个 PDF，共 7,031 页<Appendix4_Files_Used>` 测试套件，其中包含文本和图像，用于获取性能时间。

   以下是按任务分组的当前结果：

.. tab:: 英文

   To benchmark |PyMuPDF| performance against a range of tasks a test suite with a fixed set of :ref:`8 PDFs with a total of 7,031 pages<Appendix4_Files_Used>` containing text & images is used to obtain performance timings.

   Here are current results, grouped by task:

.. include:: about-performance.rst

.. raw:: html

   <br/>

.. tab:: 中文

   .. note::

      有关这些性能计时方法的更多详细信息，请参阅 :ref:`性能比较方法<Appendix4>` 。

.. tab:: 英文

   .. note::

      For more detail regarding the methodology for these performance timings see: :ref:`Performance Comparison Methodology<Appendix4>`.

.. _About_License:

许可和版权
----------------------

License and Copyright

.. tab:: 中文


   |PyMuPDF| 和 :title:`MuPDF` 现可在开源 :title:`AGPL` 和商业许可协议下使用。请阅读 :title:`AGPL` 许可协议的全文，可在分发材料（文件 COPYING）和 `此处 <https://www.gnu.org/licenses/agpl-3.0.html>`_ 中找到，以确保您的用例符合许可指南。如果您确定无法满足 :title:`AGPL` 的要求，请联系 `Artifex <https://artifex.com/contact/pymupdf-inquiry.php?utm_source=rtd-pymupdf&utm_medium=rtd&utm_content=inline-link>`_ 以获取有关商业许可的更多信息。

   .. raw:: html

      <button id="licenseButton" class="cta orange" onclick="window.location='https://artifex.com/licensing?utm_source=rtd-pymupdf&utm_medium=rtd&utm_content=cta-button'">了解有关许可的更多信息</button>
      <p></p>

      <script>
         let langC = document.getElementsByTagName('html')[0].getAttribute('lang');

         if (langC=="ja") {
            document.getElementById("licenseButton").innerHTML = "さらに詳しく";
         }

      </script>



   :title:`Artifex` 是 :title:`MuPDF` 的独家商业授权代理商。

   :title:`Artifex` 、:title:`Artifex` 徽标、:title:`MuPDF` 和 :title:`MuPDF` 徽标是 :title:`Artifex Software Inc.` 的注册商标。

.. tab:: 英文

   |PyMuPDF| and :title:`MuPDF` are now available under both, open-source :title:`AGPL` and commercial license agreements. Please read the full text of the :title:`AGPL` license agreement, available in the distribution material (file COPYING) and `here <https://www.gnu.org/licenses/agpl-3.0.html>`_, to ensure that your use case complies with the guidelines of the license. If you determine you cannot meet the requirements of the :title:`AGPL`, please contact `Artifex <https://artifex.com/contact/pymupdf-inquiry.php?utm_source=rtd-pymupdf&utm_medium=rtd&utm_content=inline-link>`_ for more information regarding a commercial license.

   .. raw:: html

      <button id="licenseButton" class="cta orange" onclick="window.location='https://artifex.com/licensing?utm_source=rtd-pymupdf&utm_medium=rtd&utm_content=cta-button'">Find out more about Licensing</button>
      <p></p>

      <script>
         let langC = document.getElementsByTagName('html')[0].getAttribute('lang');

         if (langC=="ja") {
            document.getElementById("licenseButton").innerHTML = "さらに詳しく";
         }

      </script>



   :title:`Artifex` is the exclusive commercial licensing agent for :title:`MuPDF`.

   :title:`Artifex`, the :title:`Artifex` logo, :title:`MuPDF`, and the :title:`MuPDF` logo are registered trademarks of :title:`Artifex Software Inc.`


.. include:: version.rst

.. include:: footer.rst
