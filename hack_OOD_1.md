1. class - To define a class, use the class keyword. To create an instance of a class, use new, e.g. new Counter();
```
class Counter {
  private int $i = 0;

  public function increment(): void {
    $this->i += 1;
  }

  public function get(): int {
    return $this->i;
  }
}
```

2.  To call an instance method/properties, use ->.  You can access the current instance with $this inside a method. To call a static method, use ::

```
$b = new IntBox();
$b->value = 42;  //Note that there is no $ used when accessing ->value.
```

Properties in Hack must be initialized. You can provide a default value, or assign to them in the constructor.  Properties with nullable types do not require initial values. They default to *null* if not set.
```
class HasDefaultValue {
  public int $i = 0;
}

class SetInConstructor {
  public int $i;
  public function __construct() {
    $this->i = 0;
  }
}
class MyExample {
  public mixed $m;
  public ?string $s;
}

```
- A static property is a property that is shared between all instances of a class.
- If your property never changes value, you might want to use a class constant instead.
- Properties and methods are in different namespaces. It's possible to have a method and a property with the same name. BUT! (Reusing a name like this is usually confusing. We recommend you use separate names.)
```
class IntBox {
  public int $value = 0;

  public function value(): int {
    return $this->value;
  }
}
//If there are parentheses, Hack knows it's a method call.
$b = new IntBox();
$b->value(); // method call
$b->value; // property access


//If you have a callable value in a property, you will need to be explicit that you're accessing the property.
class FunctionBox {
  public function __construct(public (function(): void) $value) {}
}

$b = new FunctionBox(() ==> { echo "hello"; });
($b->value)(); //Use parentheses to access and call the wrapped function.
```

3. If a method is intended to override a method in a parent class, you should annotate it with <<_ _ Override>>. This has no runtime effect, but ensures you get a type error if the parent method is removed.  
- Hack does not support method overloading. Subclasses methods must have a return type, parameters and visibility that is compatible with the parent class.
- BUT The only exception is constructors, which may have incompatible signatures with the parent class. You can use <<_ _ ConsistentConstruct>> to require subclasses to have compatible types.
- can use parent:: to call an overridden method in the parent class.
```
class IntBox {
  public function __construct(protected int $value) {}

  public function get(): int {
    return $this->value;
  }
}

class IncrementedIntBox extends IntBox {
  <<__Override>>
  public function get(): int {
    return parent::get() + 1;
  }
  
  class MyParent {
  public static function foo(): int {
    return 0;
  }
}

class MyChild extends MyParent {
  <<__Override>>
  public static function foo(): int {
    return parent::foo() + 1;
  }
}

```
- A final class cannot have subclasses. If your class has subclasses, but you want to prevent additional subclasses, use <<_ _ Sealed>>.   If you want to inherit from a final class for testing, use <<_ _ MockClass>>.
- You can also combine final and abstract on classes. This produces a class that cannot be instantiated or have subclasses. The class is effectively a namespace of grouped functionality.
```
abstract final class Example {
  public static function callMe(int $i): int {
    return static::helper($i);
  }

  private static function helper(int $i): int {
    return $i + 1;
  }
}
```

- A final method cannot be overridden in subclasses.



4. A constructor has the name _ _ construct. As such, a class can have only one constructor. (Hack does not support method overloading.)
- constructor paramter promotion: All you do is put a visibility modifier in front of the constructor parameter
```
final class User {
  public function __construct(
    private int $id,
    private string $name,
  ) {}
}
// equals to the below code
final class User {
  private int $id;
  private string $name;

  public function __construct(
    int $id,
    string $name,
  ) {
    $this->id = $id;
    $this->name = $name;
  }
}
```
- Rules for using:
  - A modifier of private, protected or public must precede the parameter declaration in the constructor.
  - Other, non-class-property parameters may also exist in the constructor, as normal.
  - Type annotations must go between the modifier and the parameter's name.
  - The parameters can have a default value.
  - Other code in the constructor is run **after** the parameter promotion assignment.
