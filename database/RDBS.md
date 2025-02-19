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

## Indexing
### Index scan vs Table scan vs Bitmap Index scan
- Index scan: DB system will scan the index table, each time it finds the data -> it will fetch that data from heap
- Table scan: DB system will do the full heap scan
- Bitmap Index scan: DB system will create a bitmap and mark all the valid value in the bitmap -> it will fetch data base on bitmap from the heap. Case use multiple condtion, all bitmaps will merge using AND, OR operator to find the valid value
### Key vs Non-Key column Database Indexing
- If you include the columns inside the index key and you only select those columns, DB system will only do the index scan and get all the data you need
- If you include to many columns inside the index key -> The size of the data is bigger -> You have to fetch more page to read the data -> Make index slower
### Index scan vs Index Only Scan
### Combining Indexes
- When you use combining indexes (Ex: col a and b) only the left most col is used in index scan (col a) or query condition using AND
### Type of indexing
- Primary Key Index
- Unique Index
- Clustered Index: Data will be ordered
- Nonclustered Index
- Text Index
- Composite Index: Combine multiple column in 1 index

## B-Tree

## Database Partitioning
- Is a technique splitting the table smaller, all smaller tables are in the same DB
- Create mutiple tables in 1 database and attach it as partition of 1 database. The DB system will manage and handle all the splitting
- Horizontal Partitioning: split by row
- Vertical Partitioning: split by col
- Type of partitioning:
  - By range: dates, ids
  - By list: discrete values (country, ...) or zip codes
  - By hash:
- Pros:
  - Improve query performance when accessing a single partition
  - Easy bulk loading (because when you attach partition to the table -> all insert data will be insert to partition) 
- Cons:
  - Update that move rows from 1 partition to another -> slow
  - Scan all partitions -> slower performance than scan the table
  - Hard to change schema -> change 1 row make all the partitions change

## Database Sharding
- Is a technique spliting the table into multiple databases
- Create multiple tables across multiple DBs with the same schema and client have a mechanic(consistent hashing) to determine where the data belong to
- Pros:
  - Scalability
  - Optimal and smaller index size
  - Security (client access control)
- Cons:
  - Complex client(need a machenic to determine where the data belong to)
  - Transactions across shards
  - Rollbacks
  - Schema changes are hard
  - Join
  - Has to know the key to determine where to query
 
## Database Replication
- Master/Standby Replication:
  - 1 master node that accepts writes/ddls
  - 1 or more backup nodes that receive those writes from the master
  - Simple to implement, no conflicts
  - Replicas are connected with the master using TCP
- Multi-master Replication
  - Multiple Master node that accepts writes
  - Need to resolves conflict
  - 1 or more backup nodes that receive those writes from the masters
- Synchronous and Asynchronous Replication:
  - Sync Replication: A write transaction to the master will be blocked until it is written to the backup nodes
  - ASync Replication: A write transaction is considered successful if it written to the master, then the write is applied to the backup nodes async
