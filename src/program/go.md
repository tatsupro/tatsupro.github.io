# Go

## Hello World

```go
// for go modules
package main

// import fmt from standard library
import "fmt"

// main will be the first function to run
func main() {
	fmt.Println("Better Call Saul!")
}
```

- As you can see, each Go program organized by the go modules and called a `package`.
- Package “fmt” is a library from standard library, which can be found [here](https://pkg.go.dev/std).
- `main` is the first function to run in executable binary

Also you can import multiple packages with wrapped syntax like this

```go
import (
	"fmt"
	"math"
	"io"
)
```

It’s better when you can use this kind of systax with another keyword like `type`, `var` and `const`.

## Variables

To declare the variable, you can use `var` keyword. Go is statically type language so you need to declare a variable with a type with it.

```go
// basic declaration
// int is integer type
var age int

// declare with a value
// go will automatically infer type for it
var age = 12

// declare multiple value
var (
	age = 12
	height = 160
)
// or in 1 line of code
var age, height = 12, 160

// without the declaration of value
// the init value will depend on each type
// with int is 0
var age int
fmt.Println(age) // output: 0
```

### Type Inference

You can also declaring a variable without specifying an explicit type by using this syntax:

```go
// with a value
age := 12

// with multiple values
age, height := 12, 160
```

### Data Types

Go offers a rich collection of types, including numerics, booleans, strings, error, and the ability to create custom types.

```go
// default data types
n := 10 // int
pi := 3.14 // float64
a, b := true, false // bool
hi := "hello" // string

// specified types
var (
	a int8 // 8-bit signed integer
	b int16 // 16-bit signed integer
	c int32 // 32-bit signed integer
	d int64 // 64-bit signed integer
	e uint8 // 8-bit unsigned integer
	f uint16 // 16-bit unsigned integer
	g uint32 // 32-bit unsigned integer
	h uint64 // 64-bit unsigned integer
	j int // Both int and uint contain same size, either 32 or 64 bit.
	k uint // Both int and uint contain same size, either 32 or 64 bit.
)
var (
	l string // slice of characters
	m byte // it's a synonym of uint8.
	// A byte has a limit of 0 – 255 in numerical range
	// which can also represent an ASCII character
	n rune // it's a synonym of int32
	// and also represent Unicode code points.
	o uintptr // it's an unsigned integer type.
	// Its width is not defined, but its can hold all the bits of a pointer value.
)
```

## Concurrency

### Goroutines

A _goroutine_ is a lightweight thread managed by the Go runtime.

```go
func f(from string) {
	for i := 0; i < 3; i++ {
		fmt.Println(from, ":", i)
	}
}

func main() {
	go f("goroutine")
	f("direct")

	// wait for those function finish
	// for a more robust approach, use WaitGroup*
	time.Sleep(time.Second)
	fmt.Println("done")
}
```

Reference: [WaitGroup](https://pkg.go.dev/sync)

When we run this program, we see the output of the blocking call first, then the output of the two goroutines. The goroutines’ output may be interleaved, because goroutines are being run concurrently by the Go runtime.

```
direct : 0
direct : 1
direct : 2
goroutine : 0
goroutine : 1
goroutine : 2
done
```

### Channels

*Channels* are the pipes that connect concurrent goroutines. You can send values into channels from one goroutine and receive those values into another goroutine.

```go
ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.
```

```go
func main() {
	// channels must be created before use
	msg := make(chan string)

	// send "ping" string to msg channel
	go func() { msg <- "ping" }()

	// receive string from msg
	message := <- msg
	fmt.Println(message)
}
```

### Channel Buffering

By default channels are _unbuffered_, meaning that they will only accept sends (chan <-) if there is a corresponding receive (<- chan) ready to receive the sent value.
Buffered channels accept a limited number of values without a corresponding receiver for those values.

```go
func main() {
    messages := make(chan string, 2)

    messages <- "buffered"
    messages <- "channel"

    fmt.Println(<-messages)
    fmt.Println(<-messages)
}
```

```
buffered
channel
```

### Channel Synchronization

We can use channels to synchronize execution across goroutines. Here’s an example of using a blocking receive to wait for a goroutine to finish.

```go
// This is the function we’ll run in a goroutine.
// The `done` channel will be used to notify another goroutine that this function’s work is done.
func worker(done chan bool) {
    fmt.Print("working...")
    time.Sleep(time.Second)
    fmt.Println("done")

	// Send a value to notify that we’re done.
    done <- true
}

func main() {
	// Start a worker goroutine, giving it the channel to notify on.
    done := make(chan bool, 1)
    go worker(done)

	// Block until we receive a notification
	// from the worker on the channel.
    <-done
}
```

If you removed the `<- done` line from this program, the program would exit before the `worker` even started.

### Channel Directions

When using channels as function parameters, you can specify if a channel is meant to only send or receive values. This specificity increases the type-safety of the program.

```go
// This `ping` function only accepts a channel for sending values.
// It would be a compile-time error to try to receive on this channel.
func ping(pings chan<- string, msg string) {
    pings <- msg
}

// The `pong` function accepts one channel for receives (`pings`) and a second for sends (`pongs`).
func pong(pings <-chan string, pongs chan<- string) {
    msg := <-pings
    pongs <- msg
}

func main() {
    pings := make(chan string, 1)
    pongs := make(chan string, 1)
    ping(pings, "passed message")
    pong(pings, pongs)
    fmt.Println(<-pongs)
}
```

# Objectives

Write application that integrate with:

- REST APIs
- Cloud Provider
- Kubernetes
- Custom Intergration
