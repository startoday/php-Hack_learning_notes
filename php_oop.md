1. objects have data fields(attribute that descripe fileds) and methods
    class: a template ; object = instance 

2. class example 
```
Class Person {
  public $fullname = false;
  public $room = false;
  function get_name() {
    if ($this->fullname != false) {
      return $this->fullname;
      }
      return false;
  }
}

$chuck = new Person();
$chuck->room= "4429NQ"
print $chuck->get_name() . "\n"


```

3.  Access "static item" in a class ( :: )   : echo DataTime::RFC822."\n"        // eg, constants in class. static methods 
  
    Access item in an object ( -> )          : echo  $z->format('Y-m-d')."\n"   //dynamic
    
    
4. inheritance: keyword "extends"
    - public; protected(accessed inside the class and derived classes); private (not visible in derived class)   
    
5. PDO : Portable Data Objects, an library for database 

