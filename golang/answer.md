# GOLANG INTERVIEW ANSWERS

1. <a name="common_1">Điều gì làm golang khác việt so biệt so với ngôn ngữ khác?</a> 
  - Concurrency (Goroutine)
  - Go compile code thẳng về binary code
  - Go là statically typed và compiled language(biến phải khai báo type và type được check trong lúc compile)
  - Để deploy Go chỉ cần 1 file binary -> deploy 1 cách đơn giản

## Slice,Array,Map
1. <a name="slice_1">So sánh parallel và multi thread</a>
2. <a name="slice_2">[0,2,3,3]; [0,2,3,3,3]</a>
- Do x đang tham chiếu đến a  nên mọi sự thay đổi của x sẽ ảnh hưởng đến a và ngược lại (tương tự với y)
```
a := [...]int{0, 1, 2, 3}
x := a[:1] // x = []int{0}
y := a[2:] // y = []int{2,3}
x = append(x, y...) // x =  [0,2,3], a = [0,2,3,3], y = [3,3]
x = append(x, y...) // x = [0,2,3,3,3]
fmt.Println(a, x)
```
3. <a name="slice_3">0A, 1M, 2C</a>
- Khi i = 0 -> In ra "0A"
- x[i+1] -> x = ["A", "M", "C"]
- x = append(x,"Z") -> Khi thực hiện append thêm giá trị Z thì golang sẽ sinh ra 1 underlying array mới cho x (gọi tạm là x2) ->  x[i+1] = "Z" sẽ là thay đổi ở x2 ko thay đổi ở x
- Vòng for range chỉ lấy snapshot của chiều dài slice tại thời điểm bắt đầu vòng lặp -> chạy với underlying của x
- Vòng for range sẽ skip qua nếu slice,array rỗng
- Vòng for range sẽ lấy snapshot của array tại thời điểm bắt đầu, mọi thay đổi ở array đều ko áp dụng với loop. Trừ TH sử dụng con trỏ để thay đổi giá trị
```
var x = []string{"A", "B", "C"}
for i, s := range x {
  print(i, s, ",")
  x[i+1] = "M"
  x = append(x, "Z")
  x[i+1] = "Z"
}
```
4. <a name="slice_3">0,1</a>
- m := make(map[string]int, 3) => Tạo ra map có độ dài bằng 3. Nhưng không có data => len(m) = 0
- m["Go"] = m["Go"] => Thêm key "Go" vào trong map => len(m) = 1
```
m := make(map[string]int, 3)
x := len(m)
m["Go"] = m["Go"]
y := len(m)
println(x, y)
```

5.

## Struct
### Interface,Method,Function
## Defer,Panic,Recovery
1. <a name="defer_panice_recovery_1"></a>
  - Defer function được đẩy vào stack -> Thứ tự thực hiện các defer func là LIFO
## Routine, Mutex
1. <a name="routine_mutex_1">Tại sao nên để số lượng worker = số CPU ?</a>
  - Giả sử CPU có N core và N routine => Mỗi worker có thể tận dụng 100% CPU core
  - TH có nhiều worker => Cần switch context giữa các worker => Giảm performance
  - Go's runtime scheduler sẽ lập lịch để chạy goroutine trong OS thread
2. <a name="routine_mutex_2">Điều gì xảy ra nếu buffered channel bị full và có data mới được đẩy vào channel?</a>
  - Routine gửi data vào channel bị full sẽ bị block đến khi nào channel giải phóng bớt hoặc bị timeout nếu set timeout cho routine
3. <a name="routine_mutex_3">So sánh thread và goroutine</a>
  - Goroutine nhẹ hơn thread
  - Không sử dụng shared memories, liên lạc với nhau thông qua channel
4. <a name="routine_mutex_4">Goroutine được quản lý và lên lịch bởi gì?</a>
  - Goroutine được quản lý bở Go runtime và lên lịch bở Go scheduler
5. <a name="routine_mutex_5">Điều gì xảy ra nếu 1 channel close() và cố gắng đẩy/lấy data từ channel đấy?</a>
- Nếu channel là buffered channel, vẫn lấy được data từ channel đến khi channel hết data
- Nếu channel đẫ hết data thì dữ liệu sẽ trả về 0 và false
- Gửi data vào channel sẽ gây ra panic
6. <a name="routine_mutex_6">Điều gì xảy ra với child routine nếu 1 parent routine panic()?</a> 
- Nếu 1 routine bị panic thì các routine associate với routine đấy cũng sẽ bị panic
