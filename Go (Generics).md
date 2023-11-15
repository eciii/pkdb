This is a summary of the "official" tutorial [Getting started with generics](https://go.dev/doc/tutorial/generics).

Say you have the following two functions:

```
func SumInts(m map[string]int64) int64 {
    var s int64
    for _, v := range m {
        s += v
    }
    return s
}

func SumFloats(m map[string]float64) float64 {
    var s float64
    for _, v := range m {
        s += v
    }
    return s
}
```

Note that these two functions are almost identical: if we ignore the name and consider only the signature and body we see a clear symmetry under the "transformation" `int64 <-> float64`. Generics enable us to make this symmetry explicit by defining just one function:

```
func SumIntsOrFloats[K comparable, V int64 | float64](m map[K]V) V {
    var s V
    for _, v := range m {
        s += v
    }
    return s
}
```

The portion `[K comparable, V int64 | float64]` in the function signature is a list of _type parameters_, in this case `K` and `V`, and their corresponding _type constraints_, `comparable` and `int64 | float64`. The type parameters of a generic function can be used anywhere in the function signature and body as if they were concrete types in a non-generic function.

The type constraint `comparable` is predeclared in Go, ans is satisfied by any type whose values may be used as an operand of the comparison operators `==` and `!=`. Since Go requires map keys to be comparable, declaring `K` as `comparable` is necessary to be able to use `K` as the key in the map variable. On the other hand the type constraint `int64 | float64` specifies a union of two types, which, by definition, is satisfied by, and only by, the two types in the union.

The generic function `SumIntsOrFloats` can be called like this:

```
ints := map[string]int64{"one": 1, "two": 2}
SumIntsOrFloats[string, int64](ints)

floats := map[string]float64{"one": 1.0, "two": 2.0}
SumIntsOrFloats[string, float64](floats)
```

The portions `[string, int64]` and `[string, float64]` are lists of _type arguments_ that are included in the function call to explicitly state the actual types that should replace the type parameters of the generic function. But in many cases the Go compiler is able to infer those type arguments from the types of the arguments provided to the function in the function call. Thus we can also call `SumIntsOrFloats` like a non-generic function:

```
ints := map[string]int64{"one": 1, "two": 2}
SumIntsOrFloats(ints)

floats := map[string]float64{"one": 1.0, "two": 2.0}
SumIntsOrFloats(floats)
```

Beware that the type inference mentioned above is not always possible. For example, if a generic function has no arguments, we would have to include the type arguments in the function call.

In our example, the type parameter of `V` is `int64 | float64`. This type parameter can be defined and used separately as follows:

```
type Number interface {
    int64 | float64
}

func SumIntsOrFloats[K comparable, V Number](m map[K]V) V {
	// same code here
}
```