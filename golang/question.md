# GOLANG INTERVIEW QUESTIONS

1. Điều gì làm golang khác việt so biệt so với ngôn ngữ khác? [Answer](./answer.md#common_1)
2. Heap và stack khác nhau thế nào? Khi nào sử dụng heap, khi nào sử dụng stack?

## Slice, Array, Map
1. Slice khác gì với Array? [Answer](./answer.md#slice_1)
2. ```
   package main
   import "fmt"

   func main() {
   	a := [...]int{0, 1, 2, 3}
   	x := a[:1]
   	y := a[2:]
   	x = append(x, y...)
   	x = append(x, y...)
   	fmt.Println(a, x)
   }
   ```
Kết quả in ra màn hình [Answer](./answer.md#slice_2)

3. ```
   package main
   func main() {
   	var x = []string{"A", "B", "C"}
   	for i, s := range x {
   		print(i, s, ",")
   		x[i+1] = "M"
   		x = append(x, "Z")
   		x[i+1] = "Z"
   	}
   }
   ```
Kết quả in ra màn hình [Answer](./answer.md#slice_3)

4. ```
   package main
   func main() {
	   m := make(map[string]int, 3)
	   x := len(m)
	   m["Go"] = m["Go"]
	   y := len(m)
	   println(x, y)
   }
   ```
Kết quả in ra màn hình [Answer](./answer.md#slice_4)
## Struct

## Interface, Method, Function
1. Golang có phải ngôn ngữ OOP không?
2. Variadic Functions là gì?
3. Generic Type là gì?
## Defer, Panic, Recovery
1. ```
   func main() {
    defer() {print(1)}
    defer() {print(2)}
    defer() {print(3)}
    defer() {print(.)}
    defer() {print(.)}
    defer() {print(.)}
    defer() {print(.)}
    defer() {print(10)}
   }
   ```
Thứ tự in ra ngoài màn hình là gì? [Answer](./answer.md#defer_panice_recovery_1)

## Routine, Mutex
1. Tại sao nên để số lượng worker = số CPU? [Answer](./answer.md#routine_mutex_1)
2. Điều gì xảy ra nếu buffered channel bị full và có data mới được đẩy vào channel? [Answer](./answer.md#routine_mutex_2)
3. So sánh thread và goroutine [Answer](./answer.md#routine_mutex_3)
4. Goroutine được quản lý và lên lịch bởi gì? [Answer](./answer.md#routine_mutex_4)
5. Điều gì xảy ra nếu 1 channel close() và cố gắng đẩy/lấy data từ channel đấy [Answer](./answer.md#routine_mutex_5)
6. Điều gì xảy ra với child routine nếu 1 parent routine panic() [Answer](./answer.md#routine_mutex_6)
7. So sánh thread và routine
8. 
