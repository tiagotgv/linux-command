awk
===

文字和資料進行處理的程式語言

## 補充說明

**awk** 是一種程式語言，用於在linux/unix下對文字和資料進行處理。資料可以來自標準輸入(stdin)、一個或多個檔案，或其它命令的輸出。它支援使用者自定義函數和動態正規表示式等先進功能，是linux/unix下的一個強大程式設計工具。它在命令列中使用，但更多是作為指令碼來使用。awk有很多內建的功能，比如陣列、函數等，這是它和C語言的相同之處，靈活性是awk最大的優勢。

## awk命令格式和選項

 **語法形式**

```
awk [options] 'script' var=value file(s)
awk [options] -f scriptfile var=value file(s)
```

 **常用命令選項**

*    **-F fs   ** fs指定輸入分隔符，fs可以是字元串或正規表示式，如-F:
*    **-v var=value   ** 賦值一個使用者定義變數，將外部變數傳遞給awk
*    **-f scripfile  ** 從指令碼檔案中讀取awk命令
*    **-m[fr] val   ** 對val值設定內在限制，-mf選項限制分配給val的最大塊數目；-mr選項限制記錄的最大數目。這兩個功能是Bell實驗室版awk的擴充套件功能，在標準awk中不適用。

## awk模式和操作

awk指令碼是由模式和操作組成的。

### 模式

模式可以是以下任意一個：

*   /正規表示式/：使用通配符的擴充套件集。
*   關係表示式：使用運算符進行操作，可以是字元串或數字的比較測試。
*   模式匹配表示式：用運算符`~`（匹配）和`~!`（不匹配）。
*   BEGIN語句塊、pattern語句塊、END語句塊：參見awk的工作原理

### 操作

操作由一個或多個命令、函數、表示式組成，之間由換行符或分號隔開，並位於大括號內，主要部分是：

*   變數或陣列賦值
*   輸出命令
*   內建函數
*   控制流語句

## awk指令碼基本結構

```
awk 'BEGIN{ print "start" } pattern{ commands } END{ print "end" }' file
```

一個awk指令碼通常由：BEGIN語句塊、能夠使用模式匹配的通用語句塊、END語句塊3部分組成，這三個部分是可選的。任意一個部分都可以不出現在指令碼中，指令碼通常是被 **單引號** 或 **雙引號** 中，例如：

```
awk 'BEGIN{ i=0 } { i++ } END{ print i }' filename
awk "BEGIN{ i=0 } { i++ } END{ print i }" filename
```

### awk的工作原理

```
awk 'BEGIN{ commands } pattern{ commands } END{ commands }'
```

*   第一步：執行`BEGIN{ commands }`語句塊中的語句；
*   第二步：從檔案或標準輸入(stdin)讀取一行，然後執行`pattern{ commands }`語句塊，它逐行掃描檔案，從第一行到最後一行重複這個過程，直到檔案全部被讀取完畢。
*   第三步：當讀至輸入流末尾時，執行`END{ commands }`語句塊。

 **BEGIN語句塊** 在awk開始從輸入流中讀取行 **之前** 被執行，這是一個可選的語句塊，比如變數初始化、列印輸出表格的表頭等語句通常可以寫在BEGIN語句塊中。

 **END語句塊** 在awk從輸入流中讀取完所有的行 **之後** 即被執行，比如列印所有行的分析結果這類資訊彙總都是在END語句塊中完成，它也是一個可選語句塊。

 **pattern語句塊** 中的通用命令是最重要的部分，它也是可選的。如果沒有提供pattern語句塊，則預設執行`{ print }`，即列印每一個讀取到的行，awk讀取的每一行都會執行該語句塊。

 **示例**

```
echo -e "A line 1nA line 2" | awk 'BEGIN{ print "Start" } { print } END{ print "End" }'
Start
A line 1
A line 2
End
```

當使用不帶參數的`print`時，它就列印當前行，當`print`的參數是以逗號進行分隔時，列印時則以空格作為定界符。在awk的print語句塊中雙引號是被當作拼接符使用，例如：

```
echo | awk '{ var1="v1"; var2="v2"; var3="v3"; print var1,var2,var3; }'
v1 v2 v3
```

雙引號拼接使用：

```
echo | awk '{ var1="v1"; var2="v2"; var3="v3"; print var1"="var2"="var3; }'
v1=v2=v3
```

{ }類似一個迴圈體，會對檔案中的每一行進行迭代，通常變數初始化語句（如：i=0）以及列印檔案頭部的語句放入BEGIN語句塊中，將列印的結果等語句放在END語句塊中。

## awk內建變數（預定義變數）

說明：[A][N][P][G]表示第一個支援變數的工具，[A]=awk、[N]=nawk、[P]=POSIXawk、[G]=gawk

