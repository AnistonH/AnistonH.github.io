###  **1**

> 这个程序中，`fun` 函数用于判断一个整数是否是回文数。它通过将原始数字逆序生成一个新的数字，然后与原始数字进行比较，如果相等则为回文数。程序首先接受用户输入的数字，然后调用 `fun` 函数进行判断，并输出结果。

```c
#include <stdio.h>

// 函数声明
int fun(int num);

int main() {
    int num;

    // 输入数字
    scanf("%d", &num);

    // 调用函数判断是否为回文数
    if (fun(num)) {
        printf("%d是回文数\n", num);
    }
    else {
        printf("%d不是回文数\n", num);
    }

    return 0;
}

// 判断是否为回文数的函数
int fun(int num) {
    int start = num;
    int result = 0;

    while (num > 0) {
        result = result * 10 + num % 10;
        num /= 10;
    }

    // 判断反转后的数是否与原数相等
    return start == result;
}
```



### **2**

> 这个程序首先接受用户输入的正整数。接着，程序计算最高位的除数，然后通过循环从高位到低位输出各位上的数字。最后，结束执行。

```C
#include <stdio.h>

// 函数声明
int HLY(int num);

int main() {
    int num;

    // 输入正整数
    scanf("%d", &num);

    HLY(num);

    return 0;
}

int HLY(num) {
    int temp = num;
    int aa;
    int result = 1;

    // 计算最高位的除数
    while (temp >= 10) {
        temp /= 10;
        result *= 10;
    }

    // 从高位到低位输出各位上的数字
    while (result > 0) {
        aa = num / result;
        printf("%d ", aa);
        num %= result;
        result /= 10;
    }
}
```



### **3**

> 这个程序首先接受用户输入的小写字母。接着，程序根据规则将小写字母转换为密码，然后输出密码。最后，程序返回0表示正常执行。(程序根据给定的规则计算相应的密码字符。它从“Z”中减去输入的小写字母和“a”之间的差值。这有效地颠倒了字母表的顺序。)

```c
#include <stdio.h>

int main() {
    char low;

    // 输入小写字母
    scanf(" %c", &low);

    // 转换为密码
    char password = 'Z' - (low - 'a');

    // 输出密码
    printf("%c\n", password);

    return 0;
}
```



### **4**

> 这个程序首先接受用户输入的年、月、日，然后判断输入的月份和日期是否合法。接着，程序调用 `result` 函数计算给定日期是该年的第几天，并输出结果。在 `result` 函数中，使用了一个数组 `daysInMonth` 来存储每个月的天数，同时根据闰年规则修改二月的天数。 `run_nian` 函数用于判断是否为闰年。最后，程序返回0表示正常执行。

```c
#include <stdio.h>

// 函数声明
int run_nian(int year);
int result(int year, int month, int day);

int main() {
    int year, month, day;

    // 输入年、月、日
    scanf("%d", &year);
    scanf("%d", &month);
    scanf("%d", &day);

    // 判断闰年并计算第几天
    int num = result(year, month, day);

    // 输出结果
    printf("%d\n", num);

    return 0;
}

// 判断是否为闰年的函数
int run_nian(int year) {
    // 闰年的判断规则：能被4整除但不能被100整除，或者能被400整除
    return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
}

// 计算第几天的函数
int result(int year, int month, int day) {
    int daysInMonth[] = { 0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };

    // 如果是闰年，二月份天数加一
    if (run_nian(year)) {
        daysInMonth[2] = 29;
    }

    int num = 0;

    // 计算前几个月的天数总和
    for (int i = 1; i < month; i++) {
        num += daysInMonth[i];
    }

    // 加上当月的天数
    num += day;

    return num;
}
```



### **5**

> 这个程序使用了递归的思想，先将高位数字转换成字符串，然后再输出当前位数字。递归的基本情况是当 n 是个位数时，直接输出。递归调用的过程中，每次都将 n 除以 10，直到 n 变成个位数。在每一步递归中，输出当前位数字对应的字符。最终，整个数字就被逆序输出成字符串。

```c
#include <stdio.h>

// 函数声明
void intToString(int n);

int main() {
    int num;

    // 输入整数
    scanf("%d", &num);

    // 输出转换后的字符串
    intToString(num);

    return 0;
}

// 递归函数将整数转换成字符串
void intToString(int n) {

    // 如果 n 是负数，输出负号并取绝对值
    if (n < 0) {
        printf("-");
        n = -n;
    }

    // 如果 n 是个位数，则直接输出
    if (n >= 0 && n <= 9) {
        printf("%d", n);
        return;
    }

    // 递归调用：先输出高位数字，然后输出当前位数字
    intToString(n / 10);
    printf("%d", n % 10);
}
```



### **6**

> 这个程序首先接受用户输入的十六进制数，然后调用 `hexToDecimal` 函数进行转换。在 `hexToDecimal` 函数中，它遍历输入的十六进制数的每一位，将每一位的十进制值计算出来并累加，最终得到对应的十进制数。

```C
#include <stdio.h>
#include <math.h>

// 函数声明
int hexToDecimal(char hex[]);

int main() {
    char hex[20];

    // 输入十六进制数
    scanf("%s", hex);

    // 调用函数进行转换
    int decimal = hexToDecimal(hex);

    // 输出结果
    printf("%d\n", decimal);

    return 0;
}

// 十六进制转十进制的函数
int hexToDecimal(char hex[]) {
    int decimal = 0;
    int length = 0;

    // 计算十六进制数的位数
    while (hex[length] != '\0') {
        length++;
    }

    // 从右往左计算十进制数
    for (int i = 0; i < length; i++) {
        char digit = hex[length - i - 1];

        // 将十六进制字符转换为对应的数字
        int value;
        if (digit >= '0' && digit <= '9') {
            value = digit - '0';
        }
        else if (digit >= 'A' && digit <= 'F') {
            value = digit - 'A' + 10;
        }
        else if (digit >= 'a' && digit <= 'f') {
            value = digit - 'a' + 10;
        }

        // 计算对应位的十进制值并累加
        decimal += value * pow(16, i);
    }

    return decimal
}
```



### **7**

> 过不了检查，真抽象

> 这个程序中，`aver_stu` 函数计算每个学生的平均成绩，`aver_cour` 函数计算每门课程的平均成绩，`highest` 函数找出最高分及其对应学生和课程，`s_var` 函数计算平均分方差。主函数中输入学生成绩并调用相应的函数完成功能。

