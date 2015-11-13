===========
 Ceph 術語
===========

Ceph is growing rapidly. As firms deploy Ceph, the technical terms such as
"RADOS", "RBD," "RGW" and so forth require corresponding marketing terms
that explain what each component does. The terms in this glossary are 
intended to complement the existing technical terminology.

Sometimes more than one term applies to a definition. Generally, the first
term reflects a term consistent with Ceph's marketing, and secondary terms
reflect either technical terms or legacy ways of referring to Ceph systems.


.. glossary:: 

	Ceph Project
        Ceph 專案
		關於 Ceph 的團隊、軟體、任務和基礎架構的統稱。

	cephx
		Ceph 的認證協定, Ceph 的運作機制類似Kerberos ,但它沒有單故障點(SPOF)。

	Ceph
	Ceph Platform
        Ceph 平台
		所有與 Ceph 相關的軟體，包括所有都位於 `http://github.com/ceph`_ \
		的原始碼資源庫。

	Ceph System
	Ceph Stack
        Ceph 系統
        Ceph 軟體堆疊
		Ceph 之中兩個或更多元件的組合。

	Ceph Node
	Node
	Host
        Ceph 節點
        節點
        主機
		Ceph 系統內的任意單一機器或伺服器。

	Ceph Storage Cluster
	Ceph Object Store
	RADOS
	RADOS Cluster
	Reliable Autonomic Distributed Object Store
        Ceph 儲存叢集
        Ceph 物件儲存
        RADOS 叢集
        可靠自主的分散式物件儲存
		The core set of storage software which stores the user's data (MON+OSD).

	Ceph Cluster Map
	cluster map
        Ceph 叢集狀態圖
        叢集狀態圖
		The set of maps comprising the monitor map, OSD map, PG map, MDS map and 
		CRUSH map. See `Cluster Map`_ for details.

	Ceph Object Storage
        Ceph 物件儲存
		The object storage "product", service or capabilities, which consists
		essentially of a Ceph Storage Cluster and a Ceph Object Gateway.

	Ceph Object Gateway
	RADOS Gateway
	RGW
        Ceph 物件閘道器
        RADOS 閘道器
		The S3/Swift gateway component of Ceph.

	Ceph Block Device
	RBD
        Ceph 區塊裝置
        RBD
		The block storage component of Ceph.

	Ceph Block Storage
        Ceph 區塊儲存
		The block storage "product," service or capabilities when used in 
		conjunction with ``librbd``, a hypervisor such as QEMU or Xen, and a
		hypervisor abstraction layer such as ``libvirt``.

	Ceph Filesystem
	CephFS
	Ceph FS
        Ceph 檔案系統
		The POSIX filesystem components of Ceph.

	Cloud Platforms
	Cloud Stacks
        雲端平台
        雲端軟體堆疊
		Third party cloud provisioning platforms such as OpenStack, CloudStack, 
		OpenNebula, ProxMox, etc.

	Object Storage Device
	OSD
        物件儲存裝置
		A physical or logical storage unit (*e.g.*, LUN).
		Sometimes, Ceph users use the
		term "OSD" to refer to :term:`Ceph OSD Daemon`, though the
		proper term is "Ceph OSD".

	Ceph OSD Daemon
	Ceph OSD
        Ceph 物件儲存背景行程
        Ceph OSD 背景行程
		The Ceph OSD software, which interacts with a logical
		disk (:term:`OSD`). Sometimes, Ceph users use the
		term "OSD" to refer to "Ceph OSD Daemon", though the
		proper term is "Ceph OSD".

	Ceph Monitor
	MON
        Ceph 監視器
        監視器
		The Ceph monitor software.

	Ceph Metadata Server
	MDS
        Ceph Metadata 伺服器
        Metadata 伺服器
		The Ceph metadata software.

	Ceph Clients
	Ceph Client
        Ceph 客戶端
		The collection of Ceph components which can access a Ceph Storage 
		Cluster. These include the Ceph Object Gateway, the Ceph Block Device, 
		the Ceph Filesystem, and their corresponding libraries, kernel modules, 
		and FUSEs.

	Ceph Kernel Modules
        Ceph 核心模組
		The collection of kernel modules which can be used to interact with the 
		Ceph System (e.g,. ``ceph.ko``, ``rbd.ko``).

	Ceph Client Libraries
        Ceph 客戶端函式庫
		The collection of libraries that can be used to interact with components 
		of the Ceph System.

	Ceph Release
        Ceph 發布
		Any distinct numbered version of Ceph.

	Ceph Point Release
        Ceph 修正版發布
		Any ad-hoc release that includes only bug or security fixes.

	Ceph Interim Release
        Ceph 臨時發布
		Versions of Ceph that have not yet been put through quality assurance
		testing, but may contain new features.

	Ceph Release Candidate
        Ceph 預發布版本
		A major version of Ceph that has undergone initial quality assurance 
		testing and is ready for beta testers.

	Ceph Stable Release
        Ceph 穩定版本
		A major version of Ceph where all features from the preceding interim 
		releases have been put through quality assurance testing successfully.

	Ceph Test Framework
	Teuthology
        Ceph 測試框架
        測試方法論
		The collection of software that performs scripted tests on Ceph.

	CRUSH(可控制的、複製、可擴展的雜湊演算法)
		Controlled Replication Under Scalable Hashing. It is the algorithm
		Ceph uses to compute object storage locations.

	ruleset
        規則集合
		A set of CRUSH data placement rules that applies to a particular pool(s).

	Pool
	Pools
        儲存池
		儲存池是物件儲存的邏輯部分。

.. _http://github.com/ceph: http://github.com/ceph
.. _Cluster Map: ../architecture#cluster-map
