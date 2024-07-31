# RFC 0086: Installer Team

This document has been written according to the process outlined in [RFC 0006](https://github.com/QuiltMC/rfcs/blob/main/structure/0006-governance.md#creating-a-team), and with reference to [RFC 0072](https://github.com/QuiltMC/rfcs/blob/main/structure/0072-legal-team.md).

## Summary

The installer team will maintain the currently used [Java Installer](https://github.com/QuiltMC/quilt-installer), and develop the work-in-progress [Native Installer](https://github.com/QuiltMC/quilt-native-installer) and planned bootstrapping server installer. This responsibility is currently in the hands of the loader team.


## Motivation

With a new Rust-based native installer in the works, and a separate bootstrapped server installer planned, the work required for installer-related activities has significantly increased.
As such, it is no longer feasable for the loader team to partake in long-term, continuous installer development, which is a requirement to get the native and bootstrap installers to a full public release.
There are also external, open-source project contributors that would like to join the installer development effort, but would not like to join a team with as wide a scope as the loader team.

This team will be responsible for maintaining the currently-used [Java Installer](https://github.com/QuiltMC/quilt-installer) until a stable native installer can be rolled out.
It will also be responsible for developing the work-in-progress [Native Installer](https://github.com/QuiltMC/quilt-native-installer), to bring it to a full release, and maintain it afterwards as well.
The planned bootstrap server installer will also be explored and developed by this team.


## Explanation

A new installer will be a big aid for attracting new users. It is the first software used by new Quilt users, and current Java installer doesn't cut it. It may be difficult to run as Java has to be installed, it has an outdated UI, and it is just mundane.
The new installer is native and requires no runtime dependencies (other than an optional DE on linux). A CLI is also available, so command line users do not have to use a GUI interface. This also helps advanced users automatically script loader upgrades and custom installations.
There is also plans for a radical new design with beautiful backdrops, better user experience and flow, and a design language similar to the website.
These will without a doubt create a better first impression for first-time users than the current installer.

The current installer code is also old and cluttered, and has gotten difficult to maintain and improve upon because of its size. An installer written from scratch in Rust will improve the code-quality and make it easier to maintain in the future.


## Drawbacks

There are already a fair few teams in the organisation. Introducing smaller more dedicated teams like this might not be necessary, and it may encourage others to create more teams for other smaller tasks too.
There is also the question of whether a separate team is really required after the major stretch of developemnt for the native and bootstrap installers are completed. If only maintanence is required afterwards, it may not be necessary for a dedicated team to remain.


## Rationale and Alternatives

The team currently responsible for installer related activities is the loader team. However this is a a rather arbitrary grouping as it may not make sense for the installer (especially a non-Java one) to come under the responsibility of the loader team.
This results in the team not being motivated to work on the installer as it isn't their main area of focus.


## Prior Art

The native installer is currently being developed by a few loader team members and open-source contributors. However the loader team is finding it difficult to manage PRs to the projects as they are often occupied with other loader-related work.


## Unresolved Questions

Members for the team and most importantly picking of a team lead, has yet to be done.
