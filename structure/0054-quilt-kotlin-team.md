## Summary

The Quilt Kotlin Team will be responsible for developing and maintaining the
Quilt Kotlin Libraries (Henceforth QKL). QKL is set of libraries, built off of
QSL, intended for use in the Kotlin programming language.


## Motivation

Giving developers an official way to use Kotlin on Quilt, as well as providing
wrappers for QSL and the Minecraft codebase for easier use in Kotlin.


## Explanation

The Quilt Kotlin Team will be responsible for reviewing pull requests,
designating new modules, developing, and maintaining QKL. It falls under RFC
0018 (Technical Teams), and will follow the same standards defined in the RFC


## Drawbacks

### Versioning

Versioning could be a hurdle, as QSL, Quilted FAPI, and Loader all use
different versions, and having a fourth could be confusing for the end user.


## Rationale and Alternatives

### Subdivision of QSL

QKL shouldn't be a subdivision of QSL, as QSL is based on Java, it'd be
difficult for maintainers who don't know Kotlin to be able to maintain QKL.
Having QKL as a subdivision of QSL would mean that changes in the respective
QSL modules would have to be copied to the QKL module before merging. This is a
bad idea, because it forces everyone to know Kotlin to be able to contribute.


### Kotlin CHASM frontend

Once CHASM is added to loader, QKT can start work on a CHASM frontend for
Kotlin, which is one of the main reasons for this RFC; A CHASM frontend is more
than would normally be in a language adapter, which means QKL will need more
special treatment.


### But Kotlin has Java interop?

Kotlin's interoperability with Java is just that, interoperability. There are
some concepts that work well in Java, but not Kotlin, and vice versa. Extension
functions and DSLs don't exist in Java, and with this we'd provide
quintessential ones for modding in Kotlin.


### Non-QSL/Minecraft libraries

Non-QSL/Minecraft (Third Party) libraries won't need wrappers for Kotlin, as
they are not the responsibility of Quilt or QKT. Wrapping Third Party libraries
would also cause some sort of favoritism to be seen in the project, and we
don't want to show favoritism for Third Party projects


## Prior Art

Fabric Language Kotlin is a library for using Kotlin on the Fabirc platform.
It only provides the Kotlin stdlib, and Kotlinx libraries, while QKL will also
provide wrappers for QSL and the Minecraft codebase