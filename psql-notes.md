# SOURCES

-   Sample Database -> https://www.postgresqltutorial.com/postgresql-sample-database/
-   GUI Tool -> PgAdmin
-   Source Site -> https://www.postgresqltutorial.com/
-   Github Name -> Serhat Karabulut / 3n0ugh

# FOREKNOWLEDGE

-   To use linux terminal commands in psql terminal -> \! command
-   Comment Line -> --

1.  # QUERYING DATA

    -   **_ SELECT => that retrieves data from a single table _**

        -   Single Columns

            -   ```sql
                SELECT user_id FROM accounts;
                ```

        -   Multiple Columns

            -   ```sql
                SELECT uname, pass FROM accounts;
                ```

        -   All Columns

            -   ```sql
                SELECT * FROM accounts;
                ```

    -   **_ Column Alias => A column alias allows you to assign a column or an expression in the select list of a SELECT statement a temporary name. _**

        -   EXAMPLE

            -   ```sql
                SELECT username AS isim FROM accounts;
                ```

        -   || ( concatenation operator )

            -   ```sql
                SELECT username || ' ' || lastname FROM accounts;
                ```
            -   ```sql
                SELECT username || ' ' || lastname AS fullname FROM accounts;
                ```

        -   Contain Space

            -   ```sql
                SELECT fullname AS "uname lname" FROM accounts;
                ```

    -   **_ ORDER BY => To sort the rows of the result set, you use the ORDER BY clause in the SELECT statement. ( two options as ASC and DESC (default ASC) )_**

        -   Sort Rows By One Column

            -   ```sql
                SELECT username, lastname FROM accounts ORDER BY username ASC;
                ```

        -   Sort Rows By Multiple Columns

            -   ```sql
                SELECT username, lastname FROM accounts ORDER BY username ASC, lastname DESC;
                ```

        -   Sort Rows By Expressions

            -   ```sql
                SELECT username, LENGTH(username) AS len FROM accounts ORDER BY len DESC;
                ```

        -   NULLS FIRST/LAST

            -   ```sql
                SELECT username FROM accounts ORDER BY DESC NULLS LAST;
                ```

    -   **_ SELECT DISTINCT => The DISTINCT clause is used in the SELECT statement to remove duplicate rows from a result set. The DISTINCT clause keeps one row for each group of duplicates. The can be to one or more columns in the select list of the SELECT statement._**

        -   One Column

            -   ```sql
                SELECT DISTINCT bcolor FROM distinct_demo ORDER BY bcolor;
                ```

        -   Multiple Column

            -   ```sql
                SELECT DISTINCT bcolor, fcolor FROM distinct_demo ORDER BY bcolor, fcolor;
                ```

        -   DISTINCT ON

            -   The following statement sorts the result set by the bcolor and fcolor,
                and then for each group of duplicates, it keeps the first row in the
                returned result set.
            -   ```sql
                SELECT DISTINCT ON(bcolor) bcolor, fcolor FROM distinct_demo ORDER BY bcolor, fcolor;
                ```

2.  # FILTERING DATA

    -   **_ WHERE => to filter rows returned by a SELECT statement._**

        -   "=" -> Equal

            -   ```sql
                SELECT username, password FROM accounts WHERE username = 'serhat';
                ```

        -   "\>" -> Greater than

        -   "<" -> Less than

        -   "\>=" -> Greater than or equal

        -   "<=" -> Less than or equal

        -   "<>" or "!=" -> Not equal

            -   ```sql
                SELECT username, password FROM accounts WHERE username <> 'serhat';
                ```

        -   AND

            -   Logical operator AND
            -   ```sql
                SELECT username, password FROM accounts WHERE username = 'serhat' AND username = 'serhat';
                ```

        -   OR

            -   Logical operator OR
            -   ```sql
                SELECT username, password FROM accounts WHERE username = 'serhat' OR password = 'qwer1234!';
                ```

        -   IN

            -   Return true if a value matches any value in a list
            -   ```sql
                SELECT username, password FROM accounts WHERE username IN ('serhat', 'serhat');
                ```

        -   LIKE

            -   Return true if a value matches a pattern
            -   ```sql
                SELECT username, password FROM accounts WHERE username LIKE 'me%';
                ```

        -   IS

            -   NULL Return true if a value is NULL

        -   NOT

            -   Negate the result of other operators

        -   BETWEEN

            -   Return true if a value is between a range of values
            -   ```sql
                SELECT username, password FROM accounts WHERE LENGTH(username) BETWEEN 5 AND 6;
                ```

    -   **_ LIMIT => to get a subset of rows generated by a query. - To Constrain The Number Of Returned Rows_**

        -   ```sql
            SELECT identity, username, password FROM accounts ORDER BY identity LIMIT 4;
            ```

        -   With OFFSET Example

            -   ```sql
                SELECT identity, username, password FROM accounts ORDER BY identity LIMIT 4 OFFSET 3;
                ```
                -- Starting from fourth row to seventh row.

        -   OFFSET To Get Top/Bottom N Rows

            -   ```sql
                SELECT identity, username, password FROM accounts ORDER BY identity DESC LIMIT 4;
                ```

    -   **_ FETCH => to retrieve a portion of rows returned by query._**

        -   ```sql
            SELECT identity, username, password FROM accounts ORDER BY identity FETCH NEXT 5 ROWS ONLY;
            ```

    -   **_ IN => to check if a value matches any value in list._**

        -   Syntax

            -   ```sql
                SELECT identity, username, password FROM accounts WHERE id IN (3, 7) ORDER BY identity DESC;
                ```
                -- returns rows that match the given id numbers.

        -   NOT IN

            -   ```sql
                SELECT identity, username, password FROM accounts WHERE id NOT IN (3, 7) ORDER BY identity DESC;
                ```
                -- returns all rows except id numbers 3 and 7.

        -   Example

            -   ```sql
                SELECT identity, username, password FROM accounts WHERE username IN (SELECT identity FROM rental WHERE CAST (creating_date AS DATE) <> '2001-01-08') ORDER BY identity;
                ```

    -   **_ BETWEEN => to match a value againts a range of values._**

        -   Syntax

            -   ```sql
                SELECT identity, username, password FROM accounts WHERE identity BETWEEN 3 and 5;
                -- returns rows that match id numbers 3, 4 and 5.
                ```

        -   NOT BETWEEN

            -   ```sql
                SELECT identity, username, password FROM accounts WHERE identity NOT BETWEEN 3 and 5;
                -- returns all rows except id numbers 3, 4 and 5.
                ```

        -   Example

            -   ```sql
                SELECT identity, username, password FROM accounts WHERE creating_date BETWEEN '2007-02-07' AND '2007-02-15';
                ```

    -   **_ LIKE => to query data using pattern matchings. (case sensitive)_**

        -   Example

            -   ```sql
                SELECT username, password FROM accounts WHERE username LIKE 'mel%';
                ```
            -   ```sql
                SELECT username, password FROM accounts WHERE username LIKE '_ele_';
                ```
            -   ```sql
                SELECT username, password FROM accounts WHERE username LIKE 'mele\_';
                ```
            -   ```sql
                SELECT username, password FROM accounts WHERE username LIKE '\_el%';
                ```
            -   ```sql
                SELECT username, password FROM accounts WHERE username LIKE '%le\_';
                ```

        -   NOT LIKE

            -   ```sql
                SELECT username, password FROM accounts WHERE username NOT LIKE 'mel%';
                ```

        -   ILIKE (non case sensitive)

            -   ```sql
                SELECT username, password FROM accounts WHERE username ILIKE 'mel%';
                ```

    -   **_ IS NULL => to check if a value is null or not._**

        -   Example

            -   ```sql
                SELECT username, password, phone FROM accounts WHERE phone IS NULL;
                ```

        -   IS NOT NULL

            -   ```sql
                SELECT username, password, phone FROM accounts WHERE phone IS NOT NULL;
                ```

