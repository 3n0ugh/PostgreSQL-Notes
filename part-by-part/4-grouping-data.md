4.  # GROUPING DATA

    -   **_GROUP BY => to divide rows into groups._**

        -   Without An Aggregate Function Example (similar to SELECT DISTINCT)

            -   `SELECT customer_id FROM payment GROUP BY customer_id;`
            -   `SELECT DISTINCT customer_id FROM payment;`

        -   With SUM () Function Example

            -   `SELECT customer_id, SUM (amounts) AS amount FROM payment GROUP BY customer_id ORDER BY amount DESC;`

        -   Clause With The JOIN Clause

            -   `SELECT first_name || '*' || last_name full_name, SUM (amounts) amount FROM payment INNER JOIN customer USING (customer_id) GROUP BY full_name ORDER BY customer_id DESC;`

        -   With COUNT () Function Example

            -   ` SELECT staff_id, COUNT (payment_id) FROM payment GROUP BY staff_id;`

        -   With Multiple Columns

            -   `SELECT customer_id, staff_id, SUM(amount) FROM payment GROUP BY staff_id, customer_id ORDER BY customer_id;`

        -   With Date Column

            -   `SELECT DATE(payment_date) paid_date, SUM(amount) sum FROM payment GROUP BY DATE(payment_date);`

    -   **_HAVING => to specify a search condition for a group or an aggregate._**

        -   HAVING vs. WHERE

            -   The WHERE clause allows you to filter rows based on a specified
                condition. However, the HAVING clause allows you to filter groups
                of rows according to a specified condition.

        -   Clause With SUM () Function Example

            -   `SELECT customer_id, SUM (amount) FROM payment GROUP BY customer_id HAVING SUM (amount) > 200;`

        -   Clause With COUNT () Function Example

            -   `SELECT store_id, COUNT (customer_id) FROM customer GROUP BY store_id HAVING COUNT (customer_id) > 300;`
