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
- Row-Oriented DB
  - Data are stored as rows in disk -> Once you find the data, you get all the columns data
  - More I/O are required because data is big -> The more pages need to find -> More expensive operation
  - Query COUNT, SUM, ... is not efficient
- Col-Oriented DB
  - Data are stored ad colummn in disk
  - Less I/O are required because data is smaller -> Less pages need to find. But if you want to get other columns data -> More I/O are requried
  - Writing data is slower because the structure is complex
  - Only get what you need -> Query more efficient. But case you need to work with multiple column of a row -> Not good
### Row ID
- DB system use their own ID (Row ID) to maintain internally
- Some DB (MySQL, innoDB) use Primary Key as Row ID but Other like Postgres have their own system Row ID
### Page
- Rows are stored in and read from pages -> Each time you read data from DB, the DB get the page that contain that data -> When you request a single row, you get get a lot more rows in that request
- Each page have a size
- The larger the data you get, the more pages are gotten
### I/O
- I/O are read requests to the disk
- 1 I/O can get 1 or multiple pages
- I/O are expensives operations so you you want to minimize this operation as much as possible.
- Some I/O go to OS cace and not disk
### Heap
- Heap is a data structure where the table and all its pages are stored -> Query the full heap is expensive
- Index will help us which part of the heap, which page(s) to find
### Index
- An index is a data structure seperate from the heap, that has "pointers" to the heap -> It is store in another place and its data is references to the heap data
- The index is also stored as pages and cose I/O to get the index
- When you search a data using the index, it will get the page(s) have the index data then go to the heap to fetch the data. The index have the exact location of the data inside the heap -> You dont need to scan the full heap, you just need to get the exact page(s) that have the data
- You can index 1 or more columns
- The smaller the index. The faster the search 