```
 **$n**  當前記錄的第n個欄位，比如n為1表示第一個欄位，n為2表示第二個欄位。
 **$0**  這個變數包含執行過程中當前行的文字內容。
[N]  **ARGC**  命令列參數的數目。
[G]  **ARGIND**  命令列中當前檔案的位置（從0開始算）。
[N]  **ARGV**  包含命令列參數的陣列。
[G]  **CONVFMT**  數字轉換格式（預設值為%.6g）。
[P]  **ENVIRON**  環境變數關聯陣列。
[N]  **ERRNO**  最後一個系統錯誤的描述。
[G]  **FIELDWIDTHS**  欄位寬度列表（用空格鍵分隔）。
[A]  **FILENAME**  當前輸入檔案的名。
[P]  **FNR**  同NR，但相對於當前檔案。
[A]  **FS**  欄位分隔符（預設是任何空格）。
[G]  **IGNORECASE**  如果為真，則進行忽略大小寫的匹配。
[A]  **NF**  表示欄位數，在執行過程中對應於當前的欄位數。
[A]  **NR**  表示記錄數，在執行過程中對應於當前的行號。
[A]  **OFMT**  數字的輸出格式（預設值是%.6g）。
[A]  **OFS**  輸出欄位分隔符（預設值是一個空格）。
[A]  **ORS**  輸出記錄分隔符（預設值是一個換行符）。
[A]  **RS**  記錄分隔符（預設是一個換行符）。
[N]  **RSTART**  由match函數所匹配的字元串的第一個位置。
[N]  **RLENGTH**  由match函數所匹配的字元串的長度。
[N]  **SUBSEP**  陣列下標分隔符（預設值是34）。
```

 **示例**

```
echo -e "line1 f2 f3nline2 f4 f5nline3 f6 f7" | awk '{print "Line No:"NR", No of fields:"NF, "$0="$0, "$1="$1, "$2="$2, "$3="$3}'
Line No:1, No of fields:3 $0=line1 f2 f3 $1=line1 $2=f2 $3=f3
Line No:2, No of fields:3 $0=line2 f4 f5 $1=line2 $2=f4 $3=f5
Line No:3, No of fields:3 $0=line3 f6 f7 $1=line3 $2=f6 $3=f7
```

使用`print $NF`可以列印出一行中的最後一個欄位，使用`$(NF-1)`則是列印倒數第二個欄位，其他以此類推：

```
echo -e "line1 f2 f3n line2 f4 f5" | awk '{print $NF}'
f3
f5
```

```
echo -e "line1 f2 f3n line2 f4 f5" | awk '{print $(NF-1)}'
f2
f4

```

列印每一行的第二和第三個欄位：

```
awk '{ print $2,$3 }' filename
```

統計檔案中的行數：

```
awk 'END{ print NR }' filename
```

以上命令只使用了END語句塊，在讀入每一行的時，awk會將NR更新為對應的行號，當到達最後一行NR的值就是最後一行的行號，所以END語句塊中的NR就是檔案的行數。

一個每一行中第一個欄位值累加的例子：

```
seq 5 | awk 'BEGIN{ sum=0; print "總和：" } { print $1"+"; sum+=$1 } END{ print "等於"; print sum }'
總和：
1+
2+
3+
4+
5+
等於
15
```

## 將外部變數值傳遞給awk

藉助 **`-v`選項** ，可以將外部值（並非來自stdin）傳遞給awk：

```
VAR=10000
echo | awk -v VARIABLE=$VAR '{ print VARIABLE }'
```

另一種傳遞外部變數方法：

```
var1="aaa"
var2="bbb"
echo | awk '{ print v1,v2 }' v1=$var1 v2=$var2
```

當輸入來自於檔案時使用：

```
awk '{ print v1,v2 }' v1=$var1 v2=$var2 filename
```

以上方法中，變數之間用空格分隔作為awk的命令列參數跟隨在BEGIN、{}和END語句塊之後。

## awk運算與判斷

作為一種程式設計語言所應具有的特點之一，awk支援多種運算，這些運算與C語言提供的基本相同。awk還提供了一系列內建的運算函數（如log、sqr、cos、sin等）和一些用於對字元串進行操作（運算）的函數（如length、substr等等）。這些函數的引用大大的提高了awk的運算功能。作為對條件轉移指令的一部分，關係判斷是每種程式設計語言都具備的功能，awk也不例外，awk中允許進行多種測試，作為樣式匹配，還提供了模式匹配表示式~（匹配）和~!（不匹配）。作為對測試的一種擴充，awk也支援用邏輯運算符。

### 算術運算符

<table style="width: 500px;" summary="運算符">

<thead>

<tr>

<th>運算符</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>+ -</td>

<td>加，減</td>

</tr>

<tr>

<td>* / &</td>

<td>乘，除與求餘</td>

</tr>

<tr>

<td>+ - !</td>

<td>一元加，減和邏輯非</td>

</tr>

<tr>

<td>^ ***</td>

<td>求冪</td>

</tr>

<tr>

<td>++ --</td>

<td>增加或減少，作為字首或字尾</td>

</tr>

</tbody>

</table>

例：

