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
0018 (Technical Teams), and will follow the same standards definedin the RFC


## Drawbacks

TBD


## Rationale and Alternatives

### Subdivision of QSL

QKL shouldn't be a subdivision of QSL, as QSL is based on Java, it'd be
difficult for maintainers who don't know Kotlin to be able to maintain QKL.
Having QKL as a subdivision of QSL would mean that changes in the respective
QSL modules would have to be copied to the QKL module before merging. This is a
bad idea, because it forces everyone to know Kotlin to be able to contribute.


## Prior Art

Fabric Language Kotlin is a library for using Kotlin on the Fabirc platform.
It only provides the Kotlin stdlib, and Kotlinx libraries, while QKL will also
provide wrappers for QSL and the Minecraft codebase


## Unresolved Questions

- Would we provide wrappers for other libraries seperate from QSL?
- What modules should be in QKL?