# CTFshow 爆破

**题目范围：Web21 - Web28**





## Web 25待解决



## Web 22 爆破子域名

题目提示：域名也可以爆破的，试试爆破这个ctf.show的子域名
既然是域名爆破，这里我用的Layer，输入域名后开始爆破

**工具：Layer子域名挖掘机 或 Oneforall工具**





## Web 23 脚本

代码如下：

```php
<?php
error_reporting(0);

include('flag.php');
if (isset($_GET['token'])) {
    $token = md5($_GET['token']);
    if (substr($token, 1, 1) === substr($token, 14, 1) && substr($token, 14, 1) === substr($token, 17, 1)) {
        if ((intval(substr($token, 1, 1)) + intval(substr($token, 14, 1)) + substr($token, 17, 1)) / substr($token, 1, 1) === intval(substr($token, 31, 1))) {
            echo $flag;
        }
    }
} else {
    highlight_file(__FILE__);
}
?>
```

#### **通义灵码：**

该PHP函数的功能是验证一个token是否有效，并根据验证结果输出一个特定的字符串。

1. 首先，通过`error_reporting(0)`关闭错误报告，避免在验证过程中出现错误信息。
2. 然后，通过`include('flag.php')`引入一个名为`flag.php`的文件。
3. 接下来，通过`if (isset($_GET['token']))`判断是否存在名为`token`的GET参数。
4. 如果存在`token`参数，则通过`$token = md5($_GET['token'])`将token值进行MD5加密。
5. 然后，通过`substr($token, 1, 1)`、`substr($token, 14, 1)`和`substr($token, 17, 1)`分别获取token的第2、15和18个字符。
6. 接下来，通过`substr($token, 1, 1) === substr($token, 14, 1)`和`substr($token, 14, 1) === substr($token, 17, 1)`判断第2、15和18个字符是否相等。
7. 如果相等，则通过`intval(substr($token, 1, 1)) + intval(substr($token, 14, 1)) + substr($token, 17, 1)`计算一个数值。
8. 然后，通过`intval(substr($token, 31, 1))`获取token的第32个字符，并将其转换为整数。
9. 最后，通过`(intval(substr($token, 1, 1)) + intval(substr($token, 14, 1)) + substr($token, 17, 1)) / substr($token, 1, 1) === intval(substr($token, 31, 1))`判断计算得到的数值是否等于第32个字符的整数值。
10. 如果验证通过，则输出`$flag`变量的值。
11. 如果不存在`token`参数，则通过`highlight_file(__FILE__)`高亮显示当前文件。

注意：该函数存在一定的安全风险，因为它直接使用了用户输入的token进行验证，可能存在注入攻击的风险。建议在实际应用中进行改进和加强安全性。



#### 脚本

**PS：不是我写的，我现在还没有学过python，就先整理了一下别人的。**

```python
import hashlib

def intval(_char):

    if _char.isdigit():

        return int(_char)
    
    return 0

for i in range(999999):

    md5 = hashlib.md5(str(i).encode('utf-8')).hexdigest()

    if md5[1] == md5[14] and md5[14] == md5[17]:

        if md5[1].isdigit() and int(md5[1]) != 0:

            if (intval(md5[1]) + intval(md5[14]) + intval(md5[17]))/int(md5[1]) == intval(md5[31]):
                
                print(i, md5)
```

```python
import hashlib

dic = '0123456789qazwsxedcrfvtgbyhnujmikolp'

for a in dic:
    for b in dic:
        t = str(a)+str(b)

        md5 = hashlib.md5(t.encode('utf-8')).hexdigest()

        if md5[1] != md5[14] or md5[14]!= md5[17]:

            continue

        if(ord(md5[1]))>=48 and ord(md5[1])<=57 and (ord(md5[31]))>=48 and ord(md5[31])<=57:

            if((int(md5[1])+int(md5[14])+int(md5[17]))/int(md5[1])==int(md5[31])):

                print(t)

# 得到token=3j
```

