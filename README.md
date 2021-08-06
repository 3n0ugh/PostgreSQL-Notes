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

* ***[Querying Data](https://github.com/3n0ugh/PostgreSQL-Notes/blob/main/part-by-part/1-querying-data.md)***
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
* ***[]()
