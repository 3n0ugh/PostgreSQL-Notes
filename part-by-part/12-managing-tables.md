12. # MANAGING TABLES

    -   **_DATA TYPES => including Boolean, character, numeric, temporal, array, json, uuid, and special types._**

        -   Boolean

            -   A Boolean data type can hold one of three possible values:
                true, false, null.
            -   When you insert data into a Boolean column, PostgreSQL
                converts it to a Boolean value:

                -   '1', 'yes', 'y', true, 't' values converted to TRUE
                -   '0', 'no', 'n', false, 'f' values converted to FALSE
                -   space value converted to NULL

        -   Character

            -   PostgreSQL provides three character data types:
                CHAR(n), VARCHAR(n), and TEXT

                -   CHAR(n) is the fixed-length character with space padded.
                    If you insert a string that is shorter than the length of the
                    column, PostgreSQL pads spaces. If you insert a string that is
                    longer than the length of the column, PostgreSQL will issue an
                    error.

                -   VARCHAR(n) is the variable-length character string. With
                    VARCHAR(n), you can store up to n characters. PostgreSQL does
                    not pad spaces when the stored string is shorter than the length
                    of the column. \* TEXT is the variable-length character string. Theoretically,
                    text data is a character string with unlimited length.

        -   Numeric

            -   Small integer ( SMALLINT) is 2-byte signed integer that has a
                range from -32,768 to 32,767.
            -   Integer ( INT) is a 4-byte integer that has a range from
                -2,147,483,648 to 2,147,483,647.
            -   Serial is the same as integer except that PostgreSQL will
                automatically generate and populate values into the SERIAL column.
                This is similar to AUTO_INCREMENT column in MySQL or AUTOINCREMENT
                column in SQLite.

        -   Floating-Point Number

            -   float(n) is a floating-point number whose precision, at least,
                n, up to a maximum of 8 bytes.
            -   real or float8 is a 4-byte floating-point number.
            -   numeric or numeric(p,s) is a real number with p digits with s
                number after the decimal point. The numeric(p,s) is the exact number.

        -   Temporal Data types

            -   The temporal data types allow you to store date and /or time
                data. PostgreSQL has five main temporal data types:

                -   DATE stores the dates only.
                -   TIME stores the time of day values.
                -   TIMESTAMP stores both date and time values.
                -   TIMESTAMPTZ is a timezone-aware timestamp data type. It is the abbreviation for timestamp with the time zone.
                -   INTERVAL stores periods of time.

        -   Arrays

            -   In PostgreSQL, you can store an array of strings, an array of
                integers, etc., in array columns. The array comes in handy in some
                situations e.g., storing days of the week, months of the year.

        -   JSON

            -   The JSON data type stores plain JSON data that requires reparsing
                for each processing, while JSONB data type stores JSON data in a
                binary format which is faster to process but slower to insert. In
                addition, JSONB supports indexing, which can be an advantage.

        -   UUID

            -   The UUID data type allows you to store Universal Unique Identifiers
                defined by RFC 4122 . The UUID values guarantee a better uniqueness
                than SERIAL and can be used to hide sensitive data exposed to the
                public such as values of id in URL.

        -   Special Data Types

            -   box – a rectangular box.
            -   line – a set of points.
            -   point – a geometric pair of numbers.
            -   lseg – a line segment.
            -   polygon – a closed geometric.
            -   inet – an IP4 address.
            -   macaddr – a MAC address.

    -   **_CREATE TABLE => statement to create new table._**

        -   Example

            -   `CREATE TABLE [IF NOT EXISTS] table_name ( column1 datatype(length) column_constraint, column2 datatype(length) column_constraint, column3 datatype(length) column_constraint, table_constraints );`
                Code language: SQL (Structured Query Language) (sql)

        -   Constraints

            -   NOT NULL -> ensures that values in a column cannot be NULL.
            -   UNIQUE -> ensures the values in a column unique across the rows
                within the same table.
            -   PRIMARY KEY -> a primary key column uniquely identify rows in a
                table. A table can have one and only one primary key. The primary
                key constraint allows you to define the primary key of a table.
            -   CHECK -> a CHECK constraint ensures the data must satisfy a
                boolean expression.
            -   FOREIGN KEY -> ensures values in a column or a group of columns
                from a table exists in a column or group of columns in another table.
                Unlike the primary key, a table can have many foreign keys.

        -   Example

            -   `CREATE TABLE accounts ( user_id serial PRIMARY KEY, username VARCHAR ( 50 ) UNIQUE NOT NULL, password VARCHAR ( 50 ) NOT NULL, email VARCHAR ( 255 ) UNIQUE NOT NULL, created_on TIMESTAMP NOT NULL, last_login TIMESTAMP );`

            -   `CREATE TABLE account_roles ( user_id INT NOT NULL, role_id INT NOT NULL, grant_date TIMESTAMP, PRIMARY KEY (user_id, role_id), FOREIGN KEY (role_id) REFERENCES roles (role_id), FOREIGN KEY (user_id) REFERENCES accounts (user_id) );`

    -   **_SELECT INTO => to create a new table from the result set of a query._**

        -   Example

            -   `SELECT select_list INTO [ TEMPORARY | TEMP | UNLOGGED ] [ TABLE ] new_table_name FROM table_name WHERE search_condition;`

            -   `SELECT film_id, title, rental_rate INTO TABLE film_r FROM film WHERE rating = 'R' AND rental_duration = 5 ORDER BY title;`

        -   Temporary Table => TEMP

            -   `SELECT film_id, title, length INTO TEMP TABLE short_film FROM film WHERE length < 60 ORDER BY title;`

    -   **_CREATE TABLE AS => to create a new table from the result set of a query._**

        -   Syntax

            -   `CREATE TABLE new_table_name AS query;`

        -   To Create Temporary Table

            -   `CREATE TEMP TABLE new_table_name AS query;`

        -   To Create Unlogged Table

            -   `CREATE UNLOGGED TABLE new_table_name AS query;`

        -   To Rename Columns

            -   `CREATE TABLE new_table_name ( column_name_list ) AS query;`

        -   To Check Table Exists Or Not

            -   `CREATE TABLE IF NOT EXISTS new_table_name AS query;`

        -   Example

            -   `CREATE TABLE IF NOT EXISTS film_rating (rating, film_count) AS SELECT rating, COUNT (film_id) FROM film GROUP BY rating;`

    -   **_AUTO-INCREMENT => SERIAL pseudo-type to define auto-incremenet columns in table._**

        -   Example

            -   `CREATE TABLE table_name ( id SERIAL );`

        -   Three SERIAL pseudo-types

            -   SMALLSERIAL 2 bytes 1 to 32,767
            -   SERIAL 4 bytes 1 to 2,147,483,647
            -   BIGSERIAL 8 bytes 1 to 9,223,372,036,854,775,807

        -   To Get The Sequence Name Of A SERIAL Column In A Table

            -   `pg_get_serial_sequence('table_name', 'column_name')`

        -   You can pass a sequence name to the currval() function to get the
            recent value generated by the sequence.

            -   `SELECT currval(pg_get_serial_sequence('fruits', 'id'));`

    -   **_SEQUENCES => sequence object to generate a sequence of numbers._**

        -   `CREATE SEQUENCE [ IF NOT EXISTS ] sequence_name [ AS { SMALLINT | INT | BIGINT } ] [ INCREMENT [ BY ] increment ] [ MINVALUE minvalue | NO MINVALUE ] [ MAXVALUE maxvalue | NO MAXVALUE ] [ START [ WITH ] start ] [ CACHE cache ] [ [ NO ] CYCLE ] [ OWNED BY { table_name.column_name | NONE } ];`

            -   [ AS { SMALLINT | INT | BIGINT } ]

                -   Specify the data type of the sequence. The valid data type
                    is SMALLINT, INT, and BIGINT. The default data type is
                    BIGINT if you skip it.

            -   [ INCREMENT [ BY ] increment ]

                -   The increment specifies which value to be added to the
                    current sequence value to create new value.
                -   A positive number will make an ascending sequence while a
                    negative number will form a descending sequence.
                -   The default increment value is 1.

            -   [ MINVALUE minvalue | NO MINVALUE ] / [ MAXVALUE maxvalue | NO MAXVALUE ]

                -   Define the minimum value and maximum value of the sequence.
                    If you use NO MINVALUEand NO MAXVALUE, the sequence will use
                    the default value.
                -   For an ascending sequence, the default maximum value is the
                    maximum value of the data type of the sequence and the default
                    minimum value is 1.
                -   In case of a descending sequence, the default maximum value is
                    -1 and the default minimum value is the minimum value of the
                    data type of the sequence.

            -   [ START [ WITH ] start ]

                -   The START clause specifies the starting value of the sequence.
                -   The default starting value is minvalue for ascending sequences
                    and maxvalue for descending ones.

            -   cache

                -   The CACHE determines how many sequence numbers are preallocated
                    and stored in memory for faster access. One value can be
                    generated at a time.
                -   By default, the sequence generates one value at a time i.e.,
                    no cache.

            -   CYCLE | NO CYCLE

                -   The CYCLE allows you to restart the value if the limit is
                    reached. The next number will be the minimum value for the
                    ascending sequence and maximum value for the descending sequence.
                -   If you use NO CYCLE, when the limit is reached, attempting to
                    get the next value will result in an error.
                -   The NO CYCLE is the default if you don’t explicitly specify
                    CYCLE or NO CYCLE.

        -   Creating An Ascending Sequence Example

            -   `CREATE SEQUENCE mysequence INCREMENT 5 START 100;`
            -   to get next value

                -   `SELECT nextval(mysequence);`

        -   Creating An Descending Sequence Example

            -   `CREATE SEQUENCE three INCREMENT -1 MINVALUE 1 MAXVALUE 3 START 3 CYCLE;`

        -   Creating A Sequence Associated With A Table Column

            -   `CREATE SEQUENCE order_item_id START 10 INCREMENT 10 MINVALUE 10 OWNED BY order_details.item_id;`

        -   Listing All Sequences In A Database

            -   `SELECT relname sequence_name FROM pg_class WHERE relkind = 'S';`

        -   Deleting Sequences

            -   If a sequence is associated with a table column, it will be
                automatically dropped once the table column is removed or the table
                is dropped.
            -   `DROP SEQUENCE [ IF EXISTS ] sequence_name [, ...] [ CASCADE | RESTRICT ];`

    -   **_IDENTITY COLUMN => to automatically assign a unique number to a column._**

        -   In This Type:

            -   The type can be SMALLINT, INT, or BIGINT.
            -   The GENERATED ALWAYS instructs PostgreSQL to always generate a
                value for the identity column. If you attempt to insert (or update)
                values into the GENERATED ALWAYS AS IDENTITY column, PostgreSQL
                will issue an error.
            -   The GENERATED BY DEFAULT also instructs PostgreSQL to generate a
                value for the identity column. However, if you supply a value for
                insert or update, PostgreSQL will use that value to insert into the
                identity column instead of using the system-generated value.

        -   GENERATED ALWAYS EXAMPLE

            -   `CREATE TABLE colors ( color_id INT GENERATED ALWAYS AS IDENTITY, color_name VARCHAR NOT NULL );`

        -   GENERATED BY DEFAULT AS IDENTITY

            -   `CREATE TABLE colors ( color_id INT GENERATED BY DEFAULT AS IDENTITY, color_name VARCHAR NOT NULL );`

        -   SEQUENCE OPTIONS EXAMPLE

            -   `CREATE TABLE colors ( color_id INT GENERATED ALWAYS AS IDENTITY (STARTS WITH 10 INCREMENT BY 10), color_name VARCHAR NOT NULL, );`

        -   Adding An Identity Column To An Existing Table

            -   `ALTER TABLE table_name ALTER COLUMN column_name ADD GENERATED { ALWAYS | BY DEFAULT } AS IDENTITY { ( sequence_option ) }`

            -   Creating a table, Then adding an identity column

                -   `CREATE TABLE shape ( shape_id INT NOT NULL, shape_name VARCHAR NOT NULL );`
                -   `ALTER TABLE shape ALTER COLUMN shape_id ADD GENERATED ALWAYS AS IDENTITY;`

        -   Changing An Identity Column

            -   `ALTER TABLE table_name ALTER COLUMN column_name { SET GENERATED { ALWAYS| BY DEFAULT } | SET sequence_option | RESTART [ [ WITH ] restart ] }`
            -   `ALTER TABLE shape ALTER COLUMN shape_id SET GENERATED BY DEFAULT;`

        -   Removing The GENERATED AS IDENTITY Constraint

            -   ` ALTER TABLE table_name ALTER COLUMN column_name DROP IDENTITY [ IF EXISTS ]`

    -   **_ALTER TABLE => to modify the structure of table._**

        -   Syntax

            -   `ALTER TABLE table_name action;`

        -   To Add A New Column

            -   `ALTER TABLE table_name ADD COLUMN column_name data_type column_constraint;`

        -   To Drop A Column

            -   `ALTER TABLE table_name DROP COLUMN column_name;`

        -   To Rename A Column

            -   `ALTER TABLE table_name ALTER COLUMN column_name To new_column_name;`

        -   To Change A Default Value Of The Column

            -   `ALTER TABLE table_name ALTER COLUMN column_name [SET DEFAULT value | DROP DEFAULT];`

        -   To Change the NOT NULL Constraint

            -   `ALTER TABLE table_name ALTER COLUMN column_name [SET NOT NULL value | DROP NOT NULL];`

        -   To Add A Check Constraint

            -   `ALTER TABLE table_name ADD CHECK expression;`

        -   To Add A Constraint To A Table

            -   `ALTER TABLE table_name ADD CONSTRAINT constraint_name constraint_definition;`

        -   To Rename A Table

            -   `ALTER TABLE RENAME TO new_table_name;`

    -   **_TRUNCATE TABLE => to quickly delete all data from large tables._**

        -   Remove All Data From One Table

            -   `TRUNCATE TABLE table_name;`

        -   To Reset The Values In The Identity Column

            -   `TRUNCATE TABLE table_name RESTART IDENTITY;`

        -   Remove All Data From A Table That Has Foreign Key References

            -   `TRUNCATE TABLE table_name CASCADE;`
