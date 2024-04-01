# Alpha 串、数组、广义表

> 编写于：2024-04-01



## 统计字符串中数字和字母的个数

请根据注释完成函数 count，实现统计字符串中数字（0-9）和字母（A-Z,a-z）出现的个数的功能。

**示例**

```
字符串："abc11def001010DJK"
```

**输出**

```
字母个数:9
数字个数:8
```



#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

//统计输入字符串中出现的A-Z及a-z字符的个数
//统计输入字符串中出现的0-9个数。
//s：输入的字符串
//pzm:指向保存A-Z和a-z个数的整型变量的指针 
//psz:指向保存0-9个数的整型变量的指针

void count(char s[], int *pzm, int *psz)
{

}

int main()
{
    char s[] = "abc11def001010DJK";
    int zm = 0; //字符串中A-Z或a-z字符的个数
    int sz = 0; //字符串中0-9的个数
    count(s,&zm,&sz); 

    printf("字母个数:%d\n数字个数:%d",zm,sz);

    return 0;
}
```

#### 答案

```c
#include <stdio.h>
#include <stdlib.h>

//统计输入字符串中出现的A-Z及a-z字符的个数
//统计输入字符串中出现的0-9个数。
//s：输入的字符串
//pzm:指向保存A-Z和a-z个数的整型变量的指针 
//psz:指向保存0-9个数的整型变量的指针

void count(char s[], int* pzm, int* psz) {
    // 初始化字母和数字的个数为 0
    *pzm = 0;
    *psz = 0;

    // 遍历字符串中的每个字符
    for (int i = 0; s[i] != '\0'; i++) {
        // 检查字符是否是字母
        if ((s[i] >= 'A' && s[i] <= 'Z') || (s[i] >= 'a' && s[i] <= 'z')) {
            (*pzm)++;
        }
        // 检查字符是否是数字
        if (s[i] >= '0' && s[i] <= '9') {
            (*psz)++;
        }
    }
}

int main() {
    char s[] = "abc11def001010DJK";
    int zm = 0; // 字符串中 A-Z 或 a-z 字符的个数
    int sz = 0; // 字符串中 0-9 的个数
    count(s, &zm, &sz);

    printf("字母个数: %d\n数字个数: %d", zm, sz);

    return 0;
}
```





## 字符串分割

请完成函数`part`，实现如下功能：给定字符串 s，按给定的长度 n 分割成两个字符串，字符串 s 的前 n 个字符存入 s1，其多余的字符存入 s2。

**示例**

```
字符串 s："abc123456789"
长度 n：5
```

**输出**

```
abc12
3456789
```



#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

void part(char *s, char *s1, char *s2, int n){
    //请在此处编写代码，实现题目要求
    
    
}

int main()
{
    char s[] = "abc123456789";
    char s1[10],s2[10];
    part(s,s1,s2,5); 
    printf("%s\n%s",s1,s2);
    return 0;
}
```

#### 答案

> 第一种方法，直接用`string.h`，注意`strncpy`和`strcpy`的区别。

> 区别如下：
>
> 1. 功能不同：
>    - `strncpy(destination, source, n)`：将源字符串 `source` 的前 `n` 个字符复制到目标字符串 `destination` 中。如果 `source` 的长度小于 `n`，则在复制完 `source` 的内容后，将剩余的字符用空字符 `\0` 填充。
>    - `strcpy(destination, source)`：将源字符串 `source` 的所有字符复制到目标字符串 `destination` 中，直到遇到空字符 `\0` 为止。
>
> 2. 长度控制不同：
>    - `strncpy` 允许指定要复制的字符个数 `n`，并且即使 `source` 的长度小于 `n`，也会在 `destination` 中填充空字符 `\0`，以保证 `destination` 的长度为 `n`。
>    - `strcpy` 没有指定要复制的字符个数，它会一直复制直到遇到 `source` 的空字符 `\0`，因此 `destination` 的长度取决于 `source` 的长度。
>
> 3. 安全性不同：
>    - `strncpy` 在复制 `source` 的前 `n` 个字符时，不会检查 `source` 的实际长度，因此可能出现未复制完整字符串或未填充足够的空字符的情况。这可能导致目标字符串 `destination` 不是一个有效的 C 字符串。
>    - `strcpy` 会复制源字符串 `source` 的所有字符，直到遇到空字符 `\0`。它会保证目标字符串 `destination` 是一个有效的 C 字符串，且以空字符结尾。
>
> 总的来说，`strncpy` 在需要控制复制字符个数的情况下比较有用，但需要小心处理字符串结束符的情况。而 `strcpy` 则是常用的字符串复制函数，适用于复制整个字符串的场景，并确保目标字符串是一个有效的 C 字符串。

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void part(char* s, char* s1, char* s2, int n) {
    // 将 s 的前 n 个字符复制到 s1
    strncpy(s1, s, n);
    s1[n] = '\0'; // 设置 s1 的末尾为字符串结束符

    // 将 s 中剩余的字符复制到 s2
    strcpy(s2, s + n);
}