3.  # JOINING MULTIPLE TABLES

    -   **_JOINS => joins including inner join, left join, right joing and full outer join._**

        -   INNER JOIN

            -   It compares the value in the fruit_a column with the value in the
                fruit_b column of each row in the second table (basket_b). If these
                values are equal, the inner join creates a new row that contains
                columns from both tables and adds this new row the result set.
            -   ```sql
                SELECT a, fruit_a, b, fruit_b FROM basket_a INNER JOIN basket_b ON fruit_a = fruit_b;
                ```

        -   LEFT JOIN

            -   The left join starts selecting data from the left table. It
                compares values in the fruit_a column with the values in the fruit_b
                column in the basket_b table. If these values are equal, the left
                join creates a new row that contains columns of both tables and adds
                this new row to the result set. (see the row -1 and -2 in the result
                set). In case the values do not equal, the left join also creates a
                new row that contains columns from both tables and adds it to the
                result set.
            -   ```sql
                SELECT a, fruit_a, b, fruit_b FROM basket_a LEFT JOIN basket_b ON fruit_a = fruit_b;
                ```

        -   RIGHT JOIN

            -   Reversed version of LEFT JOIN.
            -   ```sql
                SELECT a, fruit_a, b, fruit_b FROM basket_a RIGHT JOIN basket_b ON fruit_a = fruit_b;
                ```

        -   FULL OUTER JOIN

            -   The full outer join or full join returns a result set that
                contains all rows from both left and right tables, with the matching
                rows from both sides if available. In case there is no match, the
                columns of the table will be filled with NULL.
            -   ```sql
                SELECT a, fruit_a, b, fruit_b FROM basket_a FULL OUTER JOIN basket_b ON fruit_a = fruit_b;
                ```

    -   **_TABLE ALIASES_**

        -   Using Table Aliases For The Long Table Name To Make Queries More Readable

            -   ```sql
                a_very_long_table_name AS alias
                ```

        -   Using Table Aliases In Join Clauses

            -   ```sql
                SELECT user_identity AS id, username, password, log_date
                FROM accounts AS a INNER JOIN log ON a.id = log.id
                ORDER BY log_date DESC;
                ```

        -   Using Table Aliases In Self-Join

            -   same thing as u think :)

    -   **_INNER JOIN_**

        -   Using PostgreSQL INNER JOIN to join two tables

            -   ```sql
                SELECT customer.customer_id, first_name, last_name, amount, payment_date
                FROM customer
                INNER JOIN payment ON payment.customer_id = customer.customer_id
                ORDER BY payment_date;
                ```

        -   Using PostgreSQL INNER JOIN to join three tables

            -   ```sql
                SELECT c.customer_id, c.first_name customer_first_name,
                c.last_name customer_last_name, s.first_name staff_first_name,
                s.last_name staff_last_name, amount, payment_date
                FROM customer c
                INNER JOIN payment p ON p.customer_id = c.customer_id
                INNER JOIN staff s ON p.staff_id = s.staff_id
                ORDER BY payment_date;
                -- Returns the names of the selling staff and customer
                ```

    -   **_LEFT JOIN_**

        -   Look at ./"3-JOINING MULTIPLE TABLES"/"1-JOINS"/"LEFT JOIN"

    -   **_SELF JOIN => to compare rows within the same table_**

        -   Query Hierarchical data

            -   ```sql
                SELECT e.first_name || ' ' || e.last_name employee,
                m .first_name || ' ' || m .last_name manager
                FROM employee e
                LEFT JOIN employee m ON m .employee_id = e.manager_id
                ORDER BY manager DESC;
                ```

        -   Comparing The Rows With The Same Table

            -   ```sql
                SELECT f1.title, f2.title, f1.length
                FROM film f1
                INNER JOIN film f2 ON f1.film_id <> f2.film_id AND f1.length = f2.length;
                ```
            -   The join predicate matches two different films
                (f1.film_id <> f2.film_id) that have the same length (f1.length = f2.length)

    -   **_FULL OUTER JOIN => to query data from two or more tables._**

        -   Example

            -   ```sql
                SELECT employee_name, department_name
                FROM employees e
                FULL OUTER JOIN departments d ON d.department_id = e.department_id;
                ```

    -   **_CROSS JOIN => to produce a cartesian product of rows from the joined tables._**

        -   Suppose you have to perform a CROSS JOIN of two tables T1 and T2.
            If T1 has n rows and T2 has m rows, the result set will have nxm rows.
            For example, the T1 has 1,000 rows and T2 has 1,000 rows, the result
            set will have 1,000 x 1,000 = 1,000,000 rows.

            -   ```sql
                SELECT * FROM T1 CROSS JOIN T2;
                ```

    -   **_NATURAL JOIN => to query data from two or more tables._**

        -   Example

            -   ```sql
                SELECT * FROM products NATURAL JOIN categories;
                ```

        -   The above statement is equivalent to the following statement that
            uses the INNER JOIN clause.

            -   ```sql
                SELECT * FROM products INNER JOIN categories USING (category_id);
                ```

4.  # GROUPING DATA

    -   **_GROUP BY => to divide rows into groups._**

        -   Without An Aggregate Function Example (similar to SELECT DISTINCT)

            -   ```sql
                SELECT customer_id FROM payment GROUP BY customer_id;
                ```
            -   ```sql
                SELECT DISTINCT customer_id FROM payment;
                ```

        -   With SUM () Function Example

            -   ```sql
                SELECT customer_id, SUM (amounts) AS amount
                FROM payment
                GROUP BY customer_id
                ORDER BY amount DESC;
                ```

        -   Clause With The JOIN Clause

            -   ```sql
                SELECT first_name || '*' || last_name full_name, SUM (amounts) amount
                FROM payment
                INNER JOIN customer USING (customer_id)
                GROUP BY full_name
                ORDER BY customer_id DESC;
                ```

        -   With COUNT () Function Example

            -   ```sql
                 SELECT staff_id, COUNT (payment_id)
                 FROM payment
                 GROUP BY staff_id;
                ```

        -   With Multiple Columns

            -   ```sql
                SELECT customer_id, staff_id, SUM(amount)
                FROM payment
                GROUP BY staff_id, customer_id
                ORDER BY customer_id;
                ```

        -   With Date Column

            -   ```sql
                SELECT DATE(payment_date) paid_date, SUM(amount) sum
                FROM payment
                GROUP BY DATE(payment_date);
                ```

    -   **_HAVING => to specify a search condition for a group or an aggregate._**

        -   HAVING vs. WHERE

            -   The WHERE clause allows you to filter rows based on a specified
                condition. However, the HAVING clause allows you to filter groups
                of rows according to a specified condition.

        -   Clause With SUM () Function Example

            -   ```sql
                SELECT customer_id, SUM (amount)
                FROM payment
                GROUP BY customer_id
                HAVING SUM (amount) > 200;
                ```

        -   Clause With COUNT () Function Example

            -   ```sql
                SELECT store_id, COUNT (customer_id)
                FROM customer
                GROUP BY store_id
                HAVING COUNT (customer_id) > 300;
                ```

