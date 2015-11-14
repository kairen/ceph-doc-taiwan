====================
 歡迎來到 Ceph 世界
====================

Ceph 獨一無二地在統一的系統提供了\ **物件、區塊、和檔案儲存功能**\ 。

.. raw:: html

	<style type="text/css">div.body h3{margin:5px 0px 0px 0px;}</style>
	<table cellpadding="10"><colgroup><col width="33%"><col width="33%"><col width="33%"></colgroup><tbody valign="top"><tr><td><h3>CEPH 物件儲存</h3>

- RESTful 介面
- 與 S3 和 Swift 相容的 API
- S3 風格的子域名
- 統一的 S3/Swift 命名空間
- 使用者管理
- 利用率追蹤
- 等量化物件（Striped objects）
- 雲端解決方案整合
- 多站點部署
- 災難復原

.. raw:: html 

	</td><td><h3>Ceph 區塊裝置</h3>


- 精簡空間配置
- 映像檔大小最大支援 16 EB（exabytes）
- 可組態的等量化（Configurable striping）
- 記憶體快取
- 快照
- 寫入時複製 cloning
- 支援核心級驅動
- 支援 KVM 與 libvirt
- 可作為雲端解決方案的後端
- 累積備份

.. raw:: html 

	</td><td><h3>Ceph 檔案系統</h3>

- 與 POSIX 語意相容
- Metadata 與資料分離
- 動態重新平衡(Dynamic rebalancing)
- 子目錄快照
- 可組態的等量化（Configurable striping）
- 核心驅動的支援
- 支援使用者空間檔案系統（FUSE）
- 可作為 NFS/CIFS 部署
- 可用於 Hadoop 上（替代 HDFS ）

.. raw:: html

	</td></tr><tr><td>

詳細請見 `Ceph 物件儲存`_\ 。

.. raw:: html

	</td><td>

詳細請見 `Ceph 區塊裝置`_\ 。

.. raw:: html

	</td><td>

詳細請見 `Ceph 檔案系統`_\ 。

.. raw::	html 

	</td></tr></tbody></table>

Ceph 擁有高可靠性、管理簡單，並且是開放式原始碼。 Ceph 的強大可以改變您公司的IT基礎架\
夠和巨量資料管理能力。想試看看 Ceph 的話，可以看\ `入門`_\ 指南；想深入理解可以看\ \
`整體架構`_\ 一節。


.. _Ceph 物件儲存: radosgw
.. _Ceph 區塊裝置: rbd/rbd
.. _Ceph 檔案系統: cephfs
.. _入門: start
.. _整體架構: architecture

.. toctree::
   :maxdepth: 1
   :hidden:

   start/intro
   start/index
   install/index
   rados/index
   cephfs/index
   rbd/rbd
   radosgw/index
   api/index
   architecture
   開發文件 <dev/index>
   release-notes
   releases
   Ceph 術語 <glossary>
