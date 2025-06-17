# GOLANG INTERVIEW ANSWERS

1. <a name="common_1">Điều gì làm golang khác việt so biệt so với ngôn ngữ khác?</a> 
  - Concurrency (Goroutine)
  - Go compile code thẳng về binary code
  - Go là statically typed và compiled language(biến phải khai báo type và type được check trong lúc compile)
  - Để deploy Go chỉ cần 1 file binary -> deploy 1 cách đơn giản
2. <a name="common_2"></a>
  - Heap dùng để cấp phát bộ nhớ cho biến cục bộ (biến trong func), rất nhanh, được dọn ngay sau khi func kết thúc và kích thước có giới hạn nhỏ (thường là vài MB)
  - Stack dùng để cấp phát bộ nhớ cho biến toàn cục, chậm hơn so với stack. được quản lý bở GC(Garbage Collector) và không có giới hạn kích thước. Thường được dùng để lưu trữ con trỏ, map, slice, array
3. <a name="common_3"></a>
  - GC sẽ chạy song song với các goroutine khác. GC sẽ tìm các đối tượng tham chiếu, các đối tượng không được tham chiếu nữa sẽ bị đánh dấu là "dead" và giải phóng khỏi heap
4. <a name="common_4"></a>
  - var: có thể khai báo biến có hoặc không có giá trị khởi tạo - TH không có giá trị thì sẽ nhận giá trị mặc định của kiểu dữ liệu, có thể khai báo biến toàn cục hoặc biến trong hàm
  - :=: chỉ khai báo biến trong hàm, bắt buộc phải có giá trị khởi tạo, không cần khai báo kiểu dữ liệu
  - new(): trả về con trỏ tới vùng nhớ đã được cấp phát và gán giá trị mặc định, dùng với kiểu dữ liệu cơ bản hoặc struct
## Slice,Array,Map
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
7. <a name="routine_mutex_7">So sánh thread với routine</a>
  - Thread được quản lý và lập lịch bở OS, số lượng tối đa ít do bị giới hạn bởi tài nguyên hệ thống, chặn toàn bộ thread nếu bị block, liên hệ với nhau sử dụng shared memory, chạy theo mô hình parallel
  - Routine đượic quản lý bởi goruntime và lập lịch bởi go scheduler, số lượng tối đa nhiều(hàng triệu) do, khi 1 routine bị chặn các routine khác vẫn chạy bt, liên hệ với nhau sử dụng channel, chạy theo mô hình concurrent
8. <a name="routine_mutex_8"></a>
  - Mỗi routine khi được khởi tạo sẽ tốn 2kB RAM và sẽ tăng tài nguyên lên nếu cần thêm. Có thể lên tới vài MB
9. <a name="routine_mutex_9"></a>
  - Routine leak là hiện tượng go routine vẫn chạy dù không càn thiết nữa -> dẫn tới tiêu tốn tài nguyên, chương trình bị chậm hoặc out of memory. VD: routine bị block vĩnh viexn khi chờ nhận/gửi dữ liệu trên channel mà ko ai gửi/nhận, routine chờ điều kiện mà không bao giờ được thỏa mãn
  - Cách phòng chống
    - Sử dụng context để hủy routine không cần thiết
    - Đóng channel khi không sử dụng nữa để các routine khác không đợi
    - Sử dụng select-case default: để tránh TH ko nhận được thông tin từ channel   
10. <a name="routine_mutex_10"></a>
