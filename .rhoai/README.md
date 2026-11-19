# API contract checks

[The workflow](../.github/workflows/api-contract.yml) configures OpenAPI
compatibility and coverage checks on pull requests to `develop`, using central
`@latest` workflows with every job in `mode: enforce`.

Execution is intended in GitHub Actions in `opendatahub-io/NeMo-Guardrails`, not
Tekton. Successful midstream CI and working-group registration remain pending.

[The descriptor](contracts.yaml) defines scope, exclusions, and tool versions for:

- `guardrails-api-v1`: `fern/openapi.yml`
- `guardrails-hooks-api-v1`: `fern/hooks-openapi.yml`

Both fixed baselines are enforced, with full SHAs recorded in the descriptor
and workflow:

- Required by the ticket: the GA tag `refs/tags/release/odh-3.5` (`623b1acc…`).
- Additional drift comparison: the RHOAI 3.6 release base (`0ab8b319…`).

The newer comparison supplements the GA baseline. Replacing a baseline with a
later commit requires approval from someone other than the author.

`guardrails-api-v1` has a per-contract baseline override at
`8fb0585fc849e945e715092dfbe662c78c4a3760`. That commit is the first revision
after `v0.25.0.rc1+rhaiv.1` that tracks the current OpenAPI specification, so
it is the practical comparison point for the upstream API changes carried into
RHAI 3.7. The override supersedes the repository baselines for that contract;
the `odh-3.5` to `v0.25.0.rc1+rhaiv.1` differences are intentional upstream
changes and are documented here rather than gated as breaks.

Regenerate specs with `make docs-fern-generate-openapi` and commit them alongside
source changes. The specification workflow fails if regeneration changes either
tracked spec; do not edit generated specs manually.

For local `check` and `coverage` commands, see the
[OpenAPI guide](https://github.com/red-hat-data-services/api-contract-tests/blob/main/docs/onboarding/openapi-compat.md).
Use this descriptor's baselines and tool versions. Onboarding requires passing CI,
a merged PR, and working-group registration of the repo, descriptor path, and revision.
