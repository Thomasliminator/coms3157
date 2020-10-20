# 11. Structures and Linked Lists

## `struct`

Similar to class in Java. Collects data fields into one object and then you can add methods to it.
In `struct` you cannot add a method in C. 

```C
struct Pt {
    double x;
    double y;
};

int main()
{
struct Pt p1 = { 1.1, 2.2 };

double x1 = p1.x;

struct Pt *q1;
q1 = &p1;

x1 = (*q1).x;
x1 = q1->x; //how to access through the pointer

printf("%lu\n", sizeof(p1)); //prints 16 
}
``` 

This represents a point in a plane. You can access individual items with `p1.x` just like in Java.

`printf("%p\n", q1);` vs. `printf("%p\n", &q1->x);` print the same pointer value. The first is of type `struct Pt *` and the latter is of `double *` type. 

`printf("%p\n", q1 + 1);` vs. `printf("%p\n", &q1->x + 1);` no longer print the same value. They differ by 8. 

## Linked List

Use `struct` to create a Node.

```C
struct Node{
    struct Node *next;
    int val;
};
```

Create a node on the heap using `malloc()`.

`assert(argc == 1)` doesn't do anything if the condition is true. If it is not true, it stops the program. It is a debugging aid. Anything can be inside of the condition.
- You can do a short hand `assert(node)`.

When freeing memory for 2 nodes, free the second one first then free the first. Look at code for `struct Node *create2Nodes(int x, int y)` method.





*updated L10/13, L10/20*