```
awk 'BEGIN{a="b";print a++,++a;}'
0 2
```

注意：所有用作算術運算符進行操作，操作數自動轉為數值，所有非數值都變為0

### 賦值運算符

<table style="width: 500px;" summary="運算符">

<thead>

<tr>

<th>運算符</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>= += -= *= /= %= ^= **=</td>

<td>賦值語句</td>

</tr>

</tbody>

</table>

例：

```
a+=5; 等價於：a=a+5; 其它同類
```

### 邏輯運算符

<table style="width: 500px;" summary="運算符">

<thead>

<tr>

<th>運算符</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>||</td>

<td>邏輯或</td>

</tr>

<tr>

<td>&&</td>

<td>邏輯與</td>

</tr>

</tbody>

</table>

例：

```
awk 'BEGIN{a=1;b=2;print (a>5 && b<=2),(a>5 || b<=2);}'
0 1
```

### 正則運算符

<table style="width: 500px;" summary="運算符">

<thead>

<tr>

<th>運算符</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>~ ~!</td>

<td>匹配正規表示式和不匹配正規表示式</td>

</tr>

</tbody>

</table>

例：

```
awk 'BEGIN{a="100testa";if(a ~ /^100*/){print "ok";}}'
ok
```

### 關係運算符

<table style="width: 500px;" summary="運算符">

<thead>

<tr>

<th>運算符</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>< <= > >= != ==</td>

<td>關係運算符</td>

</tr>

</tbody>

</table>

例：

```
awk 'BEGIN{a=11;if(a >= 9){print "ok";}}'
ok
```

注意：> < 可以作為字元串比較，也可以用作數值比較，關鍵看操作數如果是字元串就會轉換為字元串比較。兩個都為數字才轉為數值比較。字元串比較：按照ASCII碼順序比較。

### 其它運算符

<table style="width: 500px;" summary="運算符">

<thead>

<tr>

<th>運算符</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>$</td>

<td>欄位引用</td>

</tr>

<tr>

<td>空格</td>

<td>字元串連線符</td>

</tr>

<tr>

<td>?:</td>

<td>C條件表示式</td>

</tr>

<tr>

<td>in</td>

<td>陣列中是否存在某鍵值</td>

</tr>

</tbody>

</table>

例：

```
awk 'BEGIN{a="b";print a=="b"?"ok":"err";}'
ok
```

```
awk 'BEGIN{a="b";arr[0]="b";arr[1]="c";print (a in arr);}'
0
```

```
awk 'BEGIN{a="b";arr[0]="b";arr["b"]="c";print (a in arr);}'
1
```

### 運算級優先順序表

!級別越高越優先
級別越高越優先

## awk高階輸入輸出

### 讀取下一條記錄

awk中`next`語句使用：在迴圈逐行匹配，如果遇到next，就會跳過當前行，直接忽略下面語句。而進行下一行匹配。net語句一般用於多行合併：

```
cat text.txt
a
b
c
d
e

awk 'NR%2==1{next}{print NR,$0;}' text.txt
2 b
4 d
```

當記錄行號除以2餘1，就跳過當前行。下面的`print NR,$0`也不會執行。下一行開始，程式有開始判斷`NR%2`值。這個時候記錄行號是`：2` ，就會執行下面語句塊：`'print NR,$0'`

分析發現需要將包含有“web”行進行跳過，然後需要將內容與下面行合併為一行：

```
cat text.txt
web01[192.168.2.100]
httpd            ok
tomcat               ok
sendmail               ok
web02[192.168.2.101]
httpd            ok
postfix               ok
web03[192.168.2.102]
mysqld            ok
httpd               ok
0
awk '/^web/{T=$0;next;}{print T":t"$0;}' test.txt
web01[192.168.2.100]:   httpd            ok
web01[192.168.2.100]:   tomcat               ok
web01[192.168.2.100]:   sendmail               ok
web02[192.168.2.101]:   httpd            ok
web02[192.168.2.101]:   postfix               ok
web03[192.168.2.102]:   mysqld            ok
web03[192.168.2.102]:   httpd               ok
```

### 簡單地讀取一條記錄

`awk getline`用法：輸出重定向需用到`getline函數`。getline從標準輸入、管道或者當前正在處理的檔案之外的其他輸入檔案獲得輸入。它負責從輸入獲得下一行的內容，並給NF,NR和FNR等內建變數賦值。如果得到一條記錄，getline函數返回1，如果到達檔案的末尾就返回0，如果出現錯誤，例如開啟檔案失敗，就返回-1。

getline語法：getline var，變數var包含了特定行的內容。

awk getline從整體上來說，用法說明：

*    **當其左右無重定向符`|`或`<`時：** getline作用於當前檔案，讀入當前檔案的第一行給其後跟的變數`var`或`$0`（無變數），應該注意到，由於awk在處理getline之前已經讀入了一行，所以getline得到的返回結果是隔行的。
*    **當其左右有重定向符`|`或`<`時：** getline則作用於定向輸入檔案，由於該檔案是剛開啟，並沒有被awk讀入一行，只是getline讀入，那麼getline返回的是該檔案的第一行，而不是隔行。

 **示例：**

