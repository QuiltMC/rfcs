# Summary
This document acts as a Request for Comments (RFC) on the topic of how a Quilt mod's metadata should be formatted. Such changes are intended to provide added value for mod developers, library developers, and users alike.

# Motivation
Since the toolchain is being forked, now is the opportune moment to make large scale changes to many of the systems mod developers use. This RFC describes a more flexible and powerful format for the mod file that defines what a mods definition should look like.

# Explanation
Below is an outline of all defined keys and values.

* [schema_version](#the-schema_version-field) — The schemaVersion to be used for reading this file
* [quilt_loader](#the-quilt_loader-field) — Information related to loading the mod
    * [group_id](#the-group_id-field) — The Maven group id
    * [id](#the-id-field) — The mod id
    * [provides](#the-provides-field) — Alternative mod ids provided by this mod
    * [version](#the-version-field) — The mods version
    * [entrypoints](#the-entrypoints-field) — Collection of entrypoints
    * [plugins](#the-plugins-field) — Collection of plugins
    * [jars](#the-jars-field) — Array of nested JARs to be loaded
    * [language_adapters](#the-language_adapters-field) — Array of language adapters
    * [depends](#the-depends-field) — Collection of mod dependencies
    * [breaks](#the-breaks-field) — Collection of mods that this mod is incompatible with
    * [repositories](#the-repositories-field) — Array of maven repositories
    * [metadata](#the-metadata-field) — Extra information about this mod and/or its authors
        * [name](#the-name-field) — Human-readable name of this mod
        * [description](#the-description-field) — Human-readable description of this mod
        * [contributors](#the-contributors-field) — Collection of contributors to this mod
        * [contact](#the-contact-field) — Collection of contact information
        * [license](#the-license-field) — One or more licenses this project is under
        * [icon](#the-icon-field) — The icon or icons associated with this project
* [mixin](#the-mixin-field) — Path(s) to mixin config file(s)
* [access_widener](#the-access_widener-field) — Path(s) to accesswidener file(s)
* [minecraft](#the-minecraft-field) - Minecraft related options
    * [environment](#the-environment-field) — What game environments this mod applies to
    
## The `schema_version` field
| Type   | Required |
|--------|----------|
| Number | True     |

The quilt mod file schema version to be used for parsing this file. Currently, the only valid version is 1.

## The `quilt_loader` field
| Type   | Required |
|--------|----------|
| Object | True     |

Information necessary for the mod loading process.

### The `group_id` field
| Type   | Required |
|--------|----------|
| String | True     |

A unique identifier for the organization behind or developers of the mod. See Maven's [guide to naming conventions](https://maven.apache.org/guides/mini/guide-naming-conventions.html).

### The `id` field
| Type   | Required |
|--------|----------|
| String | True     |

A unique identifier for the mod or library defined by this file, matching the `^[a-z][a-z0-9-_]{1,63}$` regular expression. Best practice is that mod ID's are in snake_case.

### The `provides` field
| Type         | Required |
|--------------|----------|
| Array/String | False     |

Either a single identifier or an array of identifiers that this mod also contains. These could be old or alternative mod ids. They should follow [the convention established for mod ids](#the-mod_id-field).

### The `version` field
| Type   | Required |
|--------|----------|
| String | True     |

Must conform to the [Semantic Versioning 2.0.0 specification](https://semver.org/). In a development environment, the value `${version}` can be used as a placeholder by quilt-gradle to be replaced on building the resulting JAR.

### The `entrypoints` field
| Type   | Required |
|--------|----------|
| Object | False    |

A collection of `key: value` pairs, where each key is the type of the entrypoints specified and each values is either a single entrypoint or an array of entrypoints. An entrypoint is an object with the following keys:
* adapter — Language adapter to use for this entrypoint
* value — Points to an implementation of the entrypoint. For the default JVM language adapter, this can be in either of the following forms:
    * `my.package.MyClass` — A class to be instantiated and used
    * `my.package.MyClass::thing` — A static field containing an instance of the entrypoint or a method handle for entrypoints that are functional interfaces

If an entrypoint does not need to specify a language adapter other than the default language adapter, the entrypoint can be represented simply as the value string instead.


### The `plugins` field
| Type   | Required |
|--------|----------|
| Array  | False    |

An array of loader plugins. A plugin is an object with the following keys:
* adapter — Language adapter to use for this plugin
* value — Points to an implementation of the `LoaderPlugin` interface. Can be in either of the following forms:
    * `my.package.MyClass` — A class to be instantiated and used
    * `my.package.MyClass::thing` — A static field containing an instance of a `LoaderPlugin`

If a plugin does not need to specify a language adapter other than the default language adapter, the plugin can be represented simply as the value string instead.

### The `jars` field
| Type   | Required |
|--------|----------|
| Array  | False    |

A list of paths to nested JAR files to load, relative to the root directory inside of the mods JAR.

### The `language_adapters` field
| Type   | Required |
|--------|----------|
| Object | False    |

A collection of `key: value` pairs, where each key is the namespace of a language adapter and the value is an implementation of the `LanguageAdapter` interface.

### The `depends` field
| Type   | Required |
|--------|----------|
| Array  | False    |

An array of [dependency object](#dependency-objects)s. Defines mods that this mod will not function without.

### The `breaks` field
| Type   | Required |
|--------|----------|
| Array  | False    |

An array of [dependency object](#dependency-objects)s. Defines mods that this mod either breaks or is broken by.

### The `repositories` field
| Type   | Required |
|--------|----------|
| Array  | False    |

A list of repository url strings where dependencies can be looked for in addition to Quilt's central maven repository.

### The `metadata` field 
| Type   | Required |
|--------|----------|
| Object | False    |

Optional metadata that can be used by mods to display information about the mods installed.

#### The `name` field
| Type   | Required |
|--------|----------|
| String | False    |

A human-readable name for this mod.

#### The `description` field
| Type   | Required |
|--------|----------|
| Object | False    |

A human-readable description of this mod. This description should be plain text, with the exception of line breaks, which can be represented with the newline character `\n`.

#### The `contributors` field
| Type   | Required |
|--------|----------|
| Object | False    |

A collection of `key: value` pairs denoting the persons or organizations that contributed to this project. The key should be the name of the person or organization, while the value can be either a string representing a single role or an array of strings each one representing a single role.

A role can be any valid string. The "Owner" role is defined as being the person(s) or organization in charge of the project.

#### The `contact` field
| Type   | Required |
|--------|----------|
| Object | False    |

A collection of `key: value` pairs denoting various contact information for the people behind this mod, with all values being strings. The following keys are officially defined, though mods can provide as many additional values as they wish:
* email — Valid e-mail address for the organization/developers
* homepage — Valid HTTP/HTTPS address for the project or the organization/developers behind it
* issues — Valid HTTP/HTTPS address for the project issue tracker
* sources — Valid HTTP/HTTPS address for a source code repository

#### The `license` field
| Type                | Required |
|---------------------|----------|
| Array/String/Object | False    |

The license or array of licenses this project operates under.

A license is defined as either an [SPDX identifier](https://spdx.org/licenses/) string or an object in the following form:
```json
{
    "name": "Perfectly Awesome License v1.0",
    "id": "PAL-1.0",
    "url": "https://theperfectlyawesomelicense.com/",
    "description": "This license does things and stuff and says that you can do things and stuff too!"
}
```
The `"description"` field is optional.

#### The `icon` field
| Type          | Required |
|---------------|----------|
| Object/String | False    |

One or more paths to a square .PNG file. If an object is provided, the keys must be the resolution of the corresponding file. For example:
```json
"icon": {
    "32": "path/to/icon32.png",
    "64": "path/to/icon64.png",
    "4096": "path/to/icon4096.png"
}
```

## The `mixin` field
| Type         | Required |
|--------------|----------|
| Array/String | False    |

A single or array of paths to mixin configuration files relative to the root of the mod JAR.

## The `access_widener` field
| Type         | Required |
|--------------|----------|
| Array/String | False    |

A single or array of paths to accesswidener files relative to the root of the mod JAR.

## The `minecraft` field
| Type   | Required |
|--------|----------|
| Object | False    |

Contains flags and options related to Minecraft specifically.

### The `environment` field
| Type   | Required |
|--------|----------|
| String | False    |

Defines the environment(s) that this mod should be loaded on. Valid values are:
* `"*"` — All environments (default)
* `"client"` — The physical client
* `"server"` — The dedicated server

## Custom Elements
In addition to the defined elements above, mods and libraries will be able to add their own elements to the quilt mod file. Mods will be expected to define up to one top-level element corresponding to their mod id. The element can be of any type, so that mods can define either a single value, array of values, or a sub-object.

## Example
An example quilt.mod.json:
```json
{
    "schema_version": 1,
    "quilt_loader": {
        "group_id": "org.quiltmc",
        "mod_id": "example_mod",
        "version": "1.0.0",
        "entrypoints": {
            "main": [
                "org.quiltmc.example_mod.impl.ExampleMod",
                "org.quiltmc.example_mod.impl.ExampleModNetworking"
            ],
            // Since we only have a single client endpoint, no array is needed.
            "client": "org.quiltmc.example_mod.impl.client.ExampleModClient",
        },
        "depends": [
            "quilt_networking_api",
            "quilt_rendering_api",
            {
                "id": "modmenu",
                "environment": "client"
            }
        ],
        "breaks": [
            {
                "id": "sodium",
                "versions": "*",
                "reason": "Sodium does not implement the Quilt Rendering API."
            },
            {
                "id": "some_random_library",
                "versions": [
                    "1.23.456", // A reason is not required
                    {
                        "versions": "<1.0.0",
                        "reason": "Stable API required"
                    },
                    {
                        "versions": "1.5.3",
                        "reason": "Contains a game-breaking bug"
                    }
                ]
            }
        ],
        "metadata": {
            "name": "Quilt Example Mod",
            "description": "An example mod for the Quilt ecosystem.",
            "contributors": {
                "Haven King": "Developer"
            },
            "contact": {
                "homepage": "https://quiltmc.org/"
            },
            "license": "CC0-1.0",
            "icon": "assets/modid/icon.png"
        }
    },
    "mixins": [
        "modid.mixins.json"
    ]
}
```

## Placeholders
Strings with defined formats such as [the version field](#the-version-field) can instead supply a string matching the regex `^\$\{[a-zA-Z_$][a-zA-Z\d_$]*\}$` in a development environment to be replaced on build. The quilt-gradle plugin defines usage of the value `${version}` as a placeholder for the version declared in gradle.properties.

## Dependency Objects
A dependency object defines what mods/plugins a given mod depends on or breaks. It can be represented as either an object containing at least [the `id` field](#the-id-field-1), a string mod identifier in the form of either `mavenGroup:modId` or `modId`, or an array of dependency objects. If an array of dependency objects is provided, the dependency matches if it matches ANY of the dependency objects.

### The `id` field
| Type   | Required |
|--------|----------|
| String | True     |

A mod identifier in the form of either `mavenGroup:modId` or `modId`.

### The `versions` field
| Type         | Required | Default |
|--------------|----------|---------|
| Array/String | False    | `"*"`   |

Should be a [version specifier](#version-specifier) or array of version specifiers defining what versions this dependency applies to. If an array of versions is provided, the dependency matches if it matches ANY of the listed versions.

### The `optional` field
| Type    | Required | Default |
|---------|----------|---------|
| Boolean | False    | `false` |

Dependencies marked as `optional` will only be checked if the mod/plugin specified by [the `id` field](#the-id-field-1) is present.

### The `unless` field
| Type             | Required |
|------------------|----------|
| DependencyObject | False    |

Describes situations where this dependency can be ignored. For example:
```json
{
    "id": "sodium",
    "unless": "indium"
}
```

Game providers and loader plugins can also add their own optional fields to the dependency object for extra context when resolving dependencies. The Minecraft game provider, for instance, might define an "environment" field that can be used like so:

```json
{
    "id": "modmenu",
    "environment": "client"
}
```

## Version Specifier
A version range specifier can make use of any of the following patterns:
* `*` — Matches any version. Will fetch the latest version available if needed
* `1.0.0` — Matches the most recent version greater than or equal to version 1.0.0 and less than 2.0.0
* `=1.0.0` — Matches exactly version 1.0.0 and no other versions
* `>=1.0.0` — Matches any version greater than or equal to 1.0.0
* `>1.0.0` — Matches any version greater than version 1.0.0
* `<=1.0.0` — Matches any version less than or equal to version 1.0.0
* `<1.0.0` — Matches any version less than version 1.0.0
* `1.0.x` — Matches any version with major version 1 and minor version 0.
* `~1.0.0` — Matches the most recent version greater than or equal to version 1.0.0 and less than 1.1.0
* `^1.0.0` — Matches the most recent version greater than or equal to version 1.0.0 and less than 2.0.0

# Drawbacks
The primary drawback to this proposed format is the break from convention established by the Fabric project. It may make it more difficult for modders to adjust to the new toolchain if they are having to make drastic changes to their mod files.

# Rationale and Alternatives
It makes sense to take advantage of the breaking divergence of the toolchain to make changes that are better for Quilt's future. We could stick more strictly to the fabric.mod.json specification, but we would lose a lot of the advantages that this iteration has.

# Prior Art
This specification builds upon the Fabric projects existing [fabric.mod.json v1 specification](https://fabricmc.net/wiki/documentation:fabric_mod_json_spec).

The [contributors section](#the-contributors-field) is loosely based on that of [the Sponge project](https://www.spongepowered.org/)s plugin metadata specification.

The structure for this specification is loosely based on the Rust project's [manifest format specification](https://doc.rust-lang.org/cargo/reference/manifest.html).

# Unresolved Questions
* How will the community (particularly the development community) react to these changes?
* What changes will need to be made in Loader to facilitate this new structure?
