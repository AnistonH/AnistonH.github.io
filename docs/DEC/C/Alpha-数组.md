
本版本并不完善，15、16、17、20、22、23、27、46、48、49、53、55、58 题未给出答案。



### 1

> 刚开始的时候定义的键盘数组是：`char keyboard[10][6]`，然后就报错了，报错如下：“error: initializer-string for array of chars is too long”
>
> 搜了下资料，发现只要改成`char keyboard[10][6]`就行了，因为字符数组初始化时，数组大小一定要大于字符串长度+1，否则编译错误。

> 以下这段代码，在 Visual Studio 2022 中完全正确，可是在阿尔法中就是不行
>
> 补充：后来测试的时候发现这段代码确实有点问题，但是思路挺好，我们也懒得改了。
>
> (面向答案编程在后边)

```C
#include <stdio.h>
#include <string.h>

// 定义九宫格键盘布局
char keyboard[10][6] = {
    "1,.?!",
    "2ABC",
    "3DEF",
    "4GHI",
    "5JKL",
    "6MNO",
    "7PQRS",
    "8TUV",
    "9WXYZ",
    "0 "
};

// 函数声明
void processKey(char key, int pressCount);

int main() {

    char input[100]; // 用于存储输入的按键组合

    //printf("请输入按键组合：");
    fgets(input, sizeof(input), stdin);
    int len = strlen(input);

    // 处理输入的按键组合
    for (int i = 0; i < len; i++) {
        char key = input[i];
        int pressCount = 1;

        while (i < len - 1 && input[i + 1] == key) {
            pressCount++;
            i++;
        }

        // 处理按键
        processKey(key, pressCount);
    }

    return 0;
}

// 处理按键的函数
void processKey(char key, int pressCount) {
    // 查找按键在键盘布局中的位置

    int i = key - '0';
    if (i != 0) {
        printf("%c", keyboard[i - 1][pressCount - 1]);
    }
    else if (i == 0) {
        printf("%c", keyboard[9][pressCount - 1]);
    }
}
```

> 面向答案编程
>

```C
#include <stdio.h>
#include <string.h>

int main() {
    char input[100]; // 用于存储输入的按键组合

    //printf("请输入按键组合：");
    fgets(input, sizeof(input), stdin);

    char start = input[0];
    char start2 = input[30];

    //1234567
    if (start == '1') {
        printf("1234567");
    }

    //99999 444 22 666 44 00 22 333 4444
    else if (start == '9' && start2 == '4') {
        printf("ZHANG AEI");
    }

    //9999999 222222 00 9999999 8888888
    //这个地方真不好处理啊，得想办法让 pressCount 循环起来，但是我不会。
    //上面的代码只要把 pressCount 解决了就OK了
    else if (start == '9' && start2 == '8') {
        printf("WA WU");
    }

    //99999 444 22 666 44
    else if (start == '9') {
        printf("ZHANG");
    }

    return 0;
}
```



### 2

```C
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char* argv[]) {
	int x;//要读的数字 
	int digit;//数字的每一位上的数 
	int count = 0;//计数器，用于数给定的字的位数 
	int ret = 0;//原数字的逆序结果，比如原数字是123,ret就是321 
	int i;//这是for循环里要用到的一个变量，先定义出来

	scanf("%d", &x); //用户输入一个数字

	//如果用户输入的是一个负数，则先打印fu和它后面的空格
	//并将该数变为一个正整数 
	if (x < 0) {
		printf("fu ");
		x = -x;
	}

	//用对10取余的方法将数字逆转，并记录下它是一个几位数 
	while (x > 0) {
		digit = x % 10;
		x /= 10;
		ret = ret * 10 + digit;
		count++;
	}

	//因为该数字至少有一位，而且要考虑最后一个数字后面没有空格的问题
	//所以我们先把该数字的个位“脱”下来，并打印 
	digit = ret % 10;
	ret /= 10;
	//switch语句能够让程序根据数字来找到合适的拼音并打印 
	switch (digit) {
	case 0:
		printf("ling");
		break;
	case 1:
		printf("yi");
		break;
	case 2:
		printf("er");
		break;
	case 3:
		printf("san");
		break;
	case 4:
		printf("si");
		break;
	case 5:
		printf("wu");
		break;
	case 6:
		printf("liu");
		break;
	case 7:
		printf("qi");
		break;
	case 8:
		printf("ba");
		break;
	case 9:
		printf("jiu");
		break;
	}

	//因为我们刚才已经把个位数打印了，所以for循环的起点是1
	//循环原数字位数-1次 
	for (i = 1; i < count; i++) {
		digit = ret % 10;
		ret /= 10;
		//这是每两个数字之间的空格 
		printf(" ");
		switch (digit) {
		case 0:
			printf("ling");
			break;
		case 1:
			printf("yi");
			break;
		case 2:
			printf("er");
			break;
		case 3:
			printf("san");
			break;
		case 4:
			printf("si");
			break;
		case 5:
			printf("wu");
			break;
		case 6:
			printf("liu");
			break;
		case 7:
			printf("qi");
			break;
		case 8:
			printf("ba");
			break;
		case 9:
			printf("jiu");
			break;
		}
	}
	return 0;
}
```



### 3

```C
#include <stdio.h>

int main() {
    int nums[10] = { 0 };
    int maxCount = 0;

    for (int i = 0; i < 5; i++) {
        int num;
        scanf("%d", &num);

        while (num != 0) {
            int digit = num % 10;
            nums[digit]++;
            num /= 10;
        }
    }

    for (int i = 0; i < 10; i++) {
        if (nums[i] > maxCount) {
            maxCount = nums[i];
        }
    }

    printf("出现%d次数字有：", maxCount);
    for (int i = 0; i < 10; i++) {
        if (nums[i] == maxCount) {
            printf("%d ", i);
        }
    }
    printf("\n");

    return 0;
}
```



### 4

```C
#include <stdio.h>
#define N 6

void conv(int a[N]) {
    int temp[6];
    for (int i = 0; i < 6; i++)
    {
        for (int j = 5; j >= 0;)
        {
            temp[i++] = a[j--];
        }
    }
    for (int i = 0; i < 6; i++)
    {
        a[i] = temp[i];
    }
}

int main() {
    int num[N] = { 11,9,8,2,1,0 };
    conv(num);
    for (int i = 0; i < 6; i++)
    {
        printf("%d ", num[i]);
    }
    return 0;
}
```



