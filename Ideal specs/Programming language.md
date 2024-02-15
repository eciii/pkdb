A **block of memory** is a connected region of memory specified by a _base address_ and a _length_. 

_Note: Blocks of memory are used to store data. Data can be classified using types. A **type of length L** represents the way the data stored in a specific block of memory of length L is to be interpreted. The type of a specific block of memory determines which kinds of operations can be performed on and with the data stored in it._

A **name** is just an identifier. 

A **name map** is a function that maps a collection of names to blocks of memory.

A **name stack** is a stack of name maps.

The **name resolution** algorithm of a name with respect to a name stack is the following:

- we start at the top of the stack, i.e at the last added name map in the stack, and check if the name is defined in that map
- if yes then we return the associated block of memory; in such a case we say that _the name resolves to the block of memory with respect to the name stack_
- if not then we continue with the next name map in the stack and try to find the name there
- if the stack is exhausted and no block of memory is found for that name we say that _the name is undefined with respect to the name stack_

---

A **box** is a section of source code that is treated as a unit by the compiler. In particular a box is the textual unit to create name maps. The names in the name map determined by a box are called the **members** of the box.

A box can be either:

- **imperative**, where statements are meant to be executed in linear order and control flow statements are allowed to alter that order (e.g the body of a function)
- **declarative**, where only declaration statements are allowed and no specific order of execution is required (e.g a file in Go). Declarative boxes can be further subdivided into:
  - **function boxes**, where only a certain function member is allowed to be called by code from outside of the box (e.g the file that contains the main function of a Go program)
  - **standard boxes**, where access to multiple members of the box from the outside is allowed (e.g any file from a Go library package). Standard boxes can be further subdivided into:
    - **static boxes**, where exactly one instance of the box exists permanently for the whole execution
    - **dynamic boxes**, where the box is created and destroyed on demand at runtime

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

---

```
#
# Within a box, the let keyword is used to declare members. The let keyword can
# be followed by the pub keyword to declare that member as public. Note that
# public means read-only public; to make a data member writtable just define a
# function that sets it.
#
# Regarding parameters in a function signature:
#
# * the list of required parameters is separated from the list of optional
#   parameters using the "|" symbol
# * all parameters can be provided "by name" using the name defined in the
#   signature; in such a case the position does not matter
# * required parameters can also be provided "by position" but is such case
#   _all_ required parameters must be provided by position
# * optional parameters must always be provided by name
# * maybe there should be an option for default values for function
#   parameters??? on the other hand, if each type has a default "zero/empty"
#   value then maybe there is no need to provide default values for function
#   parameters...
#
```

The following is an attempt to define a static box initialization pattern:

```
let sshconn = box {
	# private and settable via init
	let host string
	let env  string

	# internal
	let controlPath string

	# public interface
	let pub init = func(host string | env string) {}
	let pub close = func() {}
	let pub execCmdOut = func(cmdstr string) ([]byte, error) {}
	let pub execCmd = func(cmdstr string) error {}
}

sshconn.init(host: "myhost", env: "foo=bar")
let err = sshconn.execCmd("ls -lhA")
sshconn.close()

# --- In Go ---

//: box sshconn {

var sshconn = struct{
	// private and settable via init
	host string
	env  string

	// internal
	controlPath string
}{}

// public interface
func sshconn_init(host string, env string) {} //: pub
func sshconn_close() {} //: pub
func sshconn_execCmdOut(cmdstr string) ([]byte, error) {} //: pub
func sshconn_execCmd(cmdstr string) error {} //: pub

//: }

sshconn_init("myhost", "foo=bar")
err := sshconn_execCmd("ls -lhA")
sshconn_close()
```

The following is an attempt to define a dynamic box initialization pattern:

```
let sshconnT = class {
	# private and settable via new
	let host string
	let env  string

	# internal
	let controlPath string

	# constructor (special static function)
	let pub new = func(host string | env string) {}

	# public interface
	let pub close = func() {}
	let pub execCmdOut = func(cmdstr string) ([]byte, error) {}
	let pub execCmd = func(cmdstr string) error {}
}

# note that new is accessed via sshconnT
let conn = sshconnT.new(host: "myhost", env: "foo=bar")
let err = conn.execCmd("ls -lhA")
conn.close()

# --- In Go ---

//: class sshconnT {

type sshconnT struct{
	// private and settable via init
	host string
	env  string

	// internal
	controlPath string
}

func newSSHconnT(host string, env string) {} //: pub

func (sshconn *sshconnT) close() {} //: pub
func (sshconn *sshconnT) execCmdOut(cmdstr string) ([]byte, error) {} //: pub
func (sshconn *sshconnT) execCmd(cmdstr string) error {} //: pub

//: }

conn := newSSHconn("myhost", "foo=bar")
err := conn.execCmd("ls -lhA")
conn.close()
```