# Redis
- Redis is an in-memory DB -> Dont need I/O operations to read/write data
- It stores data in RAM -> Extremely write and read access but have limited space
- Data in redis are stored in {key:value} pair
- Persistence
  - Snapshot
  - AOF(Append Only File): Every time a redis server receive a cmd, it will write to a log file -> When you restart the server, redis will run the log again
  - Use replica
- Data Structure: Array, Hash Map, String, Sets, Sorted Sets
- Usecases
  - Cache: Reduce DB load, response API faster
  - Session: Store user session
  - Distrubuted Lock: Case multiple client want to access the same resources
  - Rate Limit:
  - Ranking/Leader Boards: Use sorted sets to order the score of a user 

# Rabbitmq

# Kafka
