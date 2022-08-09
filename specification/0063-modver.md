# Summary

This RFC introduces the Modular Versioning (ModVer) specification format, for use in quilt.mod.json
and other locations where SemVer is too specific to be user-friendly.

In contrast to SemVer, ModVer allows:
- More than three version segments (1.0.0.5) or less than three version segments (1.0), with
    non-existent segments assumed to be 0 for comparison purposes.
- Consistent support for dots in alpha, beta, RC, and prerelease suffixes (1.2.3-pre.5)
- Formalized dependency version metadata (1.5+1.19.2)

# Motivation

SemVer is *very* strict. This is both a blessing, as it makes writing a parser for SemVer much
easier, and a curse, as it prevents a lot of genuinely important data from being included in the
version. There have been multiple discussions in Quilt spaces that have ended in a consensus of the
SemVer requirement in Quilt needing a replacement, but up to now nobody has put in the effort to
write a proposal for its replacement.

This RFC is still a draft; its intent is stated above, and a full formalized specification will be
written when I have the time. Others are welcome to write a BNF or spec for the format and
contribute it to this RFC in the meantime.