==========
 Ceph 介紹
==========

不管你是否想為\ :term:`雲端平台`\ 提供 :term:`Ceph 物件儲存`\ 或 :term:`Ceph \
區塊裝置`\ ，都應該部署一個 :term:`Ceph 檔案系統`\ ，以備不時之需；所有 \
:term:`Ceph 儲存叢集`\ 的部署都始於各 :term:`Ceph 節點`\ 、網路和 Ceph 儲存叢\
集。最簡單的 Ceph 儲存叢集至少要一個監視器(Monitor)和兩個 OSD 背景行程，只有運行 Ceph 檔\
案系統時，Metadata 伺服器才是需要運行。

.. ditaa::  +---------------+ +---------------+ +---------------+
            |      OSDs     | |    Monitor    | |      MDS      |
            +---------------+ +---------------+ +---------------+

- **Ceph OSDs**: :term:`Ceph 物件儲存背景行程`\ （Ceph OSD）的功能是儲存資料，處\
  理資料複製、恢復、回填與重新平衡，並向監視器提供鄰近 OSD 的心跳檢查。一個 Ceph 儲存\
  叢集自動複製兩份資料時，至少需要兩個 OSD 背景行程才能達到 ``active+clean`` \
  狀態（Ceph 預設複製兩份，該值是可以調整的）。

- **Monitors**: :term:`Ceph 監視器`\ 維護著各種叢集狀態圖，包括 Monitor 狀態圖，OSD \
  狀態、放置群組（PG）狀態圖與 CRUSH 狀態圖。 Ceph 維護著監視器、OSD 和 PG 各自的狀態變\
  化歷史紀錄（稱為 epoch）。

- **MDSs**: :term:`Ceph Metadata 伺服器`\ （MDS）是用來 :term:`Ceph 檔案系統`\ 儲存 Metadata\
  使用（也就是說 Ceph 區塊裝置和 Ceph 物件儲存不依賴 Metadata）。Metadata 伺服器有益於 \
  POSIX 檔案系統語意調用，像是 ``ls``\ 、\ ``find`` 等等指令，因為此無需每次直接讀取 Ceph \
  儲存叢。

Ceph 把客戶端資料保存於儲存池（Pools）內的物件。根據 CRUSH 演算法，Ceph 可計算出哪個放置\
群組（PG）應該持有指定物件，然後進一步計算出哪個 OSD 背景行程持有放置群組，正因為有了 \
CRUSH 演算法， Ceph 儲存叢集才具備動態延展、重新平衡和自動修復功能。


.. raw:: html

	<style type="text/css">div.body h3{margin:5px 0px 0px 0px;}</style>
	<table cellpadding="10"><colgroup><col width="50%"><col width="50%"></colgroup><tbody valign="top"><tr><td><h3>建議</h3>

開始把 Ceph 用於生產環境前，您應該看看我們的硬體和作業系統建議。

.. toctree::
   :maxdepth: 2

   硬體推薦 <hardware-recommendations>
   作業系统推薦 <os-recommendations>


.. raw:: html

	</td><td><h3>參與</h3>

   歡迎您加入社群，貢獻文件、程式碼，或發現軟體缺陷(Bugs)。

.. toctree::
   :maxdepth: 2

   get-involved
   documenting-ceph

.. raw:: html

	</td></tr></tbody></table>
