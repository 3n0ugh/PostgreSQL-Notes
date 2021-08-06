10. # TRANSACTIONS

    -   **_TRANSACTION => transactions using the BEGIN, COMMIT, and ROLLBACK statements._**

        -   A database transaction is a single unit of work that consists of one or more operations.

        -   A PostgreSQL transaction is atomic, consistent, isolated, and durable.
            These properties are often referred to as ACID:

            -   Atomicity guarantees that the transaction completes in an
                all-or-nothing manner.
            -   Consistency ensures the change to data written to the database
                must be valid and follow predefined rules.
            -   Isolation determines how transaction integrity is visible to
                other transactions.
            -   Durability makes sure that transactions that have been committed
                will be stored in the database permanently.

        -   To Start A Transaction

            -   ```sql
                BEGIN;
                ```

        -   Commit A Transaction

            -   ```sql
                COMMIT;
                ```

        -   Rolling Back A Transaction

            -   ```sql
                ROLLBACK;
                ```

        -   Bank Account Example

            -   ```sql
                    -- start a transaction
                    BEGIN;
                ```

                ```sql
                    -- deduct 1000 from account 1
                    UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
                ```

                ```sql
                    -- add 1000 to account 2
                    UPDATE accounts SET balance = balance + 1000 WHERE id = 2;
                ```

                ```sql
                    -- select the data from accounts
                    SELECT id, name, balance FROM accounts;
                ```

                ```sql
                    -- commit the transaction / -- or roll back the transaction
                    COMMIT; / ROLLBACK;
                ```
