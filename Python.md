### About the location of libraries

To check which directories are searched when resolving the location of a python library use:

    import sys
    for p in sys.path: print(p)

To check where a library was actually found do either this

    import mylib
    print(mylib.__file__)

or this

    import inspect
    import mylib
    print(inspect.getfile(mylib))

### About bytecode files

When a python script is executed the python interpreter first compiles the script into bytecode and then executes it. The interpreter also stores the bytecode of each imported library in a file somewhere near that library. This allows the interpreter to speed up later executions by executing the pre-compiled bytecode file instead of compiling the library again. Additionally python is smart enough to notice when the library's source code changed. In such case the interpreter compiles the library again and overwrites the previous bytecode file.

Previous versions of python used to store the bytecode file in the same directory where the original library was located. The same file name was used but with the file extension `.pyc` for bytecode with debugging symbols or `.pyo` for optimized bytecode (i.e without debugging symbols). However this behavior was changed with [PEP 488 – Elimination of PYO files](https://peps.python.org/pep-0488/) and [PEP 3147 – PYC Repository Directories](https://peps.python.org/pep-3147/). Nowadays modern versions of python store the bytecode file in a directory called `__pycache__` located in the same directory where the library is located. The same file name is used but additional information is appended to it, namely the python version and the optimization level. Additionally all bytecode files have the extension `.pyc`.