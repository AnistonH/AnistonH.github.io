# 三元组

## 第一种：动态

```c++
/*
	Name:        Triplet.cpp
	Author:      侯霖钰
	data:        24/03/10 15:23
	Description: 三元组的一系列操作
*/

#define _CRT_SECURE_NO_WARNINGS 1

#include <iostream> 
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define ERROR 0
#define OK 1

using namespace std;

typedef int ElemType;

typedef struct {
	ElemType* elem;
}Triplet;


// 函数声明
void Menu();

void destroyTriplet(Triplet& trip);
void displayTriplet(Triplet& trip);
void initTriplet(Triplet& trip, ElemType aa, ElemType bb, ElemType cc);

int getElem(Triplet& trip, int index, ElemType* getValue);
void putElem(Triplet& trip, int index, ElemType putValue);

bool isSameValue(Triplet& trip);
bool isAscending(Triplet& trip);
bool isDscending(Triplet& trip);

ElemType getMaxElem(Triplet& trip);
ElemType getMinElem(Triplet& trip);

void addTriplet(Triplet trip1, Triplet trip2, Triplet& result);
void subTriplet(Triplet trip1, Triplet trip2, Triplet& result);
void multipTriplet(Triplet trip1, Triplet trip2, Triplet& result);
void divTriplet(Triplet trip1, Triplet trip2, Triplet& result);

Triplet Work(Triplet& trip);
void multip(Triplet& trip);



int main() {

	Triplet trip;

	printf("请先初始化三元组，请输入三个值，用空格分隔：\n");
	ElemType a, b, c;
	// scanf("%d %d %d", &a, &b, &c);

	cin >> a;
	cin >> b;
	cin >> c;

	initTriplet(trip, a, b, c);

	Menu();

	int function;

	scanf("%d", &function);


	while (function != 0) {
		switch (function) {
		case 1: {
			destroyTriplet(trip);
			printf("销毁成功！\n");
			break;
		}

		case 2: {
			displayTriplet(trip);

			printf("\n--------------------------------\n");
			printf("请输入你的下一步操作：");
			// Menu();
			scanf("%d", &function);
			break;
		}

		case 3: {
			int index;
			ElemType getValue;
			printf("请输入索引值：");
			scanf("%d", &index);

			getElem(trip, index, &getValue);
			//printf("三元组中第%d个元素为%d\n", index, getValue);
			cout << "三元组中第" << index << "个元素为 " << getValue << endl;

			printf("\n--------------------------------\n");
			printf("请输入你的下一步操作：");
			// Menu();
			scanf("%d", &function);
			break;
		}

		case 4: {
			int index;
			ElemType putValue;

			printf("请输入要修改的元素的索引和值：");
			// scanf("%d %d", &index, &putValue);
			cin >> index;
			cin >> putValue;

			putElem(trip, index, putValue);

			printf("修改成功！");
			displayTriplet(trip);

			printf("\n--------------------------------\n");
			printf("请输入你的下一步操作：");
			// Menu();
			scanf("%d", &function);
			break;
		}

		case 5: {
			if (isSameValue(trip)) {
				printf("该三元组的元素是 相同 的。\n");
			}
			else {
				if (isAscending(trip)) {
					printf("该三元组的元素 是 升序的。\n");
				}
				else {
					printf("该三元组的元素 不是 升序的。\n");
				}
			}

			printf("\n--------------------------------\n");
			printf("请输入你的下一步操作：");
			// Menu();
			scanf("%d", &function);
			break;
		}

		case 6: {
			if (isSameValue(trip)) {
				printf("该三元组的元素是 相同 的。\n");
			}
			else {
				if (isDscending(trip)) {
					printf("该三元组的元素 是 降序的。\n");
				}
				else {
					printf("该三元组的元素 不是 降序的。\n");
				}
			}

			printf("\n--------------------------------\n");
			printf("请输入你的下一步操作：");
			// Menu();
			scanf("%d", &function);
			break;
		}

		case 7: {
			ElemType Max = getMaxElem(trip);
			printf("三元组中最大的元素为：%d\n", Max);

			printf("\n请输入你的操作：");
			// Menu();
			scanf("%d", &function);
			break;
		}

		case 8: {
			ElemType Min = getMinElem(trip);
			printf("三元组中最小的元素为：%d\n", Min);

			printf("\n--------------------------------\n");
			printf("请输入你的下一步操作：");
			// Menu();
			scanf("%d", &function);
			break;
		}

		case 9: {
			Triplet result = Work(trip);
			displayTriplet(result);

			printf("\n--------------------------------\n");
			printf("请输入你的下一步操作：");
			// Menu();
			scanf("%d", &function);
			break;
		}

		case 10: {
			multip(trip);
			displayTriplet(trip);

			printf("\n--------------------------------\n");
			printf("请输入你的下一步操作：");
			// Menu();
			scanf("%d", &function);
			break;
		}

		default: {
			printf("输入的值非法，请重新输入：\n");

			// Menu();
			scanf("%d", &function);
			break;
		}

		}
	}

	return 0;
}


void Menu() {
	printf("**************************************************************\n");
	printf("******************** 请选择你的操作 **************************\n");
	printf("**                                                          **\n");
	printf("**       1: DestroyTriplet  销毁三元组                      **\n");
	printf("**       2: displayTriplet  打印三元组                      **\n");
	printf("**       3: getElem         查看三元组中的第index个元素     **\n");
	printf("**       4: putElem         修改三元组的第index个元素       **\n");
	printf("**       5: isAscending     三个元素是否升序                **\n");
	printf("**       6: isDscending     三个元素是否降序                **\n");
	printf("**       7: getMaxElem      返回三个元素中的最大值          **\n");
	printf("**       8: getMinElem      返回三个元素中的最小值          **\n");
	printf("**       9: work            两个三元组的对应分量加减乘除    **\n");
	printf("**       10:multip          三元组的各分量同乘一个比例因子  **\n");
	printf("**       0: 退出                                            **\n");
	printf("**                                                          **\n");
	printf("**************************************************************\n");
}


void displayTriplet(Triplet& trip) {
	cout << "Triplet (elem[0]: " << trip.elem[0] << " elem[1]: " << trip.elem[1] << " elem[2]: " << trip.elem[2] << endl;
}


void initTriplet(Triplet& trip, ElemType aa, ElemType bb, ElemType cc) {
	trip.elem = (ElemType*)malloc(3 * sizeof(ElemType));

	if (trip.elem == NULL) {
		printf("内存分配失败\n");
		exit(1);
	}

	trip.elem[0] = aa;
	trip.elem[1] = bb;
	trip.elem[2] = cc;

}


int getElem(Triplet& trip, int index, ElemType* getValue) {
	if (index < 1 || index > 3) {
		printf("索引值非法，请重新输入：\n");
		scanf("%d", &index);

		while (index < 1 || index > 3) {
			printf("索引值非法，请重新输入：\n");
			scanf("%d", &index);
		}
	}

	switch (index) {
	case 1:
		*getValue = trip.elem[0];
		break;
	case 2:
		*getValue = trip.elem[1];
		break;
	case 3:
		*getValue = trip.elem[2];
		break;
	default:
		printf("无效的索引\n");
		exit(1);

		return 0;
	}
}


void putElem(Triplet& trip, int index, ElemType putValue) {
	if (index < 1 || index > 3) {
		printf("索引值非法，请重新输入：\n");
		scanf("%d", &index);

		while (index < 1 || index > 3) {
			printf("索引值非法，请重新输入：\n");
			scanf("%d", &index);
		}
	}

	switch (index) {
	case 1:
		(trip.elem[0]) = putValue;
		break;
	case 2:
		trip.elem[1] = putValue;
		break;
	case 3:
		trip.elem[2] = putValue;
		break;
	default:
		printf("无效的索引\n");
		exit(1);
	}
}


bool isSameValue(Triplet& trip) {
	if (trip.elem[0] == trip.elem[1] && trip.elem[1] == trip.elem[2]) {
		return true;
	}
	else {
		return false;
	}
}


bool isAscending(Triplet& trip) {
	return(trip.elem[0] <= trip.elem[1] && trip.elem[1] <= trip.elem[2]);
}


bool isDscending(Triplet& trip) {
	return(trip.elem[0] >= trip.elem[1] && trip.elem[1] >= trip.elem[2]);
}


ElemType getMaxElem(Triplet& trip) {
	ElemType temp;
	temp = trip.elem[0] > trip.elem[1] ? trip.elem[0] : trip.elem[1];
	temp = temp > trip.elem[2] ? temp : trip.elem[2];
	return temp;
}


ElemType getMinElem(Triplet& trip) {
	ElemType temp;
	temp = trip.elem[0] < trip.elem[1] ? trip.elem[0] : trip.elem[1];
	temp = temp < trip.elem[2] ? temp : trip.elem[2];
	return temp;
}


void destroyTriplet(Triplet& trip) {
	free(trip.elem);
}


void addTriplet(Triplet trip1, Triplet trip2, Triplet& result) {
	result.elem[0] = trip1.elem[0] + trip2.elem[0];
	result.elem[1] = trip1.elem[1] + trip2.elem[1];
	result.elem[2] = trip1.elem[2] + trip2.elem[2];
}


void subTriplet(Triplet trip1, Triplet trip2, Triplet& result) {
	result.elem[0] = trip1.elem[0] - trip2.elem[0];
	result.elem[1] = trip1.elem[1] - trip2.elem[1];
	result.elem[2] = trip1.elem[2] - trip2.elem[2];
}


void multipTriplet(Triplet trip1, Triplet trip2, Triplet& result) {
	result.elem[0] = trip1.elem[0] * trip2.elem[0];
	result.elem[1] = trip1.elem[1] * trip2.elem[1];
	result.elem[2] = trip1.elem[2] * trip2.elem[2];
}


void divTriplet(Triplet trip1, Triplet trip2, Triplet& result) {
	result.elem[0] = trip1.elem[0] / trip2.elem[0];
	result.elem[1] = trip1.elem[1] / trip2.elem[1];
	result.elem[2] = trip1.elem[2] / trip2.elem[2];
}


Triplet Work(Triplet& trip) {
	ElemType aa = NULL, bb = NULL, cc = NULL;
	Triplet result;

	initTriplet(result, aa, bb, cc);
	Triplet tripTemp;

	printf("--------------------\n");
	printf("请先初始化第二个三元组。\n");
	printf("请输入三个值，用空格分隔。\n");
	// scanf("%d %d %d", &aa, &bb, &cc);

	cin >> aa;
	cin >> bb;
	cin >> cc;

	initTriplet(tripTemp, aa, bb, cc);
	printf("\n\n第二个三元组如下：\n");
	displayTriplet(tripTemp);


	char operate = NULL;
	printf("\n\n请输入要执行的操作(+、-、*、/):\n");
	scanf(" %c", &operate);

	switch (operate) {
	case '+':
		addTriplet(trip, tripTemp, result);
		break;
	case '-':
		subTriplet(trip, tripTemp, result);
		break;
	case '*':
		multipTriplet(trip, tripTemp, result);
		break;
	case '/':
		if (tripTemp.elem[0] == 0 || tripTemp.elem[1] == 0 || tripTemp.elem[2] == 0) {
			printf("第二个三元组中有非法值‘0’，无法进行计算！\n");
		}
		else {
			divTriplet(trip, tripTemp, result);
			break;
		}
	default:
		printf("操作非法！\n");
		break;
	}

	return result;

}


void multip(Triplet& trip) {
	ElemType a;
	printf("请输入比例因子:");
	// scanf("%d", &elem);
	cin >> a;

	trip.elem[0] *= a;
	trip.elem[1] *= a;
	trip.elem[2] *= a;
}

```



