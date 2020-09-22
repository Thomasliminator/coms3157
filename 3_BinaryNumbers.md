# 3. Integer Data Types and Binary Numbers

## Integer Data Types in C

`char` is an integer (smallest data type) that takes up one byte (8 bits) of memory.
- can represent 256 distinct values (-128 ~ 127)

`short` is a type that is two bytes.
- represents values from -2<sup>15</sup> ~ 2<sup>15</sup> - 1

`int` is four bytes. Most common integer data type that is used
- from -2<sup>31</sup> ~ 2<sup>31</sup> - 1

`long` is typically 8 bytes. In older computers it was sometimes 4 bytes.

`long long` is always 8 bytes

2<sup>10</sup> = 1024 (1K)
2<sup>20</sup> = 1024 * 1024 (1M)
2<sup>30</sup> = 1G

### `unsigned`

if you declare something as `unsigned` it only uses positive numbers but has twice the range. Can also be expressed as `uint`.

## Binary Numbers

### Translating from bits to numbers

The standard is two's complement notation. 

Assume we have a 4 bit integer type `int4_t`.

The rule for negative numbers are is that:
- the MSB is given a negative weight

#### ex. 1101

-1 * 2<sup>3</sup> + 1 * 2<sup>2</sup> + 1 * 2<sup>0</sup>

### Consequences of Two's Complement

If the MSB is 1, then can you can always assume that it is a negative number.

If all the bits are 1, then it will be equal to -1. This is the case for any number of bits.

The number 100...0 is the largest (magnitude) negative number.

### Hexadecimal Numbering System

The hexadecimal numbering system goes from 0-9-A-F.

If you mean hex number, you have to write preface it with `0x`
- for example `0x12` is a hexadecimal number. 

#### Examples

`int x = 0xffffffff;` is the same as `int x = -1`

`0x7fffffff` this is the maximum positive number.

`0x80000000` is the greatest magnitude negative number.

### ASCII

```C
{
    int x = 'a';
    
    //is the same as
    int x = 97
    
    int y = 'a' + 1; //is a valid expression
}
``` 