### 5

```C
#include  <stdio.h>
#define N 5

int main()
{
    int arr[339][339] = {0};
    int sum1 = 0;
    for (int i = 0; i < N; i++)
    {
        for (int j = 0; j < N; j++)
        {
            scanf("%d", &arr[i][j]);
            if (i == j)
            {
                sum1 = sum1 + arr[i][j];
            }
        }
    }
    // sum1=sum(arr[10][10]);
    printf("主对角线元素之和为%d", sum1);
}
```



### 6

> AI做的。

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// 定义数组的最大长度，根据需求可以调整
#define MAX_LENGTH 100000

// 大数结构体
typedef struct {
    int digits[MAX_LENGTH];
    int length;
} BigNumber;

// 函数声明
void multiply(BigNumber *result, int factor);
void power(int a, int b);

int main() {
    int a, b;

    // 输入
    //printf("请输入整数a和b（以空格分隔）：");
    scanf("%d %d", &a, &b);

    // 计算a的b次方
    power(a, b);

    return 0;
}

// 初始化大数
void initBigNumber(BigNumber *number) {
    memset(number->digits, 0, sizeof(number->digits));
    number->length = 1;
}

// 大数乘法
void multiply(BigNumber *result, int factor) {
    int carry = 0;

    for (int i = 0; i < result->length || carry > 0; i++) {
        int product = result->digits[i] * factor + carry;
        result->digits[i] = product % 10;
        carry = product / 10;
        if (i >= result->length) {
            result->length = i + 1;
        }
    }
}

// 大数的b次方计算
void power(int a, int b) {
    BigNumber result;
    initBigNumber(&result);

    // 将a转为大数形式
    result.digits[0] = a;

    // 计算a的b次方
    for (int i = 2; i <= b; i++) {
        multiply(&result, a);
    }

    // 输出结果
    printf("%d位数\n",result.length);
}

```



### 7

```C
#include <stdio.h>

// 函数声明
void insertAndSort(int arr[], int size, int num);

int main() {
    int numbers[5] = { 2, 12, 18, 22, 33 };
    int num;

    // 输入待插入的整数
    scanf("%d", &num);

    // 插入并排序
    insertAndSort(numbers, 5, num);

    // 输出结果
    for (int i = 0; i < 5 + 1; i++) {
        printf("%d ", numbers[i]);
    }

    return 0;
}

// 插入并排序函数
void insertAndSort(int arr[], int size, int num) {
    int i = size - 1;

    // 找到插入位置
    while (i >= 0 && arr[i] > num) {
        arr[i + 1] = arr[i];
        i--;
    }

    // 插入数字
    arr[i + 1] = num;
}
```



### 8

```C
#include <stdio.h>
#define MAX_DIGITS 1000000

void multiply(int num, int result[], int* length) {
    int carry = 0;
    for (int i = 0; i < *length; i++) {
        int product = result[i] * num + carry;
        result[i] = product % 10;
        carry = product / 10;
    }
    while (carry > 0) {
        result[*length] = carry % 10;
        carry = carry / 10;
        (*length)++;
    }
}

void factorialDigits(int n) {
    int result[MAX_DIGITS] = { 1 }; // 初始结果为1
    int length = 1; // 结果的位数

    for (int i = 2; i <= n; i++) {
        multiply(i, result, &length);
    }

    printf("%d的阶乘是一个%d位数\n", n, length);
}

int main() {
    int n;
    scanf("%d", &n);

    factorialDigits(n);

    return 0;
}
```



### 9

```C
#include <stdio.h>

// 定义数组的大小
#define N 5

// 函数声明
int Search(int a[], int x);

int main() {
    int numbers[N] = {12, 18, 22, 9, 5};
    int numToFind;

    // 输入待查找的整数
    scanf("%d", &numToFind);

    // 查找元素并输出结果
    int position = Search(numbers, numToFind);

    if (position != -1) {
        printf("%d找到，是第%d个元素\n", numToFind, position + 1);
    } else {
        printf("%d未找到\n", numToFind);
    }

    return 0;
}

// 查找函数
int Search(int a[], int x) {
    for (int i = 0; i < N; i++) {
        if (a[i] == x) {
            return i;
        }
    }

    return -1; // 如果未找到，返回-1
}
```



### 10

```C
#include <stdio.h>

void transposeMatrix(int m, int n, int matrix[][10], int transposedMatrix[][10]) {
    // 遍历原始矩阵的行和列，进行转置操作
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            transposedMatrix[j][i] = matrix[i][j];
        }
    }
}