執行linux的`date`命令，並通過管道輸出給`getline`，然後再把輸出賦值給自定義變數out，並列印它：

```
awk 'BEGIN{ "date" | getline out; print out }' test
```

執行shell的date命令，並通過管道輸出給getline，然後getline從管道中讀取並將輸入賦值給out，split函數把變數out轉化成陣列mon，然後列印陣列mon的第二個元素：

```
awk 'BEGIN{ "date" | getline out; split(out,mon); print mon[2] }' test
```

命令ls的輸出傳遞給geline作為輸入，迴圈使getline從ls的輸出中讀取一行，並把它列印到螢幕。這裡沒有輸入檔案，因為BEGIN塊在開啟輸入檔案前執行，所以可以忽略輸入檔案。

```
awk 'BEGIN{ while( "ls" | getline) print }'
```

### 關閉檔案

awk中允許在程式中關閉一個輸入或輸出檔案，方法是使用awk的close語句。

```
close("filename")
```

filename可以是getline開啟的檔案，也可以是stdin，包含檔名的變數或者getline使用的確切命令。或一個輸出檔案，可以是stdout，包含檔名的變數或使用管道的確切命令。

### 輸出到一個檔案

awk中允許用如下方式將結果輸出到一個檔案：

```
echo | awk '{printf("hello word!n") > "datafile"}'
或
echo | awk '{printf("hello word!n") >> "datafile"}'
```

## 設定欄位定界符

預設的欄位定界符是空格，可以使用`-F "定界符"`  明確指定一個定界符：

```
awk -F: '{ print $NF }' /etc/passwd
或
awk 'BEGIN{ FS=":" } { print $NF }' /etc/passwd
```

在`BEGIN語句塊`中則可以用`OFS=“定界符”`設定輸出欄位的定界符。

## 流程控制語句

在linux awk的while、do-while和for語句中允許使用break,continue語句來控制流程走向，也允許使用exit這樣的語句來退出。break中斷當前正在執行的迴圈並跳到迴圈外執行下一條語句。if 是流程選擇用法。awk中，流程控制語句，語法結構，與c語言類型。有了這些語句，其實很多shell程式都可以交給awk，而且效能是非常快的。下面是各個語句用法。

### 條件判斷語句

```
if(表示式)
  語句1
else
  語句2
```

格式中語句1可以是多個語句，為了方便判斷和閱讀，最好將多個語句用{}括起來。awk分枝結構允許巢狀，其格式為：

```
if(表示式)
  {語句1}
else if(表示式)
  {語句2}
else
  {語句3}
```

示例：

```
awk 'BEGIN{
test=100;
if(test>90){
  print "very good";
  }
  else if(test>60){
    print "good";
  }
  else{
    print "no pass";
  }
}'

very good
```

每條命令語句後面可以用`;` **分號** 結尾。

### 迴圈語句

#### while語句

```
while(表示式)
  {語句}
```

示例：

```
awk 'BEGIN{
test=100;
total=0;
while(i<=test){
  total+=i;
  i++;
}
print total;
}'
5050
```

#### for迴圈

for迴圈有兩種格式：

格式1：

```
for(變數 in 陣列)
  {語句}
```

示例：

```
awk 'BEGIN{
for(k in ENVIRON){
  print k"="ENVIRON[k];
}

}'
TERM=linux
G_BROKEN_FILENAMES=1
SHLVL=1
pwd=/root/text
...
logname=root
HOME=/root
SSH_CLIENT=192.168.1.21 53087 22
```

注：ENVIRON是awk常量，是子典型陣列。

格式2：

```
for(變數;條件;表示式)
  {語句}
```

示例：

```
awk 'BEGIN{
total=0;
for(i=0;i<=100;i++){
  total+=i;
}
print total;
}'
5050
```

#### do迴圈

```
do
{語句} while(條件)
```

例子：

```
awk 'BEGIN{
total=0;
i=0;
do {total+=i;i++;} while(i<=100)
  print total;
}'
5050
```

### 其他語句

*    **break**  當 break 語句用於 while 或 for 語句時，導致退出程式迴圈。
*    **continue**  當 continue 語句用於 while 或 for 語句時，使程式迴圈移動到下一個迭代。
*    **next**  能能夠導致讀入下一個輸入行，並返回到指令碼的頂部。這可以避免對當前輸入行執行其他的操作過程。
*    **exit**  語句使主輸入迴圈退出並將控制轉移到END,如果END存在的話。如果沒有定義END規則，或在END中應用exit語句，則終止指令碼的執行。

## 陣列應用

陣列是awk的靈魂，處理文字中最不能少的就是它的陣列處理。因為陣列索引（下標）可以是數字和字元串在awk中陣列叫做關聯陣列(associative arrays)。awk 中的陣列不必提前聲明，也不必聲明大小。陣列元素用0或空字元串來初始化，這根據上下文而定。

