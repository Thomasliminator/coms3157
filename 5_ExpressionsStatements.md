# 5. Expressions and Statements in C

## Expressions

Expressions are a piece of C program that has a *value*. Every value in C has a *type*.

#### Examples of expressions:

`5` (int)
`x+1` (depends on x)
`f(100)` some function that has a return value
`prime(5)` a function that was written

Things might be less straightforward with an expression such as `x = 1`, where x was declared to be an integer.
- the value of an assignment expression is the value of the variable on the left side (L-value) after the assignment. (This is what should be expected)
- that means that we can do stuff like `(x = 1) = 2` (although unconventional)
- x is still 1 after the above expression although the expression evaluates to 3.

`int x = 1;` this is not an expression, this is declaring and initializing. All expressions are statements but not all statements are expressions.

#### The following are all equivalent:

`x = x + 1;`
`x += 1;`
`++x` (prefix)

### Prefix vs. Postfix

```C
int x = 1;
int y = ++x;
printf("%d", x); //will print 2.
printf("%d", y); //will print 2.
```

```C
int x = 1
int y = x++;
printf("%d", x); //will print 2;
printf("%d", y); //will print 1;
```

Prefix increment, the value of the expression `++x` is the value of the variable after the increment.
The value of the expression `x++` is the value of x before the increment.

What if we do `int y = (x++);`? We still get 1!

### Comparisons (Logical Expressions)

`x == 5` is a comparison. C uses integers as Boolean. Boolean does not exist in C. The type of this expression is int.
- 0 is false; 1 is true; anything that is nonzero is true. 

`if(x < 100 && f(x)) { } ` is a typical complex Boolean expression.

With a logical and (&&) it will short circuit the logical expression if the first half of the expression is false. The same thing works with logical or (||). 

### Bitwise Operator

```C
int x = 1;
int y = 2;
int z;

z = x | y; //3; this is bitwise or.
z = x & y; //0; this is bitwise and.

z = x << 2; //4
```

For bitwise or, you line up the binary representation and do "or" on each digit. If you have at least one "1" you return 1. Same with "and"

`<<` is a left shift. You shift the digits to the left. The ones that fall off are discarded. The remaining on the right side are filled in with zeros. 
Right shift `>>` works the same way except, you fill in what was the left most bit to begin with. You do this because it makes sense to preserve the sign. 

`int z = ~0;` this will flip all of the bits. The previous expression evaluates to -1. 

`int z = 19 & (1<<4);` evaluates to 16. It just picked out a particular bit `000...010000`.
- picks out the 5th bit from the right

#### Practice Problems

```C
int f(int x){
    return x |= (1<<4);
}
```

The above will turn on the 5th bit from the right, whether it was on or not. 

```C
int g(int x){
    return x &= ~(1<<4);
}
```

The above function turns off the 5th bit from the right. 

## Statements and Variables

### Loops

```C
//the following are all ways to loop 10 times.

int i;
for(int=0; i<10; i++){
    
}

while(i<10){
    
    i++;
}
```

### Scopes

A scope in C is a section/region of code, enclosed within {}. Variables declared inside only exist inside the scope. 

```C
foo(){
    int x; //automatic/local/stack variable
    x = 0;
    {
        int x;
        x = 1;
        printf("%d", x);
    }
    printf("%d", x); //this x is from the outside, will print 0.
}
```
Local/**automatic**/stack variables only exist inside the scope they are declared, or any inner scopes. Function parameters are also automatic variables.

### Static Variables

There is a second type of variable called **static** variable. (Has nothing to do with Java "static") There are three different kinds of static variables.
- 1. Global static: these are global variables, declared outside of any function. Discouraged, passing parameters through functions are much better.
- 2. File static
- 3. Function static

bar.c
```C
int x = 5; //this is a global static variable

int f1()
{
    int z; //local variable
    x = x + 1;
    printf("%d", x);
}

int f2() {
```

foo.c
```C
extern int x; //declare int x is external to this file, will be found in link time.
int f3()
{
    x  = x+2; //you can access x declared in main.c
}
```

The difference between the three static variables types is who can access it. In file static, we cannot access outside of the file that it is declared in:

bar.c
```C
static int x = 5; //the static keyword prevents access from any other file.

int f1(){
   
}

int f2() {
```

Function static variables don't behave like they look like they should. 

```C
int f()
{
    static int count = 0; //this does not get reset, the gets set to 0 when it first starts the program.
    count ++;
    return count;
}

int main()
{
    int s = 0;
    for(int i = 0; i < 10; i++) //will count 10 times.
        s += count();
    printf("%d", s);
}
```

`static int count = 0` will get skipped every time the function is called. However, it makes it so that no one else can modify count. This is what you do if you want a global life time but only want to be able to modify it inside of the function.

## Process Address Space

RAM memory has addresses from 0 to (512) (bytes).

Every program/running process thinks that it has 0 to 512G access to RAM. This is fake, **virtual** memory. For example, every single tab in Chrome thinks it has access to this. 

A lot of underlying mapping that goes from the virtual memory to the RAM. Assume you have access to the entire address space. 

64-bit computer means you can fetch/store 8 bytes at a time. Memory address 0 is not used because it is reserved for something special.

In some low address, the program code is stored. Before it calls the main function, it takes up some more on top to put the static variables. 

The automatic variables grow and shrink in size, they are stored at the top region called "stack". 

There is another place called "heap". `malloc` works with the heap region. 


*edited NL9/17, NL9/19, L9/24*