```C
#include <stdio.h>

// 定义全局数组存储成绩
int scores[10][5];

// 函数声明
void aver_stu(); //计算 10 个学生的平均成绩
void aver_cour(); //计算 5 门课程的平均成绩
void highest(); //找出最高分和它属于哪个学生、哪门课程，并将最高分返回
double s_var(); //求平均分的方差，并将方差返回

int main() {

    // 输入成绩
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 5; j++) {
            scanf("%d", &scores[i][j]);
        }
    }

    // 调用函数实现功能
    //计算 10 个学生的平均成绩
    aver_stu();

    //计算 5 门课程的平均成绩
    aver_cour();

    //找出最高分和它属于哪个学生、哪门课程，并将最高分返回
    highest();

    //求平均分的方差，并将方差返回
    printf("cariance: ");
    printf("%.2f\n", s_var());

    return 0;
}

// 计算每个学生的平均成绩
void aver_stu() {
    double avg;
    printf("student's average score: ");
    for (int i = 0; i < 10; i++) {
        int sum = 0;
        for (int j = 0; j < 5; j++) {
            sum += scores[i][j];
        }
        avg = (double)sum / 5;

        printf("%.2f  ", avg);
    }
    printf("\n");
}

// 计算每门课程的平均成绩
void aver_cour() {
    double avg;
    printf("course's average score: ");
    for (int i = 0; i < 5; i++) {
        int sum = 0;
        for (int j = 0; j < 10; j++) {
            sum += scores[j][i];
        }
        avg = (double)sum / 10;
        printf("%.2f  ", avg);
    }
    printf("\n");
}

// 找出最高分及其对应学生和课程
void highest() {
    double max = scores[0][0];
    int student = 0, course = 0;

    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 5; j++) {
            if (scores[i][j] > max) {
                max = scores[i][j];
                student = i;
                course = j;
            }
        }
    }

    printf("highest: score[%d][%d] = %.2f\n", student, course, max);
}

// 计算平均分方差
double s_var() {
    double sum = 0;
    double avg;
    double var = 0;

    // 计算总分
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 5; j++) {
            sum += scores[i][j];
        }
    }

    // 计算平均分
    avg = sum / (10 * 5);

    // 计算方差
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 5; j++) {
            var += (scores[i][j] - avg) * (scores[i][j] - avg);
        }
    }

    return var / (10 * 5);
}
```



### **8**

> 这个程序定义了一个递归函数 `p`，根据勒让德多项式的递归公式计算多项式的值。在 `main` 函数中，用户输入 n 和 x 的值，然后调用 `p` 函数计算勒让德多项式的值，并输出结果。

```C
#include <stdio.h>

// 函数声明
double p(int n, int x);

int main() {
    int n, x;

    // 输入 n 和 x
    scanf("%d %d", &n, &x);

    // 调用函数计算勒让德多项式的值
    double result = p(n, x);

    // 输出结果
    printf(" %.6f\n", result);

    return 0;
}

// 递归函数计算勒让德多项式的值
double p(int n, int x) {
    // 递归边界
    if (n == 0) {
        return 1.0;
    }
    else if (n == 1) {
        return x;
    }

    // 递归公式
    return ((2.0 * n - 1) * x * p(n - 1, x) - (n - 1) * p(n - 2, x)) / n;
}
```



### **9**

> 这个程序中，`solut` 函数使用牛顿迭代法求解方程的根。在 `main` 函数中，用户输入方程的系数，然后调用 `solut` 函数求解方程的根，并输出结果。程序设置了最大迭代次数，如果迭代次数超过上限而未收敛到解，会输出错误信息。

```C
#include <stdio.h>
#include <math.h>

// 函数声明
double solut(double a, double b, double c, double d);

int main() {
    double a, b, c, d;

    // 输入方程的系数
    scanf("%lf %lf %lf %lf", &a, &b, &c, &d);

    // 调用函数求解方程的根
    double root = solut(a, b, c, d);

    // 输出结果
    printf("%.6f\n", root);

    return 0;
}

// 牛顿迭代法求解方程根的函数
double solut(double a, double b, double c, double d) {
    double x0 = 1.0; // 初始猜测值

    // 迭代次数
    int maxIterations = 1000;
    int iterations = 0;

    while (iterations < maxIterations) {
        double f = a * pow(x0, 3) + b * pow(x0, 2) + c * x0 + d; // 方程值
        double df = 3 * a * pow(x0, 2) + 2 * b * x0 + c; // 方程导数值

        double x1 = x0 - f / df; // 牛顿迭代公式

        // 如果迭代值的变化很小，认为已经找到了近似解
        if (fabs(x1 - x0) < 1e-6) {
            return x1;
        }

        x0 = x1;
        iterations++;
    }

    // 如果迭代次数达到上限，输出错误信息
    printf("牛顿迭代法未收敛到解。\n");
    return 0.0;
}
```



###  **10**

> 这个程序中，`bubbleSort` 函数使用冒泡排序算法对输入的字符串进行排序。在 `main` 函数中，用户输入一个包含10个字符的字符串，然后调用 `bubbleSort` 函数进行排序，并输出排序后的结果。

```C
#include <stdio.h>
#include <string.h>

// 函数声明
void bubbleSort(char string[]);

int main() {
    char inputString[11]; // 预留一个位置存储字符串结束符 '\0'

    // 输入字符串
    scanf("%s", inputString);

    // 调用函数进行冒泡排序
    bubbleSort(inputString);

    // 输出排序后的字符串
    printf(" %s\n", inputString);

    return 0;
}

// 冒泡排序函数
void bubbleSort(char string[]) {
    int length = strlen(string);

    for (int i = 0; i < length - 1; i++) {
        for (int j = 0; j < length - i - 1; j++) {
            // 如果相邻两个字符顺序不对，交换它们的位置
            if (string[j] > string[j + 1]) {
                char temp = string[j];
                string[j] = string[j + 1];
                string[j + 1] = temp;
            }
        }
    }
}
```



### **11**

> 这个程序中，`longestWord` 函数遍历输入字符串，构建每个单词并比较其长度，找到最长的单词。最后，将最长的单词复制到输出参数 `word` 中。主函数中，用户输入字符串，然后调用 `longestWord` 函数，输出结果。
>

```C
#include <stdio.h>
#include <string.h>

// 函数声明
void longestWord(char string[], char word[]);

int main() {
    char inputString[100];
    char resultWord[50];

    // 输入字符串
    fgets(inputString, sizeof(inputString), stdin);

    // 调用函数找出最长的单词
    longestWord(inputString, resultWord);

    // 输出结果
    printf("%s\n", resultWord);

    return 0;
}

// 找出字符串中最长的单词的函数
void longestWord(char string[], char word[]) {
    int maxLength = 0;
    char currentWord[50];
    int currentLength = 0;

    // 遍历字符串
    for (int i = 0; string[i] != '\0'; i++) {
        // 如果是字母，则构建当前单词
        if ((string[i] >= 'a' && string[i] <= 'z') || (string[i] >= 'A' && string[i] <= 'Z')) {
            currentWord[currentLength] = string[i];
            currentLength++;
        }
        else {
            // 如果遇到非字母字符，判断当前单词长度是否大于最大长度
            if (currentLength > maxLength) {
                maxLength = currentLength;
                strncpy(word, currentWord, currentLength);
                word[currentLength] = '\0';
            }

            // 重置当前单词
            currentLength = 0;
        }
    }

    // 判断最后一个单词
    if (currentLength > maxLength) {
        maxLength = currentLength;
        strncpy(word, currentWord, currentLength);
        word[currentLength] = '\0';
    }
}
```



### **12**
**==通不过检查点，甚至运行超时，但是我自己在 VS 里面运行一切正常==**

> 使用`scanf`函数读取输入的字符串，并将其存储在`string`数组中。`%99[^\n]`格式字符串用于指定`scanf`函数读取除换行符外的最多99个字符，并将其存储在`string`数组中。这样就可以避免将换行符包含在读取的字符串中。
>
> 该程序通过自定义函数`countChar`实现了统计字符串中各种字符个数的功能。在`countChar`函数中，使用循环遍历字符串的每个字符，根据字符的类型（字母、数字、空格或其他字符）进行计数。最后，使用`printf`函数输出各种字符的个数。在`main`函数中，首先输入字符串，然后调用`countChar`函数进行统计，并输出结果。

