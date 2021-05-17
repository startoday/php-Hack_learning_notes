1.  Key/Value array
     ```
    <?php 
        $stuff = array ("Hi ", "There");
        echo $stuff[1], "\n"     // There
    ?>
    ```
    can also store key/value
    
    ```
    <?php 
        $stuff = array ("Name" => "chuck" , "course" => "php");
        echo $stuff["course"], "\n"     // php
    ?>
    ```
    
    print_r($stuff) //shows php data || var_dump()   //you can print FALSE! it print both type and value
    
    
 2. 
    ```
    $va = array();
    $va[] = "Hello";
    $va[] = "Wolrd";
    print_r($va)   // Array([0] => Hello [1] => World)
    ```
    loop through an array:
    ```
    <?php
      foreach($stuff as $k => $v){
        echo "Key = ", $k, " Val=", $v, "/n";
      }
    ?>
 3. you can also have arrays of arrays

    ``` 
    $prods = array(
      'pap' =>array(
          'cd' => "yeah",
          'bug' => "free"
       ),
       'pen' =>array(
          'dvd' => "ya",
          'bu' => "fless"
       )
    );
    echo $prods['pap']['cd'];
    ```
    
4. array functions
  ```
  echo isset($za['name']) ?  "name is set\n" : "name is not set\n";
  echo $za['name'] ?? "name is not set\n";  // in php 7
  ```
   sort($za) throw the keys! //worst!!  use ksort and asort
   
   slipt strings into array  $tmp = explode(' ', $inp);
   

5. functions
     ```
     function greet($name = "es"){ //default value
          print "Hello \n"
          return "done"
     }
     greet();
     greet("fr");
     ```
     
   the paramters are passed by value(copy) with $
   can all by ref with an "&" before $name =>  function greet(&$name)



5. global variable (key word global)
     $GET, $POST etc are super globals 
     ```
          function dozap(){
               global $val;
               $val = 100;
          }
          $val = 10;  // name needs to be the same 
          dozap();
          echo "DoZap = $val\n";  // 100 
      ```
  
6.  there is a function called "function_exists("function name") will check if a function exist (due to different versions, some function  may not exist)
    bool array_key_exists()

7.  ```
          <?php
               phpinfo(); //shows internal configuration
          ?>
    ```
          
8. Modularity  
     include "header.php" 
     include_once "header.php" 
     require "header.php"  - pull in the file here and die if it is missing 
     require_once "header.php"  - pull the file unless it ready there, die if it is missing
