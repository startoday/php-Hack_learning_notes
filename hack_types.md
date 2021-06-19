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