```python
import hashlib

dic = '0123456789qazwsxedcrfvtgbyhnujmikolp'

for a in dic:

    for b in dic:

        t = str(a)+str(b)
        
        md5 = hashlib.md5(t.encode('utf-8')).hexdigest()

        if md5[1:2] == md5[14:15] and md5[14:15]== md5[17:18]:

            if(ord(md5[1:2]))>=48 and ord(md5[1:2])<=57 and (ord(md5[14:15]))>=48 and ord(md5[14:15])<=57:

                if (ord(md5[17:18]))>=48 and ord(md5[17:18])<=57 and (ord(md5[31:32]))>=48 and ord(md5[31:32])<=57:

                    if(int(md5[1:2])+int(md5[14:15])+int(md5[17:18]))/int(md5[1:2])==int(md5[31:32]):

                        print(t)
```

```python
import hashlib

for i in range(10000000):

    token = hashlib.md5(str(i).encode()).hexdigest()

    if token[1] == token[14] == token[17]:

        if (int(token[1], 16) + int(token[14], 16) + int(token[17], 16)) / int(token[1], 16) == int(token[31], 16):

            print(f"Found token: {i}")
            
            break
```

```python
import hashlib

for i in range(100000):

    hash_value = hashlib.md5(str(i).encode()).hexdigest()

    if hash_value[-1] == "3" and hash_value[1] == hash_value[14] == hash_value[17]:
        
        print(f"找到符合条件的数字：{i}，其MD5值为：{hash_value}")
```

**PHP：**

```php
<?php
$chars = array("a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z", "A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z", "0", "1", "2", "3", "4", "5", "6", "7", "8", "9");

function get_token($str)
{

    $token = md5($str);

    if (substr($token, 1, 1) === substr($token, 14, 1) && substr($token, 14, 1) === substr($token, 17, 1)) {

        if (substr($token, 1, 1) != 0) {

            if ((intval(substr($token, 1, 1)) + intval(substr($token, 14, 1)) + substr($token, 17, 1)) / substr($token, 1, 1) === intval(substr($token, 31, 1))) {

                echo $str . ' ';
            }
        }
    }
}

//如果1层没运算成功 那么就运算两层 两层不够 运算三层。

foreach ($chars as $k => $v) {

    foreach ($chars as $kk => $vv) {

        get_token($v . $vv);
    }
}
```





## Web 24 伪随机数

这个题很有意思，代码如下：

```php
<?php
error_reporting(0);

include("flag.php");
if (isset($_GET['r'])) {
    $r = $_GET['r'];
    mt_srand(372619038);
    if (intval($r) === intval(mt_rand())) {
        echo $flag;
    }
} else {
    highlight_file(__FILE__);
    echo system('cat /proc/version');
}
?>
```

**php伪随机数`mt_rand()`函数**

先是mt_srand(372619038)，播种了一个随机数种子，然后比较用户传的GET参数r与否与mt_rand()生成的随机数一样，若一样则输出flag。

逻辑很简单，只要发送的GET请求中的参数与后台生成的随机数一样即可获取flag。

这里存在一个伪随机数漏洞，在php中每一次调用mt_rand()函数，都会检查一下系统有没有播种。（播种为mt_srand()函数完成），当随机种子生成后，后面生成的随机数都会根据这个随机种子生成。所以同一个种子下，随机数的序列是相同的，这就是漏洞点。

上面已经用一个固定数字播种了随机数，mt_srand(372619038)，所以只要在同一的php版本中，复现一下，也用mt_srand(372619038)，播种随机数种子，再用mt_rand()生成随机数，得到的随机数都是一样的

所以第一步，用Burp随便传一个r过去，，查看网络中的响应包，获取靶机php版本，再在自己主机上搭建同样版本的php环境，播种同样的随机数种子，生成的随机数当作参数r的值发送请求，即可获取flag。r值为：url/?r=1155388967

其实这儿用一些在线的php运行平台就可以。

#### 官方说明：

mt_scrand(seed)这个函数的意思，是通过分发seed种子，然后种子有了后，靠mt_rand()生成随机 数。 提示：从 PHP 4.2.0 开始，随机数生成器自动播种，因此没有必要使用该函数 因此不需要播种，并且如果设置了 seed参数 生成的随机数就是伪随机数，意思就是每次生成的随机数 是一样的。





