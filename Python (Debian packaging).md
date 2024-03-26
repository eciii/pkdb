### About Python 2

`python2` is no longer available in Debian (bullseye). If it is installed in a modern Debian system it most certainly came from an older Debian release.

The `python-is-python2` and `python-is-python3` packages define the target of the "bare" `python` command (neither `python2` nor `python3` define it).

### About the location of libraries

When resolving the location of a library in Debian 11, Python searches the following directories in the stated order:

	(empty)                           (directory where the script is located)
	/usr/lib/python3X.zip             (???)
	/usr/lib/python3.X                (core and standard library, managed by apt)
	/usr/lib/python3.X/lib-dynload    (core and standard library, managed by apt)
	/usr/local/lib/python3.X/dist-packages   (managed by pip)
	/usr/lib/python3/dist-packages    (additional packages, managed by apt)

### About `apt` and `pip` compatibility

`pip` is Python's package manager. It is available in Debian through the `python3-pip` package. Since both `pip` and `apt` manage python packages `pip` tries not to get in the way of `apt` by installing all its packages under `/usr/local/lib/python3.X` and leaving `/usr/lib/python3.X` and `/usr/lib/python3` untouched (these are the locations used by `apt`, the former for the python core and standard library while the latter for any additional packages.).

However there are potential risks:

- Since `pip` uses `/usr/local/lib/python3.X` it is unclear what happens when Debian is upgraded to a new release that uses a newer minor version, say `3.Y`. I would guess that in such a case `apt` is able to seamlessly upgrade the python core and standard library to the new location `/usr/lib/python3.Y`. The new `python3` binary will search for libraries in `/usr/local/lib/python3.Y` and won't be able to find anything there since all the libraries installed previously by `pip` are in `/usr/local/lib/python3.X`.
- When installing a dependency of a package, say `depX`, `pip` searches if such dependency is already satisfied by an already installed package (possibly installed and managed by `apt`). If no such dependency is found `pip` will install it with the latest version under `/usr/local/lib/python3.X`. At a later time `apt` might try to install a package that also depends on `depX` and will install the dependency in `/usr/lib/python3`, possibly with a lower version number. This might break the package installed by `apt` because `/usr/local/lib/python3.X` has higher precedence over `/usr/lib/python3`.

Starting with Debian 12 this issues are solved by [PEP 668](https://peps.python.org/pep-0668/). See also the file `/usr/share/doc/python3.11/README.venv` on a Debian 12 system for more information.