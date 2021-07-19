# Technical Team Structure

## Summary

This RFC describes the structure and processes that technical teams of the
Quilt project will follow. It does not detail members of each team nor does
it restrict them from committing to additional policies in the future.

## Motivation

Laying out a clean, formal specification for how people will interact with
and decide on changes will help technical processes run more smoothly, from
creating entirely new projects to making small revisions to existing ones.

## Explanation

### Technical Leads
Technical leads are the highest technical authority in the Quilt Project.
They are not aligned to any particular technical team, but instead act as
guides, steering the other teams in a direction that's beneficial to Quilt
as a whole. While the administrative board exists to oversee and facilitate
policy changes throughout the project, technical leads exist to oversee and
facilitate larger technical decisions.

### Team Responsibilities
Technical teams are typically aligned to a specific project or set of
projects. They are responsible for reviewing pull requests and ensuring that
they meet the Quilt projects goals in quality and purpose. Once a team has 
decided that a pull request is suitable for merging, a technical lead will be 
responsible for the actual merging.

### Technical Teams
The following is a list of top-level technical teams.

| Team | Repositories | Notes
|-|-|-|
| Quilt Loader | [Quilt Loader](https://github.com/QuiltMC/quilt-loader)<br>[Quilt Installer](https://github.com/QuiltMC/quilt-installer)<br>[Sat4j](https://github.com/QuiltMC/quilt-loader-sat4j)<br>[Access Widener](https://github.com/QuiltMC/access-widener)<br>[Mixin](https://github.com/QuiltMC/Mixin)<br>[Quilt JSON5](https://github.com/QuiltMC/quilt-json5)
| Quilt Standard Libraries | [Quilt Standard Libraries](https://github.com/QuiltMC/quilt-standard-libraries)<sup>TBD</sup>| Will contain additional teams for each library
| Infrastructure | [Quilt Meta](https://github.com/QuiltMC/quilt-meta)
| Mappings | [Yarn](https://github.com/QuiltMC/yarn)<br>[Intermediary](https://github.com/QuiltMC/intermediary)<br>[Matcher](https://github.com/QuiltMC/matcher)<br>[Matcher MC](https://github.com/QuiltMC/matcher-mc)<br>[Enigma](https://github.com/QuiltMC/yarn)<br>[Lorenz Tiny](https://github.com/QuiltMC/lorenz-tiny)<br>[Stitch](https://github.com/QuiltMC/stitch)<br>[Tiny Mappings Parser](https://github.com/QuiltMC/tiny-mappings-parser)<br>[Tiny Remapper](https://github.com/QuiltMC/tiny-remapper)
| Build Tools | [Quilt Loom](https://github.com/QuiltMC/quilt-loom)<br>[Gradle Convention Plugins](https://github.com/QuiltMC/gradle-convention-plugins)<br>[Sponge Mixin Compile Extensions](https://github.com/QuiltMC/sponge-mixin-compile-extensions)<br>[Dev Launch Injector](https://github.com/QuiltMC/dev-launch-injector)| This team will also be responsible for developing a replacement for Quilt Loom when it becomes possible to do so
| Decompilers | [Quiltflower](https://github.com/QuiltMC/quiltflower)<br>[CFR](https://github.com/QuiltMC/cfr)<br>[Procyon](https://github.com/QuiltMC/procyon)
| ASMR | [ASMR Processor Prototype](https://github.com/QuiltMC/asmr-processor-prototype) |

## Drawbacks

An overcomplicated leadership structure may lead to further stagnation in the development process, rather than have the intended effects of easing it. This approach is a two-way door, however, and can always be reversed if it proves unsuccessful.


## Rationale and Alternatives

Many companies today follow the idea of separation of concerns. By putting people in charge of a relatively smaller number of tasks and projects, they will be able to better focus on such tasks to get things done. The alternative approach would be for a small number of people to "hold the keys" over all technical projects. Fabric currently does this, and it leads to a generally very long and arduous process for getting contributions accepted.


## Expected Response

It is expected that with more refined teams being dedicated to individual projects, community members outside of those teams will also be more willing to contribute to individual projects since there will be a direct point of contact for each individual initative.

