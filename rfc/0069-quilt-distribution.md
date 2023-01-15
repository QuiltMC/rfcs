# ????
# Summary
This RFC proposes changing our goal of how we distribute Quilt to users. Instead of simply installing Quilt Loader and letting Dependency Downloading take care of everything else, users will also install a version of QSL, Quilted Fabric API, and related projects. They will be able to update these projects by re-running the installer, or downloading them manually from Curseforge or Modrinth.
# Motivation
## Assumptions about Dependency Downloading
This RFC makes the following assumptions on Dependency Downloading and the current approach for QSL:
- QSL is currenlty intended to only be distributed as its individual modules, and only installed when other mods depend on that module
- Mods depend on the *exact version* of QSL modules they depend on, down to the hash of the jar
- Dependencies are never updated automatically
 
## Problems with Dependency Downloading
While Dependency Downloading is certainly useful, and this RFC does not seek to replace it as a whole, it comes with some pretty major drawbacks when:
- It is the only way to aquire a dependency
- The dependency adds content of its own, independent of the mod depending on it (e.g. a Keybinds API that adds chords)

These problems are:
### Problem 1: Bugfixes and picking a version
If dependency downloading is the only way to select a dependency, then users only get the latest version of it that their mods depend on. If a bugfix is released, users will not get that bugfix until they happen to download a mod that uses the bugfix.
### Problem 2: Conflicts
QSL is only tested when all of its modules are the exact same version. Unexpected behavior and conflicts might arise from mis-matching versions, which is made worse by the fact that users cannot manually select a version.
### Problem 3: QSL is a content mod
Certain QSL modules add features that do not require another mod to be present in order to affect users' games, such as (the up-and-coming) Keybinds API. It adds things that aren't possible in Vanilla, like chords. Core game features shouldn't change depending on if an unrelated mod that adds a keybind is installed.
### Problem 4: Extra modules
With dependency downloading, there is no easy way to ask users "Do you want to include ModMenu?". The only way to aquire it is through a) downloading it directly or b) happening to pull it in via a random mod the user downloads.
Currently, there is no system to install something like ModMenu at install-time.
### Side note: Fabric mods
It's likely that Fabric mods will be running alongside Quilt ones for the foreseeable future; while Quilt-only modpacks will certainly exist, it's unrealistic to expect every major and minor content mod to migrate to Quilt.
Since the distribution model for Fabric is to always load all of Fabric API, that means that almost every Quilt install will also need all of QSL via QFAPI.

# Explanation
Quilt will adopt two systems of distribution:
- The classic, Fabric-like system, "Quilt Lite", that follows the current approach of users downloading Loader through Installer/their launcher, and QSL through a mod hosting platform
- A new, Forge-like distribution, "Quilt" or "Quilt Full" that bundles all major parts of Quilt into one package. This version is only available for full releases of Minecraft (snapshots, April Fools, etc. excluded; see [rationale](#rationale)).


## Contents of the Full bundle
The full Quilt bundle will initially include just Quilted Fabric API. In the future, default Loader plugins, ModMenu, and libraries external to QSL (for example, FREX) could be added by default as well.
Mods in the bundle should not substantially change Vanilla behavior and capabilities, but small and neccesary deviations are OK, like keybind chords or new datapack behavior.

The full bundle would only include stable versions of each library (if one exists); users must use the Lite distribution if they want to select alphas or betas.
## Technical Details
Meta would provide a JSON like the following:
```json
{
  "id": "quilt-1.19.2-build.32",
  "inheritsFrom": "1.19.2",
  "releaseTime": "2022-08-05T16:45:18+0000",
  "time": "2022-08-05T16:45:18+0000",
  "type": "release",
  "mainClass": "org.quiltmc.loader.impl.launch.knot.KnotClient",
  "libraries": [
      "name": "org.quiltmc:quilt-config:1.0.0-beta.4",
      "url": "https://maven.quiltmc.org/repository/release/"
    },
    {
      "name": "org.quiltmc:hashed:1.19.2",
      "url": "https://maven.quiltmc.org/repository/release/"
    },
    {
      "name": "net.fabricmc:intermediary:1.19.2",
      "url": "https://maven.fabricmc.net/"
    },
    {
      "name": "org.quiltmc:quilt-loader:0.17.0",
      "url": "https://maven.quiltmc.org/repository/release/"
    },
    {
        "name": "org.quiltmc:quilted-fabric-api:8.0.0+1.19.2:fat",
        "url": "https://maven.quiltmc.org/repository/release"
    }
    // other quilt libraries omitted
  ]
}
```

When QFAPI (or another library specific to a Minecraft version) is updated, only relevant bundle is updated, with a build number bump.
When Loader is updated, all versions that the Loader version is compatible with recieve a new build. (A hypothetical Loader 1.0 might update the bundles for 1.19, 1.19.1, 1.19.2, and 1.19.3, but not 1.18.2 or 1.20)
# Drawbacks
- This approach requires significant new infrastructure for Quilt.
- Launchers may not support the new system right away

# Rationale and Alternatives
## Rationale
Ultimately, this is the approach that is as simple as possible for launchers and ourselves to implement. We cover the two primary usecases, of normal users and power users, and don't put any kind of burden on the launcher to implement a complex UI or extra configurability if they don't want to.

Additional capabilities, like a "beta" distribution that includes beta versions of Loader, or Full bundles for snapshot versions, could be investigated if it is determined to be needed. This proposal does not include either of those to keep things simple and reduce pressure on Meta; we don't want to update 60 or 80 versions each time a commit is pushed to Loader.
### No snapshot support
## Alternative: "Latest versions" endpoint
This approach would have us simply provide an unversioned JSON endpoint on meta that provides the latest recommended version of each library for a specific Minecraft version, for example:

```json!
{
    "quilt_loader": "0.12.0",
    "qsl": "0.11.3+1.13",
    "mod_menu": "1.2.3+1.13"
    // etc
}
```

However, this approach shifts the burden of implementing to launchers, which are often not built to handle modloaders with multiple moving parts. We want Quilt to be simple to use on all launchers, and we don't want to make it excessively difficult for Quilt support to be added on new launchers.
