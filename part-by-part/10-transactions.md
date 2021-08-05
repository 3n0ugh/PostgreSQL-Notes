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

            -   `BEGIN;`

        -   Commit A Transaction

            -   `COMMIT;`

        -   Rolling Back A Transaction

            -   `ROLLBACK;`

        -   Bank Account Example

            -   `-- start a transaction BEGIN;`

                `-- deduct 1000 from account 1 UPDATE accounts SET balance = balance - 1000 WHERE id = 1;`

                `-- add 1000 to account 2 UPDATE accounts SET balance = balance + 1000 WHERE id = 2;`

                ` -- select the data from accounts SELECT id, name, balance FROM accounts;`

                `-- commit the transaction / -- or roll back the transaction COMMIT; / ROLLBACK;`
