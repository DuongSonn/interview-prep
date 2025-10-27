1. Bạn sẽ giải quyết bài toán distributed transaction này thế nào? Bạn có biết về Saga pattern hoặc Outbox pattern không, và có thể mô tả cách áp dụng chúng trong bài toán này?
  - Saga pattern (Distributed Transaction bằng Compensation)
Cách làm: chia giao dịch lớn (trải qua nhiều service) thành chuỗi local transactions.
Mỗi local transaction có 2 phần:
Action (ví dụ: trừ tiền ví, cộng tiền ví, ghi log).
Compensation (ngược lại: cộng lại số tiền, huỷ log, gửi event rollback).
Nếu 1 bước fail, bạn chạy compensation cho các bước trước đó để đưa hệ thống về trạng thái nhất quán.
  - Vấn đề: khi bạn update DB xong rồi publish message sang broker, có thể xảy ra: update thành công nhưng publish fail → dữ liệu lệch.
Giải pháp:
Trong cùng transaction DB, vừa update record vừa ghi event vào bảng outbox.
Một background process đọc bảng outbox và publish sang message broker (Kafka/RabbitMQ).
Khi publish thành công → mark event là “processed”.
2. Các thuật toán có thể sử dụng cho ratelimit là gì? Giải thích
  - Token bucket: VD bạn có 1 cái xô nước, mỗi 1 request sẽ tương ứng với 1 lượng nước lấy ra khỏi xô. Khi xô hết nước thì ko cho lấy ra nũa. Thuận toán này sẽ có 2 input (size của xô, và thời gian refill nước vào xô)
  - Leaking bucket
  - Fixed window counter
  - Sliding window log
  - Sliding window counter 
