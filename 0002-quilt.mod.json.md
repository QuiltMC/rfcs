# Summary
This document acts as a Request for Comments (RFC) on the topic of how a quilt.mod.json should be formatted. Such changes are intended to provide added value for mod developers, library developers, and users alike.

# Motivation
Since the toolchain is being forked, now is the opportune moment to make large scale changes to many of the systems mod developers use. This RFC describes a more flexible and powerful format for the mod.json file that defines what a mods definition should look like.

# Explanation
Below is an outline of all defined keys and values.

* [schema_version](#the-schema_version-field) — The schemaVersion to be used for reading this file
* [quilt_loader](#the-quilt_loader-field) — Information related to loading the mod
    * [group_id](#the-group_id-field) — The Maven group id
    * [mod_id](#the-modId-field) — The mod id
    * [version](#the-version-field) — The mods version
    * [entrypoints](#the-entrypoints-field) — Collection of entrypoints
    * [plugins](#the-plugins-field) — Collection of plugins
    * [jars](#the-jars-field) — Array of nested JARs to be loaded
    * [language_adapters](#the-language_adapters-field) — Array of language adapters
    * [mixins](#the-mixins-field) — Array of mixin config files
    * [access_wideners](#the-access_wideners-field) — Path to an accessWidener file
    * [depends](#the-depends-field) — Collection of mod dependencies
    * [breaks](#the-breaks-field) — Collection of mods that this mod is incompatible with
    * [repositories](#the-repositories-field) — Array of maven repositories
* [minecraft](#the-minecraft-field) - Minecraft related options
    * [environment](#the-environment-field) — What game environments this mod applies to
* [metadata](#the-metadata-field) — Extra information about this mod and/or its authors
    * [name](#the-name-field) — Human-readable name of this mod
    * [description](#the-description-field) — Human-readable description of this mod
    * [authors](#the-authors-field) — Array of authors/owners of this mod
    * [contributors](#the-contributors-field) — Array of contributors to this mod
    * [contact](#the-contact-field) — Collection of contact information
    * [license](#the-license-field) — One or more licenses this project is under
    * [icon](#the-icon-field) — The icon or icons associated with this project

## The `schema_version` field
| Type   | Required |
|--------|----------|
| Number | True     |

The quilt.mod.json schema version to be used for parsing this file. Currently, the only valid version is 1.

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

### The `mod_id` field
| Type   | Required |
|--------|----------|
| String | True     |

A unique identifier or the mod or library defined by this file, matching the `^[a-z][a-z0-9-_]{1,63}$` regular expression. Best practice is that mod ID's are in snake_case.

### The `version` field
| Type   | Required |
|--------|----------|
| String | True     |

Must conform to the [Semantic Versioning 2.0.0 specification](https://semver.org/). In a development environment, the value `${version}` will be allowed as a placeholder to be replaced on building the resulting JAR.

### The `entrypoints` field
| Type   | Required |
|--------|----------|
| Object | False    |

A collection of `"key": value` pairs, where each key is the type of the entrypoints specified and each values is either a single entrypoint or an array of entrypoints. An entrypoint is an object with the following keys:
* adapter — Language adapter to use for this entrypoint
* value — Points to an implementation of the entrypoint. Can be in either of the following forms:
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

A collection of `"key": value` pairs, where each key is the namespace of a language adapter and the value is an implementation of the `LanguageAdapter` interface.

### The `mixins` field
| Type   | Required |
|--------|----------|
| Array  | False    |

An array of paths to mixin configuration files relative to the root of the mod JAR.

### The `access_wideners` field
| Type   | Required |
|--------|----------|
| Array  | False    |

An array of paths to accesswidener files relative to the root of the mod JAR.

### The `depends` field
| Type   | Required |
|--------|----------|
| Object | False    |

Defines mods that this mod will not function without.

A collection of `"key": value` pairs, where each key is in the form of either `mavenGroup:modId` or `modId` and each value is either a version range specifier or array of version range specifiers. If an array of range specifiers is provided, the version matches if it matches ANY of the listed specifiers. A version range specifier can make use of any of the following patterns:
* `*` — Matches any version. Will fetch the latest version available if needed
* `1.0.0` — Matches exactly version 1.0.0 and no other versions
* `=1.0.0` — Matches exactly version 1.0.0 and no other versions
* `>=1.0.0` — Matches any version greater than or equal to 1.0.0
* `>1.0.0` — Matches any version greater than version 1.0.0
* `<=1.0.0` — Matches any version less than or equal to version 1.0.0
* `<1.0.0` — Matches any version less than version 1.0.0
* `1.0.x` — Matches any version with major version 1 and minor version 0.
* `~1.0.0` — Matches the most recent version greater than or equal to version 1.0.0 and less than 1.1.0
* `^1.0.0` — Matches the most recent version greater than or equal to version 1.0.0 and less than 2.0.0

### The `breaks` field
| Type   | Required |
|--------|----------|
| Object | False    |

Defines mods that this mod either breaks or is broken by.

A collection of `"key": value` pairs, where each key is in the form of either `mavenGroup:modId` or `modId` and each value is either a version range specifier or array of version range specifiers. If an array of range specifiers is provided, the version matches if it matches ANY of the listed specifiers. See [above](#the-depends-field) for the version range specifier format.

### The `repositories` field
| Type   | Required |
|--------|----------|
| Array  | False    |

A list of repositories where dependencies can be looked for in addition to Quilt's central maven repository.

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

## The `metadata` field 
| Type   | Required |
|--------|----------|
| Object | False    |

Optional metadata that can be used by mods to display information about the mods installed.

### The `name` field
| Type   | Required |
|--------|----------|
| String | False    |

A human-readable name for this mod.

### The `description` field
| Type   | Required |
|--------|----------|
| Object | False    |

A human-readable description of this mod.

### The `authors` field
| Type   | Required |
|--------|----------|
| Array  | False    |

An array of strings, each one the name of an author/owner or organization in charge of this project.

### The `contributors` field
| Type   | Required |
|--------|----------|
| Array  | False    |

An array of strings, each one the name of a contributor to this project.

### The `contact` field
| Type   | Required |
|--------|----------|
| Object | False    |

A collection of `"key": value` pairs denoting various contact information for the people behind this mod. The following keys are officially defined, though mods can provide as many additional values as they wish:
* email — Valid e-mail address for the organization/developers
* homepage — Valid HTTP/HTTPS address for the project or the organization/developers behind it
* issues — Valid HTTP/HTTPS address for the project issue tracker
* sources — Valid HTTP/HTTPS address for a source code repository

### The `license` field
| Type         | Required |
|--------------|----------|
| Array/String | False    |

The license or array of licenses this project operates under.

### The `icon` field
| Type          | Required |
|---------------|----------|
| Object/String | False    |

One or more paths to a square .PNG file. If an object is provided, the keys must be the resolution of the corresponding value.

## Custom Elements
In addition to the defined elements above, mods and libraries will have direct access to the quilt.mod.json file as a json object. Mods can thus access any top-level defined custom values. Naming conventions aren't enforced. It is suggested that you use your mod id as the key for your custom value, prefixed with your maven group if the mod id itself is at risk of overlap. The custom value can also be any type you'd like, though it's recommended to group large numbers of keys together under a single object.

# Drawbacks
The primary drawback to this proposed format is the break from convention established by the Fabric project. It may make it more difficult for modders to adjust to the new toolchain if they are having to make drastic changes to their mod.json files.

# Rationale and Alternatives
It makes sense to take advantage of the breaking divergence of the toolchain to make changes that are better for Quilt's future. We could stick more strictly to the fabric.mod.json specification, but we would lose a lot of the advantages that this iteration has.

We've discussed moving away from JSON for the mod definition file, but decided that a lenient JSON parser in dev will work, with quilt-gradle stripping comments when building jars.

# Prior Art
This specification builds upon the Fabric projects existing [fabric.mod.json v1 specification](https://fabricmc.net/wiki/documentation:fabric_mod_json_spec).

The structure for this specification is loosely based on the Rust project's [manifest format specification](https://doc.rust-lang.org/cargo/reference/manifest.html).

# Unresolved Questions
* How will the community (particularly the development community) react to these changes?
* What changes will need to be made in Loader to facilitate this new structure?
