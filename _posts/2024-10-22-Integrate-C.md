## Integrating C code to Python

> When I deployed AI models in both C and Python, there had been some mismatches between the results achieved by different languages. Moreover, replacing Python code with C code can improve the execution time. The reason is that the implementations are different in different languages. Then, I tried to integrate C code to replace the origin implementation in Python for resolving the mismatched. I found that there are two ways for integrating C code into Python. One is by using 'Ctypes', another is by using 'Python.h' and API of numpy. The two ways have their own pros and cons respectively. The note is for summarizing the two different methods.


- [Integrating C code to Python](#integrating-c-code-to-python)
  - [Ctypes](#ctypes)
    - [What is Ctypes ?](#what-is-ctypes-)
    - [Examples using Ctypes](#examples-using-ctypes)
  - [Using Python and Numpy C API](#using-python-and-numpy-c-api)
    - [What are Python and Numpy C API ？](#what-are-python-and-numpy-c-api-)
  - [Examples using Python and Numpy C API](#examples-using-python-and-numpy-c-api)



### Ctypes

#### What is Ctypes ?

When asking Github Copilot, it tells me that **`ctypes` is a foreign function library for Python. It provides C compatible data types and allows calling functions in DLLs or shared libraries. It can be used to wrap these libraries in pure Python**.

In a nutshell, I can load the dynamic libraries by `ctypes` package. While, I still need to declare the matching C input and output datatypes in `ctypes` for loading these dynamic libraries, here is the [link](https://docs.python.org/3/library/ctypes.html#fundamental-data-types) for these datatypes interfaces.

From my view, using `ctypes` is quite straightforward without much extra knowledge. While, it quite cumbersome to declare all input and output datatypes.

#### Examples using Ctypes

Here is an example of a code snippet for a step for singular value decomposition,

```c
// sort_transpose.c
void sort_transpose(float *v, float *w, int n)
{
    float temp = 0.0f;
    // transpose V to Vt
    for (int i = 0; i < n; i++)
    {
        for (int j = i; j < n; j++)
        {
            temp = v[i * n + j];
            v[i * n + j] = v[j *n + i];
            v[j * n + i] = temp;
        }
    }
    // sort w and order the u v with w sequential
    for (int step = 0; step < n - 1; ++step)
    {
        for (int i = 0; i < n - step - 1; ++i)
        {
            if (w[i] < w[i + 1])
            {
                temp = w[i];
                w[i] = w[i + 1];
                w[i + 1] = temp;
                for (int j = 0; j < n; j++)
                {
                    temp = v[i * n + j];
                    v[i * n + j] = v[(i + 1) * n + j];
                    v[(i + 1) * n + j] = temp;
                }
            }
        }
    }
    for (int i = 0; i < n; i++)
    {
        if (w[i] != 0.0f)
        {
            w[i] = 1.0f / w[i];
        }
    }

    return;

}
```

For reusing the the C code in Python, first need to compile it into shared library

```bash
# linux
gcc -shared -fPIC -o sort_transpose.so sort_transpose.c
# windows
gcc -shared -o sort_transpose.dll sort_transpose.c
# can be optimized with -O3/-O2 option
```

Second, load the shared library and initializing the input and output datatypes

```python
import sys
import ctypes
pf = sys.platform
# void sort_transpose(float *v, float *w, int n)

_sort_transpose = ctypes.CDLL('sort_transpose.{suffix}'.format(folder='c_code_windows' if 'win' in pf else 'c_code_linux', suffix='dll' if 'win' in pf else 'so'))  

_sort_transpose.sort_transpose.argtypes = (ctypes.POINTER(ctypes.c_float), ctypes.POINTER(ctypes.c_float), ctypes.c_int)

def compute_st(v, s, feature_number):
	# declare v array type
	v_array_type = ctypes.c_float * (feature_number * feature_number)   
	v_array = v_array_type(*v)
	# declare s array type
	s_array_type = ctypes.c_float * feature_number   
	s_array = s_array_type(*s)
	_sort_transpose.sort_transpose(v_array, s_array, ctypes.c_int(feature_number))     v_array_list = [v_array[i] for i in range(feature_number * feature_number)]   
	s_array_list = [s_array[i] for i in range(feature_number)]      
	return v_array_list, s_array_list

```

### Using Python and Numpy C API
#### What are Python and Numpy C API ？

**Python.h**

From GitHub copilot, `Python.h` is a header file provided by the Python C API. It contains the declarations of functions, macros, and data structures needed to interact with the Python interpreter from C or C++ code. Including `Python.h` in your C or C++ source files allows you to:
1. **Embed Python**: Embed the Python interpreter in your C/C++ applications.
2. **Extend Python**: Create Python modules and extensions in C/C++.
3. **Access Python Objects**: Manipulate Python objects and call Python functions from C/C++.

**Numpy C API (numpy/ndarrayobject.h)**

From GitHub copilot, **numpy/ndarrayobject.h** includes the header file for NumPy's `ndarray` object. This header file is part of the NumPy C API and provides the necessary declarations and functions to work with NumPy arrays in C or C++ code.

### Examples using Python and Numpy C API

For building sort_transpose.c as an extension module, first, defining the interfaces of Python in a c code.

```c
#include "sort_transpose.h"
  
static PyObject* py_sort_transpose(PyObject* self, PyObject* args) {
    PyObject *v_obj, *w_obj;
    // defining the input data
    int n;
    if (!PyArg_ParseTuple(args, "OOi", &v_obj, &w_obj, &n)) {
        return NULL;
    }
    float *v = (float *) PyArray_DATA(v_obj);
    float *w = (float *) PyArray_DATA(w_obj);
    sort_transpose(v, w, n);
    Py_RETURN_NONE;
}

static PyMethodDef UtilityExtensionMethods[] = {
    {"sort_transpose", py_sort_transpose, METH_VARARGS, "Sort and transpose matrices"},
    {NULL, NULL, 0, NULL}
};

static struct PyModuleDef untilityextensionmodule = {
    PyModuleDef_HEAD_INIT,
    "utility_extension",
    NULL,
    -1,
    UtilityExtensionMethods
};

PyMODINIT_FUNC PyInit_utilityextension(void) {
	// Initialize the Numpy C API
    import_array();
    return PyModule_Create(&untilityextensionmodule);
}
```

Second, use the setuptool to build the python extension module,

```python
from setuptools import setup, Extension
import numpy
import os
import shutil

# python setup.py build_ext --inplace

module = Extension('utilityextension',
                   sources=['sort_transpose.c'],
                   depends=['sort_transpose.h'],
                   extra_compile_args=['-O3', '-Wall'],
                   language='c',
                   include_dirs=[numpy.get_include()])
                   
setup(name='utilityextension',

      version='1.0',

      description='Utility extension module',

      ext_modules=[module])
```

Third, I can use the code as a python module,

```Python
import utilityextension
```

