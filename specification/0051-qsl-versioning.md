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
This should not cover the case of a port as [the metadata](#metadata-content) already gives enough information.

> Build metadata MUST be ignored when determining version precedence. Thus two versions that differ only in the build metadata, have the same precedence.
>
> from [semver](https://semver.org/#spec-item-10)

This means that to ensure proper solving by Quilt loader, <abbr title="Quilt Standard Libraries">QSL</abbr> modules must depend on exactly one Minecraft version.

### Version qualifiers

Now, with the values of the main three version numbers out of the way, we have to carefully consider how we approach version qualifiers.

We will divide this section in different cases:

- __Stable release:__  
  stable releases should have no qualifiers, thus for the first ever release we would have `1.0.0` as the version string (ignoring [the metadata for now](#metadata-content)).
- __Beta release:__  
  a beta release is a release which is not yet stable, its unstability means any breaking change still can happen and the module/library is still under development.
  Though it also means the module/library is close to full release, it starts to stabilize: the goal is to detect any defects before actual release.
  Thus the version should be qualified as beta, betas are versioned separately with `-beta.<beta_number>`, `beta_number` starts at `1` and increases for every new beta of the designated release.
  Beta releases are not available through the stable <abbr title="Quilt Standard Libraries">QSL</abbr> Maven artifact, or through a stable library version, but they are available through the beta version of the artifact.
  A Beta artifact may depend on stable components if there's no betas for those components currently.
- __Experimental release:__  
  an experimental release is a release which is far from stable: the module/library is still highly under development and may change at any time.
  The purpose of experimental releases is when too much disagreements or uncertainty over an API happens but actual experimentations alongside modders has to be done to pinpoint details.
  This is the difference of a beta release, most details have already been pinpointed and are ready for adoption, while experimental releases highly need feedback on usability and usefulness.
  Thus the version should be qualified as experimental, experimental versions are versioned separately with `-SNAPSHOT` at the end, after [the metadata](#metadata-content) to allow publishing on the snapshot maven.
  Those versions are only available through the snapshot maven.

### Metadata content

The metadata should **always** contain the Minecraft version the <abbr title="Quilt Standard Libraries">QSL</abbr> module targets.
Thus if 22w11a is targetted, the version will contain `+22w11a`, if 1.18.2 is targetted the version will contain `+1.18.2`.

The targetted version should always be entirely written out without omitting elements,
as this will cause confusion and various issues that already were witnessed when using the `-SNAPSHOT` system, or in other libraries.

## Drawbacks

In the long run, having beta releases mean there will be at least 2 publishing channels for <abbr title="Quilt Standard Libraries">QSL</abbr>.
This is only an issue for the start of the project as dependency downloading is not a thing yet, so it will lead to possible confusion from users, but hopefully should not be too much of an issue and will be negligeable once dependency downloading is a thing.

## Rationale and Alternatives

- <abbr title="Quilt Standard Libraries">QSL</abbr> needs a versioning scheme to ensure consistent versioning, reduce confusion.
  - It also needs it for the its beta as it cannot infinitely use the `1.0.0-SNAPSHOT` version.
- Alternative is to do exactly like [Fabric API], but this might not be something we want as it still is at `0.x.x` while it has encountered several breaking changes.
  - It's also very confusing to work with when updating to a new Minecraft version, especially snapshots.
  - It doesn't have a defined way to handle experimental modules.

## Prior Art

This specification highly bases itself on [semver] as it's the only versioning scheme supported by [the <abbr title="quilt.mod.json">QMJ</abbr> mod manifest][QMJ].

[Fabric API] also served as prior art, though its versioning is not clearly defined, what is sure is the Minecraft version is present in the metadata, but it's slightly more arbitrary:
the metadata takes the targetted version and only keeps the major and minor numbers, disarding the patch number **unless** huge breaking changes happened in that patch.
In the case of a snapshot, the targetted version is the version the snapshot is for.

## Unresolved Questions

- Is depending on exactly one Minecraft version desirable?

<!-- URLs -->
[semver]: https://semver.org/ "Semantic Versioning 2.0.0 specification"
[Fabric API]: https://github.com/FabricMC/fabic "Fabric API repository"
[QMJ]: ./0002-quilt.mod.json.md

<!-- Notes -->
<!-- This document was written with <abbr> elements for abbreviation purposes, sadly the GitHub markdown viewer strips out the title attribute, meaning the elements do not work properly -->
