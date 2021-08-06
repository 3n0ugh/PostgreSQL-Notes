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