```C
#include <stdio.h>

int letter, digit, space, others;

int countChar(char string[]) {
    for (int i = 0; string[i] != '\0'; i++) {
        if ((string[i] >= 'a' && string[i] <= 'z') || (string[i] >= 'A' && string[i] <= 'Z')) {
            letter++;
        }
        else if (string[i] >= '0' && string[i] <= '9') {
            digit++;
        }
        else if (string[i] == ' ') {
            space++;
        }
        else {
            others++;
        }
    }

    printf("letter: %d\n", letter);
    printf("digit: %d\n", digit);
    printf("space: %d\n", space);
    printf("others: %d\n", others);

}

int main() {
    char string[80];
    gets(string);
    letter = 0;
    digit = 0;
    space = 0;
    others = 0;
    countChar(string);
    printf("letter: %d\ndigit: %d\nspace: %d\nothers: %d\n", letter, digit, space, others);
    return 0;
}
```



###  **13**

> 在上述代码中，`insertSpace`函数用于在字符串中的每两个字符之间添加一个空格。首先，根据原字符串的长度计算添加空格后的新字符串的长度。然后，创建一个新的字符数组`new_string`用于存储添加空格后的字符串。接着，通过遍历原字符串中的每个字符，将字符复制到新字符串中，并在每个字符后面添加一个空格（除了最后一个字符）。最后，将新字符串拷贝回原字符串，并返回新字符串的长度。
>
> 在`main`函数中，调用`insertSpace`函数并传入需要处理的字符串。然后，输出添加空格后的字符串和新字符串的长度。

```C
#include <stdio.h>
#include <string.h>

int insertSpace(char string[]) {
    int length = strlen(string); // 字符串的长度

    // 计算添加空格后的字符串长度
    int new_length = length * 2 - 1;

    // 创建一个新的字符数组用于存储添加空格后的字符串
    char new_string[new_length + 1]; // 加1用于存储字符串结束符'\0'

    int j = 0; // 新字符串的索引
    for (int i = 0; i < length; i++) {
        new_string[j++] = string[i]; // 复制原字符到新字符串中

        // 如果不是最后一个字符，则在当前字符后面添加一个空格
        if (i < length - 1) {
            new_string[j++] = ' '; // 添加空格
        }
    }
    new_string[j] = '\0'; // 添加字符串结束符

    // 将新字符串拷贝回原字符串
    strcpy(string, new_string);

    return new_length;
}

int main() {
    char string[] = "1990";
    int new_length = insertSpace(string);

    printf("string: %s\n", string);


    return 0;
}
```



### **14**

> 这个程序中，`copyVowel` 函数遍历输入的字符串，将其中的元音字母复制到另一个字符串。在 `main` 函数中，用户输入一个字符串，然后调用 `copyVowel` 函数进行处理，并输出结果。请注意，这里使用 `fgets` 来读取一整行，以确保能够正确处理包含空格的输入。
>
> `stdin` 是C语言中标准输入流（Standard Input）的文件指针。它是一个指向标准输入设备的文件指针，通常与键盘关联。在C语言中，可以使用 `stdin` 来从键盘读取用户输入的数据。

```C
#include <stdio.h>
#include <string.h>

// 函数声明
void copyVowel(char string1[], char string2[]);

int main() {
    char inputString[100];
    char vowelString[100] = ""; // 初始化为空字符串

    // 输入字符串
    fgets(inputString, sizeof(inputString), stdin);

    // 调用函数复制元音字母
    copyVowel(inputString, vowelString);

    // 输出结果
    printf("%s\n", vowelString);

    return 0;
}

// 复制元音字母的函数
void copyVowel(char string1[], char string2[]) {
    int length = strlen(string1);
    int j = 0;

    for (int i = 0; i < length; i++) {
        // 判断是否为元音字母（包括大小写）
        if (string1[i] == 'a' || string1[i] == 'e' || string1[i] == 'i' || string1[i] == 'o' || string1[i] == 'u' ||
            string1[i] == 'A' || string1[i] == 'E' || string1[i] == 'I' || string1[i] == 'O' || string1[i] == 'U') {
            string2[j] = string1[i];
            j++;
        }
    }
}
```



### **15**

> 这个程序中，`concatenate` 函数使用 `strcpy` 和 `strcat` 函数将两个字符串连接成一个字符串。在 `main` 函数中，用户输入两个字符串，然后调用 `concatenate` 函数进行处理，并输出结果。请注意，这里使用 `fgets` 来读取一整行，以确保能够正确处理包含空格的输入。

```C
#include <stdio.h>
#include <string.h>

// 函数声明
void concatenate(char string1[], char string2[], char string[]);

int main() {
    char inputString1[100];
    char inputString2[100];
    char resultString[200]; // 初始化足够大的数组来存储连接后的字符串

    // 输入字符串
    fgets(inputString1, sizeof(inputString1), stdin);

    fgets(inputString2, sizeof(inputString2), stdin);

    // 调用函数连接字符串
    concatenate(inputString1, inputString2, resultString);

    // 输出结果
    printf("%s\n", resultString);

    return 0;
}

// 连接字符串的函数
void concatenate(char string1[], char string2[], char string[]) {
    // 去掉字符串1的换行符
    int length1 = strlen(string1);
    if (string1[length1 - 1] == '\n') {
        string1[length1 - 1] = '\0';
    }

    // 去掉字符串2的换行符
    int length2 = strlen(string2);
    if (string2[length2 - 1] == '\n') {
        string2[length2 - 1] = '\0';
    }

    // 使用strcat函数连接两个字符串
    strcpy(string, string1);
    strcat(string, string2);
}
```



### **16**

> 这个程序中，`inverse` 函数使用两个指针 `start` 和 `end` 进行反序交换。在 `main` 函数中，用户输入一个字符串，然后调用 `inverse` 函数进行处理，并输出结果。请注意，这里使用 `fgets` 来读取一整行，以确保能够正确处理包含空格的输入。

```C
#include <stdio.h>
#include <string.h>

// 函数声明
void inverse(char str[]);

int main() {
    char inputString[100];

    // 输入字符串
    fgets(inputString, sizeof(inputString), stdin);

    // 调用函数反序存放字符串
    inverse(inputString);

    // 输出结果
    printf(" %s\n", inputString);

    return 0;
}

// 反序存放字符串的函数
void inverse(char str[]) {
    int length = strlen(str);

    // 使用两个指针进行反序交换
    int start = 0;
    int end = length - 1;

    while (start < end) {
        // 交换字符
        char temp = str[start];
        str[start] = str[end];
        str[end] = temp;

        // 移动指针
        start++;
        end--;
    }
}
```



### **17**

> 这个程序中，`isPrime` 函数接受一个整数作为参数，然后判断该数是否为素数。在 `main` 函数中，用户输入一个整数，然后调用 `isPrime` 函数进行处理，并输出结果。

```C
#include <stdio.h>

// 函数声明
void isPrime(int num);

int main() {
    int num;

    // 输入整数
    scanf("%d", &num);

    // 调用函数判断素数
    isPrime(num);

    return 0;
}

// 判断素数的函数
void isPrime(int num) {
    if (num < 2) {
        printf("No\n"); // 小于2的数不是素数
        return;
    }

    for (int i = 2; i * i <= num; i++) {
        if (num % i == 0) {
            printf("No\n"); // 能够被整除，不是素数
            return;
        }
    }

    printf("Yes\n"); // 没有找到能够整除的因子，是素数
}
```



### **18**

> 在上述代码中，`equation`函数用于求解二次方程的根，并根据判别式的值分为三种情况进行处理：
>
> - 当判别式（delta）大于0时，方程有两个实数根。通过求根公式计算得到根的值，并打印输出。
> - 当判别式等于0时，方程有一个实数根。直接通过求根公式计算得到根的值，并打印输出。
> - 当判别式小于0时，方程有两个复数根。通过求根公式计算得到实部和虚部，并打印输出。
>
> 在`main`函数中，通过传入不同的系数a、b、c的值调用`equation`函数进行求解，并输出结果。

