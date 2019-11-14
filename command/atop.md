atop
===

監控Linux系統資源與程序的工具

## 補充說明

[非內部程式，需要安裝]它以一定的頻率記錄系統的執行狀態，所採集的資料包含系統資源(CPU、記憶體、磁碟和網路)使用情況和程序執行情況，並能以日誌檔案的方式儲存在磁碟中，伺服器出現問題後，我們可獲取相應的atop日誌檔案進行分析。atop是一款開源軟體，我們可以從這裡獲得其源碼和rpm安裝包。

## 語法

```
atop(選項)(參數)
```

## 說明

**ATOP列**：該列顯示了主機名、資訊取樣日期和時間點

**PRC列**：該列顯示程序整體執行情況

- sys、usr欄位分別指示程序在核心態和使用者態的執行時間
- #proc欄位指示程序總數
- #zombie欄位指示僵死程序的數量
- #exit欄位指示atop取樣週期期間退出的程序數量


**CPU列**：該列顯示CPU整體(即多核CPU作為一個整體CPU資源)的使用情況，我們知道CPU可被用於執行程序、處理中斷，也可處於空閒狀態(空閒狀態分兩種，一種是活動程序等待磁碟IO導致CPU空閒，另一種是完全空閒)

- sys、usr欄位指示CPU被用於處理程序時，程序在核心態、使用者態所佔CPU的時間比例
- irq欄位指示CPU被用於處理中斷的時間比例
- idle欄位指示CPU處在完全空閒狀態的時間比例
- wait欄位指示CPU處在“程序等待磁碟IO導致CPU空閒”狀態的時間比例

CPU列各個欄位指示值相加結果為N00%，其中N為cpu核數。

cpu列：該列顯示某一核cpu的使用情況，各欄位含義可參照CPU列，各欄位值相加結果為100%

**CPL列**：該列顯示CPU負載情況

- avg1、avg5和avg15欄位：過去1分鐘、5分鐘和15分鐘內執行佇列中的平均程序數量
- csw欄位指示上下文交換次數
- intr欄位指示中斷髮生次數

**MEM列**：該列指示記憶體的使用情況

- tot欄位指示實體記憶體總量
- free欄位指示空閒記憶體的大小
- cache欄位指示用於頁快取的記憶體大小
- buff欄位指示用於檔案快取的記憶體大小
- slab欄位指示系統核心佔用的記憶體大小

**SWP列**：該列指示交換空間的使用情況

- tot欄位指示交換區總量
- free欄位指示空閒交換空間大小

**PAG列**：該列指示虛擬記憶體分頁情況

swin、swout欄位：換入和換出記憶體頁數

**DSK列**：該列指示磁碟使用情況，每一個磁碟裝置對應一列，如果有sdb裝置，那麼增多一列DSK資訊

- sda欄位：磁碟裝置標識
- busy欄位：磁碟忙時比例
- read、write欄位：讀、寫請求數量

**NET列**：多列NET展示了網路狀況，包括傳輸層(TCP和UDP)、IP層以及各活動的網口資訊

- XXXi  欄位指示各層或活動網口收包數目
- XXXo 欄位指示各層或活動網口發包數目


## atop日誌

每個時間點取樣頁面組合起來就形成了一個atop日誌檔案，我們可以使用"atop -r XXX"命令對日誌檔案進行檢視。那以什麼形式儲存atop日誌檔案呢？

對於atop日誌檔案的儲存方式，我們可以這樣：

- 每天儲存一個atop日誌檔案，該日誌檔案記錄當天資訊
- 日誌檔案以"atop_YYYYMMDD"的方式命名
- 設定日誌失效期限，自動刪除一段時間前的日誌檔案

其實atop開發者已經提供了以上日誌儲存方式，相應的atop.daily指令碼可以在源碼目錄下找到。在atop.daily指令碼中，我們可以通過修改INTERVAL變數改變atop資訊取樣週期(預設為10分鐘)；通過修改以下命令中的數值改變日誌儲存天數(預設為28天)：

```
(sleep 3; find $LOGPATH -name 'atop_*' -mtime +28 -exec rm {} \; )&
```

最後，我們修改cron檔案，每天凌晨執行atop.daily指令碼：

```
0 0 * * * root /etc/cron.daily/atop.daily
```

## 相關資料

- [官方手冊](http://www.atoptool.nl/download/man_atop-1.pdf)

<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->