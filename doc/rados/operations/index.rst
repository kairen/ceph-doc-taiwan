==========
 叢集維護
==========

.. raw:: html

	<table><colgroup><col width="50%"><col width="50%"></colgroup><tbody valign="top"><tr><td><h3>進階維護</h3>

進階叢集操作主要包括用 ``ceph`` 服務管理腳本啟動、暫停、重開叢集與叢集健康狀態檢\
查、監控和操作叢集。

.. toctree::
	:maxdepth: 1 

	operating
	monitoring
	monitoring-osd-pg
	user-management

.. raw:: html 

	</td><td><h3>資料放置</h3>

你的叢集開始運行後，就可以嘗試資料放置了。 Ceph 是 PB 級資料儲存叢集，它用 CRUSH \
演算法、靠儲存池（Pool）和放置群組（PG）在叢集内分散資料。

.. toctree::
	:maxdepth: 1

	data-placement
	pools
	erasure-code
	cache-tiering
	placement-groups
	crush-map



.. raw:: html

	</td></tr><tr><td><h3>低階維護</h3>

低階叢集維護包括啟動、暫停與重新啟動叢集内的某個具體背景行程；更改某背景行程或子系統配\
置；新增或移除背景行程。低階維護還經常遇到擴展、縮減 Ceph 叢集，以及更換老舊、或損\
壞的硬體。

.. toctree::
	:maxdepth: 1

	add-or-rm-osds
	add-or-rm-mons
	指令參考 <control>



.. raw:: html

	</td><td><h3>故障排除</h3>

Ceph 仍在積極開發中，所以可能會碰到一些問題，需要評估 Ceph 組態檔案，並修改 Logs 和調\
試選項來糾正它。

.. toctree::
	:maxdepth: 1 

	../troubleshooting/community
	../troubleshooting/troubleshooting-mon
	../troubleshooting/troubleshooting-osd
	../troubleshooting/troubleshooting-pg
	../troubleshooting/log-and-debug
	../troubleshooting/cpu-profiling
	../troubleshooting/memory-profiling




.. raw:: html

	</td></tr></tbody></table>
