1. +-*/%**   *Arithmetic*

for +, -, *
If both operands have type int, the result is int. Otherwise, the operands are converted to float and the result is float.
```
-10 + 100       // int with value 90
100 + -3.4e2    // float with value -240
9.5 + 23.444    // float with value 32.944
-10 - 100       // int with value -110
100 - -3.4e2    // float with value 440
9.5 - 23.444    // float with value -13.944

-10 * 100        // int result with value -1000
100 * -3.4e10    // float result with value -3400000000000.0
```
for / if it is exactly int, then results an int, otherwise float   ； for 指数  If both operands have non-negative integer values and the result can be represented as an int, the result has type int; otherwise, the result has type float
```
300 / 100       // int result with value 3
100 / 123       // float result with value 0.8130081300813
12.34 / 2.3     // float result with value 5.3652173913043


2 ** 3;        // int with value 8
2 ** 3.0;      // float with value 8.0
2.0 ** 3.0;    // float with value 8.0
```

for % : The operator % produces the int remainder from dividing the left-hand int operand by the right-hand int operand. If the right hand side is 0, an exception is thrown.

float operands are rounded to int values.

```
5 % 2     // int result with value 1
5.1 % 2.9 // int result with value 1
```

Note that due to underflow, negating the smallest negative value produces the same value.

2. string concatenation  (.)
```
-10 . \NAN               // string result with value "-10NAN"
\INF . "2e+5"            // string result with value "INF2e+5"
true . null              // string result with value "1"
```

3. comparsion 

comparisons also work on strings. This uses a lexicographic ordering unless both strings are numeric, in which case the numeric values are used:
```
"a" < "b"   // true
"ab" < "b"  // true

"01" < "1"  // false (1 == 1)
```
Arrays are compared by length, then by lexicographic ordering on elements:
```
varray[5] < varray[1, 2]   // true (LHS has fewer elements)

varray[0, 2] < varray[1,2] // true (0 < 1)
```
