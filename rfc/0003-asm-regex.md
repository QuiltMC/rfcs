# DISCLAIMER: Scope
ASMR is a massive undertaking, and the author believes the only way that the development can be productive is if the project is approached in small pieces.

This RFC is meant to define ASMR through a **high level** overview of the project. This document only intends to define *what* ASMR is, and it does not intend to define any strict specification on how any given component will actually work.

Each section under the Explanation category will have its own RFC to define its actual file format, API structure, *etc*.


# Summary

One of the core goals of the Quilt project, and of the Fabric project before it, is to provide regular modders with the ability to alter game code without depending on first-party ready-made solutions, and without sacrificing mod compatibility. This RFC intends to introduce a new general-purpose bytecode manipulation library - codenamed "ASM-Regex" - that would fulfill those goals while offering new capabilities that the Mixin framework presently cannot. That framework would be able to modify Minecraft and mod jars both at runtime, and at import-time in the same manner as Jar Processors.

This document defines the four major sections of ASM Regex, Quilt's own bytecode transformation library, and provides general guidelines for consideration when writing future RFCs and prototyping ASMR.

# Motivation

When the Fabric project was first started, a choice was made to use the Mixin framework to provide bytecode modification capabilities. This choice had the benefit of fulfilling the outlined goals of the project to an acceptable extent without delaying launch. Mixin has served its purpose admirably, however as it got used for a variety of mods and libraries, mod developers started to notice some limitations:

- Many modders could benefit from more advanced injection/patching strategies. For example:
  - Stacking redirects
  - Force conditional jumps to go a certain way
  - Target every invocation of a specific method, or every access to a specific field
  - Adding to `enum` classes
  - Extract a slice of code from a large method into another method that can be called from outside, and keep other injections made to this slice
- Mixin's safeguards make it difficult to patch it in a way where certain mixins are pre-applied to the development  environment. This, while not useful functionality for an abstraction layer, would be great to have for viewing code in an environment with  large amounts of patches one directly interacts with.
  - Notably, since no way is available for an API to make changes to Minecraft code that are visible at compile-time, modders are expected to [just know](https://github.com/FabricMC/fabric/blob/1.16/fabric-object-builder-api-v1/src/main/java/net/fabricmc/fabric/api/object/builder/v1/entity/FabricEntityTypeBuilder.java) that a specific extension exists. In extreme cases (such as the Fabric Rendering API), there may be an API to allow a mod to  [*completely replace*](https://github.com/FabricMC/fabric/pull/1182) an existing Minecraft construct, with no indication to the modder that this could happen.
- Large modpacks can see lengthened loading times due to the amount of bytecode transformations being applied at every launch. A new system that allows static application could shorten loading times through use of caching.
- A lot of non-standard kludges were used to make Mixin work in the Fabric context. This carries both a maintenance cost and a risk of  divergence from upstream.
- New features that cannot be implemented through Mixin - such as Access Wideners - have to be added directly to quilt-loader, outside of the Mixin library. This division means several bytecode altering systems have to be maintained concurrently, and may create added fragility.

This proposal is based on asie's [Ponderings on a post-Mixin world](https://github.com/FabricMC/fabric-loader/issues/244) thread.

# Explanation
ASMR comes in four major parts: the [Frontend](#the-compiler), the [Compiler](#the-compiler), [Transformers](#transformers), and the [Processor](#the-processor).

Specifically, the ASMR project will need to implement two key kinds of components:

- **Processor** - a backend library which applies *transformers* to the application's bytecode.
- **Compiler** - a piece of software which can take in some form of input (annotated classes, domain-specific language, etc.)  and turn them into *transformers* usable by the processor.

A typical workflow could look like this:

![ASMR diagram](0003-assets/ASMR.svg)

The **Compiler** is responsible for reading the **Frontend** as high-level metadata and emitting **Transformers** as low-level classes loadable by the JVM. The **Processor** is responsible for loading **Transformers** and applying their transformations to the classpath. Those individual parts are discussed in further detail in their respective sections below.

ASMR is *not* intended to be restricted to only being used by Quilt - any mod could provide their own Compiler and Backend plugins with exactly the same privileges as default ASMR plugins.

## The Frontend
### Scope
The Frontend is only needed during the compilation process, not runtime; only [Transformers](#transformers) will be present in the final jar.
### Format
The Frontend is essentially a map of `plugin id` -> `metadata`, where the `metadata` is provided to the Compiler plugin associated with the plugin id.
Some extra metadata, called "global settings", can be created by ASMR as needed and specified at the file or transformer level.

## Compilers
### Scope
Each Compiler runs as a Gradle plugin, and has access to the Frontend file(s) and full access to the compile-time classpath of the mod being compiled.
Compilers are defined explicitly in the `build.gradle` file of the mod, except for the default plugins (`access_widener`, `mixin`, etc.), which are implicitly added in every environment.

### Capabilities
Compilers are very powerful. They take Frontend data in, and output a list of transformers. Each compiler reads its own metadata, and can output any amount of transformer class files.
For example, the `quilt:sponge_mixin` compiler may output an assortment of transformers based on each Mixin class.

### Transformers
#### Scope
Transformers are put in the compiled jar of each mod. All mods have their backends processed at runtime, but at import-time (*i.e.* the time jar processors run) each mod defines in their `build.gradle` which dependencies they want to have applied.
### The Processor
#### Scope
The Processor runs both at import-time in Gradle (see: jar processors) and at class load during runtime.

#### Capabilities
There will be utilities and abstractions available for Processor plugins to make their ASM safer and easier to program. The exact nature of these abstractions is TBD in the Processor RFC.

## Drawbacks

This RFC, if accepted, may increase the complexity of creating Quilt mods, as mod developers will be required to learn at least one of the various Compiler plugins. A Mixin Frontend will be provided at release with the intent of being compatible with most existing mods, but some Mixin features such as mixin plugins and int priorities will eventually not be supported.

## Rationale and Alternatives

ASM-Regex is designed to enable creative modders to create their own APIs and modifications that would not be possible under other mod loaders. However, this is a massive undertaking and will take a long time to refine, and QSL will most likely have to be migrated to ASMR after it has already begun development.


A Mixin fork could be attempted, but the amount of refactoring required to obtain similar features to the project outlined here would be prohibitive. It would also be difficult to try and force Mixin to submit to ideas unwanted by the Sponge leadership.


One last alternative is to keep the Status Quo, redirecting development efforts towards other projects. It seems however that undertaking this project would prove its value in the long run, improving mod quality and the health of the Quilt environment as a whole.

## Prior Art

- [Mixin](https://github.com/spongepowered/mixin) 
- [CoreMods](https://github.com/MinecraftForge/CoreMods)

## Unresolved Questions

- Two more RFC documents will have to be proposed, respectively for the processor and the compiler.

## Expected Response
Coremodding is a difficult subject, and it is expected for opinions on ASMR to be mixed at best. However, it appears that Quilt can build an API that is significantly more pleasant to use than the currently available alternatives.