### 陣列的定義

數字做陣列索引（下標）：

```
Array[1]="sun"
Array[2]="kai"
```

字元串做陣列索引（下標）：

```
Array["first"]="www"
Array"[last"]="name"
Array["birth"]="1987"
```

使用中`print Array[1]`會列印出sun；使用`print Array[2]`會列印出kai；使用`print["birth"]`會得到1987。

 **讀取陣列的值**

```
{ for(item in array) {print array[item]}; }       #輸出的順序是隨機的
{ for(i=1;i<=len;i++) {print array[i]}; }         #Len是陣列的長度
```

### 陣列相關函數

 **得到陣列長度：**

```
awk 'BEGIN{info="it is a test";lens=split(info,tA," ");print length(tA),lens;}'
4 4
```

length返回字元串以及陣列長度，split進行分割字元串為陣列，也會返回分割得到陣列長度。

```
awk 'BEGIN{info="it is a test";split(info,tA," ");print asort(tA);}'
4
```

asort對陣列進行排序，返回陣列長度。

 **輸出陣列內容（無序，有序輸出）：**

```
awk 'BEGIN{info="it is a test";split(info,tA," ");for(k in tA){print k,tA[k];}}'
4 test
1 it
2 is
3 a
```

`for…in`輸出，因為陣列是關聯陣列，預設是無序的。所以通過`for…in`得到是無序的陣列。如果需要得到有序陣列，需要通過下標獲得。

```
awk 'BEGIN{info="it is a test";tlen=split(info,tA," ");for(k=1;k<=tlen;k++){print k,tA[k];}}'
1 it
2 is
3 a
4 test
```

注意：陣列下標是從1開始，與C陣列不一樣。

 **判斷鍵值存在以及刪除鍵值：**

```
#錯誤的判斷方法：
awk 'BEGIN{tB["a"]="a1";tB["b"]="b1";if(tB["c"]!="1"){print "no found";};for(k in tB){print k,tB[k];}}'
no found
a a1
b b1
c
```

以上出現奇怪問題，`tB[“c”]`沒有定義，但是迴圈時候，發現已經存在該鍵值，它的值為空，這裡需要注意，awk陣列是關聯陣列，只要通過陣列引用它的key，就會自動建立改序列。

```
#正確判斷方法：
awk 'BEGIN{tB["a"]="a1";tB["b"]="b1";if( "c" in tB){print "ok";};for(k in tB){print k,tB[k];}}'
a a1
b b1
```

`if(key in array)`通過這種方法判斷陣列中是否包含`key`鍵值。

```
#刪除鍵值：
[chengmo@localhost ~]$ awk 'BEGIN{tB["a"]="a1";tB["b"]="b1";delete tB["a"];for(k in tB){print k,tB[k];}}'
b b1
```

`delete array[key]`可以刪除，對應陣列`key`的，序列值。

### 二維、多維陣列使用

awk的多維陣列在本質上是一維陣列，更確切一點，awk在儲存上並不支援多維陣列。awk提供了邏輯上模擬二維陣列的訪問方式。例如，`array[2,4]=1`這樣的訪問是允許的。awk使用一個特殊的字元串`SUBSEP(�34)`作為分割欄位，在上面的例子中，關聯陣列array儲存的鍵值實際上是2�344。

類似一維陣列的成員測試，多維陣列可以使用`if ( (i,j) in array)`這樣的語法，但是下標必須放置在圓括號中。類似一維陣列的迴圈訪問，多維陣列使用`for ( item in array )`這樣的語法遍歷陣列。與一維陣列不同的是，多維陣列必須使用`split()`函數來訪問單獨的下標分量。

```
awk 'BEGIN{
for(i=1;i<=9;i++){
  for(j=1;j<=9;j++){
    tarr[i,j]=i*j; print i,"*",j,"=",tarr[i,j];
  }
}
}'
1 * 1 = 1
1 * 2 = 2
1 * 3 = 3
1 * 4 = 4
1 * 5 = 5
1 * 6 = 6
...
9 * 6 = 54
9 * 7 = 63
9 * 8 = 72
9 * 9 = 81
```

可以通過`array[k,k2]`引用獲得陣列內容。

另一種方法：

```
awk 'BEGIN{
for(i=1;i<=9;i++){
  for(j=1;j<=9;j++){
    tarr[i,j]=i*j;
  }
}
for(m in tarr){
  split(m,tarr2,SUBSEP); print tarr2[1],"*",tarr2[2],"=",tarr[m];
}
}'
```

## 內建函數

awk內建函數，主要分以下3種類似：算數函數、字元串函數、其它一般函數、時間函數。

### 算術函數

<table height="241" width="907">

<thead>

<tr>

<th>格式</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>atan2( y, x )</td>

<td>返回 y/x 的反正切。</td>

</tr>

<tr>

<td>cos( x )</td>