5.  # SET OPERATIONS

    -   **_ UNION => to combine result sets of multiple queries into a single result sets._**

        -   Example

            -   ```sql
                SELECT * FROM most_popular_films
                UNION
                SELECT * FROM top_rated_films;
                ```

        -   UNION ALL

            -   the duplicate row is retained in the result set.
            -   ```sql
                SELECT * FROM most_popular_films
                UNION ALL
                SELECT * FROM top_rated_films;
                ```

    -   **_ INTERSECT => to combine result sets of two or more queries._**

        -   ```sql
            SELECT * FROM most_popular_films
            INTERSECT
            SELECT * FROM top_rated_films;
            ```

    -   **_ EXCEPT => to return the rows in the first query that do not appear in the_**
        output of the second query.

        -   Example

            -   ```sql
                SELECT * FROM most_popular_films
                EXCEPT
                SELECT * FROM top_rated_films;
                ```

6.  # GROUPING SETS, CUBE AND ROLLUP

    -   **_GROUPING SETS => to generate multiple grouping sets in a query._**

        -   The Outputs Of The Following Two Clauses Are The Same.

            -   ```sql
                SELECT brand, segment, SUM (quantity)
                FROM sales
                GROUP BY GROUPING SETS ( (brand, segment), (brand), (segment), () );
                ```
            -   ```sql
                SELECT brand, segment, SUM (quantity)
                FROM sales
                GROUP BY brand, segment
                UNION ALL
                SELECT brand, NULL, SUM (quantity)
                FROM sales GROUP BY brand
                UNION ALL
                SELECT NULL, segment, SUM (quantity)
                FROM sales GROUP BY segment
                UNION ALL
                SELECT NULL, NULL, SUM (quantity)
                FROM sales;
                ```

    -   **_CUBE => to generate multiple grouping sets. (with all combinations)_**

        -   Given Same Output as GROUPING SETS's Examples.

            -   ```sql
                SELECT brand, segment, SUM (quantity) quantity
                FROM sales
                GROUP BY CUBE (brand, segment);
                ```

    -   **_ROLLUP => to generate multiple grouping sets. (it just makes subset of those.)_**

        -   Example

            -   ```sql
                SELECT brand, segment, SUM (quantity)
                FROM sales
                GROUP BY ROLLUP (brand, segment)
                ORDER BY segment, brand;
                ```

7.  # SUBQUERY

    -   **_SUBQUERY => that allows you to construct complex queries._**

        -   With IN EXAMPLE

            -   ```sql
                SELECT film_id, title FROM film
                WHERE film_id IN (
                    SELECT inventory.inventory_id
                    FROM rental
                    INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
                    WHERE return_date
                    BETWEEN '2005-05-29' AND '2005-05-30'
                    );
                ```

        -   With EXISTS Operator

            -   ```sql
                SELECT first_name, last_name
                FROM customer
                WHERE EXISTS (
                    SELECT 1 FROM payment
                    WHERE payment.customer_id = customer.customer_id
                    );
                ```

    -   **_ANY OPERATOR => to compare a scalar value with a set of values returned by a subquery._**

        -   The subquery must return exactly one column.

        -   The ANY operator must be preceded by one of the following comparison
            operator =, <=, >, <, > and <>.

        -   The ANY operator returns true if any value of the subquery meets the
            condition, otherwise, it returns false.

        -   SOME is synonym for ANY.

        -   The = ANY is equivalent to IN operator.

            -   ```sql
                SELECT title, category_id FROM film
                INNER JOIN film_category
                USING (film_id)
                WHERE category_id = ANY (
                    SELECT category_id FROM category
                    WHERE NAME = 'Action' OR NAME = 'Drama'
                    );
                ```
            -   ```sql
                SELECT title, category_id FROM film
                INNER JOIN film_category
                USING (film_id)
                WHERE category_id IN (
                    SELECT category_id FROM category
                    WHERE NAME = 'Action' OR NAME = 'Drama'
                    );
                ```

        -   The <> ANY operator is different from NOT IN.

            -   ```sql
                x <> ANY (a, b, c)
                ```
            -   ```sql
                x <> a OR x <> b OR x <> c
                ```

    -   **_ALL OPERATOR => to compare a value with a list of values returned by a subquery._**

        -   The ALL operator must be preceded by a comparison operator such as
            equal (=), not equal (!=), greater than (>), greater than or equal
            to (>=), less than (<), and less than or equal to (<=).

            -   column_name > ALL (subquery) the expression evaluates to true if
                a value is greater than the biggest value returned by the subquery.

            -   column_name >= ALL (subquery) the expression evaluates to true if
                a value is greater than or equal to the biggest value returned by the subquery.

            -   column_name < ALL (subquery) the expression evaluates to true if
                a value is less than the smallest value returned by the subquery.

            -   column_name <= ALL (subquery) the expression evaluates to true if
                a value is less than or equal to the smallest value returned by the subquery.

            -   column_name = ALL (subquery) the expression evaluates to true if
                a value is equal to any value returned by the subquery.

            -   column_name != ALL (subquery) the expression evaluates to true if
                a value is not equal to any value returned by the subquery.

        -   The ALL operator must be followed by a subquery which also must be
            surrounded by the parentheses.

            -   ```sql
                 SELECT film_id, title, length
                 FROM film
                 WHERE length > ALL (
                     SELECT ROUND (AVG (length),2)
                     FROM film
                     GROUP BY rating
                     ) ORDER BY length;
                ```

    -   **_EXISTS => to test for existence of rows in a subquery._**

        -   Example

            -   ```sql
                SELECT first_name, last_name
                FROM customer c
                WHERE EXISTS (
                    SELECT 1 FROM payment p
                    WHERE p.customer_id = c.customer_id AND amount > 11
                    ) ORDER BY first_name, last_name;
                ```

        -   NOT EXISTS

            -   ```sql
                SELECT first_name, last_name
                FROM customer c
                WHERE NOT EXISTS (
                    SELECT 1 FROM payment p
                    WHERE p.customer_id = c.customer_id AND amount > 11
                    ) ORDER BY first_name, last_name;
                ```

        -   NULL

            -   ```sql
                SELECT first_name, last_name
                FROM customer
                WHERE EXISTS( SELECT NULL
                ) ORDER BY first_name, last_name;
                ```

