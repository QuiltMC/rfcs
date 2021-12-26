# Automated Test Framework and Team

## Summary

Extend Vanilla minecraft integrated test framework by:
 * having any documentation at all
 * creating Classes for common test types.
 * TestConcepts that simulate concepts.
 * offering Junit integration.
 * (later) a pool of tests that regression test vanilla

## Motivation

Automated testing is useful and allows developers to spend their time on other things.
Automated tests can be written by developers less experienced than lead developers, but will end up saving time for the lead developer.
The currently exposed testing framework is insufficient as it is, but it has potential.

To make it useable, it will need some or all of the proposed extensions.

A team will be made exclusively for this to avoid continuous discussion on where it belongs.
It will also seek to be in a public Beta by mid January 2022, which means it will try to work independently of the other teams.

## Explanation

Minecraft ships with it's testing framework
so fabric API, forge API and QSL habe exposed it to mod developers and we can use it.  

mojang dev explaining system: https://www.youtube.com/watch?v=vXaWOJTCYNg  

original PR exposing feature in fabric API:
https://github.com/FabricMC/fabric/pull/1622  

wiki stub(atow):
https://fabricmc.net/wiki/tutorial:gametest

It does not ship with documentation or test that test vanilla features.

It's currently practically unuseable:
* **it's hard to get started and understand**
* lots of stuff is hardcoded in a dozen places across several projects
* tests have to be with the rest of the source code of your mod
  * but there are work-arounds

therefore, first and foremost, we need to document how the framework works, how to use it, how you can add it to your projects, how to configure the build system to not end up shipping it to the end-user and make this documentation available to the user.

Initially, documentation will be kept along with the source code of the project, as Quilt wiki does not yet exist and the Author is banned from Fabric. We will hopefully be able to reach out to current users of the API to contribute to this documentation.

Boilerplate classes to cover common usages should make writing tests easy, the more high-level, the better. for instance: "test if this item gets duped [All]" should test whether a variety of common edgecases cause the item to be duplicated, be it pistons, inventories, disconnect, etc.

Junit integration is both pleasant to use and saves us having to code and maintain IDE integration and general/engine concepts.

This module (automated tests) should be able to be used in development environments and not affect what is dependent on for the mod itself.

This team might have to work with Standard Library and Build tools teams to pass certain changes, for instance, the reported hardcoding within them to be able to launch a test server instance, and replace them with a mutually agreeable solution.

As far as the author can tell, this project has no strong dependency on anything besides ofcourse the API that exposes the built-in test framework.

A should be formed exclusively for the development of this framework and will be dissolved upon it's integration with another quilt development team.
Team structure is determined by those participating in the team and recorded internally, but it is required to have a(1) technical lead as tie breaker and representative. The technical lead determines how this is determined.

Once the framework is mature enough to be released (hopefully end Q1 2022), responsibility for it's maintenance will hopefully be given to another team. At this time, a new team should be considered for maintaining the common test pool if this pool is created.

## Drawbacks
- It might end up an awkward duck, straggling between Standard library and build tools.
- It will require configuration that excludes it from being shipped to the end-user.
- a common pool of tests needs some developer time to quality control
- a separate team might lead to the code not being up to the standards of the the team that might take on the maintenance once it is finished unless coding standards are received early on.

## Rationale and Alternatives

The best possible design extends the test framework while being able to be forward,backward and side ported across versions and modding platforms.

It should be easy to get started, easy to use, flexible to allow most tests.

Alternative IDE integration could be accomplished by making interfaces of the classes that touch the Junit api, but i would see no need to do this for the initial release.

You could consider writing it entirely from scratch, no hooking into the in-game test framework already developed and maintained by Mojang.  
This would allow tests to work on versions where this internal framework is absent.
This could be done at a later date if desired.


"What is the impact of not doing this?"

Opportunity cost. The earlier you make the tests the better the pay off over time.
it is a kind of infrastructure for your code and it pays dividends over time.

right now, almost no developers use unit tests or in-game unit tests, and they will continue not using it until it gets useable.

## Prior Art

| titles                             | Just Enough Debug Tools                           | Elmendorf                               | Librarian Lib                                                         | Minecraft Testing Library                     | MinecraftJUnit                                       | native/api                    |
|------------------------------------|---------------------------------------------------|-----------------------------------------|-----------------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------|-------------------------------|
| platform                           | fabric                                            | fabric                                  | fabric                                                                | forge                                         | forge                                                | all                           |
| project links                      | https://github.com/FoxShadew/JustEnoughDebugTools | https://github.com/Ladysnake/Elmendorf/ | https://github.com/TeamWizardry/LibrarianLib/tree/1.17-quilt/testcore | https://github.com/alcatrazEscapee/mcjunitlib | https://github.com/BuiltBrokenModding/MinecraftJUnit | minecraft itself              |
| license                            | apache 2.0                                        | MIT license                             | LGPL-3.0                                                              | MIT license                                   | MIT license                                          | ARR EULA:free to mod??        |
| documentation                      | partial                                           | partial                                 | incomplete                                                            | yes                                           | none                                                 | (basically) none              |
| pre-made tests                     | yes(3)                                            | yes(2)                                  | yes(?)                                                                | none?                                         | yes(2+)                                              | none(but footage of examples) |
| has notion of packets/fake clients | no                                                | yes                                     | no?                                                                   | no?                                           | yes                                                  | no                            |
| integration with environment       | no?                                               | no?                                     | yes                                                                   | yes                                           | yes                                                  | no                            |
| tests resources                    | no?                                               | no?                                     | yes(translation)                                                      | unknown                                       | unknown                                              | no                            |



## Unresolved Questions

- what happens if the dimensions of a structure were larger than 48^3?
- Is the scope of this RFC too broad?
- Is it nescessary for a seperate team to be constructed, or is it possible to start working with another team immediatly.
- Who will own it after it is stable and released?
- A general Fake players class has different design needs than an automated testing player and will not be developed, these can later be merged if needed.
- What standards should the documentation be held to?