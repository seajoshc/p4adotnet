# Go cookbook

## Organization

Use [internal packages](https://dave.cheney.net/2019/10/06/use-internal-packages-to-reduce-your-public-api-surface).

## Structs

### Nested structs

Structs can be nested to represent more complex entities.

```go
type car struct {
  Make string
  Model string
  Height int
  Width int
  FrontWheel Wheel
  BackWheel Wheel
}

type Wheel struct {
  Radius int
  Material string
}
```

The fields of a struct can be accessed using the dot . operator.

```go
myCar := car{}
myCar.FrontWheel.Radius = 5
```

### Embedded structs

```go
type car struct {
  make string
  model string
}

type truck struct {
  // "car" is embedded, so the definition of a
  // "truck" now also additionally contains all
  // of the fields of the car struct
  car
  bedSize int
}
```

An embedded struct's fields are accessed at the top level, unlike nested structs.
Promoted fields can be accessed like normal fields except that they can't be used in composite literals.

```go
lanesTruck := truck{
  bedSize: 10,
  car: car{
    make: "toyota",
    model: "camry",
  },
}

fmt.Println(lanesTruck.bedSize)

// embedded fields promoted to the top-level
// instead of lanesTruck.car.make
fmt.Println(lanesTruck.make)
fmt.Println(lanesTruck.model)
```

## Working with JSON

https://mholt.github.io/json-to-go/

[AMAZING blog post](https://blog.boot.dev/golang/json-golang/), [mirror](https://gist.github.com/seajoshc/29ebbecaf25b08d2d4a6ace10bcd3e20)

The `WebhookPayload` structure in the Go code represents the JSON payload that your webhook expects to receive. Let's break down the nested structure to understand how it correlates with your JSON format:

1. **Overall Structure**: `WebhookPayload` is designed to match the top-level structure of your JSON payload. It contains two fields, `Component` and `Parameters`, each corresponding to a key in your JSON.

2. **The `Component` Field**:

   - This is a nested structure within `WebhookPayload`.
   - It corresponds to the `"component"` key in your JSON.
   - It is defined as a struct `Component`, which has fields `ID`, `Name`, and `Repository`.
   - These fields are meant to directly map to the nested JSON object under the `"component"` key.

3. **The `Parameters` Field**:

   - This represents the `"parameters"` key in your JSON.
   - It's defined as a `map[string]string`.
   - This is a flexible structure where keys and values are both strings.
   - It's used here because your parameters appear to be a collection of key-value pairs where both the keys and the values are strings.

4. **JSON Tags**:
   - Notice the `json:"..."` tags in the struct definitions. These tags tell the Go `json` package how to map the JSON keys to struct fields.
   - For example, `ID string json:"id"` in the `Component` struct means that the `ID` field in Go is mapped to the `"id"` key in your JSON payload.

Here's a visual breakdown:

Your JSON Payload:

```json
{
  "component": {
    "id": "...",
    "name": "...",
    "repository": "..."
  },
  "parameters": {
    "key1": "value1",
    "key2": "value2"
    // ... more key-value pairs ...
  }
}
```

Mapped to Go Structs:

```go
type WebhookPayload struct {
    Component  Component  `json:"component"`  // Maps to "component" in JSON
    Parameters Parameters `json:"parameters"` // Maps to "parameters" in JSON
}

type Component struct {
    ID         string `json:"id"`          // Maps to "id" in component
    Name       string `json:"name"`        // Maps to "name" in component
    Repository string `json:"repository"`  // Maps to "repository" in component
}

type Parameters map[string]string // Maps to key-value pairs in "parameters"
```

When a JSON payload is received, Go's `json.Unmarshal` function uses these definitions to correctly parse the JSON into a `WebhookPayload` object, making the data structured and easy to work with in your code.

### Example

Handling a webhook of a specific shape. e.g.

```json
{
  "component": {
    "id": "...",
    "name": "...",
    "repository": "..."
  },
  "parameters": {
    "key1": "value1",
    "key2": "value2"
    // ... more key-value pairs ...
  }
}
```

```go
package main

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)

// Define structs to match the JSON structure
type Component struct {
	ID         string `json:"id"`
	Name       string `json:"name"`
	Repository string `json:"repository"`
}

type Parameters map[string]string

type WebhookPayload struct {
	Component  Component  `json:"component"`
	Parameters Parameters `json:"parameters"`
}

func webhookHandler(w http.ResponseWriter, r *http.Request) {
	if r.Method != "POST" {
		http.Error(w, "Only POST method is accepted", http.StatusMethodNotAllowed)
		return
	}

	body, err := ioutil.ReadAll(r.Body)
	if err != nil {
		http.Error(w, "Error reading request body", http.StatusInternalServerError)
		return
	}
	defer r.Body.Close()

	var payload WebhookPayload
	if err := json.Unmarshal(body, &payload); err != nil {
		http.Error(w, "Error decoding JSON", http.StatusBadRequest)
		return
	}

	// Process the payload here
	fmt.Printf("Received component: %+v\n", payload.Component)
	fmt.Printf("Received parameters: %+v\n", payload.Parameters)

	// Respond to the webhook sender
	fmt.Fprintf(w, "Webhook received and processed")
}

func main() {
	http.HandleFunc("/webhook", webhookHandler)
	log.Fatal(http.ListenAndServe(":8080", nil))
}


```

### Send tokens to a channel

Waiting for databases to come online for example.

```go
func getDatabasesChannel(numDBs int) chan struct{} {
	ch := make(chan struct{})
	go func() {
		for i := 0; i < numDBs; i++ {
			ch <- struct{}{} // send an empty struct, used as a "token"
			fmt.Printf("Database %v is online\n", i+1)
		}
	}()
	return ch
}
```

Send an empty struct, `struct{}{}`, to a channel to be used as a "token".

Then block and wait for a specific number of tokens in another function.

```go
	for i := 0; i < numDBs; i++ {
		<-dbChan // unary operator pops off empty struct token from queue
	}
}
```
