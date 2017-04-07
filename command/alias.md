alias
===

用來設定指令的別名

## 補充說明

**alias命令** 用來設定指令的別名。我們可以使用該命令可以將一些較長的命令進行簡化。使用alias時，使用者必須使用單引號`''`將原來的命令引起來，防止特殊字元導致錯誤。

alias命令的作用只侷限於該次登入的操作。若要每次登入都能夠使用這些命令別名，則可將相應的alias命令存放到bash的初始化檔案`/etc/bashrc`中。

### 語法

```
alias(選項)(參數)
```

### 選項

```
-p：列印已經設定的命令別名。
```

### 參數

命令別名設定：定義命令別名，格式為“命令別名=‘實際命令’”。

### 例項

 **alias 的基本使用方法為：**

```
alias 新的命令='原命令 -選項/參數'
```

例如：`alias l=‘ls -lsh'`將重新定義ls命令，現在只需輸入l就可以列目錄了。直接輸入 alias 命令會列出當前系統中所有已經定義的命令別名。

要刪除一個別名，可以使用 unalias 命令，如 unalias l。

 **檢視系統已經設定的別名：**

```
alias -p
alias cp='cp -i'
alias l.='ls -d .* --color=tty'
alias ll='ls -l --color=tty'
alias ls='ls --color=tty'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
```


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->
