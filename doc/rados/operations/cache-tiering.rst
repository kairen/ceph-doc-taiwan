==========
 分層快取（Cache Tiering）
==========

分層快取提供了 Ceph client 與儲存在後端的儲存層資料子集，可以有更好的 I/O 效能。需要透過一個高速昂貴的儲存裝置（SSD）組成的 Pool 當作 Cache tier 以及一個低速廉價的儲存裝置（HDD）組成的 Pool（或者是 Erasure Coded）當作 Economical storage tier。Ceph objecter 處理物件該往哪裡儲存，Tiering agent 則處理何時從 cache 中，將物件 flush 到後端儲存層。所以 cache tier 與 backing storage tier 對 Ceph client 來說是完全透明的。


.. ditaa::
           +-------------+
           | Ceph client |
           +------+------+
                  ^
     Tiering is   |
    Transparent   |              Faster I/O
        to Ceph   |           +---------------+
     client Ops   |           |               |
                  |    +----->+   Cache Tier  |
                  |    |      |               |
                  |    |      +-----+---+-----+
                  |    |            |   ^
                  v    v            |   |   Active Data in Cache Tier
           +------+----+--+         |   |
           |   Objecter   |         |   |
           +-----------+--+         |   |
                       ^            |   |   Inactive Data in Storage Tier
                       |            v   |
                       |      +-----+---+-----+
                       |      |               |
                       +----->|  Storage Tier |
                              |               |
                              +---------------+
                                 Slower I/O


Cache tiering agent 自動處理 cache tier 與 backing storage tier 之間的資料搬移。\
但是管理上可以設置這些搬移的規則，主要有兩種情況：

- **回寫模式（Writeback Mode）：** 當管理員把 cache tier 設置為 ``writeback`` 模式时，Ceph client 會資料寫入 cache tier 並接收來自 cache tier 發送的 ACK；隨著時間的推移，寫入到 cache tier 中的資料會搬移到 storage tier 並從 cache tier 刷新掉。從概念上來說 cache tier 位於 backing storage tier 的前面，當 Ceph client 需要要讀取位於 storage tier 的資料時，cache tiering agent 會把這些資料搬移到 cache tier，然後再送往 Ceph client。此後，Ceph client 將與 caceh tier 進行 I/O 操作，直到資料不再被讀寫。此模式對於易變資料（熱資料）來說較為理想（如照片/視訊串流、事物資料等）。

- **僅讀模式（Read-only Mode）：** 當管理員把 cache tier 設置為 ``readonly`` 模式时， Ceph 會直接把資料寫入後端。讀取時，Ceph 會把對應的物件從 backing tier 複製到 cache tier，將根據定義的策略與 ``dirty 物件`` 從 cache tier 移出。此模式適合不變的資料（冷資料），如社交網路上顯示的圖片與視訊、DNA 資料與 X-Ray 照片等，因為 cache 儲存池讀出的資料可能包含過期的資料，因此一致性較差。對易變資料不要用 ``readonly`` 模式。

正因為所有的 Ceph client 都能用 Cache tiering，所以才能提升區塊設備、物件儲存、Ceph 檔案系統與原生的 I/O 效能的潛力。


設置儲存池
==========

要設定 Cache tier，必須要有兩個儲存池。一個作為 ``backing storage``，另一個作為 ``cache``。


設定 Backing storage 儲存池
--------------

設定 backing storage 儲存池通常會遇到兩種情況：

- **標準儲存（Standard storage）：** 此時，Ceph 儲存叢集內的儲存池，會保存了一個物件的多個 ``副本``。

- **抹除碼（Erasure coding）：** 此時，儲存池使用 Erasure coding 更有效地儲存資料，但效能會稍有損失。

在 Standard storage 情況中，你可以設置 CRUSH 規則集合來標示 ``failure domain`` （諸如：osd、host、chassis、rack、 row...etc.）。當規則集合涉及的所有驅動規格、速度（轉速與吞吐量）與類型相同時， OSD 背景行程會執行的最佳。建立 CRUSH 規則集合的細節可以參考  `CRUSH Map`_ 。建立好 CRUSH 規則集合後，再建立 backing storage 儲存池。

在 Erasure coding 情況中，建立儲存池時指定好參數就會自動生成合適的規則集合，細節可參考 `建立儲存池`_ 。

在後續例子，我們會把 ``cold-storage`` 當作 backing storage 儲存池。


設定 Cache 儲存池
----------

Cache 儲存池的設定步驟大致與 Standard storage 差不多，不同的部分為 cache tier 所使用的驅動通常都是高效能的，且安裝在專用伺服器上，有自己的規則集合。制定規則集合時，需要考慮到裝有高效能驅動裝置的主機，並過濾沒有高效能驅動裝置的主機。詳細可參考 `放置不同的 OSD 於不同儲存池`_ 。

