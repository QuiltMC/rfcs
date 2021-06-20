# Quilt Loader PR Policy
# Summary and Scope
This RFC defines the criteria for merging Pull Requests (PR)s into Quilt Loader. It applies to any repositories managed by the Quilt Loader team, as well as RFCs under the scope of Loader.
This RFC does not define policy for anything out of the scope of the Loader team, but we hope that other teams will structure their policies similarly.

# Motivation
This RFC intends to make the process of creating, reviewing, and merging PRs easier for both the Quilt Loader team and potential contributors.

The goals for the PR process are:
- **Democratic** -- Features are added to Loader through consensus of the team, and not through a benevolent dictator for life (BDFL) who has total control over the project.
- **Quick** -- Tools should be in place to allow the team to shut down bikeshedding and say good enough on features if needed. However, this doesn't mean that pull requests should be forced through.
- **Hands-off** -- Every Loader team member shouldn't have to approve every PR. The team as a whole has a responsibility to ensure PRs are reviewed in a timely manner, but no person should have a responsibility to view every change.

It is **not a goal** of this PR to be fool-proof from bad actors within the Loader team abusing the system. Being on any team in Quilt requires trust, and bad actors should be removed instead of worked around.

# Explanation
## Initial Review Process
A Pull Request is considered to have been approved when all of the following are true:
- The Pull Request has received 2 approvals by members of the Loader team.
- The Pull Request has no changes requested reviews by any members of the Loader team
- Nobody in the Loader team has has their review requested on Pull Request
    - This includes someone requesting the review of themselves or another person

Team members with outstanding changes requested reviews may dismiss their own reviews.

Admins may dismiss outstanding reviews at their discretion, but should only do so if the team member cannot be contacted within a reasonable time-frame. Conflicts should *not* be resolved by an admin, but use 

## Final Comment Period (FCP)
- Begins immediately after the above criteria are met
- Lasts 2 days for small implementation changes, 5 days for major refactors, and 10 days for new API introductions or RFCs. Bugfixes have no FCP and are considered mergable immediately.
- May be ended at any time by a team member if they have concerns.


## Conflicts
If the Loader team cannot agree on whether to merge a PR or deny it, there are steps they can take to avoid decision paralysis. This should be a rare process--teams are encouraged to resolve these disagreements through talking the details out, but sometimes that isn't an option.

In the event the disagreement is over API structure, the PR may, at the Loader team's option, be closed and an RFC be created that defines the feature.

In the event the disagreement is over implementation details, or would otherwise not benefit from being forced through the long RFC process, the voting method defined below can be used.

### Voting
A vote is complete when one of the following is true:
- Ten days has passed and the number of votes for one disposition is greater than the number of votes for the other disposition.
- An absolute majority has been formed for one disposition.
    - This means that even if all of the team members who have not yet voted suddenly voted for the currently losing side, the end result would not change. For example, if 7 out of a team of 10 have voted yes, the remaining 3 team members could not change the outcome of the vote.

In the event of a tie, the vote is opened to the Admin Board and their votes count as 3. If the vote is still tied, the vote fails (the policy is not implemented, the PR is not closed, etc.)

# Drawbacks
- Implementing a defined policy adds a layer of bureaucracy to getting PRs merged, which may be confusing.
- There may be situations that this policy does not define, which could cause conflict or confusion within the Loader team.
- Not having one person who has to review every PR could also lead to death by staleness, where a PR is never merged because nobody gets around to reviewing it.

# Rationale and Alternatives
We could do nothing and keep the current guessing game on when enough discussion has happened for a merge, but this could lead to PRs being rejected too quickly or merged too quickly. The latter is especially problematic because Loader is such a stable project.

We could assign a few benevolent dictators to be in charge of merging PRs. At the time of writing, the most obvious choice for this would be one of the Admin Board members on the team, such as i509VCB or Haven King. However, doing so would require them to spend much more time on Loader than their other projects to avoid death by staleness, and there would be no way for the team to merge PRs if the Admins didn't feel like it.

# Prior Art
The policy defined here is a reaction to the issues observed with two of the other main modloaders, Forge and Fabric. Both Forge and Fabric use the BDFL model with success. However, both Forge and Fabric struggle with the issues described in the previous section. These include having no way to resolve disagreements with project leaders, requiring a very small amount of people (usually one or two) to view every single code change, and the staleness of PRs that get overlooked by those dictators.

# Unresolved Questions
The exact numbers used in policy such as FCP length and required amount of reviews will need to change as Loader evolves.

# Expected Response
This RFC should be uncontroversial in the community at large.

Steps should be taken to provide a summary of this process to contributors to Loader, so that they can understand the basics of the PR process without having to read a >1000 word document.