```
final class User {
  private static dict<int, User> $allUsers = dict[];
  private int $age;

  public function __construct(
    private int $id,
    private string $name,
    // Promoted parameters can be combined with regular non-promoted parameters.
    int $birthday_year,
  ) {
    $this->age = \date('Y') - $birthday_year;
    // The constructor parameter promotion assignments are done before the code
    // inside the constructor is run, so we can use $this->id here.
    self::$allUsers[$this->id] = $this;
  }
}
```


5. For some types—the legacy container types Vector, Map, Set, et al., and closures—there is no way to write an initializer that is considered to be a compile-time constant. Therefore, a class constant cannot have such a type. However, the types array, vec, dict, and set, can be used provided all their subinitializers are constant expressions.


6. Type constants provide a way to abstract a type name. However, type constants only make sense in the context of interfaces and inheritance hierarchies, so they are discussed under those topics. A type constant has public visibility and is implicitly static. By convention, type constant names begin with an uppercase T.

```
abstract class CBase {
  abstract const type T;
  // ...
}

class CString extends CBase {
  const type T = string;
  // ...
}


// example in this way you can help multiple types

abstract class User {
  abstract const type T as arraykey;
  public function __construct(private this::T $id) {}
  public function getID(): this::T {
    return $this->id;
  }
}

trait UserTrait {
  require extends User;
}

interface IUser {
  require extends User;
}

// We know that AppUser will only have int ids
class AppUser extends User implements IUser {
  const type T = int;
  use UserTrait;
}

class WebUser extends User implements IUser {
  const type T = string;
  use UserTrait;
}

class OtherUser extends User implements IUser {
  const type T = arraykey;
  use UserTrait;
}

<<__EntryPoint>>
function run(): void {
  $au = new AppUser(-1);
  \var_dump($au->getID());
  $wu = new WebUser('-1');
  \var_dump($wu->getID());
  $ou1 = new OtherUser(-1);
  \var_dump($ou1->getID());
  $ou2 = new OtherUser('-1');
  \var_dump($ou2->getID());
}



// another good example 


abstract class UserTC {
  abstract const type Ttc as arraykey;
  public function __construct(private this::Ttc $id) {}
  public function getID(): this::Ttc {
    return $this->id;
  }
}

class AppUserTC extends UserTC {
  const type Ttc = int;
}

function get_id_from_userTC(AppUserTC $uc): AppUserTC::Ttc {
  return $uc->getID();
}

<<__EntryPoint>>
function run(): void {
  $autc = new AppUserTC(10);
  \var_dump(get_id_from_userTC($autc));
}


//useage of instance method

abstract class Box {
  abstract const type T;
  public function __construct(private this::T $value) {}
  public function get(): this::T {
    return $this->value;
  }
  public function set(this::T $val): this {
    $this->value = $val;
    return $this;
  }
}

class IntBox extends Box {
  const type T = int;
}

<<__EntryPoint>>
function run(): void {
  $ibox = new IntBox(10);
  \var_dump($ibox);
  $ibox->set(123);
  \var_dump($ibox);
}

```
- Notice the syntax abstract const type <name> [ as <constraint> ];. All type constants are const and use the keyword type. You specify a name for the constant, along with any possible constraints that must be adhered to.
  
- For type constants declared in classes, it is possible to provide a constraint as well as a concrete type. When a constraint is provided this allows the type constant to be overridden by child classes. This feature is not supported for interfaces.
  

  
  

7. Object Disposal
- Note that once we've marked a class as implementing IDisposable, we can only instantiate that class in a using clause's controlling expression or in an appropriately annotated factory method.
- For objects of a class type to be used in an asynchronous context, that type must implement interface IAsyncDisposable instead, which requires a public method called __disposeAsync to be defined with the following signature:
- Now, related using clauses must be preceded by await.
