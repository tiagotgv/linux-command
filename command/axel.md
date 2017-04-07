axel
===

多執行緒下載工具

## 補充說明

**axel** 是Linux下一個不錯的HTTP/ftp高速下載工具。支援多執行緒下載、斷點續傳，且可以從多個地址或者從一個地址的多個連線來下載同一個檔案。適合網速不給力時多執行緒下載提高下載速度。比如在國內VPS或伺服器上下載lnmp一鍵安裝包用Axel就比wget快。

### 安裝

CentOS安裝Axel：

目前yum源上沒有Axel，我們可以到http://pkgs.repoforge.org/axel/下載rpm包安裝。

32位CentOS執行下面命令：

```
wget -c http://pkgs.repoforge.org/axel/axel-2.4-1.el5.rf.i386.rpm
rpm -ivh axel-2.4-1.el5.rf.i386.rpm
```

64位CentOS執行下面命令：

```
wget -c http://pkgs.repoforge.org/axel/axel-2.4-1.el5.rf.x86_64.rpm
rpm -ivh axel-2.4-1.el5.rf.x86_64.rpm
```

Debian/Ubuntu安裝Axel：

```
apt-get install axel
```

### 語法

```
axel [options] url1 [url2] [url...]
```

### 選項

```
--max-speed=x , -s x         最高速度x
--num-connections=x , -n x   連線數x
--output=f , -o f            下載為本地檔案f
--search[=x] , -S [x]        搜尋映象
--header=x , -H x            新增標頭檔案字元串x（指定 HTTP header）
--user-agent=x , -U x        設定使用者代理（指定 HTTP user agent）
--no-proxy ， -N             不使用代理伺服器
--quiet ， -q                靜默模式
--verbose ，-v               更多狀態資訊
--alternate ， -a            Alternate progress indicator
--help ，-h                  幫助
--version ，-V               版本資訊
```

### 例項

如下載lnmp安裝包指定10個執行緒，存到/tmp/：

```
axel -n 10 -o /tmp/ http://www.jsdig.com/lnmp.tar.gz
```

如果下載過程中下載中斷可以再執行下載命令即可恢復上次的下載進度。


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->
