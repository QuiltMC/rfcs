# Quilt PR Policy
# Summary and Scope
This RFC defines a system for merging Pull Requests (PR)s into Quilt projects. It applies to any repositories which opt-in to this system by adding a section (given below) to their CONTRIBUTING file. This document is meant to be a guideline, not a strict definition; projects may make amendments to this process (e.g. allowing it to be bypassed for typo fixes or setting restrictions on who's approvals count towards a PR) in their respective CONTRIBUTING files.

This RFC does not seek to require projects to use this process, but we encourage all projects expecting many pull requests to do so.

# Motivation
This RFC intends to make the process of creating, reviewing, and merging PRs easier for both Quilt teams and potential contributors.

The goals for the PR process are:
- **Democratic** -- Features are added through consensus of the team, and not through a benevolent dictator for life (BDFL) who has total control over the project.
- **Quick** -- Tools should be in place to allow the team to shut down bikeshedding and say good enough on features if needed. However, this doesn't mean that pull requests should be easily forced through.
- **Hands-off** -- Every team member shouldn't have to approve every PR. The team as a whole has a responsibility to ensure PRs are reviewed in a timely manner, but no person should have a responsibility to view every change.

It is **not a goal** of this PR to be fool-proof from bad actors within the Quilt technical teams abusing the system. Being on any team in Quilt requires trust, and bad actors should be removed instead of worked around.

# Explanation
## Vocab
- **Required Approvals**: The total number of approvals needed by the team assigned to a PR, or that team's superiors.
    - An approval is defined as an approving review using the GitHub review feature
- **Minimum Team Approvals**: The amount of approvals that must come directly from the assigned team. When multiple teams are assigned to a PR, this number applies to each team.
    - This prevents PRs from getting merged by Technical Leads/etc without any oversight by the responsible team
## Acceptance Criteria
A Pull Request is considered to have been approved when all of the following are true:
- The Pull Request has the needed amount of Required Approvals and Minimum Team Reviews
- The Pull Request has no changes requested reviews by any members of the team
- Nobody in the team has an outstanding review requested on the Pull Request using the system on Github
    - This includes someone requesting the review of themselves or another person
- All other criteria defined in the team's CONTRIBUTING file have been met.

## Final Comment Period (FCP)
- Eligable to begin immediately after the Acceptance Criteria are met.
- Formally begins when labeled and a comment is posted with the end date of the FCP.
- Lasts a certain amount of days, depending on the category the PR was assigned to.
- May be ended or restarted at any time by a team member or the PR author if they have legitimate and substantiated concerns.

## Other information
Team members may dismiss their own reviews.

Admins and technical leads may dismiss outstanding and requested reviews at their discretion, but should only do so if the team member cannot be contacted within a reasonable time-frame and must leave a comment on the PR with their justification. Reviews should *not* be dismissed simply because the majority (or that specific admin/technical lead) disagrees with one person or feels their concerns are invalid. Instead, a formal vote to merge a PR should be used.

## Template for CONTRIBUTING.MD
```markdown
### Pull Request Process
To get a pull request merged into << PROJECT >>, it must get a certain number of approvals from the maintainers, and then it will enter a Final Comment Period. If the pull request passes the final comment period without opposition, the PR will be merged. Otherwise, the PR will return to being in review. 

The exact number of reviews needed, and the length of the Final Comment Period, varies depending on the scope and complexity of the pull request. The numbers for each category are listed below.

#### << CATEGORY ONE >>
**Required Approvals**: X
- At least Y of these must come directly from the << TEAM >> team and not its superiors.

**Final Comment Period**: X days

<< Continue for each category >>

This is only a summary of the process. The exact rules are defined in [RFC 39](https://github.com/QuiltMC/rfcs/blob/master/structure/0039-pr-policy.md)
```
## Conflicts
If the team cannot agree on whether to merge a PR or deny it, there are steps they can take to avoid decision paralysis. This should be a rare process--teams are encouraged to resolve these disagreements through talking the details out, but sometimes members of the team may have fundamental disagreements over the details of a PR.

In the event the disagreement is over API structure, the PR may, at the team's option, be closed and an RFC be created that defines the feature.

In the event the disagreement is over implementation details, or would otherwise not benefit from being forced through the long RFC process, the voting method defined in RFC 6 can be used. Once a vote is requested by a team member, the PR will immediately enter a Final Comment Period lasting two weeks that cannot be ended by any team member. The two-week FCP can be used for team members to further discuss the PR, and for the author to make changes to the PR as needed. At the end of this period, the team will vote on if the PR should be left open, merged, or closed.
# Drawbacks
- Implementing a defined policy adds a layer of bureaucracy to getting PRs merged, which may be confusing.
- There may be situations that this policy does not define, which could cause conflict or confusion within the teams.
- Not having one person who has to review every PR could also lead to death by staleness, where a PR is never merged because nobody can be bothered to review it.
- Final comment periods create a limbo where consensus on a PR has been reached but it still must wait to be merged.

# Rationale and Alternatives
We could do nothing and keep the current guessing game on when enough discussion has happened for a merge, but this could lead to PRs being rejected too quickly or merged too quickly. The latter is especially problematic because most of Quilt's projects are stable for long periods of time.

We could assign a "benevolent dictator" for each project, similar to how many other projects are run. However, we would need to find a perfect person who is willing to devote all of their time to just one or a couple Quilt projects that everyone universally loves, and it would violate the Quilt philosophy of a democratic, team managed system. 
# Prior Art
The policy defined here is a reaction to the issues observed with two of the other main modloaders, Forge and Fabric. Both Forge and Fabric use the BDFL model with success. However, both Forge and Fabric struggle with all the drawbacks of such a system. These include having no way to resolve disagreements with project leaders, requiring a very small amount of people (usually one or two) to view every single code change, and the staleness of PRs that get overlooked by those dictators.

