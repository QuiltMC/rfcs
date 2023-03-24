# Summary
This document is intended to detail the governing structure of the Quilt project.

# Motivation
This document has been written with the explicit purpose of defining guidelines and expectations for project staff.
It has been written with the idea of decentralizing power in mind, to help contribute to the goal of a truly open project.

# Explanation

## Teams

A team is a group of team members dedicated to solving a specific issue,
maintaining a specific sub project, or overseeing a specific area.

All teams exist as teams on GitHub and can be viewed on our website at https://quiltmc.org/about/teams/.

### Team Members

A team member is a member of the organization assigned to a specific team on GitHub.
A member of the organization may be assigned to multiple teams.
A team must have at least one member.

### Team Leads

A team lead is a team member with the [Team Maintainer](https://docs.github.com/en/organizations/organizing-members-into-teams/assigning-the-team-maintainer-role-to-a-team-member#about-team-maintainers) 
role for the relevant team on GitHub.
A member of the organization may be a team lead for multiple teams.
Each team must have at least one team lead.
A team lead may be nominated via general consensus of the team, or by an election, the process being detailed below.

### Team Responsibilities

Team responsibilities will vary and be detailed on a team-by-team basis.
In general, a team is responsible for performing the tasks outlined in the RFC that proposed its creation.

Most teams will also maintain one or more repositories on GitHub.
The team is responsible for reviewing pull requests and ensuring that they meet The Quilt Project's goals in quality and purpose.
Once a team has decided that a pull request is suitable for merging, a team lead will be responsible for the actual merging. Team leads can also decide to allow some or all team members to perform merges.

### Creating a Team

A new team can be created by passing an RFC proposing that team.
The initial team leads must be specified in the pull request adding the RFC and must approve the RFC before it is merged.
Once the [RFC process](0001-rfc-process.md) is concluded and the RFC is merged,
a new team with the specified leads will be created on GitHub by the Administrative Board.

The RFC proposing the new team should contain the following major sections:

- **Motivation**:
  - Why is this team needed?
  - What problems will this team solve?
  - What tasks will this team be responsible for?

- **Explanation**:
  - What will this team *do*?
  - What new projects will be created that this team will be responsible for?
  - How will each of these projects benefit the Quilt organization/community?
  - What processes will this team follow for each of the different kinds of changes their projects may undergo?

- **Drawbacks**:
  - Is there overlap of responsibilities?
  - Is there excessive granularity?

- **Rationale and Alternatives**:
  - Is there already a team responsible for the described tasks?
  - If this RFC doesn't pass, which team would be responsible for the described tasks?

- **Prior Art**:
  - Are there other organizations or projects with comparable tasks?

### Sub Teams

Some teams may wish for more granular organization of their structure.
In those cases, these teams may create sub teams.

Sub teams behave like regular teams, except they are not required to have a team lead.
If the sub team doesn't have its own team lead, the parent team lead's acts as its team lead.
In general, a sub team inherits its parent's structure according to their parent's team RFC.

## The Administrative Board

The administrative board is a group of organization members responsible for tasks that are not assigned to another team.
It is made up of at least three members to prevent deadlocks in the decision-making process.
Members of the administrative board may be members of other teams.

If it is decided that more administrative board members are required, either due to a current member stepping down, or due to it being decided that the admin board needs to be expanded, an election must take place.

### Admin board Voting Process

 Due to the fact that multiple candidates may be nominated for the admin position when a vote is held, the fact that there may be multiple spots to fill on the board, and the fact that the decision affects the entire organization, the standard voting procedure cannot be used. Instead, the following procedure will be employed:

Before the vote begins, a group of people, henceforth referred to as the Election Oversight Board or EOB, must be selected to oversee the voting process. They will be responsible for receiving candidate nominations, concerns about nominated candidates from other organization members, and hosting the election. In normal circumstances, the EOB consists of the existing members of the admin board, but, if this is not possible for any reason, the EOB will instead consist of members with the Community Manager role in the Community Team. The initial EOB may add any members that they feel would be a valuable addition to the board, with a simple majority vote.

The vote is conducted using [Single Transferrable Vote](https://en.wikipedia.org/wiki/Single_transferable_vote) (STV), or [Instant Runoff voting](https://en.wikipedia.org/wiki/Instant-runoff_voting) (IRV) in instances where there is only one space on the admin board to be filled, to ensure a minimum amount of "wasted votes". When using Single Transferrable vote, the [Droop Quota](https://en.wikipedia.org/wiki/Droop_quota) must be used to calculate the amount of votes required for a candidate to win a position on the admin board.

At the start of the voting process, each organization member may nominate themselves. Nominations must be submitted to the EOB within one week of the start of the process, after which time the vote is opened.

After one week of preparation and nomination submission, nominations must close, the list of candidates must be published, and the vote must begin, lasting one week. The list of voters cannot change beyond that point, unless a voter is removed from the organization and poses serious threat to the vote integrity. The system used to conduct the vote must allow voters to change their votes, and enable voters to rank the candidates in order of preference, while also having the option to not rank any given candidate. All organization members (which at this time is anyone with the "Community Team" or "Quilt Developer" role in Discord) are able to vote. During the vote, voters can contact the EOB about any concerns they have over the vote (for example, concerns about a candidate being elected, or concerns about the vote being influenced by an external force), to be dealt with as outlined in the paragraph below.

If the EOB receives any concerns over a candidate, either internally or submitted by voter, they must decide if the concerns are worth investigating. If they are not, no further action is taken. If they are, the team must investigate, and, if significant results are found, must publish the results to the voters, so that they can factor them into their decision. The EOB cannot prevent any candidate from running, this is to ensure that they don't have direct control over who can be elected. If, however, the vote is due to end before the EOB can complete their investigation, they may delay the end of the vote by up to 30 additional days.

After the voting period concludes, the EOB may take up to three days to count the votes. In the event of a tie, another vote will be held by the current members of the Administrative Board including only the tied candidates. The EOB may privately inform the candidates before publishing the results to the organization members and subsequently the community at large.

## Processes

To streamline working with teams, some standard processes need to be defined.
The following processes apply to all teams unless that team's RFC specifies a different process.

### Voting

Unless specified otherwise, the following process is used for voting:

A vote in a team may be called at any time by a team lead for that team or for one of its parent teams.
A vote can also be initiated as required by another process, such as adding or removing a team member. 

When a vote is called within a team, all members of that team and its parent teams are eligible to vote.
All polls should include a positive and negative option, as well as the option to abstain.
Voting members that abstain are excluded from percentages and tallies.
Voting members not casting a vote within the allotted time frame are counted as abstained.

After one week, the majority wins. The vote may also be called early if all eligible members 
casted their vote.

### Introduction of Staff

Any community member is eligible to be added to a team by a majority vote of all eligible organization members.
The vote is started by request from an eligible organization member. A community member wishing to join a team may
kindly ask an eligible organization member to request a vote. A positive response from the organization member isn't
mandatory.
The following members are eligible:
- All members of the relevant team
- All members of all the sub teams of the relevant team
- All members of all the parent teams of the relevant team

During the vote, especially if the community member isn't already part of any team, the rest of the organization is
consulted for opinions.

### Removal of Staff

Any team member or administrative board member may be removed by a majority vote of all eligible organization members.
The following members are eligible:
- All members of the relevant team
- All members of all the sub teams of the relevant team
- All members of all the parent teams of the relevant team

In case the demotion is against an administrative board member, all the existing teams are considered as relevant.

# Drawbacks

In the present day with most accounts and digital spaces being controlled primarily by one person,
the idea of an "administrative board" has some downsides.
A member of the board who is asked to step down may simply refuse to relinquish any resources they control.
For this reason, personal character needs to be a high priority when electing members to the board,
so that such a situation can be avoided. This can also be avoided by keeping the roles of the administrative board
purely as steering and oversight and delegate ownerships to other relevant teams, such as Infrastructure.
