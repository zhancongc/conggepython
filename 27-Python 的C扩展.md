# Python 的C扩展

## 前言

大部分的Python扩展都是用C语言写的，也很容易移植到C++中。所有能够整合导入到python脚本的代码，都可以成为扩展。扩展可以用纯Python编写，也可以用编译型的语言来扩展。

就算相同架构的两台电脑之间，也不要相互共享二进制文件，最好在各自的电脑上编译python的扩展。

## 需要python扩展的理由：

- 添加功能，提供python中没有的功能，比如创建新的数据类型，将python嵌入到已经存在的应用程序中。
- 优化性能瓶颈，解释型语言比编译型语言满，想要提高性能。全部改写成编译型语言不划算。最好的办法是先做性能测试，找出性能瓶颈，再用扩展实现。
- 保持专有代码的私密，脚本语言的共同缺陷是，执行的是源代码，没有保密性。把部分代码功能用编译型语言重写，就可以保证私密性。使用pyc也是一种方法。

## 创建python扩展的步骤

### 创建应用程序代码

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define BUFSIZE 10

int fac(int n) {
    if (n < 2)
        return 1;
    return n * fac(n - 1);
}

char *reverse(char *s) {
    register char t;
    char *p = s;
    char *q = (s + (strlen(s) - 1));
    while (p < q) {
        t = *p;
        *p++ = *q;
        *q-- = t;
    }
    return s;
}

int main() {
    char s[BUFSIZE];
    printf("4! == %d\n", fac(4));
    printf("8! == %d\n", fac(8));
    printf("12! == %d\n", fac(12));
    strcpy(s, "abcdef");
    printf("reversing 'abcdef', we get '%s'\n", reverse(s));
    strcpy(s, "madam");
    printf("reversing 'madam', we get '%s'\n", reverse(s));
    return 0;
}
```

使用gcc编译，并执行

### 利用样板来包装代码

整个扩展的实现都是围绕“包装”这个概念来进行的。你的设计尽量让你的语言与python无缝贴合。接口的代码又被称为“样板”代码。

1. 包含python的头文件

2. 为每个模块的每个函数添加一个形如PyObject* Module_func()的包装函数

3. 为每个模块增加一个型如PyMethodDef ModuleMethods[]的数组

4. 增加模块初始化函数void initMethod()

最终的代码如下：

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "Python.h"

#define BUFSIZE 10

int fac(int n) {
    if (n < 2)
        return 1;
    return n * fac(n - 1);
}

char *reverse(char *s) {
    register char t;
    char *p = s;
    char *q = (s + (strlen(s) - 1));
    while (p < q) {
        t = *p;
       *p++ = *q;
       *q-- = t;
    }
    return s;
}

static PyObject *
Extest_fac(PyObject *self, PyObject *args) {
    int res;
    int num;
    PyObject* retval;

    res = PyArg_ParseTuple(args, "i", &num);
    if (!res) {
        return NULL;
    }
    res = fac(num);
    retval = (PyObject *)Py_BuildValue("i", res);
    return retval;
}

static PyObject *
Extest_reverse(PyObject *self, PyObject *args) {
    char *orignal;
    if (!(PyArg_ParseTuple(args, "s", &orignal))) {
        return NULL;
    }
    return (PyObject *)Py_BuildValue("s", reverse(orignal));
}

static PyObject *
Extest_doppel(PyObject *self, PyObject *args) {
    char *orignal;
    char *resv;
    PyObject *retval;
    if (!(PyArg_ParseTuple(args, "s", &orignal))) {
        return NULL;
    }
    retval = (PyObject *)Py_BuildValue("ss", orignal, resv=reverse(strdup(orignal)));
    free(resv);
    return retval;
}

static PyMethodDef 
ExtestMethods[] = {
    {"fac", Extest_fac, METH_VARARGS},
    {"doppel", Extest_doppel, METH_VARARGS},
    {"reverse", Extest_reverse, METH_VARARGS},
    {NULL, NULL},
};

void initExtest() {
    Py_InitModule("Extest", ExtestMethods);
}

int main() {
    char s[BUFSIZE];
    printf("4! == %d\n", fac(4));
    printf("8! == %d\n", fac(8));
    printf("12! == %d\n", fac(12));
    strcpy(s, "abcdef");
    printf("reversing 'abcdef', we get '%s'\n", reverse(s));
    strcpy(s, "madam");
    printf("reversing 'madam', we get '%s'\n", reverse(s));
    test();
    return 0;
}
```

### 编译、测试





> 转载自 Geeking [python扩展实现方法--python与c混和编程](https://www.cnblogs.com/btchenguang/archive/2012/09/04/2670849.html)




