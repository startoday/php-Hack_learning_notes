1. An interface is a set of method declarations and constants.  
2. Traits are a mechanism for code reuse that overcomes some limitations of Hack single inheritance model. 
In its simplest form a trait defines properties and method declarations. 
A trait cannot be instantiated with new, but it can be used inside one or more classes, via the use clause.

**Informally, whenever a trait is used by a class, the property and method definitions of the trait are inlined (copy/pasted) inside the class itself.**
```
trait T {
  public int $x = 0;

  public function return_even() : int {
    invariant($this->x % 2 == 0, 'error, not even\n');
    $this->x = $this->x + 2;
    return $this->x;
  }
}

class C1 {
  use T;

  public function foo() : void {
    echo "C1: " . $this->return_even() . "\n";
  }
}

class C2 {
  use T;

  public function bar() : void {
    echo "C2: " . $this->return_even() . "\n";
  }
}

<<__EntryPoint>>
function main() : void {
  (new C1()) -> foo();
  (new C2()) -> bar();
}

```
- Traits can contain both instance and static members. If a trait defines a static property, then each class using the trait has its own instance of the static property.
- A trait can access methods and properties of the class that uses it, but these dependencies must be declared using trait requirements.
- For methods, a rule of thumb is "traits provide a method implementation if the class itself does not". 
```

trait T {
  const FOO = 'trait';
}

class B {
  const FOO = 'parent';
}

class A extends B { use T; }

<<__EntryPoint>>
function main() : void {
  \var_dump(A::FOO);  //string(6) "parent"
}
```
- If multiple used traits declare the same constant, the constant inherited from the first trait is used.
```
trait T1 {
  const FOO = 'one';
}

trait T2 {
  const FOO = 'two';
}

class A { use T1, T2; }

<<__EntryPoint>>
function main() : void {
  \var_dump(A::FOO);  //string(3) "one"
}

//example of conflict:, constants inherited from interfaces declared on the class conflict with other inherited constants, including constants declared on traits.
trait T {
  const FOO = 'trait';
}

interface I {
  const FOO = 'interface';
}

class A implements I { use T; }

<<__EntryPoint>>
function main() : void {
  \var_dump(A::FOO);
}

// hh out put:  conflicts with constant `FOO` inherited from `HHVM\UserDocumentation\Guides\Hack\TraitsAndInterfaces\UsingATrait\Traitconflict\I`.


// but this makes sense
interface I1 {
  const FOO = 'one';
}

trait T implements I1 {}

interface I {
  const FOO = 'two';
}

class A implements I { use T; }

<<__EntryPoint>>
function main() : void {
  \var_dump(A::FOO);  // string(3) "two"
}
```