<td>返回 x 的餘弦；x 是弧度。</td>

</tr>

<tr>

<td>sin( x )</td>

<td>返回 x 的正弦；x 是弧度。</td>

</tr>

<tr>

<td>exp( x )</td>

<td>返回 x 冪函數。</td>

</tr>

<tr>

<td>log( x )</td>

<td>返回 x 的自然對數。</td>

</tr>

<tr>

<td>sqrt( x )</td>

<td>返回 x 平方根。</td>

</tr>

<tr>

<td>int( x )</td>

<td>返回 x 的截斷至整數的值。</td>

</tr>

<tr>

<td>rand( )</td>

<td>返回任意數字 n，其中 0 <= n < 1。</td>

</tr>

<tr>

<td>srand( [expr] )</td>

<td>將 rand 函數的種子值設定為 Expr 參數的值，或如果省略 Expr 參數則使用某天的時間。返回先前的種子值。</td>

</tr>

</tbody>

</table>

舉例說明：

```
awk 'BEGIN{OFMT="%.3f";fs=sin(1);fe=exp(10);fl=log(10);fi=int(3.1415);print fs,fe,fl,fi;}'
0.841 22026.466 2.303 3

```

OFMT 設定輸出資料格式是保留3位小數。

獲得隨機數：

```
awk 'BEGIN{srand();fr=int(100*rand());print fr;}'
78
awk 'BEGIN{srand();fr=int(100*rand());print fr;}'
31
awk 'BEGIN{srand();fr=int(100*rand());print fr;}'
41
```

### 字元串函數

<table width="100%">

<thead>

<tr>

<th>格式</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>gsub( Ere, Repl, [ In ] )</td>

<td>除了正規表示式所有具體值被替代這點，它和 sub 函數完全一樣地執行。</td>

</tr>

<tr>

<td>sub( Ere, Repl, [ In ] )</td>

<td>用 Repl 參數指定的字元串替換 In 參數指定的字元串中的由 Ere 參數指定的擴充套件正規表示式的第一個具體值。sub 函數返回替換的數量。出現在 Repl 參數指定的字元串中的 &（和符號）由 In 參數指定的與 Ere 參數的指定的擴充套件正規表示式匹配的字元串替換。如果未指定 In 參數，預設值是整個記錄（$0 記錄變數）。</td>

</tr>

<tr>

<td>index( String1, String2 )</td>

<td>在由 String1 參數指定的字元串（其中有出現 String2 指定的參數）中，返回位置，從 1 開始編號。如果 String2 參數不在 String1 參數中出現，則返回 0（零）。</td>

</tr>

<tr>

<td>length [(String)]</td>

<td>返回 String 參數指定的字元串的長度（字元形式）。如果未給出 String 參數，則返回整個記錄的長度（$0 記錄變數）。</td>

</tr>

<tr>

<td>blength [(String)]</td>

<td>返回 String 參數指定的字元串的長度（以位元組為單位）。如果未給出 String 參數，則返回整個記錄的長度（$0 記錄變數）。</td>

</tr>

<tr>

<td>substr( String, M, [ N ] )</td>

<td>返回具有 N 參數指定的字元數量子串。子串從 String 參數指定的字元串取得，其字元以 M 參數指定的位置開始。M 參數指定為將 String 參數中的第一個字元作為編號 1。如果未指定 N 參數，則子串的長度將是 M 參數指定的位置到 String 參數的末尾 的長度。</td>

</tr>

<tr>

<td>match( String, Ere )</td>

<td>在 String 參數指定的字元串（Ere 參數指定的擴充套件正規表示式出現在其中）中返回位置（字元形式），從 1 開始編號，或如果 Ere 參數不出現，則返回 0（零）。RSTART 特殊變數設定為返回值。RLENGTH 特殊變數設定為匹配的字元串的長度，或如果未找到任何匹配，則設定為 -1（負一）。</td>

</tr>

<tr>

<td>split( String, A, [Ere] )</td>

<td>將 String 參數指定的參數分割為陣列元素 A[1], A[2], . . ., A[n]，並返回 n 變數的值。此分隔可以通過 Ere 參數指定的擴充套件正規表示式進行，或用當前欄位分隔符（FS 特殊變數）來進行（如果沒有給出 Ere 參數）。除非上下文指明特定的元素還應具有一個數字值，否則 A 陣列中的元素用字元串值來建立。</td>

</tr>

<tr>

<td>tolower( String )</td>

<td>返回 String 參數指定的字元串，字元串中每個大寫字元將更改為小寫。大寫和小寫的對映由當前語言環境的 LC_CTYPE 範疇定義。</td>

</tr>

<tr>

<td>toupper( String )</td>

<td>返回 String 參數指定的字元串，字元串中每個小寫字元將更改為大寫。大寫和小寫的對映由當前語言環境的 LC_CTYPE 範疇定義。</td>

</tr>

<tr>

<td>sprintf(Format, Expr, Expr, . . . )</td>

