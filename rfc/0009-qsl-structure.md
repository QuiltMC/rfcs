# QSL Structure
## Summary
This documents provides the basis on how to structure the Quilt Standard Library. It seeks to define how Fabric's current API will be split into several libraries (collections of modules) for the purposes of development. It does not define the structure of the modules within these libraries; that is left to the teams discretion when creating QSL, besides the guideline that modules should be generally larger than what is expected in Fabric API.

Additionally, it defines that mods will depend on QSL as a fat jar, and Loom is expected to detect what (sub/super)modules are being used and automatically mark them to be downloaded at runtime.
## Motivation
### Prior Art
Organization of code into visible, transparent modules is a zero-sum game involving QSL maintainers, mod developers, and the end users.  
Here is a quick comparison between current systems that exist today:
- Fabric API
	- 
	- Large amount of single-purpose, small, and sometimes interconnected modules, consumed by developers and users as a manually downloaded fat jar.
	- Maintainers
		-
		- Benefits
			- Replacing large swaths of code is easy
			- The small parts keep scope in check, avoiding "dumping grounds"
		- Drawbacks
			-  Everybody just uses the fat jar
			-  API visibility is low since modules don't usually inter-connect
				-  Compare Forge's Item.setModelRenderer (devoldified) to using a registry for everything
			-  The massive amount of modules makes navigating FAPI confusing
			-  Even though there are small modules, modules are often very tiny--Extensions of Items might be split across Content Registries, Item API, Tool Item API, 
- Forge
	-
	- Benefits
		- Monolithic API makes adding new features straightforward
	- Drawbacks
		- It is more difficult to remove parts of code as Mojang breaks them
		- Lack of compartmentalization leads to seemingly unrelated APIs being intertwined
		- No api/impl split makes it difficult to know what part of the code you are actually supposed to interact with
### Goals
- Be mobile and quick to update like Fabric
- Have high API visibility like Forge
- Find a balance between Forge's confusingly monolithic and Fabric's confusingly modular structures
- Have teams in charge of different parts of QSL's code
- Avoid making users download and run code they don't need
## Explanation
This is an overview of the top-level structure of QSL. Libraries are included, with the names of the FAPI modules that would be under its scope. The actual structure of the modules in each library is up to the team, and it is not a goal to define how they must structure their modules.
### Libraries
Each library would be its own folder in the QSL monorepo, with a dedicated team responsible for triage, review, and accepting/denying pull requests. In contrast to Fabric or Forge's system of only one person signing off for the entire standard library, no authority higher than the people on the team would be required to sign off on the PR before it is merged. More clarification on team structure is expected to come in a future RFC.

Libraries only exist for internal structure--there is no fat jar for, say, all `worldgen` modules, neither is there a fat jar for all of QSL. This also means that QSL and its libraries have no versioning.
### Modules
Modules follow the following general rules:
- Maven group of `org.quiltmc.qsl.$library`
- Artifact id of `mod-id`
- Semantic Versioning. Hashes are *not* used to identify modules. 
- Can only depend on modules from `core`, `common`, or within their own library.

To modders, most mods would not explicitly depend on libraries or their modules, but Loom would automatically determine the modules to depend on by scanning at compile time, and those modules will be downloaded at runtime by Loader.
### Top-level Structure
Keep in mind that the modules listed here are the names of the current Fabric API modules, and the final internal structure of QSL **will not** be the same as Fabric API.
- `core` - All modules here are implicitly depended on by mods by default, even if they do not use any code from them. Mods can choose to opt-out from this, but if they choose to use the fat QSL dependency they are responsible for ensuring these modules don't get transitively pulled in.
    - Lifecycle Events
    	- Here because of the Networking API
    	- Some parts might be split into `entities` and `world`
    - Networking API (maybe with relevant parts split into other modules)
        - Registry Sync (merged in)
        - (future) Proper Handshake API
    - Crash Reports
    - Resource Loader
- `transfer`
    - API Lookup API
    - fluid api
    - item transfer api
- `command` // This really is anything server-side only related to administration, but "command" is a nice word to describe the primary use-case.
    - Command API
    - Game Rule API
    - (future) Permissions API
- `rendering` // This entire library should be client-side only
	- BlockRenderLayer Registration
	- Indigo
	    - need an easy way to exclude this
	- Models
	- Renderer API
		- This may need a better name in a spot?
	- Rendering
	- Renderer Registries
	- Rendering Fluids
	- Textures
	- Object Builder API (FabricModelPredicateProviderRegistry)
- `data` - Tools for working with, generating, or consuming data (e.g. datapacks, advancements, recipes, tags)
    - (future) Recipe API
    - (future) Datagen APIs
    - Rendering Data Attachment
    - Tag Extensions
    - Loot Tables
    - Object Builder API (Criterion)
- `block` 
	- BlockEntity Networking (merged in)
    - Object Builder API (FabricBlockSettings, FabricBlockEntityBuilder and FabricMaterialBuilder)
    - (future) Block Extensions (think IForgeBlock)
    - Content Registries (Flammable Block Registry)
- `item`
    - Item API
    - Content Registries (Fuel Registry, Composting Registry)
    - Tool Attribute API
    - (future) Item Extensions (think IForgeItem)
    - Item Groups
- `entity`
    - Object Builder API (EntityType Builder, Trades, Villager Professions and Types)
    - Entity Events
- `content_other`
    	- Particles
    	- Object Builder API (Point of interest)
    	- Interaction Events
        	- Could probably use refactoring, but as it stands this absolutely deals with events happening in the world
- `worldgen`
    - Dimension API
    - Biome API
    - Structure API
- `gui`
    - Screen API
        - Despite the name, all this module really does is add hooks for modifying prexisting screens
    - ScreenHandler API
        - this could also go in `world`
    - (potentially in the future) Screen Extensions
    - Key Bindings API
## Drawbacks
Yet another change in structure and interaction with the API may make it more difficult to contribute to QSL.

It may be difficult to find the right level of granularity for libraries, and there might be lots of flux as we figure out the correct amount of libraries and how large we want the teams to be.

The compile-time scanning of used features adds additional complexity to our build process, and might be buggy.
## Unresolved Questions
Most things not defined here are left to the discretion of the QSL Technical Lead and their teams, as defined in the (future) Technical Teams RFC.

In the future, there may be a "library group"--a group of modules that is allowed to bypass the library sandboxing restriction within that group. This will be defined in a later RFC if we find it is actually needed.
## Expected Response
Because Fabric API is almost always used in a monolithic structure anyway, this new structure is expected to have little impact on the average modder.

The separation of QSL into teams and their own repositories may increase the complexity of submitting PRs, but if executed properly this shouldn't be a big issue.
