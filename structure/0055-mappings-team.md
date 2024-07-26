# Mappings team restructure

## Summary

The mappings team was originally created in [RFC 18][RFC18], but has since
evolved: a triage team was created for [Quilt Mappings][QM], and the need to
split the mappings team into smaller subteams has appeared. This RFC describes
the proposed structure for the top-level team and those subteams.


## Motivation

The scope of the repositories maintained by the main mappings team is very big,
from the main Quilt Mappings repository, to all of the toolchain used and 
related to it. By splitting the mappings team, its members can be assigned to
projects more specifically, and review requests in PRs would only reach to the
repository maintainers rather than the whole team.


## Explanation

The mappings team would be split into three sub-teams: Quilt Mappings, Mappings
Tooling, and the existing Mappings Triage sub-team.

The Mappings Triage team is only in charge of reviewing mappings submitted to
QM, ensuring their quality and making sure they stick to the conventions.
This team only has triage access to the QM repository.

The Quilt Mappings team would be in charge of maintaining QM, and also shares
the responsibilities of the triage team. The main difference between the triage
team and the QM team is their access level to the repository, meaning only QM
members can merge PRs or push commits directly.

The Mappings Tooling team would be in charge of maintaining all of the mappings
toolchain (ie. [Mappings Hasher][Hasher], [Enigma], [Tiny Remapper][TR], etc.),
and subsequently for reviewing PRs to these repositories.


## Drawbacks

Having different people maintain the toolchain and QM could mean more
bureaucracy to make changes in both QM and the toolchain at the same time.


## Rationale and Alternatives

- Having multiple sub-teams instead of one big team can make assigning people
  to tasks easier, who manages each project clearer
- Even splitting the mappings team into QM and tooling, the number of projects
  handled by the tooling team is very broad. The tooling team could be split
  into more sub-teams
  - Most of the projects are in charge of one or two people, the sub-teams
    wouldn't be bigger than 2 people in most cases.
- Currently, all of the responsabilities mentioned are already handled by the
  mappings team or the triage team.


## Unresolved Questions

- Who should be part of the top-level team?
- What permissions should top-level team members have in QM, and the toolchain
  repositories?
- Should triage team members be considered as Quilt Developers?


[RFC 18]: 0018-technical-teams.md
[QM]: https://github.com/QuiltMC/quilt-mappings
[Hasher]: https://github.com/QuiltMC/mappings-hasher
[Enigma]: https://github.com/QuiltMC/enigma
[TR]: https://github.com/QuiltMC/tiny-remapper
