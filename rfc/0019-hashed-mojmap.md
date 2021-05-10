# Hashed Mojmap

## Summary

This RFC proposes replacing intermediary with a hashed version of Mojmap. Everywhere where intermediary is currently used, hashed Mojmap will be used instead.

## Motivation

- One of the biggest, if not the biggest, reliance of Quilt on upstream Fabric is intermediary. For as long as Fabric keeps updating to new Minecraft versions, we will be behind them on updating because of this limitation. It is also not a good thing for a hard fork to have a reliance on upstream.

- One of the primary goals of intermediary is to make sure that mods don't have to recompile between versions. However, when mods make the switch from Fabric to Quilt, they will have to recompile anyway. This gives us a unique opportunity to make breaking changes to the format of intermediary without breaking mods.

- Intermediary was a good solution at the time of its inception. Since then, Mojang has provided us with their official mappings, with permission to use them as long as we don't redistribute them. Having to match intermediary between versions introduces a high maintenance cost for every snapshot and potentially also limits the bus factor of the whole project. The majority of the time updating Fabric to snapshots is typically spent matching.


## Explanation

Hashed Mojmap will produce base-62 hashes of information taken from Mojmap to produce mixed-case alphanumeric names like the following:

- Classes: `Cls_hE89sr` (6 digits)
- Fields: `fd_p9TXc40` (7 digits)
- Methods: `md_fjH9ac8` (7 digits)

To produce the hashes, the Mojmap names are processed as follows:

1. Classes are converted from dotty format (`net.minecraft.a.Foo$Bar`) to slashy format (`net/minecraft/a/Foo$Bar`).
1. Overriding methods in subclasses are discarded if present.
1. Package names are removed from classes whose simple names are unique. That is, `net/minecraft/a/Bar` gets shortened to `Bar` if there is no other class named `Bar`, but if there are classes `net/minecraft/a/Foo` and `net/minecraft/b/Foo` that exist, then neither of them will be shortened. This includes references to these classes in method descriptors.
1. Method descriptors are replaced with an empty string for methods whose name is unique within the class that the method is declared.
1. Field names are converted to `fd_$x` where `$x` is the last 7 digits of the base-62 representation of the SHA-1 hash of the string `f$declaring_class.$field_name`.
1. Method names are converted to `md_$x` where `$x` is the last 7 digits of the base-62 representation of the SHA-1 hash of the string `m$declaring_class.$method_name$descriptor`.
1. Class names are converted to `Cls_$x` where `$x` is the last 6 digits of the base-62 representation of the SHA-1 hash of the class name.

The above steps are designed to preserve some of the benefits of intermediary as much as possible without the need for matching. Testing has shown that around 50% of class renames in Mojmap are only repackages, which are unlikely to affect hashed Mojmap. The descriptor removal is designed to preserve method names where only the descriptor has changed, while ensuring hashed Mojmap names are all unique.

### Probability of collision

As with any hash, a hash of Mojmap names is not guaranteed to be unique. Therefore, hash lengths were chosen such that the probability that we ever encounter such a collision within the lifetime of Quilt (and then some) falls below an acceptable level.

The probability of any two hashes colliding is p = 1 - 2ⁿ! / (2ᵏⁿ (2ⁿ - k)!), where n is the number of bits in the hash, and k is the number of items we are hashing. Factorials of this size are hard to compute exactly, so we will instead work with the accurate approximation p = 1 - exp(-k²/2ⁿ⁺¹).

To find out how many bits of the hash we need to get a certain probability, we can rearrange the formula: n = log₂(-k²/ln(1-p)) - 1.

Using the estimate that there are 5000 classes in Minecraft, and that we want a probability of 0.001, we get n = 34.54 bits. Rounding up to 36, we find that we need 6 base-64 digits to achieve this probability, so 6 base-62 digits to achieve approximately this probability as well. Using an estimate of 40,000 fields/methods in Minecraft, and the same target probability, you can work out in a similar way that 7 digits is appropriate.

