# 9. Pointers

## Pointer Types

```C
foo()
{
    int x = 1, y = 2;
    int *p;
    
    p = &x;
    y = *p; //basically same thing as y = x;
    
    *p = 0; 
    ++*p; 
    (*p)++; //if you omit the parenthesis, this is *(p++)
}
```

`int *` is a type. You can also write it as `int* = p` although the former is convention. This is a variable that contains the memory address to another (int) variable. They are always 8 bytes long.

`p = &x;` will give you the address. 

`*` is an operator that operates on a pointer variable and it goes into what it's pointing to.

```
int i = 3;
double d = 3.14;
int *p = &i;
double *q = &d;

//p = &d cannot be done because they are incompatbile

p = (int*) &d //you can do this to force it, but this is more complicated
```

`void *r` is a generic pointer. Then you can do something like `r = p;` then `q = r`.

### The Null Pointer

`char *q = 0;` is the same as `char *q = NULL;`

Normally, you cannot add values to a pointer variables. i.e. 1000, 23 ...
However, `0` is an exception. It can be assigned to a pointer variable, there is an automatic conversion to a pointer value.

`#define NULL((void*)0)` is how the NULL pointer is defined. This is done (casted) so that expressions such as `NULL + 1` would be illegal.

```
char c = 0;
char *p = &c;
```

Is `p` a null pointer? No! This has an actual memory address.

You can also do something like `if(p) {` which will test if `p` is a null pointer. In the above case, it is true. Every pointer is true except for the NULL pointer. 

`if(*p){` is false because it is "what it is pointing to". 

If you try to access a region that hasn't yet been mapped, segmentation error.
If you try to write to the code region, segmentation error. This is because it is "read only" at runtime. 

## Arrays

```C
int foo()
{
    int a[10] = {100, 101, 102, 103, 104, 105, 106, 107, 108, 109};
    int *p = &a[0]; //address of (a[0])
    printf("%d", sizeof(a));
    
}
```

Same way as declaring a variable. The format is (type) (name)(type). Accessing the array is typical.

`sizeof()` will evaluate to the byte size. `a` is an array of 10 integers. 
Therefore, `sizeof(a)` is 40. `sizeof(a[0])` = 4. `sizeof(p)` = 8. `sizeof(*p)` = 4.

With an array, C will make sure that the elements of the array are put in memory contiguously. 

Consider the expression `p+1`. What does it mean to add 1 to a pointer? It is the address of the next element after the current variable. 
`p+1` would add the value of `sizeof(*p)`. 

`*p` is the same as `a[0]`
`*(p+1)` is the same as `a[1]`
`p+1` is the same as `&a[1]`

Given a pointer, `*` means you are going to what it points to, what it has the address of. If you take the address of that, `&`, you get the pointer of that. Therefore, the `*` and `&` operators cancel out. 

**Theorem No. 1**
In general, `*(p+1) = a[i]`

### Theorem No. 2

```C
for(int i=0; i<sizeof(a)/sizeof(a[0]); i++){ //goes 0-9
    //method 1
    printf("%d", a[i]);
    
    //method 2
    printf("%d", *(p+i));
    
    //method 3
    printf("%d", *p);
    p++;
    
    //method 4
    printf("%d", *p++);
}
```

In method 3, we do `p++` which will move the pointer to the next element. 

`*p++` is a super idiomatic expression in C. The expression is completely equivalent to `*(p++)`. The incrementation is "delayed". An easier way to think of it is method 3, but you have to recognize that it is NOT equal to `(*p)++`.

`a` is an array name, but sometimes (most of the times), it immediately becomes `&a[0]`. 
- for example, you can do something like `a + 1` which becomes `&a[0] + 1` which is `p + 1`. 
- `*a`: dereferencing an array makes no sense but it becomes the expression `*(&a[0])` which is the same as `a[0]` which is `*p`.
- in most cases, the array name is interchangeable with the address of the first element. 

**Theorem No. 2**
`a` --> `&a[0]` "array name is the address of the first element"

### Grand Unified Theory (GUT) of Arrays and Pointers

From Theorem 1 and Theorem 2, we get:

**Theorem No. 3**
`*(a+i)` is the same as `a[i]`.
`*(p+i)` is the same as `p[i]`
**`*(x+y)` <==> `x[y]`**(This is the GUT).

*Dereferencing and the bracket are basically the same thing.*

If you have `*p` is the same as `p[0]`.
If you have `a[0]` it is `*a`. 

Let's say, `0[a]` was written instead of `a[0]`. This will be immediately transformed into `*(0+a)`. This has to be equivalent to `*(a+0)` due to commutative properties. This is therefore the same as `a[0]`. 
This is legal in C!

## Heaps

When you return from a function, local variables and arrays will cease to exist.
How can we declare an array that multiple functions can use? 
- that is what the heap is for.

```C
int *foo()
{
    int *p;
    p = malloc(10 * sizeof(int)); //value is 40
    
    ...
    
    return p;
}

int main()
{
    int *a = foo();
    bar(a); 
    
    ...
    
    free(a);
}
```

`malloc()` is a library function that stands for "memory allocation". It will create some (40) byte region in the heap area and will return a pointer to the first byte of the memory.  
Even though it was allocated inside of `foo()` it will continue to live on as long as we keep the pointer. 

`a` will get the pointer that points to the piece of memory in the heap. Then, you can pass that to another function. 

When we do `free(a)` it will mark the (40) bytes as not allocated so we can use that memory for another call of `malloc()`. 

You can keep `malloc()`ing and the heap will continue to grow. It will also consist of a lot of holes as the program keeps executing. 

*edited NL9/19, NL9/24*
