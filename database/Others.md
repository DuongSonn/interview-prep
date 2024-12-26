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

# RabbitMQ
- Components: Producer, Queue, Cosumer, Exchange
  - Producer: user that sends message
  - Queue: buffer store message
  - Consumer: user that receives message
  - Exchange: receive message from Producer and send it to Queue. It decides what to do with the messages 
- Work Queues:
  - Round-Robin dispatching: By default, RabbitMQ will send each message to the next consumer, in sequence -> All consumer will get the same number of messages
  - Message Acknowledgemnt: By default, RabbitMQ delivers mesage to a consumer then it will delete the message -> There will be case consumer die, it will lead to message lost. An acknowlegment is sent by the consumer to tell rabbitMQ the message has been received, then RabbitMQ will delete the message. Case the consumer die, RabbitMQ will requeue the message and send it to another consumber
  - Message durability: By default, RabbitMQ crash it will forget all the queues and messages -> You need to tell RabbitMQ to save both queue and messages. But there is still a chance a message is lost before it is save to disk (use pulisher confirms to solve this)
  - Fair Dispatch: By default, RabbitMQ dispatch the messages evenly. There are cases when message needs a lot of work and message doesn't need a lot of work -> 1 consumer have to do a lot of work, and others is free. You can config so that a consumber doesn't take more than 1 message at a time, doesn't receive acknowlege = not ready. But this will cause the queue to fill up and lead to message lost or rejected if all consumers are busy.
- Publish/Subcribe:
  - Fanout: exchange broadcasts all the messages it receives to all the queues
  - Direct: exchange send the message to a specific queues it bind with. If exchange binds with multiple queues, it sends the message to all the queues
  - Topic: is like direct exchange but you can use pattern for the queues
  - Headers:

# Kafka
