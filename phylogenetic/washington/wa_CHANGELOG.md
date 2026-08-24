# CHANGELOG

We use this CHANGELOG to document breaking changes, bug fixes, and config value changes that affect the WA build. Changes to the Nexstrain build as a whole are logged by the Nextstrain team in the repo root: Changelog.md.

## 2026

* 30 July 2026: Change min_length from 8,000 to 14,000 for all subsamples. Results in loss of 32 WA seqs, 53 non-WA USA seqs, 0 non-USA North American seqs
* 27 July 2026:
  *  Change divergence units to mutations.
  *  Fix subsampling scheme (was present but not actually implemented) - build is not impacted because thresholds are set > # of available seqs
* 21 July 2026: Merged breaking changes from upstream. Build specifications were not changed, but 3 non-WA sequences were dropped from build for unknown reason.

