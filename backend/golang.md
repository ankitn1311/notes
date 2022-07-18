Go lang
Opinionated

go get - to download dependencies
godoc for pacakges

go ()
-bin
-src
github.com
ankitn1311
-go_project_1
#project files
-go_project_2
#project files

go env

GOPATH = "/Users/ankit/go/"

mkdir go
cd go
mkdir bin src
cd src
mkdir github.com
cd github.com
mkdir ankitn1311
cd ankitn1311
mkdir go_crash_course

cd go_crash_course

How to instal something using go get

go get github.com/aws/aws-sdk-go/aws it's like npm i

main.go - entry file

###Function

```
package main
import "fmt" // to print something it provides function

func main() {
  // run automatically
  fmt.Println("Hello world")
}
```

go run main

go install

###Types
string
bool
int
int int8 int16 int32 int64
uint uint8 uint16 uint32 uint64 uintptr
byte - alias for uint8
rune - alias for int32
float32 float64
complex64 complex128
