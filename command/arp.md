arp
===

顯示和修改IP到MAC轉換表

## 補充說明

**arp命令** 用於操作主機的arp緩衝區，它可以顯示arp緩衝區中的所有條目、刪除指定的條目或者新增靜態的ip地址與MAC地址對應關係。

### 語法

```
arp(選項)(參數)
```

### 選項

```
-a<主機>：顯示arp緩衝區的所有條目；
-H<地址類型>：指定arp指令使用的地址類型；
-d<主機>：從arp緩衝區中刪除指定主機的arp條目；
-D：使用指定介面的硬體地址；
-e：以Linux的顯示風格顯示arp緩衝區中的條目；
-i<介面>：指定要操作arp緩衝區的網路介面；
-s<主機><MAC地址>：設定指定的主機的IP地址與MAC地址的靜態對映；
-n：以數字方式顯示arp緩衝區中的條目；
-v：顯示詳細的arp緩衝區條目，包括緩衝區條目的統計資訊；
-f<檔案>：設定主機的IP地址與MAC地址的靜態對映。
```

### 參數

主機：查詢arp緩衝區中指定主機的arp條目。

### 例項

```
[root@localhost ~]# arp -v
Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.0.134            ether   00:21:5E:C7:4D:88   C                     eth1
115.238.144.129          ether   38:22:D6:2F:B2:F1   C                     eth0
Entries: 2      Skipped: 0      Found: 2
```


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->
