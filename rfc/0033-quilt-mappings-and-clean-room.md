# Quilt Mappings and the Cleanroom
## Summary

This RFC proposes a new approach to mappings within the Quilt project, specifically regarding Yarn's cleanroom approach, licensing and related enforcement in Quilt community spaces.

With Mojang's mappings (Mojmap) quickly becoming the de-facto mappings standard throughout the modding ecosystem, and MCP mappings effectively dead and gone, it no longer makes sense to maintain the purity of Yarn. This is especially true given the original motivations behind Yarn's cleanroom approach, the questionably impure approach taken by some of the upstream maintainers, and the lack of formal accountability for its contributors.

This RFC proposes that non-Yarn mappings should be allowed to be referenced in all Quilt community and development spaces, with the goal of creating a more inclusive support ecosystem and raising the quality of Quilt's fork of Yarn.

As Quilt's Yarn fork will be significantly different from upstream Yarn, this RFC also proposes changing its name to Quilt Mappings (QM).

Any discussions related to migrating existing Quilt projects to other mappings standards are out of scope of this RFC, and there is - as of this writing - no intention to do so.

## Motivation

In order to understand the motivation for this RFC, it's important to look at how we got here. A lot has changed since Yarn was concieved in 2016, and the approach it takes isn't really relevant in today's modding landscape.

### Background

Yarn was originally created on August 12, 2016. It was created for a project called Open Mod Loader, the first iteration of what would be known as Fabric.

It would remain in relative obscurity until October of 2018, when Yarn began tracking early snapshots for Minecraft 1.14.

### Rift

During the 1.13 development cycle, Forge chose to rewrite most of their scaffolding, and a stable Forge build didn't appear until Minecraft 1.14. Rift was created in response to this, as a lightweight mod loader that could do basic coremodding on Minecraft 1.13.

However, Rift made use of parts of the Forge toolchain without permission. Forge declined to grant this permission to them after the fact, and Rift-related discussions were effectively banned in most modding-related communities.

### Yarn's Motivation

Due to the strict licensing of some parts of the Forge toolchain at the time, Fabric needed to avoid making use of Forge projects such as MCP, and thus avoid making the same mistakes that Rift did. In order to achieve this, Yarn was licensed under the [Creative Commons Zero License](https://creativecommons.org/share-your-work/public-domain/cc0/) (CC0), a public domain dedication.

Making use of the CC0 in this way meant that all names in Yarn needed to be original contributions - it would have been unacceptable for a name to make it in from another mappings project, unless that project was also licensed under the CC0. For this reason, Yarn needed to take a cleanroom approach, rejecting contributions that added names from other mappings projects, and banning discussions of names from non-CC0 projects in related community spaces.

### Issues With the Cleanroom

While discussing how Quilt's approach to mappings should work, the Quilt Mappings Team has made the following observations:

* Almost everyone that ended up taking up Fabric and Yarn were switching from other projects, generally Spigot or Forge
* Regardless of who contributed a name, the CC0's warranty and infringement disclaimers mean that Quilt would be fully responsible in the event of DMCA on our mappings.
* Some Yarn names are clearly inspired by or lifted from MCP, and couldn't have been named how they are without it
* Some Yarn contributors have been known to be working with other mappings projects, either in their own mods or directly, while remaining active Yarn contributors - even with video evidence, in some cases

For these reasons, the Quilt Mappings Team has concluded that it's impossible to say with absolute certainty that all Yarn names are unique, and not inspired by (or copied from) other mappings projects.

It's also clear that Mojang feels that their mappings should be used in this way - or at least have no wish to prevent it - given that they haven't intervened in other projects that use it in a similar way.

It's become abundantly clear that Yarn's cleanroom isn't very clean, and that it doesn't serve any real purpose in today's modding ecosystem.

### Changes to the Ecosystem

As mentioned earlier, a lot has changed in the time since Yarn was concieved:

* Most other modloader projects are using Mojmap, or something layered on top of it
* MCP has been retroactively relicensed to the zlib license, and is no longer being maintained as an isolated mappings project
* It's become clear that Microsoft has many other ways to kill off modloader projects, even though it doesn't have any discernable motivation to do so - and regardless of whether they'd be in the right to do so, it's unlikely that anyone would be able or willing to fund a court case in defense

### Conclusion

In conclusion, the current approach to Yarn doesn't really make sense given the modern modding ecosystem. While this RFC has no direct bearing on upstream Yarn, the Quilt Project feels that this RFC is the best way to move forward with Quilt Mappings.

## Explanation

Should this RFC be approved:

* Quilt's fork of Yarn will be renamed to "Quilt Mappings" (QM)
* All restrictions on referencing mappings projects (other than Quilt Mappings) within Quilt's community and development spaces will be lifted
* A file named `ATTRIBUTION.md` will be created in the Quilt Mappings repository, which will properly attribute both the MCP project and Mojang for any names that may be inspired by or attributed to those projects, with the possibility of adding more as required
* The Quilt Mappings project will provide additional attribution to other mappings projects and providers (including MCP and Mojang) as required, which may also include changes to Quilt's toolchain
* The Quilt Mappings repository will have its current restrictions regarding mentions of other mappings projects lifted, and replaced with a requirement that contributors may only reference other mappings projects that are public domain, licensed under the CC0, or attributed in the `ATTRIBUTION.md` file
* The other Quilt Mappings contribution requirements, such as quality of names or conventions and standards will not be changed by this RFC
* This will not affect Quilt's other plans going forward, and everything else will continue as normal - including the planned move from intermediary to Hashed Mojmap.

These changes are likely to happen in many steps, but it's expected that they'll will be made in quick succession.

## Drawbacks

### Community Confusion

It's possible that giving and receiving support in Quilt community spaces may be complicated somewhat, as different people will be familiar with different types of mappings.

However, this issue already exists with the cleanroom approach in a sense, as people that aren't used to Yarn currently already have to find a way to translate names from other mappings to Yarn in order to get support in Quilt community spaces.

This could be mitigated by adding mappings translation commands to Cozy, the Quilt Discord bot.

### Fragmentation

By removing restrictions on which mappings projects may be discussed in community and development spaces, it's likely that more projects will switch to Mojmap - as they're now free to do so and still request help in official Quilt spaces. It's possible that this could create a split in Quilt's developer community, where developers making use of one set of mappings may find it difficult to start contributing to projects that use others.

This could be mitigated somewhat by providing a recommendation that new projects built exclusively for Quilt make use of Quilt Mappings, with projects that need to support multiple modloaders using Mojmap instead.

### Reduced Quality of Quilt Mappings Names

Although unlikely, it's possible that contributors to the Quilt Mappings project may believe that Mojang's names are the best in all cases, and attempt to contribute those names directly to QM because they're what Mojang uses. These types of contributions would significantly lower the quality of the names included in the Quilt Mappings project.

This can be mitigated by continuing the current comprehensive review process - reviewing names based upon their quality alone, and not using origin-based arguments as a measure of that quality.
