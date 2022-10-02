# Summary

This document describes the Quilt Community Collab program, the roles of each member and the associated processes.

# Motivation

The Minecraft community is a complex space, where it can be required for moderators to share intel between communities. The main goal of the Quilt Community Collab program is to facilitate exchange between a selected group of communities and improve user safety.

The server also serves as a coordination server for events and other projects happening in the Minecraft sphere.

# Explanations

The Quilt Community Collab is a private space for Minecraft-centered communities to exchange information and coordinate. The main focus of the exchanges is set on moderation, but it can also be used for events and anything else that may be fitting.

## What Community Collab isn’t

- It isn’t an official affiliation with the Quilt project itself or any other community part of the program
- It isn’t a place to blindly synchronise punishments between communities
    - Each community must decide on appropriate moderation action based on facts shown to them and their own rules and policies. Infringing this rule may get your community removed from the Community Collab program.

## Roles inside the server

### Keyholder

The keyholder for this space is the same as for the Quilt Community Team. This role is described in the [RFC 0007 # Keyholders](https://github.com/QuiltMC/rfcs/blob/master/structure/0007-community-team.md#keyholders).

### Managers

Managers are responsible for keeping the collab program up and running.

- Maintain and organise the associated Discord server
- Actioning changes to the collab-related spaces
- Organise polls and votes as required by formal processes
- Help resolve conflicts between communities
- Stay in touch with any community part of the program

Community Collab Managers are the same as Community Managers of the Quilt Community Team.

### Collaborators

Collaborators are moderators of one or more communities in the program. They have access to the server to the moderation part of the server, such as general moderator chats and case threads. Access is also given to the event category.

### Event Planners

Event Planners are members of the staff team of one or more communities in the program, who focus on event organisation. They have access to the events part of the server.

### Project Roles

Other roles may be given surrounding the different projects happening inside the program. Those roles are informal and will depend on each project.

### Invitees

Invitees are temporary users, with no access to the rest of the server. They only have access to temporary channels required by the process they are visiting for.

## Voting process

If required, a vote can be called by a Manager where the involved Collab communities are each constituents. A poll and an associated thread are created in an internal channel designated for that purpose, including as many details as possible.

Each community gets a single vote. They are free to write their own process for determining their vote, but it must follow those rules:

- This process must be democratic and give a voice to any community member who is able to join the Community Collab server under the Collaborator role.
- Each eligible voter must have single, equal voting power.
- The process can remove the voting power under some reasonable inactivity threshold.

Once a community has decided their vote, one member can report their vote in the thread, mentioning the community name, and the option being voted for, with corresponding proof (such as a screenshot of an internal poll).

Each community also has the option of skipping its vote if none of its members has an opinion on the poll. Such instances should be reported the same way.

Polls must include a closing date of at least ten days. If required, the closing date can be pushed away by a maximum of a month, upon the informal agreement with the majority of the other communities. During that period, any Collaborator can use their veto right (see below).

Once the poll is closed, the vote can be interpreted, depending on the specific process this vote is part of, if any.

## Veto rights

While the vote is active, any Collaborator can use their veto right by sending a message inside the thread, detailing their reasoning.

If another Collaborator believes the reasoning isn’t satisfying, they can start the overruling process. If so, another poll is set up, if more than half of the individual votes are positive after three days, the veto is discarded.

If no Collaborator overrules the veto after three days, the vote is considered as failed right away. The Managers are free to only communicate the result to the Invitees at a later date to keep the veto secret.

## Process: adding a community

Any community, ideally having their main user space on Discord, centred around Minecraft, can apply to join the Community Collab program. The process is defined as follows:

1. A member of the community who wishes to join, ideally someone holding administrative and moderation powers, should get in touch with a Manager to start the process. The following questions should be asked to the community and collected, along with an invite to the server:
    - What is the main focus of the server?
    - Why is your community looking to join the Community Collab program?
    - What is your current ruleset?
    - Who is currently part of your staff team, and who would have access to Community Collab?
2. Once those answers are collected, the Manager can inform all the Collaborators about the community, and start a discussion to gauge the interest of the community.
3. The members of the community are invited to the Community Collab server under the Invitee role and a channel is set up to allow discussion between Collaborators and Invitee.
4. Once the Collaborators informally agree that enough information has been gathered, the vote can be created with the option to approve or deny, according to the process defined in this document. The Invitees should be informed the vote has begun.
5. After the poll is closed, the vote is considered passed if more than 75% of the votes cast are positive. The discussion channel must be archived and kept read-only.
- If the vote has passed: the Invitees are elevated to the rank of Collaborators. Internal discussions can be purged first if needs be. The community is added to any listing that may exist.
- If the vote is rejected: enough time is given to the Invitees to acknowledge the result and leave the server. Managers are free to kick Invitees if necessary.

# Rationale and alternatives

The Quilt Community Collab program existed prior to this document. This document tries to summarize how the server has been working so far while formalising some aspects that were still left up in the air.

More importantly, three polls have been held inside the server, with the following results:

- When voting for changes on who is or isn't part of the CC network, how should we count votes?
    - 10 answered: One vote per community: each community will have to write down a formal process on which they decide to vote for or against changes. This process must be democratic and fair.
    - 4 answered: One vote per collaborator: each user or plural system present in the server gets one vote
- When voting on who is or isn't part of the CC network, what proportions of approvals versus neutral votes are required to pass? (multiple answers accepted)
    - 11 answered: >75% approval to pass (there must be at least 3 times more approval than neutral)
    - 4 answered: >50% approval to pass (there must be more approval than neutral)
    - 1 answered: 100% approval to pass (there are no neutral votes, only vetos)
- When voting for changes on who is or isn't part of the CC network, should we have veto rights, and if so how should they work?
    - 17 answered: Any voter can veto, which can then be overruled by the majority if the concerns aren't believed to be deserved
    - Nobody answered: There are no veto rights.
    - Nobody answered: Any voter can veto, which will immediately cancel the vote

# Prior art

Quilt Community Collab is an indirect replacement of the Modding Interpol, with a wider range of activities, such as events and related projects

# Unresolved questions

The following questions will have to be solved at a later date:

- This currently doesn’t define how to remove communities
- We aren’t defining any role for administrators of the communities part of the program
- We only allow for full communities to join, we should look at whenever we also want to invite individuals