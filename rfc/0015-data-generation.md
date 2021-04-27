## Summary

This document defines a scheme for supporting both static and dynamic data
generation, so that modders can spend less time writing JSONs while still
keeping the advantages of statically-defined data when dynamic generation
is unnecessary.

## Motivation

Since the beginning of The Datapack Era, modders have consistently expressed
frustration with the amount of JSON files that need to be written. A standard
block will require:

- A blockstate JSON to define what models to use
- Separate model JSONs for each permutation of visual change
- A separate model JSON for the item model
- A separate JSON for each recipe to obtain the block
- A JSON for each tag the block belongs to
- A JSON for each tag the *item form* of the block belongs to
- A JSON for unlocking the block's recipe through an advancement
- A JSON for defining the block drop loot table, even if it just drops itself

While there are multiple programns that will help generate various forms of
data (see Prior Art), none of them are integrated with the standard Gradle
build system or mod code. This means that if you decide to generate data after
you've already defined a dozen blocks and items, you need to copy out that list
of already-defined items by hand to generate data, and do it again every time
you add new content.

There are also scenarios in which data statically defined at compile time
is insufficient for the purposes a mod is trying to fulfill. Take, for example,
the mod [Artis](https://www.curseforge.com/minecraft/mc-mods/artis), which
lets pack developers define custonm workbenches of arbitrary size without
needing to write a mod. There is no way of knowing what tables will be defined
at compile time, so we can't possibly provide data satisfactorally. However,
current runtime data generation tools do not cache their outputs, and therefore
run every time the game is launched or resources are reloaded. For a mod with
very few things to generate, this is not a problem, but when sizable mods like
[Modern Industrialization](https://github.com/AztechMC/Modern-Industrialization)
generate nearly all of their block and item models at reload time, this can
result in significantly impacted load times, even if the data could be defined
statically with no issue.

## Explanation

Data generation can easily be separated into *static* and *dynamic* generation.
Static generation is not expected to change between launches or reloads; only
between separate releases of a mod. Dynamic generation can change arbitrarily
based on configuration, data, or other sources.

Both static and dynamic generators will write to a `PackWriter` passed to the
datagen method, which will manage loading of data and conversion to hard files.
`PackWriter` contains methods to accept JSON objects, strings, or byte arrays
to write to packs.

### Static Generation

Static data generation is performed in the `datagen` source set. `datagen` has
access to `main`, as well as all dependencies accessible by `main`. Any methods
with no return and accepting one `PackWriter` annotated by a `@DataGen`
annotation will be invoked during build, with results written to hard viles.
The `@DataGen` annotation requires an argument of `DataType.CLIENT_ASSETS` or
`DataType.SERVER_DATA` to specify whether to write to `assets` or `data`. Any
files added via datagen will be ignored if a file with that namespace and path
in that type of data exists as a hard file in `src/main/resources`.

### Dynamic Generation

Dynamic data generation is performed through implementors of
`DynamicDataGenerator`, which are registered to `ClientDataGenManager` for
resource packs and `ServerDataGenManager` for data packs. A 
`DynamicDataGenerator` defines both a method invoked during resource reload and
a method that returns a state object as JSON. The `onReload` method will pass
path and content strings to a `PackWriter`, which will place those files in a
`quilt_generated` folder in the base game directory. The state object will
be written as well, and compared to whenever a resource reload is triggered.
If the current state and the saved state are different, the resources will be
regenerated, allowing for reliable caching.

## Drawbacks

- Getting multiple source sets, especially arbitrary source sets, to play nice
with IDEs can be challenging. This would require work in Quilt Gradle to ensure
that source sets are executed on any build, whether it's to test or to release.
- In order to properly bootstrap the registries necessary for datagen, it may
be necessary to launch the Minecraft client during build tasks in order to fire
entrrypoints properly. I am not currently aware of a workaround.
- The "datagen" name on the source set may be confusing to end users, implying
that *all* data generation must be done in that source set. I cannot currently
think of another succinct alternative.

## Rationale and Alternatives

Java, sadly, has very little capacity for compile-time metaprogramming,
making the task of generating files based on Java code difficult. Using a
separate source set ensures that Java-written data generators can be used in
both compile time and runtime, reducing code duplication.

A previous version of this RFC proposed a system using annotations and Groovy
closures to generate data.

## Prior Art

There are multiple existing projects designed around runtime data generation in
the existing Fabric ecosystem, including:

- [Artifice](https://github.com/natanfudge/artifice)
- [Runtime Resource Pack](https://github.com/Devan-Kerman/ARRP)

While these are designed to handle dynamic runtime data generation, they have
been repeatedly used in place of statically generating JSON files, which leads
to various issues, including higher load times and lower transparency to the
end user about what data is being added. There are also a few external
programs for static data generation:

- [Json Factory](https://github.com/CottonMC/json-factory)
- [mcgen](https://github.com/emilyalexandra/mcgen)
- [Destruc7i0n's recipe generator](https://crafting.thedestruc7i0n.ca/)
- [Minecraft.tools](https://minecraft.tools/)
- [Blockbench](https://blockbench.net/)

## Unresolved Questions

- Is there a better name for the `datagen` source set?
- Should any validation be done to ensure that generated data is usable?


## Expected Response

This will likely obsolete other dynamic data generation tools, so it needs to
be a concrete improvement over them. Allowing existing solutions to use the
cache system may also help.