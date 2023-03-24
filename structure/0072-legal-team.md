# RFC 4269: Legal Team

This document has been written according to the process outlined in [RFC 0006](https://github.com/QuiltMC/rfcs/blob/main/structure/0006-governance.md#teams).

# Summary

The Legal Team is a standalone team in charge of handling legal matters to the best of its ability. This team will not require its members to be lawyers 
or have any form of training, although this would be preferred. 

It is made clear that this team will not be providing legal guidance but only advices to the best of their ability and team members cannot be held
liable for their work in good faith as a Legal Team member.

# Motivation

Historically, legal matters have been handled by various different people without any proper structure. The de-facto team who would handle particular 
matters (eg. the Admin Board for registering Quilt as a non-profit) may feel unqualified to handle those issues and/or may not have the bandwidth for it. 
Moreover, someone handling legal matters leaving that de-facto team should not prevent them from handling those matters, especially 
considering the current scarcity of trained or knowledgeable members.

Some laws may also require us to have a designated legal team, such as to designate a Data Protection Officer 
([Art. 37 EU GDPR](https://gdpr-info.eu/art-37-gdpr/)).

# Explanation

## Responsibilities

The team is responsible for the following:

- Ensuring Quilt stays up to date with data regulation laws (eg. EU GDPR, COPPA)
    - Keeping the privacy policies and terms of services up to date
    - Designating a DPO
    - Responding to data privacy requests
    - Assisting the Infrastructure team in staying compliant
- Reviewing contracts signed by the organisation
- Handling registration of the organisation as a legal entity, if decided
- Advising other teams on any legal issues that may arise
- Answering questions and concerns from community members to the best of their ability
- Writing guidelines on what changes to Quilt projects require legal verification and providing a process for this verification to happen
- Writing internal or public processes on how to handle requests
- Any other legal matter not explicitly listed in this document

## Legal Team Server

The Legal Team has a private server not viewable by any other team. This server must only be used for discussing problems 
which need the uttermost privacy, as required by law. This includes, for example, talking about an ongoing data request or DMCA takedown
without any name being redacted.

While this deviates from the usual observable approach that makes sure every team can be monitored by at least one other independent team, 
privacy is legally required to handle some matters, such as data privacy requests.

The server shall be owned by one of the Legal Team leads, who is in charge of managing the server. 

## Requesting Changes

In order to stay compliant with regulations and laws, the Legal Team has the power to block any Pull Request or community space change that
could negatively affect compliance. This overrides any decision made by a Team Lead.

If a conflict arises between the Legal Team and a Team Lead, the Admin Board can be summoned for arbitration. In arbitration, the 
Admin Board's decision is final and overrides the Legal Team.

The Legal Team also has the power to open high-priority issues on organisation repositories and request changes to be 
made as fast as reasonably possible.

## Communication

To be able to communicate with users, Legal Team members are provided with an email address, connected to one or many distribution lists.

## Transparency Reports

Because of the lack of oversight, the Legal Team will be required to publish public reports twice as year, containing as many information on
their activity as allowed by the applicable laws, and taking into account user privacy, similar to [Discord's transparency reports](https://discord.com/tags/transparency-reports).

# Drawbacks

This team will most likely contain non-professional members. While their knowledge may still be extensive of some legal issues, we have to keep in 
mind they aren’t lawyers.
While legal liability may be a concern, the non-profit and its insurance will be here to provide some protection if an issue arises.

Due to legal needs, some if not all of the team members may have their real names appear publicly, such as when signing bylaws. 
Explicit approval from a team member will be required before publishing a real name publicly and approval cannot be made mandatory.

The ability to request changes may create some friction between developers and legal team members.

# Rationale and Alternative

Alternatively, other teams can sporadically handle legal matters. This has some issues, such as forcing people who aren’t motivated to handle those 
matters to do so, not centralizing the information and lacking the means required to stay compliant. The existence of this team is also legally required
in some cases, as outlined above. 

# Prior art

This team is close in spirit to the idea of a legal department in a workplace, although without requiring legal training. It is worth noting that 
the workplace definition of a legal department includes a large majority of employees who aren't lawyers, but some other qualifying background
or field training.

Transparency Reports are similar to [Discord's transparency reports](https://discord.com/tags/transparency-reports).
