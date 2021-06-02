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
     
    **=>和->** 
    
        =>数组成员访问符号，->对象成员访问符号；
        $this->$name=$value：将当前类的name变量的值设为$value;
        $this代表了类本身，->是访问其类成员的操作符
        双冒号运算符（::）类名::静态属性/方法
        “::”用来调用类中静态的属性和方法
    
    
    
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

session is not cookie， usually the id will be stored in the cookie, if you destory the session, if it is in the same browser/webpage, the id won't change but the session will be empty


9. Example Code of Class:
```
<?php
//类的定义以关键字class开始，类的命名通常以每个单词第一个字母大写
    class NbaPlayer{
        public $name = "Jordan"; //定义属性
        public $height = "198cm";
        public $team = "Bull";
        public $playerNumber = "23";
        
    //定义方法
    public function run(){
        echo "Running\n";
    }
    public function dribblr(){
        echo "Dribbling\n";
    }
    public function pass(){
        echo "Passing\n";
    }
}
    //类到对象的实例化
    //类的实例化为对象时使用关键字new，new之后紧跟类的名称和一对括号
    $jordan = new NbaPlayer();  
    
    //对象中的属性成员可以通过"->"符号来访问
    echo $jordan->name."\n";
    
    //对象中的成员方法可以通过"->"符号来访问
    $jordan->dribble();
    $jordan->run();
?>

//example of consts and static stuff in class

<?php
    class Computer{
        const PI = 3.14159;
    }
echo Computer::PI;
?>

```


10. Constructor and destructor 
```
<?php
//类的定义以关键字class开始，类的命名通常以每个单词第一个字母大写
    class NbaPlayer{
        public $name = "Jordan"; //定义属性
        public $height = "198cm";
        public $team = "Bull";
        public $playerNumber = "23";
        
        //构造函数，在对象被实例化的时候自动调用
        function __construct($name,$height,$weight,$team){
            echo "It is an  NbaPlayer constructor\n";
            $this->name = $name;
            //$this是PHP里面的伪变量，表示对象自身。可以通过$this->的方式访问对象的属性和方法
            $this->height = $height;
            $this->weight = $weight;
            $this->team = $team;
        }
        
        //析构函数，在程序执行结束的时候会自动调用
        //析构函数通常被用于清理程序使用的资源。比如程序使用了打印机，那么可以再析构函数里面释放打印机资源
        function __destruct(){
            echo "Destroying".$this->name."\n";
        }
        
        //定义方法
    public function run(){
        echo "Running\n";
    }
    public function dribblr(){
        echo "Dribbling\n";
    }
    public function pass(){
        echo "Passing\n";
    }
}
    //类到对象的实例化
    //类的实例化为对象时使用关键字new，new之后紧跟类的名称和一对括号
    $jordan = new NbaPlayer("Jordan","198cm","98kg","Bull");    
    
    //对象中的属性成员可以通过"->"符号来访问
    echo $jordan->name."\n";
    
    //对象中的成员方法可以通过"->"符号来访问
    $jordan->dribble();
    $jordan->run();
    
    //每一次用new实例化对象的时候，都会用类名后面的参数列表调用构造函数
    $james = new NbaPlayer("James","203cm","120kg","Heat")
    echo $james->name."\n";
    
    //通过把变量设为null，可以触发析构函数的调用
    //当对象不再使用的时候会触发析构函数
    $james = null;
    echo "from now on James will not be used.\n"
?>
```


11. extend, you can only extend from one parent

```
<?php

class foo
{
    public function printItem($string) 
    {
        echo 'Foo: ' . $string . PHP_EOL;
    }

    public function printPHP()
    {
        echo 'PHP is great.' . PHP_EOL;
    }
}

class bar extends foo
{
    public function printItem($string)
    {
        echo 'Bar: ' . $string . PHP_EOL;
    }
}

$foo = new foo();
$bar = new bar();
$foo->printItem('baz'); // Output: 'Foo: baz'
$foo->printPHP();       // Output: 'PHP is great' 
$bar->printItem('baz'); // Output: 'Bar: baz'
$bar->printPHP();       // Output: 'PHP is great'

?>

```

12. example of how to write a static function in class 
```
class NbaPlayer
{
    // 类的属性的定义
    public $team="Bull";
    public $playerNumber="23";

    private $age="40";  

    public static $president="David Stern";

    public static function changePresident($newPrsdt){
        static::$president = $newPrsdt; // self用于表示当前类，"::"操作符用于访问类的静态成员
        // static关键字也可以用于访问当前类的静态成员
        // echo $this->age . "\n"; // 不能在静态方法中使用this伪变量，也不能用对象的->方式调用静态成员

    }
}   

// 类名加“::”可以访问类的静态成员
// 静态成员不需要实例化就可以访问
echo "The president is ". NbaPlayer::$president. "\n";//The president is David Stern 

NbaPlayer::changePresident("Adam Silver");

echo "The president is changed to ". NbaPlayer::$president. "\n";//The president is changed to Adam Silver

?>
```

13. "Final" a final class can't be extend and a final function in parent class can't be override


14. implments interface: the methods needs to be defined if it inherit any interface and one 
