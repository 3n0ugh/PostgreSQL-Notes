16. # PSQL COMMANDS

    1. **_Connect To PostgreSQL Database_**

        - psql -d database -U user -W

        - If you want to connect to a database that resides on another host,
          you add the -h option as follows:

            - psql -h host -d database -U user -W

        - In case you want to use SSL mode for the connection, just specify it as
          shown in the following command:

            - psql -U user -h host "dbname=db sslmode=require"

    2. **_Switch Connection To A New Database_**

        - \c dbname username

    3. **_List Available Databases_**

        - \l

    4. **_List Available Tables_**

        - \dt

    5. **_Describe A Table_**

        - \d table_name

    6. **_List Available Schema_**

        - \dn

    7. **_List Available Functions_**

        - \df

    8. **_List Available Views_**

        - \dv

    9. **_List Users And Their Roles_**

        - \du

    10. **_Execute The Previous Command_**

        - \g

    11. **_Command History_**

        - \s

        - To save command history to a file.
            - \s file_name

    12. **_Executes Psql Commands From A File_**

        - \i file_name

    13. **_Get Help On Psql Commands_**

        - \?

        - To get help on specific PostgreSQL statement:
            - \h statement

    14. **_Turn On Query Execution Time_**

        - \timing

    15. **_Edit Command In Your Own Editor_**

        - \e

        - To editing a function
            - \ef [function name]

    16. **_Switch Output Options_**

        - \a command switches from aligned to non-aligned column output.

        - \H command formats the output to HTML format.

    17. **_Quit Psql_**

        - \q