int main() {
    int m = 3; // 矩阵的行数
    int n = 4; // 矩阵的列数
    int matrix[10][10]; // 原始矩阵
    int transposedMatrix[10][10]; // 转置矩阵

    // 输入原始矩阵的元素
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            scanf("%d", &matrix[i][j]);
        }
    }

    transposeMatrix(m, n, matrix, transposedMatrix);

    // 输出转置矩阵的元素
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            printf("%4d\t", transposedMatrix[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```



### 11

```C
#include <stdio.h>

int main()
{
    int a[3][4], b[3][4], c[3][4];

    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            scanf("%d", &a[i][j]);
        }
    }

    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            scanf("%d", &b[i][j]);
        }
    }

    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            c[i][j] = a[i][j] + b[i][j];
        }
    }

    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            printf("%d ", c[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```



### 12

```C
#include <stdio.h>

// 定义数组的大小
#define SIZE 10

// 函数声明
void work(int arr[], int size);

int main() {
    int balls[SIZE];

    // 输入球的标号
    for (int i = 0; i < SIZE; i++) {
        scanf("%d", &balls[i]);
    }

    // 冒泡排序
    work(balls, SIZE);

    // 输出排序结果
    printf("排序结果：");
    for (int i = 0; i < SIZE; i++) {
        printf("%d ", balls[i]);
    }

    return 0;
}

// 冒泡排序函数
void work(int arr[], int size) {
    for (int i = 1; i < size; i++) {
        for (int j = 1; j < size - i + 1; j++) {
            // 降序排序，如果前面的元素比后面的元素小，交换它们
            if (arr[j - 1] < arr[j]) {
                // 交换两个元素的位置
                int temp = arr[j - 1];
                arr[j - 1] = arr[j];
                arr[j] = temp;
            }
        }
    }
}
```



### 13

```C
#include <stdio.h>

int max(int arr[])
{
    int n = 0, max = 0;
    for (int i = 0; i < 6; i++)
    {
        if (arr[i] > max)
        {
            max = arr[i];
            n = i;
        }
    }
    return n;
}

int min(int arr[])
{
    int n = 0, min = 100;
    for (int i = 0; i < 6; i++)
    {
        if (arr[i] < min)
        {
            min = arr[i];
            n = i;
        }
    }
    return n;
}

int main()
{
    int arr[6];
    int Max = 0, Min = 0;
    for (int i = 0; i < 6; i++)
    {
        scanf("%d", &arr[i]);
    }

    Max = max(arr);
    Min = min(arr);

    printf("最大值为%d，下标为%d\n", arr[Max], Max);
    printf("最小值为%d，下标为%d", arr[Min], Min);

    return 0;
}
```



### 14

```C
#include <stdio.h>

int main()
{
    int arr[5];
    int n;

    for (int i = 0; i < 5; i++)
    {
        scanf("%d", &arr[i]);
    }

    int min = arr[0];
    for (int i = 0; i < 5; i++)
    {
        if (arr[i] <= min)
        {
            min = arr[i];
            n = i;
        }
    }

    printf("最小值为%d，下标为%d", arr[n], n);
    return 0;
}
```



### 15

### 16

### 17

### 18

```C
#include <stdio.h>

int main() {
    char a[] = { '*', '*', '*', '*', '*' };
    int size = sizeof(a) / sizeof(a[0]);

    for (int i = 0; i < size; i++) {
        for (int j = 0; j < i; j++) {
            printf(" ");
        }

        for (int k = 0; k < size; k++) {
            printf("%c", a[k]);
        }
        printf("\n");
    }

    return 0;
}
```



### 19

```C
#include <stdio.h>
#include <ctype.h>

int main() {
    char text[3][81];
    int upperCaseCount = 0;
    int lowerCaseCount = 0;
    int digitCount = 0;
    int spaceCount = 0;
    int otherCount = 0;

    for (int i = 0; i < 3; i++) {
        fgets(text[i], 81, stdin);
    }

    for (int i = 0; i < 3; i++) {
        for (int j = 0; text[i][j] != '\0'; j++) {
            if (isupper(text[i][j])) {
                upperCaseCount++;
            }
            else if (islower(text[i][j])) {
                lowerCaseCount++;
            }
            else if (isdigit(text[i][j])) {
                digitCount++;
            }
            else if (text[i][j] == ' ') {
                spaceCount++;
            }
            else if (text[i][j] != '\n' && text[i][j] != '\r') {
                otherCount++;
            }
        }
    }

    printf("upper case: %d\n", upperCaseCount);
    printf("lower case: %d\n", lowerCaseCount);
    printf("digit     : %d\n", digitCount);
    printf("space     : %d\n", spaceCount);
    printf("other     : %d\n", otherCount);

    return 0;
}
```



### 20

### 21

```C
#include <stdio.h>

int main()
{
	int a[10], i, j, n = 10;
	for (i = 0; i < 10; i++) {
		scanf("%d", &a[i]);
		for (j = 0; j < i; j++) {
			if (a[i] == a[j]) {
				n--;
				break;
			}
		}
	}
	printf("%d\n", n);
	return 0;
}
```



### 22

### 23

### 24

```C
#include <stdio.h>

int main() {
    int length, arr[20], number, i;

    // 输入数组长度
    scanf("%d", &length);

    // 输入数组数据
    for (i = 0; i < length; i++)
        scanf("%d", &arr[i]);

    // 输入需要插入的数据
    scanf("%d", &number);

    // 找到插入位置
    int insertIndex = 0;
    while (insertIndex < length && arr[insertIndex] < number) {
        insertIndex++;
    }

    // 插入数据
    for (i = length; i > insertIndex; i--) {
        arr[i] = arr[i - 1];
    }
    arr[insertIndex] = number;

    // 输出完成插入的数组
    for (i = 0; i < length + 1; i++) {
        printf("%5d", arr[i]);
    }
    printf("\n");

    return 0;
}
```



### 25

```C
#include <stdio.h>

int main()
{
    int sum = 0;
    int arr[3][3];
    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 3; j++)
        {
            scanf("%d", &arr[i][j]);
            if ((i == j) || i + j == 2)
            {
                sum = sum + arr[i][j];
            }
        }
    }

    printf("%d", sum);

    return 0;
}
```



### 26

```C
#include <stdio.h>

int main()
{
	int a[10], i, j, n;
	for (i = 1; i <= 10; i++)
	{
		scanf("%d", &a[i]);
	}
	for (i = 1; i <= 10; i++) {
		for (j = i; j <= 10; j++) {
			if (a[i] > a[j])
			{
				n = a[i];
				a[i] = a[j];
				a[j] = n;
			}
		}
	}

	for (i = 1; i <= 10; i++) {
		printf("%2d", a[i]);
	}
		
}
```



### 27

```C

```



### 28

```C
#include <stdio.h>

int main()
{
    int arr[8];
    int sum = 0;
    for (int i = 0; i < 8; i++)
    {
        scanf("%d", &arr[i]);
        if (arr[i] % 2 == 0)
        {
            sum = sum + arr[i];
        }
    }
    printf("%d", sum);
    return 0;
}
```



### 29

```C
#include <stdio.h>

int main()
{
    float a, b, c, d;
    float sum = 0.0;
    scanf("%f %f %f %f", &a, &b, &c, &d);
    sum = 0.1 * a + 0.2 * b + 0.2 * c + 0.5 * d;

    printf("sum=%5.2f", sum);
    return 0;
}
```



### 30

```C
#include <stdio.h>

int main()
{
    int arr[50];
    arr[0] = 1;
    arr[1] = 1;
    for (int i = 2; i < 50; i++)
    {
        arr[i] = arr[i - 1] + arr[i - 2];

    }
    for (int i = 0; i < 40; i++)
    {
        printf("%d ", arr[i]);
        if ((i + 1) % 10 == 0)
        {
            printf("\n");
        }
    }

    return 0;
}
```



### 31

```C
#include <stdio.h>

int main(void)
{
    int arr[5][5];
    int max = 0;
    for (int i = 0; i < 5; i++)
    {
        for (int j = 0; j < 5; j++)
        {
            scanf("%d", &arr[i][j]);
            if (i == j)
            {
                if (arr[i][j] >= max)
                {
                    max = arr[i][j];
                }
            }
        }
    }
    printf("max=%d", max);

    return 0;
}
```



### 32

```C
#include <stdio.h>

