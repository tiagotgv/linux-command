arpd
===

收集免費ARP資訊

## 補充說明

**arpd命令** 是用來收集免費arp資訊的一個守護程序，它將收集到的資訊儲存在磁碟上或者在需要時，提供給核心使用者用於避免多餘廣播。

### 語法

```
arpd(選項)(參數)
```

### 選項

```
-l：將arp資料庫輸出到標準輸出裝置顯示並退出；
-f：指定讀取和載入arpd資料庫的文字檔案，檔案的格式與“-l”輸出資訊類似；
-b：指定arpd資料庫檔案，預設的位置為“/var/lib/arpd.db”；
-a：指定目標被認為死掉前查詢的次數；
-k：禁止通過核心傳送廣播查詢；
-n：設定緩衝失效時間。
```

### 參數

網路介面：指定網路介面。

### 例項

啟動arpd程序：

```
arpd -b /var/tmp/arpd.db
```

執行一段時間後，檢視結果：

```
arpd -l -b /var/tmp/arpd.db
```


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->