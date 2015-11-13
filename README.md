# Ceph 中文翻譯
本文件主要是從```drunkard/ceph-Chinese-doc```進行繁體中文翻譯，並針對新版本做更新與修正為翻譯部分。

### 參與貢獻
1. 在 ```Github``` 上 ```fork``` 到自己的 Repository，例如：```<User>/ceph-doc-taiwan.git```，然後 ```clone```到 local 端，並設定 Git 使用者資訊。

 ```sh
git clone https://github.com/kairen/ceph-doc-taiwan.git
cd ceph-doc-taiwan
git config user.name "User"
git config user.email user@email.com
```
2. 修改程式碼或頁面後，透過 ```commit``` 來提交到自己的 Repository：

 ```sh
git commit -am "Fix issue #1: change helo to hello"
git push
```
> 若新增採用一般文字訊息，如```Add ceph block device intro...```。

3. 在 GitHub 上提交一個 Pull Request。
4. 持續的針對 Project Repository 進行更新內容：

 ```sh
 git remote add upstream  https://github.com/kairen/ceph-doc-taiwan.git
 git fetch upstream
 git checkout master
 git rebase upstream/master
 git push -f origin master
 ```

### 編譯
編譯好的網站位於： https://github.com/drunkard/ceph-readable-doc/tree/master/output/html
編譯環境的配置會比較麻煩，若不需要自己編譯可以直接閱讀上面連結。


Java 虛擬機應該用 ```oracle-jdk-bin-1.8``` 或 ```oracle-jdk-bin-1.7`` ，使用 jre 會缺少必要的lib檔案。

### 問題
* 編譯系統是基於 ```python2```，不支援```python3```，會有語法錯誤問題，在```Gentoo/Funtoo``` 下如果環境錯誤，可以用以下方式臨時切換到 ```python2.7```：
```sh
eselect python set python2.7
ln -sf /usr/bin/python2.7 /usr/bin/python
```

* ditaa 圖片不不能翻譯為中文，因為渲染時的字體問題還沒有解決；

### 編譯步驟
這些檔案是從```Ceph```原始碼中的```doc/```目錄翻譯而來，其結構未變，所以您仍然可以用原始文件
的構建方法構建此文件。只需用此repository的```ceph-doc```目錄替換ceph repository的```doc```目錄，具體步驟如下：

* 首先```git clone git://github.com/drunkard/docs_zh.git```，然後 ```git clone git://github.com/ceph/ceph.git```。
* 然後把Ceph原始碼裡的```doc```目錄備份為```doc-orig```，然後把這個Repository的```ceph-doc ```目錄連結為原Ceph Repository的```doc```。
* 執 行Ceph Repository 裡面的admin目錄下的```build-doc```開始建立文件。
* 執行```serve-doc```，就可以透過 [HTTP](http://localhost:8080/) 方式閱讀文件了。


### 文件編寫風格
要盡量遵守原文件的書寫風格，如縮排、不超過80 列寬、等。有些風格是中文版特有的，諸如：

* 中文和非中文用空格分隔
* 插入指令，前一段行尾加空格和雙引號（ ' ::' ）
* 引用和正文間加""，這樣不影響編譯系統又不會額外增加空格
* 用反斜線"\"換行，這樣不會額外增加空格
* 原文中用雙引號引用的詞語在中文文件裡可去掉引號，因為原文引用它已經很顯眼了
* 由於字體和程式的問題，在"Ternimal"下能更好地對齊到80列

### 更新方式
UPDATE 文件內容是```doc/```目錄下的最新commit，說明本次更新到了這裡。之後的更新可以對照原文和```doc/```目錄下的commit歷史，不用一個一個對照的繁瑣。

獲取此 commit ID 的指令為：
```sh
git log -1 --pretty=oneline doc/ | cut -d ' ' -f 1
```
查看```doc/```目錄自上次更新以來的變更：
```sh
CEPH_REPO=/git/ceph
ID=`cat UPDATETO` && cd $CEPH_REPO && gitview ${ID}.. doc/ &
```