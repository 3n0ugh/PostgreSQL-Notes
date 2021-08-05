11. # IMPORT & EXPORT DATA (FROM/TO CSV)

    -   **_IMPORT CSV FILE INTO PostgreSQL TABLE_**

        -   Example

            -   `COPY persons(first_name, last_name, dob, email) FROM 'C:\sampledb\persons.csv' DELIMITER ',' CSV HEADER;`
            -   If u haven't permission to read that csv file, use \copy instead of COPY.

    -   **_EXPORT PostgreSQL TABLE TO CSV FILE_**

        -   Example

            -   `COPY film TO '~/sampleDB/film_db.csv' DELIMITER ',' CSV HEADER;`
            -   If u haven't permission to read that csv file, use \copy instead of COPY.

        -   Another Example Of Usage \copy Command

            -   `\copy (SELECT * FROM film) to '~/sampleDB/film_db.csv' with csv`
