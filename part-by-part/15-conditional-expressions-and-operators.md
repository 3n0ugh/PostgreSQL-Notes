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
