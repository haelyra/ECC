# ECC-029 carrier slice evidence

Date: September 8, 2026. Milestone: M1 canonical context profiles. The P2a/P2b/P2c stack follows [PR #3037](https://github.com/affaan-m/ECC/pull/3037), based on main `5064474d4d762dc9640234a41617cccb79185cec`. Environment: macOS 26.6.2 arm64, Node 24.9.0, ECC 2.2.1. This source-only report records local development evidence. The packed [carrier contract](context-carriers.md) defines the public boundaries.

## Test-first slices and review regressions

| Slice or regression | RED checkpoint | GREEN checkpoint and evidence |
| --- | --- | --- |
| P2a explicit required-resource output | `3b3a7c72`: 3 resource cases passed, 10 failed for missing declarations | `935861ac`: 13 resource cases pass; resource byte digests retain their meaning, while declaration changes affect provenance |
| P2b pure five-layout file planner | `09ec70d9`: 20 cases fail for the intended missing public module | `bdb317eb`: 22 planner cases pass, including subsequent path-alias regressions |
| Read-only carrier CLI journey | `3b3a7c72`: 1 CLI case passed, 6 failed for missing command behavior | `bdb317eb`: 7 cases pass; deterministic JSON, five layouts, exclusions, unsupported targets, argument rejection, unchanged temporary caller state |
| Packed public surface | `a2963136`: both publish-surface cases fail for the missing carrier contract | `bdb317eb`: 2 cases pass with the library, schema and public contract included |
| Portable path collision rejection | `337c560c`: 20 planner cases passed, 2 failed for case/NFC-equivalent directory prefixes | `bdb317eb`: all 22 pass; aliases with different child names fail before returning an artifact |
| P2c disposable acceptance fixture | `09ec70d9`: the intended helper entry point is absent | `fccadba2`: 19 fixture cases pass, including independent expected-plan and manifest checks, source removal, binary bytes, tampering, symlinks and cleanup |
| Fixture aliases fail before writes | `9454a0d5`: 17 cases passed, 2 failed because staging performed 6 writes before rejection | `fccadba2`: both adversarial cases reject with zero writes |

Preserve the RED/GREEN commits. Independent security/code review checked the file planner and CLI, reproduced the portable ancestor collision, and approved the corrected implementation. The acceptance helper received separate review and remains test-only. Source files and skill bodies are data during these checks; scripts are copied but never executed. Narrow manifests omit hooks and MCP settings, while preserved authority-related skill metadata remains a separate pre-activation policy gate.

## Focused checks and coverage

```sh
./node_modules/.bin/c8 --all \
  --include='scripts/lib/context*.js' \
  --include='scripts/profile.js' \
  --include='scripts/ci/validate-context-profiles.js' \
  --reporter=text --reporter=json-summary \
  --reports-dir=/tmp/ecc-029-carrier-coverage \
  --check-coverage --lines=80 --functions=80 --branches=80 --statements=80 \
  node --test tests/lib/context-pack-registry.test.js \
  tests/lib/context-profiles.test.js tests/lib/context-resources.test.js \
  tests/lib/context-carriers.test.js tests/lib/context-carrier-fixture.test.js \
  tests/scripts/profile.test.js tests/scripts/profile-carrier.test.js \
  tests/ci/context-profiles.test.js
node tests/scripts/npm-publish-surface.test.js
npm run lint
npm test
git diff --check
```

Focused results: 119 logical cases passed, zero failed or skipped. The breakdown is 18 registry, 12 compiler, 13 resource, 22 carrier, 19 fixture, 25 original CLI, 7 carrier CLI and 3 CI cases. Node's outer TAP summary reports 93 because the original CLI and CI files each wrap their own cases.

Runtime coverage: 98.33% statements/lines, 91.16% branches and 100% functions. All thresholds pass. A separate test-helper-inclusive review run reports 100% statements/lines/functions and 90.54% branches for that helper. Runtime coverage excludes test infrastructure.

## Real inventory and package verification

All ten source-tree Lean/Full combinations across Claude, Codex, Pi, OpenCode and Cursor passed disposable structural verification against the actual canonical inventory. Full contains 286 skills and 464 bundled files. Claude, Codex and Pi add one narrow manifest, giving 465 files; OpenCode and Cursor retain 464. Lean contains 3 skills and 3 source files, plus a manifest where applicable.

At implementation head `d52d3430`, the full `npm test` exited 0 and its legacy aggregate reported 4,423 passed and zero failed. That aggregate does not separately count the new node:test cases, which are reported explicitly above. Full ESLint/Markdown lint and whitespace checks passed before this source-only evidence update.

A real `npm pack` ran the normal prepack build. The archive SHA-256 was `dd0577889bfa09071cbd87b430b200f8d0eaf036c6b0fb583dc71ae2f855fd78`. A disposable consumer installed it with `npm install --offline --ignore-scripts --omit=dev --no-audit --no-fund --userconfig=/dev/null`, using a task-local cache explicitly primed online during the preceding PR-readiness check. This proves an offline cached install, not a dependency-free install.

The installed public dispatcher produced all ten Lean/Full carrier objects with deep equality to the checkout, including their complete digests. Each installed artifact then passed structural materialization using the installed package's own canonical skill resources and an independently compiled expected plan. Full's 464 bundled resources were verified in every layout. The isolated subprocess environment was allowlisted and its disposable home remained absent. Packed runtime resolution confirmed js-yaml 4.3.2.

A separate policy simulation denying Windows file symlinks passed all 54 new resource/carrier/fixture cases with zero skips. Directory links use junctions on Windows. This simulation supplies no native Windows filesystem or provider evidence.

Hosted review of the prerequisite PR subsequently identified dry-run argument ordering and directory-enumeration bounds. Fixes and their dependent-stack revalidation are recorded separately below when complete; the `d52d3430` results remain a pinned earlier checkpoint.

## Claims deliberately left unobserved

Native discovery, exact native exclusions, invocation, executable-mode needs, workflow outcomes, activation, hook consent, rollback, automatic routing and actual token savings still require their own gates. Schema validation checks shape; it cannot certify supplied artifact semantics. The independently compiled fixture checks exact layout, selection, file set and bytes. It uses a trusted private temporary parent and does not certify an arbitrary-destination transaction writer against hostile concurrent mutation. No native provider, model, container or VM was launched, and no package was published.