int main() {
    char s[] = "abc123456789";
    char s1[10], s2[10];
    part(s, s1, s2, 5);
    printf("%s\n%s", s1, s2);
    return 0;
}
```

> 第二种方法，手动实现`string.h`功能。

```c
#include <stdio.h>

void part(char* s, char* s1, char* s2, int n) {
    int i;

    // 复制 s 的前 n 个字符到 s1
    for (i = 0; i < n && s[i] != '\0'; i++) {
        s1[i] = s[i];
    }
    s1[i] = '\0'; // 设置 s1 的末尾为字符串结束符

    // 复制 s 中剩余的字符到 s2
    for (int j = 0; s[i] != '\0'; i++, j++) {
        s2[j] = s[i];
    }
    s2[i - n] = '\0'; // 设置 s2 的末尾为字符串结束符
}

int main() {
    char s[] = "abc123456789";
    char s1[10], s2[10];
    int n = 5;
    part(s, s1, s2, n);
    printf("%s\n%s", s1, s2);
    return 0;
}
```





## 字符串的插入

请完成函数`insert`，实现如下功能：将字符串 t 插入到字符串 s 中，插入位置为 pos。

**示例**

```
字符串s："1234567890"
字符串t："abcd"
插入位置pos：6
```

**输出**

```
12345abcd67890
```



#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

void  insert(char *s,char *t,int pos){
	//请在此处编写代码,完成题目要求
    
    
}

int main()
{
    char s[20] = "1234567890";
    char t[20] = "abcd";
    insert(s,t,6);
    printf("%s",s);
    return 0;
}
```

#### 答案

```c
#include <stdio.h>
#include <stdlib.h>

void insert(char* s, char* t, int pos) {
    int len_s = 0;
    int len_t = 0;

    // 计算字符串 s 和 t 的长度
    while (s[len_s] != '\0') {
        len_s++;
    }
    while (t[len_t] != '\0') {
        len_t++;
    }

    // 因为首元素下标为‘1’
    pos--;

    // 这一段可有可无
    // 检查插入位置是否超出 s 的长度范围
    // 如果 pos 大于 len_s，则将 pos 设置为 len_s
    // 以确保插入位置不会超出字符串 s 的末尾。
    if (pos > len_s) {
        pos = len_s;
    }

    // 为 t 中的字符腾出空间，移动原来的字符向后
    for (int i = len_s; i >= pos; i--) {
        s[i + len_t] = s[i];
    }

    // 将 t 中的字符复制到插入位置开始的位置
    for (int i = 0; i < len_t; i++) {
        s[pos + i] = t[i];
    }

    // 在 s 的末尾添加字符串结束符
    s[len_s + len_t] = '\0';
}

int main() {
    char s[20] = "1234567890";
    char t[20] = "abcd";
    int pos = 6;
    insert(s, t, pos);
    printf("%s", s);
    return 0;
}
```





## KMP模式匹配

请编写模式匹配函数`KMP`，借助 KMP 算法来实现如下功能：如果匹配成功，则返回对应的位置；如果匹配不成功，则返回-1。

### 示例 1

**调用函数**

```
KMP("aabaabbaaabab","aaab")
```

**输出**

```
8
```

### 示例 2

**调用函数**

```
KMP("aabaabbaaabab","aaabbb")
```

**输出**

```
-1
```



#### 题目

```c
#include <stdio.h>
#include <string.h>

//匹配成功，则函数返回位置
int KMP(char * S,char * T){

}
int main() {
    int i=KMP("aabaabbaaabab","aaab");
    printf("%d",i);
    return 0;
}
```

#### 答案

> 构建`next`数组的时候`int next[len_t];`，VS会报错，“E0028：表达式必须含有常量值”。解决方法嘛，要么动态分配内存，要么就直接填个`next[100]`算了。
>
> 如果VS报错：“E0167：`const char *`类型的实参与`char *`类型的形参不兼容”，则请将文件后缀由`.cpp`改为`.c`。
>
> 
>
> 下面这个的`next[]`首元素是`-1`，和上课的`0`有些许区别。不过都一样。
>
> 这个计算的是“首元素下标为0”，但是题目要求的是“首元素是下标为1”，所以最后return的时候需要+1。

为了实现 KMP 算法来进行模式匹配，并返回匹配成功的位置或 -1，可以按照以下步骤完成 `KMP` 函数：

