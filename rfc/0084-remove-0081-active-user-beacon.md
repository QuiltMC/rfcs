# Revert RFC 81: Active User Beacon

## Summary

This RFC reverts the RFC 81: active user beacon.

## Motivation

A significant number of developers and users have expressed unhappiness at the current opt-out behaviour of the active user beacon, and want it changed or removed. Since not annoying users is important, I'd like to remove the beacon entirely.

Some people have made the argument that other projects have similar amounts (or significantly more) user tracking. I don't think this justifies this inclusion - each case of tracking should be looked at separately.

## Explanation

This requires three changes:

- The `specification/0081-active-user-beacon.md` file should be removed from this repository.
- The `ActiveUserBeacon.java` file in the quilt-loader repository should be removed, as well as any references to it in the repository.
- The infrastructure team should take down the server at `beacon.quiltmc.org`.


## Drawbacks

This potentially means less sponsorships for quilt in the future, which is not ideal.


## Rationale and Alternatives

In theory we could change the beacon to be opt-in, rather than opt-out. However this has the disadvantage of making the "monthly active users" metric useless, which renders the whole feature pointless.


## Prior Art

Many other projects have telementry (of various kinds) and many have had objectors.

For example [audacity](https://en.wikipedia.org/wiki/Audacity_audio_editor#Reception) tried to add (much) more invasive telementry, and then removed it after much user backlash. This example isn't fully applicable to quilt though, since we don't collect anywhere near as much user data of them.

## Unresolved Questions

Does this prevent us from having any sort of opt-in telementry in the future? For example, an update checker for quilt-loader - would people be okay with an option for quilt-loader to check quilts servers for updates if a mod requires a newer version of loader?
Or should that go in a future RFC?

