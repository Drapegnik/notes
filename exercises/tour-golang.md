# [A Tour of Go](https://tour.golang.com)

Exercises

## [Loops and Functions](https://tour.golang.com/flowcontrol/8)

### task

Implement this in the `func Sqrt` provided. A decent starting guess for `z` is 1, no matter what the input. To begin with, repeat the calculation 10 times and print each `z` along the way. See how close you get to the answer for various values of `x` (1, 2, 3, ...) and how quickly the guess improves.

```go
package main

import (
	"fmt"
)

func Sqrt(x float64) float64 {
}

func main() {
	fmt.Println(Sqrt(2))
}
```

### solution

```go
package main

import (
	"fmt"
)

func Sqrt(x float64) float64 {
	var z = 1.0
	for i := 0; i < 10; i++ {
		z -= (z*z - x) / (2 * z)
		fmt.Println(" -> ", z)
	}
	return z
}

func main() {
	fmt.Println(Sqrt(2))
}
```

## [Slices](https://tour.golang.com/moretypes/18)

### task

Implement `Pic`. It should return a slice of length `dy`, each element of which is a slice of `dx` 8-bit unsigned integers. When you run the program, it will display your picture, interpreting the integers as grayscale (well, bluescale) values.

```go
package main

import "golang.org/x/tour/pic"

func Pic(dx, dy int) [][]uint8 {
}

func main() {
	pic.Show(Pic)
}
```

### solution

```go
package main

import "golang.org/x/tour/pic"

func Pic(dx, dy int) [][]uint8 {
	var p [][]uint8
	for i := 0; i < dx; i++ {
		var d []uint8
		for j := 0; j < dy; j++ {
			d = append(d, uint8(i^j))
		}
		p = append(p, d)
	}
	return p
}

func main() {
	pic.Show(Pic)
}
```

### output

