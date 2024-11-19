# Database Prep
## Transactions
### ACID
- Atomicity
  - All queris in a transaction must succeed
  - If 1 query fail => all queries should rollback
  - If db went down before a transaction is committed, all queries should rollback
- Isolation
  - Read phenomena
    - Dirty Read: Read data HAVEN'T BEEN COMMITTED by other transactions
    - Non-repeatable Read: Read data using 1 query but the 2nd query the data is UPDATED -> data is different. But you want the results of 2 queries is the same (Ex: `Select * From transactions` to get the list of transactions then `Select Sum(transactions.amount) From transactions` to get to total amount)
    - Phantom Read: The same as Non-repeatable Read but in the 2nd query. You read data have been NEWLY INSERTED
    - Lost Updates: Try to read uncommitted data but the data is modified by other transactions
  - Isolation Level:
    - Read Uncommitted: No isolation, READ EVERYTHING
    - Read Committed: Only see commited changes by other transcations
    - Repeatable Read: The tranasction make sure data you read inside the tranctions is unchanged BY LOCKING THE DATA while it is running -> Other transaction can't update the data unless the transaction release the lock. Newly added data can still be read
    - Snapshot: Before the start of the transaction, the database take a snapshot and all queries inside the transaction only see data in the snapshot -> Other transaction can modified the data freely
    - Serializable: Transactions will run one after another. Not at the same time
  - Implementation:
    - Pessimistic: Row level locks, Table level locks, Page level locks to avoid lost update
    - Optimistic: No locks but when conflict happen -> Fail the transactions
    - Serializable
    - Non Repeatable:
- Consistency
  - Consistency in Data
  - Consistency in Read
- Durability
  - Changes made by a transaction must be persisted in a storage
  - Techniques:
    - WAL (Write Ahead Log)
    - Async snapshot
    - AOF (Apend Only File)
    - OS Cache

## Database Internal
### Table
### Row ID
- DB system use their own ID (Row ID) to maintain internally
- Some DB (MySQL, innoDB) use Primary Key as Row ID but Other like Postgres have their own system Row ID
### Page
- Rows are stored in and read from pages -> Each time you read data from DB, the DB get the page that contain that data -> When you request a single row, you get get a lot more rows in that request
- Each page have a size
- The larger the data you get, the more pages are gotten
