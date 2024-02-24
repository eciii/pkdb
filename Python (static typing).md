We can model how types are implemented in Python using the tblock model (see [[Programming language]]). The translation of the tblock model to Python terminology is:

- A tblock is an **object**.
- The base address of a tblock is the object's **id**. This id can be retrieved using the built-in function `id()`.
- The type of a tblock is the object's **type**. This type can be retrieved using the built-in function `type()`.
- The data in the block of memory represented by a tblock is the object's **value**.

**Facts about types in Python**

All data in a Python program is represented by objects.

The type of an object is itself an object, a **type object**, whose type is the special **`type` object**. Be aware not to confuse _a_ type object with _the_ `type` object. _A_ type object is any object whose type is _the_ `type` object. The `type` object is special because it itself is its own type. Thus the `type` object is also a type object. See the [[python_types.svg]] diagram.



---

Sources:

- Talk _Static Typing in Python_ by Dustin Ingram at PyCon US 2020 ([link to video](https://www.youtube.com/watch?v=ST33zDM9vOE))