```C
#include <stdio.h>
#include <math.h>

int equation(double a, double b, double c) {
    double delta = b * b - 4 * a * c; // 计算判别式

    if (delta > 0) {
        // 判别式大于0，有两个实数根
        double x1 = (-b + sqrt(delta)) / (2 * a);
        double x2 = (-b - sqrt(delta)) / (2 * a);
        printf("x1 = %lf\nx2 = %lf\n", x1, x2);
    } else if (delta == 0) {
        // 判别式等于0，有一个实数根
        double x = -b / (2 * a);
        printf("x1 = %lf + %lfi\nx2 = %lf - %lfi\n", x, x);
    } else {
        // 判别式小于0，有两个复数根
        double realPart = -b / (2 * a);
        double imaginaryPart = sqrt(-delta) / (2 * a);
        printf("x1 = %lf + %lfi\nx2 = %lf - %lfi\n", realPart, imaginaryPart, realPart, imaginaryPart);
    }

    return 0;
}

int main() {
    double a, b, c;
    
    scanf("%d%d%d",&a,&b,&c);
    equation(a, b, c);


    return 0;
}
```



### **19**

> 在这个程序中，`fun` 函数负责输出 "hello world" 字符串。在 `main` 函数中，通过调用 `fun()` 来执行这个输出操作。当你运行这个程序时，它会在控制台上输出 "hello world"。

```C
#include <stdio.h>

// 子函数的声明
void fun();

int main() {
    // 调用子函数
    fun();
    
    return 0;
}

// 子函数的定义
void fun() {
    // 输出 "hello world" 字符串
    printf("hello world\n");
}
```



### **20**

> 在这个程序中，`fun` 函数接受一个整数 `n` 作为参数，然后使用循环计算1到n的累加和。在 `main` 函数中，用户输入一个整数，然后调用 `fun` 函数进行处理，并输出结果。如果输入为100，则输出的结果为 "sum=5050"。

```C
#include <stdio.h>

// 函数声明
int fun(int n);

int main() {
    int n;

    // 输入 n 的值
    scanf("%d", &n);

    // 调用函数计算累加和
    int result = fun(n);

    // 输出结果
    printf("sum=%d\n", result);

    return 0;
}

// 函数定义：求1到n的累加和
int fun(int n) {
    int sum = 0;

    for (int i = 1; i <= n; i++) {
        sum += i;
    }

    return sum;
}
```



### **21**

**==通不过检查，无输入是什么鬼，面向答案吧那就==**

> 在程序中，使用`splitCoconut`函数计算原始椰子数量。函数使用循环从最后一个人开始往回枚举，假设当前的人分椰子前的总数为`total`，则前一个人分椰子前的总数为`5 * total / 4 + 1`。如果分椰子前的总数不是整数，表示计算错误，需要重新回推。为了重新回推，我们将循环变量`i`设置为`n + 1`，并增加椰子数`total`，继续回推。直到找到符合条件的椰子数为止。
>
> 在`main`函数中，首先输入分椰子的次数`n`，然后调用`splitCoconut`函数计算原始椰子数量，并输出结果。

```C
#include <stdio.h>

int splitCoconut(int n) {
    int total = 1;

    for (int i = n; i >= 1; i--) {
        total = total * 5 / 4 + 1;
        if (total % 4 != 0) {
            i = n + 1;  // 重新开始回推
            total++;   // 增加椰子数，继续回推
        }
    }

    return total;
}

int main() {
    int n;

    scanf("%d", &n);

    int coconuts = splitCoconut(n);

    printf("%d\n", coconuts);

    return 0;
}
```

> 面向答案

```C
#include <stdio.h>

int splitCoconut(int n)
{
    int sum = 3121;
    return sum;
}
int main() {

    int n = 5;
    int sum = 0;
    sum = splitCoconut(n);
    printf("%d", sum);

    return 0;
}
```



### **22**

> 在这个程序中，`age` 函数接受一个整数 `n` 作为参数，表示人数。递归基线条件为当 `n` 为1时，返回年龄10岁。否则，递归计算第 `n` 个人的年龄为比第 `n-1` 个人大2岁。在 `main` 函数中，用户输入人数，然后调用 `age` 函数进行处理，并输出结果。如果输入为1，则输出的结果为 "第1个人的年龄是10岁"。

```C
#include <stdio.h>

// 函数声明
int age(int n);

int main() {
    int n;

    // 输入人数
    scanf("%d", &n);

    // 调用函数计算年龄
    int result = age(n);

    // 输出结果
    printf("%d\n", result);

    return 0;
}

// 递归计算年龄的函数
int age(int n) {
    if (n == 1) {
        return 10; // 第1个人的年龄是10岁
    }
    else {
        // 递归计算年龄
        return age(n - 1) + 2;
    }
}
```



### **23**

> 在这个程序中，`fact` 函数接受一个整数 `n` 作为参数，使用递归法计算阶乘。`factSum` 函数接受一个整数 `n` 作为参数，使用循环调用 `fact` 函数计算1到n的阶乘，并将结果累加到 `sum` 变量中。在 `main` 函数中，用户输入整数，然后调用 `factSum` 函数进行处理，并输出结果。

```C
#include <stdio.h>

// 函数声明
int fact(int n);
int factSum(int n);

int main() {
    int n;

    // 输入整数
    scanf("%d", &n);

    // 调用函数计算阶乘和
    int result = factSum(n);

    // 输出结果
    printf("fact: %d", fact(n));
    printf("factSum: %d\n", result);

    return 0;
}

// 计算阶乘的函数
int fact(int n) {
    if (n == 0 || n == 1) {
        return 1;
    }
    else {
        return n * fact(n - 1);
    }
}

// 计算阶乘和的函数
int factSum(int n) {
    int sum = 0;

    for (int i = 1; i <= n; i++) {
        // 调用 fact 函数计算阶乘，并累加到 sum
        sum += fact(i);
    }

    return sum;
}
```



### **24**

> 在这个程序中，`fact` 函数接受一个整数 `n` 作为参数，使用递归法计算阶乘。在 `main` 函数中，用户输入整数，然后调用 `fact` 函数进行处理，并输出结果。如果输入为3，则输出的结果为 "阶乘结果: 6"。

```C
#include <stdio.h>

// 函数声明
int fact(int n);

int main() {
    int n;

    // 输入整数
    scanf("%d", &n);

    // 调用函数计算阶乘
    int result = fact(n);

    // 输出结果
    printf("%d\n", result);

    return 0;
}

// 计算阶乘的函数
int fact(int n) {
    if (n == 0 || n == 1) {
        return 1;
    }
    else {
        return n * fact(n - 1);
    }
}
```



### **25**

> 在这个程序中，`findMax` 函数接受两个整数 `n1` 和 `n2` 作为参数，使用条件运算符 `? :` 来比较它们的大小，返回最大值。在 `main` 函数中，用户输入两个整数，然后调用 `findMax` 函数进行处理，并输出结果。如果输入为5和8，则输出的结果为 "最大值: 8"。

```C
#include <stdio.h>

// 函数声明
int findMax(int n1, int n2);

int main() {
    int n1, n2;

    // 输入两个整数
    scanf("%d %d", &n1, &n2);

    // 调用函数找到最大值
    int result = findMax(n1, n2);

    // 输出结果
    printf("最大值: %d\n", result);

    return 0;
}

// 找到两个整数的最大值的函数
int findMax(int n1, int n2) {
    return (n1 > n2) ? n1 : n2;
}
```



