## Summary

This RFC proposes a possible standardized way to synchronize feature toggles (boolean values) between the client and server.


## Motivation

Currently there is no easy way for developers to implement feature toggles unless they are using the custom payload packets directly with the networking API which requires more work.

The drawbacks of developers using non-standardized protocols is the impossibility to write a common server mod to disable some features in some mods. This aims to reduce work to multiple different parties.


## Explanation

### How would this work?

The idea behind this RFC is for mod developers there would be an API for which mods can register a feature, for a given feature you can set a client value (from the client config), and query if the feature is enabled or not (by checking if the server allows it then it returns the client value, else it returns false).  
Each features are identified by an ID.  
Servers can send a packet changing the values of those features by ID.  
When a client joins a server it would send a or multiple packet(s) describing which features exist on the client.

Mods can set whether the server must explicitely authorize the features to enable it, or explicitely disallow the features to disable it.

### Networking specification

To standardize this feature toggle system it needs a standardized networking specification so anyone can re-implement it with a custom API for different modding platforms (Spigot, Sponge, Forge, etc).

The networking specification is divided in multiple parts:
 - networking types
 - networking packets

All types used are either defined in this RFC or are used in MC's own networking. `S->C` means Server to Client and `C->S` means Client to Server.

In this specification, the type `string` without parenthesis describes an UTF-8 string with a maximum length of `32767`.

### Array `A[]`

An array holds a list of values of the specified type.
It also contains the size `N` of the array.

| Fields | Type     | Description                                 |
|:------:|:--------:|:--------------------------------------------|
| size   | [VarI32] | The size of the array.                      |
| 0      | A        | The first element of the array if present.  |
| 1      | A        | The second element of the array if present. |
| ...    | A        | An element of the array if present.         |
| N-1    | A        | The N-1 element of the array if present.    |

#### FeatureList

| Fields    | Type     | Description                                                     |
|:---------:|:--------:|:---------------------------------------------------------------:|
| namespace | string   | The namespace of the features. Can be considered the modid too. |
| features  | string[] | The features inside the given namespace.                        |

#### FeatureAuthorization

Can be described as an enum with two possible values: `ALLOW` or `DENY`.

 - `ALLOW` means that the client should follow the user configuration's and is allowed to enable the related feature.
 - `DENY` means that the client is forbidden to enabled the related feature.

In networking this can be represented as a boolean with `ALLOW` being `true` and `DENY` being `false`.

#### Feature

Represents a feature with an authorization value.

Can be seen as a tuple of a string for the identifier path, and a [FeatureAuthorization].

#### FeatureOverrideList

Represents a list of features with authorization values.

| Fields    | Type     | Description                                                     |
|:---------:|:--------:|:---------------------------------------------------------------:|
| namespace | string   | The namespace of the features. Can be considered the modid too. |
| features  | [Feature]\[\] | The features inside the given namespace.                        |

#### `quilt:available_features` packet

Describes the available features on the client to the server. This packet can be split into multiple packets.

Direction: `C->S`

| Fields      | Type              | Description                                                     |
|:-----------:|:-----------------:|:---------------------------------------------------------------:|
| expect_more | bool              | Determines whether the server should expect follow-up packets or not. If a `quilt:available_features` packet is sent after a similar packet which had `expect_more` field set to `false`, then it should override the feature list. |
| features    | [FeatureList]\[\] | The features available on the client.                           |

#### `quilt:features_override` packet

Describes the overriden features on the client by the server. This packet can be split into multiple packets.

Direction: `S->C`

| Fields      | Type                      | Description             |
|:-----------:|:-------------------------:|:-----------------------:|
| features    | [FeatureOverrideList]\[\] | The features overrides. |

### Who implements it?

In the case of Quilt, this would likely be its own QSL module, or integrated in the QLS networking module.

In the case of other modding platforms, anyone is free to re-implement the protocol and offer an API.

## Drawbacks

This could add an extra burden to some mod developers and the QSL.

It could also place too much trust into the client which should be avoided as the number one1 rule of client/server relationships is do not trust the client.


## Rationale and Alternatives

- This design allows for a generic server-mod to disallow/allow features due to the standardization.
- If we do not implement a standard feature toggle, then mods allowing servers to disallow/allow features will be more rare thus pushing some server admins to be way more strict about mods.


## Prior Art

 - [LambdaControls networking](https://github.com/LambdAurora/LambdaControls/wiki/LambdaControls-Networking#lambdacontrolsfeature)


## Unresolved Questions

- Is the `quilt` namespace appropriate for this networking specification?
- How can the networking specification be improved?


## Expected Response

The wider community might respond positively to this change, hopefully. This RFC aims to simplify the life of some mod developers and server administrators.  
Cheat/hacked mod developers will most likely straight up ignore this feature altogether due to their nature.

[VarI32]: https://wiki.vg/Protocol#VarInt_and_VarLong "wiki.vg documentation"
[FeatureAuthorization]: #FeatureAuthorization "FeatureAuthorization enum"
[FeatureList]: #FeatureList "FeatureList object"
[Feature]: #Feature "Feature tuple"
[FeatureOverrideList]: #FeatureOverrideList "FeatureOverrideList object"
