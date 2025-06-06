# DATABASE INTERVIEW ANSWERS
1. Kỹ thuật để chống race condition khi có 2 request đến cùng lúc
- Redis lock
- Database transaction
2. Giả sử có 1k message trong RabbitMQ, làm thế nào để tăng tốc độ cao xử lý để tránh tràn queue
- Tăng consumer (sử dụng routine, chạy nhiều instance)
- Sử lý tin nhắn theo kiểu batch 
3. So sánh Mysql với postgres
