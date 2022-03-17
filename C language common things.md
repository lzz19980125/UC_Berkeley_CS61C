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
*p++;     ==>    p = p+1;
```

```
(*p)++;   ==>   *p = *p+1;
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
```

指向结构体的指针使用 arrow operator （->） 提取结构字段。

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

