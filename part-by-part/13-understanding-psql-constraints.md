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
