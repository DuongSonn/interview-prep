# GOLANG INTERVIEW ANSWERS
## Slice,Array,Map
## Struct
### Interface,Method,Function
## Defer,Panic,Recovery
1. <a name="defer_panice_recovery_1"></a>
- Defer function được đẩy vào stack -> Thứ tự thực hiện các defer func là LIFO
## Routine, Mutex
1. <a name="routine_mutex_1"></a> Tại sao nên để số lượng worker = số CPU ?
  - Giả sử CPU có N core và N routine => Mỗi worker có thể tận dụng 100% CPU core
  - TH có nhiều worker => Cần switch context giữa các worker => Giảm performance
  - Go's runtime scheduler sẽ lập lịch để chạy goroutine trong OS thread
2.
