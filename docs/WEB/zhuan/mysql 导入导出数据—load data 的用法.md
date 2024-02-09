## **一、loaddata 导入数据的语法 load data infile**

### **基本语法**

```
load data [low_priority] [local] infile 'file_name.txt' [replace|ignore]
into table table_name
fields
[terminated by '\t']
[[OPTIONALLY] enclosed by '']
[escaped by '\' ]
[lines terminated by '\n']
[ignore number lines]
[(col_name,   )]
```



### **参数说明**

<table border="0"><tbody><tr><td>&nbsp; &nbsp; &nbsp;参&nbsp; &nbsp; &nbsp;数&nbsp;</td><td>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;说&nbsp; &nbsp; 明&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</td><td>&nbsp;备&nbsp; &nbsp; 注&nbsp;</td></tr><tr><td>low_priority</td><td>MySQL 将会等到没有其他人读这个表的时候，才把数据插入</td><td>load data low_priority infile "/tmp/harara.sql" into table orders;</td></tr><tr><td>local</td><td>表明从客户主机读文件。如果 local 没指定，文件必须位于服务器上</td><td>load data&nbsp;low_priority local infile "/tmp/harara.sql" into table orders;</td></tr><tr><td>&nbsp;replace</td><td>如果你指定 replace，新行将代替有相同的唯一健值的现有行</td><td>load data&nbsp;low_priority&nbsp;local&nbsp;infile "/tmp/harara.sql" replace into table orders;</td></tr><tr><td>&nbsp;ignore</td><td>如果你指定 ignore，跳过有唯一键的现有行的重复行输入</td><td>&nbsp;load data&nbsp;low_priority&nbsp;local&nbsp;infile "/tmp/harara.sql" ignore into table orders;</td></tr><tr><td>&nbsp;fields terminated by&nbsp;</td><td>描述字段的分隔符，默认情况下是 tab 字符（\t）</td><td rowspan="3">load data infile "/tmp/harara.sql" replace into table orders fields terminated by ',' enclosed by '"'escaped by'\';</td></tr><tr><td>&nbsp;fields enclosed by</td><td>描述的是字段的括起字符。</td></tr><tr><td>&nbsp;fields escaped by&nbsp;</td><td>escaped by 描述的转义字符。默认的是反斜杠: \&nbsp;</td></tr><tr><td>&nbsp;lines terminated by'\n'</td><td>指定每条记录的分隔符默认为 "\n" 即为换行符</td><td>load data infile "/tmp/harara.sql" replace into table test fields terminated by ',' lines terminated by '\n';</td></tr><tr><td>&nbsp;(col_name,)</td><td>可以按指定的列把文件导入到数据库中</td><td><p>当我们要把数据的一部分内容导入，需要加入一些栏目（列 / 字段 / field）到 MySQL 数据库中，以适应一些额外的需要。</p><p>比如，我们要从 Access 数据库升级到 MySQL 数据库的时候，下面的例子显示了如何向指定的栏目 (field) 中导入数据：</p><p>load data infile "/home/harara.sql" into table orders(order_number,order_date,customer_id)</p></td></tr></tbody></table>



### **导入数据完整示例**

```
load data local infile '/var/lib/mysql/emp8/customers.txt' replace into table lf_lanx_tmplp1 character set gbk 
fields terminated by ',' enclosed by '"' 
lines terminated by '\n' 
(TMPl_NAME);
```





## **二、loaddata 导出数据的语法 select into outfile**

### **基本语法**

```
select ... into outfile 'file_name'
        [character set charset_name]
        [export_options]


export_options:
     [{fields | columns}
       [terminated by 'string']
       [[optionally] enclosed by 'char']
       [escaped by 'char']
    ]
    [lines
        [starting by 'string']
        [terminated by 'string']
    ]
```



### **参数说明**

<table border="0"><tbody><tr><td>&nbsp; &nbsp; &nbsp; 参&nbsp; &nbsp; &nbsp; &nbsp; 数&nbsp; &nbsp; &nbsp;&nbsp;</td><td>&nbsp; &nbsp; &nbsp; &nbsp;说&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;明&nbsp; &nbsp; &nbsp; &nbsp;</td></tr><tr><td>fields terminated by 'str'</td><td>设置字段之间的分隔符，默认是 "\t"</td></tr><tr><td>fields&nbsp; enclosed by 'char'</td><td>设置包括住字段的值的符号，如单引号、双引号等，默认情况下不使用任何符号</td></tr><tr><td>fields optionally enclosed by 'char'</td><td>设置括住 CHAR、VARCHAR 和 TEXT 等字符型字段的分隔符，默认情况下不使用任何符号</td></tr><tr><td>fields escaped by 'char'</td><td>设置转义字符，默认值为 "\"</td></tr><tr><td>lines starting by 'str'</td><td>设置每行数据开头的字符，可以为单个或多个字符。默认情况下不使用任何字符</td></tr><tr><td>lines terminated by 'char'</td><td>设置每行数据结尾的字符，可以为单个或多个字符。默认值是 "\n"</td></tr></tbody></table>



### **完整实例**

```
SELECT TMPl_NAME INTO OUTFILE 'u/customers.txt' 
FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n'
FROM LF_LANX_TMPLP1;
```



## 参考地址:

mybatis 处理大批量数据。使用 mysql 的 LOAD DATA INFILE : [https://blog.csdn.net/xcc_2269861428/article/details/83861665](https://blog.csdn.net/xcc_2269861428/article/details/83861665)

mysql 高效导入导出 load data [infile][outfile] 用法 : [https://www.cnblogs.com/yyy-blog/p/11073855.html](https://www.cnblogs.com/yyy-blog/p/11073855.html)