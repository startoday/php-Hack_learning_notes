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

6. use prepared statements to avoid sql injection ->  :em    (not $em string cat anymore)

    $pdo -> setAttribute(PDO :: ATTR_ERRMODE, PDO ::ERRORMODE_EXCEPTION)    
    
    SCLIENT is the default mode(really bad!!! also WARNING is not useful very much;  EXECPTION is the one we usually want to choose - >use try and catch for exceptions)
    
    but for productions usually are in logs; tail -f yourlog.file ;  you can check where the log stores in by checking php info
    

7. cookies(browser and http concept)

cookies are pieces of data chosen by the web server and sent to the browser, the browser returns them unchanged to the server. introducing a state(memory of previous events) into otherwise stateless HTTP transcations

cookies are marked ast to the web addresses they come from, and they usually have an expiration date

$_COOKIE is how php set and store cookie


8. session: data stored in server

session is also a super global

in most server applications, as soon as we meet a new unmarked browser, we create a session. We set a session cookie to be stored in the browser which indidates the session id in use. The creation and destruction of sessions is handled by a web framework or some utility code that we use in our applications

session start/ session destory

session is not cookie



