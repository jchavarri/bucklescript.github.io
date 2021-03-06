---
title: Intro to External
---

Many parts of the interop system uses a concept called `external`, so we'll specially introduce it here.

`external` is a keyword for declaring a value in BuckleScript/OCaml/Reason:

```ocaml
external myCFunction : int -> string = "theCFunctionName"
```

Reason syntax:

```reason
external myCFunction : int => string = "theCFunctionName";
```

It's like a `let`, except that the body of an external is, as seen above, a string. That string usually has specific meanings depending on the context. For native OCaml, it usually points to a C function of that name. For BuckleScript, these externals are usually decorated with certain `[@bs.blabla]` attributes.

Once declared, you can use an `external` as a normal value.

BuckleScript `external`s are turned into the expected JS values, inlined into their callers during compilation, **and completely erased afterward**. You won't see them in the JS output. It's as if you wrote the generated JS code by hand! No performance cost either, naturally.

**Note**: do also use `external`s and the `[@bs.blabla]` attributes in the interface files. Otherwise the inlining won't happen.

## Special Identity External

One external worth mentioning is the following one:

```ocaml
external myShadyConversion : foo -> bar = "%identity"
```

Reason syntax:

```reason
external myShadyConversion : foo => bar = "%identity";
```

This is a final escape hatch which does nothing but converting from the type `foo` to `bar`. In the following sections, if you ever fail to write an `external`, you can fall back to using this one. But try not to.
