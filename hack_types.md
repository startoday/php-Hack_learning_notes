1. A soft type hint <<__ Soft>> Foo allows you to add types to code without crashing if you get the type wrong.  Calling definitely_int("foo") will produce an error at runtime, whereas probably_int("foo") will only log a warning.
```
function probably_int(<<__Soft>> int $x): @int {
  return $x + 1;
}

function definitely_int(int $x): int {
  return $x + 1;
}
```

2. A type ?Foo is either a value of type Foo, or null.
3. When an expression of type arraykey is converted, if that expression is currently an int, then int conversion rules apply; otherwise, string conversion rules apply.
4. -  If the source is an empty string or the string "0", the result value is false; otherwise, the result value is true.
   -  If the source is an array with zero elements, the result value is false; otherwise, the result value is true.
   -  If the source is an object, the result value is true, with some legacy exceptions
   -  The library function intval allows values to be converted to int. The library function floatval allows values to be converted to float.
   -  If the source is a resource, the result is the resource's unique ID. If the source is a numeric string or leading-numeric string having integer format, if the precision can be preserved the result value is that string's integer value; otherwise, the result is undefined. For any other string, the result value is 0.
   -  Except for the type classname, no non-string type can be converted implicitly to string. 
   -  If the source type is the classname type, the result value is a string containing the corresponding fully qualified class or interface name without any leading \.
   -  The library function strval allows values to be converted to string.
   -  The predefined resource-like constants STDIN, STDOUT, and STDERR, can be converted implicitly to resource. No other non-resource type can be so converted. No explicit conversions exist.
   -  Any type can be converted implicitly to mixed. No explicit conversions exist.

5. A type alias can be created in two ways: using *type* and *newtype*.
   -  An alias created using *type* is a transparent type alias. For a given type, that type and all transparent aliases to that type are all the same type and can be freely interchanged. There are no restrictions on where a transparent type alias can be defined, or which source code can access its underlying implementation.
   -  An alias created using newtype (such as Point above) is an opaque type alias.  Any file that includes this file has no knowledge that a Counter is really an integer, meaning the including file cannot perform any integer-like operations on that type.  newtype Counter as int = int;

Here, we have a class-specific type constant that is an alias, which allows a value of type T2 to be used in any context an arraykey is expected. After all, any int value is also an arraykey value.
```
class C {
  const type T2 as arraykey = int;
  ...
}
```

6. if T2 is a subtype of T1, program elements designed to operate on T1 can also operate on T2.
- The root of the type hierarchy is the type mixed; as such, every type is a subtype of that type. Any type is a subtype of itself.
- int and float are subtypes of num. int and string are subtypes of arraykey.
- For each type T, T  and null is a subtype of the nullable type ?T.
- string is a subtype of Stringish.
- The predefined types vec, dict, and keyset are subtypes of Container, KeyedContainer, KeyedTraversable, and Traversable.
- If A is an alias for a type T created using type, then A is a subtype of T, and T is a subtype of A.
- If A is an alias for a type T created using newtype, inside the file containing the newtype definition, A is a subtype of T, and T is a subtype of A. Outside that file, A and T have no relationship, except that given newtype A as C = T, outside the file with the newtype definition, A is a subtype of C.
- A class type is a subtype of all its direct and indirect base-class types. A class type is a subtype of all the interfaces it and its direct and indirect base-class types implement. An interface type is a subtype of all its direct and indirect base interfaces.
- A shape type S2 whose field set is a superset of that in shape type S1, is a subtype of S1
- Although noreturn is not a type, per se, it is regarded as a subtype of all other types, and a supertype of none.


7. Certain program elements are capable of changing the type of an expression using what is called type refinement. Consider the following:
```
function f1(?int $p): void {
  //  $x = $p % 3;       // rejected; % not defined for ?int
  if ($p is int) { // type refinement occurs; $p has type int
    $x = $p % 3; // accepted; % defined for int
  }
}
```
An implementation is not required to produce the correct type refinement when using multiple criteria directly.
```
function f4(?num $p): void {
  if (($p is int) || ($p is float)) {
    //    $x = $p**2;    // rejected
  }
}
```
Inside the true path of the if statement, even though we know that $this->p1 is an int to begin with, once any method in this class is called, the implementation must assume that method could have caused a type refinement on anything currently in scope. As a result, the second attempt to left shift is rejected.

```
class C {
  private ?int $p = 8; // holds an int, but type is ?int
  public function m(): void {
    if ($this->p is int) { // type refinement occurs; $this->p1 is int
      $x = $this->p << 2; // allowed; type is int
      $this->n(); // could involve a permanent type refinement on $p
      //      $x = $this->p << 2;   // disallowed; might no longer be int
    }
  }
  public function n(): void { /* ... */ }
```

8. While certain kinds of variables must have their type declared explicitly, others can have their type inferred by having the implementation look at the context in which those variables are used. 
```
$v = 100;
function is_leap_year(int $yy): bool {   // As we can see, $v is implicitly typed as int, and $yy is explicitly typed.
```
- Types must be declared for properties and for the parameters and the return type of named functions.
- Types can be declared or inferred for constants and for the parameters and return type of unnamed functions.

9. The integer type int is signed and uses twos-complement representation for negative values. At least 64 bits are used, so the range of values that can be stored is at least [-9223372036854775808, 9223372036854775807].

Namespace HH\Lib\Math contains the following integer-related constants: INT64_MAX, INT64_MIN, INT32_MAX, INT32_MIN, INT16_MAX, INT16_MIN, and UINT32_MAX.


10. The floating-point type, float, allows the use of real numbers. Using predefined constant names, those values are written as -INF, INF, and NAN (Not-a-Number (NaN)), respectively.


11. A shape is a lightweight type with named fields. It's similar to structs or records in other programming languages. Shape fields are accessed with array indexing syntax, similar to dict. Note that field names must be string literals.
- Shapes are copy-on-write.
- Normally, the type checker will enforce that you provide exactly the fields specified. This is called a 'closed shape'. Shape types may include ... to indicate that additional fields are permitted. This is called an 'open shape'.
```
$my_point = shape('x' => -3, 'y' => 6, 'visible' => true);

$s1 = shape('name' => 'db-01', 'age' => 365);
$s2 = $s1;

$s2['age'] = 42;
// $s1['age'] is still 365.

function takes_named(shape('name' => string) $_): void {}

// Type error: unexpected field 'age'.
takes_named(shape('name' => 'db-01', 'age' => 365));

function takes_named(shape('name' => string, ...) $_): void {}

// OK.
takes_named(shape('name' => 'db-01', 'age' => 365));


// To access the additional fields in an open shape, you can use Shapes::idx.

function takes_named(shape('name' => string, ...) $n): void {
  // The value in the shape, or null if field is absent.
  $nullable_age = Shapes::idx($n, 'age');

  // The value in the shape, or 0 if field is absent.
  $age_with_default = Shapes::idx($n, 'age', 0);
}


function takes_server2(shape('name' => string, 'age' => ?int) $s): void {
  $age = $s['age'] ?? 0;
}

function takes_server3(shape('name' => string, ...) $s): void {
  $age = Shapes::idx($s, 'age', 0) as int;
}

```

12. The type arraykey can represent any integer or string value.
13. 
