6.  # GROUPING SETS, CUBE AND ROLLUP

    -   **_GROUPING SETS => to generate multiple grouping sets in a query._**

        -   The Outputs Of The Following Two Clauses Are The Same.

            -   `SELECT brand, segment, SUM (quantity) FROM sales GROUP BY GROUPING SETS ( (brand, segment), (brand), (segment), () );`
            -   `SELECT brand, segment, SUM (quantity) FROM sales GROUP BY brand, segment UNION ALL SELECT brand, NULL, SUM (quantity) FROM sales GROUP BY brand UNION ALL SELECT NULL, segment, SUM (quantity) FROM sales GROUP BY segment UNION ALL SELECT NULL, NULL, SUM (quantity) FROM sales;`

    -   **_CUBE => to generate multiple grouping sets. (with all combinations)_**

        -   Given Same Output as GROUPING SETS's Examples.

            -   `SELECT brand, segment, SUM (quantity) quantity FROM sales GROUP BY CUBE (brand, segment);`

    -   **_ROLLUP => to generate multiple grouping sets. (it just makes subset of those.)_**

        -   Example

            -   `SELECT brand, segment, SUM (quantity) FROM sales GROUP BY ROLLUP (brand, segment) ORDER BY segment, brand;`
