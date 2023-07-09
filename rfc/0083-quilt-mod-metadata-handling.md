# Quilt mod metadata handling

## Summary

This RFC specifies how the metadata of Quilt mods should be handled by its own toolchain, with the focus being on loader, build system and third-parties in the ecosystem.


## Motivation

While the handling of Quilt mod metadata has enjoyed its informality for quite a while, with `quilt.mod.json` and its specification being considered the definitive sources of truth, the plans to enhance the development experience by introducing a JSON5-based variant of the Quilt mod metadata (`quilt.mod.json5`) that must be converted into its JSON-based sibling means that further clarification about how the Quilt toolchain handles mod metadata and how the Quilt ecosystem should handle it will be needed.


## Explanation

The file representing the Quilt mod metadata is `quilt.mod.json`, a file written in [the JSON format](http://www.rfc-editor.org/info/rfc7159) present at the root of the mod's JAR file that follows the [`quilt.mod.json` specification](../specification/0002-quilt.mod.json.md). All published Quilt mods that are to be loaded by Quilt Loader itself and not a loader plugin must have this file present, with the failure of having it present making the metadata invalid and the mod itself unreadable. This means that any tooling that needs to read Quilt mod metadata must read from `quilt.mod.json` while following its specification. 

For the convenience of developers, the Quilt mod metadata can also be represented by a `quilt.mod.json5` file, a file written in [the JSON5 format](https://spec.json5.org/) that also follows the `quilt.mod.json` specification. Unlike its JSON-based sibling, the `quilt.mod.json5` file is expected to only be utilized in a development environment by the loader and the build system, with the end goal being the conversion from `quilt.mod.json5` to `quilt.mod.json` by the build system.

Loaders, build systems, launchers, and other tools that require a Quilt's mod metadata must unconditionally handle the `quilt.mod.json` file.

Loaders and build systems may support handling `quilt.mod.json5`, and if supported, it must reject its coexistence with a `quilt.mod.json` file in order to prevent unintended behavior and it must only handle `quilt.mod.json5` files within a development context. This means that while a mod loader may parse `quilt.mod.json5` files inside a development environment, it must reject that handling if such file is found on a production environment, and build systems must always convert `quilt.mod.json5` files into `quilt.mod.json` ones during the process of building a Quilt mod.


## Drawbacks

The JSON5 format is a much less ubiqutous format than JSON, which reflects into how the toolchain and the ecosystem may handle them. Considering Minecraft's usage of JSON for both game data and launcher metadata and its absence of JSON5 usage, it is reasonable to expect that the handling of `quilt.mod.json` files won't require as much effort than the handling of `quilt.mod.json5`, and while the toolchain already handles the lack of support for the JSON5 format through the [Quilt Parsers]() library, it is unreasonable to expect tooling such as launchers and mod scanners to go for an extra dependency that may not even exist within the utilized framework.


## Rationale and Alternatives

The usage of `quilt.mod.json5` as the primary representative of Quilt mod metadata has already been considered with the past, with the toolchain's tooling for parsing JSON5 being built with the handling of such files in mind. However, considering that third-parties may struggle to handle those files compared to `quilt.mod.json`, it has been decided to go with `quilt.mod.json` as the Quilt mod metadata file.

Eventually, the limited handling of `quilt.mod.json5` has been introduced for the sake of developer's convenience, due to the format's support for comments and due to it eliminating a lot of boilerplate that a developer may face, as well as allowing for certain actions to be better performed without any struggle. However, it must be contained to a development context, considering that third-party tooling isn't expected to handle those files.