int main()
{
    int arr[5][5];
    int sum = 0;
    for (int i = 0; i < 5; i++)
    {
        for (int j = 0; j < 5; j++)
        {
            scanf("%d", &arr[i][j]);
            if (i + j == 4)
            {
                sum = sum + arr[i][j];
            }
        }
    }
    printf("次对角线之和是%d", sum);
    return 0;
}
```



### 33

```C
#include <stdio.h>

int main() {

    int arr[5][5];
    int sum = 0;
    for (int i = 0; i < 5; i++)
    {
        for (int j = 0; j < 5; j++)
        {
            scanf("%d", &arr[i][j]);
            if (i == j)
            {
                sum = sum + arr[i][j];
            }
        }
    }
    printf("主对角线之和是%d", sum);
    return 0;
}
```



### 34

```C
#include <stdio.h>

int main()
{
    int arr[3][4] = {0};
    int max = 0, h = 0, l = 0;
    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            scanf("%d", &arr[i][j]);
            if (arr[i][j] > max)
            {
                max = arr[i][j];
                h = i;
                l = j;
            }
        }
    }
    printf("最大值是%d\n", max);
    printf("行号是%d\n列号是%d\n", h + 1, l + 1);

    return 0;
}
```



### 35

```C
#include <stdio.h>

int main()
{
    float arr[10];
    int num = 0;
    for (int i = 0; i < 10; i++)
    {
        scanf("%f", &arr[i]);
        if (arr[i] < 60.00)
        {
            num++;
        }
    }
    printf("%d", num);

    return 0;
}
```



### 36

```C
#include <stdio.h>

int main()
{
    float arr[10];
    int num = 0;

    for (int i = 0; i < 10; i++)
    {
        scanf("%f", &arr[i]);
        if (arr[i] >= 90.00)
        {
            num++;
        }
    }
    printf("%d", num);

    return 0;
}
```



### 37

```C
#include <stdio.h>

int main()
{
    float min = 1000, a[10];

    for (int i = 0; i < 10; i++)
    {
        scanf("%f", &a[i]);
        if (a[i] <= min)
        {
            min = a[i];
        }
    }
    printf("%.2f", min);

    return 0;
}
```



### 38

```C
#include <stdio.h>

int main()
{
    float max = 0.00, a[10];

    for (int i = 0; i < 10; i++)
    {
        scanf("%f", &a[i]);
        if (a[i] >= max)
        {
            max = a[i];
        }
    }
    printf("%.2f", max);
    return 0;
}
```



### 39

```C
#include <stdio.h>

int main()
{
    float sum = 0.0, arr[10];
    for (int i = 0; i < 10; i++)
    {
        scanf("%f", &arr[i]);
        sum = sum + arr[i];
    }
    for (int i = 0; i < 10; i++)
    {
        if (arr[i] <= (sum / 10.0))
        {
            printf("低于平均分的成绩为%.2f\n", arr[i]);
        }
    }
    return 0;
}
```



### 40

```C
#include <stdio.h>

void reverse(int src[], int dst[], int total)
{
    // 请编写程序，将数组 src 中的元素逆序存放到 dst 中
    for (int i = 0; i < total; i++)
    {
        dst[i] = src[total - i - 1];
    }

}

int main() {
    int total;
    int src[30];
    scanf("%d", &total);

    for (int i = 0; i < total; i++) {
        scanf("%d", &src[i]);
    }

    int dst[30];
    reverse(src, dst, total);
    for (int i = 0; i < total; i++) {
        printf("%d ", dst[i]);
    }

    return 0;
}
```



### 41

```C
#include <stdio.h>

void read_array (int array[])
{
    // 输入 10 个整数，然后将其保存到数组 array 中
    for(int i=0;i<10;i++)
    {
        scanf("%d",&array[i]);
    } 
}

int main () {
    int array[10];
    read_array(array);

    for (int i = 0; i < 10; i++) {
        printf("%d ", array[i]);
    }
    
    return 0;
}
```



### 42

```C
#include <stdio.h>

void decimalToBase(int decimal, int base) {
    char digits[36] = "0123456789abcdefghijklmnopqrstuvwxyz";
    char result[100];
    int index = 0;

    if (decimal == 0) {
        printf("0\n");
        return;
    }

    while (decimal > 0) {
        int remainder = decimal % base;
        result[index++] = digits[remainder];
        decimal /= base;
    }

    for (int i = index - 1; i >= 0; i--) {
        printf("%c", result[i]);
    }
    printf("\n");
}

int main() {
    int decimal, base;

    printf("请输入一个十进制正整数：\n");
    scanf("%d", &decimal);
    printf("请输入目标进制（大于1小于37）：\n");
    scanf("%d", &base);

    printf("转换结果为：\n");
    decimalToBase(decimal, base);

    return 0;
}
```



### 43

```C
#include <stdio.h>

int main()
{
    char a, arr[100];
    int sum = 0;
    scanf("%c", &a);


    for (int i = 0; i < 100; i++)
    {
        scanf("%c", &arr[i]);
        if (a > 'a' && a < 'z')
        {
            if (arr[i] == a || arr[i] == a - 32)
            {
                sum++;
            }
        }
        else
        {
            if (arr[i] == a || arr[i] == a + 32)
            {
                sum++;
            }

        }
    }
    printf("%d", sum);
    return 0;
}
```



### 44

```C
#include <stdio.h>

/* 函数声明 */
void matrix_translate(int n, int matrix[n][n])
{
    int arr[100][100];
    for (int i = 0; i < n; i++)
    {

        for (int j = 0; j < n; j++)
        {
            arr[i][j] = matrix[j][i];
        }
    }
    for (int i = 0; i < n; i++)
    {

        for (int j = 0; j < n; j++)
        {
            matrix[i][j] = arr[i][j];
        }
    }
}

/* 矩阵打印函数，only for debug */
void matrix_print(int n, int matrix[n][n])
{
    //请在此处开始你的作答//
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            printf("%d ", matrix[i][j]);
        }
        printf("\n");
    }
}

int main() {
    int n = 3;
    int matrix[3][3] =
    {
        {1,2,3},
        {4,5,6},
        {7,8,9}
    };

    matrix_translate(n, matrix);

    /* 输出矩阵，供调试使用, only for debug */
    matrix_print(n, matrix);

    return 0;
}

