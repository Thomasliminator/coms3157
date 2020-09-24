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

## Statements

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
    int x;
    x = 0;
    {
        int x;
        x = 1;
        printf("%d", x);
    }
    printf("%d", x);
}
```

*edited NL9/24*
