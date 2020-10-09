# 10. Arrays and Strings

## char [ ]

It works exactly the same way as an `int` array. The only difference is the size of each element. 

```C
{ 
    int a[10];
    char b[5] = {100, -2, 37, -5, 42};
    
    //char c[4] = {97, 98, 99, 0};
    char c[4] {'a', 'b', 'c', '\0'}; //this is equivalent to the line above
    char c[4] = "abc"; //also equivalent
    
    char *p = "abc";
}
```

The array `b` takes up the space of 5 bytes that are initialized with the five numbers listed.
If we have `char` arrays that end with 0, then we treat those as a special case. They are treated as strings.

Note that `'0'` <==> 48. `\0` <==> 0.

String literal expression such as `"abc"` is an anonymous array consisting of ASCII characters and one more character, which is `0`. The type of the expression is `char[4]`.

It becomes a little bit more confusing if we have `char *p = "abc";`. There is no array being declared here. We are only declaring a pointer `p`. We are initializing it with the anonymous array `"abc"` with type `char[4]`. 
Immediately turns itself into `char *` type which will point to the first element. Then where is this `"abc"` stored?
- it is just part of the code, it is a string literal that is not able to be changed or modified. Immutable!

`printf("%s", "hello" + 1)` is a valid line of code. The string is of type `char *` so this is perfectly valid. This statement will print `"ello"`.

## The `strcpy()` function

```C
    void strcpy(char *t, char *s)
    {
        //method 1
        while((*t = *s) != 0)
        {
            t++;
            s++;
        }
        
        //method 2 (canonical way to write strcpy function)
        while((*t++ = *s++) != 0)
            ;
    }
    
    int main()
    {
        char t[4];
        char *s = "abc";
        strcpy(t, s);
    }
```

`t` and `s` are different variables in `strcpy()` and `main()`. In the "canonical" version, at the end of the loop we will be pointing one past the last element, which is okay in C.






*updated NL9/26*
