Postgres Reference

- [1. Select](#1-select)
  - [1.1. WHERE](#11-where)
  - [1.2. JOIN](#12-join)
  - [1.3. GROUP BY](#13-group-by)
  - [1.4. HAVING](#14-having)
  - [1.5. LIMIT & OFFSET](#15-limit--offset)
  - [1.6. UNION, INTERSECT & EXCEPT](#16-union-intersect--except)
  - [1.7. SUB QUERIES](#17-sub-queries)
  - [1.8. GREATEST](#18-greatest)
  - [1.9. LEAST](#19-least)
- [2. Insert](#2-insert)
  - [2.1. COALECE](#21-coalece)
- [3. Create](#3-create)
  - [3.1. CHECK](#31-check)
- [4. General](#4-general)
  - [4.1. Common Table Expressions](#41-common-table-expressions)
    - [4.1.1. CTE](#411-cte)
    - [4.1.2. Recurssive CTE](#412-recurssive-cte)
  - [4.2. Views](#42-views)
    - [4.2.1. Normal View](#421-normal-view)
    - [4.2.2. Materialized View](#422-materialized-view)
  - [4.3. Transaction](#43-transaction)
    - [4.3.1. BEGIN](#431-begin)
    - [4.3.2. COMMIT](#432-commit)
    - [4.3.3. ROLLBACK](#433-rollback)
  - [Operations](#operations)
  - [4.4. COST](#44-cost)
    - [Numbers in Explain](#numbers-in-explain)
    - [Cost Estimation](#cost-estimation)

# 1. Select

## 1.1. WHERE

Syntax 

    SELECT 
        table.column1, table.column2, ...
    FROM
        table
    WHERE
        condition;

    condition = column operator value

| Operator | Implication                                                                 |
| -------- | --------------------------------------------------------------------------- |
| =        | Equal                                                                       |
| >        | Greater than                                                                |
| <        | Less than                                                                   |
| >=       | Greater than or equal                                                       |
| <=       | Less than or equal                                                          |
| <>       | Not equal. Note: In some versions of SQL this operator may be written as != |
| BETWEEN  | Between a certain range                                                     |
| LIKE     | Search for a pattern                                                        |
| IN       | To specify multiple possible values for a column                            |

## 1.2. JOIN

Syntax

    SELECT
        table1.column1, ..., table2.column1, ...
    FROM table1
    (INNER | LEFT | RIGHT | FULL) JOIN table2 ON table1.column = table2.column;

## 1.3. GROUP BY

Syntax 

    SELECT
        column1,
        column2,
        AGGREGATE_FUNCTION (column3)
    FROM
        table1
    GROUP BY
        column1,
        column2;

| Aggregate Function | Implication              |
| ------------------ | ------------------------ |
| COUNT              | Count of rows            |
| AVG                | Average of all row value |
| MAX                | Max of all row value     |
| MIN                | Min of all row value     |
| SUM                | Sum of all row value     |

## 1.4. HAVING

Syntax
    SELECT
        column1,
        column2,
        AGGREGATE_FUNCTION (column3)
    FROM
        table1
    GROUP BY
        column1,
        column2
    HAVING
        group_condition;

    group_condition = column operator value

| Operator | Implication                                                                 |
| -------- | --------------------------------------------------------------------------- |
| =        | Equal                                                                       |
| >        | Greater than                                                                |
| <        | Less than                                                                   |
| >=       | Greater than or equal                                                       |
| <=       | Less than or equal                                                          |
| <>       | Not equal. Note: In some versions of SQL this operator may be written as != |
| BETWEEN  | Between a certain range                                                     |
| LIKE     | Search for a pattern                                                        |
| IN       | To specify multiple possible values for a column                            |

## 1.5. LIMIT & OFFSET

Syntax
    SELECT 
        column_list
    FROM
        table1
    ORDER BY column_list
    LIMIT row_count OFFSET offset;

## 1.6. UNION, INTERSECT & EXCEPT

Syntax
    SELECT 
        table1.column1, table1.column2
    FROM
        table1 
    UNION | INTERSECT | EXCEPT
    SELECT 
        table2.column1, table2.column2
    FROM
        table2;

*Both select must have same number of columns & same datatypes, and it will be aligned regardless of their name

| Operation | Implication                         |
| --------- | ----------------------------------- |
| union     | combines both set of values         |
| intersect | selects only common values          |
| except    | selects only exclusive to lhs table |


## 1.7. SUB QUERIES

Syntax
    SELECT 
        table1.column1, table1.column2, (SCALAR SUB QUERY) as alias
    FROM
        (TABLE SUBQUERY) AS alias
    JOIN
        (TABLE SUBQUERY) AS alias on join_condition
    WHERE
        column == (SCALAR SUB QUERY)
        column IN (SINGLE COLUMN, MULTIROW SUB QUERY)

## 1.8. GREATEST
    GREATEST(val1, val2, ...)

## 1.9. LEAST
    LEAST(val1, val2, ...)

# 2. Insert

## 2.1. COALECE

Syntax 
    COALESCE (argument_1, argument_2, …);

# 3. Create

## 3.1. CHECK

Syntax
    CHECK (constraint)

# 4. General

## 4.1. Common Table Expressions

### 4.1.1. CTE
    WITH cte_name AS(
        CTE_query_definition 
    ) SELECT * FROM cte_name;

### 4.1.2. Recurssive CTE
    WITH RECURSIVE cte_name (col...) AS(
        CTE_query_definition -- non-recursive term
        UNION [ALL]
        CTE_query definion  -- recursive term
    ) SELECT * FROM cte_name;

## 4.2. Views

### 4.2.1. Normal View
- Similar to cte but globally available

### 4.2.2. Materialized View
- Cache of a query result which has to be force updated

## 4.3. Transaction

### 4.3.1. BEGIN
- Starts a transaction, like a virtual space for a connection.
- Main data will remain old after queries executed here until commit.
  
### 4.3.2. COMMIT
- Commits transaction changes after begin to main data 

### 4.3.3. ROLLBACK
- Reverts all changes after begin and main data remain unaffected.
- Use Rollback when transaction is in aborted state.

## Operations

- Index Scan
- Hash
- Sequential scan
- Hash Join

## 4.4. COST

[Postgres query planning](https://www.postgresql.org/docs/13/runtime-config-query.html)

Use EXPLAIN ANALYZE query to get the planning information.

### Numbers in Explain
*eg1: cost=0.00..123.14*
- first result / start time(first row) ".." total cost calculation time 

*eg2: rows=123*
- total number of rows estimated

*eg3: width=12*
- total column (bytes) estimated.

### Cost Estimation

- (#rand_page) * rand_page_cost (4.0) +
- (#seq_page) * seq_page_cost (1.0) +
- (#row) * cpu_tuple_cost (0.01) +
- (#index_row) * cpu_index_tuple_cost (0.005) +
- (#times func/op evaluated) * cpu_operator_cost (0.0025) 



psql commands [reference](https://www.postgresqltutorial.com/psql-commands/)