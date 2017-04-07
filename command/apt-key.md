apt-key
===

管理Debian Linux系統中的軟體包金鑰

## 補充說明

**apt-key命令** 用於管理Debian Linux系統中的軟體包金鑰。每個釋出的deb包，都是通過金鑰認證的，apt-key用來管理金鑰。

### 語法

```
apt-key(參數)
```

### 參數

操作指令：APT金鑰操作指令。

### 例項

```
apt-key list          #列出已儲存在系統中key。
apt-key add keyname   #把下載的key新增到本地trusted資料庫中。
apt-key del keyname   #從本地trusted資料庫刪除key。
apt-key update        #更新本地trusted資料庫，刪除過期沒用的key。
```


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->
