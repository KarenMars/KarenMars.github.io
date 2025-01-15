## Memory Leakage in Using Python C-API

### Background
In [previous post, Integrating C code to Python](https://karenmars.github.io/), the pitfall of memory leakage can occur in the integration of C code to Python. This is due to the different memory management of PyObject compared to the common variables in C. When I was doing a project for C code integration, I spent a lot of time in debugging these issues related to memory leakage. The notes below gives some explanation and possible solutions for avoiding this pitfall.

### Memory Management in C

Typically，C uses `free()` to deallocate variables allocated by `malloc`.

```C
char *p = (char *)malloc(sizeof(char)*42);
free(p);
```

Further references: [Memory management in C](https://www.cnblogs.com/yif1991/p/5049638.html)

### Memory Management in Python
It is well known that Python uses the garbage collection for memory management. In general, Python uses **reference counting** on objects. When the number of reference count of an objects is zero, the object is cleared. When we operate these PyObjects in CPython, we need to take care of the memory management of these PyObjects by ourselves. From my view, we need to do things be ourselves when we already enter the level of interpreter.

### Memory Management in Python C-API
Memory management of PyObjects is similar to C. `Py_DECREF` is used to free the memory occupied by a PyObject. For each PyObject, it has a reference count variable. `Py_DECREF` can decreases the reference count by 1. When the reference count of a PyObject is 0, the memory of the PyObject will be released. 

```C
#include "Python.h"
void print_hello_world(void) {
    PyObject *pObj = NULL;

    pObj = PyBytes_FromString("Hello world\n");   /* Object creation, ref count = 1. */
    PyObject_Print(pObj, stdout, 0);
    Py_DECREF(pObj);                              /* ref count = 0 so object deallocated. */
    /* Accidentally use pObj... */
    PyObject_Print(pObj, stdout, 0);
}
```

While difference Python C-APIs have different affects for the reference count. These differences make it hard for when to use the `Py_DECREF` (Using `Py_DECREF` for a non-existing PyObject also leads to crashes). In general, there are three different affects, which are 

    1. New references occur when a PyObject is constructed, for example when creating a new list.
    2. Stolen references occur when composing a PyObject, for example appending a value to a list. “Setters” in other words.
    3. Borrowed references occur when inspecting a PyObject, for example accessing a member of a list. “Getters” in other words. Borrowed does not mean that you have to return it, it just means you that don’t own it. If shared references or pointer aliases mean more to you than borrowed references that is fine because that is exactly what they are. 
   

### Tricks for avoiding Python C-API memory leaks

1. Using numpy C-API instead of Python C-API (numpy C-API makes it easier for memory management).







#### References
- [Python Extension Patterns ( Very detailed materials for writing Python C extensions )](https://pythonextensionpatterns.readthedocs.io/)
- [Paul Ross - Here be Dragons - Writing Safe C Extensions - PyCon 2016 ( A concise tutorials of memory management of PyObjects)](https://www.youtube.com/watch?v=Yq__HtUIH5Y&t=641s)