在後續範例中，``hot-storage`` 會作為 cache 儲存池，``cold-storage`` 會作為 backing storage 儲存池。

關於 Cache tier 組態以及預設值的詳細解釋，可以參考 `儲存池——調整儲存池`_ 。


建立一個分層快取
==========

設定一個 Cache tier 需要把 cache 儲存池串接到 backing storage 儲存池上： ::

	ceph osd tier add {storagepool} {cachepool}

例如： ::

	ceph osd tier add cold-storage hot-storage

用下面指令設定快取模式： ::

	ceph osd tier cache-mode {cachepool} {cache-mode}

例如： ::

	ceph osd tier cache-mode hot-storage writeback

Cache Tiers 覆蓋於 backing storage tier 之上，所以我們要多設定一個步驟：必須把所有 client 的流量從儲存池搬移到 cache 儲存池。使用以下指令將 client 流量指向 cache 儲存池： ::

	ceph osd tier set-overlay {storagepool} {cachepool}

例如： ::

	ceph osd tier set-overlay cold-storage hot-storage


配置一個分層快取
==========

Cache tier 支援了幾個設定選項，可按下列指令設定： ::

	ceph osd pool set {cachepool} {key} {value}

詳細參考 `儲存池——調整儲存池`_ 。


目標大小與類型
--------------

在 Ceph 生產環境下，Cache tier 的 ``hit_set_type`` 參數使用一個 `Bloom Filter`_ ： ::

	ceph osd pool set {cachepool} hit_set_type bloom

例如： ::

	ceph osd pool set hot-storage hit_set_type bloom

``hit_set_count`` 與 ``hit_set_period`` 選項可控制各種 HitSet 計算的時間區間，以及保留多少個這樣的 HitSet。目前 ``hit_set_count`` > 1 微小的優勢，由於 agent 還不能處理複雜的訊息。 ::

	ceph osd pool set {cachepool} hit_set_count 1
	ceph osd pool set {cachepool} hit_set_period 3600
	ceph osd pool set {cachepool} target_max_bytes 1000000000000

分級存取隨著時間允許 Ceph，來確認一個 Ceph client 是否在一段時間內存取了某個物件一次或多次（“age” vs “temperature”）。

``min_read_recency_for_promote`` 定義了多少 HitSets 以檢查
處理讀取操作時存在的物件。檢查結果被用來決定是否以異步方式推動物件。 其值應介於 0 到 ``hit_set_count`` 值之間。如果設置為 0，則該物件總是被推動。如果它被設置為 1，則當前的 HitSet 會被檢查。如果這個物件是當前 HitSet 的話，它會被推動，反之則不會推動。對於其他的值，歸檔 HitSets 的準確數量進行檢查。如果物件被找到最新的 ``min_read_recency_for_promote`` HitSets，這個物件就會被推動。

類似的參數可以進行寫入操作的設定，在 ``min_write_recency_for_promote`` 參數進行設定。 ::

  ceph osd pool set {cachepool} min_read_recency_for_promote 1
  ceph osd pool set {cachepool} min_write_recency_for_promote 1

.. note:: 當統計時間越長、數量越多， ``ceph-osd`` 背景行程消耗的記憶體就越多，特別是 agent 正\
   忙著 flush 或 evict 物件時，此時所有 ``hit_set_count`` 的 HitSet 都要載入記憶體。


快取大小（CACHE SIZING）
------------

Cache tiering agent 主要有兩個功能：

- **Flushing：** Agent 會識別修改過（或者變質）的物件，並把它們轉發給儲存池作為長期的儲存。

- **Evicting：** Agent 會識別未修改過（或者乾淨）的物件，並把未用過的物件移出 cache。


相對大小（RELATIVE SIZING）
~~~~~~~~~~~~

Cache tiering agent 能夠根據 cache 儲存池的相對大小對物件進行 flush 或者 evict。cache 儲存池包含了已修改（或者變質）物件達到一個比例時，cache tier agent 就把它們 flush 到儲存。用以下指令設定比例 ``cache_target_dirty_ratio`` ： ::

	ceph osd pool set {cachepool} cache_target_dirty_ratio {0.0..1.0}

例如，設定為 ``0.4`` 時，dirty 物件達到 cache 儲存池容量的 40% 就開始 flush： ::

	ceph osd pool set hot-storage cache_target_dirty_ratio 0.4

