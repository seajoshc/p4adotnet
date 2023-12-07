# Go cookbook

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
