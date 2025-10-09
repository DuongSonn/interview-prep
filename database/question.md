# DATABASE INTERVIEW ANSWERS
1. Kỹ thuật để chống race condition khi có 2 request đến cùng lúc
  - Redis lock
  - Database transaction
  - Unique Index
2. Giả sử có 1k message trong RabbitMQ, làm thế nào để tăng tốc độ cao xử lý để tránh tràn queue
  - Tăng consumer (sử dụng routine, chạy nhiều instance)
  - Sử lý tin nhắn theo kiểu batch 
3. So sánh Mysql với postgres
  - Postgres tối ưu cho việc đọc phức tạp, update thì postgres sẽ chậm hơn 1 chút do cơ chế đánh index của postgres khá lằng nhằng. Index mặc định của postgres là non-clustered index
  - Mysql tối ưu cho việc đọc/ghi đơn giản, update thì mysql sẽ nhanh hơn chút. Index mặc định của postgres là clustered index
4. Tại sao Foreign Key có thể ảnh hưởng đến tốc độ truy vấn của query
  - Khi bảng cha thực hiện update/delete có liên quan đến foreign key thì bảng cha sẽ lock bảng con lại -> Trong thời gian này có query ở bảng con sẽ phải đợi query bảng cha hoàn thành
  - Khi thực hiện insert/update/delete trên bảng cha hoặc con, DB sẽ phải kiểm tra rằng buộc để đẩm bảo toàn vẹn dữ liệu, TH xấu có thể kiểm tra toàn bộ bảng con.
  - MySQL không cho phép tạo quan hệ cha-con khi sử dụng partition -> Nếu đã sử dụng FK thì phải tái cấu trúc bảng
5. Các vấn đề cần chú ý khi sử dụng cache là gì?
  - Chỉ dùng cache khi cần thiết
  - Chiến lược expire / TTL (Time-To-Live)
Rất quan trọng: TTL giúp tránh dữ liệu stale, nhưng chọn TTL quá ngắn → tốn cache miss.
Có thể dùng randomized TTL (ví dụ TTL = 300 ± rand(30)) để tránh cache stampede khi nhiều key hết hạn cùng lúc.
  - Đồng bộ cache – DB (Cache Consistency)
3 chiến lược phổ biến:
Cache-aside (lazy loading): app đọc cache, nếu miss → đọc DB → ghi cache. Dễ triển khai, nhưng dễ stale.
Write-through: ghi DB + cache cùng lúc → consistency tốt hơn, write chậm hơn.
Write-behind: ghi cache trước, flush về DB sau → nhanh nhưng dễ mất đồng bộ khi crash.
  - Single Point of Failure / Cache Cluster
Chuẩn: nếu chỉ dùng 1 Redis → khi node chết hoặc restart, cache miss spike làm tăng tải DB → cache avalanche.
Giải pháp: replication, sharding, hoặc cluster mode.
  - Cache Eviction Policy (LRU, LFU, FIFO, …)
Chính xác: khi cache đầy, policy ảnh hưởng đến hit rate. Redis mặc định volatile-lru, nhưng có nhiều lựa chọn (allkeys-lfu, volatile-ttl...).
  - Cache Stampede / Thundering Herd
Khi nhiều request cùng query 1 key vừa hết hạn → tất cả cùng truy vấn DB → DB overload.
Giải pháp: Random TTL, Mutex/lock key để chỉ 1 request rebuild cache, các request khác chờ, Pre-warming hoặc refresh trước khi TTL hết.
