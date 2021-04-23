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
based on configuration, data, or other sources. To reflect these very disparate
needs, this RFC defines separate methods for generating static and dynamic
data.

### Static Generation

Compile-time data generation will be integrated with Quilt's Gradle plugin,
defining a datagen block as such:

```groovy
datagen {
    package "com.example.mymod.registry"
    namespace "mymod" //optional, inferred from archivesBaseName if not specified
    generator { consumer -> //optional
        <...> 
    }
}
```

This flags the `com.example.mymod.registry` package to be scanned by an
annotation processor for fields annotated to generate common templates, such as
common blockstate JSONs, basic block/item models, recipes, recipe advancements,
tags, and any other necessary data.

For data generation that's too complicated for annotations to define, the
`generator` closure allows programmatic definition of JSON objects which can
then be passed to `consumer` to be added as a JSON file in the mod's
resources.


### Dynamic Generation

Dynamic data generation is performed through implementors of
`DynamicDataGenerator`, which are registered to `ClientDataGenManager` for
resource packs and `ServerDataGenManager` for data packs. A 
`DynamicDataGenerator` defines both a method invoked during resource reload and
a method that returns a state object as JSON. The `onReload` method will pass
path and content strings to a file writer, which will place those files in a
`quilt_generated` folder in the base game directory. The state object will
be written as well, and compared to whenever a resource reload is triggered.
If the current state and the saved state are different, the resources will be
regenerated, allowing for reliable caching.

## Drawbacks

While the package structure for storing registration classes in a single
directory is a loose standard, it goes directly against the package format that
Minecraft has used in effectively every set of mappings, and I'm not sure of
a better way to specify.

## Rationale and Alternatives

Java, sadly, has very little capacity for compile-time metaprogramming;
statically-defined annotations are the only method of generating files based
on Java source while compiling without messing around with classpaths. As
Minecraft modding already uses Gradle for its build system, processing
annotations with Gradle for easily-defined data like basic models or block
drops removes a significant amount of data generation work. A gradle task for
additional complex generation is the best compile-time option unless we want
to significantly complicate the build process.

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

- Are there alternatives to annotations and Gradle scripts that can be better
integrated with mod code?


## Expected Response

This will likely obsolete other dynamic data generation tools, so it needs to
be a concrete improvement over them. Allowing existing solutions to use the cache system may also help.