If we got an entirely new set of mappings every snapshot, and snapshots were to occur on average once per week, then we would expect to get a class/field/method name collision about once every 192 years. But we don't get an entirely new set of mappings every snapshot, in reality the mappings are mostly the same from snapshot to snapshot, so it will take orders of magnitude longer than 192 years until we actually expect to run into any collision.

### Compatibility with Fabric

- Fabric mods will be remapped from intermediary to hashed Mojmap when they are loaded.
- Quilt Loader's mapping resolver will recognize the `intermediary` channel when Fabric mods are loaded, and remap it to the current runtime mappings (`hashed_mojmap` in production). Quilt Loader will also recognize the `hashed_mojmap` channel.
- The buildscript in Quilt's fork of Yarn will be updated to generate hashed Mojmap to Yarn mappings when built. When we start accepting PRs to our fork, the repository itself will be changed from using intermediary to hashed Mojmap. If necessary, a tool will be written to merge upstream from that point on.

## Drawbacks

- Hashed Mojmap names are more likely to change than intermediary, although not by as much as popularly believed.
  - After applying the aforementioned corrections for package names, we found that Mojmap class names are twice as likely to change when the intermediary doesn't, than intermediary changes when Mojmap doesn't. 
  - However, after manually checking the nature of these renames, it turned out that some of them were renamed to reflect a change in the purpose of the class, but the intermediary still matched because the classes were most similar. It is difficult to quantify exactly how common this is, as it requires going through all renamed classes manually.
- It is possible that lambda methods change when intermediary doesn't, although this is not as likely in comparison to intermediary as popularly believed.
  - Lambda methods are indeed included in Mojmap, and are in the javac format `lambda$enclosingMethod$index`.
  - This name will only change if the enclosing method name changes, or lambdas are inserted before that lambda. Other lambdas being inserted into this method is also likely to fool Matcher and therefore change the intermediary.
  - Not much effort is put into matching lambda methods that aren't auto-matched anyway. Lambda methods have been known to change intermediary with even the tiniest change in their content.
- Mixed case names are harder to remember than all-lowercase names.
  - Mixed case has been chosen to decrease the likelihood of a name collision, but it is still extremely unlikely that any two names will differ only by case.
  - Any good mappings lookup should be case-insensitive.
  - Eclipse is known to have case-sensitive auto-completion, but the right class/field/method will come up after typing only the first couple of characters, making it unnecessary to remember the whole thing for auto-completion.

## Rationale and Alternatives

- We could continue using Fabric intermediary.
  - The problems mentioned in the "Motivation" section would then persist.
  - We would miss our opportunity to change from intermediary without breaking mods, and would have to break mods if we decide to change in the future.
- We could fork intermediary and diverge but otherwise keep it the same.
  - Does not improve the maintenance burden of updating to the latest Minecraft version.
  - Does not handle forks of Minecraft versions (e.g. April fools snapshots) very well.
- We could auto-match using Mojmap using the aforementioned techniques to minimize the chance of bad renames.
  - Still does not handle forks of Minecraft versions (e.g. April fools snapshots) very well.
  - Requires a server to be online to run the update. If ever Minecraft modding becomes unpopular it is better to have a decentralized approach that will work forever.
- We could use Mojmap directly in place of intermediary.
  - Taints Yarn contributors when seen in stack traces and unmapped code.
  - More likely to break mods than hashed Mojmap because no package name correction has been done.

## Prior Art

Forge has chosen to use Mojmap directly. Unlike Forge, we have our own thriving, and official, mapping set to cater to, Yarn, and we cannot allow Yarn contributors to be tainted.

## Unresolved Questions


## Expected Response

This approach has so far been controversial, but there has been much support for it too. Discussion will take place in the PR comments of this RFC, with a poll after both sides have had enough time to put forward their arguments.

Remember one of the reasons Quilt was founded:

> Faster Iteration & Experimentation - The Quilt project aims to fail fast. We would rather try something and fix it then spend countless months debating whether to move forward with it in the first place.