當 dirty 物件到達一定比例的容量時，在 flush dirty 物件時會有比較快的速度. 可以透過以下指令設定 ``cache_target_dirty_high_ratio``： ::

  ceph osd pool set {cachepool} cache_target_dirty_high_ratio {0.0..1.0}

例如，設定為 ``0.6`` 時，當 dirty 物件達到總容量的 60% 將開始 flush dirty 物件： ::

  ceph osd pool set hot-storage cache_target_dirty_high_ratio 0.6

當 cache 儲存池利用率達到總容量的一定比例時，cache tier agent 將 evict 部分物件來維持足過的空間。用以下指令來設定 ``cache_target_full_ratio`` ： ::

	ceph osd pool set {cachepool} cache_target_full_ratio {0.0..1.0}

例如，設定為 ``0.8`` 時，當 clean 物件達到總容量的 80% 就開始 evict cache 儲存池： ::

	ceph osd pool set hot-storage cache_target_full_ratio 0.8


絕對大小（ABSOLUTE SIZING）
~~~~~~~~~~~~

Cache tiering agent 能根據總 bytes 或者物件的數量來 flush 或者 evict 物件，可以使用下列指令指定最大的bytes： ::

	ceph osd pool set {cachepool} target_max_bytes {#bytes}

例如，設定一個達到 1TB 時，flush 或 evict 物件： ::

	ceph osd pool set hot-storage target_max_bytes 1000000000000


以下指令可以指定 cache 物件的最大數量： ::

	ceph osd pool set {cachepool} target_max_objects {#objects}

例如，設定一個當物件達到 1M 時，開始 flush 與 evict 物件： ::

	ceph osd pool set hot-storage target_max_objects 1000000

.. note:: 如果兩個都有設定，cache tiering agent 會按照先到的門檻值優先執行 flush 與 evict。


快取壽命（CACHE AGE）
--------

你可以規範 cache tiering agent 必須延遲多久時間，才能把某個已修改（變質）的物件 flush 回到 backing storage 儲存池： ::

	ceph osd pool set {cachepool} cache_min_flush_age {#seconds}

例如，讓一個已修改（變質）物件延遲 10 分鐘才 flush，可以執行此指令： ::

	ceph osd pool set hot-storage cache_min_flush_age 600

你也可以指定某物件，在 cache tier 放置多長時間才會被 evict： ::

	ceph osd pool {cache-tier} cache_min_evict_age {#seconds}

例如，設定 30 分鐘後才 evict 物件，可以執行此指令： ::

	ceph osd pool set hot-storage cache_min_evict_age 1800


移除分層快取
==========

移除 ``writeback`` 以及 ``read-only`` 模式的 cache tier 過程並不同。


移除一個 Read-Only 快取
------------

``read-only`` 沒有改變的資料，所以停用不會導致近期的任何資料遺失。

#. 首先把 cache tier mode 改為 ``none``，來停用。 ::

	ceph osd tier cache-mode {cachepool} none

   例如： ::

	ceph osd tier cache-mode hot-storage none

#. 然後刪除 backing storage 儲存池的 cache 儲存池。 ::

	ceph osd tier remove {storagepool} {cachepool}

   例如： ::

	ceph osd tier remove cold-storage hot-storage



移除一個 WriteBack 快取
------------

``writeback`` 模式可能包含變更的資料，所以在停用並刪除前，必須採取一些方式，以免遺失 cache 內最近改變的資料。


#. 首先把 cache 模式改成 ``forward`` ，這樣新的與更改過的物件，將直接 flush 到 backing storage 儲存池。 ::

	ceph osd tier cache-mode {cachepool} forward

   例如： ::

	ceph osd tier cache-mode hot-storage forward


#. 透過 ``rados`` 確認 cache 儲存池已 flush，這邊會等待一點時間： ::

	rados -p {cachepool} ls

   如果 cache 儲存池裡面還有物件，可以透過手動來 flush，例如： ::

	rados -p {cachepool} cache-flush-evict-all


#. 移除此 overlay，使 client 不再在被指到 cache 中。 ::

	ceph osd tier remove-overlay {storagetier}

   例如： ::

	ceph osd tier remove-overlay cold-storage


#. 最後，從 backing 儲存池中刪除 cache 儲存池。 ::

	ceph osd tier remove {storagepool} {cachepool}

   例如： ::

	ceph osd tier remove cold-storage hot-storage


.. _建立儲存池: ../pools#create-a-pool
.. _儲存池——調整儲存池: ../pools#set-pool-values
.. _放置不同的 OSD 於不同儲存池: ../crush-map/#placing-different-pools-on-different-osds
.. _Bloom filter: http://en.wikipedia.org/wiki/Bloom_filter
.. _CRUSH Map: ../crush-map