8.  # COMMON TABLE EXPRESSIONS

    -   **_CTE => to simplfy complex queries._**

        -   Simple Example

            -   ```sql
                WITH cte_film AS (
                    SELECT film_id, title, (
                        CASE WHEN length < 30 THEN 'Short'
                        WHEN length < 90 THEN 'medium' ELSE 'Long' END
                        ) length FROM film
                    )SELECT film_id, title, length
                    FROM cte_film
                    WHERE length = 'Long'
                    ORDER BY title;
                ```

        -   Window Function Example

            -   ```sql
                WITH cte_film AS (
                    SELECT film_id, title, rating, length, RANK()
                    OVER (
                        PARTITION BY rating
                        ORDER BY length DESC
                        ) length_rank FROM film
                    ) SELECT * FROM cte_film
                    WHERE length_rank = 1;`
                ``
                ```

        -   Joining CTE With Table Example

            -   ```sql
                WITH cte_rental AS (
                    SELECT staff_id, COUNT(rental_id) rental_count
                    FROM rental
                    GROUP BY staff_id
                    ) SELECT s.staff_id, first_name, last_name, rental_count
                    FROM staff s
                    INNER JOIN cte_rental USING (staff_id);
                ```

    -   **_RECURSIVE QUERY_**

        -   Example

            -   ```sql
                 WITH RECURSIVE subordinates AS (
                     SELECT employee_id, manager_id, full_name
                     FROM employees
                     WHERE employee_id = 2
                     UNION SELECT e.employee_id, e.manager_id, e.full_name
                     FROM employees e
                     INNER JOIN subordinates s ON s.employee_id = e.manager_id
                     ) SELECT * FROM subordinates;
                ```

        -   Non-recursive term: the non-recursive term is a CTE query definition that forms the base result set of the CTE structure.

        -   Recursive term: the recursive term is one or more CTE query definitions joined with the non-recursive term using the UNION or UNION ALL operator. The recursive term references the CTE name itself.

        -   Termination check: the recursion stops when no rows are returned from the previous iteration.

9.  # MODIFYING DATA

    -   **_INSERT => to insert a new row into a table._**

        -   RETURNING Clause

            -   RETURNING clause that returns the information of the inserted row.
            -   ```sql
                INSERT INTO accounts (name, lastname, password)
                VALUES ('serhat', 'krblt', 'qwEr123!')
                RETURNING *;
                ```

        -   Inserting A Single Row Into A Table

            -   ```sql
                INSERT INTO accounts (name, lastname, password)
                VALUES ('serhat', 'krblt', 'qwEr123!');
                ```

        -   Inserting Character String That Contains A Single Quote

            -   ```sql
                INSERT INTO links (url, name)
                VALUES('http://www.oreilly.com','O''Reilly Media');
                ```

        -   Inserting A Date value

            -   ```sql
                INSERT INTO links (url, name, last_update)
                VALUES('https://www.google.com', 'Google', '2013-06-01');
                -- YYYY-MM-DD
                ```

        -   Getting The Last Insert Id

            -   ```sql
                INSERT INTO links (url, name)
                VALUES('http://www.postgresql.org','PostgreSQL')
                RETURNING id;
                ```

    -   **_INSERT MULTIPLE ROWS => to insert multiple rows into a table._**

        -   Example

            -   ```sql
                INSERT INTO links (url, name)
                VALUES ('https://www.google.com', 'google'), ('https://www.github.com', 'github');
                ```

        -   Inserting Multiple Rows And Returning Inserted Rows

            -   ```sql
                INSERT INTO links (url, name)
                VALUES ('https://www.google.com', 'google'), ('https://www.github.com', 'github')
                RETURNING *;
                ```

    -   **_UPDATE => to update existing data in a table._**

        -   Updating One Row

            -   ```sql
                UPDATE courses
                SET published_date = '2020-08-01'
                WHERE course_id = 3;
                ```

        -   Updating A Row And Returning The Updated Row

            -   ```sql
                UPDATE courses
                SET published_date = '2020-08-01'
                WHERE course_id = 3
                RETURNING *;
                ```

    -   **_UPDATE JOIN => to update data in a table based on values in another table._**

        -   Example

            -   ```sql
                UPDATE product
                SET net_price = price - price \* discount
                FROM product_segment
                WHERE product.segment_id = product_segment.id;
                ```

    -   **_DELETE => to delete data from a table._**

        -   To Delete One Row From The Table

            -   ```sql
                DELETE FROM links WHERE id = 8;
                ```

        -   Delete A Row And Return The Deleted Row

            -   ```sql
                DELETE FROM links WHERE id = 8 RETURNING *;
                ```

        -   Delete Multiple Rows From The Table

            -   ```sql
                DELETE FROM links WHERE id IN (5,7) RETURNING *;
                ```

        -   Delete All Rows From The Table
            -   ```sql
                DELETE FROM links;
                ```

    -   **_UPSERT => to insert or update data if the row that is being inserted already exists in the table._**

        -   ON CONFLICT

            -   ```sql
                INSERT INTO customers (name, email)
                VALUES('Microsoft','hotline@microsoft.com')
                ON CONFLICT (name) DO NOTHING;
                ```
            -   ```sql
                INSERT INTO customers (name, email)
                VALUES('Microsoft','hotline@microsoft.com')
                ON CONFLICT (name)
                DO UPDATE SET email = EXCLUDED.email || ';' || customers.email;
                ```

10. # TRANSACTIONS

    -   **_TRANSACTION => transactions using the BEGIN, COMMIT, and ROLLBACK statements._**

        -   A database transaction is a single unit of work that consists of one or more operations.

        -   A PostgreSQL transaction is atomic, consistent, isolated, and durable.
            These properties are often referred to as ACID:

            -   Atomicity guarantees that the transaction completes in an
                all-or-nothing manner.
            -   Consistency ensures the change to data written to the database
                must be valid and follow predefined rules.
            -   Isolation determines how transaction integrity is visible to
                other transactions.
            -   Durability makes sure that transactions that have been committed
                will be stored in the database permanently.

        -   To Start A Transaction

            -   ```sql
                BEGIN;
                ```

        -   Commit A Transaction

            -   ```sql
                COMMIT;
                ```

        -   Rolling Back A Transaction

            -   ```sql
                ROLLBACK;
                ```

        -   Bank Account Example

            -   ```sql
                    -- start a transaction
                    BEGIN;
                ```

                ```sql
                    -- deduct 1000 from account 1
                    UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
                ```

                ```sql
                    -- add 1000 to account 2
                    UPDATE accounts SET balance = balance + 1000 WHERE id = 2;
                ```

                ```sql
                    -- select the data from accounts
                    SELECT id, name, balance FROM accounts;
                ```

                ```sql
                    -- commit the transaction / -- or roll back the transaction
                    COMMIT; / ROLLBACK;
                ```

11. # IMPORT & EXPORT DATA (FROM/TO CSV)

    -   **_IMPORT CSV FILE INTO PostgreSQL TABLE_**

        -   Example

            -   ```sql
                COPY persons(first_name, last_name, dob, email)
                FROM 'C:\sampledb\persons.csv' DELIMITER ',' CSV HEADER;
                ```
            -   If u haven't permission to read that csv file, use \copy instead of COPY.

    -   **_EXPORT PostgreSQL TABLE TO CSV FILE_**

        -   Example

            -   ```sql
                COPY film TO '~/sampleDB/film_db.csv' DELIMITER ',' CSV HEADER;
                ```
            -   If u haven't permission to read that csv file, use \copy instead of COPY.

        -   Another Example Of Usage \copy Command

            -   ```sql
                \copy (SELECT * FROM film) to '~/sampleDB/film_db.csv' with csv
                ```

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

            -   ```sql
                CREATE TABLE [IF NOT EXISTS] table_name (
                    column1 datatype(length) column_constraint,
                    column2 datatype(length) column_constraint,
                    column3 datatype(length) column_constraint,
                    table_constraints );
                ```

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

            -   ```sql
                CREATE TABLE accounts (
                    user_id serial PRIMARY KEY,
                    username VARCHAR ( 50 ) UNIQUE NOT NULL,
                    password VARCHAR ( 50 ) NOT NULL,
                    email VARCHAR ( 255 ) UNIQUE NOT NULL,
                    created_on TIMESTAMP NOT NULL,
                    last_login TIMESTAMP );
                ```

            -   ```sql
                CREATE TABLE account_roles (
                    user_id INT NOT NULL,
                    role_id INT NOT NULL,
                    grant_date TIMESTAMP,
                    PRIMARY KEY (user_id, role_id),
                    FOREIGN KEY (role_id)
                    REFERENCES roles (role_id),
                    FOREIGN KEY (user_id)
                     REFERENCES accounts (user_id) );
                ```

    -   **_SELECT INTO => to create a new table from the result set of a query._**

        -   Example

            -   ```sql
                SELECT select_list
                INTO [ TEMPORARY | TEMP | UNLOGGED ] [ TABLE ] new_table_name
                FROM table_name
                WHERE search_condition;
                ```

            -   ```sql
                SELECT film_id, title, rental_rate
                INTO TABLE film_r
                FROM film
                WHERE rating = 'R' AND rental_duration = 5
                ORDER BY title;
                ```

        -   Temporary Table => TEMP

            -   ```sql
                SELECT film_id, title, length
                INTO TEMP TABLE short_film
                FROM film
                WHERE length < 60
                ORDER BY title;
                ```

    -   **_CREATE TABLE AS => to create a new table from the result set of a query._**

        -   Syntax

            -   ```sql
                CREATE TABLE new_table_name AS query;
                ```

        -   To Create Temporary Table

            -   ```sql
                CREATE TEMP TABLE new_table_name AS query;
                ```

        -   To Create Unlogged Table

            -   ```sql
                CREATE UNLOGGED TABLE new_table_name AS query;
                ```

        -   To Rename Columns

            -   ```sql
                CREATE TABLE new_table_name ( column_name_list ) AS query;
                ```

        -   To Check Table Exists Or Not

            -   ```sql
                CREATE TABLE IF NOT EXISTS new_table_name AS query;
                ```

        -   Example

            -   ```sql
                CREATE TABLE IF NOT EXISTS film_rating (
                    rating, film_count
                    ) AS SELECT rating, COUNT (film_id)
                    FROM film
                    GROUP BY rating;
                ```

    -   **_AUTO-INCREMENT => SERIAL pseudo-type to define auto-incremenet columns in table._**

        -   Example

            -   ```sql
                CREATE TABLE table_name ( id SERIAL );
                ```

        -   Three SERIAL pseudo-types

            -   SMALLSERIAL 2 bytes 1 to 32,767
            -   SERIAL 4 bytes 1 to 2,147,483,647
            -   BIGSERIAL 8 bytes 1 to 9,223,372,036,854,775,807

        -   To Get The Sequence Name Of A SERIAL Column In A Table

            -   ```sql
                pg_get_serial_sequence('table_name', 'column_name')
                ```

        -   You can pass a sequence name to the currval() function to get the
            recent value generated by the sequence.

            -   ```sql
                SELECT currval(pg_get_serial_sequence('fruits', 'id'));
                ```

    -   **_SEQUENCES => sequence object to generate a sequence of numbers._**

        -   ```sql
            CREATE SEQUENCE [ IF NOT EXISTS ] sequence_name
            [ AS { SMALLINT | INT | BIGINT } ]
            [ INCREMENT [ BY ] increment ]
            [ MINVALUE minvalue | NO MINVALUE ]
            [ MAXVALUE maxvalue | NO MAXVALUE ]
             [ START [WITH ] start ]
             [ CACHE cache ]
             [ [ NO ] CYCLE ]
             [ OWNED BY { table_name.column_name | NONE } ];
            ```

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

            -   ```sql
                CREATE SEQUENCE mysequence INCREMENT 5
                START 100;
                ```
            -   to get next value

                -   ```sql
                    SELECT nextval(mysequence);
                    ```

        -   Creating An Descending Sequence Example

            -   ```sql
                CREATE SEQUENCE three INCREMENT -1 MINVALUE 1 MAXVALUE 3
                START 3 CYCLE;
                ```

        -   Creating A Sequence Associated With A Table Column

            -   ```sql
                CREATE SEQUENCE order_item_id
                START 10 INCREMENT 10 MINVALUE 10
                OWNED BY order_details.item_id;
                ```

        -   Listing All Sequences In A Database

            -   ```sql
                SELECT relname sequence_name
                FROM pg_class
                WHERE relkind = 'S';
                ```

        -   Deleting Sequences

            -   If a sequence is associated with a table column, it will be
                automatically dropped once the table column is removed or the table
                is dropped.
            -   ```sql
                DROP SEQUENCE [ IF EXISTS ] sequence_name [, ...] [ CASCADE | RESTRICT ];
                ```

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

            -   ```sql
                CREATE TABLE colors (
                    color_id INT GENERATED ALWAYS AS IDENTITY,
                    color_name VARCHAR NOT NULL );
                ```

        -   GENERATED BY DEFAULT AS IDENTITY

            -   ```sql
                CREATE TABLE colors (
                    color_id INT GENERATED BY DEFAULT AS IDENTITY,
                    color_name VARCHAR NOT NULL );
                ```

        -   SEQUENCE OPTIONS EXAMPLE

            -   ```sql
                CREATE TABLE colors (
                    color_id INT GENERATED ALWAYS AS IDENTITY (STARTS WITH 10 INCREMENT BY 10),
                    color_name VARCHAR NOT NULL, );
                ```

        -   Adding An Identity Column To An Existing Table

            -   ```sql
                ALTER TABLE table_name
                ALTER COLUMN column_name
                ADD GENERATED { ALWAYS | BY DEFAULT }
                AS IDENTITY { ( sequence_option ) }
                ```

            -   Creating a table, Then adding an identity column

                -   ```sql
                    CREATE TABLE shape (
                        shape_id INT NOT NULL,
                        shape_name VARCHAR NOT NULL );
                    ```
                -   ```sql
                    ALTER TABLE shape
                    ALTER COLUMN shape_id
                    ADD GENERATED ALWAYS AS IDENTITY;
                    ```

        -   Changing An Identity Column

            -   ```sql
                ALTER TABLE table_name
                ALTER COLUMN column_name
                { SET GENERATED { ALWAYS| BY DEFAULT }
                | SET sequence_option |
                RESTART [ [ WITH ] restart ] }
                ```
            -   ```sql
                ALTER TABLE shape
                ALTER COLUMN shape_id
                SET GENERATED BY DEFAULT;
                ```

        -   Removing The GENERATED AS IDENTITY Constraint

            -   ```sql
                 ALTER TABLE table_name
                 ALTER COLUMN column_name
                 DROP IDENTITY [ IF EXISTS ]
                ```

    -   **_ALTER TABLE => to modify the structure of table._**

        -   Syntax

            -   ```sql
                ALTER TABLE table_name action;
                ```

        -   To Add A New Column

            -   ```sql
                ALTER TABLE table_name
                ADD COLUMN column_name data_type column_constraint;
                ```

        -   To Drop A Column

            -   ```sql
                ALTER TABLE table_name
                DROP COLUMN column_name;
                ```

        -   To Rename A Column

            -   ```sql
                ALTER TABLE table_name
                ALTER COLUMN column_name To new_column_name;
                ```

        -   To Change A Default Value Of The Column

            -   ```sql
                ALTER TABLE table_name
                ALTER COLUMN column_name
                [SET DEFAULT value | DROP DEFAULT];
                ```

        -   To Change the NOT NULL Constraint

            -   ```sql
                ALTER TABLE table_name
                ALTER COLUMN column_name
                [SET NOT NULL value | DROP NOT NULL];
                ```

        -   To Add A Check Constraint

            -   ```sql
                ALTER TABLE table_name
                ADD CHECK expression;
                ```

        -   To Add A Constraint To A Table

            -   ```sql
                ALTER TABLE table_name
                ADD CONSTRAINT constraint_name constraint_definition;
                ```

        -   To Rename A Table

            -   ```sql
                ALTER TABLE RENAME TO new_table_name;
                ```

    -   **_TRUNCATE TABLE => to quickly delete all data from large tables._**

        -   Remove All Data From One Table

            -   ```sql
                TRUNCATE TABLE table_name;
                ```

        -   To Reset The Values In The Identity Column

            -   ```sql
                TRUNCATE TABLE table_name RESTART IDENTITY;
                ```

        -   Remove All Data From A Table That Has Foreign Key References

            -   ```sql
                TRUNCATE TABLE table_name CASCADE;
                ```

13. # UNDERSTANDING POSTGRESQL CONSTRAINTS

    -   **_PRIMARY KEY => A primary key is a column or a group of columns used to identify a row uniquely in a table._**

        -   Define Primary Key While Creating A Table

            -   ```sql
                 CREATE TABLE TABLE (
                     column_1 data_type PRIMARY KEY,
                     column_2 data_type, … );
                ```

        -   To Define Primary Key For Two Or More Column

            -   ```sql
                CREATE TABLE TABLE (
                    column_1 data_type,
                    column_2 data_type, …
                    PRIMARY KEY (column_1, column_2) );
                ```

        -   Define Primary Key While Changing The Existing Table Structure

            -   ```sql
                ALTER TABLE table_name
                ADD PRIMARY KEY (column_1, column_2);
                ```

        -   To Add An Auto-Incremented Primary Key To An Existing Table

            -   ```sql
                ALTER TABLE existing_table_name
                ADD COLUMN column_name SERIAL PRIMARY KEY;
                ```

        -   Remove Primary Key

            -   ```sql
                 ALTER TABLE table_name
                 DROP CONSTRAINT primary_key_constraint;
                ```

    -   **_FOREIGN KEY => A foreign key is a column or a group of columns in a table that reference the primary key of another table._**

        -   Syntax

            -   ```sql
                [CONSTRAINT fk_name]
                FOREIGN KEY(fk_columns)
                REFERENCES parent_table(parent_key_columns)
                [ON DELETE delete_action]
                [ON UPDATE update_action]
                ```

        -   NO ACTION

            -   If you try to delete a referenced primary key, you will get an error.

        -   SET NULL

            -   The SET NULL automatically sets NULL to the foreign key columns in
                the referencing rows of the child table when the referenced rows in
                the parent table are deleted.
            -   ```sql
                CREATE TABLE contacts(
                    contact_id INT GENERATED ALWAYS AS IDENTITY,
                    customer_id INT,
                    contact_name VARCHAR(255) NOT NULL,
                    phone VARCHAR(15),
                    email VARCHAR(100),
                    PRIMARY KEY(contact_id),
                    CONSTRAINT fk_customer
                    FOREIGN KEY(customer_id)
                    REFERENCES customers(customer_id)
                    ON DELETE SET NULL -- Important!!!! );
                ```

        -   CASCADE

            -   The ON DELETE CASCADE automatically deletes all the referencing
                rows in the child table when the referenced rows in the parent
                table are deleted.
            -   ```sql
                CREATE TABLE contacts(
                    contact_id INT GENERATED ALWAYS AS IDENTITY,
                    customer_id INT,
                    contact_name VARCHAR(255) NOT NULL,
                    phone VARCHAR(15),
                    email VARCHAR(100),
                    PRIMARY KEY(contact_id),
                    CONSTRAINT fk_customer
                    FOREIGN KEY(customer_id)
                    REFERENCES customers(customer_id)
                    ON DELETE CASCADE );
                ```

        -   SET DEFAULT

            -   The ON DELETE SET DEFAULT sets the default value to the foreign
                key column of the referencing rows in the child table when the
                referenced rows from the parent table are deleted.

        -   Add A Foreign Key Constraint To An Existing Table

            -   ```sql
                ALTER TABLE child_table
                ADD CONSTRAINT constraint_name
                FOREIGN KEY (fk_columns)
                REFERENCES parent_table (parent_key_columns);
                ```

    -   **_CHECK => A CHECK constraint is a kind of constraint that allows you to specify if values in a column must meet a specific requirement._**

        -   Define PostgreSQL CHECK Constraint For New Tables

            -   ```sql
                DROP TABLE IF EXISTS employees;
                CREATE TABLE employees (
                    id SERIAL PRIMARY KEY,
                    first_name VARCHAR (50),
                    last_name VARCHAR (50),
                    birth_date DATE CHECK (birth_date > '1900-01-01'
                ), joined_date DATE CHECK (joined_date > birth_date),
                salary numeric CHECK(salary > 0) );
                ```

        -   Define PostgreSQL CHECK Constraint For Existing Tables

            -   ```sql
                ALTER TABLE prices_list
                ADD CONSTRAINT price_discount_check
                CHECK ( price > 0 AND discount >= 0 AND price > discount );
                ```

    -   **_UNIQUE => to constraint the uniqueness of the data correctly._**

        -   When a UNIQUE constraint is in place, every time you insert a new row,
            it checks if the value is already in the table. It rejects the change
            and issues an error if the value already exists. The same process is
            carried out for updating existing data.

        -   Syntax (two variant)

            -   ```sql
                email VARCHAR (50) UNIQUE UNIQUE (email)
                CREATE TABLE person (
                    id SERIAL PRIMARY KEY,
                    first_name VARCHAR (50),
                    last_name VARCHAR (50),
                    email VARCHAR (50) UNIQUE );
                ```

        -   Creating A UNIQUE Constraint On Multiple Columns

            -   ```sql
                CREATE TABLE table (
                    c1 data_type,
                    c2 data_type,
                    c3 data_type
                    UNIQUE (c2, c3) );
                ```

        -   Adding UNIQUE Constraint Using A UNIQUE INDEX

            -   ```sql
                CREATE UNIQUE INDEX CONCURRENTLY
                equipment_equip_id ON equipment (equip_id);
                ```
            -   Add a unique constraint to the equipment
                table using the equipment_equip_id index.

            -   ```sql
                ALTER TABLE equipment
                ADD CONSTRAINT unique_equip_id
                UNIQUE USING INDEX equipment_equip_id;
                ```

        -   ALTER TABLE statement acquires an exclusive lock on the table. If you
            have any pending transactions, it will wait for all transactions to
            complete before changing the table.

    -   **_NOT-NULL => to ensure the values of a column are not null._**

        -   NOT NULL CONSTRAINT

            -   ```sql
                CREATE TABLE table_name (
                    ... column_name data_type NOT NULL,
                    ... );
                ```

        -   Declaring NOT NULL Columns

            -   ```sql
                CREATE TABLE invocies (
                    id SERIAL PRIMARY KEY,
                    product_id INT NOT NULL,
                    qty NUMERIC NOT NULL CHECK (qty > 0),
                    net_price numeric CHECK (net_price > 0) );
                ```

        -   Adding NOT NULL Constraint To An Existing Table

            -   ```sql
                ALTER TABLE table_name
                ALTER COLUMN column_name SET NOT NULL;
                ```

        -   The Special Case Of NOT NULL Constraint

            -   Besides the NOT NULL constraint, you can use a CHECK constraint
                to force a column to accept not NULL values.
            -   ```sql
                CHECK (column IS NOT NULL)
                ```
            -   ```sql
                CREATE TABLE users (
                    id serial PRIMARY KEY,
                    username VARCHAR (50),
                    password VARCHAR (50),
                    email VARCHAR (50),
                    CONSTRAINT username_email_notnull CHECK ( NOT ( (
                        username IS NULL OR username = ''
                        ) AND (
                            email IS NULL OR email = ''
                            ) ) ) );
                ```

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

15. # CONDITIONAL EXPRESSIONS AND OPERATORS

    -   **_CASE_**

        -   The PostgreSQL CASE expression is the same as IF/ELSE statement in other
            programming languages. It allows you to add if-else logic to the query to
            form a powerful query.

        -   Since CASE is an expression, you can use it in any places where an
            expression can be used e.g.,SELECT, WHERE, GROUP BY, and HAVING clause.

        -   General PostgreSQL Case Expression

            -   Syntax

                -   ```sql
                    CASE
                    WHEN condition_1 THEN result_1
                    WHEN condition_2 THEN result_2
                    [WHEN ...]
                    [ELSE else_result]
                    END
                    ```
                -   In this syntax, each condition (condition_1, condition_2…) is
                    a boolean expression that returns either true or false.
                -   When a condition evaluates to false, the CASE expression
                    evaluates the next condition from the top to bottom until it
                    finds a condition that evaluates to true.

            -   Example

                -   ```sql
                    SELECT title,
                    length,
                    CASE
                    WHEN length> 0
                    AND length <= 50 THEN 'Short'
                    WHEN length > 50
                    AND length <= 120 THEN 'Medium'
                    WHEN length> 120 THEN 'Long'
                    END duration
                    FROM film
                    ORDER BY title;
                    ```

        -   Using Case With An Aggregate Function Example

            -   ```sql
                SELECT
                SUM (CASE
                WHEN rental_rate = 0.99 THEN 1
                ELSE 0
                END
                ) AS "Economy",
                SUM (
                CASE
                WHEN rental_rate = 2.99 THEN 1
                ELSE 0
                END
                ) AS "Mass",
                SUM (
                CASE
                WHEN rental_rate = 4.99 THEN 1
                ELSE 0
                END
                ) AS "Premium"
                FROM
                film;
                ```

        -   Simple PostgreSQL Case Expression

            -   Syntax

                -   ````sql
                    CASE expression
                    WHEN value_1 THEN result_1
                    WHEN value_2 THEN result_2
                    [WHEN ...]
                    ELSE
                    else_result
                    END
                    ```
                    ````

            -   Example

                -   ```sql
                    SELECT title,
                    rating,
                    CASE rating
                    WHEN 'G' THEN 'General Audiences'
                    WHEN 'PG' THEN 'Parental Guidance Suggested'
                    WHEN 'PG-13' THEN 'Parents Strongly Cautioned'
                    WHEN 'R' THEN 'Restricted'
                    WHEN 'NC-17' THEN 'Adults Only'
                    END rating_description
                    FROM film
                    ORDER BY title;
                    ```

        -   Using Simple PostgreSQL Case Expression With Aggregate Function Example

            -   ```sql
                SELECT
                SUM(CASE rating
                WHEN 'G' THEN 1
                ELSE 0
                END) "General Audiences",
                SUM(CASE rating
                WHEN 'PG' THEN 1
                ELSE 0
                END) "Parental Guidance Suggested",
                SUM(CASE rating
                WHEN 'PG-13' THEN 1
                ELSE 0
                END) "Parents Strongly Cautioned",
                SUM(CASE rating
                WHEN 'R' THEN 1
                ELSE 0
                END) "Restricted",
                SUM(CASE rating
                WHEN 'NC-17' THEN 1
                ELSE 0
                END) "Adults Only"
                FROM film;
                ```

    -   **_COALESCE => to returns the first non-null argument._**

        -   Syntax

            -   ```sql
                COALESCE (argument_1, argument_2, …);
                ```

        -   The COALESCE function accepts an unlimited number of arguments. It
            returns the first argument that is not null. If all arguments are null,
            the COALESCE function will return null.

        -   The COALESCE function evaluates arguments from left to right until it
            finds the first non-null argument.

        -   Example

            -   ```sql
                SELECT
                product,
                (price - COALESCE(discount,0)) AS net_price
                FROM
                items;
                ```

        -   Usage With Case Expression

            -   ```sql
                SELECT
                product,
                (price - COALESCE(discount,0)) AS net_price
                FROM
                items;
                ```

    -   **_NULLIF => to handle null values._**

        -   Syntax

            -   ```sql
                NULLIF(argument_1,argument_2);
                ```

        -   The NULLIF function returns a null value if argument_1 equals to
            argument_2, otherwise it returns argument_1.

        -   Example

            -   ```sql
                SELECT
                id,
                title,
                COALESCE (
                NULLIF (excerpt, ''),
                LEFT (body, 40)
                )
                FROM
                posts;
                ```

        -   Use NULLIF To Prevent Division-By-Zero Error

            -   ```sql
                SELECT
                (
                SUM (
                CASE
                WHEN gender = 1 THEN
                1
                ELSE
                0
                END
                ) / NULLIF (
                SUM (
                CASE
                WHEN gender = 2 THEN
                1
                ELSE
                0
                END
                ),
                0
                )
                ) \* 100 AS "Male/Female ratio"
                FROM
                members;
                ```

    -   **_CAST => to convert a value of one type to another._**

        -   Syntax

            -   ```sql
                CAST ( expression AS target_type );
                ```

        -   PostgreSQL Type Cast :: Operator

            -   Example

                -   ```sql
                    SELECT
                    '100'::INTEGER,
                    '01-OCT-2015'::DATE;
                    ```

        -   Cast A String To An Integer Example

            -   ```sql
                SELECT
                CAST ('100' AS INTEGER);
                ```

        -   Cast A String To A Date Example

            -   ```sql
                SELECT
                CAST ('2015-01-01' AS DATE), -- convert to January 1st 2015
                CAST ('01-OCT-2015' AS DATE); -- convert to October 1st 2015
                ```

        -   Cast A String To A Double Example

            -   ```sql
                SELECT
                CAST ('10.2' AS DOUBLE);
                ```

        -   Cast A String To A Boolean Example

            -   ```sql
                SELECT
                CAST('true' AS BOOLEAN),
                CAST('false' as BOOLEAN),
                CAST('T' as BOOLEAN),
                CAST('F' as BOOLEAN);
                ```

        -   Convert A String To A Timestamp Example

            -   ```sql
                SELECT '2019-06-15 14:30:20'::timestamp;
                ```

        -   Convert A String To A Interval Example

            -   ```sql
                SELECT '15 minute'::interval,
                '2 hour'::interval,
                '1 day'::interval,
                '2 week'::interval,
                '3 month'::interval;
                ```