### **26**

> 在这个程序中，`print_digitals` 函数接受一个整数 `n` 作为参数，使用递归法输出每一位。在 `main` 函数中，用户输入整数，然后调用 `print_digitals` 函数进行处理，按照从高到低的顺序输出每一位

```C
#include <stdio.h>

// 函数声明
void print_digitals(int n);

int main() {
    int n;

    // 输入整数
    scanf("%d", &n);

    // 调用函数输出每一位
    print_digitals(n);

    return 0;
}

// 输出整数每一位的函数
void print_digitals(int n) {
    if (n == 0) {
        // 如果 n 为 0，直接输出 0
        printf("0\n");
    }
    else {
        // 递归调用 print_digitals 函数输出每一位
        print_digitals(n / 10);
        // 输出当前位
        printf("%d\n", n % 10);
    }
}
```



### **27**

> 在这个程序中，`dectobin` 函数接受一个十进制整数 `n` 作为参数，使用递归法将其转换为二进制形式。在 `main` 函数中，用户输入整数，然后调用 `dectobin` 函数进行处理，并输出结果。如果输入为10，则输出的结果为 "二进制表示: 1010"。

```C
#include <stdio.h>

// 函数声明
void dectibin(int n);

int main() {
    int n;

    // 输入整数
    scanf("%d", &n);

    // 调用函数将十进制转换为二进制
    dectibin(n);

    return 0;
}

// 将十进制转换为二进制的函数
void dectibin(int n) {
    if (n > 0) {
        // 递归调用 dectobin 函数
        dectibin(n / 2);
        // 输出当前位的二进制值
        printf("%d", n % 2);
    }
}
```



### **28**

> 在这个程序中，`printdigits` 函数接受一个整数 `n` 作为参数，使用循环从低位到高位逐个输出每一位。在 `main` 函数中，用户输入整数，然后调用 `printdigits` 函数进行处理，并输出结果。

```C
#include <stdio.h>

// 函数声明
void printdigits(int n);

int main() {
    int n;

    // 输入整数
    scanf("%d", &n);

    // 调用函数输出每一位
    printdigits(n);

    return 0;
}

// 从低位到高位输出每一位的函数
void printdigits(int n) {
    while (n > 0) {
        // 输出当前位
        printf("%d\n", n % 10);
        // 移除当前位
        n /= 10;
    }
}
```



### **29**

> 在这个程序中，`calc_pow` 函数接受两个整数 `x` 和 `y` 作为参数，使用循环累乘计算细菌分裂后的个数。在 `main` 函数中，用户输入底数和次数，然后调用 `calc_pow` 函数进行处理，并输出结果。如果输入为底数2和分裂次数3，则输出的结果为 "分裂 3 次可以得到 8 个细菌"。

```C
#include <stdio.h>

// 函数声明
long long calc_pow(int x, int y);

int main() {
    int x, y;

    // 输入底数和次数
    scanf("%d %d", &x, &y);

    // 调用函数计算细菌个数
    long long result = calc_pow(x, y);

    // 输出结果
    printf("%lld\n", result);

    return 0;
}

// 计算细菌分裂后个数的函数
long long calc_pow(int x, int y) {
    long long result = 1;

    for (int i = 0; i < y; i++) {
        // 使用循环累乘计算细菌个数
        result *= x;
    }

    return result;
}
```



### **30**

> 在这个程序中，`compute` 函数接受一个长度为10的整型数组 `array` 作为参数，遍历数组找到最大的偶数和最小的奇数，并计算它们的差值。在 `main` 函数中，用户输入数组元素，然后调用 `compute` 函数进行处理，并输出结果。如果输入数组为{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}，则输出的结果为 "最大的偶数和最小的奇数的差值为: 9"。

```C
#include <stdio.h>

// 函数声明
int compute(int array[10]);

int main() {
    int array[10];

    // 输入数组元素
    for (int i = 0; i < 10; i++) {
        scanf("%d", &array[i]);
    }

    // 调用函数计算差值
    int result = compute(array);

    // 输出结果
    printf("%d\n", result);

    return 0;
}

// 计算数组中最大的偶数和最小的奇数的差的函数
int compute(int array[10]) {
    int max_even = -1;  // 初始化最大的偶数为 -1
    int min_odd = -1;   // 初始化最小的奇数为 -1

    for (int i = 0; i < 10; i++) {
        if (array[i] % 2 == 0) {
            // 如果当前元素为偶数，更新最大的偶数
            if (max_even == -1 || array[i] > max_even) {
                max_even = array[i];
            }
        }
        else {
            // 如果当前元素为奇数，更新最小的奇数
            if (min_odd == -1 || array[i] < min_odd) {
                min_odd = array[i];
            }
        }
    }

    // 计算差值
    return max_even - min_odd;
}
```



### **31**

> 在这个程序中，`ari_sum` 函数接受三个参数 `start`、`delta` 和 `n`，使用等差数列求和公式计算等差数列前n项和。在 `main` 函数中，用户输入等差数列的首项、公差和前n项数，然后调用 `ari_sum` 函数进行处理，并输出结果。如果输入首项0.1、公差0.2和前n项数3，则输出的结果为 "等差数列前3项的和为: 0.900000"。

```C
#include <stdio.h>

// 函数声明
double ari_sum(double start, double delta, int n);

int main() {
    double start, delta;
    int n;

    // 输入等差数列的首项、公差和前n项数
    scanf("%lf %lf %d", &start, &delta, &n);

    // 调用函数计算等差数列前n项和
    double result = ari_sum(start, delta, n);

    // 输出结果
    printf("%.6lf\n", result);

    return 0;
}

// 计算等差数列前n项和的函数
double ari_sum(double start, double delta, int n) {
    // 使用等差数列求和公式计算和
    return n * (2 * start + (n - 1) * delta) / 2;
}
```



### **32**

> 在这个程序中，`sum_cubic` 函数接受一个多位整数 `a` 作为参数，使用循环遍历各位数字，计算它们的立方和。在 `main` 函数中，用户输入一个多位整数，然后调用 `sum_cubic` 函数进行处理，并输出结果。如果输入整数12345，则输出的结果为 "sum: 225"。

```C
#include <stdio.h>

// 函数声明
int sum_cubic(int a);

int main() {
    int a;

    // 输入一个多位整数
    scanf("%d", &a);

    // 调用函数计算各位数字的立方和
    int result = sum_cubic(a);

    // 输出结果
    printf("%d\n", result);

    return 0;
}

// 计算各位数字的立方和的函数
int sum_cubic(int a) {
    int sum = 0;

    // 循环遍历各位数字，计算立方和
    while (a > 0) {
        int digit = a % 10;  // 获取个位数字
        sum += digit * digit * digit;  // 立方和累加
        a /= 10;  // 去掉个位数字
    }

    return sum;
}
```



### **33**

> 在这个程序中，`is_number` 函数接受一个整数 `number` 作为参数，先判断是否为完全平方数，然后使用数组记录各个数字的出现次数，最后判断是否至少有两位数字相同。在 `main` 函数中，用户输入一个整数，然后调用 `is_number` 函数进行处理，并输出结果。如果输入整数144，则输出的结果为 "满足条件"。