![](https://res.cloudinary.com/dzsjwgjii/image/upload/v1542639293/gopic.png)

## [Maps](https://tour.golang.com/moretypes/23)

### task

Implement `WordCount`. It should return a map of the counts of each “word” in the string `s`. The `wc.Test` function runs a test suite against the provided function and prints success or failure.

```go
package main

import (
	"golang.org/x/tour/wc"
)

func WordCount(s string) map[string]int {
	return map[string]int{"x": 1}
}

func main() {
	wc.Test(WordCount)
}
```

### solution

```go
package main

import (
	"golang.org/x/tour/wc"
	"strings"
)

func WordCount(s string) map[string]int {
	var m = make(map[string]int)
	for _, w := range strings.Fields(s) {
		m[w] += 1
	}
	return m
}

func main() {
	wc.Test(WordCount)
}
```

## [Fibonacci closure](https://tour.golang.com/moretypes/26)

Implement a `fibonacci` function that returns a function (a closure) that returns successive fibonacci numbers.

### task

```go
package main

import "fmt"

// fibonacci is a function that returns
// a function that returns an int.
func fibonacci() func() int {
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
```

### solution

```go
package main

import "fmt"

// fibonacci is a function that returns
// a function that returns an int.
func fibonacci() func() int {
	var a, b, counter = 0, 1, 0
	return func() int {
		var sum int
		if counter == 0 {
			sum = a
		}
		if counter > 0 {
			sum = a + b
			b = a
			a = sum
		}

		counter += 1
		return sum
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
```

## [Stringers](https://tour.golang.com/methods/18)

### task

Make the `IPAddr` type implement `fmt.Stringer` to print the address as a dotted quad.

For instance, `IPAddr{1, 2, 3, 4}` should print as `"1.2.3.4"`.

```go
package main

import "fmt"

type IPAddr [4]byte

// TODO: Add a "String() string" method to IPAddr.

func main() {
	hosts := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
}
```

### solution

```go
package main

import (
	"fmt"
	"strings"
)

type IPAddr [4]byte

func (ip IPAddr) String() string {
	var ips []string
	for _, v := range ip {
		ips = append(ips, fmt.Sprintf("%v", v))
	}
	return fmt.Sprintf(strings.Join(ips, "."))
}

func main() {
	hosts := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
}
```

## [Errors](https://tour.golang.com/methods/20)

### task

`Sqrt` should return a non-nil error value when given a negative number, as it doesn't support complex numbers.

Change your `Sqrt` function to return an `ErrNegativeSqrt` value when given a negative number.

```go
package main

import (
	"fmt"
)

func Sqrt(x float64) (float64, error) {
	return 0, nil
}

func main() {
	fmt.Println(Sqrt(2))
	fmt.Println(Sqrt(-2))
}
```

### solution

```go
package main

import (
	"fmt"
)

type ErrNegativeSqrt float64

func (e ErrNegativeSqrt) Error() string {
	return fmt.Sprintf("cannot Sqrt negative number: %v", float64(e))
}

func Sqrt(x float64) (float64, error) {
	if x < 0 {
		return 0, ErrNegativeSqrt(x)
	}

	var z = 1.0
	for i := 0; i < 10; i++ {
		z -= (z*z - x) / (2 * z)
	}
	return z, nil
}

func main() {
	fmt.Println(Sqrt(2))
	fmt.Println(Sqrt(-2))
}
```

## [Readers](https://tour.golang.com/methods/22)

Implement a `Reader` type that emits an infinite stream of the ASCII character `'A'`.

### task

```go
package main

import "golang.org/x/tour/reader"

type MyReader struct{}

// TODO: Add a Read([]byte) (int, error) method to MyReader.

func main() {
	reader.Validate(MyReader{})
}
```

### solution

```go
package main

import "golang.org/x/tour/reader"

type MyReader struct{}

func (r MyReader) Read(b []byte) (int, error) {
	b[0] = 'A'
	return 1, nil
}

func main() {
	reader.Validate(MyReader{})
}
```

## [rot13Reader](https://tour.golang.com/methods/23)

Implement a `rot13Reader` that implements `io.Reader` and reads from an `io.Reader`, modifying the stream by applying the [rot13](https://en.wikipedia.org/wiki/ROT13) substitution cipher to all alphabetical characters.

The `rot13Reader` type is provided for you. Make it an `io.Reader` by implementing its `Read` method.

### task

```go
package main

import (
	"io"
	"os"
	"strings"
)

type rot13Reader struct {
	r io.Reader
}

func main() {
	s := strings.NewReader("Lbh penpxrq gur pbqr!")
	r := rot13Reader{s}
	io.Copy(os.Stdout, &r)
}
```

### solution

```go
package main

import (
	"io"
	"os"
	"strings"
)

type rot13Reader struct {
	r io.Reader
}

func Shift(b byte, start byte) byte {
	return start + (b - start + 13) % 26
}

func (r13 rot13Reader) Read(b []byte) (int, error) {
	var n, err = r13.r.Read(b)
	if err != nil {
		return n, err
	}
	for i := 0; i < n; i++ {
		if b[i] >= 'a' && b[i] <= 'z' {
			b[i] = Shift(b[i], 'a')
		}
		if b[i] >= 'A' && b[i] <= 'Z' {
			b[i] = Shift(b[i], 'A')
		}
	}
	return n, nil
}

func main() {
	s := strings.NewReader("Lbh penpxrq gur pbqr!")
	r := rot13Reader{s}
	io.Copy(os.Stdout, &r)
}
```

### output

> You cracked the code!

## [Images](https://tour.golang.com/methods/25)

Define your own `Image` type, implement the necessary methods, and call `pic.ShowImage`.

`Bounds` should return a `image.Rectangle`, like `image.Rect(0, 0, w, h)`.

`ColorModel` should return `color.RGBAModel`.

`At` should return a color; the value `v` in the last picture generator corresponds to `color.RGBA{v, v, 255, 255}` in this one.

### task

```go
package main

import "golang.org/x/tour/pic"

type Image struct{}

func main() {
	m := Image{}
	pic.ShowImage(m)
}
```

### solution

```go
package main

import (
	"golang.org/x/tour/pic"
	"image"
	"image/color"
)

type Image struct {
	w, h int
}

func (i Image) ColorModel() color.Model {
	return color.RGBAModel
}

func (i Image) Bounds() image.Rectangle {
	return image.Rect(0, 0, i.w, i.h)
}

func (i Image) At(x, y int) color.Color {
	v := uint8(x ^ y)
	return color.RGBA{v, v, 255, 255}
}

func main() {
	m := Image{500, 500}
	pic.ShowImage(m)
}
```

## [Equivalent Binary Trees](https://tour.golang.com/concurrency/7)

```go
type Tree struct {
    Left  *Tree
    Value int
    Right *Tree
}
```

1.  Implement the `Walk` function.
2.  Test the `Walk` function.
3.  Implement the `Same` function using `Walk` to determine whether `t1` and `t2` store the same values.
4.  Test the `Same` function.

### task

```go
package main

import "golang.org/x/tour/tree"

// Walk walks the tree t sending all values
// from the tree to the channel ch.
func Walk(t *tree.Tree, ch chan int)

// Same determines whether the trees
// t1 and t2 contain the same values.
func Same(t1, t2 *tree.Tree) bool

func main() {
}
```

### solution

```go
package main

import (
	"fmt"
	"golang.org/x/tour/tree"
)

func _Walk(t *tree.Tree, ch chan int) {
	if t == nil {
		return
	}
	_Walk(t.Left, ch)
	ch <- t.Value
	_Walk(t.Right, ch)
}

// Walk walks the tree t sending all values
// from the tree to the channel ch.
func Walk(t *tree.Tree, ch chan int) {
	_Walk(t, ch)
	close(ch)
}

// Same determines whether the trees
// t1 and t2 contain the same values.
func Same(t1, t2 *tree.Tree) bool {
	ch1 := make(chan int)
	ch2 := make(chan int)
	go Walk(t1, ch1)
	go Walk(t2, ch2)
	for v1 := range ch1 {
		v2 := <-ch2
		if v1 != v2 {
			return false
		}
	}
	return true
}

func main() {
	ch := make(chan int)
	go Walk(tree.New(1), ch)

	for v := range ch {
		fmt.Println(v)
	}

	if Same(tree.New(1), tree.New(1)) {
		fmt.Println("OK")
	}
	if !Same(tree.New(1), tree.New(2)) {
		fmt.Println("OK")
	}
}
```

## [Web Crawler](https://tour.golang.com/concurrency/10)

Modify the Crawl function to fetch URLs in parallel without fetching the same URL twice.

### task

```go
package main

import (
	"fmt"
)

type Fetcher interface {
	// Fetch returns the body of URL and
	// a slice of URLs found on that page.
	Fetch(url string) (body string, urls []string, err error)
}

// Crawl uses fetcher to recursively crawl
// pages starting with url, to a maximum of depth.
func Crawl(url string, depth int, fetcher Fetcher) {
	// TODO: Fetch URLs in parallel.
	// TODO: Don't fetch the same URL twice.
	// This implementation doesn't do either:
	if depth <= 0 {
		return
	}
	body, urls, err := fetcher.Fetch(url)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Printf("found: %s %q\n", url, body)
	for _, u := range urls {
		Crawl(u, depth-1, fetcher)
	}
	return
}

func main() {
	Crawl("https://golang.org/", 4, fetcher)
}

// fakeFetcher is Fetcher that returns canned results.
type fakeFetcher map[string]*fakeResult

type fakeResult struct {
	body string
	urls []string
}

func (f fakeFetcher) Fetch(url string) (string, []string, error) {
	if res, ok := f[url]; ok {
		return res.body, res.urls, nil
	}
	return "", nil, fmt.Errorf("not found: %s", url)
}

// fetcher is a populated fakeFetcher.
var fetcher = fakeFetcher{
	"https://golang.org/": &fakeResult{
		"The Go Programming Language",
		[]string{
			"https://golang.org/pkg/",
			"https://golang.org/cmd/",
		},
	},
	"https://golang.org/pkg/": &fakeResult{
		"Packages",
		[]string{
			"https://golang.org/",
			"https://golang.org/cmd/",
			"https://golang.org/pkg/fmt/",
			"https://golang.org/pkg/os/",
		},
	},
	"https://golang.org/pkg/fmt/": &fakeResult{
		"Package fmt",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
	"https://golang.org/pkg/os/": &fakeResult{
		"Package os",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
}
```

### solution

```go
type SafeMap struct {
	urls map[string]bool
	mux  sync.Mutex
}

func (m *SafeMap) Save(key string) {
	m.mux.Lock()
	m.urls[key] = true
	m.mux.Unlock()
}

func (m *SafeMap) Check(key string) bool {
	m.mux.Lock()
	defer m.mux.Unlock()
	return m.urls[key]
}

// Crawl uses fetcher to recursively crawl
// pages starting with url, to a maximum of depth.
func Crawl(url string, depth int, fetcher Fetcher, store SafeMap) {
	if depth <= 0 || store.Check(url) {
		return
	}

	store.Save(url)

	body, urls, err := fetcher.Fetch(url)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Printf("found: %s %q\n", url, body)

	for _, u := range urls {
		go Crawl(u, depth-1, fetcher, store)
	}
	return
}

func main() {
	store := SafeMap{urls: make(map[string]bool)}
	go Crawl("https://golang.org/", 4, fetcher, store)
	time.Sleep(time.Second)
}
```
