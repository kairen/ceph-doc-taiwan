==============
 安裝（快速）
==============

.. raw:: html

	<style type="text/css">div.body h3{margin:5px 0px 0px 0px;}</style>
	<table cellpadding="10"><colgroup><col width="33%"><col width="33%"><col width="33%"></colgroup><tbody valign="top"><tr><td><h3>步驟一：事前準備</h3>

:term:`Ceph 客戶端`\ 和 :term:`Ceph 節點`\ 需要基本規格配置才能部署 Ceph 儲存叢\
集，你也可以加入 Ceph 社群請求幫助。

.. toctree::

   事前檢查 <quick-start-preflight>

.. raw:: html

	</td><td><h3>步驟二：儲存叢集</h3>

完成事前檢查表後，應該可以開始部署 Ceph 儲存叢集了。

.. toctree::

	儲存叢集入門 <quick-ceph-deploy>


.. raw:: html

	</td><td><h3>步骤三： Ceph 客戶端</h3>

大多數 Ceph 使用者不會直接使 Ceph 儲存叢集的儲存物件(RadosGW)來使用，他們一般會用 Ceph 區塊裝置、 \
Ceph 檔案系統、或 Ceph 物件儲存三個功能之一或多個。

.. toctree::

   區塊裝置入門 <quick-rbd>
   檔案系統入門 <quick-cephfs>
   物件儲存入門 <quick-rgw>

.. raw:: html

	</td></tr></tbody></table>
