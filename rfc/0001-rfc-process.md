# Summary
In many other open source projects and open standards, an RFC (or "Request For Comments") process is used to give all stakeholders a chance to voice their opinions and concerns on a particular change to some project or standard. This allows the community to have a voice, while still eventually forcing an actual decision.

# Motivation
The RFC process aims to facilitate thoughtful discussion in the decision-making process for the Quilt organization. This Rust-inspired RFC process encourages contributors to get changes approved by submitting a detailed RFC, which will accept criticism until the team in charge of the project motions for a final comment period, wherein the team states its intent to merge, close, or postpone the RFC, allowing for resolution of the RFC unless a substantial argument arises.

This will allow us to put a time limit on unproductive discussion where necessary, so actual progress can still be made while giving the community a place to voice their opinions.

# The Quilt RFC Process
This document gives an outline of how the RFC process works and explains how contributors should submit their RFCs to the project. 

## When you must submit an RFC
The RFC process requires that any large top-level organizational changes should be preceded by a "Request For Comments" document just like this one, explaining the proposed change, the motivation behind it, potential drawbacks, and other considerations. These documents are then discussed and revised by the community for some period of time until a maintainer decides that enough discussion has happened and begins a final comment period, after which a final decision is made.

Most smaller changes to Quilt projects (such as bugfixes) can be submitted via the normal GitHub pull request system, however some are substantial enough that we would prefer that members of the community are given some time to discuss the potential change and give their feedback.

While this document describes when RFC's are required on an organizational level, individual organizational units, such as the Quilt Standard Libraries or Quilt Loader teams, may also require RFCs.

## Creating an RFC
A proper RFC is a high quality, well thought out document describing in detail all the possible benefits, drawbacks, and other consequences of the suggested change. A low effort or low quality RFC is likely to be rejected quickly.

Before beginning the process, it can be helpful to discuss your ideas with other community members on our [discord server](https://discord.quiltmc.org/) to get a feel for the community's opinion on the issue.

Once you have a good idea of what you want to suggest, and how the community feels about it, you can begin writing your RFC.

## The Process
When you are ready to submit an RFC, the process will work as follows:
- Fork the RFC git repository
- Copy `0000-template.md` to `0000-my-suggestion.md` (replace `my-suggestion`
  with a descriptive name for your RFC). Don't change the number yet; you will
  replace the `0000` with your pull request number from GitHub once you have
  submitted the PR.
- Fill in the template. This should be done with care and effort. You are
  making a major suggestion for a change to the project &ndash; take it
  seriously.
- Submit a pull request. An open pull request represents an RFC which is still
  in the commentary phase. You will receive comments and feedback from other
  community members. You should be prepared to revise your RFC in response to
  some of this feedback.
- Now that you have an open pull request, rename your markdown file to replace
  the `0000` with your PR number.
- Your PR will be labelled with the relevant project team's label. That team
  will be responsible for handling the RFC process from here on.
- Build consensus among the community and incorporate feedback from others.
  RFCs with broad support are more likely to be accepted.
- The team responsible for the RFC will discuss the RFC as much as possible in
  the pull request's comments. Discussion that occurs in other spaces should be
  summarized in the PR comments.
- When the team responsible for the RFC decides that there has been enough discussion, a member of the team will issue a "motion for final comments". If the motion passes, it begins a 10 day period where the community has the last chance to give feedback on an RFC.
  + This step is taken when enough discussion has happened to make a final decision on the issue. It is designed to prevent RFCs from becoming stales.
  + A consensus among all community members *is not necessary* for an RFC to
    enter the final comment period. However, there should not be a strong
    consensus *against* the disposition given for the final comment period, and
    some argument supporting the decision should be clearly presented somewhere
    in prior discussion.
  + If discussion has gone on for a long time, the team member who motions for
    final comments should post a summary comment outlining all of the
    discussion that has occurred.
  + Before actually entering the final comment period, all members of the team
    must sign their approval.
  + When a team member motions for final comments, they also give a
    "disposition" which can be either accept, reject, or postpone. This
    determines what will happen to the RFC at the end of the final comment
    period, although the disposition may change during that period.
- At the end of the final comment period, the RFC will be either accepted or
  rejected depending on its disposition at the close of the final comment
  period.
    + If an RFC is accepted, its PR will be merged into the main RFCs
      repository on GitHub and an issue (or issues) will be created on the
      relevant repositories to actually implement the changes. This does not
      necessarily mean that PRs implementing it will be automatically merged.
      All PRs are still subject to code quality standards and the like.
    + If an RFC is rejected, its PR will be closed and the suggested changes
      will not be implemented.
    + An RFC may also be "postponed". This means the RFC is effectively
      rejected at the moment because it does not fit into the project's
      near-term goals, but may be accepted at a later date.

# Drawbacks
- This RFC process will require constant moderation in order to ensure the
  quality of submitted RFCs. Low quality submissions and comments will need to
  be removed.
- This process does not actually eliminate the bikeshedding problem, but merely
  puts a time limit on it. At the end of the final comment period, it's
  possible community members may still not be happy with the final state of the
  document.
- If the team assigned to an RFC cannot come to a consensus on whether to enter
  the final comment period, an RFC could potentially end up stuck in limbo
  until the team can agree.
- If an RFC is required for too many changes, it could slow down the
  development process.

# Prior Art
This document and the process described herein was inspired heavily by the Rust programming language's own [RFC process](https://github.com/rust-lang/rfcs). The Rust language project has been quite successful in its RFC-directed development process and overall governance structure, and a lot of this could potentially help solve some of the problems that the Fabric project experienced.


# Unresolved Questions
- How do we decide who is on the teams that will be responsible for reviewing
  RFCs?
- How do we determine when enough discussion has happened for the final comment
  period to begin?
