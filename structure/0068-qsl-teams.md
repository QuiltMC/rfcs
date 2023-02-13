# QSL Teams
## Summary
This RFC establishes and defines the structure, roles, and responsibilities of the QSL teams.

<!-- For simplicity, a lot of the boilerplate not relevant to a team definition was left out of this RFC-->
## Explanation
The QSL Team is made up of three subteams, QSL Core, the QSL team, and Quilted Fabric API. One cannot only be a member of the "QSL Team", they must also be a member of at least one subteam.

New subteams can be created by the QSL Core team by amending this RFC.

### QSL Core
#### Responsibilities
The QSL Core team is made up of the main maintainers of QSL. They have the final say on the direction of the project, and have admin access to all QSL repositories and end-user distribution points (i.e. Modrinth, CurseForge) of QSL, QFAPI, and any other QSL team projects.
#### Joining the team
The QSL Core team can add new members to itself by unanimous consent of all active members of the Core team.

### Quilted Fabric API
#### Responsibilities
The Quilted Fabric API team is responsible for the maintenance of the QFAPI project. They operate under the advice and consent of the Core team, but have full admin access to the QFAPI repository and its end-user distribution points.
#### Joining the team
New members can join the QFAPI team by unanimous consent of all active members of both the Quilted Fabric API and QSL Core teams.

### QSL Team
#### Responsibilities
The QSL team provides triage, reviews, and testing for QSL. Members of this team are not given permission to push directly to the main QSL branches, but are given enough permission to carry out their repsonsibilities.
#### Joining the team
New members can join the QSL team by unanimous consent of all active members of the QSL Core team. The Core team should check with the QSL team before accepting new members.

## Rationale
(note: this section might not include rationale for later updates; see the original version of this RFC for context)
This RFC mostly defines the unspoken status quo that already occurs between the QSL teams. The major difference is the removal of library-specific subteams. This change was made for the following reasons:
1. **Efficiency**: There simply weren't enough interested members to require subteam reviews on each PR. Either teams would be unfilled (defaulting to the status quo this PR defines), or be stuck waiting on one specific person to review the PR. We didn't see any benefit from this system over the downside of slowing down PRs. Additionally, we found that QSL subteams did not end up taking the responsibility of each porting their own library; it was simply first-come, first-serve, or often done all at once by a QSL Core member.
2. **Interest**: Because of the efficiency problem, joining a QSL subteam involves accepting a lot of responsibility. Filtering everything out to the top level reduces the amount of perceived pressure put on new maintainers of QSL; they can pick and choose which PRs they want to adopt over every part of QSL
