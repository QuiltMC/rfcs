## Summary

This RFC describes a privacy-friendly mechanism through which the loader can signal themselves as an active user, in order to provide MAU (Monthly Active User) metrics. 

## Motivation

When discussing with partners, requesting grants or negotiating sponsorships, it is quite important for The Quilt Project to be able to give accurate MAU metrics. 
The choice to not include any metadata in the signal was made to keep the mechanism privacy-friendly, as Quilt collects zero personal data or usage statistics.

## Explanations

On launch, the loader will start the following process asynchronously:
1. Check if the `loader.disable_beacon` property or the `QUILT_LOADER_DISABLE_BEACON` environment variable is set to `true`. If so, the process is aborted (the launch process is unaffected).
2. Check for the last signalled month in a standard persistent location, if it is equal to the current month the process is aborted.
3. Send a `POST` request to `https://beacon.quiltmc.org/signal` without any specific body or headers.
4. Save the current month into the same standard persistent location as step 2.

This process and opt-out methods MUST be clearly documented through the appropriate means.

## Beacon Server Restrictions

The server hosted at `beacon.quiltmc.org` MUST:
1. Be open-source.
2. Be auditable by any Quilt developer who wish to, within reason.
3. Not store any data present inside the request.
4. Not process any data present inside the request beside what is strictly required to establish the connection, following current best practices and technical feasibility. 

## Drawbacks

This process can be seen as privacy-breaking by some. We implemented an opt-out and decided to not collect any data and to make the process open source to minimize as much of that concern as possible. 

## Rationale and Alternatives

We've explored various alternatives to this, who didn't pan out for different reasons:
- Download statistics: technical infeasibility.
- Installer statistics: only represent a small part of the downloads.
- QSL download statistics: not considered as a "reliable source" by third parties.

It is also worth noting all of these provide download statistics and not MAU count.

## Privacy Statement

Nor in this RFC or in any further RFC is Quilt going to implement data collection without explicit user consent and without any functionality loss if said consent isn't given, unless said functionality technically requires the data to be collected.
