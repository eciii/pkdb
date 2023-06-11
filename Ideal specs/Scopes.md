A **scope** can be either

- an **imperative scope**, where statements are meant to be executed in linear order and control flow statements are allowed to alter that order (e.g the scope defined by a function body is imperative)
- or a **declarative scope**, where only declaration statements are allowed and no specific order of execution is required (e.g the scope defined by file in Go is declarative). Declarative scopes can be further subdivided into
  - **single-entry scopes**, where only a certain function is allowed to be called by code from outside the scope (e.g the file that contains the main function of a Go program defines a single-entry declarative scope)
  - **multi-entry scopes**, where multiple functions are allowed to be called by code from outside the scope (e.g any file from a Go library package define a multi-entry declarative scope, where each public function represents an entry point)

---

- A **piece of data** is a connected region of memory specified by a starting point and a length.
- A **type of length L** represents the way a specific piece of data of length L is to be interpreted. The type of a specific piece of data determines which kinds of operations can be performed on and with that piece of data.

---

```
# This declares/defines a single-entry scope where the single-entry is the
# function myFunc
func myFunc {
	# This anonymous function represents the single-entry function myFunc
	func(...) {}

	# Functions that start with underscore are extention functions. They are
	# **only** visible from the single-entry function.
	func _extFunc1(...) {}
	func _extFunc2(...) {}
	func _extFunc3(...) {}

	# Functions that do not start with underscore are helper functions. They
	# are visible from anywhere in the scope. Helper functions must be used in
	# at least two different functions within the scope, otherwise they sould be
	# declared as extention functions.
	func hlpFunc1(...) {}
	func hlpFunc2(...) {}
	func hlpFunc3(...) {}

	# Both kinds of functions can be declared/defined using nested scopes,
	# which are themselves also single-entry scopes
	func _extFunc4 {
		func(...) {}
		func _extFunc41(...) {}
		func _extFunc42(...) {}
		func hlpFunc41(...) {}
		scope hlpFunc42 {
			func() {}
			func _extFunc421(...) {}
		}
	}

	# This declares a multi-entry scope. In contrast with single-entry scopes,
	# which must contain exactly one anonymous function and no public items,
	# multi-entry scopes must contain at least two public items and cannot
	# contain anonymous functions. Additionally a multi-entry scope must contain
	# at least one private item, otherwise the scope would be superfluous.
	scope {
		pub func publicFunc1(...) {}
		pub func publicFunc2(...) {}

		pub func publicFunc3 {
			func(...) {}
			func _extFunc31(...) {}
		}

		# Top-level functions in a multi-entry scope that are not public must
		# be helper functions. Thus, regardless of whether the scope is single
		# or multi-entry, helper functions are always private.
		func hlpFunc1(...) {}
	}
}
```

‌