1. 计算模式字符串 `T` 的长度。
2. 创建辅助数组 `next`，用于存储模式字符串中每个位置前缀的最长可匹配前缀子串的结束位置。
3. 根据模式字符串 `T` 构建 `next` 数组。
   - 初始化 `next[0]` 为 -1。
   - 初始化两个指针 `i` 和 `j`，其中 `i = 0`，`j = -1`。
   - 通过比较 `T[i]` 和 `T[j]` 的字符来更新 `next[i+1]`：
     - 如果 `T[i]` 和 `T[j]` 相等，则将 `next[i+1]` 设置为 `next[j] + 1`，然后递增 `i` 和 `j`。
     - 如果 `T[i]` 和 `T[j]` 不相等，将 `j` 更新为 `next[j]` 的值，直到 `j` 为 -1 或 `T[i]` 和 `T[j]` 相等。然后将 `next[i+1]` 设置为 `j + 1`，然后递增 `i`。
4. 使用 `next` 数组进行字符串匹配。
   - 初始化两个指针 `i` 和 `j`，其中 `i = 0`，`j = 0`。
   - 通过比较 `S[i]` 和 `T[j]` 的字符进行匹配：
     - 如果 `S[i]` 和 `T[j]` 相等，则递增 `i` 和 `j`。
     - 如果 `S[i]` 和 `T[j]` 不相等：
       - 如果 `j` 为 0，说明当前字符不匹配且无法继续回退，将 `i` 递增。
       - 否则，将 `j` 更新为 `next[j]` 的值。
5. 根据匹配的结果返回相应的位置或 -1。

下面是完整的代码实现：

```c
#include <stdio.h>
#include <string.h>

int KMP(char *S, char *T) {
    int len_s = strlen(S);
    int len_t = strlen(T);

    // 构建 next 数组
    int next[len_t];
    next[0] = -1;
    int i = 0, j = -1;
    while (i < len_t - 1) {
        if (j == -1 || T[i] == T[j]) {
            i++;
            j++;
            next[i] = j;
        } else {
            j = next[j];
        }
    }

    // 使用 next 数组进行字符串匹配
    i = 0;
    j = 0;
    while (i < len_s && j < len_t) {
        if (j == -1 || S[i] == T[j]) {
            i++;
            j++;
        } else {
            j = next[j];
        }
    }

    if (j == len_t) {
        return i - j + 1; // 返回匹配成功的位置
    } else {
        return -1; // 匹配不成功
    }
}

int main() {
    int result = KMP("aabaabbaaabab", "aaab");
    printf("%d\n", result);

    result = KMP("aabaabbaaabab", "aaabbb");
    printf("%d\n", result);

    return 0;
}
```

运行以上代码，将输出：

```
8
-1
```





## 统计字符串中字符频度

请完成函数`Account_char_count`，该函数接受一个字符串参数，统计字符串中各个字符的频度，并输出每个字符及其频度。（字符串的合法字符为`A ~ Z`这`26`个大写字母和`0 ~ 9`这`10`个数字，其它字符忽略）。

### 示例

**输入**

```
请输入待统计的字符串（合法字符为A~Z和0~9）：123321ASDDSA
```

**输出**（请按此格式输出）

```
1的频度为：2
2的频度为：2
3的频度为：2
A的频度为：2
S的频度为：2
D的频度为：2
```



#### 题目

```c
#include <stdio.h>
#define SIZE 1000

void Account_char_count(char str[]){
     //请在此处编写代码
    
	 
}

int main(){
     int i=0;
     char str[SIZE];
     printf("请输入待统计的字符串（合法字符为A~Z和0~9）：");
     while((str[i]=getchar())!='\n')
         i++;
     str[i]='\0';
     Account_char_count(str);
} 
```

#### 答案

> 先看看我第一次写的，通不过检查，因为输入顺序是按照ABCD顺序，而非题目输入顺序。

```c
#include <stdio.h>
#define SIZE 1000

void Account_char_count(char str[]) {
    int count[36] = { 0 };  // 用于统计字符频度的数组，下标 0-25 对应 A-Z，下标 26-35 对应 0-9

    for (int i = 0; str[i] != '\0'; i++) {
        char c = str[i];
        if (c >= 'A' && c <= 'Z') {
            count[c - 'A']++;
        }
        else if (c >= '0' && c <= '9') {
            count[c - '0' + 26]++;
        }
    }

    for (int i = 26; i < 36; i++) {
        if (count[i] != 0) {
            printf("%c的频度为：%d\n", '0' + i - 26, count[i]);
        }
    }

    for (int i = 0; i < 26; i++) {
        if (count[i] != 0) {
            printf("%c的频度为：%d\n", 'A' + i, count[i]);
        }
    }

}

int main() {
    int i = 0;
    char str[SIZE];

    printf("请输入待统计的字符串（合法字符为A~Z和0~9）：");
    while ((str[i] = getchar()) != '\n') {
        i++;
    }
    str[i] = '\0';

    Account_char_count(str);

    return 0;
}
```

> 按照输入顺序输出。

