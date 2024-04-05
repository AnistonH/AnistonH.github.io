# CTFshow XXE

**题目范围：Web373 - Web378**



**关于XXE：**[一篇文章带你深入理解漏洞之 XXE 漏洞](https://xz.aliyun.com/t/3357)



## Web 373

代码如下：

```php
<?php
error_reporting(0);
libxml_disable_entity_loader(false);
$xmlfile = file_get_contents('php://input');
if (isset($xmlfile)) {
    $dom = new DOMDocument();
    $dom->loadXML($xmlfile, LIBXML_NOENT | LIBXML_DTDLOAD);
    $creds = simplexml_import_dom($dom);
    $ctfshow = $creds->ctfshow;
    echo $ctfshow;
}
highlight_file(__FILE__);
?>
```

> 该段PHP代码具有以下功能：
>
> 1. **禁用PHP错误报告**（第1行）:
>
>     - 通过调用`error_reporting(0)`函数，设置全局错误报告级别为0，即关闭所有PHP错误、警告和通知的显示。这样做可能出于安全考虑或避免在生产环境中向用户暴露不必要的内部信息。
>
> 2. **启用libxml实体加载**（第2行）:
>
>     - 调用`libxml_disable_entity_loader(false)`函数以重新启用libxml库中的外部实体加载功能。默认情况下，此功能被禁用以防止潜在的XML实体注入攻击。在此场景中，由于需要处理包含DTD（文档类型定义）的XML输入，因此选择启用实体加载。
>
> 3. **从输入流中获取XML数据**（第3行）:
>
>     - 使用`file_get_contents('php://input')`从PHP的输入流（`php://input`）中读取原始POST数据。这表明预期接收的是非表单编码的数据，如XML内容，且传递方式可能是POST或其他支持原始数据传输的方法。
>
> 4. **检查并解析XML数据**（第4-6行）:
>
>     - 首先，使用`isset()`函数检查是否存在接收到的XML数据。
>
>         - 若存在，创建一个新的DOMDocument对象，并调用其`loadXML()`方法来解析XML字符串。传入两个参数：
>
>         - `$xmlfile`：待解析的XML字符串。
>
>         - `LIBXML_NOENT | LIBXML_DTDLOAD`
>
>             这两个常量作为标志位，分别表示：
>
>             - `LIBXML_NOENT`: 在解析时替换实体引用。
>             - `LIBXML_DTDLOAD`: 加载外部DTD（如果有），以便正确解析XML结构和验证数据。
>
>     - 然后，使用`simplexml_import_dom()`函数将已解析的`DOMDocument`对象转换为更易于操作的`SimpleXMLElement`对象。
>
>     - 最后，通过`$creds->ctfshow`访问并存储`ctfshow`节点的值。
>
> 5. **输出`ctfshow`节点的值**（第7行）:
>
>     - 使用`echo`语句将之前获取到的`$ctfshow`变量（即`ctfshow`节点的值）发送至浏览器，供客户端查看或进一步处理。
>
> 6. **高亮显示当前PHP源代码**（第8行）:
>
>     - `highlight_file(__FILE__)`函数调用会输出当前执行脚本（由`__FILE__`魔术常量标识）的源代码，并应用语法高亮。这种做法通常用于开发环境，便于开发者直观查看和理解代码结构。在生产环境中，出于安全考虑，不建议启用此功能。
>
> 综上所述，这段PHP代码的主要目的是接收并解析含有DTD的XML输入，提取其中`ctfshow`节点的值，并将该值返回给客户端。同时，在开发阶段，它还展示了当前脚本的源代码以辅助调试。



### **payload：直接file:///flag**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE XXE [
<!ENTITY cmd SYSTEM "file:///flag">
]>
<mumuzi>
    <ctfshow>&cmd;</ctfshow>
</mumuzi>
```

> 1. **XML声明**：
>
>     - `<?xml version="1.0" encoding="UTF-8"?>` 这是XML文档的起始声明，指定了使用的XML版本为1.0以及字符编码方式为UTF-8。
>
> 2. **DOCTYPE声明与内部子集**：
>
>     - ```
>         <!DOCTYPE XXE [ ... ]>
>         ```
>
>         这是一个DOCTYPE声明，用于指定文档遵循的DTD（Document Type Definition，文档类型定义）。这里创建了一个名为`XXE`的自定义DOCTYPE。
>
>         - 在方括号`[ ... ]`内定义了内部子集，包含了针对当前DOCTYPE的实体、属性等定义。
>
> 3. **实体定义**：
>
>     - ```
>         <!ENTITY cmd SYSTEM "file:///flag">
>         ```
>
>         在内部子集中定义了一个名为`cmd`的实体。这个实体具有以下属性：
>
>         - `SYSTEM "file:///flag"`：指定实体值应从指定的URI（统一资源标识符）获取。这里的URI指向本地文件系统中的路径`/flag`，使用的是`file://`协议。
>
> 4. **XML元素**：
>
>     - `<mumuzi>` 和 `</mumuzi>` 定义了一个根元素，名为`mumuzi`。
>     - `<ctfshow>` 和 `</ctfshow>` 作为`mumuzi`的子元素，其内容为`&cmd;`。