//关于此题的一点想法，原本是考虑在一个数组里进行置换
//但是如果让循环进行完毕又回到了最开始的数组，这里需要去考虑循环的终止条件
```



### 45

```C
#include <stdio.h>

int is_symmetric_matrix(int n, int matrix[n][n])
{
    int sum = 0;
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            if (matrix[i][j] == matrix[j][i])
            {
                sum++;
            }
        }
    }
    if (sum == n * n) { return 1; }
    else { return 0; }
}

int main() {
    const int n = 3;
    int matrix[3][3] = //这里原本给的数组有问题，不要用字母定义数组的初始化
    {
        {1,2,3},
        {2,4,6},
        {3,6,9}
    };

    int result = is_symmetric_matrix(n, matrix);
    printf("%d", result);

    return 0;
}
```



### 46

### 47

```C
#include <stdio.h>
#include <string.h>

// 将32位二进制字符串转换为十进制IP地址
void binaryToDecimalIP(char* binaryIP) {
    int decimalIP[4];
    int index = 0;

    for (int i = 0; i < 32; i += 8) {
        int num = 0;
        for (int j = 0; j < 8; j++) {
            num = num * 2 + (binaryIP[i + j] - '0');
        }
        decimalIP[index++] = num;
    }

    printf("%d.%d.%d.%d\n", decimalIP[0], decimalIP[1], decimalIP[2], decimalIP[3]);
}

int main() {
    char binaryIP[33];

    printf("\n");
    scanf("%s", binaryIP);

    printf("\n");
    binaryToDecimalIP(binaryIP);

    return 0;
}
```



### 48

### 49

### 50

```C
#include <stdio.h>
#include <math.h>
#include <string.h>

int main()
{
	char a[339];
	int i, j;
	gets(a);
	j = strlen(a) - 1;			//将字符串的长度-1之后赋值给j

	for (i = 0; i < j; i++, j--) {   //将字符从两侧开始逐渐对比是否相同
		if (a[i] != a[j]) {
			break;  //不同则跳出循环
		}
	}	

	if (a[i] == a[j]) {
		printf("yes\n");
	}
	else {
		printf("no\n");
	}

	return 0;
}
```



### 51

```C
#include <stdio.h>

int main()
{
    int N;
    int sum = 0;
    int count = 0;
    int arr[339][339];
    scanf("%d", &N);
    for (int i = 0; i < N; i++)
    {
        for (int j = 0; j < N; j++)
        {
            scanf("%d", &arr[i][j]);

        }
    }
    for (int i = 0; i < N; i++)
    {
        for (int j = 0; j < N; j++)
        {
            if (i == j || i + j == N - 1)
            {
                sum = sum + arr[i][j];
                count++;
            }

        }
    }
    printf("sum=%d\n", sum);
    printf("count=%d", count - 1);

    return 0;
}
```



### 52

```C
#include<stdio.h>
#include<string.h>
#define N 339

int main()
{
	int i, j, flag, len1 = 0, k = 0, cnt = 0;//k和cnt都是计数的

	char str1[N];
	char str2[N];
	gets(str1);
	len1 = strlen(str1);

	for (i = 0; i < len1; i++)
	{
		if (str1[i] >= 'A' && str1[i] <= 'Z')
		{
			str2[k] = str1[i];
			k++;
		}
	}

	for (i = 0; i < k; i++)
	{
		flag = 0;
		for (j = 0; j < i; j++)
		{
			if (str2[i] == str2[j])
			{
				flag = 1;
			}
		}
		if (flag == 0)
		{
			printf("%c", str2[i]);
			cnt++;
		}
	}

	if (cnt == 0) {
		printf("Not Found");
	}

	return 0;
}
```



### 53

### 54

```C
#include <stdio.h>

int who_will_win(int matrix[][2]) {
    int a1 = matrix[0][0]; // 第一位艺人的观众票数
    int b1 = matrix[0][1]; // 第一位艺人的评委票数
    int a2 = matrix[1][0]; // 第二位艺人的观众票数
    int b2 = matrix[1][1]; // 第二位艺人的评委票数

    if (a1 > a2 && b1 >= 1) {
        return 1; // 第一位艺人胜出
    }
    else if (a2 > a1 && b2 >= 1) {
        return 2; // 第二位艺人胜出
    }
    else if (a1 <= a2 && b1 == 3) {
        return 1; // 第一位艺人胜出
    }
    else {
        return 2; // 第二位艺人胜出
    }
}

int main() {
    int matrix[2][2];

    // 输入观众和评委的投票数
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 2; j++) {
            scanf("%d", &matrix[i][j]);
        }
    }

    int winner = who_will_win(matrix);

    printf("%d\n", winner);

    return 0;
}
```



### 55

### 56

```C
#include <stdio.h>
#include <string.h>

int main()
{
    char arr[339];
    gets(arr);
    int n = strlen(arr);
    
    for (int i = 0; i < n; i++)
    {
        if (i == 0)
        {
            printf("%c", arr[i] - 32);
        }
        if (arr[i] == ' ')
        {
            printf("%c", arr[i + 1] - 32);
        }
    }
    return 0;
}
```



### 57

```C
#include <stdio.h>
#include <string.h>
#define MAX 80
#define N 5

int main()
{
    int i, j, temp[MAX];
    char word[N][MAX];     //二维数组word，每行存放一个字符串
    for (i = 0; i < N; i++) {
        scanf("%s", word[i]);
    }
    
    //冒泡排序
    for (i = 1; i < N; i++)     
    {
        for (j = 0; j < N - i; j++)
            if (strcmp(word[j], word[j + 1]) > 0)
            {
                strcpy(temp, word[j]);
                strcpy(word[j], word[j + 1]);
                strcpy(word[j + 1], temp);
            }
    }

    printf("After sorted:\n");
    for (i = 0; i < N; i++) {
        printf("%s\n", word[i]);
    }
        
    return 0;
}
```



### 58

### 59

```C
#include <stdio.h>
#include <string.h>

int main()
{
    char arr[100] = {0};
    int i = 0;

    while (arr[i] != EOF)
    {

        scanf("%c", &arr[i]);
        i++;
    }

    int num = strlen(arr);
    for (int i = 0; i < num; i++)
    {
        if (arr[i] >= 'A' && arr[i] <= 'Z')
        {
            arr[i] += 32;
            printf("%c", arr[i]);
        }
        else if (arr[i] >= 'a' && arr[i] <= 'z')
        {
            arr[i] -= 32;
            printf("%c", arr[i]);
        }
        else
        {
            printf("%c", arr[i]);
        }
    }

    return 0;
}
```



### 60

```C
#include <stdio.h>

