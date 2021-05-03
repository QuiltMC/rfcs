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
| Quilt Loader | [Quilt Loader](https://github.com/QuiltMC/quilt-loader)<br>[Sat4j](https://github.com/QuiltMC/quilt-loader-sat4j)<br>[Access Widener](https://github.com/QuiltMC/access-widener)<br>[Mixin](https://github.com/QuiltMC/Mixin)<br>[Quilt JSON5](https://github.com/QuiltMC/quilt-json5)
| Quilt Standard Libraries | [Quilt Standard Libraries](https://github.com/QuiltMC/quilt-standard-libraries)<sup>TBD</sup>| Will contain additional teams for each library
| Infrastructure | [Quilt Installer](https://github.com/QuiltMC/quilt-installer)<br>[Quilt Meta](https://github.com/QuiltMC/quilt-meta)
| Mappings | [Yarn](https://github.com/QuiltMC/yarn)<br>[Intermediary](https://github.com/QuiltMC/intermediary)<br>[Matcher](https://github.com/QuiltMC/matcher)<br>[Matcher MC](https://github.com/QuiltMC/matcher-mc)<br>[Enigma](https://github.com/QuiltMC/yarn)<br>[Lorenz Tiny](https://github.com/QuiltMC/lorenz-tiny)<br>[Stitch](https://github.com/QuiltMC/stitch)<br>[Tiny Mappings Parser](https://github.com/QuiltMC/tiny-mappings-parser)<br>[Tiny Remapper](https://github.com/QuiltMC/tiny-remapper)
| Build Tools | [Quilt Loom](https://github.com/QuiltMC/quilt-loom)<br>[Gradle Convention Plugins](https://github.com/QuiltMC/gradle-convention-plugins)<br>[Sponge Mixin Compile Extensions](https://github.com/QuiltMC/sponge-mixin-compile-extensions)<br>[Dev Launch Injector](https://github.com/QuiltMC/dev-launch-injector)| This team will also be responsible for developing a replacement for Quilt Loom when it becomes possible to do so
| Decompilers | [Quiltflower](https://github.com/QuiltMC/quiltflower)<br>[CFR](https://github.com/QuiltMC/cfr)<br>[Procyon](https://github.com/QuiltMC/procyon)
| ASMR | [ASMR Processor Prototype](https://github.com/QuiltMC/asmr-processor-prototype) |

## Drawbacks

Why should we not do this?


## Rationale and Alternatives

- Why is this the best possible design?
- What other designs are possible and why should we choose this one instead?
- What other designs have benefits over this one? Why should we choose an
  alternative instead?
- What is the impact of not doing this?


## Prior Art

If this has been done before by some other project, explain it here. This could
be positive or negative. Discuss what worked well for them and what didn't.

There may not always be prior art, and that's fine.


## Unresolved Questions

- What should be resolved before this RFC gets merged?
- What should be resolved while implementing this RFC?
- What unresolved questions do you consider out of scope for this RFC, that
  could be addressed in the future?


## Expected Response

How do might the wider community respond to this change? Who will be affected
by it and how? Who has advocated for this change? Who has advocated against it?

