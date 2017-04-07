aptitude
===

Debian Linux系統中軟體包管理工具

## 補充說明

**aptitude命令** 與apt-get命令一樣，都是Debian Linux及其衍生系統中功能極其強大的包管理工具。與apt-get不同的是，aptitude在處理依賴問題上更佳一些。舉例來說，aptitude在刪除一個包時，會同時刪除本身所依賴的包。這樣，系統中不會殘留無用的包，整個系統更為乾淨。它通過文字操作選單和命令兩種方式管理軟體包。

### 語法

```
aptitude(選項)(參數)
```

### 選項

```
-h：顯示幫助資訊；
-d：僅下載軟體包，不執行安裝操作；
-P：每一步操作都要求確認；
-y：所有問題都回答“yes”；
-v：顯示附加資訊；
-u：啟動時下載新的軟體包列表。
```

### 參數

操作命令：使用者管理軟體包的操作命令。

### 例項

以下是我總結的一些常用aptitude命令，僅供參考：

```
aptitude update            #更新可用的包列表
aptitude upgrade           #升級可用的包
aptitude dist-upgrade      #將系統升級到新的發行版
aptitude install pkgname   #安裝包
aptitude remove pkgname    #刪除包
aptitude purge pkgname     #刪除包及其配置檔案
aptitude search string     #搜尋包
aptitude show pkgname      #顯示包的詳細資訊
aptitude clean             #刪除下載的包檔案
aptitude autoclean         #僅刪除過期的包檔案
```

當然，你也可以在文字介面模式中使用 aptitude。


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->