<td>根據 Format 參數指定的 printf 子例程格式字元串來格式化 Expr 參數指定的表示式並返回最後生成的字元串。</td>

</tr>

</tbody>

</table>

注：Ere都可以是正規表示式。

 **gsub,sub使用**

```
awk 'BEGIN{info="this is a test2010test!";gsub(/[0-9]+/,"!",info);print info}'
this is a test!test!
```

在 info中查詢滿足正規表示式，`/[0-9]+/` 用`””`替換，並且替換後的值，賦值給info 未給info值，預設是`$0`

 **查詢字元串（index使用）**

```
awk 'BEGIN{info="this is a test2010test!";print index(info,"test")?"ok":"no found";}'
ok
```

未找到，返回0

 **正規表示式匹配查詢(match使用）**

```
awk 'BEGIN{info="this is a test2010test!";print match(info,/[0-9]+/)?"ok":"no found";}'
ok
```

 **擷取字元串(substr使用）**

```
[wangsl@centos5 ~]$ awk 'BEGIN{info="this is a test2010test!";print substr(info,4,10);}'
s is a tes
```

從第 4個 字元開始，擷取10個長度字元串

 **字元串分割（split使用）**

```
awk 'BEGIN{info="this is a test";split(info,tA," ");print length(tA);for(k in tA){print k,tA[k];}}'
4
4 test
1 this
2 is
3 a
```

分割info，動態建立陣列tA，這裡比較有意思，`awk for …in`迴圈，是一個無序的迴圈。 並不是從陣列下標1…n ，因此使用時候需要注意。

 **格式化字元串輸出（sprintf使用）**

格式化字元串格式：

其中格式化字元串包括兩部分內容：一部分是正常字元，這些字元將按原樣輸出; 另一部分是格式化規定字元，以`"%"`開始，後跟一個或幾個規定字元,用來確定輸出內容格式。

<table width="100%">

<thead>

<tr>

<th>格式</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>%d</td>

<td>十進位制有符號整數</td>

</tr>

<tr>

<td>%u</td>

<td>十進位制無符號整數</td>

</tr>

<tr>

<td>%f</td>

<td>浮點數</td>

</tr>

<tr>

<td>%s</td>

<td>字元串</td>

</tr>

<tr>

<td>%c</td>

<td>單個字元</td>

</tr>

<tr>

<td>%p</td>

<td>指針的值</td>

</tr>

<tr>

<td>%e</td>

<td>指數形式的浮點數</td>

</tr>

<tr>

<td>%x</td>

<td>%X 無符號以十六進位制表示的整數</td>

</tr>

<tr>

<td>%o</td>

<td>無符號以八進位制表示的整數</td>

</tr>

<tr>

<td>%g</td>

<td>自動選擇合適的表示法</td>

</tr>

</tbody>

</table>

```
awk 'BEGIN{n1=124.113;n2=-1.224;n3=1.2345; printf("%.2f,%.2u,%.2g,%X,%on",n1,n2,n3,n1,n1);}'
124.11,18446744073709551615,1.2,7C,174
```

### 一般函數

<table width="100%">

<thead>

<tr>

<th>格式</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>close( Expression )</td>

<td>用同一個帶字元串值的 Expression 參數來關閉由 print 或 printf 語句開啟的或呼叫 getline 函數開啟的檔案或管道。如果檔案或管道成功關閉，則返回 0；其它情況下返回非零值。如果打算寫一個檔案，並稍後在同一個程式中讀取檔案，則 close 語句是必需的。</td>

</tr>

<tr>

<td>system(command )</td>

<td>執行 Command 參數指定的命令，並返回退出狀態。等同於 system 子例程。</td>

</tr>

<tr>

<td>Expression | getline [ Variable ]</td>

<td>從來自 Expression 參數指定的命令的輸出中通過管道傳送的流中讀取一個輸入記錄，並將該記錄的值指定給 Variable 參數指定的變數。如果當前未開啟將 Expression 參數的值作為其命令名稱的流，則建立流。建立的流等同於呼叫 popen 子例程，此時 Command 參數取 Expression 參數的值且 Mode 參數設定為一個是 r 的值。只要流保留開啟且 Expression 參數求得同一個字元串，則對 getline 函數的每次後續呼叫讀取另一個記錄。如果未指定 Variable 參數，則 $0 記錄變數和 NF 特殊變數設定為從流讀取的記錄。</td>

</tr>

<tr>

<td>getline [ Variable ] < Expression</td>

<td>從 Expression 參數指定的檔案讀取輸入的下一個記錄，並將 Variable 參數指定的變數設定為該記錄的值。只要流保留開啟且 Expression 參數對同一個字元串求值，則對 getline 函數的每次後續呼叫讀取另一個記錄。如果未指定 Variable 參數，則 $0 記錄變數和 NF 特殊變數設定為從流讀取的記錄。</td>

</tr>

<tr>

<td>getline [ Variable ]</td>

<td>將 Variable 參數指定的變數設定為從當前輸入檔案讀取的下一個輸入記錄。如果未指定 Variable 參數，則 $0 記錄變數設定為該記錄的值，還將設定 NF、NR 和 FNR 特殊變數。</td>

</tr>

</tbody>

</table>

 **開啟外部檔案（close用法）**

```
awk 'BEGIN{while("cat /etc/passwd"|getline){print $0;};close("/etc/passwd");}'
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
```

 **逐行讀取外部檔案(getline使用方法）**

```
awk 'BEGIN{while(getline < "/etc/passwd"){print $0;};close("/etc/passwd");}'
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
```

```
awk 'BEGIN{print "Enter your name:";getline name;print name;}'
Enter your name:
chengmo
chengmo
```

 **呼叫外部應用程式(system使用方法）**

```
awk 'BEGIN{b=system("ls -al");print b;}'
total 42092
drwxr-xr-x 14 chengmo chengmo     4096 09-30 17:47 .
drwxr-xr-x 95 root   root       4096 10-08 14:01 ..
```

b返回值，是執行結果。

### 時間函數

<table width="100%">

<thead>

<tr>

<th>格式</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>函數名</td>

<td>說明</td>

</tr>

<tr>

<td>mktime( YYYY MM dd HH MM ss[ DST])</td>

<td>生成時間格式</td>

</tr>

<tr>

<td>strftime([format [, timestamp]])</td>

<td>格式化時間輸出，將時間戳轉為時間字元串
具體格式，見下表.</td>

</tr>

<tr>

<td>systime()</td>

<td>得到時間戳,返回從1970年1月1日開始到當前時間(不計閏年)的整秒數</td>

</tr>

</tbody>

</table>

 **建指定時間(mktime使用）**

```
awk 'BEGIN{tstamp=mktime("2001 01 01 12 12 12");print strftime("%c",tstamp);}'
2001年01月01日 星期一 12時12分12秒
```

```
awk 'BEGIN{tstamp1=mktime("2001 01 01 12 12 12");tstamp2=mktime("2001 02 01 0 0 0");print tstamp2-tstamp1;}'
2634468
```

求2個時間段中間時間差，介紹了strftime使用方法

```
awk 'BEGIN{tstamp1=mktime("2001 01 01 12 12 12");tstamp2=systime();print tstamp2-tstamp1;}'
308201392
```

 **strftime日期和時間格式說明符**

<table width="100%">

<thead>

<tr>

<th>格式</th>

<th>描述</th>

</tr>

</thead>

<tbody>

<tr>

<td>%a</td>

<td>星期幾的縮寫(Sun)</td>

</tr>

<tr>

<td>%A</td>

<td>星期幾的完整寫法(Sunday)</td>

</tr>

<tr>

<td>%b</td>

<td>月名的縮寫(Oct)</td>

</tr>

<tr>

<td>%B</td>

<td>月名的完整寫法(October)</td>

</tr>

<tr>

<td>%c</td>

<td>本地日期和時間</td>

</tr>

<tr>

<td>%d</td>

<td>十進位制日期</td>

</tr>

<tr>

<td>%D</td>

<td>日期 08/20/99</td>

</tr>

<tr>

<td>%e</td>

<td>日期，如果只有一位會補上一個空格</td>

</tr>

<tr>

<td>%H</td>

<td>用十進位制表示24小時格式的小時</td>

</tr>

<tr>

<td>%I</td>

<td>用十進位制表示12小時格式的小時</td>

</tr>

<tr>

<td>%j</td>

<td>從1月1日起一年中的第幾天</td>

</tr>

<tr>

<td>%m</td>

<td>十進位制表示的月份</td>

</tr>

<tr>

<td>%M</td>

<td>十進位制表示的分鐘</td>

</tr>

<tr>

<td>%p</td>

<td>12小時表示法(AM/PM)</td>

</tr>

<tr>

<td>%S</td>

<td>十進位制表示的秒</td>

</tr>

<tr>

<td>%U</td>

<td>十進位制表示的一年中的第幾個星期(星期天作為一個星期的開始)</td>

</tr>

<tr>

<td>%w</td>

<td>十進位制表示的星期幾(星期天是0)</td>

</tr>

<tr>

<td>%W</td>

<td>十進位制表示的一年中的第幾個星期(星期一作為一個星期的開始)</td>

</tr>

<tr>

<td>%x</td>

<td>重新設定本地日期(08/20/99)</td>

</tr>

<tr>

<td>%X</td>

<td>重新設定本地時間(12：00：00)</td>

</tr>

<tr>

<td>%y</td>

<td>兩位數字表示的年(99)</td>

</tr>

<tr>

<td>%Y</td>

<td>當前月份</td>

</tr>

<tr>

<td>%Z</td>

<td>時區(PDT)</td>

</tr>

<tr>

<td>%%</td>

<td>百分號(%)</td>

</tr>

</tbody>

</table>


<!-- Linux命令列搜尋引擎：https://jaywcjlove.github.io/linux-command/ -->
