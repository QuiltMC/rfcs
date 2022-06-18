
## Summary

This document outlines the process for creating an official Mod Menu for usage with Quilt alongside an accompanying team that would maintain it. This mod menu will act as a way to view loaded mods, change configs, read credits, and get links to contacts from within Minecraft.


## Motivation
   As of right now, we do not supply an official Mod Menu for Quilt. The current solution is TerraformerMC's Mod Menu for Fabric, and that does not fully support our additions (Full Quilt support, Quilt Config, quilt.mod.json spec improvements, etc.). Having a standardized Mod Menu would allow for support for these features. Furthermore, it would also be an officially endorsed mod, which would help us lead players to it, and we at The Quilt Project would be able to ensure its quality.


## Explanation

### Integration
A toggle in the Quilt Installer will allow for this mod to be installed at the same time as loader

### Why are we doing this?
Native Quilt Support, which lets us add additional features from things like quilt.mod.json or Quilt Config without issue.
Standardization, allowing an official solution compared to Fabric's unofficial Mod Menu

### Why not a fork of Terraformer's Mod Menu?
The author of the Mod Menu himself, Prospector, said that ModMenu is "janky" - it is likely not worth the effort to clean up code AND integrate with other Quilt Standards. 

### How will we deal with Fabric Mods that require configs?
We will supply the Mod Menu API to allow for backwards compatibility with configs in Fabric mods.


## Drawbacks

### Potentially Unnecessary
   It maybe be considered to be unnecessary to make this as the current Mod Menu solution on Fabric works fine. It also may complicate the user experience to have multiple mod menu mods. 
   
Fabric's Mod Menu is a separate mod, however, so it can simply be removed if a user finds it distracting.

### Removes User Choice

Some people may not want to be forced upon a Mod Menu in that case we are taking away their choice on whether or not to have a Mod Menu. 
    
This, however, is almost a non-issue; mod menus are often lightweight, and they are often the best way to abstract access to a mod's config. 

### Burden of Maintenance

Some people may view this as yet another Libarary / Mod to maintain every update and push new features to. 

This is probably the most serious issue - an official Quilt Mod Menu will require official support - that means PR triage, updating between versions, and ensuring that as changes occur to things like the QMJ, the Mod Menu is appropriately updated. We propose the creation of a new team, the Mod Menu team, that would be responsible for its management.



## Rationale and Alternatives


- This allows for features from Loader and QSL to be used within the mod more effectively, and it also allows for easier communication within our teams in designing necessary features
- As for alternatives, we could wait for a Quilt Mod Menu to show up, we could (against the author's recommendation) fork the TerraformersMC Mod Menu (and still have to create a new team for support). The proposed path is the best option.


## Prior Art

- TerraformerMCs Mod Menu
    - Benefits
        - Very well designed UI. Its visually pleasing and easy to use.
        - API to allow for support of other config systems
        - Supports showing parent/JiJed mods
    - Drawbacks
        - No native Quilt support. Thus it cant have support for features in Quilt such as support for Loader Plugin loaded mods, Quilt Config, Contributor Roles.
        - Its downloaded and installed manually, causing confusion amongst new users

- Forge's Mods screen
    - Benefits
        - No installation needed, thus making it more standard and causing less confusion amongst users
    - Drawbacks
        - Not particularly well-designed - can be a bit confusing to deal with.
        - Does not support alternative config systems.
        - Does not support showing parent/JIJed mods


## Unresolved Questions
- Specific details about the creation of the Mod Menu Team 
