# PostgreSQL-Notes

<img src="https://www.mshowto.org/images/articles/2020/05/1_PY24xlr4TpOkXW04HUoqrQ.jpeg">

## Introduction

This repo was created to share self-taken PostgreSQL notes from the tutorial site (https://www.postgresqltutorial.com/).

I hope it will be useful for you.

## Instruction

1. Clone the repository:
  ```bash
     git clone https://github.com/3n0ugh/PostgreSQL-Notes.git
  ```
2. Open the cloned repository in the code editor.

  - If you are using Visual Studio Code as code editor, try `Ctrl + k Ctrl + 0 ` or `Cmd + k Cmd + 0` shortcut to collapse all file.
 
<p align="center">
  <img src="https://user-images.githubusercontent.com/69458980/128481392-0981a1df-ac7e-469f-be93-262a26422be3.gif" width="700" heigth="400" />
</p>

3. Importing The Sample Database:
   - Download the dvdrental.zip file from the following website:
        https://www.postgresqltutorial.com/wp-content/uploads/2019/05/dvdrental.zip
       
   - Unpack the zip file.
   - Open the terminal, then go to the directory where you unpack the zip file.
   - Execute the following instructions:
      - First enter the PostgreSQL terminal. 
      ```bash
        psql -U user_name
      ```
      - Then create a database named dvdrental.
      ```sql  
        CREATE DATABASE dvdrental;
      ```
      - And then exit from the PostgreSQL terminal using "\q" command.
      ```sql
        \q
      ```
      - After that, use the pg_restore tool to load data into the dvdrental database:
      ```bash
        pg_restore -U username -d dvdrental dvdrental.tar
      ```
       
## Table Of Content For Notes


* ***[QUERYING DATA](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/1-querying-data.md)***
* ***[FILTERING DATA](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/2-filtering-data.md)***
* ***[JOINING MULTIPLE TABLES](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/3-joining-multiple-tables.md)***
* ***[GROUPING DATA](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/4-grouping-data.md)***
* ***[SET OPERATIONS](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/5-set-operations.md)***
* ***[GROUPING SETS, CUBE AND ROLLUP](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/6-grouping-sets-cube-and-rollup.md)***
* ***[SUBQUERY](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/7-subquery.md)***
* ***[COMMON TABLE EXPRESSIONS](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/8-common-table-expressions.md)***
* ***[MODIFYING DATA](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/9-modifiying-data.md)***
* ***[TRANSACTIONS](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/10-transactions.md)***
* ***[IMPORT & EXPORT DATA (FROM/TO CSV)](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/11-import-export-data.md)***
* ***[MANAGING TABLES](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/12-managing-tables.md)***
* ***[UNDERSTANDING POSTGRESQL CONSTRAINTS](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/13-understanding-psql-constraints.md)***
* ***[POSTGRESQL DATA TYPES IN DEPTH](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/14-psql-data-types-in-depth.md)***
* ***[CONDITIONAL EXPRESSIONS AND OPERATORS](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/15-conditional-expressions-and-operators.md)***
* ***[PSQL COMMANDS](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/16-psql-commands.md)***
* ***[POSTGRESQL RECIPES](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/17-psql-recipes.md)***