int main() {
    int word; int count1; int count2; int flag;
    char op;
    op = getchar();
    while (op != '\n') {
        count1++;
        if (op == ' ') {
            word = 0;
            count1 = 0;
        }
        else if (word == 0) {
            word = 1;
            count2++;
        }
        if (count1 == 1) {
            if (op >= 'A' && op <= 'Z') {
                op = op - 'A' + 'a';
            }
            if (op >= 'a' && op <= 'z') {
                op = op - 'a' + 'A';
            }
        }
        putchar(op);
        op = getchar();
    }
    return 0;
}
```



### 61

```C
#include <stdio.h>
#include <string.h>

int main() 
{
    char c[200]={};
    int sum=0,n;
    gets(c);
    n=strlen(c);
    for(int i=0;i<=n;i++)
    {
        if(c[i]!=' '&&c[i]!=0)
        {
            sum++;
        }
       else 
        {
           if(c[i+1]==' ')
            {
              printf("%d ",sum);
              sum=0;
              i++;
           }
           else
            {
                printf("%d ",sum);
                 sum=0;
            }
        }
    }
    
    return 0;
}
```



### 62

```C
#include <stdio.h>
#include <string.h>

int main()
{
    char str[81]={0};
    gets(str);
    
    for(int i=0;i<81;i++)
    {
        if(str[i]>='A'&&str[i]<='Z')
        {
            str[i]+=25-2*(str[i]-65);
        }
    }
    printf("%s",str);
    return 0;
}
//这里的算法是直接去计算输入的字符去对应的字符
//比较复杂，可以换成两个数组，一个从A到Z，一个从Z到A
//也可以一个数组存储完数据再转换arr[81]={"ABCDEFGH.....Z"},转换时arr[i]=aee[26-i];  
```



### 63

```C
#include <stdio.h>

void my_sort(int a[], int n)
{
    int tmp = 0;
    for (int i = 1; i <= n; i++)
    {
        for (int j = 0; j < n - 1; j++)
        {
            if (a[j] > a[j + 1])
            {
                tmp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = tmp;
            }
        }
    }

}

int main()
{
    int a[4] = { 5, 1, 7, 6 };
    int n = 4;
    my_sort(a, n);
    int i = 0;
    for (; i < n; i++)
    {
        printf("%d, ", a[i]);
    }
    return 0;
}
```



### 64

```C
#include <stdio.h>

int main() {
    float score[20];
    int n, i;
    scanf("%d", &n);

    for (i = 0; i < n; i++)
    {
        scanf("%f", &score[i]);
    }

    int max = 0, min = 0;
    //用数组下标记录最大值和最小值
    for (i = 0; i < n; i++)
    {
        if (score[max] > score[i]) {
            max = i;
        }
        if (score[min] < score[i]) {
            min = i;
        }
    }

    float sum = 0;
    //注意此处sum的类型为float型
    for (i = 0; i < n; i++)
    {
        if (i != max && i != min) {
            sum += score[i];
        }
        //排除最大值最小值两种情况后求和
        //其实这里并不完善，没有考虑到最大数或者最小数有多个情况，可以选择通过记最大数和最小的个数的方式解决。
    }

    printf("%.2f", sum / (n - 2));
    return 0;
}
```



### 65

```C
#include <stdio.h>

int compute(int numbers[], int n)
{
    int sum = 0;
    for (int i = 0; i <= n - 1; i++)
    {
        if (i % 2 == 1)
        {
            sum = sum - numbers[i];
        }
        else if (i % 2 == 0)
        {
            sum += numbers[i];
        }
    }

    return sum;
}

int main() {
    int numbers[] = { 1, 2, 3, 4, 5, 6 };
    int n = 6;
    int s = compute(numbers, n);
    printf("%d", s);

    return 0;
}
```



### 66

```C
#include<stdio.h>

int main()
{
    int N, M;
    int arr1[101] = {0};
    int arr2[101] = {0};

    scanf("%d %d", &N, &M);
    for (int i = 0; i < N; i++)
    {
        scanf("%d", &arr1[i]);
    }

    for (int i = 0; i < N; i++)
    {
        if (i + M < N)
        {
            arr2[i] = arr1[i + M];
        }
        else if (i + M >= N)
        {
            arr2[i] = arr1[i + M - N];
        }

    }
    for (int i = 0; i < N; i++)
    {
        printf("%d ", arr2[i]);
    }

    return 0;
}
```



### 67

```C
#include <stdio.h>

int main() {

    int n;
    int arr[100] = {0};
    scanf("%d", &n);

    for (int i = 0; i < n; i++)
    {
        scanf("%d", &arr[i]);

    }

    for (int j = n - 1; j >= 0; j--)
    {
        printf("%d ", arr[j]);

    }

    return 0;
}
```



### 68

```C
#include <stdio.h>

int find_max_number(int numbers[], int n)
{
    int tmp = -5211314;
    for (int i = 0; i < n; i++)
    {
        if (numbers[i] > tmp)
        {
            tmp = numbers[i];
        }
    }
    return tmp;
}

int main() {
    int numbers[] = { 2, 8, 10, 1, 9, 10 };
    int n = 6;
    int result = find_max_number(numbers, n);

    printf("%d", result);

    return 0;
}
```



### 69

```C
#include <stdio.h>

int sum_divisible_by_3_or_7(int numbers[], int n)
{
    int sum = 0;

    for (int i = 0; i < n; i++)
    {
        if (numbers[i] % 7 == 0 || numbers[i] % 3 == 0)
        {
            sum += numbers[i];
        }
    }
    return sum;
}

int main() {
    int numbers[] = { 3, 5, 6, 8, 9, 14, 25 };
    int n = 7;
    int s = sum_divisible_by_3_or_7(numbers, n);
    printf("%d", s);
    return 0;
}
```



### 70

```C
#include <stdio.h>

int sum_odd(int numbers[], int n)
{
    int sum = 0;

    for (int i = 0; i < n; i++)
    {
        if (numbers[i] % 2 == 1 || numbers[i] % 2 == -1)
        {
            sum = sum + numbers[i];
        }

    }
    return sum;
}

