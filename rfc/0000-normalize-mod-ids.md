
# Normalise Mod ID Separators

## Summary

Treat dashes and underscores as the same for mod id comparison purposes.

## Motivation

An occasional cause for confusion is when two different mods only differ by underscores (`_`) and dashes(`-`) in their mod-ids. Since those characters aren't usually used to discriminate between mods, I propose we consider them as the same for the purposes of mod resolution and solving.

## Explanation

I propose we normalise all quilt mod-ids by replacing every dash (`-`) and underscore (`_`) with an internal character (likely `#`, but any character that's not permitted by the specification, and is unlikely to be part of the specification in the future, would work. Ascii is preferable due to string optimisations though).

This normalisation would purely happen internally to quilt-loader, and wouldn't be shown to users, or exposed in methods like `ModContainer.getId()`. Instead this would allow dependencies and breaks to accidently target the wrong ID without causing issues, and would automatically cause mods with underscores or dashes to "provide" the other representations automatically.

For example, a mod with an ID "potion_craft-expanded" would result in:
- `QuiltLoader.getModContainer("potion_craft-expanded")` would return the mod
- `QuiltLoader.getModContainer("potion-craft-expanded")` would return the same mod
- The same mod is also returned for strings `potion_craft_expanded` and `potion-craft_expanded`
- `QuiltLoader.isModLoaded` would return true for all 4 strings
- `QuiltLoader.getEntrypoints` would return the same entrypoint list for each id.
- `QuiltLoader.getAllMods().stream().filter(mod -> mod.metadata().id().equals("potion_craft-expanded")).toList()` would result in a single mod in the list.
- `ModMetadata.provides()` would NOT contain provided entries for every possible combination, instead it would be empty (if no provided entries exist in the quilt.mod.json)


## Drawbacks

If someone intentionally uses dashes and underscores as a way to differentiate mods then their mods will be broken by this change. Personally I think this is unlikely though.


## Rationale and Alternatives

We could instead normalise all dashes to underscores (or vice versa) but that has a few issues:
* During debugging, it won't be easy to tell if a string contains a normalised modid or a non-normalised id, leading to more bugs.
* We'd have to choose a favourite character, which could result in a fairly lengthy debate. Sidestepping that seems useful.

We could even remove both characters entirely during normalisation, so `quilt_loader` would get normalised to `quiltloader`. However this has another problem, in addition to those listed above:
* It disallows the use of separator characters in mod-ids, which are genuinly quite useful.

## Prior Art

I'm not aware of forge, fabric, or quilt related projects doing this before.

## Unresolved questions

I'm not certain what character should be used, but '#' seems like a good candidate.
I'm not sure if calls to `QuiltLoader.getModContainer` or `QuiltLoader.isModLoaded` using the normalised ID should return the mod, null, or if that should be left unspecified.
