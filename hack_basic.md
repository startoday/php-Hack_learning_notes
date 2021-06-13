1. hh_client is the cmd line interface for hack's static analysis, used to find errors in program

   hhvm is used to execute your Hack code, can be used for CLI or as a server

2. use .php extension, will must start with an optional shebang line  (e.g. #!/usr/bin/env hhvm), followed by a header line.  

   ```
    <?hh // strict: this is currently the recommended header, and makes Hack work in the documented ways
   ```

3. special comments

   ```
   // FALLTHROUGH in switch statements
   // strict and // partial in headers
   /* HH_FIXME[1234] */ or /* HH_IGNORE_ERROR[1234] */, which suppresses typechecker error reporting for error 1234
   ```
   
   
4. A name must begin with an upper- or lowercase letter or underscore, which can optionally be followed by those same characters or decimal digits.


Names beginning with two underscores (__) are reserved by the Hack language.


Note that XHP classes have different name constraints. Class names may contain :, and must start with :. XHP categories names start with %.


5. scopes
```
Script, which means from the point of declaration/first initialization through to the end of that script, including any included scripts.
Function, which means from the point of declaration/first initialization through to the end of that function.
      
Each function has its own function scope. An anonymous function has its own scope separate from that of any function inside which that anonymous function is defined.


Class, which means the body of that class and any classes derived from it (defining a class).
Interface, which means the body of that interface, any interfaces derived from it, and any classes that implement it (implementing an interface).
Namespace, which means from the point of declaration/first initialization through to the end of that namespace.
```


6. namespaces

A namespace is a container for a set of (typically related) definitions of classes, interfaces, traits, functions, and constants

The namespace whose prefix is part of a sub-namespace need not actually exist for the sub-namespace to exist. That is, NS1\Sub can exist without NS1.

```
namespace NS1 {
  const int CON1 = 100;
  function f(): void {
    echo "In ".__FUNCTION__."\n";
  }

  class C {
    const int C_CON = 200;
    public function f(): void {
      echo "In ".__NAMESPACE__."...".__METHOD__."\n";
    }
  }

  interface I {
    const int I_CON = 300;
  }

  trait T {
    public function f(): void {
      echo "In ".__TRAIT__."...".__NAMESPACE__."...".__METHOD__."\n";
    }
  }
}

namespace NS2 {
  use type NS1\{C, I, T};

  class D extends C implements I {
    use T;
  }

  function f(): void {
    $d = new D();
    echo "CON1 = ".\NS1\CON1."\n";
    \NS1\f();
  }
}
namespace Hack\UserDocumentation\Fundamentals\Namespaces\Examples\Main;

<<__EntryPoint>>
function main(): void {
  \NS2\f();
}
```


When importing many member names from a given namespace, we can use a group form of use. For example:
```
use NS1\{C, I, T};
```

7. list() is a special syntac for unpacking tuples. 

```
$tuple = tuple('top level', tuple('inner', 'nested'));
list($top_level, list($inner, $nested)) = $tuple;
echo "top level -> {$top_level}, inner -> {$inner}, nested -> {$nested}\n";

```

You can use the special $_ variable to ignore certain elements of the tuple. You can use $_ multiple times in one assignment and have the types be different. You MUST use $_ if you want to ignore the elements at the end. You are not allowed to use a list() with fewer elements than the length of the tuple.


8. example of "[ ]"
```
//For a vec, the brackets can be empty, provided the subscript expression is the destination of an assignment. This results in a new element being inserted at //the right-hand end. The type and value of the result is the type and value of the new element. For example:

$v = vec[10, 25, -6];
$v[] = 99;    // creates new element with key 3, value 99
```

9. The operator -> is used to access instance properties and instance methods on objects.

10. The operator ?-> allows access to objects that may be null.

If the value is null, the result is null. Otherwise, ?-> behaves like ->.
```
function my_example(?IntBox $ib): ?int {
  return $ib?->getX();
}
//Note that arguments are always evaluated, even if the object is null. $x?->foo(bar()) will call bar() even if $x is null.
```

11. :: operator 
``` 
class MyRangeException extends \Exception {
  public function __construct(string $message, ...)
  {
    parent::__construct($message);
    ...
  }
  ...
}

class Base {
  public function b(): void {
    static::f();  // calls the most appropriate f()
  }
  public static function f(): void { ... }
}
class Derived extends Base {
  public static function f(): void { ... }
}
$b1 = new Base();
$b1->b(); // as $b1 is an instance of Base, Base::b() calls Base::f()
$d1 = new Derived();
$d1->b(); // as $d1 is an instance of Derived, Base::b() calls Derived::f()

```

12. error control

The operator @ suppresses any error messages generated by the evaluation of the expression that follows it. For example:

$infile = @fopen("NoSuchFile.txt", 'r');
On open failure, the value returned by fopen is false, which you can use to handle the error. There is no need to have any error message displayed.

If a custom error-handler has been established using the library function set_error_handler that handler is still called.

This syntax can be disabled with the disallow_silence option in .hhconfig.

13. type cast
```
(float)1; // 1.0
(int)3.14; // 3, rounds towards zero

(bool)0; // false
(string)new MyClass(); // calls __toString()
//Casts are only supported for bool, int, float and string. 
```

14.type assertion: is and as

is 用来check type 会return 一个bool

```
function transport(Vehicle $v): void {
  if ($v is Car) {
    $v->drive();
  } else if ($v is Plane) {
    $v->fly();
  } else {
    invariant($v is Boat, "Expected a boat");
    $v->sail();
  }
}

// A common pattern with is refinement is to use nonnull rather than an explicit type.
function transport(?Car $c): void {
  if ($c is nonnull) {
    // Infers that $c is Car, but saves us
    // repeating the name of the type.
    $c->drive();
  }
}
```

It can also check enum
```
enum MyEnum: int {
  FOO = 1;
}
1 is MyEnum       // true
42 is MyEnum      // false
'foo' is MyEnum   // false
```

for generics: Since is provides a runtime check, it cannot be used with erased generics. For generic types, you must use _ placeholders for type parameters.

```
$v = vec[1, 2, 3];

// We can't use `is vec<int>` here.
$v is vec<_>; // true
```

for tuples: For tuples and shapes, is validates the size and recursively validates every field in the value.
```
$x = tuple(1, 2.0, null);
$x is (int, float, ?bool); // true

$y = shape('foo' => 1);
$y is shape('foo' => int); // true
```

aliases:  **is** also works with type aliases and type constants, by testing against the underlying runtime type.
```
type myint = int;
1 is myint; // true
```

**as**
as performs the same checks as is.

However, it throws TypeAssertionException if the value has a different type. The type checker understands that the value must have the type specified afterwards, so it refines the value.

```
1 as int;        // 1
'foo' as int;    // TypeAssertionException

// As enables you to narrow a type.

// Normally you'd want to make transport take a Vehicle
// directly, so you can check when you call the function.
function transport(mixed $m): void {
  // Exception if not a Vehicle.
  $v = $m as Vehicle;

  if ($v is Car) {
    $v->drive();
  } else {
    // Exception if $v is not a Boat.
    $v as Boat;
    $v->sail();
  }
}
```

Hack also provides ?as, which returns null if the type does not match.

```
1 ?as int;        // 1
'foo' ?as int;    // null
```

Note that as can also be used in type signatures when using generics.
