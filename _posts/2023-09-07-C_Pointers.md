# Understand Pointers of C

- [Understand Pointers of C](#understand-pointers-of-c)
  - [References](#references)
  - [About memory fault](#about-memory-fault)


## References
- https://www.youtube.com/watch?v=zuegQmMdy8M


## About memory fault
- In the book of C pointers, the below code presents a type of error in C,
  
  
  ```c
  // an example of memory fault
  #include <stdio.h>
  int main(void){
    int* a; // a pointer without initialization
    *a = 12;
    printf("The address of a is %p\n", a);
    printf("The value of a is %d\n", *a);
    return 0;
    }
  ```


  the pointer `a` wasn't initialized in this code, and it would result in `segmentation violation` or `memory fault`. However, when I wrote and compiled this code on my MAC, I didn't have the error. And ChatGPT gave the following explanation:

  The reason why this code does not result in a memory fault when compiled on macOS is because C/C++ compilers do not always immediately trigger an error in such situations. However, this code represents undefined behavior, which means it may produce different outcomes, including crashes or incorrect behavior, on different compilers and platforms.

  In this code, you declare a pointer named `a` without allocating memory for it. Then, you attempt to assign a value to the memory location pointed to by `a` with `*a = 12;`. Since `a` is uninitialized, it effectively points to an unknown memory location, leading to undefined behavior.

  In some cases, the compiler may set `a` to a specific default value without crashing. However, this is not a reliable behavior and depends on the compiler and compiler flags.

  To avoid such errors, you should allocate memory for a pointer before using it or point it to previously allocated memory. For example:

  ```c
  int* a = malloc(sizeof(int)); // Allocate memory
  if (a != NULL) {
     *a = 12;
     printf("The address of a is %p\n", (void*)a); // Print the pointer's address using %p
     printf("The value of a is %d\n", *a);
     free(a); // Free the memory
     } else {
    // Handle memory allocation failure
  }
  ```

  This code safely allocates memory, stores the value 12 in the allocated memory, and frees the memory after use.

  **In a nutshell, it depends on the compiler and the platform. While, it still recommended to allocate memory for a pointer before using it.**

