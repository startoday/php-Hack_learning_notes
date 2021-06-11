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
   
   
