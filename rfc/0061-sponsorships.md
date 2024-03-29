# Sponsorship Guidelines

## Summary

This RFC outlines the procedure for applying for, accepting, running, and terminating sponsorships that the project obtains.

## Motivation

As an open-source project, there are many opportunities for us to be provided with funds and/or free or discounted services, sometimes in exchange for advertisement or endorsement. Since we have access to limited funds, we should be taking advantage of this whenever we can, however, there are many sponsorships that we would not be able to accept in good conscience, either due to deceptive marketing, dishonest practices, or some other reason. Because of this, we need a clearly defined system that allows us to use and possibly endorse sponsored products in a way that is transparent, ethical, and in good faith.

## Explanation

This RFC provides a set of procedures for applying for, accepting, and terminating sponsorships, as well as guidelines for determining if a sponsor is suitable, and measures to take to ensure we remain transparent with and trusted by the community.

### Glossary of Terms

- Endorsed Sponsorship: A sponsorship agreement that requires an endorsement from us, such as a mention on our website, for example, Starchild Systems hosting and managing our forum.
- Non-Endorsed Sponsorship: A sponsorship that does not require an endorsement from us, for example, our free 1Password Teams account.
- Financial Sponsorship: Sponsorships that provide direct financial contributions to the project.
- Non-Financial Sponsorship: Sponsorships that do not provide direct financial contributions to the project. They may provide something else, like tooling or server space.

### Sponsor Guidelines

In order to provide a sponsorship to the project, sponsors should meet the following requirements. These are not hard requirements, as we understand that there is generally more nuance in the circumstances surrounding a given practice than can be expressed in a few bullet-points, and being too strict in our requirements could prevent us from accepting sponsorships we are otherwise comfortable with. It is more important (though still not required) that Endorsed Sponsors meet these guidelines, as people may discover and support them through us, and we don't want to be the cause of an unsound product or company receiving support that it does not deserve.

Sponsors **should**:
- Provide a high-quality product or service at a fair and competitive price.
- Market their product or service in a way that is transparent and honest.
  
Sponsors **should not**:
- Use, promote or otherwise endorse technology that is clearly being used in a harmful or anti-consumer way.
- Have a recent, reliably-sourced history of discriminating against minorities or endorsing those who do that has not been clearly and successfully addressed.
- Have a recent, verified history of serious anti-consumer practices that have not been clearly and successfully addressed.
- Have a recent history of employee mistreatment, that has not been clearly and successfully addressed
- Conduct business in a way that is unsustainable or especially harmful to the environment.

### Measures taken to ensure transparency

