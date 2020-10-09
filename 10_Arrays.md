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
    
    void strcat(char *t, char *s)
    {
        while(*t) {t++;}
        strcpy(t, s);
    }
    
    int main()
    {
        /*
        char t[4];
        char *s = "abc";
        strcpy(t, s);
        */
        
        char t[6];
        strcpy(t, "abc");
        strcat(t, "xy");
    }
```

`t` and `s` are different variables in `strcpy()` and `main()`. In the "canonical" version, at the end of the loop we will be pointing one past the last element, which is okay in C.

`strcat()` is concatenation, appending to the end of the string. We move the pointer of `t` to `0` and then call `strcpy()` to copy `s` at the end of the string.

Buffer overflow: because the string that you are copying is the target allocated array, you will keep writing past the array. That behavior can be exploited by malicious attackers.

`strcpy()` and `strcat()` will blindly do the while loop until it sees a 0, which is kind of dangerous.

### `strncpy()`

Better safer versions of copy and concatenation:

`strncpy(char *t, char *s, size_t n)`
- `size_t` is basically an unsigned int. It is a limit for the copy (number of characters)

`strncat(char *t, char *s, size_t n)`

## args

Can you write a program that takes command line arguments?

```C
int main(int argc, char **argv) //in the textbook written as char *argv[]
{
```

If we do `$ ./a.out hello world` it will 

`argc` is 3. It will prepare an array of length four where each element is a pointer to an array. It points to the arguments in the command line. The final element in the array is a null pointer.

`argv` points to the array of pointers. 

```C
{
int a[10]
int *p = &a[0];
}
```

Writing a piece of code that prints out the arguments in the command line. Also skip the executable file name. 

```C
int main(int argc, char **argv)
{
    argv++;
    while(*argv)
    {
        printf("%s", *argv++);
    }
}
```

## Lab 2, Part II

In lab 2, make sure that everything is of the right type. Type analysis is very important in the lab.

There will be a main function that can't be changed. 

Recommend doing it in stages.
1. Make sure that you successfully copied and make sure they point to the same strings.
2. Then duplicate the strings and change to capital letters. 

When freeing, you have to free the strings first before you free the middle. 

*updated NL9/26*
