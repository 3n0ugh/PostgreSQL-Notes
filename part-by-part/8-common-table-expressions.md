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
