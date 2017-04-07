at
===

在指定時間執行一個任務

## 補充說明

**at命令** 用於在指定時間執行命令。at允許使用一套相當複雜的指定時間的方法。它能夠接受在當天的hh:mm（小時:分鐘）式的時間指定。假如該時間已過去，那麼就放在第二天執行。當然也能夠使用midnight（深夜），noon（中午），teatime（飲茶時間，一般是下午4點）等比較模糊的 詞語來指定時間。使用者還能夠採用12小時計時制，即在時間後面加上AM（上午）或PM（下午）來說明是上午還是下午。 也能夠指定命令執行的具體日期，指定格式為month day（月 日）或mm/dd/yy（月/日/年）或dd.mm.yy（日.月.年）。指定的日期必須跟在指定時間的後面。

上面介紹的都是絕對計時法，其實還能夠使用相對計時法，這對於安排不久就要執行的命令是很有好處的。指定格式為：`now + count time-units`，now就是當前時間，time-units是時間單位，這裡能夠是minutes（分鐘）、hours（小時）、days（天）、weeks（星期）。count是時間的數量，究竟是幾天，還是幾小時，等等。 更有一種計時方法就是直接使用today（今天）、tomorrow（明天）來指定完成命令的時間。

### 語法

```
at(選項)(參數)
```

### 選項

```
-f：指定包含具體指令的任務檔案；
-q：指定新任務的佇列名稱；
-l：顯示待執行任務的列表；
-d：刪除指定的待執行任務；
-m：任務執行完成後向使用者傳送E-mail。
```

### 參數

日期時間：指定任務執行的日期時間。

### 例項

三天後的下午 5 點鍾執行`/bin/ls`：

```
[root@localhost ~]# at 5pm+3 days
at> /bin/ls
at> <EOT>
job 7 at 2013-01-08 17:00
```

明天17點鐘，輸出時間到指定檔案內：

```
[root@localhost ~]# at 17:20 tomorrow
at> date >/root/2013.log
at> <EOT>
job 8 at 2013-01-06 17:20
```

計劃任務設定後，在沒有執行之前我們可以用atq命令來檢視系統沒有執行工作任務：

```
[root@localhost ~]# atq
8       2013-01-06 17:20 a root
7       2013-01-08 17:00 a root
```

刪除已經設定的任務：

```
[root@localhost ~]# atq
8       2013-01-06 17:20 a root
7       2013-01-08 17:00 a root

[root@localhost ~]# atrm 7
[root@localhost ~]# atq
8       2013-01-06 17:20 a root
```

顯示已經設定的任務內容：

```
[root@localhost ~]# at -c 8
#!/bin/sh
# atrun uid=0 gid=0
# mail     root 0
umask 22此處省略n個字元
date >/root/2013.log
```


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->
