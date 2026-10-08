# macos-stock-bash floor check - live drive under self-built GNU bash 3.2.57

The "Run snapshot consumers with stock Bash" step body was extracted from .github/workflows/ci.yml with yq
(the counting section onward), /bin/bash swapped for a locally compiled bash-3.2.57, and run with that
bash 3.2 as the step shell, against git-initialized copies of the tree.

| Scenario | Tree | Step exit | Result |
|---|---|---|---|
| s0 base commit 1d539ba2 (hardcoded 18) | base | 1 | `::error::expected 18 snapshot/fleet-view tests, got 20` - red job reproduced |
| s1 target commit 1f474349 | worktree | 0 | 90 ok lines (20+60+1+1+6+2), no error |
| s2 contributor adds a 21st snapshot test | copy | 0 | 91 ok lines, passes with no ci.yml change |
| s3 two snapshot tests removed from run list | copy | 1 | floor error: 18 passing, expected at least 20 |
| s4 stale FM_TEST_ONLY name for churn regression | copy | 1 | floor error: 0 passing, expected at least 1 |
| s5 a snapshot test fails | copy | 1 | `not ok - injected regression`, step stops before any count |
