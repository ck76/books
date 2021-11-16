

- test.go

```go
package main
import "fmt"
func main() {
 	fmt.Println("Test Message")
}
```



- llgo -dump test.go

```c
$llgo -dump test.go
; ModuleID = 'main'
target datalayout = "e-p:64:64:64..."
target triple = "x86_64-unknown-linux"
%0 = type { i8*, i8* }
...

```

