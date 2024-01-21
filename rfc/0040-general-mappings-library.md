# General Mappings Library

## Summary

Currently Quilt has several different libraries dedicated to working with mappings:
* Tiny Mappings Parser (TMP)
* Tiny Remapper (TR)
* Lorenz Tiny
* Stich

These libraries all have very similar overlap, but because they are seperate projects, much of the code is dupilcated (ie 3 different implementations of a mapping tree). This RFC proposes one library to integrate all 4 of these projects into one project.

## Motivation

Currently lots of code is either duplicated or not used in the rest of the toolchain, increasing the total amount of code that needs to be maintained and makes it hard to tell what should be included in what library. For example, Stitch is mostly intended as a command line tool with the goal of manipulating mappings easily. Stitch is used in both Quilt Mappings and Quilt Gradle. However, Stitch is somehow closely integrated into the Enigma process for Quilt Mappings, and is not used in Quilt Gradle. For example, the `compareTriples` method in `CommandReorderTiny` in Stitch is a useful comparison method, but should not be implemented in Stitch. `EntryTriple` is a TMP class, and should implement `Comparable<EntryTriple>` as the functionality of `compareTriples` should not be limited to Stitch.

While mappings has some of the most extensive tooling in the Quilt toolchain, a single minecraft update can bring stuff to a serious halt, such as 21w37a. Record remapping was broken, and LVTs needed to be fixed. While I could be wrong, I believe Stitch handled the snowman LVT conversion, however, Fabric and I chose to patch the new `$$n` LVT names inside tiny remapper. Which ever one is better, the fact that both projects overlap on what they believe is their scope show that features need to be closer together, not farther apart. 

## Explanation

I propose a new project, `Tiny Utils` that will be a combination of the four projects above.

`Tiny Utils` will be a single repository, but it will have multiple subprojects. It is still useful to have multiple jars for the different sub projects, as both Stitch and Tiny Remapper have command line utility. 

The main subproject will still be Tiny Mappings Parser, and will be expanded to include some of the Stitch features internally, instead of having Stitch completely rebuild a mapping tree. The legacy code will be removed and the different source sets will either be merged into other subprojects or different quilt projects (See `MixinRemapper`).

Lorenz Tiny will remain relatively unchanged.

Tiny Remapper will have the tiny mapping implementation removed and will be extensively rewritten to simplify the addition of new features through configuration classes.

Stitch will also have its tiny mapping implementation removed. I believe that all of the TinyV1 features can also be removed. If possible, Stitch will also have its command line features be implemented in a similar fashion to Tiny Remapper's as that is very clean. Its Enigma features will be moved into Quilt Mappings.

I propose that the jar merging features be moved to a seperate project entirely, but the limited scope of that feature plus the fact that it is used closely along with the rest of the features from Stitch make it unnessicary to bring in another dependency.

Another major goal is to document code through both javadocs and with comments, as currently the implementation of these projects is mostly known only by the creators.

The sub projects will still be published to the maven under the same artifact names, but should have their major version bumped to signify major changes.

## Drawbacks

This will take time. I am about to start college in person and will not have as much time as I have. In addition, many projects will need to be updated and the development process will be slow. However, I believe with enough dedication, the Quilt Mappings team should be able to get to this done by winter, and hopefully before 1.18 release. This should not affect much outside the internal features, as code is still published to the same artifacts on the maven. 