- We will maintain a list of all endorsed and non-endorsed sponsorships that the project holds, both past and present, on our website.
- We will have a Sponsorship section on our forum, where our community can voice concerns about current and future sponsors, suggest sponsorships we might like to consider, and request information about certain sponsorships.
- All proceeds from financial sponsorships must be paid directly into our [Open Collective account](https://opencollective.com/quiltmc)

### Validation Process for Endorsed Sponsorships

If we plan to either apply for or accept an endorsed sponsorship that meets the Base Requirements, they must go through a process of validation, to help maintain trust with our community. 

The Outreach Team will create a post on the Sponsorship section of the forum, explaining our intention to endorse a given sponsor, and the broad terms of the sponsorship agreement. We can then discuss with members of the community the terms of the agreement, and whether they think it would be appropriate for us to apply for or accept the sponsorship. Based on their feedback, we can then decide whether to proceed with applying for or accepting the sponsorship, though if the sponsorship was offered to us by a sponsor, we should always discuss negative points with them before denying a sponsorship.

### Process for Applying for or Accepting a sponsorship

If a sponsorship is suggested, either by a team member or community member, or offered to us by the sponsor themselves, the team(s) that it would affect will discuss if it would be useful to them, and if there are any preferable alternatives (for example, a development tool would be discussed by the development teams, a moderation tool would be discussed by the moderation team, etc.). In the case of financial sponsorships, the Admin Board should take on the role of the affected team, with input from the rest of the Quilt teams. If it is decided that the sponsorship would be beneficial to the project and its terms are acceptable, it will be briefly discussed internally to make sure no-one has any immediate concerns, and then we will proceed to the Validation Process, if applicable.

If the Validation Process is successful, the project can then proceed to apply for or accept the sponsorship. Preferably, this should be the job of the Outreach Team (in accordance with their role of "managing relations with other platforms and communities outside of a moderation context"), however, it may be necessary for a member of the Admin Board to apply if the sponsor requires it.

### Sponsor Concerns

If a member of the team or community has a concern about a sponsor, they can bring it up in internal channels (or by using Discord ModMail/Discourse Moderator PMs in the case of community members), where the team can discuss it. If the concern is not immediately deemed invalid or extremely clear-cut, and we have contact with the sponsor, then the Outreach Team (or sometimes the Admin Board) should attempt to reach out to them for clarification before making a decision. If the sponsor has a satisfactory explanation to quell the concern, we can inform the reporter, and make a post on the forum if the concern is widespread and the sponsor gives us permission. If they do not, we can terminate the sponsorship.

### Terminating a sponsorship

At some point, it may become necessary to terminate a sponsorship agreement, either if concerns about the sponsor come to light, or if they are simply no longer needed.

When we terminate a sponsorship, we will make a post on the forum acknowledging the termination, in order to keep the community informed. If we're terminating a sponsorship because we no longer need it, the post doesn't have to be long, but if it is due to a concern about a sponsor, we should include the concern and how we determined it to be valid. It may occasionally be that the details of a concern that resulted in the termination of the sponsorship may be too sensitive to post publically, but in this case we should still ensure that it is documented internally to ensure that we do not enter into a similar sponsorship in the future without ensuring that the concern has been addressed.

## Drawbacks

This process could extend the already significant time that it would take to apply for or accept a sponsorship that would benefit the project. It also might mean that we're unable to accept a sponsorship that would significantly benefit the project if it does not pass verification, and this would be to the project's detriment.

## Rationale and Alternatives

Quilt is not designed to be a financially-driven project, and it is common knowledge that introducing money into open-source without extreme care and caution can lead to corruption. Unfortunately, by necessity we have several costs which sponsors could offset, and have need of tools that would be inaccessible to us without a sponsorship (e.g, 1Password). As well as this, endorsing disreputable companies as part of a sponsorship may undermine the trust that the community has in us, and so we need formal procedures that involve the community as much as we can to help ensure this doesn't happen. That said, here are some alternatives:

- We could not involve the community in the decision-making process. This would make it faster and possibly avoid drama in the short-term, however, not involving our community in such important decisions may undermine their trust in us and contradict Quilt's goal to be a caring, community-driven project.
- We could have the sponsorship discussions on Discord, where there is higher traffic, especially if the forum channels are implemented. However, the forum is more open than Discord, and more reliable and permanent in the long term, since we control it. In addition, forums are more suited to long-form discussions, like those about sponsorships would likely be. The issue of the forum's low traffic compared to Discord could be offset by posting new forum posts to Discord via a webhook.
- We could forgo the base guidelines and validation process, as it could be argued that people can tell the difference between a sponsorship and a full endorsement. However, the fact that sponsorships have value at all is evidence that this is not universally true. If we endorse a bad sponsor, we could be responsible for people discovering and supporting it, and endorsing unethical companies could give our critics more ammunition against us.
- We could require non-endorsed sponsorships to undergo the validation process, but since we're not endorsing them and using a paid service for free, we're really just costing them money, and people won't discover and support them through our endorsement, since we're not endorsing them.
- We could turn the base guidelines into hard requirements for Endorsed Sponsors, but they do not take into account the fact that some sponsors are more valuable to us than others (An AWS sponsorship would be more valuable than a Minecraft server host, for example), and that there is often more nuance in the policies and practices of a business then can be expressed in a few bullet points. Furthermore, since all sponsors are validated by the community, that should be sufficient to stop any sponsors we feel uncomfortable working with from being accepted/applied for.
- We could post non-sensitive concerns about sponsors on the forum, and this would be a more open system that would allow the entire community to participate in the discussion. However, it runs the risk of people publically posting concerns that end up having no basis, and other people acting on those concerns, needlessly hurting the sponsor and possibly affecting our relationship with them.

## Prior Art

Linus Media Group, the company behind the Linus Tech Tips and Techquickie YouTube channels (among others), have a [Sponsor Discussion section](https://linustechtips.com/forum/98-lmg-sponsor-discussion/) on their forum, where they ask their community for their opinions on potential sponsorships, to make sure they're only endorsing products and services which their audience holds in high regard, and recent examples of the success of this at the time of writing include blocking the sponsorship of [Ladder Life](https://linustechtips.com/topic/1429149-thoughts-on-ladder-life/) and [Geologie](https://linustechtips.com/topic/1424178-thoughts-on-geologie/), and approving sponsorships from XSplit.


## Unresolved Questions

How will we internally document sensitive concerns? Who should have access to them?
