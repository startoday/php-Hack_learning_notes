1. Example 
  
```   

DELETE FROM Users WHERE email='ted@umich.edu' ;  //where just like an if statement
UPDATE Users SET name='new name' WHERE email='ted@umich.edu' ;
SELECT * FROM Users WHERE email='ted@umich.edu' ;
SELECT * FROM Users ORDER BY email ;
SELECT * FROM Users WHERE name LIKE '%e%' ;  // LIKE is like a "wild card" 
SELECT COUNT( * ) FROM Users WHERE email='ted@umich.edu' ;   // giving you a column of count

```

2. data type: 

- string fields: 
         char (allocates the entire space - faster for small strings where length is known/fixed) vs varchar (usually used for 5-20 chars; allocates a variable amount of space depending on the data length - less space)
- text fields: generally not used with indexing or sorting 
- binary fields(BYTE, VARBINARY): not indexed or sorted
- binary large oject(BLOB): files, images, word, pdf, movies... large raw files; no translation, indexing or char set
- integers (TINYINT, SMALLINT, INT, BIGINT)
- Floating Point Numbers (DOUBLE 64-bit, FLOAT is 32-bit)
- dates (DATETIME, DATE, TIME...)

3. PK and index 
   - Primary key(unique, also extremely fast for integer fields), very little space, exact match 
        
   - Index (I will need to looks this a lot later, please be efficent) - it like a shortcuts, can be hashes or B-trees (sorting)
        good for individual row lookup and sorting/grouping results-> works best with exact matches or prefix lookups
        You can deciding sth like to use a Btree or hash, but can also let system to decide it
                
                ```
                ALTER TABLE User ADD INDEX ( email ) USING BTREE
                ```
                

4. Database Normalization(3NF)
  - Do not replicate data. Instead reference data
  - Use integers for keys 
  - add a special "key" column to each table, which you will make reference to
         
5. Keys
  - Primary Key : Generally an integer auto-increment field (album_id)
  - Logical Key : what the outside world uses for lookup(eg: a column called title) (usually a string)
  - Foreign Key : generally an integer key pointing to a row in another table (eg: artist_id)

  Best practice:
  
  Never use your logical key(string) as the primary key(number)! // even your logical key is unique! it can change, also will be slow, less efficient for relationships; eg if you use email as your pk, when you change, so many table will be changed, usually we just store it into one table! again, 3NF!
  
  When all pks are ints then all fks are ints, this is VERY GOOD!
  
  

6. use Constraint foreign key (artist_id) references Album (album_id) on delete cascade on update cascade

7. The JOIN operation links across several tables as part of a SELECT operation. Use Join with ON to make connection between the tables  // just like a "when"
    joining two tables without an ON clause gives all possible combinations of rows 

  ```
  SELECT Album.title, Artist.name FROM Album JOIN Artisit ON Album.artist_id = Artist.artist_id  //select is tmp, it does not store anything for real
  SELECT Album.title, Artist.name FROM Album JOIN Artisit JOIN C ON Album.artist_id = Artist.artist_id and C.id = Artist.artist_id
  ```
  
  
 8. on delete cascase: what if the depended changed, follow it -> clean up broken references; you can also choose other stuff like set null, restrict ...etc. 
