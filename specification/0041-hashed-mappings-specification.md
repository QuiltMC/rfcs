# Hashed Mapping Specification

## Summary

This document intends to revise and formalize the process for creating a
hashed mapping as originally proposed in the hashed mojmap [RFC 19](https://github.com/QuiltMC/rfcs/blob/master/rfc/0019-hashed-mojmap.md).

## Motivation

Hashed Mojmap is intended to replace intermediary as our runtime mappings.
It provides some benefits over intermediary, most notably the reduced maintenance cost.

One of the design goals of hashed was, that it is not built incrementally,
but can always be created from an existing official mapping set without knowledge of earlier hashed versions.
This also means that it should be possible for anyone to generate hashed mappings,
even if they don't plan to use the mappings-hasher designed by Quilt.
In order to allow this, a specification for the hashed mapping is needed.

In order to specify our intended behaviour, this document describes the process to generate a hashed mapping.
Anything not specified in this RFC is considered a bug in the generation of hashed mappings.

## Explanation

Given an original mappings set and the corresponding classes, a new mapping set is created with the following properties:
- All new mappings are a hash of the corresponding original mapping.
- All new mappings are unique in the new mapping set where possible.
- The new mappings must ensure that the resulting remapped jar can be verified and executed.
- The new mapping set should be as resilient to changes in the original mapping set and corresponding classes as possible.

In order to achieve this, the following operations are performed on the original mapping set:
- Convert all class/method/field names into their corresponding "raw name".
- Ensure methods in the same "override hierarchy" have the same raw name. (See "Method Name Sets".)
- Hash all raw names using the same algorithm.
- Mark all hashed class/method/field names with a corresponding prefix.
- Prefix hashed class mappings with a default package.
- Strip any mapping information that is unnecessary.

### Raw Names
For classes, the raw name is their binary name with the package omitted where possible.
The package of a class can be omitted if and only if the original simple class name (i.e. the original class name without the package) is unique in the original mapping set.

For methods and fields, the raw name is the raw name of the owner class, followed by their original name and descriptor, unless the descriptor can be omitted.
The descriptor of a method/field can be omitted if and only if the original method/field name is unique in its owner class.

The exact format is:
- Class: `<class_name>`
- Method: `m;<raw_class_name>.<original_method_name>;<original_descriptor>`
- Field: `f;<raw_class_name>.<original_field_name>;<original_descriptor>`

### Method Name Sets
All methods in a name set are required to have the same raw name.
Method A and method B are in the same name set if and only if one of the following is true:
- A overrides B.
- B overrides A.
- There exists a method C that is both in the same name set as A and in the same name set as B.

> "A overrides B" means that the owner of B is a superclass or superinterface of the owner of A and that A "can override" B according to `JVMS 4.5.4`.

The raw name each set uses is the lexicographically lowest raw name of all methods in the name set that don't override another method.

### Hash function
The raw names are hashed using the SHA-256 hash algorithm.
The resulting hash is converted into base-26 (a-z).
The least significant 8 base-26 digits are then used as the hash, most significant digit first.

### Prefix
The following prefixes are used:
- Class: `C_`
- Method: `m_`
- Field: `f_`

### Default Package
All hashed class names are prefixed with a default package.
The default package for hashed mojmap is `net/minecraft/unmapped/`.

### Omitted mapping information
The mapping of a hashed name is omitted if one of the following is true:
- The corresponding mapping in the original mapping set is an identity mapping AND the original mapping is longer than one letter.
- The method corresponding to the method mapping overrides another method in the mapping set.

## Example
The following example is intended to illustrate the process described above, but is not to be viewed as specification.

Note: In this example `ClassB` extends `ClassA` and implements `InterfaceA`.
| Java Name                             | Raw Name                      | Hashed Name   |
| ------------------------------------- | ----------------------------- | ------------- |
| `org.example.ClassA`                  | `ClassA`                      | `C_fshauzuk`  |
| `org.example.ClassA.InnerClass`       | `ClassA$InnerClass`           | `C_zldtvwcl`  |
| `org.example.ClassA.fieldA`           | `f;ClassA.fieldA;`            | `f_xskwwtwz`  |
| `org.example.ClassA.methodA()`        | `m;ClassA.methodA;`           | `m_onqxeppm`  |
| `org.example.ClassB`                  | `ClassB`                      | `C_irypywpy`  |
| `org.example.ClassB.InnerClass`       | `ClassB$InnerClass`           | `C_znuawieq`  |
| `org.example.ClassB.methodA()`        | `m;ClassA.methodA;`           | `m_onqxeppm`  |
| `org.example.ClassB.methodB()`        | `m;InterfaceA.methodB;()V`    | `m_reinsnrb`  |
| `org.example.InterfaceA`              | `InterfaceA`                  | `C_wtvyrzif`  |
| `org.example.InterfaceA.methodA()`    | `m;ClassA.methodA;`           | `m_onqxeppm`  |
| `org.example.InterfaceA.methodB()`    | `m;InterfaceA.methodB;()V`    | `m_reinsnrb`  |
| `org.example.InterfaceA.methodB(int)` | `m;InterfaceA.methodB;(I)V`   | `m_mkznfdir`  |
| `org.example.packageA.ClassC`         | `org/example/packageA/ClassC` | `C_hehlvtlo`  |
| `org.example.packageB.ClassC`         | `org/example/packageB/ClassC` | `C_rvxxwsev`  |

## Drawbacks

- This version of hashed is potentially still less stable than intermediary, although this has not been quantified.
  - An "ideal" intermediary would be perfectly stable.
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
- Unique hashed names provide several benefits:
  - Easier deobfuscation of stacktraces and other errors (especially when using bots for manual translation).
  - Allow source remapping from hashed to any other mapping without needing context (no classpath or even full class files needed).
  - Un-hides fields.
  - Basically no leaking of mojmap.
    - Makes it easier to argue about mojmap license.
    - Would allow using it in "taint free" contexts if it ever comes to it (e.g. yarn)
- Detection of unobfuscated names is now done by checking whether mappings are identity mappings.
  - Detection via annotations was attempted, but required too many special cases (constructors, enum values array, etc.)
  - Single character names in the original mapping have a chance of coinciding with the obfuscated names.
    - We decided to treat all single character identity mappings as obfuscated

## Prior Art

The first attempt at hashed mappings was outlined in [RFC 19](https://github.com/QuiltMC/rfcs/blob/master/rfc/0019-hashed-mojmap.md).
This was then implemented in the [mappings-hasher](https://github.com/QuiltMC/mappings-hasher).

Later, an attempt was made to create a [simplified hasher](https://github.com/QuiltMC/mappings-hasher/tree/archive/simplified_hasher),
by only hashing the mappings without information of the jar.
However, this idea was scrapped due to the drawbacks of non-unique mapping names.

## Unresolved Questions

- Some of our current tooling might not be fully capable of dealing with these new mappings.
  However, this is to be fixed in the corresponding tooling, rather than the hashed mappings.

- While this simple format seems to work fine, no extensive testing has been done with the rest of the toolchain.
  Hashed Mojmap will remain on the snapshot maven until we are confident that the tooling works and this RFC is merged.