```c
#include <stdio.h>
#include <string.h>
#define SIZE 1000

void Account_char_count(char str[]) {
    int count[36] = { 0 };  // 用于统计字符频度，下标 0-25 对应 A-Z，下标 26-35 对应 0-9
    int visited[36] = { 0 };  // 记录字符是否已经输出过，0表示没有输出过，1表示输出过

    // 统计字符频度
    for (int i = 0; str[i] != '\0'; i++) {
        char c = str[i];

        if (c >= 'A' && c <= 'Z') {
            count[c - 'A']++;
        }
        else if (c >= '0' && c <= '9') {
            count[c - '0' + 26]++;
        }
    }

    // 输出
    for (int i = 0; str[i] != '\0'; i++) {
        char c = str[i];

        // 这里使用了一个额外的数组 visited 来记录字符是否已经输出过，避免重复输出相同字符的频度。
        // 在遍历字符串时，如果遇到一个字符是第一次遇到的字符，则输出其频度，并将对应的 visited 数组标记为已输出。
        // 这样可以保证按照输入字母的顺序输出字符及其频度。
        if (c >= 'A' && c <= 'Z' && !visited[c - 'A']) {
            visited[c - 'A'] = 1;

            printf("%c的频度为：%d\n", c, count[c - 'A']);
        }
        else if (c >= '0' && c <= '9' && !visited[c - '0' + 26]){
            visited[c - '0' + 26] = 1;

            printf("%c的频度为：%d\n", c, count[c - '0' + 26]);
        }
    }
}

int main() {
    int i = 0;
    char str[SIZE];

    printf("请输入待统计的字符串（合法字符为A~Z和0~9）：");
    while ((str[i] = getchar()) != '\n') {
        i++;
    }
    str[i] = '\0';

    Account_char_count(str);

    return 0;
}
```





## 将一段字符插入到另一段字符中的指定位置

编写算法，实现下面函数的功能。函数`void insert(char *s, char *t, int pos)`将字符串`t`插入到字符串`s`中，插入位置为`pos`，插入后字符之间不能有空位置。假设分配给字符串`s`的空间足够让字符串`t`插入。

### 示例

**输入**

```
请输入字符串s：123

请输入字符串t：asd

输入插入的位置（位置从1开始）：2
```

**输出**

```
字符串插入后的新字符串为：1asd23
```



#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

#define SIZE1 200
#define SIZE2 50

void insert(char *s, char *t, int pos){
     //请在此处编写代码
	
}

int main(){ 

    char s[SIZE1], t[SIZE2];
    int pos;

    printf("请输入字符串s：");
    scanf("%s", s); 
    printf("\n请输入字符串t：");
    scanf("%s", t); 

    printf("\n输入插入的位置（位置从1开始）：");
    scanf("%d", &pos);
    printf("\n字符串插入后的新字符串为：");
    insert(s, t, pos);
    return 0;
}
```

#### 答案

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define SIZE1 200
#define SIZE2 50

void insert(char* s, char* t, int pos) {
    int s_len = strlen(s);
    int t_len = strlen(t);
    int new_len = s_len + t_len;
    int i, j;

    pos--; // 设置首元素下标为‘1’

    // 向右移动原字符串s中的字符，给插入的字符串t腾出空间
    for (i = s_len; i >= pos; i--) {
        s[i + t_len] = s[i];
    }

    // 将字符串t插入到字符串s中
    for (j = 0; j < t_len; j++) {
        s[pos + j] = t[j];
    }

    // 输出插入后的新字符串
    for (i = 0; i < new_len; i++) {
        printf("%c", s[i]);
    }
}

int main() {
    char s[SIZE1], t[SIZE2];
    int pos;

    printf("请输入字符串s：");
    scanf("%s", s);
    printf("\n请输入字符串t：");
    scanf("%s", t);

    printf("\n输入插入的位置（位置从1开始）：");
    scanf("%d", &pos);
    printf("\n字符串插入后的新字符串为：");
    insert(s, t, pos);
    return 0;
}
```





## 将数组中的正数排在负数前面

设任意`n`个整数存放于数组`A[1...n]`中，试编写算法，完善函数`Change`，将所有正数排在所有负数前面（要求算法复杂度为`O(n)`）。

### 示例

**输入**

```
请输入数组中元素个数：3

请输入第1个元素个数：-1

请输入第2个元素个数：2

请输入第3个元素个数：3
```

**输出**

```
调整次序后的元素序列为：3 2 -1
```



#### 题目

```c
#include <stdio.h>
#define MAX 100
 
void Change(int A[],int n){
     //请在此处编写代码
    
}
 
int main(){
    int A[MAX],i,n;
    printf("请输入数组中元素个数：");
    scanf("%d",&n);
    for(i=0;i<n;i++){
        printf("\n请输入第%d个元素个数：",i+1);
        scanf("%d",&A[i]);
     }
	
    Change(A,n);
    printf("\n调整次序后的元素序列为：");
    for(i=0;i<n;i++)
        printf("%d ",A[i]);
    return 0;
}
```

#### 答案

