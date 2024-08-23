# python正则表达式

## 正则表达式介绍

正则表达式，简称`RegEx`，是用来匹配字符的一种工具，它常被用在网页爬虫，文稿整理，数据筛选等方面，最常用的就是用在网页爬虫，数据抓取。



## 常见符号解释

### 正则表达式的基本组成

- **元字符**：具有特殊意义的字符，如`.`、`*`、`?`、`+`等。
- **字符集**：用`[]`表示，匹配括号内的任一字符。
- **边界匹配符**：如`^`（字符串开始）、`$`（字符串结束）等。
- **量词**：用于指定前面的字符或字符集出现的次数，如`*`（0次或多次）、`+`（1次或多次）、`?`（0次或1次）等。
- **分组与捕获**：通过`()`将正则表达式的一部分分组，以便后续引用或处理。



##### 限定符

- `*`：匹配前面的子表达式零次或多次。例如，`zo*`能匹配`z`以及`zoo`。
- `+`：匹配前面的子表达式一次或多次。例如，`zo+`能匹配`zo`以及`zoo`，但不能匹配`z`。
- `?`：匹配前面的子表达式零次或一次。例如，`do(es)?`可以匹配`do`或`does`中的`do`。
- `{n}`：n是一个非负整数。匹配确定的n次。例如，`o{2}`不能匹配`Bob`中的`o`，但能匹配`food`中的两个`o`。
- `{n,}`：至少匹配n次。例如，`o{2,}`不能匹配`Bob`中的`o`，但能匹配`foooood`中的所有`o`。
- `{n,m}`：m和n均为非负整数，其中n<=m。最少匹配n次且最多匹配m次。例如，`o{1,3}`将匹配`fooooood`中的前三个`o`。

##### 定位符

- `^`：匹配输入字符串的开始位置。例如，`^a`匹配字符串`abc`中的`a`。
- `$`：匹配输入字符串的结束位置。例如，`a$`匹配字符串`bca`中的`a`。
- `\b`：匹配一个单词边界，即字与空格间的位置。例如，`er\b`可以匹配`verb`中的`er`，但不匹配`eraser`中的`er`。
- `\B`：非单词边界匹配。

##### 连接符与字符集

- `.`：匹配除换行符`\n`之外的任何单字符。
- `[ ]`：标记一个字符集的开始和结束。例如，`[abc]`匹配`a`、`b`或`c`。
- `-`：在字符集中表示范围。例如，`[a-z]`匹配任意小写字母。
- `^`：在字符集的开始处表示否定。例如，`[^abc]`匹配除`a`、`b`、`c`之外的任意字符。

##### 分组与捕获

- `()`：标记一个子表达式的开始和结束位置，可以进行分组和捕获。捕获的组可以在后续的表达式中通过`\1`、`\2`等进行引用。

##### 选择符

- `|`：指明两项之间的一个选择。例如，`z|food`匹配`z`或`food`。

##### 转义字符

