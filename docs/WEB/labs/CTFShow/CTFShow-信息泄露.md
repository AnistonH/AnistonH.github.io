# CTFshow 信息泄露（1-20）

## web 2 源码

URL前加入`view-source:`    查看源代码。

或者直接`ctrl+u`





## web 5 .phps

**.phps导致源码泄露**
 phps文件就是php的源代码文件，通常用于提供给用户（访问者）直接通过Web浏览器查看php代码的内容。
 因为用户无法直接通过Web浏览器“看到”php文件的内容，所以需要用phps文件代替。
 并不是所有的php文件都存在.phps后缀，不是默认带有，只会在特殊情况下存在。





## Web 6 源码泄露

**常见的网站源码备份文件后缀**

- tar
- tar.gz
- zip
- rar
- bak

**常见的网站源码备份文件名**
- web
- website
- backup
- back
- www
- wwwroot
- temp

**脚本：**

```python
import requests
 
url1 = 'url'
 
# url为被扫描地址
 
list1 = ['web', 'website', 'backup', 'back', 'www', 'wwwroot', 'temp']
 
list2 = ['tar', 'tar.gz', 'zip', 'rar']
 
for i in list1:
 
    for j in list2:
 
        back = str(i) + '.' + str(j)
 
        url = str(url1) + '/' + back
 
        print(back + '    ', end='')
 
        print(requests.get(url).status_code)
```





## web 9 vim缓存

在使用vim时会创建临时缓存文件，关闭vim时缓存文件则会被删除，当vim异常退出后，因为未处理缓存文件，导致可以通过缓存文件恢复原始文件内容。

以 index.php 为例：第一次产生的交换文件名为 .index.php.swp。

再次意外退出后，将会产生名为 .index.php.swo 的交换文件。

第三次产生的交换文件则为 .index.php.swn。

注意：最前面有一个点，也就是说应该是`http://xxxxx.com/.index.php.swp`而不是`http://xxxxx.com/index.php.swp`

（但是这个题没有点呀）





## ??web 11

查询域名解析地址，基本格式：`nslookup host [server]`

查询域名的指定解析类型的解析记录 基本格式：`nslookup -type=type host [server]`

查询全部 基本格式：`nslookup -query=any host [server]`

编辑`nslookup -query=any flag.ctfshow.com`

C:\Users\16032>nslookup -query=any flag.ctfshow.com 服务器:  public-dns-a.baidu.com Address:  180.76.76.76



### nslookup

**nslookup** 主要用来诊断域名系统 (DNS) 基础结构的信息。查询DNS的记录，查询域名解析是否正常，在网络故障时用来诊断网络问题。

1. **查询域名解析地址**

`nslookup sohu.com` 采用默认的DNS服务器查询

`nslookup sohu.com 114.114.114.114` 采用指定的DNS服务器查询

> 注：
>
> 1. 每个DNS服务器查询到的IP可能不相同
>
> 2. 可能查询出来的记录会出现多个
>
> 3. 对于被污染的域名，查询的结果是不准确的

2.  查询域名的指定解析类型的解析记录

格式：`nslookup -type=type domain [dns-server]`

3. 查询全部

`nslookup -query=any qq.com`





## Web 11-2 CDN绕过

不知道为什么，bilibili上的视频中的Web 11和网站上的不一样，视频上的是要找到网站真实的ip地址，但是通过ping网站以及用各类工具网站都没有查到，最后将网址加上“www”然后用加了3w的进行ping，成功返回了真实的ip地址。

这个就是说明，原网站是有3W的网站，然后一些cdn会每隔一段时间从原网站抓取一下静态的数据，然后当我们访问时访问的就是这些cdn加速服务器的ip

**参考文章：**[干货-6种CDN绕过技术](https://www.freebuf.com/articles/others-articles/278391.html)





## web 16 PHP探针

PHP探针实际上是一种Web脚本程序，主要是用来探测虚拟空间、服务器的运行状况，而本质上是通过PHP语言实现探测PHP服务器敏感信息的脚本文件，通常用于探测网站目录、服务器操作系统、PHP版本、数据库版本、CPU、内存、组件支持等，基本能够很全面的了解服务器的各项信息。

> 常见的PHP探针有：
>
> - 雅黑php探针：一般用于linux系统，目前不支持PHP7以上版本。
> - X探针：（又名刘海探针）是一款由 INN STUDIO 原创主导开发的开源 PHP 探针，支持PHP5.3+以上。
> - UPUPW php探针：安全性较高，可以防探针泄露，一般用于window系统。

探针的文件常见命名为`tz.php`，我们可以尝试一下御剑扫描

可以通过这个打开`phpinfo`





## web 20  mdb文件泄露

 mdb文件是早期asp+access构架的数据库文件，文件泄露相当于数据库被脱裤了。

dirsearch递归扫描

> 可能是 /db/db.mdb
>





## other .DS_Store

.DS_Store 是 Mac OS 保存文件夹的自定义属性的隐藏文件。如文件的图标位置或背景色，相当于Windows的desktop.ini。 其删除以后的副作用就是这些信息的失去。通过.DS_Store可以知道这个目录里面所有文件的清单。	

和别人交换文件（或你做的网页需要上传的时候）应该把 .DS_Store 文件删除比较妥当，因为里面包含了一些你不一定希望别人看见的信息。尤其是网站，通过 .DS_Store 可以知道这个目录里面所有文件的清单，很多时候这是一个不希望出现的问题。

**DS_Store文件泄漏**
.DS_Store是Mac下Finder用来保存如何展示 文件/文件夹 的数据文件，每个文件夹下对应一个。由于开发/设计人员在发布代码时未删除文件夹中隐藏的.DS_store，可能造成文件目录结构泄漏、源代码文件等敏感信息的泄露。