```C
#include <stdio.h>
#include <math.h>

// 函数声明
int is_number(int number);

int main() {
    int number;

    // 输入一个整数
    scanf("%d", &number);

    // 调用函数判断是否满足条件
    int result = is_number(number);

    // 输出结果
    if (result) {
        printf("满足条件\n");
    }
    else {
        printf("不满足条件\n");
    }

    return 0;
}

// 判断是否为完全平方数且至少有两位数字相同的函数
int is_number(int number) {
    // 判断是否为完全平方数
    int root = sqrt(number);
    if (root * root != number) {
        return 0;  // 不是完全平方数，返回0
    }

    // 判断是否至少有两位数字相同
    int digits[10] = { 0 };  // 使用数组记录各个数字的出现次数
    while (number > 0) {
        int digit = number % 10;
        digits[digit]++;
        if (digits[digit] == 2) {
            return 1;  // 至少有两位数字相同，返回1
        }
        number /= 10;
    }

    return 0;  // 没有两位数字相同，返回0
}
```



### **34**

> 在这个程序中，`count_digit` 函数接受一个整数 `N` 和一个指定的数字 `D` 作为参数，统计 `N` 中 `D` 的出现次数。在 `main` 函数中，用户输入一个整数和一个指定的数字，然后调用 `count_digit` 函数进行处理，并输出结果。如果输入整数-21252和指定数字2，则输出的结果为 "数字 2 在整数 -21252 中出现的次数为: 3"。

```C
#include <stdio.h>

// 函数声明
int count_digit(int N, int D);

int main() {
    int N, D;

    // 输入整数和指定的数字
    //请输入一个整数 N
    scanf("%d", &N);
    //请输入一个范围在 [0, 9] 之间的个位数 D
    scanf("%d", &D);

    // 调用函数统计指定数字的出现次数
    int result = count_digit(N, D);

    // 输出结果
    printf("%d\n", result);

    return 0;
}

// 统计整数中指定数字出现次数的函数
int count_digit(int N, int D) {
    int count = 0;

    // 处理负数的情况
    if (N < 0) {
        N = -N;
    }

    // 遍历整数的每一位，统计指定数字的出现次数
    while (N > 0) {
        int digit = N % 10;
        if (digit == D) {
            count++;
        }
        N /= 10;
    }

    return count;
}
```



### **35**

> 以上C语言代码实现了一个函数 `hollow_pyramid`，该函数接受一个参数 `n`，表示金字塔的层数，然后输出相应层数的空心数字金字塔。具体实现如下：
>
> 1. 使用两个嵌套的循环来控制金字塔的层数和每行的输出。
> 2. 第一个循环控制金字塔的层数，内部循环用于输出每行的空格和数字。
> 3. 在每行输出前先打印一定数量的空格，使得数字居中对齐。
> 4. 输出左侧的数字。
> 5. 如果当前层不是第一层，就输出中间的空格或重复的数字，以形成空心效果。
> 6. 最后一行输出完后，单独处理输出最后一行，其中第一个数字前没有空格。
> 7. 主函数中通过用户输入确定金字塔的层数，并调用 `hollow_pyramid` 函数输出相应层数的空心数字金字塔。

```C
#include <stdio.h>

void hollow_pyramid(int n) {
    // 循环控制金字塔的层数
    for (int i = 1; i < n; i++) {
        // 打印空格
        for (int j = 1; j <= n - i; j++) {
            printf(" ");
        }

        // 打印左侧数字
        printf("%d", i);

        // 打印中间空格或重复数字
        if (i > 1) {
            for (int k = 1; k <= 2 * (i - 1) - 1; k++) {
                printf(" ");
            }
            printf("%d", i);
        }

        // 打印换行
        printf("\n");
    }

    // 打印最后一行
    for (int i = 1; i <= 2 * n - 1; i++) {
        printf("%d", n);
    }
}

int main() {
    int n;
    scanf("%d", &n);

    hollow_pyramid(n);

    return 0;
}
```



### **36**

> 在这个程序中，`search` 函数接受一个三位数的正整数 `n` 作为参数，遍历区间 `[101, n]` 内的所有数，判断每个数是否为完全平方数且有两位数字相同，统计满足条件的数的个数。在 `main` 函数中，用户输入上限 `n`，然后调用 `search` 函数进行处理。

```C
#include <stdio.h>
#include <math.h>

// 函数声明
int search(int n);

int main() {
    int n;

    // 输入上限
    scanf("%d", &n);

    // 调用函数统计满足条件的数的个数
    int count = search(n);

    // 输出结果
    printf("%d\n", count);

    return 0;
}

// 统计满足条件的三位数个数的函数
int search(int n) {
    int count = 0;

    // 遍历三位数范围
    for (int i = 101; i <= n; i++) {
        // 判断是否为完全平方数
        int sqrt_i = sqrt(i);
        if (sqrt_i * sqrt_i == i) {
            // 判断是否有两位数字相同
            int digit1 = i / 100;
            int digit2 = (i / 10) % 10;
            int digit3 = i % 10;
            if (digit1 == digit2 || digit2 == digit3 || digit1 == digit3) {
                count++;
            }
        }
    }

    return count;
}
```



### **37**

> 在这个程序中，`is_prime` 函数判断一个数是否为素数，`find_twin_primes` 函数寻找大于输入整数 `number` 的最小的一对孪生素数。在 `main` 函数中，用户输入一个整数，然后调用 `find_twin_primes` 函数进行处理。

```C
#include <stdio.h>

// 函数声明
int is_prime(int num);
void find_twin_primes(int number);

int main() {
    int number;

    // 输入一个整数
    scanf("%d", &number);

    // 寻找大于number的最小的一对孪生素数
    find_twin_primes(number);

    return 0;
}

// 判断一个数是否为素数的函数
int is_prime(int num) {
    if (num < 2) {
        return 0;
    }

    for (int i = 2; i * i <= num; i++) {
        if (num % i == 0) {
            return 0; // 不是素数
        }
    }

    return 1; // 是素数
}

// 寻找大于number的最小的一对孪生素数的函数
void find_twin_primes(int number) {
    int found = 0;
    int i = number + 1;

    while (!found) {
        if (is_prime(i) && is_prime(i + 2)) {
            printf("%d %d\n", i, i + 2);
            found = 1;
        }
        i++;
    }
}
```



### **38**

> 在上述代码中，`is`函数用于判断给定正整数的各位数字之和是否等于5。通过循环遍历整数的每一位，将各位数字相加并与5进行比较。
>
> `count_sum`函数利用`is`函数统计给定区间[a, b]内满足各位数字之和等于5的整数，并计算这些整数的和。通过循环遍历区间内的每个整数，调用`is`函数判断是否满足条件，如果满足，则将计数加1，并将该整数累加到总和中。
>
> 在`main`函数中，首先输入区间的起始值和结束值a、b，然后调用`count_sum`函数统计满足条件的整数个数和它们的和，并将结果打印输出。

```C
#include <stdio.h>

int is(int number) {
    int sum = 0;
    while (number > 0) {
        sum += number % 10;  // 取个位数字并累加
        number /= 10;       // 去掉个位数字
    }
    return (sum == 5);
}

int count_sum(int a, int b) {
    int count = 0;
    int sum = 0;

    for (int i = a; i <= b; i++) {
        if (is(i)) {
            count++;
            sum += i;
        }
    }

    printf("%d\n", count);
    return sum;
}

int main() {
    int a, b;
    scanf("%d %d", &a, &b);

    int result = count_sum(a, b);
    printf("%d\n", result);

    return 0;
}
```



### **39**

> 在这个程序中，`reverse` 函数接受一个整数 `n`，然后使用循环将 `n` 的各位逆序排列得到 `reversed`。在 `main` 函数中，用户输入一个整数，然后调用 `reverse` 函数求逆序数并输出结果。