> 使用两个指针来实现将正数排在负数前面的功能。一个指针从数组的开头开始，另一个指针从数组的末尾开始。然后，交换两个指针指向的元素，直到两个指针相遇。这样，正数就会排在负数前面。
>
> 算法的时间复杂度是 O(n)，因为我们只需要遍历数组一次来将正数排在负数前面。

> **直接上POE吧：**
>
> 1. 初始化左指针 `left` 为数组的开头（索引为 0），右指针 `right` 为数组的末尾（索引为 `n - 1`）。
>
> 2. 进入循环，条件是左指针小于右指针。这是因为当左指针和右指针相遇时，意味着已经完成了所有元素的调整，不再需要继续交换。
>
> 3. 在循环中，左指针从数组的开头开始向右移动，直到找到第一个负数。这是通过检查 `A[left] > 0` 来判断的。如果 `A[left]` 是正数，就继续向右移动左指针。这样，左指针最终会指向第一个负数或者数组的末尾。
>
> 4. 在循环中，右指针从数组的末尾开始向左移动，直到找到第一个正数。这是通过检查 `A[right] < 0` 来判断的。如果 `A[right]` 是负数，就继续向左移动右指针。这样，右指针最终会指向第一个正数或者数组的开头。
>
> 5. 如果左指针小于右指针，说明找到了需要交换的元素。在这种情况下，交换左指针和右指针指向的元素。这样就将一个正数放在了负数的前面。
>
> 6. 重复步骤 3 到 5，直到左指针和右指针相遇，完成所有元素的调整。
>
> 这个算法的关键在于通过左指针寻找负数，右指针寻找正数，然后交换它们的位置。通过不断向中间移动指针，并在需要交换时执行交换操作，最终可以将所有正数排在负数前面。
>

```c
#include <stdio.h>

#define MAX 100

void Change(int A[], int n) {
    int left = 0;  // 左指针
    int right = n - 1;  // 右指针

    while (left < right) {
        // 左指针找到第一个负数
        while (left < right && A[left] > 0) {
            left++;
        }

        // 右指针找到第一个正数
        while (left < right && A[right] < 0) {
            right--;
        }

        // 交换左右指针指向的元素
        if (left < right) {
            int temp = A[left];
            A[left] = A[right];
            A[right] = temp;
        }
    }
}

int main() {
    int A[MAX], i, n;
    printf("请输入数组中元素个数：");
    scanf("%d", &n);
    for (i = 0; i < n; i++) {
        printf("\n请输入第%d个元素个数：", i + 1);
        scanf("%d", &A[i]);
    }

    Change(A, n);
    printf("\n调整次序后的元素序列为：");
    for (i = 0; i < n; i++)
        printf("%d ", A[i]);
    return 0;
}
```





## 字符串逆置

请编写代码，实现字符串的逆置操作。`Char_Reverse`是一个递归函数，该函数接受一个字符数组，通过递归方式实现字符串的逆置，并将逆置后的字符串存储在原数组中。

注意：逆置的字符串以字符’#'作为结束标志。字符数组的长度不超过预定义的最大长度。

### 示例

**输入**

```
请输入一个字符串：asdfg#
```

**输出**

```
字符串逆置之后为：gfdsa
```



#### 题目

```c
#include <stdio.h>
#define MAXSIZE 100
 
void Char_Reverse(char array[]){
    //请在此处编写代码

}
 
int main(){
    char array[MAXSIZE];
    printf("请输入一个字符串（以“#”结束）：");
    Char_Reverse(array);
    printf("字符串逆置之后为：%s",array);
    return 0;
}
```

#### 答案

> 先来个错误答案，没有按照题目进行递归。

```c
#include <stdio.h>
#include <string.h>
#define MAXSIZE 100

void swap(char* a, char* b) {
    char temp = *a;
    *a = *b;
    *b = temp;
}

void Char_Reverse(char array[]) {
    int len = strlen(array);
    int start = 0;
    //int end = len - 1; // 这种情况会让‘#’也参与到交换
    int end = len - 2; // 这种情况不会让‘#’参与到交换

    while (start < end) {
        if (array[start] == '#') {
            break;  // 遇到结束标志'#'，停止逆置
        }
        swap(&array[start], &array[end]);

        start++;
        end--;
    }

    array[len - 1] = '\0'; // 修改‘#’为‘\0’，因为示例中就没有输出‘#’
}

int main() {
    char array[MAXSIZE];
    printf("请输入一个字符串（以“#”结束）：");
    scanf("%s", array);
    Char_Reverse(array);
    printf("字符串逆置之后为：%s", array);
    return 0;
}
```

> 下面这个是递归，但是还是通不过Alpha检查，感觉是平台问题。

