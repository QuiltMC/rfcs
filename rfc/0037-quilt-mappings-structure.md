# Quilt Mappings Structure

## Summary

As the Quilt Mappings Repositiory is close to opening and being ready for changes, a guideline for how the repository is run should be evaluated. 
For this plan, there will be a branch for every major release of Minecraft, i.e. `1.16.5`, `1.17`, `1.17.1`, which will be stable with minor changes, and these will be the release branches. The first release on a new release branch will always have no new names over the previous release branch.
The `main` branch will be for developement only, meaning no commits happen on the release branches without happening on the `main` branch first. 

This RFC also outlines how pull requests are accepted.

## Motivation

Some of the motivation behind this change is that the snapshot phases for mappings have often introduced refactors, which if not backported, can cause quite a bit of pain for modders updating to the latest version, as the `migrateMappings` task is not used as much as it should. 
The first release should help with this, as they will only have to deal with changed/removed methods, and not new names. 
In addition, the build number for mappings can often jump from `1` to `15` overnight, then stagnate for a long time. 

The motivation behind a single development branch stems from the fact that Yarn effectively works like this already, as most versions have their branch start from the end of the previous version's. 
The only downside to this change is that it could be harder to find the mappings for a snapshot, but this could be solved with tags on new snapshots.
The snapshot branch would allow for many new changes to be merged in at once, then be ported as a single new version to the release branch. This also removes new releases that happen with no actual changes to the mappings

For refactors and backports, I believe a main development branch will make things easier, as the changes to the current Minecraft version can be cherrypicked back 90% of the time.

## Explanation

### Branches

* Branches for every major release version of Minecraft, with the initial commit be just the changes from the previous major version.
* A single branch for development.

The releases branch would publish to the releases maven, while snapshots would be published to the snapshot maven. This would help people scanning the maven find the latest version easier.

### Versioning

With the developement branch and release branches, it might be possible to have consistent versioning across Minecraft versions, so the the gradle dependency name could be `org.quiltmc:quilt-mappings:${mappings-version}-${minecraft-version}`

### Pull Requests

New mappings will always happen to the `main` branch, and should be reviewed by at least 3 members of the mappings team. (This number can change depending on how many people are on the teams, this is the number Fabric used).
Pull requests should have the `snapshot` or `refactor` target. 
* The `snapshot` pull requests should only happen during snapshot phases of Minecraft's development, and will only change mappings introduced between major versions.
* The `refactor` pull requests can happen at any time, and should only update mappings from the current and previous versions of Minecraft. This will help increase consistentcy between names across versions. If a refactor wants to change names that follow the new changes, but are in only one of the versions, another pull request should accompany it.

## Drawbacks

Yarn works. While backports are slightly janky and not required, there is nothing seriously broken about how Yarn currently works. This RFC only asks for changes because it can and to spur discussion. 

Also finding mappings for snapshots could be harder to find, but since most modders do not target these versions, it won't be hugely impactful

## Unresolved Questions

- Should mappings try to be consistent against a wider range of versions?
