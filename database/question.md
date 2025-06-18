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
5.
