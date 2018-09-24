# Linux/UNIX进程

##进程环境

### 进程的终止

1. 从main函数返回

2. 调用exit

3. 调用\_exit或\_Exit

4. 最后一个线程从其启动例程返回

5. 从最后一个线程调用pthread\_exit

异常退出
6. 调用abort
7. 接到一个信号
8. 最后一个线程对取消请求做出响应

#### 退出函数
```C
#include <stdlib.h>
void exit(int status);
void _Exit(int status);
#include <unistd.h>
void _exit(int status);
```
exit函数总是执行一个标准I/O库的清理关闭操作：对所有打开流调用fclose函数。
退出函数都带有一个终止状态的参数，如果(a)调用这些函数不带终止状态，(b)main函数
执行了一个无返回值得return语句，(c)main没有申明返回类型为整型，则该进程的终止
状态是未定义的。

但是，如果main函数的返回类型是整型，且main函数执行到最后一条语句时隐式返回，那么，
该进程的终止状态是0.
#### atexit函数
```C
#include <stdlib.h>
int atexit(void (*func) (void));
```
返回值：成功，0；失败，非0.
一个进程可以等级至多32个函数，这些函数有exit自动调用，调用顺序与登记顺序相反。

### 命令行参数
调用exec的进程可以将命令行的参数传递给该新程序。
ISO C和POSIX都要求argv[argc]是一个空指针。

### 环境表
每个程序都接受一张环境表，该表是一个字符指针数组。全局变量environ包含了该数组指针的地址：
```C
extern char \*\* environ
```
使用getenv和putenv函数可以范围吗特定的环境变量，而查看整个环境，则必须使用environ指针。
### 存储空间分配
1、malloc，分配指定字节数的存储区。此存储区的初始值不确定。
2、calloc，为指定数量指定长度的对象分配存储空间。该空间中的每一位都初始化为0.
3、realloc，增加或者减少以前分片区的长度，新增区域的初始值不确定。分配后内存地址可能改变。
```C
#include <stdlib.h>
void *malloc(size_t size);
void *calloc(size_t nobj, size_t size);
void *realloc(void *ptr, size_t new_size);

void free(void * ptr);
```
返回值：成功，非空指针；失败，NULL.
### 环境变量
### setjmp和longjmp
### getrlimt和serlimit
---
## 进程控制
### fork

