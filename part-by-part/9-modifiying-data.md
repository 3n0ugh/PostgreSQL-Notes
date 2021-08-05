9.  # MODIFYING DATA

    -   **_INSERT => to insert a new row into a table._**

        -   RETURNING Clause

            -   RETURNING clause that returns the information of the inserted row.
            -   `INSERT INTO accounts (name, lastname, password) VALUES ('melek', 'yildiz', 'qwEr123!') RETURNING *;`

        -   Inserting A Single Row Into A Table

            -   `INSERT INTO accounts (name, lastname, password) VALUES ('melek', 'yildiz', 'qwEr123!');`

        -   Inserting Character String That Contains A Single Quote

            -   `INSERT INTO links (url, name) VALUES('http://www.oreilly.com','O''Reilly Media');`

        -   Inserting A Date value

            -   `INSERT INTO links (url, name, last_update) VALUES('https://www.google.com', 'Google', '2013-06-01'); -- YYYY-MM-DD`

        -   Getting The Last Insert Id

            -   `INSERT INTO links (url, name) VALUES('http://www.postgresql.org','PostgreSQL') RETURNING id;`

    -   **_INSERT MULTIPLE ROWS => to insert multiple rows into a table._**

        -   Example

            -   `INSERT INTO links (url, name) VALUES ('https://www.google.com', 'google'), ('https://www.github.com', 'github');`

        -   Inserting Multiple Rows And Returning Inserted Rows

            -   `INSERT INTO links (url, name) VALUES ('https://www.google.com', 'google'), ('https://www.github.com', 'github') RETURNING *;`

    -   **_UPDATE => to update existing data in a table._**

        -   Updating One Row

            -   `UPDATE courses SET published_date = '2020-08-01' WHERE course_id = 3;`

        -   Updating A Row And Returning The Updated Row

            -   `UPDATE courses SET published_date = '2020-08-01' WHERE course_id = 3 RETURNING *;`

    -   **_UPDATE JOIN => to update data in a table based on values in another table._**

        -   Example

            -   `UPDATE product SET net_price = price - price \* discount FROM product_segment WHERE product.segment_id = product_segment.id;`

    -   **_DELETE => to delete data from a table._**

        -   To Delete One Row From The Table

            -   `DELETE FROM links WHERE id = 8;`

        -   Delete A Row And Return The Deleted Row

            -   `DELETE FROM links WHERE id = 8 RETURNING *;`

        -   Delete Multiple Rows From The Table

            -   `DELETE FROM links WHERE id IN (5,7) RETURNING *;`

        -   Delete All Rows From The Table
            -   `DELETE FROM links;`

    -   **_UPSERT => to insert or update data if the row that is being inserted already exists in the table._**

        -   ON CONFLICT

            -   `INSERT INTO customers (name, email) VALUES('Microsoft','hotline@microsoft.com') ON CONFLICT (name) DO NOTHING;`
            -   `INSERT INTO customers (name, email) VALUES('Microsoft','hotline@microsoft.com') ON CONFLICT (name) DO UPDATE SET email = EXCLUDED.email || ';' || customers.email;`
