# A Common Configuration API for QSL

## Summary

Specify and implement an API for assigning metadata to configuration objects ("Settings") which can be used to generate config screens.
Define a common metadata API for all mods that have configuration that can be modified in-game.


## Motivation

Mod configuration is a common problem that mods must solve. Defining an API within QSL that Quilt mods should use to define configuration metadata
unifies the GUI representation of configuration among mods and allows mods to avoid reinventing the wheel. Exposing a common API to query config metadata
allows config screens to be generated rather than relying on mods to manually construct screens using general-purpose GUI libraries.


## Explanation

### Definitions
- **Setting**: A single value and its associated metadata.
- **Category**: A logical grouping of Settings. A Setting may be a member of multiple Categories.
- **Validation Group**: A logical grouping of Settings used for validation. A Setting may be a member of only one Validation Group.
- **Formatter**: A function that maps a value to Minecraft `Text` suitable for display.
- **Setting Validator**: A function that is invoked to accept or reject the value of a single Setting.
- **Group Validator**: A function that is invoked to accept or reject the set of values in a single Validation Group.
- **Environment Policy**: A value that defines how a Setting interacts with the client and/or server.

A new QSL module, `quilt_config_metadata`, contains the implementation of this API. The API is built on top of the Loader config API, specialized
with metadata for Minecraft mods.

### Settings API Detail

A Setting has the following metadata.
- Widget Type (cycle, check box, select, text box, etc.)
- Label
- Tooltip
- Sections
- Validation Group
- Environment Policy

#### Widget Type

```java
public enum WidgetType {
  TOGGLE,
  CYCLE,
  SELECT,
  FREE_TEXT,
  SLIDER,
  NUMERIC,
  UNORDERED_COLLECTION
  ORDERED_COLLECTION;
}

MetadataType<WidgetType, ?> WIDGET_TYPE;
```

A Setting's Widget Type describes the type of widget that best represents the Setting.
- `TOGGLE` widgets are binary, typically displayed as check boxes or buttons.
- `CYCLE` widgets are small ordered lists of options, typically displayed as cycle buttons or radio buttons.
- `SELECT` widgets are large unordered lists of options, typically displayed as a drop-down or searchable menu.
- `FREE_TEXT` widgets are strings of text, typically displayed as a text box.
- `SLIDER` widgets are imprecise numeric values, typically displayed as a slider.
- `NUMERIC` widgets are precise numeric values, typically displayed as a text box.
- `UNORDERED_COLLECTION` widgets are made up of a collection of values with no defined ordering, all with the same Widget Type.
- `ORDERED_COLLECTION` widgets are made up of a sequence of values with a defined ordering, all with the same Widget Type.

The most appropriate Widget Type for a given Setting depends on the Setting's underlying data type and on any constraints a Setting's
value must obey.

If a Setting's Widget Type is `UNORDERED_COLLECTION` or `ORDERED_COLLECTION`, additional data is stored `WIDGET_ELEMENT_TYPE`.

```java
MetadataType<WidgetType, ?> WIDGET_ELEMENT_TYPE;
```

#### Label

```java
MetadataType<Text, ?> LABEL;
```

A Setting's Label is the simple display name of a Setting.

#### Environment Policy

```java
public enum EnvironmentPolicy {
  CLIENT_ONLY,
  SERVER_ONLY,
  CLIENT_SYNCED,
  SERVER_SYNCED
}

MetadataType<EnvironmentPolicy, ?> ENVIRONMENT_POLICY;
```

- `CLIENT_ONLY` Settings are specific to a player and are always editable.
- `CLIENT_SYNCED` Settings are specific to a player and may be overridden by the server.
- `SERVER_ONLY` Settings are specific to a level and are not sent to the client from a dedicated server.
- `SERVER_SYNCED` Settings are specific to a level and are sent to the client from a dedicated server.

// todo

## Out of Scope

Key binding configuration is not done in this API. However, the existence of this API should not preclude a separate API for key bindings.


## Drawbacks

Why should we not do this?

Mod developers may want to more tightly control their runtime config representation for querying or display purposes.


## Rationale and Alternatives

- Why is this the best possible design?
- What other designs are possible and why should we choose this one instead?
- What other designs have benefits over this one? Why should we choose an
  alternative instead?
- What is the impact of not doing this?

This RFC describes a common API between mods that define configuration and mods that consume configuration.
This approach gives the most control to mods that wish to consume multiple mods' configuration, e.g. to construct a config screen,
while avoiding including too many "batteries" that not all mods may use.

## Prior Art

If this has been done before by some other project, explain it here. This could
be positive or negative. Discuss what worked well for them and what didn't.

There may not always be prior art, and that's fine.

- ModConfig, a global config screen that extracts configuration metadata from mods that use Cloth UI.
- Fiber, a library that defines a rich config tree.


## Unresolved Questions

- What should be resolved before this RFC gets merged?
- What should be resolved while implementing this RFC?
- What unresolved questions do you consider out of scope for this RFC, that
  could be addressed in the future?
