14. # POSTGRESQL DATA TYPES IN DEPTH

    -   **_BOOLEAN_**

        -   The following table shows the valid literal values for TRUE and
            FALSE in PostgreSQL. - TRUE - '1', 't', 'y', 'yes', 'true', true - FALSE - '0', 'f', 'n', 'no', 'false', false

        -   To Check Boolean Column Are false

            -   ```sql
                SELECT * FROM table_name WHERE column_name = 'no';
                ```
            -   ```sql
                SELECT * FROM table_name WHERE column_name = '0';
                ```

        -   To Check Boolean Column Are true

            -   ```sql
                SELECT * FROM table_name WHERE column_name = 't';
                ```
            -   ```sql
                SELECT * FROM table_name WHERE column_name = '1';
                ```

        -   Set A Default Value Of The Boolean Column
            -   ```sql
                ALTER TABLE table_name
                ALTER COLUMN column_name
                SET DEFAULT FALSE;
                ```

    -   **_CHAR, VARCHAR AND TEXT_**

        -   The following table illstrate the character types in PostgreSQL:

            -   CHARACTER VARYING(n), VARCHAR(n)

                -   variable-length with length limit

            -   CHARACTER(n), CHAR(n)

                -   fixed-length, blank padded

            -   TEXT(n), VARCHAR

                -   variable unlimited length

    -   **_NUMERIC_**

        -   Syntax

            -   NUMERIC (precision, scale)
            -   In this syntax, the precision is the total number of digits and
                the scale is the number of digits in the fraction part. For example,
                the number 1234.567 has the precision 7 and scale 3.

        -   Numeric Type And NaN

            -   NaN - Not A Number

        -   The NaN is not equal to any number including itself. It means that the
            expression NaN = NaN returns false.

        -   Two NaN values are equal and NaN is greater than other numbers. This
            implementation allows PostgreSQL to sort NUMERIC values and use them in
            tree-based indexes.

    -   **_INTEGER_**

        -   The following table illustrates the specification of each integer type:

            -   SMALLINT - 2 bytes
            -   INTEGER - 4 bytes
            -   BIGINT - 8 bytes

        -   Unlike MySQL integer, PostgreSQL does not provide unsigned integer types.

        -   BIGINT

            -   Using BIGINT type is not only consuming a lot of storage but also
                decreasing the performance of the database, therefore, you should
                have a good reason to use it.

    -   **_DATE_**

        -   PostgreSQL uses 4 bytes to store the date values.

        -   The lowest and highest values of the DATE data type are 4713 BC and
            5874897 AD.

        -   CURRENT DATE

            -   column_name DATE NOT NULL SET DEFAULT CURRENT DATE
            -   You may get a different posting date value based on the current
                date of the database server.

        -   Output A PostgreSQL Date Value In A Specific Format

            -   ```sql
                SELECT column_name (NOW() :: DATE, 'dd/mm/yyyy');
                ```

        -   Get The Interval Between Two dates

            -   ```sql
                SELECT username, now() - creating_date AS age FROM accounts;
                ```

        -   Calculate Ages In Years, Months And Days

            -   To calculate age at the current date in years, months, and days,
                you use the AGE() function.
            -   ```sql
                SELECT first_name, lastname, AGE(birth_date) FROM employees;
                ```

        -   If you pass a date value to the AGE() function, it will subtract that
            date value from the current date. If you pass two arguments to the AGE()
            function, it will subtract the second argument from the first argument.

            -   ```sql
                SELECT first_name, lastname, AGE('2015-01-01', birth_date) FROM employees;
                ```

        -   Extract Year, Quarter, Month, Week, Day From A Date Value

            -   ```sql
                SELECT employee_id, first_name, last_name,
                EXTRACT (YEAR FROM birth_date) AS YEAR,
                EXTRACT (MONTH FROM birth_date) AS MONTH,
                EXTRACT (DAY FROM birth_date) AS DAY
                FROM employees;
                ```

    -   **_TIMESTAMP_**

        -   PostgreSQL provides you with two temporal data types for handling
            timestamp:

            -   timestamp : a timestamp without timezone one.
            -   timestamptz : timestamp with a timezone.

        -   While using timestamp data type, value stored in the database will
            not changed automatically if you change the timezone of your database.

        -   PostgreSQL stores the timestamptz in UTC value.

        -   Both timestamp and timestamptz uses 8 bytes for storing the timestamp
            values.

        -   To Change The timezone

            -   ```sql
                Set timezone = 'your_timezone_location';
                ```

        -   To Show Timezone

            -   ```sql
                SHOW TIMEZONE;
                ```

        -   Timestamp Functions

            -   To get current timestamp

                -   ```sql
                    SELECT NOW();
                    ```

            -   To get current time

                -   ```sql
                    SELECT CURRENT_TIME;
                    ```

            -   To get timeofday in the string format
                -   ```sql
                    SELECT TIMEOFDAY;
                    ```

        -   Convert Between Timezones

            -   To convert a timestamp to another time zone, you use the
                timezone(zone, timestamp) function.
            -   ```sql
                SELECT timezone('America/New_York','2016-06-01 00:00');
                ```
            -   we pass the timestamp as a string to the timezone() function,
                PostgreSQL casts it to timestamptz implicitly. It is better to cast
                a timestamp value to the timestamptz data type explicitly as the
                following statement:

                -   ```sql
                    SELECT timezone('America/New_York','2016-06-01 00:00'::timestamptz);
                    ```

    -   **_INTERVAL_**

        -   The interval data type allows you to store and manipulate a period of
            time in years, months, days, hours, minutes, seconds, etc.

        -   Syntax

            -   @ interval [ fields ] [ (p) ]

        -   An interval value requires 16 bytes storage size that can store a
            period with the allowed range from -178,000,000 years to 178,000,000
            years.

        -   In addition, an interval value can have an optional precision value p
            with the permitted range is from 0 to 6. The precisionp is the number of
            fraction digits retained in the second field.

        -   Example

            -   ```sql
                SELECT NOW(), NOW() INTERVAL '1 year 3 month 20 minute' AS "3 months 20 minutes ago of last year";
                ```

        -   Interval Input Format

            -   Syntax
                -   quantity unit [quantity unit...] [direction]
                -   quantity - is a number, sign + or - is also accepted
                -   unit - can be any of millennium, century, decade, year, month,
                    week, day, hour, minute, second, millisecond, microsecond, or
                    abbreviation (y, m, d, etc.,) or plural forms (months, days, etc.).
                -   direction - can be ago or empty string ''

        -   ISO 8601 Interval Format

            -   Syntax
                -   P quantity unit [ quantity unit ...] [ T [ quantity unit ...]]
                -   In this format, the interval value must start with the letter P.
                    The letter T is for determining time-of-day unit.
                -   Y: Years
                -   M: Months (in the date part)
                -   W: Weeks
                -   D: Days
                -   H: Hours
                -   M: Minutes (in the time part)
                -   S: Seconds
                    !!! M can be months or minutes depending on whether it
                    appears before or after the letter T.
            -   Example
                -   P6Y5M4DT3H2M1S
                -   6 Years 5 Months 4 Days 3 Hours 2 Minutes 1 Seconds
            -   Alternative Form Of ISO 8601 Form
                -   P [ years-months-days ] [ T hours:minutes:seconds ]
            -   Example
                -   P0006-05-04T03:02:01

        -   Interval Output Format

            -   PostgreSQL provides four output formats: sql standard, postgres,
                postgresverbose, and iso_8601. PostgresQL uses the postgres style
                by default for formatting the interval values.
            -   sql standard : +6-5 +4 +3:02:01
            -   postgres : 6 years 5 mons 4 days 03:02:01
            -   postgresverbose : @ 6 years 5 mons 4 days 3 hours 2 mins 1 sec
            -   iso_8601 : P6Y5M4DT3H2M1S

        -   Interval operators

            -   You can apply the arithmetic operator ( +, -, \*, etc.,) to the interval values.
            -   ```sql
                SELECT INTERVAL '2h 50m' + INTERVAL '10m'; -- 03:00:00
                ```
            -   ```sql
                SELECT INTERVAL '2h 50m' - INTERVAL '50m'; -- 02:00:00
                ```
            -   ```sql
                SELECT 600 \* INTERVAL '1 minute'; -- 10:00:00
                ```

        -   Converting PostgreSQL Interval To String

            -   To convert an interval value to string, you use the TO_CHAR()
                function.

                -   ```sql
                    TO_CHAR(interval,format)
                    ```

            -   Example

                -   ```sql
                    SELECT TO_CHAR( INTERVAL '17h 20m 05s', 'HH24:MI:SS' );
                    ```

        -   Extracting Data From A 0PostgreSQL Interval

            -   To extract field such as year, month, date, etc., from an interval,
                you use the EXTRACT() function.

            -   Syntax

                -   ```sql
                    EXTRACT(field FROM interval)
                    ```

            -   Example

                -   ```sql
                    SELECT EXTRACT ( MINUTE FROM INTERVAL '5 hours 21 minutes' );
                    ```

        -   Adjusting Interval Values

            -   PostgreSQL provides two functions justifydays and justifyhours
                that allows you to adjust the interval of 30-day as one month and
                the interval of 24-hour as one day:

                -   ```sql
                    SELECT justify_days(INTERVAL '30 days'), -- 1 mon justify_hours(INTERVAL '24 hours'); -- 1 day
                    ```

            -   The justify_interval function adjusts interval using justifydays
                and justifyhours with additional sign adjustments:

                -   ```sql
                    SELECT justify_interval(interval '1 year -1 hour'); -- 11 mons 29 days 23:00:00
                    ```

    -   **_TIME_**

        -   Syntax

            -   ```sql
                column_name TIME(precision);
                ```
            -   A time value may have a precision up to 6 digits. The precision
                specifies the number of fractional digits placed in the second field.

        -   The TIME data type requires 8 bytes and its allowed range is from
            00:00:00 to 24:00:00.

        -   Common Format Of Time Values:

            -   HH:MI with precision HH:MI.pppppp
            -   HH:MI:SS with precision HH:MI:SS.pppppp
            -   HHMISS with precision HHMISS.pppppp

        -   Time With Time Zone Type

            -   Besides the TIME data type, PostgreSQL provides the TIME with time
                zone data type that allows you to store and manipulate the time of
                day with time zone.
            -   column_name TIME with time zone

        -   Getting Current Time

            -   ```sql
                SELECT CURRENT_TIME;
                ```
            -   ```sql
                SELECT CURRENT_TIME(5); -- 5 is the precision
                ```

        -   Getting Local Time

            -   ```sql
                SELECT LOCALTIME;
                ```
            -   ```sql
                SELECT LOCALTIME(5); -- 5 is the precision
                ```

        -   Converting Time To A Different Time Zone

            -   [TIME with time zone] AT TIME ZONE time_zone
            -   ```sql
                SELECT LOCALTIME AT TIME ZONE 'UTC-7';
                ```

        -   Extracting Hours, Minutes, Seconds From A Time Value

            -   ```sql
                SELECT LOCALTIME,
                EXTRACT (HOUR FROM LOCALTIME) as hour,
                EXTRACT (MINUTE FROM LOCALTIME) as minute,
                EXTRACT (SECOND FROM LOCALTIME) as second,
                EXTRACT (milliseconds FROM LOCALTIME) as milliseconds;
                ```

        -   Arithmetic Operations On Time values

            -   PostgreSQL allows you to apply arithmetic operators such as +, -,
                and \* on time values and between time and interval values.

                -   ```sql
                    SELECT time '10:00' - time '02:00' AS result; -- 08:00:00
                    ```
                -   ```sql
                    SELECT LOCALTIME + interval '2 hours' AS result; -- Adding 2 hour to local time
                    ```

    -   **_UUID_**

        -   UUID stands for Universal Unique Identifier defined by RFC 4122 and
            other related standards.

        -   A UUID value is 128-bit quantity generated by an algorithm that make it
            unique in the known universe using the same algorithm.

        -   The following shows some examples of the UUID values:

            -   40e6215d-b5c6-4896-987c-f30f3678f608
            -   6ecd8c99-4036-403d-bf84-cf8400f67836
            -   3f333df6-90a4-4fda-8dd3-9485d27cee36

        -   Generating UUID Values

            -   PostgreSQL allows you store and compare UUID values but it does
                not include functions for generating the UUID values in its core.

            -   To install the uuid-ossp module, you use the CREATE EXTENSION
                statement as follows:

                -   ```sql
                    CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
                    ```

            -   To generate the UUID values based on the combination of computer’s
                MAC address, current timestamp, and a random value, you use the
                uuid_generate_v1() function:

                -   ```sql
                    SELECT uuid_generate_v1();
                    ```

            -   To generate the UUID values based on the combination of computer’s
                MAC address, current timestamp, and a random value, you use the
                uuid_generate_v1() function:

                -   ```sql
                    SELECT uuid_generate_v4();
                    ```

        -   Creating Table With UUID Column

            -   ```sql
                CREATE TABLE table_name ( ... column_name uuid DEFAULT uuid_generate_v4(); ... );
                ```

    -   **_ARRAY_**

        -   Every data type has its own companion array type e.g., integer has an
            integer[] array type, character has character[] array type, etc.

        -   Creating Table With Array Column

            -   ```sql
                CREATE TABLE contacts ( id serial PRIMARY KEY, name VARCHAR (100), phones TEXT [] );
                ```

        -   Inserting Array Values

            -   ```sql
                INSERT INTO contacts (name, phones)
                VALUES('John Doe',ARRAY [ '(408)-589-5846','(408)-589-5555' ]);
                ```
            -   ```sql
                INSERT INTO contacts (name, phones)
                VALUES('Lily Bush','{"(408)-589-5841"}'), ('William Gate','{"(408)-589-5842","(408)-589-58423"}');
                ```

                -   When you use curly braces, you need to use single quotes ' to
                    wrap the array and double quotes " to wrap text array items.

        -   Query Array Data

            -   We can access array elements using the subscript within square
                brackets []. By default, PostgreSQL uses one-based numbering for
                array elements. It means the first array element starts with number 1.

                -   ```sql
                    SELECT column_name [1]
                    FROM table_name; -- To get first item of array.
                    ```

            -   We can use array element in the WHERE clause as the condition to
                filter the rows.

        -   Modifying Array

            -   PostgreSQL allows you to update each element of an array or the
                whole array. The following statement updates the second phone number
                of William Gate.

                -   ```sql
                    UPDATE contacts
                    SET phones [2] = '(408)-589-5843'
                    WHERE ID = 3;
                    ```

            -   To change whole array:

                -   ```sql
                    UPDATE contacts
                    SET phones = '(408)-589-5843'
                    WHERE ID = 3;
                    ```

        -   Search In Array

            -   ANY() Function

                -   ```sql
                    SELECT name, phones
                    FROM contacts
                    WHERE '(408)-589-5555' = ANY (phones);
                    ```

        -   Expand Arrays

            -   PostgreSQL provides the unnest() function to expand an array to
                a list of rows.

                -   ```sql
                    SELECT name, unnest(phones)
                    FROM contacts;
                    ```

    -   **_HSTORE_**

        -   The hstore module implements the hstore data type for storing
            key-value pairs in a single value.

        -   Enable Hstore Extension

            -   ```sql
                CREATE EXTENSION hstore;
                ```

        -   Create A Table With Hstore Data Type

            -   ```sql
                CREATE TABLE books (
                    id SERIAL PRIMARY KEY,
                    title VARCHAR (255),
                    attr hstore );
                ```

        -   Insert Data Into Hstore Column

            -   ```sql
                INSERT INTO books (title, attr)
                VALUES ( 'PostgreSQL Tutorial',
                '"paperback" => "243",
                "publisher" => "postgresqltutorial.com",
                "language" => "English",
                "ISBN-13" => "978-1449370000",
                "weight"=> "11.2 ounces"' );
                ```

        -   Query Data From An Hstore Column

            -   ```sql
                SELECT attr FROM books;
                ```

        -   Query Value For A Specific Key

            -   Postgresql hstore provides the -> operator to query the value of
                a specific key from an hstore column.

            -   If we want to know ISBN-13 of all available books in the books
                table, we can use the -> operator as follows:

                -   ```sql
                    SELECT attr -> 'ISBN-13' AS isbn FROM books;
                    ```

        -   Use Value In The Where Clause

            -   You can use the -> operator in the WHERE clause to filter the
                rows whose values of the hstore column match the input value
            -   The following query retrieves the title and weight of a book that has ISBN-13 value matches 978-1449370000:

                -   ```sql
                    SELECT title, attr -> 'weight' AS weight
                    FROM books
                    WHERE attr -> 'ISBN-13' = '978-1449370000';
                    ```

        -   Add key-value pairs to existing rows

            -   Syntax

                -   ```sql
                    UPDATE books SET attr = attr || '"freeshipping"=>"yes"' :: hstore;
                    ```

        -   Update Existing Key-Value Pair

            -   Syntax

                -   ```sql
                    UPDATE books SET attr = attr || '"freeshipping"=>"no"' :: hstore;
                    ```

        -   Remove Existing Key-Value Pair

            -   ```sql
                UPDATE books SET attr = delete(attr, 'freeshipping');
                ```

        -   Check For A Specific Key In Hstore Column

            -   You can check for a specific key in an hstore column using the
                ? operator in the WHERE clause.
            -   ```sql
                SELECT title, attr->'publisher' as publisher, attr
                FROM books
                WHERE attr ? 'publisher';
                ```

        -   Check For A Key-Value Pair

            -   You can query based on the hstore key-value pair using the @> operator.
            -   ```sql
                SELECT title
                FROM books
                WHERE attr @> '"weight"=>"11.2 ounces"' :: hstore;
                ```

        -   Query Rows That Contain Multiple Specified keys

            -   You can query the rows whose hstore column contain multiple keys
                using ?& operator.
            -   To check if a row whose hstore column contains any key from a
                list of keys, you use the ?| operator instead of the ?& operator.
            -   ```sql
                SELECT title
                FROM books
                WHERE attr ?& ARRAY [ 'language', 'weight' ];
                ```

        -   Get All Keys From An Hstore Column

            -   To get all keys from an hstore column, you use the akeys()
                function as follows:

                -   ```sql
                    SELECT akeys (attr) FROM books;
                    ```

            -   Or you can use the skey() function if you want PostgreSQL
                to return the result as a set.

                -   ```sql
                    SELECT skeys (attr) FROM books;
                    ```

        -   Get All Values From An Hstore Column

            -   Like keys, you can get all values from an hstore column using the
                avals() function in the form of arrays.

                -   ```sql
                    SELECT avals (attr) FROM books;
                    ```

            -   Or you can use the svals() function if you want to get the
                result as a set.

                -   ```sql
                    SELECT svals (attr) FROM books;
                    ```

        -   Convert Hstore Data To Json

            -   PostgreSQL provides the hstore_to_json() function to convert
                hstore data to JSON.

                -   ```sql
                    SELECT title, hstore_to_json (attr) AS json FROM books;
                    ```

        -   Convert Hstore Data To Sets

            -   To convert hstore data to sets, you use the each() function as follows:

                -   ```sql
                    SELECT title, (EACH(attr) ).* FROM books;
                    ```

    -   **_JSON_**

        -   JSON stands for JavaScript Object Notation. JSON is an open standard
            format that consists of key-value pairs.

        -   The main usage of JSON is to transport data between a server and a
            web application. Unlike other formats, JSON is human-readable text.

        -   Syntax

            -   ```sql
                CREATE TABLE orders (
                    id serial NOT NULL PRIMARY KEY,
                    info json NOT NULL );
                ```
            -   The orders table consists of two columns:

                -   The id column is the primary key column that identifies the order.
                -   The info column stores the data in the form of JSON.

        -   Insert Json Data

            -   To insert data into a JSON column, you have to ensure that data
                is in a valid JSON format.

                -   ```sql
                    INSERT INTO orders (info)
                    VALUES
                    ('{ "customer": "Lily Bush", "items": {"product": "Diaper","qty": 24}}'),
                    ('{ "customer": "Josh William", "items": {"product": "Toy Car","qty": 1}}'),
                    ('{"customer": "Mary Clark", "items": {"product": "Toy Train","qty": 2}}');
                    ```

        -   Querying Json Data

            -   To query JSON data, you use the SELECT statement, which is similar
                to querying other native data types:

                -   ```sql
                    SELECT info FROM orders;
                    ```

            -   PostgreSQL provides two native operators -> and ->> to help you
                query JSON data.

                -   The operator -> returns JSON object field by key.

                    -   ```sql
                        SELECT info -> 'customer' AS customer FROM orders;
                        ```

                -   The operator ->> returns JSON object field by text.

                    -   ```sql
                        SELECT info ->> 'customer' AS customer FROM orders;
                        ```

            -   Because -> operator returns a JSON object, you can chain it with
                the operator ->> to retrieve a specific node. For example, the
                following statement returns all products sold:

                -   ```sql
                    SELECT info -> 'items' ->> 'product' as product
                    FROM orders
                    ORDER BY product;
                    ```

        -   Use Json Operator In Where Clause

            -   We can use the JSON operators in WHERE clause to filter the
                returning rows.

                -   ```sql
                    SELECT info ->> 'customer' AS customer,
                    info -> 'items' ->> 'product' AS product
                    FROM orders
                    WHERE CAST ( info -> 'items' ->> 'qty' AS INTEGER) = 2;
                    ```

            -   Notice that we used the type cast to convert the qty field
                into INTEGER type and compare it with two.

        -   Apply Aggregate Functions To Json Data Type

            -   We can apply aggregate functions such as MIN, MAX, AVERAGE, SUM,
                etc., to JSON data.

                -   ```sql
                    SELECT
                    MIN (CAST (info -> 'items' ->> 'qty' AS INTEGER)),
                    MAX (CAST (info -> 'items' ->> 'qty' AS INTEGER)),
                    SUM (CAST (info -> 'items' ->> 'qty' AS INTEGER)),
                    AVG (CAST (info -> 'items'->> 'qty' AS INTEGER))
                    FROM orders;
                    ```

        -   PostgreSql Json Functions

            -   json_each function

                -   The json_each() function allows us to expand the outermost
                    JSON object into a set of key-value pairs.
                -   ```sql
                    SELECT json_each (info)
                    FROM orders;
                    ```
                -   If you want to get a set of key-value pairs as text, you
                    use the json_each_text() function instead.

            -   json_object_keys function

                -   To get a set of keys in the outermost JSON object, you use the
                    json_object_keys() function.
                -   ```sql
                    SELECT json_object_keys (info->'items')
                    FROM orders;
                    ```

            -   json_typeof function

                -   The json_typeof() function returns type of the outermost JSON
                    value as a string. It can be number, boolean, null, object,
                    array, and string.
                -   ```sql
                    SELECT json_typeof (info->'items')
                    FROM orders;
                    ```
                -   ```sql
                    SELECT json_typeof (info->'items'->'qty')
                    FROM orders;
                    ```

    -   **_USER DEFINED DATA TYPES_**

        -   Besides built-in data types, PostgreSQL allows you to create
            user-defined data types through the following statements:

            -   CREATE DOMAIN creates a user-defined data type with constraints
                such as NOT NULL, CHECK, etc.

            -   CREATE TYPE creates a composite type used in stored procedures
                as the data types of returned values.

        -   PostgreSQL Create Domain Statement

            -   In PostgreSQL, a domain is a data type with optional constraints
                e.g., NOT NULL and CHECK. A domain has a unique name within the
                schema scope.

            -   Example

                -   ```sql
                    CREATE TABLE mailing_list (
                        id SERIAL PRIMARY KEY,
                        first_name VARCHAR NOT NULL,
                        last_name VARCHAR NOT NULL,
                        email VARCHAR NOT NULL,
                        CHECK ( first_name !~ '\s' AND last_name !~ '\s' )
                        );
                    ```
                -   In this table, both first_name and last_name columns do not
                    accept null and spaces.

        -   Getting Domain information

            -   To get all domains in a specific schema, you use the following
                query:

                -   ```sql
                    SELECT typname
                    FROM pg_catalog.pg_type
                    JOIN pg_catalog.pg_namespace
                    ON pg_namespace.oid = pg_type.typnamespace
                    WHERE typtype = 'd' and nspname = '<schema_name>';
                    ```

            -   The following statement returns domains in the public schema of
                the current database:

                -   ```sql
                    SELECT typname
                    FROM pg_catalog.pg_type
                    JOIN pg_catalog.pg_namespace
                    ON pg_namespace.oid = pg_type.typnamespace
                    WHERE typtype = 'd' and nspname = 'public';
                    ```

        -   PostgreSQL CREATE TYPE

            -   The CREATE TYPE statement allows you to create a composite type,
                which can be used as the return type of a function.
            -   ```sql
                 CREATE TYPE film_summary AS (
                     film_id INT,
                     title VARCHAR,
                     release_year SMALLINT );
                ```
            -   ```sql
                CREATE OR REPLACE FUNCTION get_film_summary (f_id INT)
                RETURNS film_summary AS

                $$
                SELECT
                    film_id,
                    title,
                    release_year
                FROM
                    film
                WHERE
                    film_id = f_id ;
                $$

                LANGUAGE SQL;
                ```

            -   To change a user-defined type, you use the ALTER TYPE statement.
            -   To remove a user-defined type, you use the DROP TYPE statement.
