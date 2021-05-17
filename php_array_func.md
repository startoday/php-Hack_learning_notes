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
