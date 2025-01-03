# GOLANG INTERVIEW ANSWERS

1. <a name="common_1"></a> Điều gì làm golang khác việt so biệt so với ngôn ngữ khác?
  - Concurrency (Goroutine)
  - Go compile code thẳng về binary code
  - Go là statically typed và compiled language(biến phải khai báo type và type được check trong lúc compile)
  - Để deploy Go chỉ cần 1 file binary -> deploy 1 cách đơn giản
2. <a name="common_2"></a> So sánh parallel và multi thread

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
2. <a name="routine_mutex_2"></a> Điều gì xảy ra nếu buffered channel bị full và có data mới được đẩy vào channel?
  - Routine gửi data vào channel bị full sẽ bị block đến khi nào channel giải phóng bớt hoặc bị timeout nếu set timeout cho routine
3. <a name="routine_mutex_3"></a> So sánh thread và goroutine
  - Goroutine nhẹ hơn thread
  - Không sử dụng shared memories, liên lạc với nhau thông qua channel
4. <a name="routine_mutex_4"></a> Goroutine được quản lý và lên lịch bởi gì?
  - Goroutine được quản lý bở Go runtime và lên lịch bở Go scheduler
5. 
