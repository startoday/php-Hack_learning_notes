1. Generics allow programmers to write a class or method with the ability to be parameterized to any set of types, all while preserving type safety.

2. Type Constraints
- T as sometype, meaning that T must be a subtype of sometype
- T super sometype, meaning that T must be a supertype of sometype
- T = sometype, meaning that T must be equivalent to sometype. (This is like saying both T as sometype and T super sometype.)
```
function max_val<T as num>(T $p1, T $p2): T {
  return $p1 > $p2 ? $p1 : $p2;
}

<<__EntryPoint>>
function main(): void {
  echo "max_val(10, 20) = ".max_val(10, 20)."\n";
  echo "max_val(15.6, -20.78) = ".max_val(15.6, -20.78)."\n";
}


interface ConstVector<+T> {
  public function concat<Tu super T>(ConstVector<Tu> $x): ConstVector<Tu>;
  // ...
}

```
