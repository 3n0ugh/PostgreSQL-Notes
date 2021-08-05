1.  # QUERYING DATA

    -   **_ SELECT => that retrieves data from a single table _**

        -   Single Columns

            -   `SELECT user_id FROM accounts;`

        -   Multiple Columns

            -   `SELECT uname, pass FROM accounts;`

        -   All Columns

            -   `SELECT * FROM accounts;`

    -   **_ Column Alias => A column alias allows you to assign a column or an expression in the select list of a SELECT statement a temporary name. _**

        -   EXAMPLE

            -   `SELECT username AS isim FROM accounts;`

        -   || ( concatenation operator )

            -   `SELECT username || ' ' || lastname FROM accounts;`
            -   `SELECT username || ' ' || lastname AS fullname FROM accounts;`

        -   Contain Space

            -   `SELECT fullname AS "uname lname" FROM accounts;`

    -   **_ ORDER BY => To sort the rows of the result set, you use the ORDER BY clause in the SELECT statement. ( two options as ASC and DESC (default ASC) )_**

        -   Sort Rows By One Column

            -   `SELECT username, lastname FROM accounts ORDER BY username ASC;`

        -   Sort Rows By Multiple Columns

            -   `SELECT username, lastname FROM accounts ORDER BY username ASC, lastname DESC;`

        -   Sort Rows By Expressions

            -   `SELECT username, LENGTH(username) AS len FROM accounts ORDER BY len DESC;`

        -   NULLS FIRST/LAST

            -   `SELECT username FROM accounts ORDER BY DESC NULLS LAST;`

    -   **_ SELECT DISTINCT => The DISTINCT clause is used in the SELECT statement to remove duplicate rows from a result set. The DISTINCT clause keeps one row for each group of duplicates. The can be to one or more columns in the select list of the SELECT statement._**

        -   One Column

            -   `SELECT DISTINCT bcolor FROM distinct_demo ORDER BY bcolor;`

        -   Multiple Column

            -   `SELECT DISTINCT bcolor, fcolor FROM distinct_demo ORDER BY bcolor, fcolor;`

        -   DISTINCT ON

            -   The following statement sorts the result set by the bcolor and fcolor,
                and then for each group of duplicates, it keeps the first row in the
                returned result set.
            -   `SELECT DISTINCT ON(bcolor) bcolor, fcolor FROM distinct_demo ORDER BY bcolor, fcolor;`
