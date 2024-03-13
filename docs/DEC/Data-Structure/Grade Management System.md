# 成绩管理系统

> 编写于：2024-03-12

## 说明

这是一个文件管理系统，由单链表实现，在“排名学生信息”功能中使用的双重指针，在导入导出文件中使用文件指针进行了对文件的操作。

## 功能如下

```C
********************************************
************** 学生成绩管理系统 ************
**                                        **
**             1: 插入学生信息            **
**             2: 删除学生信息            **
**             3: 查询学生信息            **
**             4: 显示学生信息            **
**             5: 排名学生信息            **
**             6: 修改学生信息            **
**             7: 导出信息至文件          **
**             8: 由文件导入信息          **
**             9: 显示小组 LOGO          **
**            10: 退出                    **
**                                        **
********************************************
```



## 代码

```c
#define _CRT_SECURE_NO_WARNINGS 1

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <windows.h>

typedef struct Student {
    char name[50];
    char ID[20] = { 0 };
    float score[4];
    float sum;
    struct Student* next;
} Student;

// 全局变量，指向链表头节点
Student* head = NULL;

int count = 0; // 统计数量

// 插入学生信息
void insertStudent() {
    Student* newStudent = (Student*)malloc(sizeof(Student));
    if (newStudent == NULL) {
        printf("内存分配失败！\a\n");
        exit(1);
    }

    newStudent->sum = 0.0;

    printf("请输入学生姓名: ");
    scanf("%s", newStudent->name);

    printf("请输入学生学号: ");
    scanf("%s", newStudent->ID);

    printf("请依次输入学生四门课程成绩: ");

    for (int i = 0; i < 4; i++) {
        scanf("%f", &(newStudent->score[i]));
        newStudent->sum += newStudent->score[i];
    }
    newStudent->next = NULL;

    // 如果链表为空，将新节点设置为头节点
    if (head == NULL) {
        head = newStudent;
    }
    else {
        // 否则，遍历链表找到最后一个节点，并将新节点插入到最后
        Student* current = head;
        while (current->next != NULL) {
            current = current->next;
        }
        current->next = newStudent;
    }

    printf("学生信息插入成功！\n");
    count++;
}

// 删除学生信息
void deleteStudent() {
    if (head == NULL) {
        printf("链表为空，无法删除学生信息！\a\n");
        return;
    }

    char studentID[20] = { 0 };
    printf("请输入要删除的学生学号: ");
    scanf("%s", studentID);

    Student* current = head;
    Student* prev = NULL;

    // 遍历链表查找要删除的学生节点
    while (current != NULL && strcmp(current->ID, studentID) != 0) {
        prev = current;
        current = current->next;
    }

    // 如果找到了要删除的学生节点
    if (current != NULL) {
        // 如果要删除的学生节点是头节点
        if (prev == NULL) {
            head = current->next;
        }
        else {
            prev->next = current->next;
        }
        free(current);
        free(prev);

        printf("学生信息删除成功！\n");
        count--;
    }
    else {
        printf("未找到学号为%s的学生信息！\a\n", studentID);
    }
}

// 查询学生信息
void queryStudent() {
    if (head == NULL) {
        printf("链表为空，无学生信息！\a\n");
        return;
    }

    char studentID[20] = { 0 };
    printf("请输入要查询的学生学号: ");
    scanf("%s", studentID);

    Student* current = head;

    // 遍历链表查找要查询的学生节点
    while (current != NULL && strcmp(current->ID, studentID) != 0) {
        current = current->next;
    }

    // 如果找到了要查询的学生节点
    if (current != NULL) {
        printf("\n");
        printf("学生姓名: %s\n", current->name);
        printf("学生学号: %s\n", current->ID);
        printf("学生成绩: ");

        for (int i = 0; i < 4; i++) {
            printf("%.2f ", current->score[i]);
        }
        printf("\n");

        printf("学生总成绩: %.2f\n", current->sum);
    }
    else {
        printf("未找到学号为%s的学生信息！\a\n", studentID);
    }
}

// 排名学生信息
void rankStudents() {
    if (head == NULL) {
        printf("链表为空，无学生信息！\a\n");
        return;
    }

    // 创建一个临时数组，用于存储学生信息
    int length = 0;
    Student* current = head;
    while (current != NULL) {
        length++;
        current = current->next;
    }

    Student** students = (Student**)malloc(sizeof(Student*) * length);
    if (students == NULL) {
        printf("内存分配失败，程序意外退出！\a\n");
        exit(1);
    }

    current = head;
    for (int i = 0; i < length; i++) {
        students[i] = current;
        current = current->next;
    }

    // 使用冒泡排序对学生信息按总成绩进行排序
    for (int i = 0; i < length - 1; i++) {
        for (int j = 0; j < length - 1 - i; j++) {
            if (students[j]->sum < students[j + 1]->sum) {
                Student* temp = students[j];
                students[j] = students[j + 1];
                students[j + 1] = temp;
            }
        }
    }


    printf("学生成绩排名如下：\n");
    for (int i = 0; i < length; i++) {
        printf("\n");
        printf("排名 %d:\n", i + 1);
        printf("姓名: %s\n", students[i]->name);
        printf("学号: %s\n", students[i]->ID);
        printf("成绩:\n");
        for (int j = 0; j < 4; j++) {
            printf("    科目 %d:  %.2f\n", j + 1, students[i]->score[j]);
        }
        printf("总成绩: %.2f\n", students[i]->sum);
        printf("--------------------\n");
    }

    free(students);
}

// 修改学生信息
void modifyStudent() {
    if (head == NULL) {
        printf("链表为空，无学生信息！\a\n");
        return;
    }

    char studentID[20] = { 0 };
    printf("请输入要修改的学生学号: ");
    scanf("%s", studentID);

    Student* current = head;

    // 遍历链表查找要修改的学生节点
    while (current != NULL && strcmp(current->ID, studentID) != 0) {
        current = current->next;
    }

    // 如果找到了要修改的学生节点
    if (current != NULL) {
        current->sum = 0.00;
        
        printf("\n******************************\n");
        printf("*** 请选择要修改的学生信息 ***\n");
        printf("**                          **\n");
        printf("**          1: 姓名         **\n");
        printf("**          2: 学号         **\n");
        printf("**          3: 课程         **\n");
        printf("**          0: 退出         **\n");
        printf("**                          **\n");
        printf("******************************\n\n");
        
        int function;
        scanf("%d", &function);
        while (function != 0) {
            switch (function) {
            case 1: {
                printf("请输入学生姓名: ");
                scanf("%s", current->name);
                printf("学生信息修改成功！\n");
                break;
            }
            case 2: {
                printf("请输入学生学号: ");
                scanf("%s", current->ID);
                printf("学生信息修改成功！\n");
                break;
            }
            case 3: {
                printf("请输入学生四门课程成绩: ");
                for (int i = 0; i < 4; i++) {
                    scanf("%f", &(current->score[i]));
                    current->sum += current->score[i];
                }
                printf("学生信息修改成功！\n");
                break;
            }
            default: {
                printf("输入错误，请重新输入：\a");
                scanf("%d", &function);
                break;
            }

            }
            printf("\n请再次选择操作：");
            scanf("%d", &function);
        }

    }
    else {
        printf("未找到学号为%s的学生信息！\a\n", studentID);
    }
}

// 导出学生信息至文件
void exportToFile() {
    if (head == NULL) {
        printf("链表为空，无学生信息！\a\n");
        return;
    }

    char filename[50] = { 0 };
    printf("请输入导出文件的名称: ");
    scanf("%s", filename);

    FILE* file = fopen(filename, "w");
    if (file == NULL) {
        printf("无法打开文件！\a\n");
        return;
    }

    Student* current = head;

    while (current != NULL) {
        fprintf(file, "Name: %s\n", current->name);
        fprintf(file, "ID: %s\n", current->ID);
        fprintf(file, "Score: ");
        for (int i = 0; i < 4; i++) {
            fprintf(file, "%.2f ", current->score[i]);
        }
        fprintf(file, "\n");
        fprintf(file, "Sum: %.2f\n", current->sum);
        fprintf(file, "\n");
        current = current->next;
    }

    fclose(file);
    printf("学生信息已成功导出到文件%s！\n", filename);
}

// 由文件导入学生信息
void readFromFile() {
    char filename[50] = { 0 };
    printf("请输入要导入的文件名: ");
    scanf("%s", filename);

    FILE* file = fopen(filename, "r");
    if (file == NULL) {
        printf("无法打开文件！\a\n");
        return;
    }

    Student* current = NULL;
    Student* previous = NULL;

    while (1) {
        Student* newStudent = (Student*)malloc(sizeof(Student));
        if (newStudent == NULL) {
            printf("内存分配失败，程序意外退出！\a\n");
            exit(1);
        }


        if (fscanf(file, "Name: %s\n", newStudent->name) != 1)
            break;

        count++;

        fscanf(file, "ID: %s\n", newStudent->ID);
        fscanf(file, "Score: ");
        for (int i = 0; i < 4; i++) {
            fscanf(file, "%f", &newStudent->score[i]);
        }
        fscanf(file, "\n");
        fscanf(file, "Sum: %f\n", &newStudent->sum);
        fscanf(file, "\n");
        // 注意看，这里的fsanf和导出数据函数中的fprintf并不是完全相同的。


        newStudent->next = NULL;

        if (current == NULL) {
            current = newStudent;
            head = newStudent;
        }
        else {
            previous->next = newStudent;
            current = newStudent;
        }
        previous = newStudent;
    }

    fclose(file);
    printf("学生信息已成功从文件%s导入！\n", filename);
}

// 显示学生信息
void displayStudents() {
    if (head == NULL) {
        printf("链表为空，无学生信息！\a\n");
        return;
    }

    Student* current = head;
    while (current != NULL) {
        printf("学生姓名: %s\n", current->name);
        printf("学生学号: %s\n", current->ID);
        printf("学生成绩: ");
        for (int i = 0; i < 4; i++) {
            printf("%.2f ", current->score[i]);
        }
        printf("\n");
        printf("学生总成绩: %.2f\n", current->sum);
        printf("\n");
        current = current->next;
    }
}

// 释放链表内存
void freeList() {
    Student* current = head;
    while (current != NULL) {
        Student* temp = current;
        current = current->next;
        free(temp);
    }
    head = NULL;
}

void logo() {
    int time = 5;
    while (time) {
        printf("                                                                                                      \n");
        printf("          JJJJJJJJJJJ     OOOOOOOOO     KKKKKKKKK    KKKKKKKEEEEEEEEEEEEEEEEEEEEEERRRRRRRRRRRRRRRRR   \n");
        printf("          J:::::::::J   OO:::::::::OO   K:::::::K    K:::::KE::::::::::::::::::::ER::::::::::::::::R  \n");
        printf("          J:::::::::J OO:::::::::::::OO K:::::::K    K:::::KE::::::::::::::::::::ER::::::RRRRRR:::::R \n");
        printf("          JJ:::::::JJO:::::::OOO:::::::OK:::::::K   K::::::KEE::::::EEEEEEEEE::::ERR:::::R     R:::::R\n");
        printf("            J:::::J  O::::::O   O::::::OKK::::::K  K:::::KKK  E:::::E       EEEEEE  R::::R     R:::::R\n");
        printf("            J:::::J  O:::::O     O:::::O  K:::::K K:::::K     E:::::E               R::::R     R:::::R\n");
        printf("            J:::::J  O:::::O     O:::::O  K::::::K:::::K      E::::::EEEEEEEEEE     R::::RRRRRR:::::R \n");
        printf("            J:::::j  O:::::O     O:::::O  K:::::::::::K       E:::::::::::::::E     R:::::::::::::RR  \n");
        printf("            J:::::J  O:::::O     O:::::O  K:::::::::::K       E:::::::::::::::E     R::::RRRRRR:::::R \n");
        printf("JJJJJJJ     J:::::J  O:::::O     O:::::O  K::::::K:::::K      E::::::EEEEEEEEEE     R::::R     R:::::R\n");
        printf("J:::::J     J:::::J  O:::::O     O:::::O  K:::::K K:::::K     E:::::E               R::::R     R:::::R\n");
        printf("J::::::J   J::::::J  O::::::O   O::::::OKK::::::K  K:::::KKK  E:::::E       EEEEEE  R::::R     R:::::R\n");
        printf("J:::::::JJJ:::::::J  O:::::::OOO:::::::OK:::::::K   K::::::KEE::::::EEEEEEEE:::::ERR:::::R     R:::::R\n");
        printf(" JJ:::::::::::::JJ    OO:::::::::::::OO K:::::::K    K:::::KE::::::::::::::::::::ER::::::R     R:::::R\n");
        printf("   JJ:::::::::JJ        OO:::::::::OO   K:::::::K    K:::::KE::::::::::::::::::::ER::::::R     R:::::R\n");
        printf("     JJJJJJJJJ            OOOOOOOOO     KKKKKKKKK    KKKKKKKEEEEEEEEEEEEEEEEEEEEEERRRRRRRR     RRRRRRR\n");
        printf("                                                                                                      \n");
        printf("                                                                                                      \n");

        //printf("倒计时 %d 秒！", time--);
        printf("\033[1;36;41m倒计时 %d 秒！\033[m\n", time--);
        Sleep(1000);
        system("cls");
    }
}


int main() {

    logo();

    while (1) {

        printf("\n********************************************\n");
        printf("************** 学生成绩管理系统 ************\n");
        printf("**                                        **\n");
        printf("**          \033[1;36;41m当前系统中有 %d 份数据！\033[m       **\n", count);
        printf("**                                        **\n");
        printf("**             1: 插入学生信息            **\n");
        printf("**             2: 删除学生信息            **\n");
        printf("**             3: 查询学生信息            **\n");
        printf("**             4: 显示学生信息            **\n");
        printf("**             5: 排名学生信息            **\n");
        printf("**             6: 修改学生信息            **\n");
        printf("**             7: 导出信息至文件          **\n");
        printf("**             8: 由文件导入信息          **\n");
        printf("**             9: 显示小组 LOGO           **\n");
        printf("**            10: 退出                    **\n");
        printf("**                                        **\n");
        printf("********************************************\n\n");

        int choice;
        printf("请选择操作：");
        scanf("%d", &choice);

        switch (choice) {
        case 1:
            insertStudent();
            break;
        case 2:
            deleteStudent();
            break;
        case 3:
            queryStudent();
            break;
        case 4:
            displayStudents();
            break;
        case 5:
            rankStudents();
            break;
        case 6:
            modifyStudent();
            break;
        case 7:
            exportToFile();
            break;
        case 8:
            readFromFile();
            break;
        case 9:
            system("cls");
            logo();
            break;
        case 10:
            freeList();  // 释放链表内存
            exit(0);

        default:
            printf("无效的选择，请重新输入：\a");
            scanf("%d", &choice);
            break;
        }

    }

    return 0;
}
```

