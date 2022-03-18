# Lecture 2（pointers）

C语言需要程序员手动管理内存；

```c
int *p;
```

* 告诉编译器，p是一个指针，保存着某个整数的地址

```c
p = &y;
```

* 告诉编译器，把y的地址赋给p
* “&”是取地址操作符

```c
int A=100;
int *p = &A;
等价于
int *p;
p = &A;
```

```c
z = *p;
```

* 告诉编译器，把p中地址对应的变量的值赋给z
* “*”是“解引用（dereference）”操作符，即获取指针指向的变量的值

```c
void *
```

上式可指向任意类型

### pointers Arithmetic

```c
*p++;     ==>    p = p+1;   ==>  在地址中增加 sizeof(指针p类型)*1
```

```
(*p)++;   ==>   *p = *p+1;
```

**多级指针的命名方式**

```c
int A[] = {50,60,70};
int *q = A;
int **p = &q;            /*注意：二级指针必须接一级指针的地址！ */
/*
	A[0]的地址是：0x7fff8eb8fbec
	**p = *q = A[0] = 50;
	*p = q = 0x7fff8eb8fbec
	p = &q = 0x7fff8eb8fbd8
	&p = 0x7fff8eb8fbe0
*/
```

**通过函数传参改变一级指针的值**

```c
void inc_ptr(int **h) {
	*h = *h + 1; 
}

int A[3] = {50, 60, 70};
int* q = A; inc_ptr(&q);
printf(“*q = %d\n”, *q);
```

### arrays vs pointers

**每一个数组的名字都是对于该数组第0个元素的只读指针**!

**pointers和arrays在某种意义上是等效的！！！**

```c
int main(void){ 
	int A[] = {5,10}; 
	int *p = A;           /*等效于  int *p=&A  */
}
```

### pointer所占空间的大小

pointer取决于你机器的体系结构，如果你的机器是32位的，则sizeof(pointer)=4，如果你的机器是64位的，则sizeof(pointer)=8;

### Struct

结构体的定义：

```c
struct point {     /* type definition */ 
    int x; 
    int y;
};

void PrintPoint(struct point p) {
	printf(“(%d,%d)”, p.x, p.y); 
}

struct point p1 = {0,10}; /* x=0, y=10 */
PrintPoint(p1);
```

利用指针指向结构体

```c
struct point *p; 
/* code to assign to pointer */ 
printf(“x is %d\n”, (*p).x); 
printf(“x is %d\n”, p->x);

typedef struct { 
    int x; 
    int y;
} Point;

Point p1; 
Point p2;
Point *paddr; 
paddr = &p2;

/* dot notation */ 
int h = p1.x; 
p2.y = p1.y;

/* arrow notation */ 
int h = paddr->x; 
int h = (*paddr).x;

/* This works too: copies all of p2 */
p1 = p2; 
p1 = *paddr;
```

指向结构体的指针使用 arrow operator （->） 提取结构字段。

**struct的内存对齐 **

struct的内存对齐需要遵循以下三个原则：

1. 第一个成员在与结构体变量偏移量为0的地址；
2. 其他成员变量要对齐到某个数字（对齐数）的整数倍的地址处；
3. **对齐数=编译器默认的一个对齐数 与 该成员大小的较小值。**
4. **linux 中默认为4；**
5. vs 中的默认值为8；
6. **结构体总大小为最大对齐数的整数倍（每个成员变量除了第一个成员都有一个对齐数）；**

譬如以下例子：

```c
struct foo { 
    int a; 
    char b;
	struct foo *c; 
}
```

则整个struct所占据的内存大小为：int（4）+char（1）+对齐（3）+指针（如果是32位机器，4） = 12*8 byte

### 利用`typedef`简化代码

```c
struct Node { 
	char *value;
	struct Node *next;
};
/* "typedef" means define a new type */ 
typedef struct Node NodeStruct;

/* 或者 */

typedef struct Node { 
    char *value;
	struct Node *next; 
} NodeStruct;
```

**Arguments in main() **

```c
int main(int argc, char *argv[])
```

* argc 表示命令行中包含的字符串参数的个数；
* argv 作为一个指向字符串数组的指针数组；

举例说明：

```c
foo hello 87 "bar baz"
```

```c
argc = 4 /* number arguments */
```

```c
argv[0] = "foo", argv[1] = "hello", argv[2] = "87", argv[3] = "bar baz"
```

