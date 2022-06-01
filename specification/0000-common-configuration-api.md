# A Common Configuration API for QSL

## Summary

Define a common metadata API for all mods that have configuration that can be modified in-game. These metadata should be sufficient to automatically
generate user-friendly configuration screens for mods. In order to achieve this, this RFC defines metadata necessary to display configuration to the
user and to convert user input into configuration values.


## Motivation

Mod configuration is a common problem that mods must solve. Defining an API within QSL that Quilt mods use to define configuration metadata
unifies the configuration UI model among mods and allows mods to avoid reinventing the wheel. Exposing a common API to query config metadata
allows config screens to be generated rather than relying on mods to manually construct screens using general-purpose GUI libraries.


## Explanation

### Definitions
- **Setting**: A single value and its associated metadata.
- **Category**: A logical grouping of Settings. A Setting may be a member of multiple Categories.
- **Validation Group**: A logical grouping of Settings used for validation. A Setting may be a member of only one Validation Group.
- **Value Parser**: A function that converts user input to the underlying data type of the Setting.
- **Formatter**: A function that maps a value to Minecraft `Text` suitable for display.
- **Setting Validator**: A function that is invoked to accept or reject the value of a single Setting.
- **Group Validator**: A function that is invoked to accept or reject the set of values in a single Validation Group.
- **Environment Policy**: A value that defines how a Setting interacts with the client and/or server.

A new QSL module, `quilt_config_metadata`, contains the implementation of this API. The API is built on top of the Loader config API, specialized
with metadata for Minecraft mods.

### Settings API Detail

A Setting has the following metadata.
- Widget Type
- Value Calculator
- Label
- Tooltip
- Categories
- Group Validator
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

#### Value Parser

```java
@FunctionalInterface
public interface ToggleValueParser<T> {
  Optional<T> parse(boolean input, ValidationDiagnostics diagnostics);
}

@FunctionalInterface
public interface CycleValueParser<T> {
  Optional<T> parse(CycleDirection direction, boolean includeHiddenOptions, ValidationDiagnostics diagnostics);
  enum CycleDirection { FORWARD, BACKWARD }
}

@FunctionalInterface
public interface SelectValueParser<T> {
  List<T> parse(String search, ValidationDiagnostics diagnostics);
}

@FunctionalInterface
public interface FreeTextValueParser<T> {
  Optional<T> parse(String input, ValidationDiagnostics diagnostics);
}

@FunctionalInterface
public interface SliderValueParser<T> {
  Optional<T> parse(double fraction, ValidationDiagnostics diagnostics);
}

@FunctionalInterface
public interface NumericValueParser<T> {
  Optional<T> parse(Number input, ValidationDiagnostics diagnostics);
}
```

A Value Calculator handles user input on a given widget type in order to produce a value for a Setting. Depending on the UI widget, user input
may include text, key presses, and button presses.

// all -> turn raw input into a value (maybe Setting Validators should do this?)
// cycle -> get next/previous value
// slider -> get next/previous value + calculate value from percentage (and visa-versa)
// collections -> add/insert/remove/move an element

#### Label

```java
MetadataType<Text, ?> LABEL;
```

A Setting's Label is the simple display name of a Setting.

#### Tooltip

```java
@FunctionalInterface
public interface TooltipSupplier<T> {
  Text getTooltip(T value);
}

MetadataType<TooltipSupplier<?>, ?> TOOLTIP;
```

A Setting's tooltip is a brief description of the current state of the Setting, dependent on the Setting's value.

#### Categories

```java
MetadataType<Collection<Identifier>, ?> CATEGORIES;
```

A Category is a logical grouping of Settings; for example, video, audio, accessibility, or gameplay. A Setting may be a member of any number
of Categories. Categories are intended to be used to partition Settings into broad groupings for display purposes.

This RFC defines a standard Category `c:accessibility`, to be used for Settings that are used to make a mod more accessible.

#### Setting Validator

Settings will use the existing `Constaint<T>` interface in Quilt Loader's configuration API to define Setting Validators.

#### Group Validator

```java
@FunctionalInterface
public interface GroupValidator {
  void validate(ValidationDiagnostics diagnositics);
  boolean equals(Object o);
}

MetadataType<GroupValidator, ?> GROUP_VALIDATOR;
```

A Group Validator is used to validate properties that pretain to the relationship among multiple Settings. For example, two Settings that
represent the minimum and maximum value of a range may have a Group Validator that ensures the maximum is always greater than or equal to
the minimum. It is intended that Group Validators that perform the same validation compare equal so that consumers of this API do not
run validation logic multiple times.

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
  - If a `CLIENT_SYNCED` Setting is a member of the Category `c:accessibility`, then the API shall issue a warning.
- `SERVER_ONLY` Settings are specific to a level and are not sent to the client from a dedicated server.
- `SERVER_SYNCED` Settings are specific to a level and are sent to the client from a dedicated server

## Drawbacks

Why should we not do this?

This API depends on (and is therefore limited by) Quilt Loader's configuration API.

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

- [ModConfig](https://github.com/kvverti/mod-config), a global config screen that extracts configuration metadata from mods that use Cloth UI.
- [Fiber](https://github.com/FabLabsMC/fiber), a library that defines a rich config tree.


## Unresolved Questions

- What should be resolved before this RFC gets merged?

### Implementation
- Which other common formatter/validator/widget type instances should this API provide? E.g. enum, registry entry, `Identifier`, etc.
- How strict should the API be on the well-formedness of metadata?

### Out of Scope
- Key binding configuration is considered out of scope for this API because Minecraft handle key bindings differently from other game options.
- Creating a standard consumer of this API, e.g. a config screen generator may be specified in another RFC.
