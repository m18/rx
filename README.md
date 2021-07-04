# rx — regular expression convenience functions

What's available

- access to matched groups by name
- matched group replacement using a callback
- bool results from find operations

## Examples

```go
package main

import (
	"fmt"
	"regexp"

	"github.com/m18/rx"
)

func main() {
	r := regexp.MustCompile(`(?P<greeting>\w+),\s*(?P<name>\w+)!`)
	res, ok := rx.FindGroups(r, "hello, world!")
	if !ok {
		fmt.Println("no matches")
		return
	}
	fmt.Printf("greeting: %s, name: %s\n", res["greeting"], res["name"])
}
```

Output
```
greeting: hello, name: world
```

```go
package main

import (
	"fmt"
	"regexp"

	"github.com/m18/rx"
)

func main() {
	r := regexp.MustCompile(`(?P<greeting>\w+),\s*(?P<name>\w+)!`)
	replace := func(m map[string]string) string {
		name := strings.ToUpper(m["name"])
		return name + ", " + m["greeting"] + "."
	}
	res := rx.ReplaceAllGroupsFunc(r, "hello, world!", replace)
	fmt.Println(res)
}
```

Output
```
WORLD, hello.
```
