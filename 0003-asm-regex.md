# DISCLAIMER: Scope
ASMR is a massive undertaking, and the author believes the only way that the development can be productive is if the project is approached in small pieces.

This RFC is meant to define ASMR at an **extremely high level**. This RFC only intends to define *what* ASMR is, and it does not intend to define any strict specification on how anything will actually work. The examples are meant to be small snippets of what ASMR might look like, not a carved-in-stone requirement or even approval on how the small details will work.

Each section under the Explanation category will have its own RFC to define its actual file format, API structure, etc.


# Summary

This document defines the four major sections of ASM Regex, Quilt's own bytecode transformation library, and provides general guidelines for consideration when writing future RFCs and prototyping ASMR.

# Motivation

API visibility has been a struggle for Fabric modders since its inception. Since no way is available for an API to make changes to Minecraft code that are visible at compile-time, modders are expected to [just know](https://github.com/FabricMC/fabric/blob/1.16/fabric-object-builder-api-v1/src/main/java/net/fabricmc/fabric/api/object/builder/v1/entity/FabricEntityTypeBuilder.java) that a specific extension exists. In extreme cases (such as the Fabric Rendering API), there may be an API to allow a mod to  [*completely replace*](https://github.com/FabricMC/fabric/pull/1182) an existing Minecraft construct, with no indication to the modder that this could happen!

Additionally, there is no way in existing bytecode manipulation libraries (besides raw ASM) for pattern-matching--Mixins are difficult to use for even targetting the same pattern where all usages are known

# Explanation
ASMR comes in four major parts: the [Frontend](#the-compiler), the [Compiler](#the-compiler), the [Backend](#the-backend), and the [Processor](#the-processor).
The Compiler is responsible for reading the high-level metadata ("the Frontend") and emitting low-level metadata ("the Backend"). The Processor is responsible for reading the Backend and actually transforming class files. They are discussed in further detail in their respective sections below.

ASMR is *not* intended to be restricted to only being used by Quilt--any mod could provide their own Compiler and Backend plugins with exactly the same privileges as default ASMR plugins.

## The Frontend
### Scope
The Frontend is only needed during the compilation process, not runtime; the [Backend](#the-backend) will be present in the final jar.
### Format
The Frontend is essentially a map of `plugin id` -> `metadata`, where the `metadata` is provided to the Compiler plugin associated with the plugin id.
Some extra metadata, called "global settings", can be created by ASMR as needed and specificed at the file or transformer level.
### <a name="frontend-example"></a> Rough Example
Since this is meant to be a quick summary, an example is worth a thousand words:
```json5
{
  "frontend_version": "1", // global setting showing the specification version
  "quilt:mixin": {
    // global settings for every transformer. These are only visible internally;
    // Compiler plugins can never define new global settings
    "required": "false",  // Marks if the transformer is required to be present at runtime,
                          // since any mod could provide a transformer
    // The actual body of the transformer's metadata (name TBD)
    // It can be anything, it is completely the Compiler plugin's responsibility to parse.
    "body": [
      // An annotation-based system may only need the classes
      // it needs to process in a simple array
      "com.example.mixin.MixinClassOne",
      "com.example.mixin.MixinClassTwo"
    ]
  },
  "quilt:access_widener": {
    // Other systems may only need metadata on what to target and how
    "body": {
      "field": {
        "net.minecraft.MinecraftClient::INSTANCE:Lnet/minecraft/MinecraftClient;": "accessible"
      }
    }
    // Or any combination of the two extremes.
  }
}
```
## The Compiler
### Scope
The Compiler runs as a Gradle plugin, and has access to the Frontend file(s) and full access to the compile-time classpath of the mod being compiled.
The Compiler's plugins are defined explicitly in the `build.gradle` file of the mod, except for the default plugins (`access_widener`, `mixin`, etc.)
### Capabilities
The Compiler's Plugins are very powerful. They can request a (read-only) `ClassNode` for any class they want from the mod's own classes or anything off of its compile classpath.

They act as a function of `List<metadata_in> -> List<metadata_out> `. Plugins only get their own metadata, determined by their plugin id, but they can output any amount of Backend transformer metadata they want.
For example, the `quilt:mixin` compiler plugin may output an assortment of `quilt:inject`, `quilt:redirect`, and `quilt:overwrite` transformers.
### Rough Example
N/A at this time, see the [Frontend](#frontend-example) and [Backend](#backend-example) sections for what the input and output of the Compiler may look like.

## The Backend
### Scope
Backend files are put in the compiled jar of each mod. All mods have their backends processed at runtime, but at import-time (i.e. the time jar processors run) each mod defines in their `build.gradle` which dependencies they want to have applied.
### Capabilities
Generally, the Backend format is very similar to the Frontend. It is a map of `transformer id` -> (`transformer-specific metadata` + `globally-defined metadata`). The main difference is that the Backend has significantly more globally defined flags, to allow transformation to be as deterministic as possible.

Globally defined flags on transformers may include:
- `before_mod: "xyz"` - must apply before `xyz`'s backend, if it exists
- `after_mod: "xyz` - must apply after `xyz`'s backend, if it exists

These will be determined in the Backend RFC.

### Rough Example
This example is in JSON, but as per discussion in [Unresolved Questions](#backend-format) this may be a different, non-human-readable format.

```json5
{
  "version": "1",
  "compilerVersion": "1.0.0",
  // any other metadata
  
  // processors will be applied in order
  // you may duplicate them as many times as you like, but processors are encouraged to design their
  // metadata so that it is pooled where possible.
  "processors": [
    {
      // global flags go here
      "id": "quilt:inject",
      "before_mod": "flamingo",
      "body": {
        // any other metadata the injector wants can go here
        "method": "com.example.MyMixin::myInject(Ljava/lang/Object;)V",
        "target": "net.minecraft.client.MinecraftClient::getPlayerFromCtx(Ljava/lang/Object);L/net/minecraft/PlayerEntity;",
        "at": {
          "injector": "HEAD"
          // etc
        }
      }
    },
    {
      "id": "quilt:access_widener"
      // etc
    }
  ]
}
```

## The Processor
### Scope
The Processor runs both at import-time in Gradle (see: jar processors) and at class load during runtime. Each processor plugin is allowed to define if it wants to run only at runtime, only at import-time, or both.


It is undecided if processor plugins will be allowed to read classes besides the one it is currently processing, see the  [Unresolved Question](#backend-format) for the discussion on that.
### Capabilities
Processor plugins accept their metadata and a mutable ClassNode, along with information about the environment (server, client, or import-time merged)
They can choose to modify it in any way they see fit, or do nothing.

Whether processor plugins have to statically know their target classes (since in most cases this could be computed at compile-time) or are allowed to do mass ASM, is an [Unresolved Question](#assorted-questions) .

There will be utilities and abstractions available for Processor plugins to make their ASM safer and easier to program. The exact nature of these abstractions is TBD in the Processor RFC.

### <a name="backend-example"></a> Rough Example
N/A at this time, see the [Backend example](#backend-example) sections for what the metadata input of a plugin may look like.

# Drawbacks

This RFC, if accepted, will increase the complexity of creating Quilt mods, since they will be required to learn the various Compiler plugins, most of which will not have close equivalents in other modding systems.

Since this is not prototyped, but just an idea, it may be discovered that this system is flawed and we will have to go back to the drawing board.

# Rationale and Alternatives

TODO

# Prior Art

Prior art is mostly asie's [Ponderings on a post-Mixin world](https://github.com/FabricMC/fabric-loader/issues/244) thread, with additional inspiration from [Mixin](https://github.com/spongepowered/mixin) and [CoreMods](https://github.com/MinecraftForge/CoreMods).

# Unresolved Questions
## Pre-Merge

### Backend Format
If the Processor is sandboxed from other classes besides the one it is processing, do we use JSON for the backend and write our own bytecode serialization (sounds really slow), or use our own format (still slow).

On the other hand, having a system for reading classes without loading them would be restrictive and potentially error-prone, and would encourage the anti-pattern of extensive processing of the input classes in the Processor instead of the Compiler
### Assorted questions:
- **Annotation Processors**. Is it even needed? How would that work?
- **The Processor name**. I'm not sure I like this name. Better ideas are welcome
- **Processor Scope clarifications**. Should the Processor plugin be able to read ClassNodes it wants to use as reference (i.e. copying over method bodies; additional verification) or should all of that be required to handled in the Compiler?
- **Mass ASM**. Can processor plugins perform mass ASM? Perhaps this would be allowed in the compiler instead, or not at all.
## Post-Merge
- Everything in this RFC needs to be expanded upon and formally defined in further RFCs
- Do Processor plugins get a ClassNode or an abstraction over a ClassNode?

# Expected Response
Coremodding is a difficult subject, and it is expected for opinions on ASMR to be mixed at best. However, overall, I believe that Quilt can build an API that is significantly more pleasant to use than the currently available alternatives.