```c
#include <stdio.h>
#include <string.h>
#define MAXSIZE 100

void reverseHelper(char array[], int start, int end) {
    if (start >= end) {
        return;  // 递归结束条件：start >= end
    }

    char temp = array[start];
    array[start] = array[end];
    array[end] = temp;

    reverseHelper(array, start + 1, end - 1);  // 递归调用，逆置剩余部分
}

void Char_Reverse(char array[]) {
    int len = strlen(array);
    reverseHelper(array, 0, len - 2); // 减2还是为了不让‘#’参与

    array[len - 1] = '\0'; // 修改‘#’为‘\0’，因为示例中就没有输出‘#’
}

int main() {
    char array[MAXSIZE];
    printf("请输入一个字符串（以“#”结束）：");
    scanf("%s", array);
    Char_Reverse(array);
    printf("字符串逆置之后为：%s", array);
    return 0;
}
```

> 正确答案来了！！！！
>
> 需要改一下`main()`，要不然真的不知道怎么递归。

```c
#include <stdio.h>
#include <string.h>

#define MAXSIZE 100

void Char_Reverse(char array[], int start, int end) {
    if (start >= end) {
        return;
    }

    // 交换首尾字符
    char temp = array[start];
    array[start] = array[end];
    array[end] = temp;

    // 递归处理剩余部分
    Char_Reverse(array, start + 1, end - 1);
}

int main() {
    char array[MAXSIZE];
    printf("请输入一个字符串（以 '#' 结束）：");
    fgets(array, MAXSIZE, stdin);

    // 移除换行符
    array[strcspn(array, "\n")] = '\0';

    // 计算字符串长度
    int length = strlen(array);

    // 逆置字符串（不包括 '#'）
    Char_Reverse(array, 0, length - 1);

    printf("字符串逆置之后为：%s\n", array);
    return 0;
}
```

> **关于`strcspn()`函数：**
>
> `strcspn` 函数是 C 语言中的一个字符串函数，其原型定义在 `<string.h>` 头文件中。
>
> 函数签名如下：
>
> ```c
> size_t strcspn(const char *str1, const char *str2);
> ```
>
> 该函数用于计算字符串 `str1` 开头连续不包含字符串 `str2` 中任何字符的字符数，也即返回 `str1` 中第一个匹配 `str2` 中任何字符的位置的索引。
>
> 具体来说，`strcspn` 函数会从 `str1` 的开头开始逐个字符比较，直到遇到 `str2` 中的任意字符，或者到达 `str1` 的结尾为止。返回的值是 `str1` 开头连续不包含 `str2` 中任何字符的字符数，即第一个匹配的字符在 `str1` 中的索引位置。
>
> 下面是一个示例来说明 `strcspn` 函数的使用：
>
> ```c
> #include <stdio.h>
> #include <string.h>
> 
> int main() {
>     char str1[] = "Hello, World!";
>     char str2[] = "Wod";
> 
>     size_t index = strcspn(str1, str2);
> 
>     printf("第一个匹配字符的索引位置：%zu\n", index);
> 
>     return 0;
> }
> ```
>
> 输出结果为：
>
> ```
> 第一个匹配字符的索引位置：6
> ```
>
> 在上面的示例中，`str1` 是要搜索的字符串，`str2` 是要匹配的字符集合。函数返回的是 `str1` 中第一个匹配 `str2` 中任何字符的位置的索引。在这个示例中，字符 'W' 在 `str1` 中的索引位置为 6。





## 复制字符串

请编写代码，完善函数`copy`，实现将字符串 s2 的全部字符复制到字符串 s1 中。该函数接受两个字符串参数 s1 和 s2，将字符串 s2 的全部字符复制到字符串 s1 中，并输出复制后的字符串 s1。

注意：禁止使用strcpy函数实现复制字符串功能。

### 示例

**输入**

```
请输入s1中的字符串：123
请输入s2中的字符串：asd
```

**输出**

```
复制之后字符串s1为：asd
```



#### 题目

```c
#include <stdio.h>
#define N 100 

void copy(char s1[], char s2[]){
    //请在此处编写代码
    

} 

int main(){
    char s1[N], s2[N];
    printf("请输入s1中的字符串：");
    scanf("%s", s1);
    printf("请输入s2中的字符串：");
    scanf("%s", s2);
    copy(s1, s2);
    return 0;
}
```

#### 答案

```c
#include <stdio.h>
#define N 100

void copy(char s1[], char s2[]) {
    int i = 0;

    // 复制字符串s2到s1
    while (s2[i] != '\0') {
        s1[i] = s2[i];
        i++;
    }

    s1[i] = '\0'; // 添加字符串结束符

    printf("复制之后字符串s1为：%s\n", s1);
}

int main() {
    char s1[N], s2[N];
    printf("请输入s1中的字符串：");
    scanf("%s", s1);
    printf("请输入s2中的字符串：");
    scanf("%s", s2);
    copy(s1, s2);
    return 0;
}
```





## 分离数组中的奇数和偶数

分离数组中的奇数和偶数，存放在原数组中。
请编写代码，完善函数`Separate`，实现将数组中的奇数和偶数分开的操作。该函数接受一个整数数组 arr 和数组的长度 length，将数组中的奇数和偶数分开，并按照奇数在前、偶数在后的顺序重新排列数组。

