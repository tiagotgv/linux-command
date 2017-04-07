atq
===

列出當前使用者的at任務列表

## 補充說明

**atq命令** 顯示系統中待執行的任務列表，也就是列出當前使用者的at任務列表。

### 語法

```
atq(選項)
```

### 選項

```
-V：顯示版本號；
-q：查詢指定佇列的任務。
```

### 例項

```
at now + 10 minutes
at> echo 1111
at> <eot>
job 3 at Fri Apr 26 12:56:00 2013

atq
3       Fri Apr 26 12:56:00 2013 a root
```


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->
