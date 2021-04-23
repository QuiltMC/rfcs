# Summary

This RFC amends the RFC process with the creation of a specifications repository, which holds the body of some policy defined through RFCs here.

This RFC does not seek to change the way policy and documentation is reviewed and accepted, only how it is amended and referenced.

# Motivation

Currently, policy on team structure, voting, and other critical aspects of governance are defined only through RFCs.
This is an issue when a major part of that RFC needs to be redefined--the entire RFC template is excluded for these changes! This is fine for clarifications, but in large-scale changes that require lots of discussion, the current process works against the point of defining these documents through RFCs in the first place.


# Explanation

The `quiltmc/specifications` repository created by this RFC can hold three major types of specifications:

1. File specifications
2. Governance policy
3. Technical definitions (see: ASMR, QSL)
   For simplicity, each of these kinds of documents will be called specifications within this RFC.
   Each specification would be its own file.
   Creation or major revisions (to be determined by a case-by-case basis) of a specification requires an RFC.
   Clarifications and other minor revisions can be made by making a PR directly to the specification repository.

Each specification only contains the actionable parts of the RFCs involved (usually summary and explaination). Motivation, Rationale, Unresolved Questions, etc. are left only on the RFC.

## Requirements for Creation

- Creation of a specification is required for file specifications and governance policy.

- Creation of a specification for technical definitions is at the author's discretion, but may be required in cases where major parts of the document are expected to be revised, or if an amendment PR to the RFC repository is deemed as "too big" and needing its own RFC.

# Drawbacks

- This change, if accepted, increases the complexity of submitting an RFC to Quilt, by requiring an additional PR to be made after acceptance of most RFCs. Additionally, it is not clear when an RFC will be required for a revision, which might discourage people from contributing revisions to Quilt specifications.
- This change assumes that it is not desired for RFCs to be the central authority on policy, but instead to provide a look at the initial motivation and intentions behind a change *before* it is implemented.

# Unresolved Questions

- Should the restrictions on technical definitions be stricter? relaxed? or are they fine?
- Is this change even needed, or will the current amendment process be fine?

# Rationale and Alternatives

- Why is this the best possible design?
- What other designs are possible and why should we choose this one instead?
- What other designs have benefits over this one? Why should we choose an
  alternative instead?
- What is the impact of not doing this?


# Expected Response

This change should make it easier for the community to locate a central authority on policy and specifications, and encourage long-form discussion of major revisions to community policy through the proper RFC process.
