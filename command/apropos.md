apropos
===

在 whatis 資料庫中查詢字元串

## 補充說明

**apropos命令** 在一些特定的包含系統命令的簡短描述的資料庫檔案裡查詢關鍵字，然後把結果送到標準輸出。 

如果你不知道完成某個特定任務所需要命令的名稱，可以使用一個關鍵字通過Linux apropos實用程式來搜尋它。該實用程式可以搜尋關鍵字並且顯示所有包含匹配項的man頁面的簡短描述。另外，使用man實用程式和-k（關鍵字）選項，可以得到和用Linux apropos實用程式相同的結果（實際上是相同的命令）。

### 語法

```
apropos [-dalhvV] -e|-[w|-r] [-s section] [-m system[,...]] [-M path] [-L locale] -C [file] keyword ...
```

### 選項

```
-d, --debug：輸出偵錯資訊。
-v, --verbose：輸出詳細的警告資訊。
-r, -- regex：將每個keyword作為正規表示式解釋。這是預設行為。每個keyword將匹配手冊頁和描述。
-w, --wildcard：將每個keyword作為shell樣式的通配符解釋。
-e, --exact：每個keyword將精確匹配手冊頁名字和描述。
-a, --and：只顯示匹配所有keyword的手冊頁和描述。預設顯示匹配任何keyword的項。
-l, --long：不根據終端寬度縮減輸出。
-s section, --section section：只查詢指定的手冊section。
-m system[,...], --systems=system[,...]：用於查詢其它作業系統的手冊頁。
-M path, --manpath=path：指定從其它以冒號分隔的手冊頁層次查詢。預設使用$MANPATH環境變數。這個選項覆蓋$MANPATH的內容。
-L locale, --locale=locale：apropos呼叫C函數setlocale來得到當前本地化資訊，包括$LC_MESSAGE和$LANG。使用該選項提供一個locale字元串來臨時更改本地化資訊。
-C file, --config-file=file：使用這個使用者配置檔案而不是預設的~/.manpath。
-h, --help：列印幫助資訊並退出。
-V, --version：列印版本資訊並退出。
```

### 返回值

返回0表示成功，1表示用法、語法或配置檔案錯誤，2表示操作錯誤，16表示沒有找到匹配的內容。

### 例項

```
[root@localhost ~]# man -k who
at.allow [at]        (5)  - determine who can submit jobs via at or batch
at.deny [at]         (5)  - determine who can submit jobs via at or batch
jwhois               (1)  - client for the whois service
jwhois              (rpm) - Internet whois/nicname client.
Net::LDAP::Extension::whoami (3pm)  - LDAP Who am I? Operation
w                    (1)  - Show who is logged on and what they are doing
who                  (1p)  - display who is on the system
who                  (1)  - show who is logged on
whoami               (1)  - print effective userid

[root@localhost ~]# apropos who
at.allow [at]        (5)  - determine who can submit jobs via at or batch
at.deny [at]         (5)  - determine who can submit jobs via at or batch
jwhois               (1)  - client for the whois service
jwhois              (rpm) - Internet whois/nicname client.
Net::LDAP::Extension::WhoAmI (3pm)  - LDAP Who am I? Operation
w                    (1)  - Show who is logged on and what they are doing
who                  (1p)  - display who is on the system
who                  (1)  - show who is logged on
whoami               (1)  - print effective userid
```

查詢手冊頁名字和描述中包含emacs和vi的手冊頁：

```
apropos -a emacs vi
```


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->
