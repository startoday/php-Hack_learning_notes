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
