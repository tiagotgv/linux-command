arping
===

通過傳送ARP協議報文測試網路

## 補充說明

**arping命令** 是用於傳送arp請求到一個相鄰主機的工具，arping使用arp資料包，通過ping命令檢查裝置上的硬體地址。能夠測試一個ip地址是否是在網路上已經被使用，並能夠獲取更多裝置資訊。功能類似於ping。

### 語法

```
arping(選項)(參數)
```

### 選項

```
-b：用於傳送乙太網廣播幀（FFFFFFFFFFFF）。arping一開始使用廣播地址，在收到響應後就使用unicast地址。
-q：quiet output不顯示任何資訊；
-f：表示在收到第一個響應報文後就退出；
-w timeout：設定一個超時時間，單位是秒。如果到了指定時間，arping還沒到完全收到響應則退出；
-c count：表示傳送指定數量的ARP請求資料包後就停止。如果指定了deadline選項，則arping會等待相同數量的arp響應包，直到超時為止；
-s source：設定arping傳送的arp資料包中的SPA欄位的值。如果為空，則按下面處理，如果是DAD模式（衝突地址探測），則設定為0.0.0.0，如果是Unsolicited ARP模式（Gratutious ARP）則設定為目標地址，否則從路由表得出；
-I interface：設定ping使用的網路介面。
```

### 參數

目的主機：指定傳送ARP報文的目的主機。

### 例項

```
[root@localhost ~]# arping www.baidu.com
ARPING 220.181.111.147 from 173.231.43.132 eth0
Unicast reply from 220.181.111.147 00:D0:03:[bc:48:00]  1.666ms
Unicast reply from 220.181.111.147 [00:D0:03:BC:48:00]  1.677ms
Unicast reply from 220.181.111.147 [00:D0:03:BC:48:00]  1.691ms
Unicast reply from 220.181.111.147 [00:D0:03:BC:48:00]  1.728ms
Unicast reply from 220.181.111.147 [00:D0:03:BC:48:00]  1.626ms
Unicast reply from 220.181.111.147 [00:D0:03:BC:48:00]  1.292ms
Unicast reply from 220.181.111.147 [00:D0:03:BC:48:00]  1.429ms
Unicast reply from 220.181.111.147 [00:D0:03:BC:48:00]  2.042ms
Sent 8 probes (1 broadcast(s))
Received 8 response(s)
```


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->