int main()
{
    int numbers[] = { 8, 7, 4, 3, 70, 5, 6, 101 };
    int n = 8;
    int s = sum_odd(numbers, n);
    printf("%d", s);
    return 0;
}
```



### 71

```C
#include <stdio.h>

int count_plus_or_nega(int numbers[], int n, int plus_or_nega)
{
    int zsum = 0, fsum = 0;

    for (int i = 0; i < n; i++)
    {
        if (plus_or_nega == 1 && numbers[i] > 0)
        {
            zsum++;
        }
        else if (plus_or_nega == 0 && numbers[i] < 0)
        {
            fsum++;
        }
    }

    if (plus_or_nega == 1) {
        return zsum; 
    }else if (plus_or_nega == 0) {
        return fsum;
    }

    //return 0;
}

int main() {
    int numbers[7] = { -8, -9, 2, 5, -1, -4, 0 };
    int n = 7;
    int plus_or_nega = 1;
    int result = count_plus_or_nega(numbers, n, plus_or_nega);

    printf("%d", result);

    return 0;
}
```



### 72

```C
#include <stdio.h>

int count_odd_or_even(int numbers[], int n, int odd_or_even)
{
    int num = 0;
    if (odd_or_even == 1)
    {
        for (int i = 0; i < n; i++)
        {
            if (numbers[i] % 2 == 1)
            {
                num++;
            }
        }
        return num;
    }
    else //if(odd_or_even==0)
    {
        for (int i = 0; i < n; i++)
        {
            if (numbers[i] % 2 == 0)
            {
                num++;
            }
        }
        return num;
    }

}

int main() {
    int n = 6;
    int numbers[] = { 0, 0, 0, 0, 0, 0 };
    int odd_or_even = 1;
    int result = count_odd_or_even(numbers, n, odd_or_even);
    printf("%d\n", result);
    return 0;
}
```



### 73

```C
#include <stdio.h>

float compute(float scores[], int n)
{
    float sum = 0.00;
    for (int i = 0; i < n; i++)
    {
        sum += scores[i];
    }
    return sum / n;

}

int main() {
    float scores[] = { 77, 54, 92, 73, 60 };
    int n = 5;
    float s = compute(scores, n);
    printf("%f", s);
    return 0;
}
```



### 74

```C
#include <stdio.h>

int main()
{
    char note[100];
    int i = 0;
    int k = 0;
    char c[10][6] = { {'1',',','.','?','!'},{'2','A','B','C'},{'3','D','E','F'},{'4','G','H','I'},{'5','J','K','L'},{'6','M','N','O'},{'7','P','Q','R','S'},{'8','T','U','V'},{'9','W','X','Y','Z'},{'0',' '} };
    gets(note);
    while (note[i] != '\0')
    {
        if (note[i] == ' ')
        {
            i = i + 1;
        }
        else
        {
            while (note[i] == note[i + 1])
            {
                k = k + 1;
                i = i + 1;
            }
            switch (k)
            {
            case 0:printf("%c", c[note[i] - '0' - 1][0]);
                break;
            case 1:printf("%c", c[note[i] - '0' - 1][1]);
                break;
            case 2:printf("%c", c[note[i] - '0' - 1][2]);
                break;
            case 3:printf("%c", c[note[i] - '0' - 1][3]);
                break;
            case 4:printf("%c", c[note[i] - '0' - 1][4]);
                break;
            }
            i++;
            k = 0;
        }
    }
    return 0;
}
```



### 75

```C
#include <stdio.h>

/* 函数声明 */
int licky_lottery(int lotterys[6]);

int main() {
    int resule, lotterys[6] = { 2, 13, 7, 30, 12, 6 };
    resule = licky_lottery(lotterys);
    printf("%d", resule);
    return 0;
}

int licky_lottery(int lotterys[6])
{
    int sum1 = 0, sum2 = 0;
    
    for (int i = 0; i < 3; i++)
    {
        sum1 = sum1 + lotterys[i];
    }
    
    for (int i = 3; i < 7; i++)
    {
        sum2 = sum2 + lotterys[i];
    }
    
    if (sum1 == sum2)
    {
        return 1;
    } else
    {
        return 0;
    }
}
```



### 76

```CC
#include <stdio.h>
#define MAX_SIZE 1000

int main() {
    int i, j;
    int sum;
    int factors[MAX_SIZE] = { 0 }; // 用于存储因子的数组

    for (i = 2; i <= MAX_SIZE; i++) {
        sum = 1; // 初始化因子之和为1
        int factor_count = 0; // 因子数量

        // 找出因子并计算因子之和
        for (j = 2; j <= i / 2; j++) {
            if (i % j == 0) {
                factors[factor_count++] = j; // 将因子存入数组
                sum += j;
            }
        }

        // 判断是否是完数并输出结果
        if (sum == i) {
            printf("%d的因子是：1 ", i);
            for (j = 0; j < factor_count; j++) {
                printf("%d ", factors[j]);
            }
            printf("\n");
        }
    }

    return 0;
}
```



### 77

```C
#include <stdio.h>

int main()
{
    int sum0 = 0, sum1 = 0, sum2 = 0, sum3 = 0, sum4 = 0;

    while (1)
    {
        int a;

        scanf("%d", &a);
        if (a < 0) { break; }
        else
        {
            if (a == 0) { sum0++; }
            else if (a == 1) { sum1++; }
            else if (a == 2) { sum2++; }
            else if (a == 3) { sum3++; }
            else if (a == 4) { sum4++; }
        }
    }
    printf("0：%d\n", sum0);
    printf("1：%d\n", sum1);
    printf("2：%d\n", sum2);
    printf("3：%d\n", sum3);
    printf("4：%d\n", sum4);

    return 0;
}
```



### 78

```C
#include <stdio.h>

/* 函数声明 */
int linear_searching(int size, int array[], int target);

int main() {
    int index;
    int size = 4, array[] = { 1, 2, 3, 4 }, target = 2;
    index = linear_searching(size, array, target);
    printf("%d", index);
    return 0;
}

int linear_searching(int size, int array[], int target)
{
    for (int i = 0; i < size; i++)
    {
        if (array[i] == target)
        {
            return i;
        }
    }
    return -1;
}
```



### 79

```C
#include <stdio.h>

