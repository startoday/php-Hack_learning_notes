1. dollar sign  $x for variable name. If without "$" sign, things can behave weird, eg: $y = x + 2 , x can be a default 0
2. single quotes and double quotes behaves differently.
    - can use \' to escape ' in single quotes.
    - single quotes will not expand \n as a new line
    - single quotes will not expand $variable as well  
3. for comments: can use // , /* */, #
4. echo can do similar thing like print, but it can take multiple parameters without parentheses eg : echo $x, "\n"
   difference between echo and print:
   - echo can print one or more strings
   - print can only print simple types such as int and string
   - print_ r can print more complicated types: array and object
   - var_dump can print both the type and value, so it is easy to read than simple print(remember the "NULL")

5. more agressive implicit type conversion
        eg : $x = "15" + 7  // => 22 
   - equality (  ==   != ) do the type conversion   eg : false == 0
   - identity ( ===   !==) no type conversion eg: false !== 0
   - string concatenation (.) // not a +, + is for math
   - Increment and Decrement (++, -- ) tend not to use it
       eg : 
            $x  = 12;
            $y = 15 + $x++;    //++$x give you y as 28 
            echo " x is $x and y is $y \n" // x is 13 and y is 27
   - Ternary a?: "when a is true": "when a fail"
   - / gives you float number 

6. casting 
    $d = "100" + 36.25 + TRUE ;  => 137.25
    $e = (int)9.9 - 1 ; => 8
    $f = "sam" + 25 ; =>  25  // not freaking out, it just ignore the string as 0
    $g = "sam".25; => sam25
    
    TRUE is 1 and FALSE is nothing~ *FALSE will PRINT NOTHING"
    eg :    echo "A".FALSE."B\n" ; // AB
            echo "X".TRUE."Y\n"  //X1Y
            
    **FALSE, NULL can all be 0, this can be tricky when you do string search (strpos) , if the char is at the zero position, it may be treated as false => suggested to use "==="**        
    
7. whitespace does NOT matter
    if() {
    }  elseif() {
    }  else {
    }

8. for loop   
    for($count=1; $count<=6;$count++){
        }
9. variables in PHP are case sensitive but functions are not; 
    
    for boolean type, TRUE and FALSE, these two are not case sensitive
   
   there are three scopes of variables: local, global, and, static
   
   eg 
   ```
       <? php
       function myTest(){
            static $x=-1;
            echo $x;
            $x--;
       }

       myTest();   // -1
       echo "<br>"
       myTest();  //-2
       echo "<br>"
       myTest();   //-3
       echo "<br>"
       ?>
   ```

10. types 
```
<?php

    var_dump((int)(26/3));     //int(8)  can use int to cast the type

?>
```
