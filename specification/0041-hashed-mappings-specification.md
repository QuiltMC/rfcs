# Hashed Mapping Specification

## Summary

This document intends to revise and formalize the process for creating a
hashed mapping as originally proposed in the hashed mojmap [RFC 19](https://github.com/QuiltMC/rfcs/blob/master/rfc/0019-hashed-mojmap.md).

## Motivation

Since the original hashed mojmap RFC some things changed in the QuiltMC ecosystem.
Most notably, [RFC 33](https://github.com/QuiltMC/rfcs/blob/master/rfc/0033-quilt-mappings-and-clean-room.md)
drops the requirement to not "taint" contributors to quilt mappings.
As a result of this, we are allowed to leak more information from mojamp in its hashed version.

The new version of hashed mappings operates solely on the original mapping set
and therefore no longer requires inspecting minecraft libraries.
This greatly reduces the complexity of the mappings hasher, reducing the chance
of breakage and improving maintainability greatly.

In order to specify our intended behaviour, this document describes the process to generate a hashed mapping.
Anything not specified in this RFC is considered a bug in the generation of hashed mappings.

## Explanation

Given a mappings file, all class names, method names and field names will be hashed.

First, the names are simplified where possible:

- For top-level classes (i.e. not inner classes), the package is stripped from their name where possible.
  This is possible if and only if the stripped class name is unique withing the mapping set.
- For inner classes, the package is stripped from their name if and only if
  its enclosing top-level class has its package stripped as well.
- For methods and fields, the descriptor is always omitted.

The resulting names are then hashed and converted to base-26.
The hashing algorithm used is SHA-256.
The resulting digest is converted into base-26 (using the letters a-z), and the lowest 8 digits
make up the hashed name, least significant digit first.

This hashed name is then prefixed with two characters to distinguish between classes, methods and fields:
- For classes, the prefix is `C_`.
- For methods, the prefix is `m_`.
- For fields, the prefix is `f_`.

An attempt is made to avoid hashing non-obfuscated names:
If the original mapping is an identity mapping with a name longer than one character,
the hashed mapping will be the same identity mapping.

Lastly, top-level class names are prefixed with a package name to avoid name collisions and improve source browsing.
For hashed mojmap we chose `net/minecraft/unmapped`.

## Drawbacks

- This version of hashed is potentially still less stable than intermediary, although this has not be quantified.
  - An "ideal" intermediary would be perfectly stable.
- Identical names in the original mapping are identical in hashed, leaking information about the original mapping.
  - Thanks to dropping the clean room, this is much less of an issue now.
  - Actual information leakage is still reasonably small.
- Hashed base-26 names might be less readable than other name formats.
  - This is unfortunately hard to quantify. Base-10 hashing would be an alternative, although with longer hashes.
- A specification of the hashed mappings might make the format inflexible.
  - Changes in hashed already come with great effects, so any such change should probably go through the RFC process anyway.

## Rationale and Alternatives

- This version of hashed is most likely more stable than Mojmap.
  - Testing has shown that around 50% of class renames in Mojmap are only repackages,
    which are avoided thanks to package stripping.
- Hashed mappings can be fully automated, saving us the work of matching intermediary manually.
  - We've already evaluated that a fully automated system can update hashed within less than 10 minutes of a new release.
- When working with layered mappings (e. g. quilt mappings on top of hashed) it is preferable
  to be able to clearly distinguish between mapped and unmapped code.
  - Encountering Mojmap names when working with QM could be highly confusing.

## Prior Art

The first attempt at hashed mappings was outlined in [RFC 19](https://github.com/QuiltMC/rfcs/blob/master/rfc/0019-hashed-mojmap.md).
This was then implemented in the [mappings-hasher](https://github.com/QuiltMC/mappings-hasher)
prior to commit [385c9b8](https://github.com/QuiltMC/mappings-hasher/commit/385c9b8795ffcba2c5d21cdbd0f6a319bc7be10e).

However, the implementation proved to be somewhat complex and it was hard to argue about correctness of the implementation.
The JVMs complex method logic we needed to respect in order to ensure binary compatibility proved to be very hard to implement.

## Unresolved Questions

- Some of our current tooling might not be fully capable of dealing with these new mappings.
  However, this is to be fixed in the corresponding tooling, rather than the hashed mappings.

- While this simple format seems to work fine, no extensive testing has been done with the new platform.
  Unforeseen issues may still arise, which should ideally fixed before hashed is considered stable.
  It is unclear at this point when this transition should be made.
