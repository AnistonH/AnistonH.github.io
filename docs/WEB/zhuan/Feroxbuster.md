# Feroxbuster使用说明

> 原文链接：[【目录扫描】feroxbuster v2.10.2 字典整合版](https://get-shell.com/1565.html#hidden-box-comment)

> Feroxbuster 是一款强大的目录扫描工具，Feroxbuster 的主要功能是基于字典的目录扫描，并且默认使用 Seclists 字典进行使用！并且具有快速和高效的特点，采用了多线程的技术来加快扫描速度。还支持暂停交互式设置等！

官方文档：[feroxbuster  Documentation](https://get-shell.com/?golink=aHR0cHM6Ly9lcGkwNTIuZ2l0aHViLmlvL2Zlcm94YnVzdGVyLWRvY3MvZG9jcy8=)



### 快速使用

**注意事项**

1、扫描时注意是扫描**目录**还是**文件**，默认**扫描目录**。  
2、默认参数的扫描线程比较暴力，建议手动调整线程（不懂直接使用本文的优化参数）

*   官方文档：[Documentation](https://get-shell.com/?golink=aHR0cHM6Ly9lcGkwNTIuZ2l0aHViLmlvL2Zlcm94YnVzdGVyLWRvY3MvZG9jcy8=)
*   用默认参数扫描目标`<url>`，且**`仅扫描目录`**

```
feroxbuster --url <url>
```

*   限制同时扫描两个目录，线程 20，排除状态码 404， 扫描目标`<url>`且**`仅扫描目录`**

```
feroxbuster --scan-limit 2 -t 20 --filter-status 404 --url <url>
```

*   【ASP.NET】用默认参数扫描目标，**`且扫描目录和html,asp,aspx,ashx,asmx文件后缀`**

```
feroxbuster -x html,asp,aspx,ashx,asmx --url <url>
```

*   【JAVA】用默认参数扫描目标，**`且扫描目录和html,htm,jsp文件后缀`**

```
feroxbuster -x html,htm,jsp --url <url>
```

*   【PHP】用默认参数扫描目标，**`且扫描目录和html,PHP文件后缀`**

```
feroxbuster -x html,php --url <url>
```

*   默认添加了 SecLists 字典，无需手动添加，开箱即用！
*   **Linux 版本在首次运行、更新后，需要执行 install.sh 脚本：`bash install.sh`**
*   添加了提示 Windows 的命令提示脚本！**执行命令 start.bat 即可查看！**