16. # PSQL COMMANDS

    1. **_Connect To PostgreSQL Database_**

        - psql -d database -U user -W

        - If you want to connect to a database that resides on another host,
          you add the -h option as follows: - psql -h host -d database -U user -W

        - In case you want to use SSL mode for the connection, just specify it as
          shown in the following command: - psql -U user -h host "dbname=db sslmode=require"

    2. **_Switch Connection To A New Database_**

        - \c dbname username

    3. **_List Available Databases_**

        - \l

    4. **_List Available Tables_**

        - \dt

    5. **_Describe A Table_**

        - \d table_name

    6. **_List Available Schema_**

        - \dn

    7. **_List Available Functions_**

        - \df

    8. **_List Available Views_**

        - \dv

    9. **_List Users And Their Roles_**

        - \du

    10. **_Execute The Previous Command_**

        - \g

    11. **_Command History_**

        - \s

        - To save command history to a file.
            - \s file_name

    12. **_Executes Psql Commands From A File_**

        - \i file_name

    13. **_Get Help On Psql Commands_**

        - \?

        - To get help on specific PostgreSQL statement:
            - \h statement

    14. **_Turn On Query Execution Time_**

        - \timing

    15. **_Edit Command In Your Own Editor_**

        - \e

        - To editing a function
            - \ef [function name]

    16. **_Switch Output Options_**

        - \a command switches from aligned to non-aligned column output.

        - \H command formats the output to HTML format.

    17. **_Quit Psql_**

        - \q

