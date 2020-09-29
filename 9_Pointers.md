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








*edited NL9/19*
