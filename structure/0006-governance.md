# Summary
This document is intended to detail the governing structure of the Quilt project.

# Motivation
This document has been written with the explicit purpose of defining guidelines and expectations for project staff.
It has been written with the idea of decentralizing power in mind, to help contribute to the goal of a truly open project.

# Explanation

## Teams

A team is a group of team members dedicated to solving a specific issue,
maintaining a specific sub project or overseeing a specific area.

All teams exist as teams on GitHub and can be viewed at https://github.com/orgs/QuiltMC/teams.

### Team Members

A team member is a member of the organization assigned to a specific team on GitHub.
A member of the organization may be assigned to multiple teams.
A team must have at least one member.

### Team Leads

A team lead is a team member with the Team Maintainer role for the relevant team on GitHub.
A member of the organization may be a team lead for multiple teams.
Each team must have at least one team lead.

### Team Responsibilities

Team responsibilities will vary and be detailed on a team-by-team basis.
In general, a team is responsible for performing the tasks outlined in the RFC that proposed its creation.

Most teams will also maintain one or more repositories on GitHub.
The team is responsible for reviewing pull requests and ensuring that they meet the Quilt projects goals in quality and purpose.
Once a team has decided that a pull request is suitable for merging, a team lead will be responsible for the actual merging.

### Creating a Team

A new team can be created by passing an RFC proposing that team.
The initial team leads must be specified in the RFC and must approve of the RFC before it is merged.
Once the [RFC process](0001-rfc-process.md) has concluded and the RFC was merged,
a new team with the specified leads will be created on GitHub by the Administrative Board.

The RFC proposing the new team should contain the following major sections:

- **Motivation**:
  - Why is this team needed?
  - What problems will this team solve?
  - What tasks will this team be responsible for?

- **Explanation**:
  - What will this team *do*?
  - Who will be the team leads?
  - What new projects will be created that this team will be responsible for?
  - How will each of these projects benefit the QuiltMC organization/community?
  - What processes will this team follow for each of the different kinds of changes their projects may undergo?

- **Drawbacks**:
  - Is there overlap of responsibilities?
  - Is there excessive granularity?

- **Rationale and Alternatives**:
  - Is there already a team responsible for the described tasks?
  - If this RFC doesn't pass, who would be responsible for the described tasks?

- **Prior Art**:
  - Are there other organizations or projects with comparable tasks?

### Sub Teams

Some teams may wish for more granular organization of their structure.
In those cases, these teams may create sub teams.

Sub teams behave like regular teams, except they are not required to have a team lead.
If the team doesn't have its own team lead, the parent's team leads act as its team lead.
In general, a sub team inherits its parent's structure according to their parent's team RFC.

## The Administrative Board

The administrative board is a group of organization members responsible for tasks that are not assigned to another team.
It is made up of at least three members to prevent deadlocks in the decision-making process.
Members of the administrative board may be members of other teams.

At any point in time, a member of the organization may nominate themselves for a position on the administrative board.
This will then initiate the voting process, with all other organization members being eligible to vote.
If the nominated member passes a two-thirds vote, they are added to the administrative board.

If there are less than three members on the administrative board, a special vote is initiated.
Each remaining board member may nominate an organization member.
Organization members may also nominate themselves.
All other organization members may then vote on the nominees.
The nominee with the most votes will be added to the administrative board.
In the case of a tie, the remaining board members must break that tie.

## Processes

To streamline working with teams, some standard processes need to be defined.
The following processes apply to all teams unless that team's RFC specifies a different process.

### Voting

Unless specified otherwise, the following process is used for voting:

A vote in a team may be called at any time by a team lead for that team or for one of its parent teams.
A member of the administrative board may also initiate a vote in any team. 

When a vote is called within a team, all members of that team, its parent teams, and the administrative board are eligible to vote.
All polls should include a positive and negative option, as well as the option to abstain.
Voting members that abstain are excluded from percentages and tallies.

After one week, the plurality of votes wins.

### Removal of Staff

Any team member or administrative board member may be removed by a majority vote of all eligible organization members.
The following members are eligible:
- All members of the relevant team
- All members of all the sub teams of the relevant team
- All members of all the parent teams of the relevant team
- All administrative board members

# Drawbacks

In the present day with most accounts and digital spaces being controlled primarily by one person,
the idea of an "administrative board" has some downsides.
A member of the board who is asked to step down may simply refuse to relinquish any resources they control.
For this reason, personal character needs to be a high priority when electing members to the board,
so that such a situation can be avoided.