### 示例

**输入**

```
main()函数中设置arr[] = { 3, 2, 3, 4, 5, 9, 7, 8, 9 };
```

**输出**

```
变换次序后的数组为：9 7 9 5 3 3 8 4 2
```



#### 题目

```C
#include <stdio.h>
#include <stdlib.h>

#define MAX 10

void Separate(int arr[],int length){
     //请在此处编写代码
	
} 

int main(){
     int arr[] = { 3, 2, 3, 4, 5, 9, 7, 8, 9 };
     int length = sizeof(arr) / sizeof(arr[0]);//计算数组的长度 
	
     // printf("变换次序后的数组为：");
     Separate(arr,length);
     for (int i = 0; i < length; i++){
         printf("%d ", arr[i]);//输出数组 
     }
     exit(0);//执行完正常退出 
     return 0;
}
```

#### 答案

> 先看一段错误代码，用的方法和上边的“将数组中的整数排在负数前面”，不知道为什么过不了检查。问题在于输出的顺序（完成了分离奇数偶数，但是奇数偶数中的顺序不对）

```c
#include <stdio.h>
#include <stdlib.h>

void Separate(int arr[], int length) {
    int i = 0;
    int j = length - 1;

    while (i < j) {
        // 从前往后找到第一个偶数
        while (i < j && arr[i] % 2 != 0) {
            i++;
        }

        // 从后往前找到第一个奇数
        while (i < j && arr[j] % 2 == 0) {
            j--;
        }

        // 交换偶数和奇数的位置
        if (i < j) {
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
}

int main() {
    int arr[] = { 3, 2, 3, 4, 5, 9, 7, 8, 9 };
    int length = sizeof(arr) / sizeof(arr[0]);

    printf("变换次序后的数组为：");
    Separate(arr, length);
    for (int i = 0; i < length; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    return 0;
}
```

> ~~正确答案~~（bushi
>
> 能过检查点的答案。（不难看出，我对这个题非常无语）

```c
#include <stdio.h>
#include <stdlib.h>

void Reverse(int arr[], int start, int end) {
    while (start < end) {
        int temp = arr[start];
        arr[start] = arr[end];
        arr[end] = temp;
        start++;
        end--;
    }
}

void Separate(int arr[], int length) {
    int oddCount = 0; // 奇数计数器

    // 统计奇数的个数
    for (int i = 0; i < length; i++) {
        if (arr[i] % 2 != 0) {
            oddCount++;
        }
    }

    // 复制原数组到临时数组tmpArr
    int* tmpArr = (int*)malloc(length * sizeof(int));
    for (int i = 0; i < length; i++) {
        tmpArr[i] = arr[i];
    }

    int oddIndex = 0; // 奇数部分的索引
    int evenIndex = oddCount; // 偶数部分的索引

    // 将奇数和偶数分别放置到对应的位置
    for (int i = 0; i < length; i++) {
        if (tmpArr[i] % 2 != 0) {
            arr[oddIndex++] = tmpArr[i];
        }
        else {
            arr[evenIndex++] = tmpArr[i];
        }
    }

    // 对奇数部分进行逆序
    Reverse(arr, 0, oddCount - 1);
    // 对偶数部分进行逆序
    Reverse(arr, oddCount, length - 1);
    
    free(tmpArr);
}

int main() {
    int arr[] = { 3, 2, 3, 4, 5, 9, 7, 8, 9 };
    int length = sizeof(arr) / sizeof(arr[0]);

    printf("变换次序后的数组为：");
    Separate(arr, length);
    for (int i = 0; i < length; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    return 0;
}
```





## 判断三维数组中是否存在鞍点

判断三维数组中是否存在鞍点，如果存在则输出。（鞍点：一行中的最大值，并且也是一列中的最小值）
请编写代码，完善函数`Find`，实现在给定的 3x3 数组中找到鞍点。鞍点是指在数组中的某个元素，它在所在行上是最大值，在所在列上是最小值。该函数接受一个二维数组 arr 和数组的行数 row 和列数 col，查找数组中的鞍点。

### 要求

```
1、如果鞍点不存在，则输出 “该数组中不存在鞍点！”。（双引号中的内容）
2、如果鞍点存在，则输出 “该数组中存在鞍点arr[%d][%d] = %d”。（%d代表相应的数据）
```

### 示例

**输入**

```
请输入数组：
7 8 3
5 7 5
1 8 3
```

**输出**

```
该数组中存在鞍点arr[1][1] = 7 （第2行第2列）
```



#### 题目

```C
#define ROW 3
#define COL 3       

#include <stdio.h>
#include <stdlib.h>

void Find(int arr[][COL], int row, int col){
     //请在此处编写代码
    
}

int main(){
     int arr[ROW][COL] = { 0 };
     Find(arr, ROW, COL);               
     return 0;
}
```

#### 答案

> 有点难啊这题。
>
> 下面这段代码，在`Visual Studio`上完全正确，但是就是通不过检查。

