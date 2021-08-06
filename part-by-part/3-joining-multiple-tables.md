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
