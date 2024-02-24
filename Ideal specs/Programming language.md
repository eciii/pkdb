**Blocks of memory and types**

A **block of memory** is a region of memory specified by a _base address_ and a _length_. Blocks of memory are used to store data. Before using the data stored inside a block of memory the compiler first needs to determine which kinds of operations can be performed on and with that data. To do that the compiler classifies all the data it can handle using **types**. A type represents the way the data stored in a specific block of memory is to be interpreted. The data stored in a block of memory and interpreted according to a type is called a **value**.

**The tblock model**

A useful mental model to understand how such associations between blocks of memory and types can be implemented is the **tblock** structure (tblock is a shorthand for "typed block"). A tblock structure represent a raw block of memory and the corresponding type used to interpret that block of memory. A very simple implementation of tblocks can be (in pseudo-C/Go):

```
structure tblock_t {
	base_address <pointer> # pointer to base address of raw block of memory
	type         <type_t>  # type to interpret the raw block of memory
}

structure type_t {
	id     <something enumish> # some way to identify the type
	length <unsigned_integer>  # length of the raw block of memory
}
```

Note that some types might have a fixed length (e.g numeric types) while others might have variable length (e.g strings or lists).

**Names**

A **name** is just an identifier. A **name map** is a function that maps a collection of names to tblock structures. A **name stack** is a stack of name maps.

The **name resolution** algorithm of a name `N` with respect to a name stack `S` is the following:

- we start at the top of the stack, i.e at the last added name map in the stack, and check if the name `N` is defined in that map
- if yes then we return the associated tblock `B`; in such a case we say that _the name `N` resolves to the tblock `B` with respect to the name stack `S`_
- if not then we continue with the next name map in the stack and try to find the name there
- if the stack is exhausted and no block of memory is found for that name we say that _the name `N` is undefined with respect to the name stack `S`_

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
# public means read (and execute in the case of functions) only; to make a data
# member writtable just define a function that sets it.
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

---

**Namespaces with access controls**

Members of a namespace are by default private. To control access to a member of a namespace the following keywords can be used:

- If the member is a data member the `pub` keyword will make it read-write. To make it just read-only use `pub ro` (this should happen only in very special situations).
- If the member is a function member the `pub` keyword will make it execute-only. To make it execute-write use `pub xw` (this should happen only in very special situations).
- If the member is a namespace member the `pub` keyword will make it public (namespace members cannot be changed).

```
#
# Reasons:
#
# (*) because the name is at the same level or at an ancestor level (regardless
#     of whether it's public or not)
# (~) because all the components after the first one in the name path are public
#
# From here I can access:
#
# var1                   # (*)
# ns1.member2            # (~)
# ns1.nsInnerPub.member3 # (~)
#
# From here I cannot access (i.e none of the reasons hold):
#
# ns1.member1
# ns1.nsInner.member3
#

var1 = value1

ns1 = namespace {
	#
	# From here I can access:
	#
	# var1               # (*)
	# member1            # (*)
	# member2            # (*)
	# nsInner.member3    # (~)
	# nsInnerPub.member3 # (~)
	#
	# Notes about name collissions:
	#
	# * A member cannot have the same name of another member at an ancestor
	#   level (for example a member here cannot have the name var1)
	# * Members of sibling namespaces can have the same name (for example
	#   nsInner.member3 and nsInnerPub.member3)
	#
	
	member1 = value1
	pub member2 = value2
	
	nsInner = namespace {
		#
		# From here I can access:
		#
		# var1, member1, member2 and member3 because of (*)
		# nsInnerPub.member3 because of (*) and (~)
		#
		# Note that I can also access member1 as ns1.member1 but there is no
		# need to do that and is confusing (such namings should be automatically
		# fixed by a formatter)
		#
		
		pub member3 = value3
	}
	
	pub nsInnerPub = namespace {
		pub member3 = value3
	}
}
```

Attempt to emulate namespaces with access controls in a language without them:

```
var1 = value1

#: namespace "ns1" {

ns1member1 = value1
ns2member2 = value2 #: pub

#: namespace "nsInner" {
ns1nsInnerMember3 = value3
#: }

#: pub namespace "nsInner" {
ns1nsInnerPubMember3 = value3 #: pub
#: }

#: }
```

but keep in mind the following considerations:

- The whole namespace hierarchy is effectively "flattened" and everything becomes effectively public.
- The names of the namespaces are not needed anymore. They are given above just for a reference but they can just as well be omitted.
- The names of the variables/members now have to reflect the namespaces where they belong. This serves two purposes: avoid collisions and give semantic context. _From anecdotal experience I've come up to the conclusion that there should not be any rigid naming conventions in this respect_. Instead it is the responsibility of the developer to use common sense to find unique, meaningful and clear names for all the variables/members in the "flattened" namespace. For example in Go:

```
//: namespace "ssh" {

var sshHost string // private and set in initSSH
var sshControlPath // private and set in initSSH

func initSSH(host string) {} //: pub
func execSSHcmd(cmd string) {} //: pub
func closeSSH() {} //: pub

//: }
```

- Since everything becomes effectively public a convention for access control is established: everything is private unless documented public with a comment. It is the responsibility of the developer to honor this access control convention.

---

Missing considerations:

- **Section** is the name of a static box.
- Anonymous sections: Names of public members are merged into the name map defined by the containing namespace. Thus a public member of an anonymous section cannot have the same name of another member of the containing namespace. This rule does not apply to private members of the anonymous section.
- Emulation of classes in Go. Some ideas:
	- The members of a class must be defined in a struct and the methods are all methods of that struct.
	- The struct behaves like a public anonymous section, that is, whatever is private in the struct remains private and whatever is public in the struct is also public outside of the class.
- Emulation of "structured enums" (like the enums in Rust) in Go using "union structs" with "union keys".