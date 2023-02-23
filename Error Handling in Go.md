When a caller of a function receives an error there are three ways the caller can react:

- Handle the error *generically*.
- Use type assertions to check if the error *has* a specific *known* type or if the error's type *satisfies* a specific *known* interface, then handle the error in one way or another depending on the outcome.
- Use the `==` operator to check if the error *is* a specific *known* error, then handle the error in one way or another depending on the outcome.

If two errors are equal using the `==` operator we say that they are **strongly equal**. On the other hand we say that an error `err` is **weakly equal** to a known error `kerr` if either `err` is strongly equal to `kerr` or if `err` has a `Is(error) bool` method and `err.Is(kerr) == true`. This allows for errors to *represent* other errors. This is used, for example, by the `Errno` type in the` syscall` package. Note that strong equality implies weak equality.

Ideally, when checking if a specific known error occurred we should check for strong **and** weak equality. The `errors.Is` function does that.

In principle, that should be enough to do a robust error handling, but the Go developers decided to add an additional feature. They implemented the concept of **error chains**: If an error `err` has an `Unwrap() error` method and `err.Uwrap() != nil` then we say that `err` **wraps** another error. Since the wrapped error might in turn wrap another error and so on, a chain of errors emerge. Semantically this means that errors don't represent just errors any more, they represent error chains.

One may extend the concept of type assertions and weak equality to error chains. This is what the Go developers did with the `errors.Is` and `errors.As` functions. An error chain "type-asserts" to an specific known type/interface if any error in the chain type-asserts to that type/interface. An error chain is weakly equal to an specific known error if any error in the chain is weakly equal to the known error.

Unfortunately, in my opinion, error chains add unnecessary complexity and expose implementation details that should otherwise remain hidden (see the section "Whether to Wrap" [here](https://go.dev/blog/go1.13-errors)).