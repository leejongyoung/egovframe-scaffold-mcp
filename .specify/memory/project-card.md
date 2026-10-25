# egovframe-scaffold-mcp project context

Observed from the `work` source at `a23a64f9970183da007ae36f1655aa16ae5489fc` on 2026-10-26.
This is a maintenance context card, not a claim that application checks were run during adoption.

## Role and public entry points

Community PoC MCP server and CLI for eGovFrame project/CRUD/configuration generation, component assembly, migration, checks and delivery. Public surfaces include `src/index.ts`, `catalog/`, packaged `dist/index.js`, CLI commands, MCP tools/resources/prompts, `README.md` and `docs/tools.md`.

## Evidence and maintenance boundaries

- `README.md`: tool/CLI surfaces, allowed roots, dryRun/transaction and supported scope.
- `package.json`: Node >=22, build/start/smoke and focused test scripts, prepublish gate.
- `docs/development.md`: offline versus network/live verification, release gate and local development.
- `docs/versioning.md`: public API and SemVer policy.
- `.github/workflows/ci.yml`: cross-platform Node 22/24 gate and separate live integration checks.
- No root AGENTS.md or CodeGraph index was present in the clean adoption checkout.

Keep the change scope and workflow policy in the [constitution](constitution.md); concept cards
link there rather than repeating the policy. Author a concept card only for actual planned work.

## Build and verification commands

- Development documented in `docs/development.md`: `npm install`, `npm run build`; `npm start` runs built server.
- Focused examples: `npm run smoke`, `npm run test:api-contract`, `npm run test:output-schemas`, `npm run test:allowed-roots`, `npm run test:transaction`; choose the relevant test for an application change.
- Release gate: `npm run prepublishOnly`, a broad build/test sequence; not run for tooling-only adoption.
- Live scripts such as test:templates, test:components, test:network-live and CI integrations need external assets/network; JDK/Maven/Gradle/Docker requirements vary by test. Distinguish offline fixture checks from live verification.

For this tooling-only adoption, verify Spec Kit CLI/integration status, installed core/bug commands,
constitution completeness, and template resolution/scaffold presence. Full application build,
lint, tests, formatter, external services, and deployment are not part of that smoke check.

## Upstream and submission baseline

Actual upstream is `EricSeokgon/egovframe-scaffold-mcp`, default `main`, verified through remote metadata. Keep existing checkout work and upstream defaults intact.

Prepare `upstream-submit` only if absent, at the freshly fetched actual upstream default SHA. Use a separate Orca submission worktree and selected application/test/user-doc patches; never merge work, copy its adoption commit or include work-only cards/scaffolding. Preserve any existing submission branch and open PR.
No PR is created by adoption. Do not merge `work` or cherry-pick its adoption commit into a submission.
Preserve existing submission branches and PRs. Inspect changed paths and commits against the exact
base before any later PR; exclude work-only scaffolding without deleting upstream-owned files.

## Working with the pinned tooling

Installed by `uvx --from specify-cli==1.2.3 specify init --here --force --non-interactive
--integration omp --script sh --extension bug` after checking for existing path conflicts.
The CLI's init metadata pins 1.2.3; native agent commands live in `.omp/commands/`, including
`speckit.bug.assess.md`, `speckit.bug.fix.md`, and `speckit.bug.test.md`.
Use official upgrade support for future changes and preserve local customizations.
The new [domain model card template](../templates/model-card-template.md) aligns concept meanings
before specification; it is not an ML performance or benchmark card.

Observed submission base: `a23a64f9970183da007ae36f1655aa16ae5489fc` (EricSeokgon/egovframe-scaffold-mcp, `main`).
Submission branch status: created-remote-at-upstream-default.
