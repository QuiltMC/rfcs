# Summary

This RFC describes plugins for the Quilt mod loader, and aims to describe the
purpose(s), functionality, and implications of such a system. At a high level,
loader plugins will allow developers to control the mod loading process itself,
giving them access to powerful tools for defining, loading, and removing mods.


# Motivation

One of the most common barriers of entry for new mod users is having to download
and install APIs and libraries in addition to the mods they want to use. Adding a
way for Quilt Loader to fetch dependencies dynamically will help ease this
headache and make Quilt a more appealing option for new mod users. Plugins will
allow us to add such behavior dynamically when required, while allowing the
behavior to be disabled if a user does not want or need it.

Plugins will allow third-party libraries to define complex mod resolution behavior
such as [allowing mods to declare "API" classes that other mods can declare implementations for](https://github.com/FabricMC/fabric-loader/issues/343).

Projects such as [PatchworkMC](https://patchworkmc.net/) and [Sponge](https://www.spongepowered.org/) will be able to run non-Quilt mods ([Forge](https://forums.minecraftforge.net/) and Sponge mods, respectively) on top of the Quilt platform by
performing transformations during the mod loading process.

Once CHASM is implemented as a universal standard for bytecode transformations
across Quilt, this system will allow us to remove the special case code for Mixin
and access wideners and reimplement those as loader plugins instead. This will make
developing third-party libraries that modify bytecode in unique and interesting
ways possible.

Loader plugins could also be used to [add mod containers for mods that don't do anything](https://github.com/FabricMC/fabric-loader/issues/175), although a
better system for that will likely be implemented separately into Loader itself.

A loader plugin could be developed to load mods from instance-specific folders instead of the single shared mods folder.

By breaking parts of the mod loading process into various projects, we can keep
each repository focused on its own specific mission. This will allow for more
finely grained separation of responsibility.


# Explanation

The addition of Quilt Loader plugins will make Quilt Loader a plugin loader, not
a traditional mod loader. Much of the existing mod loading behavior will be
moved to one or more first-party loader plugins that are installed by default,
keeping only the bare minimum amount of code required for mod resolving in Loader
proper. Loader plugins will be capable of effecting the mod loading process by
adding mods and even adding other plugins. Plugins will also have the ability to
open/create UI elements which can be used for as simple a thing as prompting the
user for confirmation, or as complex of one as writing a full-scale mod launcher
as a loader plugin.

## Application
Loader plugins are initially added from the classpath, allowing default loader
plugins for an instance to be added via the profile json. All plugins are then
applied in cycles. During each cycle, the following things happen:
- Each plugin is given a chance to add any plugin candidates
    - Each new plugin added this way is given a chance to add any plugin candidates.
      Rinse and repeat.
- Each plugin is given a chance to add any required or tentative mod candidates,
  rules, or options
- Quilt Loader will evaluate the sat4j tree
    - Each plugin will be given a chance to claim unsatisfied rules for resolution
    - Each plugins resolvers will be run sequentially

If any changes to the mod set have taken place during a given cycle, another cycle
will be run. Otherwise, the game will either launch or crash, depending on whether
or not all rules have been satisfied.


## API Surface

Below is a tentative plan of what the surface level API classes might look like.
Everything here is subject (and likely) to change, but helps provide a visual
idea of how developers might use this feature down the road.

<details>
    <summary>QuiltPluginContext</summary>

```java
/**
 * Passed to loader plugins to define what actions they are able to take.
 */
public sealed interface QuiltPluginContext permits QuiltPluginContextImpl {

/**
 * The plugin that this context is for. This method is useless, it just indicates that every other method here has an 
 * implicit paramater of "The Loader Plugin" for the UI / logging to use in some way
 */
QuiltLoaderPlugin plugin();

void addCandidate(ModCandidate candidate);

void addCandidate(PluginCandidate candidate);

/**
 * Adds a tentative mod candidate which indicates that downloading / fetching a new mod will fix a rule somewhere.
 * This tentative mod won't be kept around to the next cycle - instead the resolver is called to actually download
 * the mod if {@link QuiltLoaderPlugin#canResolve} returns true after each plugin has been checked.
 *
 * @param resolver a future that will resolve this candidate, with value of null if it succeeds and an error message
                   if it fails
 */
void addTentativeCandidate(String group, String modId, Version version, Future<@Nullable String> resolver);

/**
 * Adds a rule to the current solver.
 */
void addRule(Rule rule);

/**
 * Adds a LoadOption to the current solver. All existing rules will have Rule#onLoadOptionAdded called, and all plugins 
 * will have ILoaderPlugin#onLoadOptionAdded called.
 *
 * If this is an ITentativeLoadOption then it will be removed at the end of the cycle, and handled by whatever plugin 
 * added it.
 */
void addOption(LoadOption option);

/**
 * Gets the metadata for a given mod.
 *
 * Because the mods in QuiltLoader only reference fully loaded mods, this method can be used during the mod loading process
 * to get the metadata for any candidates (tentative or not) that were present prior to this cycle.
 */
ModMetadata getMetadata(String modId);
}
```
</details>

<details>
    <summary>QuiltLoaderPlugin</summary>

```java
/**
 * @param <T> the types of resolver this plugin can resolve
 */
interface QuiltLoaderPlugin<T extends Future<@Nullable String>>  {
/**
 * Called once per cycle as the first action in the cycle.
 * 
 * This is where mods can be added with {@link QuiltPluginContext#addCandidate} and
 * {@link QuiltPluginContext#addTentativeCandidate}.
 */
default void run(QuiltPluginContext context) {}

/**
 * Called once per cycle after the sat4j solving has finished, but before any resolvers are run.
 *
 * Should NOT invoke the resolvers.
 *
 * @return true if all of the resolvers can be called, false otherwise
 */
default boolean canResolve(List<T> resolvers) {
    return false;
}

/**
 * Called if loader can't simplify this error down into any of the other error handling methods.
 * @return True if this plugin did something which will solve / change the error in future,
 *         and so loader won't ask any other plugins to solve this.
 *         Loader will temporarily remove this rule so it won't be sent to #handleOtherErrors again in this cycle.
 *         If this returns false then no rules will be removed, and instead loader will assume that
 *         the error has been handled in some other way. (and it will promptly crash if you haven't)
 */
default @Nullable Rule handleOtherErrors(QuiltPluginContext context, List<Rule> errorChain) { return null; }

/**
 * @param dep The dependency which is missing completely. If you can find a valid source for this then you should add 
 *            it with {@link QuiltContext#addTentativeCandidate()}
 * @return True if this plugin did something which will solve / change the error in future,
 *         and so loader won't ask any other plugins to solve this.
 *         Loader will temporarily remove this rule so it won't be sent to #handleOtherErrors again in this cycle.
 *         If this returns false then no rules will be removed, and instead loader will assume that
 *         the error has been handled in some other way. (and it will promptly crash if you haven't)
 */
default boolean handleMissingDependencyError(QuiltPluginContext context, ModDependencyRule dep, List<Rule> fullErrorChain) {
    return handleOtherErrors(ctx, fullErrorChain);
}

/**
 * Called whenever a new LoadOption is added, for plugins to add Rules based on this. (For example the default plugin 
 * creates rules based on the dependencies and breaks sections of the quilt.mod.json if this option is a 
 * {@link MainModLoadOption}).
 * <p>
 * Most plugins are not expected to implement this.
 */
default void onLoadOptionAdded(QuiltPluginContext context, LoadOption option) {}
}
```
</details>

<details>
    <summary>ITentativeLoadOption</summary>
    
```java
/**
 * {@link LoadOption}s can implement this if they must be processed at the end of the cycle in order to either be
 * added as a normal LoadOption, or removed automatically.
 */
public interface ITentativeLoadOption {

}
```
</details>

<details>
    <summary>LoadOption</summary>
    
```java
/**
 * A boolean option, which quilt loader will resolve down to "true" or "false" according to the {@link Rule}s added by plugins.
 */
public abstract class LoadOption {

}
```
</details>

<details>
    <summary>ModDependencyRule</summary>

```java
sealed abstract class ModDependencyRule extends Rule /* implemented by quilt */ {
abstract ModCandidate from();

abstract VersionLimits versions();

abstract @Nullable ModDependencyRule unless();

abstract List<ModLoadOption> valid();

abstract List<ModLoadOption> invalid();
}
```
</details>

<details>
    <summary>Rule</summary>

```java
/**
 * A boolean expression, which controls the links between {@link LoadOption}s
 */
public abstract class Rule {

/**
 * Invoked for every Rule by quilt-loader whenever a load option is added, in order to update this rule.
 * For example {@link ModDependencyRule} uses this to add ModLoadOption to it's valid and invalid lists.
 */
public abstract void onLoadOptionAdded(LoadOption option);

/**
 * Invoked when tentative LoadOptions are removed at the end of a cycle.
 */
public abstract void onLoadOptionRemoved(LoadOption option);

/**
 * Called at the start of each cycle to encode this rule in sat4j.
 */
public abstract void define(IRuleDefiner definer);
}
```
</details>


# Drawbacks

All existing behavior can be implemented using loader plugins. As such, the largest
drawback may be the increased scope for the Quilt project as a whole, as we will no
longer be limited to mods specifically. This is less of a drawback and more of an
organizational change that needs to be considered moving forward.

Another drawback is the potential confusion of end users that may stem from a more
complicated mod loading process. If a plugin fails to provide a required mod which
therefore causes a crash, it's possible that users may receive error messages that
further confuse them rather than cause clarity. Careful consideration will have to
be put into the development of Quilt's official loader plugins to ensure that each
of our loader plugins emits thorough and helpful error messages upon failure.


# Rationale and Alternatives

While we could continue with the current status quo of loading mods from the mods
folder and only the mods folder, this is a rigid and outdated way of doing so that
does not allow for iteration and innovation.

If loader plugins are not implemented, we would have a very hard time
differentiating ourselves from other mod loaders. Plugins will allow both us and
the community itself to add features to Quilt loader easily, making it much easier
for innovation in where mods come from and how they are loaded.


# Prior Art

If this has been done before by some other project, explain it here. This could
be positive or negative. Discuss what worked well for them and what didn't.

Similar projects have been looked at by others in the past:
- FabLabs very briefly explored this concept [here](https://github.com/FabLabsMC/fabric-loader/tree/feature/modproviders).
- [@Chocohead](https://github.com/Chocohead) has implemented [loading extra mods alongside Fabric Loader](https://github.com/Chocohead/Modjam/blob/master/src/com/chocohead/sm/loader/PreLoader.java), but not as actual Fabric mods.

None of them have been implemented into the mod loader itself, and as such this is
the first project to allow such liberal changes to the mod loading process.


# Unresolved Questions

Before loader plugins can be considered fully implemented, CHASM will need to be
functional so that all bytecode modifications can go through a single source of
truth.

For the purposes of this RFC, I did not go into the drawbacks of
dependency downloading specifically, as this RFC is interested more generally
in the idea of loader plugins as a whole. How dependency downloading is implemented
and other concerns regarding that will take place at a later date, in another RFC.


# Expected Response

Loader plugins have been one of the earliest features Quilt promised to deliver. As
such, I expect the community will largely be excited to see this feature
implemented. Projects such as [Bundle Installer](https://github.com/FoundationGames/Bundle-Installer) will be able to make use of loader plugins right from day one, and
I expect that we'll see a lot of similar projects that aim to make the mod
installation process as easy as possible for end users.