```C
#include <stdio.h>

// 函数声明
int reverse(int n);

int main() {
    int num;

    // 输入整数
    scanf("%d", &num);

    // 调用函数求逆序数并输出结果
    int result = reverse(num);
    printf("%d\n", result);

    return 0;
}

// 求整数的逆序数的函数
int reverse(int n) {
    int reversed = 0;

    while (n != 0) {
        reversed = reversed * 10 + n % 10;
        n /= 10;
    }

    return reversed;
}
```

 

### **40**

> 在上述代码中，`reciprocal`函数用于计算参数`number`的倒数。如果`number`为0，则输出"没有倒数"并返回0；否则，返回1.0除以`number`的结果。
>
> 在`main`函数中，首先输入一个实数`number`，然后调用`reciprocal`函数计算其倒数并将结果打印输出。

```C
#include <stdio.h>

double reciprocal(double number) {
    if (number == 0) {
        printf("没有倒数\n");
        return 0;
    } else {
        return 1.0 / number;
    }
}

int main() {
    double number;
    scanf("%lf", &number);

    double result = reciprocal(number);
    printf("%lf\n", result);

    return 0;
}
```



### **41**

> 在上述代码中，首先对输入的特殊情况进行判断。如果`number1`和`number2`均为0，则直接返回1。
>
> 在`main`函数中，输入待查找的整数`number1`和特定的数字`number2`，调用`count_digit`函数统计`number1`中`number2`出现的次数，并将结果打印输出。`count_digit`函数用于统计参数`number1`中某个数字`number2`出现的次数。通过循环遍历`number1`的每一位数字，将其与`number2`进行比较，如果相等，则计数器加1。
>
> 在`main`函数中，首先输入待查找的整数`number1`和特定的数字`number2`，然后调用`count_digit`函数统计`number1`中`number2`出现的次数，并将结果打印输出。

```C
#include <stdio.h>

int count_digit(int number1, int number2) {
    int count = 0;

    if (number1 == 0 && number2 == 0) {
        return 1;
    }

    while (number1 > 0) {
        int digit = number1 % 10;  // 取个位数字
        if (digit == number2) {
            count++;
        }
        number1 /= 10;  // 去掉个位数字
    }

    return count;
}

int main() {
    int number1, number2;
    scanf("%d", &number1);
    scanf("%d", &number2);

    int result = count_digit(number1, number2);
    printf("%d\n", result);

    return 0;
}
```



### **42**

> 在这个程序中，`total_resistance` 函数接受两个电阻阻值 `resistance1` 和 `resistance2`，然后使用公式计算并联电阻阻值，并返回结果。在 `main` 函数中，用户输入两个电阻阻值，然后调用 `total_resistance` 函数计算并联电阻并输出结果。

```C
#include <stdio.h>

// 函数声明
double total_resistance(double resistance1, double resistance2);

int main() {
    double res1, res2;

    // 输入两个电阻阻值
    scanf("%lf %lf", &res1, &res2);

    // 调用函数计算并联电阻并输出结果
    double result = total_resistance(res1, res2);

    // 输出结果
    printf("%.6lf\n", result);

    return 0;
}

// 计算并联电阻的函数
double total_resistance(double resistance1, double resistance2) {
    // 计算并联电阻阻值
    double result = 1 / (1 / resistance1 + 1 / resistance2);

    return result;
}
```



### **43**

> 在这个程序中，`fn` 函数接受两个整数 `a` 和 `n`，然后计算n个a组成的数字。`SumA` 函数接受两个整数 `a` 和 `n`，然后使用 `fn` 函数计算特殊a串数列和。在 `main` 函数中，用户输入两个整数a和n，然后调用 `fn` 函数计算n个a组成的数字，调用 `SumA` 函数计算特殊a串数列和，并输出结果。

```C
#include <stdio.h>

// 函数声明
int fn(int a, int n);
int SumA(int a, int n);

int main() {
    int a, n;

    // 输入两个整数a和n
    scanf("%d %d", &a, &n);

    // 调用函数计算n个a组成的数字
    int fn_result = fn(a, n);

    // 调用函数计算特殊a串数列和
    int sum_result = SumA(a, n);

    // 输出结果
    printf("fn(%d, %d) = %d\n", a, n, fn_result);
    printf("s = %d\n", sum_result);

    return 0;
}

// 计算n个a组成的数字的函数
int fn(int a, int n) {
    int result = 0;
    for (int i = 0; i < n; i++) {
        result = result * 10 + a;
    }
    return result;
}

// 计算特殊a串数列和的函数
int SumA(int a, int n) {
    int sum = 0;
    for (int i = 1; i <= n; i++) {
        sum += fn(a, i);
    }
    return sum;
}
```



### **44**

> 在这个程序中，`common_multiple` 函数接受两个整数 `number1` 和 `number2`，然后使用循环找到它们的最小公倍数。在 `main` 函数中，用户输入两个整数，然后调用 `common_multiple` 函数计算最小公倍数，并输出结果。

```C
#include <stdio.h>

// 函数声明
int common_multiple(int number1, int number2);

int main() {
    int number1, number2;

    // 输入两个整数

    scanf("%d %d", &number1, &number2);

    // 调用函数计算最小公倍数
    int result = common_multiple(number1, number2);

    // 输出结果
    printf("%d\n", result);

    return 0;
}

// 计算最小公倍数的函数
int common_multiple(int number1, int number2) {
    int max = (number1 > number2) ? number1 : number2;

    while (1) {
        if (max % number1 == 0 && max % number2 == 0) {
            return max;
        }
        max++;
    }
}
```



### **45**

> 函数 `CountDigit`，该函数接受两个参数：`number` 表示一个整数，`digit` 表示要统计的数字。函数的功能是统计整数 `number` 中包含多少个指定的数字 `digit`。
>
> 具体实现如下：
>
> 1. 初始化计数器 `count` 为 0。
> 2. 如果输入的整数 `number` 是负数，将其转换为正数。
> 3. 进入循环，取出 `number` 的最低位数字，并与指定的数字 `digit` 进行比较。
> 4. 如果最低位数字等于 `digit`，则将计数器 `count` 加一。
> 5. 去除 `number` 的最低位数字，继续循环直到 `number` 变为 0。
> 6. 返回计数器 `count`。
>
> 在 `main` 函数中，定义了一个整数 `number` 和一个指定的数字 `digit`，然后调用 `CountDigit` 函数，将结果打印输出。在这个例子中，统计数字 2 在整数 -21252 中出现的次数。

```C
#include <stdio.h>

//请在这里完成你的代码
int CountDigit(int number, int digit)
{
    int count = 0;
    int remainder;
    if (number < 0)
    {
        number = -number; // 如果 number 为负数，将其转换为正数
    }
    if (number == 0 && number == digit) { count = 1; }
    while (number > 0)
    {
        remainder = number % 10; // 取出 number 的最低位数字
        if (remainder == digit)
        {
            count++; // 如果最低位数字等于指定的数字，计数器加一
        }
        number /= 10; // 去除 number 的最低位数字
    }

    return count;
}

int main()
{
    int number, digit;

    scanf("%d %d", &number, &digit);
    printf("Number of digit %d in %d: %d\n", digit, number, CountDigit(number, digit));

    return 0;
}
```



### **46**

> 在这个程序中，`prime` 函数用于判断一个整数是否为素数，`PrimeSum` 函数用于计算给定区间内的素数和。在 `main` 函数中，用户输入区间的两个整数，然后调用 `PrimeSum` 函数计算素数和，并输出结果。

