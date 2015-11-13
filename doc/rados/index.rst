===============
 Ceph 儲存叢集
===============

所有的 Ceph 部署都始於 :term:`Ceph 儲存叢集`\ 。基於 :abbr:`RADOS (Reliable \
Autonomic Distributed Object Store ，可靠自主的分散式物件儲存)` 的 Ceph 物件\
儲存叢集包含兩類背景行程：term:`物件儲存背景行程`\ （ OSD ）把儲存節點上的資\
料儲存成物件； term:`Ceph 監視器`\ （ MON ）維護叢集狀態圖（Cluster Map）的主要拷貝。一個 \
Ceph 叢集可以擁有幾千個儲存節點，最簡化的系統至少需要一個監視器與兩個 OSD 才能\
做到資料複製。

Ceph 檔案系統、Ceph 物件儲存與 Ceph 區塊裝置叢 Ceph 儲存叢集讀出和寫入資料。

.. raw:: html

	<style type="text/css">div.body h3{margin:5px 0px 0px 0px;}</style>
	<table cellpadding="10"><colgroup><col width="33%"><col width="33%"><col width="33%"></colgroup><tbody valign="top"><tr><td><h3>組態與部署</h3>

Ceph 儲存叢集的某些組態設定是必要的，但大多數都有預設值。典型的部署是透過部署工具定義\
叢集，並啟動監視器的，關於 ``ceph-deploy`` 的詳細請看\ `部署`_\ 。

.. toctree::
	:maxdepth: 2

	組態 <configuration/index>
	部署 <deployment/index>

.. raw:: html 

	</td><td><h3>維運</h3>

部署後就可以開始操作 Ceph 叢集了。

.. toctree::
	:maxdepth: 2


	維運 <operations/index>

.. toctree::
	:maxdepth: 1

	手冊頁 <man/index>


.. toctree:: 
	:hidden:

	故障排除<troubleshooting/index>

.. raw:: html 

	</td><td><h3>應用程式介面（ API ）</h3>

大多數 Ceph 部署都使用了 `Ceph 區塊裝置`_\ 、 `Ceph 物件儲存`_\  與 `Ceph 檔案系\
統`_\ 。也可以開發程式直接與 Ceph 物件儲存串接。

.. toctree::
	:maxdepth: 2

	APIs <api/index>

.. raw:: html

	</td></tr></tbody></table>

.. _Ceph 區塊裝置: ../rbd/rbd
.. _Ceph 檔案系統: ../cephfs/
.. _Ceph 物件儲存: ../radosgw/
.. _部署: ../rados/deployment/
