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
5. <a name="common_5">Interface trong Go có gì đặc biệt? Sự khác biệt so với interface của Java/C++?</a>
  - interface trong golang là dùng để khai báo method cho 1 struct của Golang, ngoài ra interface còn có thể tượng chưng cho loại dữ liệu bất kỳ trong golang
  - điểm khác nhau giữa interface của golang và java:
    - interface của golang ko cần dùng keyword implements hay extends giống của java
    - interface của golang ko có keyword default giống của java
    - có empty inteface = kiểu data bất kỳ
6. <a name="common_6">Zero value trong Go là gì, tại sao lại quan trọng?</a>
  - Zero value trong Go là giá trị mặc định các data type của golang: VD string là "", int hay float là 0, bool là false, map là nil,....
  - Zero value trong golang giúp xác định được giá trị mặc định của 1 biến khi mới khai báo, ko cần khai báo giá trị mặc định của 1 biến mà có thể dùng luôn
7. <a name="common_7"> Tại sao Go không hỗ trợ kế thừa (inheritance) mà dùng  composition?</a>
  - Vấn đề nếu dùng kế thừa khi thay đổi interface cha -> phải thay đổi cả code của interface con
  - cho phép kế thừa 2 hoặc nhiều interface có method giống nhau (java không cho phép)
8. <a name="common_8"> SOLID là gì và được áp dụng cho golang như thế nào?</a>
  - S – Single Responsibility Principle
    - Mỗi struct/class/module chỉ nên có 1 lý do để thay đổi.
    - Ví dụ: struct UserRepository chỉ lo việc lưu/truy xuất dữ liệu User, không xử lý logic validate.
  - O – Open/Closed Principle
    - Mở rộng thì dễ (open for extension), chỉnh sửa code cũ thì hạn chế (closed for modification).
    - Ví dụ: dùng interface trong Go để thêm cách lưu trữ mới (DB, cache, file) mà không sửa code core. 
  - L – Liskov Substitution Principle
    - Một type con có thể thay thế type cha mà không phá logic.
    - Trong Go: bất kỳ struct nào implement interface thì đều thay thế được, miễn là hành vi hợp lý. 
  - I – Interface Segregation Principle
    - Interface nên nhỏ gọn, tránh “fat interface” mà mọi struct phải implement toàn bộ. 
  - D – Dependency Inversion Principle
    - Phụ thuộc vào abstraction (interface) chứ không phụ thuộc vào implementation cụ thể.
    - Trong Go: code business logic dùng interface (Storage), còn implementation (MySQLStorage, RedisStorage) inject từ ngoài vào.
9. <a name="common_9"> Rune trong golang được xử dụng thế nào và dùng trong TH nào? </a>
rune trong Go chủ yếu dùng khi xử lý ký tự Unicode (UTF-8), vì byte chỉ đúng cho ASCII (1 byte/char). rune thường dùng để:
- Khi xử lý chuỗi có ký tự UTF-8
- Khi cần so sánh hoặc kiểm tra ký tự
- Khi cần duyệt ký tự thực sự
- Khi muốn sửa / thay ký tự trong chuỗi
- Khi xử lý substring theo ký tự

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
  - Không điều gì xảy ra cả. Vì 2 routine này chạy trên 2 stack riêng biệt. Tuy nhiên 1 routine bị panic nếu ko xử lý sẽ gây panic cho main => Dừng cả chương trình.
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
