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


4. The ??= operator can be used for conditionally writing to a variable if it is null, or to a collection if the specified key is not present or has null value.
The ?? operator is similar to the built-in function idx(). However, an important difference is that idx() only falls back to the specified default value if the given key does not exist, while ?? uses the fallback value even if a key exists but has null value.


5. assign operator (=)
```
$s = "ab";
$s[0] = "x"; // in bounds
$s[3] = "y"; // $s is now "xb y"
```


6.function can take default parameters, Required parameters must come before optional parameters!

You can use ... to define a function that takes a variable number of arguments.
```
function add_value(int $x, int $y = 1): int {
  return $x + $y;
}

function sum_ints(int $val, int ...$vals): int {
  $result = $val;
  
  foreach ($vals as $v) {
    $result += $v;
  }
  return $result;
}

```

Functions are values in Hack, so they can be passed as arguments too. 
```
function apply_func(int $v, (function(int): int) $f): int {
  return $f($v);
}

function usage_example(): void {
  $x = apply_func(0, $x ==> $x + 1);
}


function takes_variadic_fun((function(int...): void) $f): void {
  $f(1, 2, 3);

  $args = vec[1, 2, 3];
  $f(0, ...$args);
}
```

If a type is wrong, HHVM will raise a fatal error. This is controlled with the HHVM option CheckReturnTypeHints. Setting it to 0 or 1 will disable this.

7. annoynomous function ( pass by value not by reference. This is also true for any object property passed to an anonymous function.)   defines by ==>
```
$f = $x ==> $x + 1;
$f2 = $x ==> { return $x + 1; };
$two = $f(1); // result of 2

$x = 5;
$f = $x ==> $x + 1;

$six = $f($x); // pass by value

echo($six); // result of 6
echo("\n");
echo($x); // $x is unchanged; result of 5

//If you need to mutate the reference, use the HSL Ref class.
```

8. format strings
The Hack typechecker can also check that format strings are being used correctly.

This requires that the format string argument is a string literal.

```
// Correct.
Str\format("First: %d, second: %s", 1, "foo");

// Typechecker error: Too few arguments for format string.
Str\format("First: %d, second: %s", 1);

$s = "Number is: %d";

// Typechecker error: Only string literals are allowed here.
Str\format($s, 1);
```

You can define your own functions with format string arguments too.

```
function takes_format_string(
  \HH\FormatString<\PlainSprintf> $format,
  mixed ...$args
): void {}

function use_it(): void {
  takes_format_string("First: %d, second: %s", 1, "foo");
}
//HH\FormatString<PlainSprintf> will check that you've used the right number of arguments. HH\FormatString<Str\SprintfFormat> will also check that arguments match the type in the format string.
```

9.inout parameters 

**inout** provides "copy-in, copy-out" arguments, which allow you to modify the variable in the caller.

**inout** must be written in both the function signature and the function call. This is enforced in the typechecker and at runtime.

dynamic calls can't be used for inout eg: you cannot use meth_caller, call_user_func or ReflectionFunction::invoke.

 ```
 function takes_inout(inout int $x): void {
  $x = 1;
}

function call_it(): void {
  $num = 0;
  takes_inout(inout $num);

  // $num is now 1.
}

//This is similar to copy-by-reference, but the copy-out only happens when the function returns. If the function throws an exception, no changes occur.

function takes_inout(inout int $x): void {
  $x = 1;
  throw new Exception();
}

<<__EntryPoint>>
function call_it(): void {
  $num = 0;
  try {
    takes_inout(inout $num);
  } catch (Exception $_) {
  }

  // $num is still 0.
}

function set_to_value(inout int $item, int $value): void {
  $item = $value;
}

function use_it(): void {
  $items = vec[10, 11, 12];
  $index = 1;
  set_to_value(inout $items[$index], 42);

  // $items is now vec[10, 42, 12].
}

```
 
