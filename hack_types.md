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
   -   
