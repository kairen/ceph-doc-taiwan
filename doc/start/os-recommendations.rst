==============
 推薦作業系統
==============

Ceph 相關內容
=========

照一般來說，我們建議在比較新的 Linux 發步版本上部署 Ceph；同樣，要選擇長期支援（LTS）的版本。


Linux 核心
----------

- **Ceph 核心客戶端**

  我們目前推薦：

  - 4.1.4 or later
  - 3.16.3 or later (rbd deadlock regression in 3.16.[0-2])
  - *NOT* 3.15.* (rbd deadlock regression)
  - 3.14.*

  如果您堅持使用很舊的版本，可以考慮這些：

  - 3.10.*

  Firefly (CRUSH_TUNABLES3) 這個版本的可調整參數要到 v3.15 才開始支援。詳細請看 \
  `CRUSH 可調參數`_ 。

- **B-tree File System (Btrfs)**

  如果您想在 ``btrfs`` 上執行的 Ceph，我們推薦使用一個最新的 Linux 核心（v3.14 or later）。

系統平台
========

下面的表格顯示 Ceph 的需求與各種 Linux 發佈版本的對應關係。一般來說 Ceph 對核\
心和系統初始化階段的依賴很少（如 sysvinit、upstart、systemd）。


Infernalis (9.1.0)
--------------

+----------+----------+--------------------+--------------+---------+------------+
| Distro   | Release  | Code Name          | Kernel       | Notes   | Testing    | 
+==========+==========+====================+==============+=========+============+
| CentOS   | 7        | N/A                | linux-3.10.0 |         | B, I, C    |
+----------+----------+--------------------+--------------+---------+------------+
| Debian   | 8.0      | Jessie             | linux-3.16.0 | 1, 2    | B, I       |
+----------+----------+--------------------+--------------+---------+------------+
| Fedora   | 22       | N/A                | linux-3.14.0 |         | B, I       |
+----------+----------+--------------------+--------------+---------+------------+
| RHEL     | 7        | Maipo              | linux-3.10.0 |         | B, I       |
+----------+----------+--------------------+--------------+---------+------------+
| Ubuntu   | 14.04    | Trusty Tahr        | linux-3.13.0 |         | B, I, C    |
+----------+----------+--------------------+--------------+---------+------------+

Hammer (0.94)
-------------

+----------+----------+--------------------+--------------+---------+------------+
| Distro   | Release  | Code Name          | Kernel       | Notes   | Testing    | 
+==========+==========+====================+==============+=========+============+
| CentOS   | 6        | N/A                | linux-2.6.32 | 1, 2    |            |
+----------+----------+--------------------+--------------+---------+------------+
| CentOS   | 7        | N/A                | linux-3.10.0 |         | B, I, C    |
+----------+----------+--------------------+--------------+---------+------------+
| Debian   | 7.0      | Wheezy             | linux-3.2.0  | 1, 2    |            |
+----------+----------+--------------------+--------------+---------+------------+
| Ubuntu   | 12.04    | Precise Pangolin   | linux-3.2.0  | 1, 2    |            |
+----------+----------+--------------------+--------------+---------+------------+
| Ubuntu   | 14.04    | Trusty Tahr        | linux-3.13.0 |         | B, I, C    |
+----------+----------+--------------------+--------------+---------+------------+

Firefly (0.80)
--------------

+----------+----------+--------------------+--------------+---------+------------+
| Distro   | Release  | Code Name          | Kernel       | Notes   | Testing    | 
+==========+==========+====================+==============+=========+============+
| CentOS   | 6        | N/A                | linux-2.6.32 | 1, 2    | B, I       |
+----------+----------+--------------------+--------------+---------+------------+
| CentOS   | 7        | N/A                | linux-3.10.0 |         | B          |
+----------+----------+--------------------+--------------+---------+------------+
| Debian   | 6.0      | Squeeze            | linux-2.6.32 | 1, 2, 3 | B          |
+----------+----------+--------------------+--------------+---------+------------+
| Debian   | 7.0      | Wheezy             | linux-3.2.0  | 1, 2    | B          |
+----------+----------+--------------------+--------------+---------+------------+
| Fedora   | 19       | Schrödinger's Cat  | linux-3.10.0 |         | B          |
+----------+----------+--------------------+--------------+---------+------------+
| Fedora   | 20       | Heisenbug          | linux-3.14.0 |         | B          |
+----------+----------+--------------------+--------------+---------+------------+
| RHEL     | 6        | Santiago           | linux-2.6.32 | 1, 2    | B, I, C    |
+----------+----------+--------------------+--------------+---------+------------+
| RHEL     | 7        | Maipo              | linux-3.10.0 |         | B, I, C    |
+----------+----------+--------------------+--------------+---------+------------+
| Ubuntu   | 12.04    | Precise Pangolin   | linux-3.2.0  | 1, 2    | B, I, C    |
+----------+----------+--------------------+--------------+---------+------------+
| Ubuntu   | 14.04    | Trusty Tahr        | linux-3.13.0 |         | B, I, C    |
+----------+----------+--------------------+--------------+---------+------------+

注意事項
----

- **1**: 預設核心 ``btrfs`` 版本較舊，不推薦用於 ``ceph-osd`` 儲存節點；要升級到推薦的核心，或者改用 ``xfs`` 、 ``ext4``。

- **2**: 預設核心內建的 Ceph 客戶端較舊，不推薦做核心空間客戶端（核心 RBD 或 Ceph 檔案系統），請升級到推薦核心。

- **3**: 預設核心或已安裝的 ``glibc`` 版本若不支援 ``syncfs(2)`` 系統調用，同一台機器上使用 ``xfs`` 或 ``ext4`` 的 ``ceph-osd`` 背景行程效能不如預期。


測試版本
------

- **B**: 我們會為此平台構建發布套件。對其中的某些平台，可能也會持續地編譯所有分支、做基本單元測試。

- **I**: 我們在這個平台上做基本的安裝和功能測試。

- **C**: 我們在這個平台上持續地做全面的功能、迴歸與壓力測試，包括開發分支、預先發佈版本、正式發佈版本。

.. _CRUSH 可調參數: ../../rados/operations/crush-map#tunables