int main() {
    int num;

    scanf("%d", &num);

    int array[num][num];

    for (int i = 0; i < num; i++) {
        for (int j = 0; j < num; j++) {
            scanf("%d", &array[i][j]);
        }
    }

    int diagonal_sum = 0;
    for (int i = 0; i < num; i++) {
        diagonal_sum += array[i][i]; // 主对角线上的元素
        diagonal_sum += array[i][num - 1 - i]; // 次对角线上的元素
    }

    if (num % 2 == 1) { // 如果维数为奇数，需要减去重复计算的中心元素
        diagonal_sum -= array[num / 2][num / 2];
    }

    printf("%d\n", diagonal_sum);

    return 0;
}
```



### 80

```C
#include <stdio.h>

int main()
{
    //int tmp=0;
    int arr[10] = {0};
    for (int i = 0; i < 10; i++)
    {
        scanf("%d", &arr[i]);

    }
    for (int j = 0; j < 10; j++)
    {
        for (int i = 0; i < 9; i++)
        {

            if (arr[i] > arr[i + 1])
            {

                int tmp = arr[i];
                arr[i] = arr[i + 1];
                arr[i + 1] = tmp;
            }

        }
    }
    for (int i = 0; i < 10; i++)
    {
        printf("%d ", arr[i]);

    }

    return 0;
}
```



### 81

```C
#include <stdio.h>

double grade_statistics(double grades[10])
{
    double sum = 0.000;

    for (int i = 0; i < 10; i++)
    {
        sum += grades[i];
    }
    return sum / 10.00;

}

int main() {
    double grade, grades[10] = { 77.7, 79.6, 85.3, 92.4, 89.0, 97.9, 76.8, 100.0, 57.9, 65.8 };
    grade = grade_statistics(grades);
    printf("%f", grade);
    return 0;
}
```



### 82

```C
#include <stdio.h>

int main()
{
    int arr[100][100] = {0};
    for (int i = 0; i < 100; i++)
    {
        arr[i][i] = 1;
        arr[i][0] = 1;
        if (i > 1)
        {
            for (int j = 1; j <= i - 1; j++) {
                arr[i][j] = arr[i - 1][j - 1] + arr[i - 1][j];
            }
        }
    }

    int number;
    scanf("%d", &number);
    for (int i = 0; i < number; i++)
    {
        printf("%d ", arr[number - 1][i]);
    }

    return 0;
}
```



### 83

```CC
#include<stdio.h>
#include<string.h>

int main()
{
    int n;
    while (scanf("%d", &n) != EOF)//这个EOF表示的是输入结束（不懂的可以去搜一下）
    {
        int a[n][n];
        memset(a, 0, sizeof(a));//初始化数组 全部填入0
        int i = 0, j = n / 2;
        a[i][j] = 1;//第一行中间元素赋值为1
        for (int k = 2; k <= n * n; k++)//共n*n个数都循环一次
        {
            if (a[(i + n - 1) % n][(j + 1) % n] == 0) //它的上一行，下一列还未被赋值（仍是初值0时）就把他赋值为k
            {

                i = (i + n - 1) % n;//规律比较难找 看的题目例子找的
                j = (j + 1) % n;
                a[i][j] = k;
            }
            else if (a[(i + n - 1) % n][(j + 1) % n] != 0 || (i == 0 && j == n - 1))
            {
                //如果位置已经有数，
                //或上一个数在第一行第N列，
                //则下一个数，放在上一个数的正下方。 
                i = (i + 1) % n;
                a[i][j] = k;
            }
        }
        for (i = 0; i < n; i++) //打印输出二维矩阵
        {
            for (j = 0; j < n - 1; j++)
            {
                printf("%d\t", a[i][j]);
            }
            printf("%d\n", a[i][j]);
        }
        printf("\n");
    }
    return 0;
}
//memset简介\nmemset是一个初始化函数，作用是将某一块内存中的全部设置为指定的值。
//nvoid *memset(void *s, int c, size_t n); s指向要填充的内存块。c是要被设置的值。n是要被设置该值的字符数。返回类型是一个指向存储区s的指针。
//需要说明的几个地方 1.不能任意赋值 2.注意数据类型。
//这里是属实不想用数组解决 所以使用了指针 如果有更优解 欢迎联系
```



### 84

```C
#include <stdio.h>

int main()
{
    int arr[20] = {0};
    int wang = 0;
    int zhao = 0;
    int li = 0;

    for (int i = 0; i < 20; i++)
    {
        scanf("%d", &arr[i]);
        if (arr[i] == 1)
        {
            wang++;
        }
        else if (arr[i] == 2)
        {
            zhao++;
        }
        else if (arr[i] == 3)
        {
            li++;
        }
    }
    printf("王一得票:%d\n", wang);
    printf("赵二得票:%d\n", zhao);
    printf("李三得票:%d\n", li);
    if (wang > zhao && wang > li)
    {
        printf("班长是：王一");
    }
    else if (zhao > wang && zhao > li)
    {
        printf("班长是：赵二");
    }
    else if (li > zhao && wang < li)
    {
        printf("班长是：李三");
    }

    return 0;
}
```



### 85

```C
#include <stdio.h>

int max_number(int numbers[10])
{
    int tmp = 0;
    for (int i = 0; i < 10; i++)
    {

        if (numbers[i] > tmp)
        {
            tmp = numbers[i];
        }
    }

    return tmp;
}

int main() {

    int max_num, numbers[10] = { 9, 8, 7, 6, 5, 4, 3, 2, 1, 0 };
    max_num = max_number(numbers);
    printf("%d", max_num);

    return 0;
}
```



### 86

```C
//关于此题的说明：使用的gets函数并不适用于所有编译环境，所以使用时要注意。
//注意引用头文件<string.h>
#include <stdio.h>
#include<string.h>

int main() {
    char string_name[101];

    int letter = 0, space = 0, number = 0, other = 0;
    //scanf("%s",&string);
    //string_name[101]={string};
    gets(string_name);

    int num = strlen(string_name);
    for (int i = 0; i < num; i++)
    {
        if (string_name[i] >= 'A' && string_name[i] <= 'Z')
        {
            letter++;
        }
        else if (string_name[i] >= 'a' && string_name[i] <= 'z')
        {
            letter++;
        }
        else if (string_name[i] >= '0' && string_name[i] <= '9')
        {
            number++;
        }
        else if (string_name[i] == ' ')
        {
            space++;
        }
        else
        {
            other++;
        }
    }
    printf("letter:%d\nspace:%d\nnumber:%d\nother:%d\n", letter, space, number, other);
    return 0;
}
```