17. # POSTGRESQL RECIPES

    -   **_Compare Two Tables In PostgreSQL_**

        -   Compare two tables using EXCEPT and UNION operators

            -   ```sql
                SELECT
                ID,
                NAME,
                'not in bar' AS note
                FROM
                foo
                EXCEPT
                SELECT
                ID,
                NAME,
                'not int bar' AS note
                FROM
                bar
                dvdrental-- UNION
                dvdrental-- SELECT
                ID,
                NAME,
                'not in foo' AS note
                FROM
                bar
                EXCEPT
                SELECT
                ID,
                NAME,
                'not in foo' AS note
                FROM
                foo;
                ```

        -   Compare two tables using OUTER JOIN

            -   ```sql
                SELECT
                id,
                name
                FROM
                foo
                FULL OUTER JOIN bar USING (id, name)
                WHERE
                foo.id IS NULL
                OR bar.id IS NULL;
                ```

    -   **_How To Delete Duplicate Rows In PostgreSQL_**

        -   Preparing Sample Data

            -   ```sql
                CREATE TABLE basket (
                id SERIAL PRIMARY KEY,
                fruit VARCHAR(50) NOT NULL
                );
                ```
            -   ```sql
                INSERT INTO basket(fruit) values('apple');
                INSERT INTO basket(fruit) values('apple');
                INSERT INTO basket(fruit) values('orange');
                INSERT INTO basket(fruit) values('orange');
                INSERT INTO basket(fruit) values('orange');
                INSERT INTO basket(fruit) values('banana');
                ```

        -   Finding duplicate rows

            -   ```sql
                SELECT
                fruit,
                COUNT( fruit )
                FROM
                basket
                GROUP BY
                fruit
                HAVING
                COUNT( fruit )> 1
                ORDER BY
                fruit;
                ```

        -   Deleting duplicate rows using DELETE USING statement

            -   ```sql
                DELETE FROM
                basket a
                USING basket b
                WHERE
                a.id < b.id
                AND a.fruit = b.fruit;
                ```

        -   Deleting duplicate rows using subquery

            -   ```sql
                DELETE FROM basket
                WHERE id IN
                (SELECT id
                FROM
                (SELECT id,
                ROW_NUMBER() OVER( PARTITION BY fruit
                ORDER BY id ) AS row_num
                FROM basket ) t
                WHERE t.row_num > 1 );
                ```

            -   In case you want to delete duplicate based on values of multiple
                columns, here is the query template:

                -   ```sql
                    DELETE FROM table_name
                    WHERE id IN
                    (SELECT id
                    FROM
                    (SELECT id,
                    ROW_NUMBER() OVER( PARTITION BY column_1,
                    column_2
                    ORDER BY id ) AS row_num
                    FROM table_name ) t
                    WHERE t.row_num > 1 );
                    ```

        -   Deleting duplicate rows using an immediate table

            -   To delete rows using an immediate table, you use the following steps:

                -   Create a new table with the same structure as the one whose duplicate rows should be removed.
                -   Insert distinct rows from the source table to the immediate table.
                -   Drop the source table.
                -   Rename the immediate table to the name of the source table.

                -   ```sql
                    -- step 1
                    CREATE TABLE basket_temp (LIKE basket);

                    -- step 2
                    INSERT INTO basket_temp(fruit, id)
                    SELECT
                    DISTINCT ON (fruit) fruit,
                    id
                    FROM basket;

                    -- step 3
                    DROP TABLE basket;

                    -- step 4
                    ALTER TABLE basket_temp
                    RENAME TO basket;
                    ```

    -   **_How To Generate A Random Number In A range_**

        -   PostgreSQL provides the random() function that returns a random number
            between 0 and 1.

            -   ```sql
                SELECT random();
                ```

        -   To generate a random number between 1 and 11, you use the following
            statement:

            -   ```sql
                  SELECT random() * 10 + 1 AS RAND_1_11;
                ```

        -   If you want to generate the random number as an integer, you apply the
            floor() function to the expression as follows:

            -   ```sql
                  SELECT floor( random() \* 10 + 1 ) :: int;
                ```

        -   You can develop a user-defined function that returns a random number
            between two numbers l and h:

            -   ```sql
                CREATE OR REPLACE FUNCTION random_between(low INT ,high INT)
                RETURNS INT AS
                $$
                BEGIN
                RETURN floor(random()* (high-low + 1) + low);
                END;
                $$ language 'plpgsql' STRICT;
                $$
                ```

        -   If you want to get multiple random numbers between two integers, you
            use the following statement:

            -   ```sql
                SELECT random_between(1,100)
                FROM generate_series(1,5);
                ```

    -   **_EXPLAIN => to display the execution plan of a statement._**

        -   The EXPLAIN statement returns the execution plan which PostgreSQL planner
            generates for a given statement.

        -   The EXPLAIN shows how tables involved in a statement will be scanned
            by index scan or sequential scan, etc., and if multiple tables are used,
            what kind of join algorithm will be used.

        -   The most important and useful information that the EXPLAIN statement
            returns are start-cost before the first row can be returned and the total
            cost to return the complete result set.

        -   Syntax

            -   ```sql
                EXPLAIN [ ( option [, ...] ) ] sql_statement;
                ```

        -   where option can be one of the following:

            -   ANALYZE [ boolean ]
            -   VERBOSE [ boolean ]
            -   COSTS [ boolean ]
            -   BUFFERS [ boolean ]
            -   TIMING [ boolean ]
            -   SUMMARY [ boolean ]
            -   FORMAT { TEXT | XML | JSON | YAML }

        -   **ANALYZE**

            -   The ANALYZE option causes the sql_statement to be executed first
                and then actual run-time statistics in the returned information
                including total elapsed time expended within each plan node and the
                number of rows it actually returned.
            -   The ANALYZE statement actually executes the SQL statement and
                discards the output information, therefore, if you want to analyze
                any statement such as INSERT, UPDATE, or DELETE without affecting
                the data, you should wrap the EXPLAIN ANALYZE in a transaction, as
                follows:

                -   ```sql
                    BEGIN; EXPLAIN ANALYZE sql_statement; ROLLBACK;
                    ```

        -   **VERBOSE**

            -   The VERBOSE parameter allows you to show additional information
                regarding the plan. This parameter sets to FALSE by default.

        -   **COSTS**

            -   The COSTS option includes the estimated startup and total costs
                of each plan node, as well as the estimated number of rows and the
                estimated width of each row in the query plan. The COSTS defaults
                to TRUE.

        -   **BUFFERS**

            -   This parameter adds information to the buffer usage. BUFFERS only
                can be used when ANALYZE is enabled. By default, the BUFFERS
                parameter set to FALSE.

        -   **TIMING**

            -   This parameter includes the actual startup time and time spent in
                each node in the output. The TIMING defaults to TRUE and it may only
                be used when ANALYZE is enabled.

        -   **SUMMARY**

            -   The SUMMARY parameter adds summary information such as total
                timing after the query plan. Note that when ANALYZE option is used,
                the summary information is included by default.

        -   **FORMAT**
            -   Specify the output format of the query plan such as TEXT, XML,
                JSON, and YAML. This parameter is set to TEXT by default.
