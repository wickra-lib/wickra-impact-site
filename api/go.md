# Go

The Go binding links the C ABI via cgo. Construct an `Impact` from a JSON spec and
drive it with `Command(json) -> (json, error)`.

```bash
go get github.com/wickra-lib/wickra-impact-go
```

```go
package main

import (
	"fmt"

	wickra "github.com/wickra-lib/wickra-impact-go"
)

func main() {
	spec := `{"strategy":{"spec_version":1,"symbol":"IMPACT","timeframe":"1h",` +
		`"indicators":{},"entry":{"ge":[{"price":"close"},0]},"exit":{"in_position":true},` +
		`"sizing":{"type":"fixed_qty","qty":10.0},` +
		`"execution":{"order_type":"market","fill_timing":"next_open"}},` +
		`"book_model":{"kind":"orderbook_walk"},"participation_cap":1.0,"latency_ms":0}`

	impact, err := wickra.New(spec)
	if err != nil {
		panic(err)
	}
	defer impact.Close()

	data := `{"IMPACT":[{"time":0,"open":100,"high":101,"low":99,"close":100,"volume":1000}]}`
	report, err := impact.Command(`{"cmd":"run","data":` + data + `}`)
	if err != nil {
		panic(err)
	}
	fmt.Println(report)
}
```

## More

- [pkg.go.dev/github.com/wickra-lib/wickra-impact-go](https://pkg.go.dev/github.com/wickra-lib/wickra-impact-go)
- [Source & examples](https://github.com/wickra-lib/wickra-impact/tree/main/examples/go)
