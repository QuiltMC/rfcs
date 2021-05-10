# Summary

This RFC amends the RFC process with the creation of a specifications repository, which holds the body of some policy defined through RFCs here.

This RFC does not seek to change the way policy and documentation is reviewed and accepted, only how it is amended and referenced.

# Motivation

Currently, policy on team structure, voting, and other critical aspects of governance are defined only through RFCs.
This is an issue when a major part of that RFC needs to be redefined--the entire RFC template is excluded for these changes! This is fine for clarifications, but in large-scale changes that require lots of discussion, the current process works against the point of defining these documents through RFCs in the first place.


# Explanation

The `quiltmc/specifications` repository created by this RFC can hold five major types of specifications:

1. File format specifications
2. Community policy
3. Governance policy (community and technical teams)
4. Project policy (standards of contribution, requirements for merging a pull request, version cycles)

For simplicity, each of these kinds of documents will be called specifications within this RFC.

Each specification would be its own file, and creation or major revisions (to be determined by a case-by-case basis) of a specification requires an RFC.
Clarifications and other minor revisions can be made by making a PR directly to the specification repository.


Each specification only contains the actionable parts of the RFCs involved (usually summary and explanation). Motivation, Rationale, Unresolved Questions, etc. are left only on the RFC.

## Requirements for Creation

- Creation of a specification is required for file specifications, community policy, and governance policy.
- Creation of a specification for project policy is only required if the project is expected to be large in scope. For example, QSL needs a clear-cut process for merging PRs, but Quiltflower or tiny-mappings-parser would not. It is not a goal to define when a project is "large enough" to need project policy specifications, but it should be handled at the managing team's discretion.

## Other details

How general policy is split into specifications is not meant to be 1 rfc -> 1 file; maintainers should go for clarity and ease of reference.

# Drawbacks

- This change, if accepted, increases the complexity of submitting an RFC to Quilt, by requiring an additional PR to be made after acceptance of most RFCs. Additionally, it is not clear when an RFC will be required for a revision, which might discourage people from contributing revisions to Quilt specifications.
- This change assumes that it is not desired for RFCs to be the central authority on policy, but instead to provide a look at the initial motivation and intentions behind a change *before* it is implemented.

# Unresolved Questions

- Should the restrictions on technical definitions be stricter? relaxed? or are they fine?
- Is this change even needed, or will the current amendment process be fine?

# Rationale and Alternatives

- Having a specifications repository keeps all official documents about policy in one spot.
- We could continue with the existing process, but having policy mixed between RFCs is not desirable as described in the Motivation section above.


# Expected Response

This change should make it easier for the community to locate a central authority on policy and specifications, and encourage long-form discussion of major revisions to community policy through the proper RFC process.
