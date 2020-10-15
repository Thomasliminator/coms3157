# 11. Function Pointers

## The `const` keyword

This is a useful function to set a variable to something constant. Makes it a read only variable.

You can say things like:
`const int BUF_SIZE = 1024;` which is a good alternative to #define.

`const int *p = &x;`
- a `const` pointer only reads to what it is pointing to but cannot write to it. 
- in the `mystrcpy` we can make `s` a `const char` pointer because we are only reading it. We will not be changing it.
- this is how `strcpy` is implemented in C. Run the command `man strcpy` to see the documentation. 

`int *const p = &x;`
`const int *const p = &x`

You can cast away "const-ness". Let's say that `t` is a `const char` pointer, then you can do the following:
`char *t2 = (char *)t;` which will make it not `const` anymore.

## Function Pointers

`qsort()` is a sorting function (quicksort) that is provided by the C library. 
You must pass in the following parameters:
1. pointer to the first element (base)
2. how many members/elements (nmemb)
3. size of the individual element (size)
4. pointer to a function (*compar)

Reference the file `lectnote07.c` in `tmp` directory to see code.

How is the `qsort()` going to compare the elements to sort? It has no way to compare two elements.
The strategy that the function takes is that it asks the caller of the function to provide another function that can compare the elements for the sort.

## Complex Declaration

Reference the file `lectnote07.c` to see the code.

`int (*f1)(const void *v1, const void *v2);` is the declaration of a variable (pointer to a function)
- you dereference the pointer `f1` and then call it using the parenthesis after. After you call it you get an integer.
- `char *a[3]` a is an array of 3 pointers to char.
- you can call the function either by dereferencing it or not (the compiler will allow you to not include the *)


`int *f2 (const void *v1, const void *v2);` is a function prototype

`int (*f3[5])(const void *v1, const void *v2);` 
- `f3` is an array of five pointers that calls a function that returns integer. The only difference between f1 and f3 is an array of 5 `f1`s. 


*updated L10/8*
