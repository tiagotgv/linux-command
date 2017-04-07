apt-get
===

Debian Linux發行版中的APT軟體包管理工具

## 補充說明

**apt-get命令** 是Debian Linux發行版中的APT軟體包管理工具。所有基於Debian的發行都使用這個包管理系統。deb包可以把一個應用的檔案包在一起，大體就如同Windows上的安裝檔案。

### 語法

```
apt-get(選項)(參數)
```

### 選項

```
-c：指定配置檔案。
```

### 參數

*   管理指令：對APT軟體包的管理操作；
*   軟體包：指定要操縱的軟體包。

### 例項

使用apt-get命令的第一步就是引入必需的軟體庫，Debian的軟體庫也就是所有Debian軟體包的集合，它們存在網際網路上的一些公共站點上。把它們的地址加入，apt-get就能搜尋到我們想要的軟體。/etc/apt/sources.list是存放這些地址列表的配置檔案，其格式如下：

```
deb web或[ftp地址] [發行版名字] main/contrib/non-[free]
```

我們常用的Ubuntu就是一個基於Debian的發行，我們使用apt-get命令獲取這個列表，以下是我整理的常用命令：

在修改`/etc/apt/sources.list`或者`/etc/apt/preferences`之後執行該命令。此外您需要定期執行這一命令以確保您的軟體包列表是最新的：

```
apt-get update
```

安裝一個新軟體包：

```
apt-get install packagename
```

解除安裝一個已安裝的軟體包（保留配置檔案）：

```
apt-get remove packagename
```

解除安裝一個已安裝的軟體包（刪除配置檔案）：

```
apt-get –purge remove packagename
```

會把已裝或已卸的軟體都備份在硬碟上，所以如果需要空間的話，可以讓這個命令來刪除你已經刪掉的軟體：

```
apt-get autoclean apt
```

這個命令會把安裝的軟體的備份也刪除，不過這樣不會影響軟體的使用的：

```
apt-get clean
```

更新所有已安裝的軟體包：

```
apt-get upgrade
```

將系統升級到新版本：

```
apt-get dist-upgrade
```

定期執行這個命令來清除那些已經解除安裝的軟體包的.deb檔案。通過這種方式，您可以釋放大量的磁碟空間。如果您的需求十分迫切，可以使用`apt-get clean`以釋放更多空間。這個命令會將已安裝軟體包裹的.deb檔案一併刪除。大多數情況下您不會再用到這些.debs檔案，因此如果您為磁碟空間不足 而感到焦頭爛額，這個辦法也許值得一試：

```
apt-get autoclean
```


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->
