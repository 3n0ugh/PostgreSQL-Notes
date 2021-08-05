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

        -   ANALYZE

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

        -   VERBOSE

            -   The VERBOSE parameter allows you to show additional information
                regarding the plan. This parameter sets to FALSE by default.

        -   COSTS

            -   The COSTS option includes the estimated startup and total costs
                of each plan node, as well as the estimated number of rows and the
                estimated width of each row in the query plan. The COSTS defaults
                to TRUE.

        -   BUFFERS

            -   This parameter adds information to the buffer usage. BUFFERS only
                can be used when ANALYZE is enabled. By default, the BUFFERS
                parameter set to FALSE.

        -   TIMING

            -   This parameter includes the actual startup time and time spent in
                each node in the output. The TIMING defaults to TRUE and it may only
                be used when ANALYZE is enabled.

        -   SUMMARY

            -   The SUMMARY parameter adds summary information such as total
                timing after the query plan. Note that when ANALYZE option is used,
                the summary information is included by default.

        -   FORMAT
            -   Specify the output format of the query plan such as TEXT, XML,
                JSON, and YAML. This parameter is set to TEXT by default.
