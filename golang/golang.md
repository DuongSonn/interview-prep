# Golang
## Data Type
- int:
  - uint8 ,uint16, uint64, int8, int16, unt31, unt64 (8,16,32,64 are how many bits each type uses)
  - uint (unsigned integer) contains positive numbers or zero
  - byte the same as uint8
  - rune the same as int32
  - default value: 0
  - default type: int
- float:
  - float32, float64
  - Larger size floating-point numbers increase its precision.
  - Can represent: NaN and positive, negative infinity
  - default value: 0
  - default type: float64
- complex:
  - complex64, complex128: represent complex number with imaginary numbers
  - default type: complex128
- string:
  - A space is also considered a character
  - String index start from 0.
  - Character is presented by byte => fmt.Println("Hello"[1]) will print 101 (byte of e). Explain: get the character index 1 in string Hello
  - default value: ""
  - default type: string
- bool:
  - default value: true
## Costant Declarations
```
// single constant declaration
const a = 1

// multi constants declaration
const (
  b = 1
  c = "1"
  e,f  = true, false // e = true, f = false
)

//
const (
  x = 1 // x = 1, y = 1
  y

  z = 2 // z = 2, g = 2
  g
)

//
```

