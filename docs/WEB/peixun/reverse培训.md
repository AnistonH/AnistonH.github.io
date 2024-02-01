# reverse培训

1. 了解不同进制之间的转化方法

```
二进制
十进制 -> 二进制
二进制 -> 十进制
十六进制
十进制 -> 十六进制
十六进制 -> 十进制
```

2. 了解字长与端序

3. 了解汇编基础指令（完成下面练习）

4. 下载工具ida，exeinfope/die（二选一）完成`BUUCTF`,`Reverse`模块前三题

5. 了解**函数调用栈与调用约定**的学习资料：

   [C语言函数调用栈(一) - clover_toeic - 博客园 (cnblogs.com)](https://www.cnblogs.com/clover-toeic/p/3755401.html)

   [C语言函数调用栈(二) - clover_toeic - 博客园 (cnblogs.com)](https://www.cnblogs.com/clover-toeic/p/3756668.html)

6. 初步了解x86架构：

   什么是**冯诺依曼体系结构**

   什么是寄存器

   什么是堆

   什么是栈

   了解进程空间布局（大致了解即可）

   

   

   练习：

   ```
   .text:000011B1 								var_4= dword ptr -4
   .text:000011B1 								arg_0= dword ptr 8
   .text:000011B1 55 							push ebp
   .text:000011B2 89 E5 						mov ebp, esp
   .text:000011B4 83 EC 10 					sub esp, 10h
   .text:000011C1 C7 45 FC 00 00 00 00 		mov [ebp+var_4], 0
   .text:000011C8 83 7D 08 00 					cmp [ebp+arg_0], 0
   .text:000011CC 79 09 						jns short loc_11D7
   .text:000011CC
   .text:000011CE C7 45 FC 01 00 00 00 		mov [ebp+var_4], 1
   .text:000011D5 EB 34 						jmp short loc_120B
   .text:000011D5
   .text:000011D7								loc_11D7:
   .text:000011D7 83 7D 08 00 					cmp [ebp+arg_0], 0
   .text:000011DB 75 09 						jnz short loc_11E6
   .text:000011DB
   .text:000011DD C7 45 FC 02 00 00 00 		mov [ebp+var_4], 2
   .text:000011E4 EB 25 						jmp short loc_120B
   .text:000011E4
   .text:000011E6 								loc_11E6:
   .text:000011E6 83 7D 08 1D 					cmp [ebp+arg_0], 1Dh
   .text:000011EA 7F 09 						jg short loc_11F5
   .text:000011EA
   .text:000011EC C7 45 FC 03 00 00 00 		mov [ebp+var_4], 3
   .text:000011F3 EB 16						jmp short loc_120B
   .text:000011F3
   .text:000011F5								loc_11F5:
   .text:000011F5 83 7D 08 45 					cmp [ebp+arg_0], 45h
   .text:000011F9 7F 09 						jg short loc_1204
   .text:000011F9
   .text:000011FB C7 45 FC 04 00 00 00			mov [ebp+var_4], 4
   .text:00001202 EB 07 						jmp short loc_120B
   .text:00001202
   .text:00001204 								loc_1204:
   .text:00001204 C7 45 FC 05 00 00 00 		mov [ebp+var_4], 5
   .text:00001204
   .text:0000120B 								loc_120B:
   .text:0000120B 8B 45 FC 					mov eax, [ebp+var_4]
   .text:0000120E C9 							leave
   .text:0000120F C3 							retn
   ```

   main 函数按照如下方式进行调用：

   ```
   .text:00001221 6A 3C						push 3Ch
   .text:00001223 E8 85 FF FF FF				call test1
   .text:00001228 83 C4 04 					add esp, 4
   ```

   1. 该函数有几个参数
   2. 该函数使用了什么函数调用约定
   3. 当程序执行到 0x1228 时，eax 的值是多少
   4. 你能还原出 C 代码吗

   

   