- `\`：用于转义特殊字符，使其表示字面的字符本身。例如，`\.`匹配`.`字符，`\\`匹配`\`字符。

##### 非贪婪匹配

- 默认情况下，`*`、`+`和`?`等限定符是贪婪的，会尽可能多地匹配字符。通过在这些限定符后加上`?`，可以使其成为非贪婪的或最小匹配。例如，`.*?`会尽可能少地匹配任意字符。

##### 特殊字符序列

- `\d`：匹配数字，相当于`[0-9]`。
- `\D`：匹配非数字，相当于`[^0-9]`。
- `\w`：匹配字母、数字、下划线，相当于`[A-Za-z0-9_]`。
- `\W`：匹配非字母、数字、下划线，相当于`[^A-Za-z0-9_]`。
- `\s`：匹配任何空白字符，包括空格、制表符、换页符等。相当于`[\f\n\r\t\v]`
- `\S`：匹配任何非空白字符。相当于`[^\f\n\r\t\v]`



| 字符 | 描述               | 等价 |
| ---- | ------------------ | ---- |
| `\n` | 代表一个换行符     |      |
| `\r` | 代表一个回车符     |      |
| `\f` | 代表换页符         |      |
| `\t` | 代表一个制表符 Tab |      |
| `\A` | 代表字符串的开始   |      |
| `\Z` | 代表字符串的结束   |      |



## 修饰符

修饰符用于改变正则表达式的行为。不同的编程语言或正则表达式引擎可能支持不同的修饰符。

##### 全局搜索（Global Search）

- 如在JavaScript中，`g`标志用于执行全局匹配（查找所有匹配项，而不是在找到第一个匹配项后停止）。

##### 不区分大小写（Case-Insensitive）

- 如在JavaScript中，`i`标志用于执行不区分大小写的匹配。

##### 多行模式（Multiline Mode）

- 如在JavaScript中，`m`标志使得`^`和`$`分别匹配输入字符串的开始和结束位置，以及任何换行符之后和之前的位置。

## 示例正则

以下正则表达式匹配一个正整数，**[1-9]**设置第一个数字不是 0，**[0-9]\*** 表示任意多个数字：

```
/[1-9][0-9]*/
```

这里不使用 + 限定符，因为在第二个位置或后面的位置不一定需要有一个数字。也不使用 ? 字符，因为使用 ? 会将整数限制到只有两位数。

如果你想设置 0~99 的两位数，可以使用下面的表达式来至少指定一位但至多两位数字。

```
/[0-9]{1,2}/
```

上面的表达式的缺点是，只能匹配两位数字，而且可以匹配 0、00、01、10 99 的章节编号仍只匹配开头两位数字。

改进下，匹配 1~99 的正整数表达式如下：

```
/[1-9][0-9]?/
```

或

```
/[1-9][0-9]{0,1}/
```



**注意：**

`*` 和 `+` 限定符都是贪婪的，因为它们会尽可能多的匹配文字，只有在它们的后面加上一个 `?` 就可以实现非贪婪或最小匹配。



## Python

Python的`re`模块提供了正则表达式的相关功能，主要函数包括：

- `re.match(pattern, string, flags=0)`：从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，`match()`就返回`none`。
- `re.search(pattern, string, flags=0)`：扫描整个字符串并返回第一个成功的匹配。
- `re.findall(pattern, string, flags=0)`：在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。
- `re.split(pattern, string, maxsplit=0, flags=0)`：根据匹配进行分割字符串，并返回一个列表。
- `re.sub(pattern, repl, string, count=0, flags=0)`：替换字符串中所有匹配正则表达式的子串。

re.match 函数
-----------

re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match() 就返回 none。

**函数语法**：

```python
re.match(pattern, string, flags=0)
```

函数参数说明：

<table><tbody><tr><th>参数</th><th>描述</th></tr><tr><td>pattern</td><td>匹配的正则表达式</td></tr><tr><td>string</td><td>要匹配的字符串。</td></tr><tr><td>flags</td><td>标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。</td></tr></tbody></table>

匹配成功 `re.match` 方法返回一个匹配的对象，否则返回 `None`。

我们可以使用` group(num)` 或` groups()` 匹配对象函数来获取匹配表达式。

<table><tbody><tr><th>匹配对象方法</th><th>描述</th></tr><tr><td>group(num=0)</td><td>匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。</td></tr><tr><td>groups()</td><td>返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。</td></tr></tbody></table>

```python
import re
print(re.match('www', 'www.runoob.com').span())  # 在起始位置匹配    (0, 3)
print(re.match('com', 'www.runoob.com'))         # 不在起始位置匹配   None
```



```python
import re
 
line = "Cats are smarter than dogs"
 
matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)
 
if matchObj:
   print "matchObj.group() : ", matchObj.group()
   print "matchObj.group(1) : ", matchObj.group(1)
   print "matchObj.group(2) : ", matchObj.group(2)
else:
   print "No match!!"
```

以上实例执行结果如下：

```python
matchObj.group() :  Cats are smarter than dogs
matchObj.group(1) :  Cats
matchObj.group(2) :  smarter
```

re.search 方法
------------

re.search 扫描整个字符串并返回第一个成功的匹配。

函数语法：

```python
re.search(pattern, string, flags=0)
```

函数参数说明：

<table><tbody><tr><th>参数</th><th>描述</th></tr><tr><td>pattern</td><td>匹配的正则表达式</td></tr><tr><td>string</td><td>要匹配的字符串。</td></tr><tr><td>flags</td><td>标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。</td></tr></tbody></table>

匹配成功 `re.search` 方法返回一个匹配的对象，否则返回 `None`。

我们可以使用 `group(num)` 或 `groups()` 匹配对象函数来获取匹配表达式。

<table><tbody><tr><th>匹配对象方法</th><th>描述</th></tr><tr><td>group(num=0)</td><td>匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。</td></tr><tr><td>groups()</td><td>返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。</td></tr></tbody></table>

```python
import re
print(re.search('www', 'www.runoob.com').span())  # 在起始位置匹配    (0, 3)
print(re.search('com', 'www.runoob.com').span())  # 不在起始位置匹配  (11, 14)
```



```python
import re
 
line = "Cats are smarter than dogs";
 
searchObj = re.search( r'(.*) are (.*?) .*', line, re.M|re.I)
 
if searchObj:
   print "searchObj.group() : ", searchObj.group()
   print "searchObj.group(1) : ", searchObj.group(1)
   print "searchObj.group(2) : ", searchObj.group(2)
else:
   print "Nothing found!!"
```

```python
searchObj.group() :  Cats are smarter than dogs
searchObj.group(1) :  Cats
searchObj.group(2) :  smarter
```

re.match 与 re.search 的区别
------------------------

`re.match` 只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回 `None`；而 `re.search` 匹配整个字符串，直到找到一个匹配。