```c
#include<stdio.h>

#define ROW 3
#define COL 3                          

void Find(int arr[][COL], int row, int col) {
    int i, j;
    int max_row, min_col;
    int saddle_point_found = 0; // 标记是否找到了鞍点

    // 遍历每一行
    for (i = 0; i < row; i++) {
        // 找到当前行中的最大值
        max_row = arr[i][0];
        for (j = 1; j < col; j++) {
            if (arr[i][j] > max_row) {
                max_row = arr[i][j];
            }
        }

        // 找到当前行中最大值对应的列索引
        for (j = 0; j < col; j++) {
            if (arr[i][j] == max_row) {
                // 检查该列中的元素是否是最小值
                min_col = arr[0][j];
                for (int k = 0; k < row; k++) {
                    if (arr[k][j] < min_col) {
                        min_col = arr[k][j];
                    }
                }

                // 如果当前行中的最大值也是所在列的最小值，则找到了鞍点
                if (min_col == max_row) {
                    printf("该数组中存在鞍点arr[%d][%d] = %d\n", i, j, max_row);
                    saddle_point_found = 1;
                }
                break; // 不再继续搜索该行
            }
        }
    }

    // 如果没有找到鞍点
    if (!saddle_point_found) {
        printf("该数组中不存在鞍点！\n");
    }
}

int main() {
    int arr[ROW][COL] = { {7, 8, 3},
                          {5, 7, 5},
                          {1, 8, 3} };
    Find(arr, ROW, COL);
    return 0;
}
```

> 破案了，又是`main()`函数造成的问题，改一下主函数就OK了。
>
> 我真的对这平台挺无语。

```c
#include <stdio.h>

#define ROW 3
#define COL 3                          

void Find(int arr[][COL], int row, int col) {
    int i, j;
    int max_row, min_col;
    int flag = 0; // 标记是否找到了鞍点

    // 遍历每一行
    for (i = 0; i < row; i++) {
        // 找到当前行中的最大值
        max_row = arr[i][0];
        for (j = 1; j < col; j++) {
            if (arr[i][j] > max_row) {
                max_row = arr[i][j];
            }
        }
        
        for (j = 0; j < col; j++) {
            // 找到当前行中最大值对应的列索引
            if (arr[i][j] == max_row) {

                // 检查该列中的元素是否是最小值
                min_col = arr[0][j];
                for (int k = 0; k < row; k++) {
                    if (arr[k][j] < min_col) {
                        min_col = arr[k][j];
                    }
                }

                // 如果当前行中的最大值也是所在列的最小值，则找到了鞍点
                if (min_col == max_row) {
                    printf("该数组中存在鞍点arr[%d][%d] = %d\n", i, j, max_row);
                    flag = 1;

                    break; // 不再继续搜索该行
                }
            }
        }
    }

    // 如果没有找到鞍点
    if (!flag) {
        printf("该数组中不存在鞍点！\n");
    }
}

int main() {
    int arr[ROW][COL] = { 0 };

    printf("请输入数组：\n");
    int i, j;
    for (i = 0; i < ROW; i++) {
        for (j = 0; j < COL; j++) {
            scanf("%d", &arr[i][j]);
        }
    }

    Find(arr, ROW, COL);
    return 0;
}
```

> **还是上POE吧：**
>
> 这段代码是一个完整的程序，用于接收用户输入的3x3整数数组，并判断是否存在鞍点。
>
> 首先，我们定义了常量`ROW`和`COL`，分别表示数组的行数和列数。在这个例子中，我们将其设置为3。
>
> 接下来，我们定义了一个函数`Find`，用于查找鞍点。该函数接受一个二维数组`arr`、数组的行数`row`和列数`col`作为参数。
>
> 在`Find`函数中，我们使用了两个嵌套的循环来遍历数组。外层循环用于遍历每一行，内层循环用于遍历每一列。
>
> 在外层循环中，我们首先找到当前行中的最大值`max_row`，并在内层循环中判断是否存在最小值`min_col`。如果当前行中的最大值也是所在列的最小值，则找到了鞍点。我们将其输出，并将`鞍点找到`标志`flag`设置为1。
>
> 若没有找到鞍点，则通过判断`flag`的值是否为0来输出"该数组中不存在鞍点！"的信息。
>
> 在`main`函数中，我们声明了一个名为`arr`的二维数组，用于存储用户输入的数组。然后，我们使用两个嵌套的循环来接收用户输入的值，并将其存储在`arr`数组中。
>
> 最后，我们调用`Find`函数，并将`arr`数组、`ROW`和`COL`作为参数传递给它，以便查找鞍点。
>
> 通过这段代码，用户可以输入一个3x3的整数数组，并根据输入的数组判断是否存在鞍点。如果存在鞍点，将输出鞍点的位置和值；如果不存在鞍点，将输出"该数组中不存在鞍点！"的提示信息。

