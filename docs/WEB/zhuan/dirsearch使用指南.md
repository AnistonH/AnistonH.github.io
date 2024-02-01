# dirsearch 使用指南

基于 python 开发的 dirsearch 可替代 windows 平台上类似御剑之类的目录扫描工具

该工具的原理其实就是根据字典文本构造 url 路径，然后构造请求报文逐个发送，只要响应报文的状态码符合条件（例如：301、200）那么就可以确定这个地址是可能存在目录的。项目在 github 上开源的，感兴趣可以大致看一下源码。

如果只想要傻瓜式操作，那么只需要看 `安装及简单使用` 这一栏即可

tips: 要求 python3.7 以上

### 安装及简单使用

```
pip3 install -r requirements.txt  # 安装下需要的依赖
python3 dirsearch.py -u [target_url]  # 攻击目标url地址，可以用 -u 参数指定多个
```

命令参数
----

这个脚本很多功能参数，但能用到的可能比较少，几乎都有默认配置，在 `default.conf` 中可以看到

### 版本以及帮助命令

```
--version               显示程序版本号并退出
-h               显示帮助信息并退出
```

### 常用命令参数

```
-u               攻击目标url地址，可以指定多个，通过逗号分隔
-l                url列表文件，比如你可以建一个 targets.txt，里面包含需要攻击的网址
-e               站点文件类型列表，如：php,asp，有默认配置：php,aspx,jsp,html,js，基本主流的格式都包含了
-X               不需要扫描的站点文件类型列表
-w               用指定爆破字典执行，若存在多个通过逗号分隔
-t               指定线程数
-i               仅现实指定的状态码，指定多个通过逗号分隔 
-x               不显示指定的状态码，指定多个通过逗号分隔 
--exclude-sizes=SIZES               不显示的响应包大小（Example: 123B,4KB）
--exclude-texts=TEXTS               不显示的响应包关键字 (Example: "Not found", "Error"）
-m               指定请求方式，默认GET
```

### 较为冷门配置

```
--cidr=CIDR               无类域间路由CIDR
--prefixes=PREFIXES               对字典中的每个项添加自定义前缀，比如字典中有个项是app，只要我指定 ~,+,=，那么就会爆破的字典项为 ~app、+app、=app，若存在多个通过逗号分隔
--suffixes=SUFFIXES               添加自定义后缀，同上若存在多个通过逗号分隔
-U               字典全部大写
-L               字典全部小写
-C               首字母大写
-d               指定HTTP request data
-H               设置HTTP请求头，Example: -H "Referer: example.com" -H "Accept: */*"
--random-user-agent=true/false               随机user-agent开关
--user-agent=USERAGENT               自定义用户凭证，比如 Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:97.0) Gecko/20100101 Firefox/97.0，可以伪造请求报文
--cookie=COOKIE               可以设置访问cookie
--exit-on-error               当出错直接关闭程序
--timeout=TIMEOUT               访问超时设置
-s DELAY, --delay=DELAY               请求间隔延迟/s，支持浮点数 Delay between requests (support float number)
--proxy=PROXY                指定访问代理，例如: localhost:8080, socks5://localhost:8088)
```

### 输出报告格式

```
--simple-report=OUTPUTFILE               简洁报告
--plain-text-report=OUTPUTFILE               纯文本格式报告
--json-report=OUTPUTFILE               json格式报告
--xml-report=OUTPUTFILE               xml格式报告
--markdown-report=OUTPUTFILE               markdown格式报告
```