## Web 25

代码如下：

```php
<?php
error_reporting(0);

include("flag.php");
if (isset($_GET['r'])) {
    $r = $_GET['r'];
    mt_srand(hexdec(substr(md5($flag), 0, 8)));
    $rand = intval($r) - intval(mt_rand());
    if ((!$rand)) {
        if ($_COOKIE['token'] == (mt_rand() + mt_rand())) {
            echo $flag;
        }
    } else {
        echo $rand;
    }
} else {
    highlight_file(__FILE__);
    echo system('cat /proc/version');
}
?>
```

#### **通义灵码：**

该PHP函数的功能是：

1. 关闭错误报告。
2. 包含名为"flag.php"的文件。
3. 如果在GET参数中存在"r"，则执行以下操作：
   - 将"r"赋值给变量$r。
   - 使用md5函数生成一个随机数，并使用substr函数提取前8位作为mt_rand函数的种子。
   - 计算`$r`与`mt_rand`函数生成的随机数的差值，并赋值给变量`$rand`。
   - 如果$rand为0，则执行以下操作：
     - 检查$_COOKIE['token']是否等于两个随机数之和。
     - 如果相等，则输出$flag。
   - 否则，输出$rand。
4. 如果在GET参数中不存在"r"，则执行以下操作：
   - 高亮显示当前文件。
   - 使用system函数执行"cat /proc/version"命令，并输出结果。

注意：该函数存在安全风险，因为它使用了不安全的随机数生成方法，并且在GET参数中直接输出了敏感信息。

#### WP：

不会做……要用到工具`php mt seed`，先跳过这个吧。





## Web 27

这个题很有意思。

整体流程没什么，但是我在做的时候遇到了一个问题：Burp抓不到“chechdb.php”数据包，firefox浏览器显示状态码是“拦截”，但是用别的浏览器就好了。

然后摸索了半天，最后在实验室学长和CTFShow官方答疑人员帮助下，终于知道了原因：**http通过js向https发送ajax请求会被跨域拦截**

这句话的意思是，在网页开发中，如果你的网页是通过 HTTP 协议加载的（即非安全的网页），而你尝试使用 JavaScript 代码通过 AJAX（Asynchronous JavaScript and XML）向另一个使用 HTTPS 协议（即安全的网页）的服务器发送请求，浏览器会阻止这个请求。这是因为浏览器的同源策略（Same-Origin Policy）的限制，它要求网页中的所有资源请求（比如 JavaScript、CSS、图片、XHR等）都必须与当前网页具有相同的协议、域名和端口。

因此，如果网页是通过 HTTP 协议加载的，而你尝试向使用 HTTPS 协议的服务器发送请求，浏览器会认为这是跨域请求，会拦截该请求，以保护用户的安全和隐私。要解决这个问题，一种常见的方法是确保网页和服务器使用相同的协议，即都是 HTTP 或都是 HTTPS。

**为什么同一环境下，firefox会被拦截，而用谷歌浏览器就不会被拦截呢？**

在同一环境下，不同浏览器对于跨域请求的处理可能会有所不同，这可能是由于浏览器实现同源策略的方式不同、安全策略的差异等原因导致的。

虽然大多数主流浏览器都遵循同源策略，但是它们可能在实现细节上有所不同，包括对于跨协议（HTTP与HTTPS）、跨域名（不同域名）请求的处理方式可能存在差异。

因此，某些浏览器可能会更严格地执行同源策略，拦截跨域请求，而其他一些浏览器可能会允许某些情况下的跨域请求通过。这可能是你观察到 Firefox 和 Chrome 在相同环境下处理跨域请求不一致的原因之一。





## Web 28 

抓包看到，进入网站直接进行一个302跳转，跳转到了url/0/1/2.txt。

**CTFShow提示：**

通过暴力破解目录/0-100/0-100/看返回数据包

爆破的时候去掉2.txt 仅仅爆破目录即可