## 第二种：伪动态（不完善）

```c++
#define _CRT_SECURE_NO_WARNINGS 1

#include <stdio.h>
#include <stdlib.h>

#define ERROR 0
#define OK 1
typedef int ElemType;


typedef struct {
	ElemType first;
	ElemType second;
	ElemType third;
}Triplet;


void displayTriplet(Triplet* trip) {
	printf("Triplet(first:%d, second:%d, third:%d)\n", trip->first, trip->second, trip->third);
}


Triplet* createTriplet(ElemType first, ElemType second, ElemType third) {
	Triplet* trip = (Triplet*)malloc(sizeof(Triplet));

	if (trip == NULL) {
		printf("内存分配失败\n");
		exit(1);
	}

	trip->first = first;
	trip->second = second;
	trip->third = third;

	return trip;
}


int getElem(Triplet* trip, int index, ElemType* getValue) {
	if (index < 1 || index > 3) {
		printf("索引值非法，请重新输入：\n");
		scanf("%d", &index);

		while (index < 1 || index > 3) {
			printf("索引值非法，请重新输入：\n");
			scanf("%d", &index);
		}
	}

	switch (index) {
	case 1:
		*getValue = trip->first;
		break;
	case 2:
		*getValue = trip->second;
		break;
	case 3:
		*getValue = trip->third;
		break;
	default:
		printf("无效的索引\n");
		exit(1);

		return 0;
	}
}


void putElem(Triplet* trip, int index, ElemType * putValue) {
	if (index < 1 || index > 3) {
		printf("索引值非法，请重新输入：\n");
		scanf("%d", &index);

		while (index < 1 || index > 3) {
			printf("索引值非法，请重新输入：\n");
			scanf("%d", &index);
		}
	}

	switch (index) {
	case 1:
		trip->first = *putValue;
		break;
	case 2:
		trip->second = *putValue;
		break;
	case 3:
		trip->third = *putValue;
		break;
	default:
		printf("无效的索引\n");
		exit(1);
	}
}


bool isSameValue(Triplet* trip) {
	if (trip->first == trip->second && trip->second == trip->third) {
		return 1;
	}
	else {
		return 0;
	}
}


bool isAscending(Triplet* trip) {
	return(trip->first <= trip->second && trip->second <= trip->third);
}


bool isDscending(Triplet* trip) {
	return(trip->first >= trip->second && trip->second >= trip->third);

}


ElemType getMaxElem(Triplet* trip) {
	ElemType temp;
	temp = trip->first > trip->second ? trip->first : trip->second;
	temp = temp > trip->third ? temp : trip->third;
	return temp;
}


ElemType getMinElem(Triplet* trip) {
	ElemType temp;
	temp = trip->first < trip->second ? trip->first : trip->second;
	temp = temp < trip->third ? temp : trip->third;
	return temp;
}


void destroyTriplet(Triplet* trip) {
	free(trip);
}


void addTriplet(Triplet* trip1, Triplet* trip2, Triplet* result) {
	result->first = trip1->first + trip2->first;
	result->second = trip1->second + trip2->second;
	result->third = trip1->third + trip2->third;
}

void subTriplet(Triplet* trip1, Triplet* trip2, Triplet* result) {
	result->first = trip1->first - trip2->first;
	result->second = trip1->second - trip2->second;
	result->third = trip1->third - trip2->third;
}

void multipTriplet(Triplet* trip1, Triplet* trip2, Triplet* result) {
	result->first = trip1->first * trip2->first;
	result->second = trip1->second * trip2->second;
	result->third = trip1->third * trip2->third;
}

void divTriplet(Triplet* trip1, Triplet* trip2, Triplet* result) {
	result->first = trip1->first / trip2->first;
	result->second = trip1->second / trip2->second;
	result->third = trip1->third / trip2->third;
}

Triplet* Work(Triplet* trip) {
	ElemType aa = NULL, bb = NULL, cc = NULL;
	Triplet* result = createTriplet(NULL, NULL, NULL);
	Triplet* tripTemp;

	printf("--------------------\n");
	printf("请先初始化第二个三元组。\n");
	printf("请输入三个值，用空格分隔。\n");
	scanf("%d %d %d", &aa, &bb, &cc);



	tripTemp = createTriplet(aa, bb, cc);
	printf("\n\n第二个三元组如下：\n");
	displayTriplet(tripTemp);


	char operate = NULL;
	printf("\n\n请输入要执行的操作(+、-、*、/):\n");
	scanf(" %c", &operate);

	switch (operate) {
	case '+':
		addTriplet(trip, tripTemp, result);
		break;
	case '-':
		subTriplet(trip, tripTemp, result);
		break;
	case '*':
		multipTriplet(trip, tripTemp, result);
		break;
	case '/':
		if (tripTemp->first == 0 || tripTemp->second == 0 || tripTemp->third == 0) {
			printf("第二个三元组中有非法值‘0’，无法进行计算！\n");
		}
		else {
			divTriplet(trip, tripTemp, result);
			break;
		}
	default:
		printf("操作非法！\n");
		break;
	}

	return result;
}


void multip(Triplet* trip) {
	ElemType elem;
	printf("请输入比例因子:");
	scanf("%d", &elem);

	trip->first *= elem;
	trip->second *= elem;
	trip->third *= elem;
}


void Menu(){
	printf("------------------------------\n");
	printf("<-- 请选择你的操作：-->\n");
	printf("1: DestroyTriplet  销毁三元组\n");
	printf("2: displayTriplet  打印三元组\n");
	printf("3: getElem         查看三元组中的第index个元素\n");
	printf("4: putElem         修改三元组的第index个元素\n");
	printf("5: isAscending     三个元素是否升序\n");
	printf("6: isDscending     三个元素是否降序\n");
	printf("7: getMaxElem      返回三个元素中的最大值\n");
	printf("8: getMinElem      返回三个元素中的最小值\n");
	printf("9: work            两个三元组的对应分量加减乘除\n");
	printf("10:multip          三元组的各分量同乘一个比例因子\n");
	printf("0:退出              \n");
	printf("------------------------------\n");
}


int main() {

	printf("请先初始化三元组，请输入三个值，用空格分隔：\n");
	ElemType a, b, c;
	scanf("%d %d %d", &a, &b, &c);
	Triplet* trip = createTriplet(a, b, c);

	Menu();

	int function;

	scanf("%d", &function);


	while (function != 0) {
		switch (function) {
		case 1: {
			destroyTriplet(trip);
			printf("销毁成功！\n");
			break;
		}

		case 2: {
			displayTriplet(trip);

			printf("\n请输入你的操作：");
			Menu();
			scanf("%d", &function);
			break;
		}

		case 3: {
			int index;
			ElemType getValue;
			printf("请输入索引值：");
			scanf("%d", &index);

			getElem(trip, index, &getValue);
			printf("三元组中第%d个元素为%d\n", index, getValue);

			printf("\n请输入你的操作：");
			Menu();
			scanf("%d", &function);
			break;
		}

		case 4: {
			int index;
			ElemType putValue;

			printf("请输入要修改的元素的索引和值：");
			scanf("%d %d", &index, &putValue);
			putElem(trip, index, &putValue);

			printf("修改成功！");
			displayTriplet(trip);

			printf("\n请输入你的操作：");
			Menu();
			scanf("%d", &function);
			break;
		}

		case 5: {
			if (isSameValue(trip)) {
				printf("该三元组的元素是 相同 的。\n");
			}
			else {
				if (isAscending(trip)) {
					printf("该三元组的元素 是 升序的。\n");
				}
				else {
					printf("该三元组的元素 不是 升序的。\n");
				}
			}

			printf("\n请输入你的操作：");
			Menu();
			scanf("%d", &function);
			break;
		}

		case 6: {
			if (isSameValue(trip)) {
				printf("该三元组的元素是 相同 的。\n");
			}
			else {
				if (isDscending(trip)) {
					printf("该三元组的元素 是 降序的。\n");
				}
				else {
					printf("该三元组的元素 不是 降序的。\n");
				}
			}

			printf("\n请输入你的操作：");
			Menu();
			scanf("%d", &function);
			break;
		}

		case 7: {
			ElemType Max = getMaxElem(trip);
			printf("三元组中最大的元素为：%d\n", Max);

			printf("\n请输入你的操作：");
			Menu();
			scanf("%d", &function);
			break;
		}

		case 8: {
			ElemType Min = getMinElem(trip);
			printf("三元组中最小的元素为：%d\n", Min);

			printf("\n请输入你的操作：");
			Menu();
			scanf("%d", &function);
			break;
		}

		case 9: {
			Triplet* result = Work(trip);
			displayTriplet(result);

			printf("\n请输入你的操作：");
			Menu();
			scanf("%d", &function);
			break;
		}

		case 10: {
			multip(trip);
			displayTriplet(trip);

			printf("\n请输入你的操作：");
			Menu();
			scanf("%d", &function);
			break;
		}

		default: {
			printf("输入的值非法，请重新输入：\n");

			Menu();
			scanf("%d", &function);
			break;
		}

		}
	}

	return 0;
}
```

