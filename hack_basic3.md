1. echo:  This intrinsic function converts the value of an expression to string (if necessary) and writes the string to standard output. 

echo cannot output an array. However, echo can output the value of an object provided its type defines a __ toString method. 
 
2.  exit performs the following operations, in order: 1.  Writes an optional string to standard output. 2. Calls any functions registered via the library function register_shutdown_function in their order of registration.

3. Invariant:  This intrinsic function behaves like a function with a void return type. It is intended to indicate a programmer error for a condition that should never occur. The first argument is a boolean expression. The second argument is a string that can contain text and/or optional formatting information as understood by the library function sprintf. The optional comma-separated list of values following the string must match the set of types expected by the optional formatting information inside that string. 
For example:

```
invariant($obj is B, "Object must have type B");   // may throw  \HH\InvariantException or calls the handler previously registered by the library function invariant_callback_register.
invariant(!$p is null, "Value can't be null");
$max = 100;
invariant(!$p is null && $p <= $max, "\$p's value %d must be <= %d", $p, $max);

```
