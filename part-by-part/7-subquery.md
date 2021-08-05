7.  # SUBQUERY

    -   **_SUBQUERY => that allows you to construct complex queries._**

        -   With IN EXAMPLE

            -   `SELECT film_id, title FROM film WHERE film_id IN ( SELECT inventory.inventory_id FROM rental INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id WHERE return_date BETWEEN '2005-05-29' AND '2005-05-30');`

        -   With EXISTS Operator

            -   `SELECT first_name, last_name FROM customer WHERE EXISTS ( SELECT 1 FROM payment WHERE payment.customer_id = customer.customer_id );`

    -   **_ANY OPERATOR => to compare a scalar value with a set of values returned by a subquery._**

        -   The subquery must return exactly one column.

        -   The ANY operator must be preceded by one of the following comparison
            operator =, <=, >, <, > and <>.

        -   The ANY operator returns true if any value of the subquery meets the
            condition, otherwise, it returns false.

        -   SOME is synonym for ANY.

        -   The = ANY is equivalent to IN operator.

            -   `SELECT title, category_id FROM film INNER JOIN film_category USING (film_id) WHERE category_id = ANY ( SELECT category_id FROM category WHERE NAME = 'Action' OR NAME = 'Drama' );`
            -   `SELECT title, category_id FROM film INNER JOIN film_category USING (film_id) WHERE category_id IN ( SELECT category_id FROM category WHERE NAME = 'Action' OR NAME = 'Drama' );`

        -   The <> ANY operator is different from NOT IN.

            -   `x <> ANY (a, b, c)`
            -   `x <> a OR x <> b OR x <> c`

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

                - `SELECT film_id, title, length FROM film WHERE length > ALL ( SELECT ROUND (AVG (length),2) FROM film GROUP BY rating ) ORDER BY length;`

    -   **_EXISTS => to test for existence of rows in a subquery._**

        -   Example

            -   `SELECT first_name, last_name FROM customer c WHERE EXISTS (SELECT 1 FROM payment p WHERE p.customer_id = c.customer_id AND amount > 11 ) ORDER BY first_name, last_name;`

        -   NOT EXISTS

            -   `SELECT first_name, last_name FROM customer c WHERE NOT EXISTS (SELECT 1 FROM payment p WHERE p.customer_id = c.customer_id AND amount > 11 ) ORDER BY first_name, last_name;`

        -   NULL

            -   `SELECT first_name, last_name FROM customer WHERE EXISTS( SELECT NULL ) ORDER BY first_name, last_name;`