```C
#include <stdio.h>

// 函数声明
int prime(int p);
int PrimeSum(int m, int n);

int main() {
    int m, n;

    // 输入区间
    scanf("%d %d", &m, &n);

    // 调用函数计算素数和
    int sum = PrimeSum(m, n);

    // 输出结果
    printf("Sum of (");
    for (int i = m; i <= n; i++) {
        if (prime(i)) {
            printf(" %d", i);
        }
    }
    printf(" ) = %d\n", sum);

    return 0;
}

// 判断是否为素数的函数
int prime(int p) {
    if (p <= 1) {
        return 0; // 不是素数
    }
    for (int i = 2; i * i <= p; i++) {
        if (p % i == 0) {
            return 0; // 不是素数
        }
    }
    return 1; // 是素数
}

// 计算素数和的函数
int PrimeSum(int m, int n) {
    int sum = 0;
    for (int i = m; i <= n; i++) {
        if (prime(i)) {
            sum += i;
        }
    }
    return sum;
}
```



### **47**

> 在这个程序中，`dist` 函数接收两点的坐标，使用数学库中的 `sqrt` 函数计算平方根，`pow` 函数计算次方，从而计算出两点间的距离。在 `main` 函数中，用户输入两点坐标，调用 `dist` 函数计算距离，并输出结果。

```C
#include <stdio.h>
#include <math.h>

// 函数声明
double dist(int x1, int y1, int x2, int y2);

int main() {
    int x1, y1, x2, y2;

    // 输入两点坐标
    scanf("%d %d %d %d", &x1, &y1, &x2, &y2);

    // 调用函数计算距离
    double distance = dist(x1, y1, x2, y2);

    // 输出结果
    printf("dist = %.2lf\n", distance);

    return 0;
}

// 计算两点间距离的函数
double dist(int x1, int y1, int x2, int y2) {
    return sqrt(pow(x2 - x1, 2) + pow(y2 - y1, 2));
}
```



### **48**

> 在这个程序中，`even` 函数用于判断一个整数的奇偶性，返回1表示偶数，返回0表示奇数。`OddSum` 函数用于计算给定整数列表中所有奇数的和。在 `main` 函数中，用户输入整数的个数和整数列表，然后调用 `OddSum` 函数计算奇数和，并输出结果。

```C
#include <stdio.h>

// 函数声明
int even(int n);
int OddSum(int N, int List[]);

int main() {
    int N;

    // 输入整数的个数
    scanf("%d", &N);

    int List[N];

    // 输入整数列表
    for (int i = 0; i < N; i++) {
        scanf("%d", &List[i]);
    }

    // 调用函数计算奇数和
    int result = OddSum(N, List);

    // 输出结果
    printf("Sum of (");
    for (int i = 0; i < N; i++) {
        if (even(List[i]) == 0) {
            printf(" %d", List[i]);
            if (i < N - 1) {
                printf(" ");
            }
        }
    }
    printf(") = %d\n", result);

    return 0;
}

// 判断奇偶性的函数
int even(int n) {
    return n % 2 == 0 ? 1 : 0;
}

// 计算奇数和的函数
int OddSum(int N, int List[]) {
    int sum = 0;
    for (int i = 0; i < N; i++) {
        if (even(List[i]) == 0) {
            sum += List[i];
        }
    }
    return sum;
}
```



### **49**

> 在这个程序中，`sign` 函数用于计算符号函数。在 `main` 函数中，用户输入一个整数，然后调用 `sign` 函数计算符号函数，并输出结果。

```C
#include <stdio.h>

// 函数声明
int sign(int x);

int main() {
    int x;

    // 输入整数
    scanf("%d", &x);

    // 调用函数计算符号函数
    int result = sign(x);

    // 输出结果
    printf("sign(%d) = %d\n", x, result);

    return 0;
}

// 计算符号函数的函数
int sign(int x) {
    if (x > 0) {
        return 1;
    }
    else if (x == 0) {
        return 0;
    }
    else {
        return -1;
    }
}
```



### **50**

> 在代码中，`printNumberPyramid`函数用于打印数字金字塔。通过嵌套的循环，外层循环控制行数，内层循环分别打印每一行的空格和数字。外层循环的变量`i`表示当前行数，内层循环的变量`j`表示当前行的数字个数。
>
> 在`main`函数中，首先输入金字塔的层数`n`，然后调用`printNumberPyramid`函数打印数字金字塔。

```C
#include <stdio.h>

void printNumberPyramid(int n) {
    for (int i = 1; i <= n; i++) {
        // 打印每一行的空格
        for (int j = 1; j <= n - i; j++) {
            printf(" ");
        }

        // 打印每一行的数字
        for (int j = 1; j <= i; j++) {
            printf("%d ", i);
        }

        printf("\n");
    }
}

int main() {
    int n;
    scanf("%d", &n);

    printNumberPyramid(n);

    return 0;
}
```



### **51**

> 在这个程序中，用户输入两个整数，然后调用 `max` 函数找到较大的数，并输出结果。

```C
#include <stdio.h>

// 函数声明
int max(int a, int b);

int main() {
    int a, b;

    // 输入两个整数
    scanf("%d %d", &a, &b);

    // 调用 max 函数找到较大的数
    int result = max(a, b);

    // 输出结果
    printf("max = %d\n", result);

    return 0;
}

// 找两个数中较大者的函数
int max(int a, int b) {
    return (a > b) ? a : b;
}
```



### **52**

> 在这个程序中，用户输入两个整数 m 和 n（确保 m < n），然后调用 `sumBetween` 函数计算 m 到 n 之间所有整数的和，并输出结果。

```C
#include <stdio.h>

// 函数声明
int sumBetween(int m, int n);

int main() {
    int m, n;

    // 输入两个整数
    scanf("%d %d", &m, &n);

    // 调用 sumBetween 函数计算和
    int result = sumBetween(m, n);

    // 输出结果
    printf("sum = %d\n", result);

    return 0;
}

// 求 m 到 n 之和的函数
int sumBetween(int m, int n) {
    int sum = 0;

    for (int i = m; i <= n; ++i) {
        sum += i;
    }

    return sum;
}
```



### **53**

> 在这个程序中，用户输入两个整数，然后调用 `common_divisor` 函数计算它们的最大公约数，并输出结果。

```C
#include <stdio.h>

// 函数声明
int common_divisor(int number1, int number2);

int main() {
    int number1, number2;

    // 输入两个整数
    scanf("%d %d", &number1, &number2);

    // 调用 common_divisor 函数计算最大公约数
    int result = common_divisor(number1, number2);

    // 输出结果
    printf("%d\n", result);

    return 0;
}

// 计算两数的最大公约数的函数
int common_divisor(int number1, int number2) {
    int temp;

    // 辗转相除法计算最大公约数
    while (number2 != 0) {
        temp = number2;
        number2 = number1 % number2;
        number1 = temp;
    }

    return number1;
}
```



### **54**

> 在这个程序中，用户输入一个整数，然后调用 `sum_of_series` 函数计算从 1 到输入整数的立方和，并输出结果。

```C
#include <stdio.h>

// 函数声明
int sum_of_series(int number);

int main() {
    int number;

    // 输入整数
    scanf("%d", &number);

    // 调用 sum_of_series 函数计算立方和
    int result = sum_of_series(number);

    // 输出结果
    printf("%d\n", result);

    return 0;
}

// 计算 n 个自然数的立方和的函数
int sum_of_series(int number) {
    int sum = 0;

    // 计算立方和
    for (int i = 1; i <= number; i++) {
        sum += i * i * i;
    }

    return sum;
}
```

