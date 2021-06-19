- [1. ACID Properties](#1-acid-properties)
  - [1.1. Transaction](#11-transaction)
    - [Commit](#commit)
    - [Rollback](#rollback)
  - [1.2. Atomicity](#12-atomicity)
  - [1.3. Isolation](#13-isolation)
    - [1.3.1. Read phenomena](#131-read-phenomena)
    - [1.3.2. Isolation Levels](#132-isolation-levels)
  - [1.4. Consistency](#14-consistency)
  - [1.5. Durability](#15-durability)


# 1. ACID Properties

## 1.1. Transaction
- A block of steps considered as a single unit of work.
- Represents any change in the state of a database.

```
BEGIN [ WORK | TRANSACTION ] [ transaction_mode [, ...] ]

where transaction_mode is one of:

    ISOLATION LEVEL { SERIALIZABLE | REPEATABLE READ | READ COMMITTED | READ UNCOMMITTED }

    READ WRITE | READ ONLY

    [ NOT ] DEFERRABLE

COMMIT/ROLLBACK;
```

### Commit 
- Commit to database, or apply the changes.
### Rollback
- Revert db to db state before transaction and close the transaction.

## 1.2. Atomicity
- Transactions must be *completely done* to *update the state* of database
- If fails (crash, os shutdown...) the previous state will persist.
- _All steps in transaction must succeed, if one fails, rollback._

## 1.3. Isolation
- Visibility of changes of db state between transaction.
- _Can current transactions see changes made by other transaction_

```
BEGIN ISOLATION LEVEL { SERIALIZABLE | REPEATABLE READ | READ COMMITTED | READ UNCOMMITTED };

Q1
Q2
...

COMMIT;
```

### 1.3.1. Read phenomena
| Phenomena           | Explaination                                             |
| ------------------- | -------------------------------------------------------- |
| Dirty Read          | Reading uncommited data (of another running transaction) |
| Lost Update         | Transactions writing different data on same row          |
| Non-Repeatable Read | Reading same row twice and getting different result      |
| Phantom Read        | Same query resulting in different set of rows            |

### 1.3.2. Isolation Levels
- Read Uncommitted – Everything is readable.
- Read Committed – r/w lock on row, when a transaction is updating a row.
  - If T1 is updating _"update tab set x = 10 where a == b;"_. T2 can't read rows where a == b before the operation is complete
- Repeatable Read – r lock on reading rows, w lock on writing rows. (Equivalent to versioning rows or OCC).
- Serializable – No concurrent transaction, execute another only after current is over. (Equivalent to full lock or PCC)

[Detail Reference](https://www.geeksforgeeks.org/transaction-isolation-levels-dbms/)

## 1.4. Consistency
- Correctness of the database.
- **Atomicity \& Isolation** results in consistancy.

- Consistency in data
  - User defined accuracy, Foreign \& Primary keys, Atomic and Isolated transactions.

eg: Visitor count a table with total visis updated concurrently. lost updates is not a problem but need performance.
- Consistency in reads
  - Will new transaction see immediate updates from another 
  - Eventual consistancy happens when horizontal scaling.

## 1.5. Durability
- Persistance of commited transaction in non-volatile storage.
- In-memory caches are non-durable.


