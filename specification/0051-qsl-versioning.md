# Quilt Standard Libraries Versioning

## Summary

This document intends to specify the versioning scheme of the Quilt Standard Libraries to ensure consistent versioning, reduce confusion and address the case of experimental and beta versions.

## Motivation

As <abbr title="Quilt Standard Libraries">QSL</abbr> gets closer to a Beta release,
it is important for it to figure out a versioning scheme.

The versioning scheme will allow to ensure consistent version names, which reduce confusion among modders,
and would make version comparison more robust.

It is also important as it's a leading step in deciding how <abbr title="Quilt Standard Libraries">QSL</abbr> experimental or beta modules should be handled.

Up until now QSL has been lightly versioned as it was in active development without release, taking the form of `1.0.0+<mc_version>-SNAPSHOT`. This was only temporary.

## Explanation

First of all, the versioning scheme is based of [semver] since it's mendatory per [the <abbr title="quilt.mod.json">QMJ</abbr> mod manifest specification][QMJ].

This answers already a lot of questions, each version take the form of a `major.minor.patch` version, with the possibility of a metadata prefixed by `+`, and version qualifiers like `-beta`.

If you don't know what is talked about with modules or libraries, please refer yourself to the [<abbr title="Quilt Standard Libraries">QSL</abbr> Structure RFC](../rfc/0009-qsl-structure.md).
We will also use the word "root library" to design the Maven artifact that depends on every <abbr title="Quilt Standard Libraries">QSL</abbr> libraries.

### Values of `major`, `minor`, and `patch`

When a module or library is created, the `major` number should always be `1` at the start.
It mays increment in the case of breaking change, whether it is caused by the <abbr title="Quilt Standard Libraries">QSL</abbr> team or by an update to Minecraft.

If a module is updated with a breaking change, the library that includes it should also increment its `major` number.
Meaning that the root library version should also have its `major` number incremented.

The `minor` number should always start at `0`, and will increment every time a module/library is updated with backwards-compatible changes. It is reset when the `major` number changes.

The `patch` number should always start at `0`, and will increment every time a module/library is updated to fix bugs in a backwards-compatible manner.
This should not cover the case of a port as [the metadata](#metdata-content) already gives enough information.

### Version qualifier



### Metadata content

The metadata should **always** contain the Minecraft version the <abbr title="Quilt Standard Libraries">QSL</abbr> module targets.
Thus if 22w11a is targetted, the version will contain `+22w11a`, if 1.18.2 is targetted the version will contain `+1.18.2`.

The targetted version should always be entirely written out without omitting elements,
as this will cause confusion and various issues that already were witnessed when using the `-SNAPSHOT` system, or in other libraries.

## Drawbacks

## Rationale and Alternatives

## Prior Art

This specification highly bases itself on [semver] as it's the only versioning scheme supported by [the <abbr title="quilt.mod.json">QMJ</abbr> mod manifest][QMJ].

[Fabric API](https://github.com/FabricMC/fabric) also served as prior art, though its versioning is not clearly defined, what is sure is the Minecraft version is present in the metadata, but it's slightly more arbitrary:
the metadata takes the targetted version and only keeps the major and minor numbers, disarding the patch number **unless** huge breaking changes happened in that patch.
In the case of a snapshot, the targetted version is the version the snapshot is for.

## Unresolved Questions


<!-- URLs -->
[semver]: https://semver.org/ "Semantic Versioning 2.0.0 specification"
[QMJ]: ./0002-quilt.mod.json.md
