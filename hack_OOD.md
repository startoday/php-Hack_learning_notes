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
