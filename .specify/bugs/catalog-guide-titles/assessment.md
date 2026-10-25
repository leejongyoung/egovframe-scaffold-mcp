# Bug Assessment: Indented catalog guide titles

- **Slug**: catalog-guide-titles (auto-generated in dispatched non-interactive work)
- **Created**: 2026-10-26
- **Source**: https://github.com/leejongyoung/egovframe-scaffold-mcp/issues/4
- **URL policy**: host github.com; allowlisted HTTPS; public issue fetched through gh.
- **Verdict**: valid
- **Severity**: medium

## Report (summarized)
Issue #4 reports 18 guide paths whose titles fall back to filenames because `title:` is indented, preventing Korean title searches. The request excludes mapping expansion and documentation-source edits.

## Symptom
Built fresh upstream returns no result for 그룹관리, despite a real mapped guide whose public Markdown title is 그룹관리. There are 24 mapping entries across 18 unique affected paths.

## Reproduction
1. Fetch actual public upstream EricSeokgon/egovframe-scaffold-mcp main: a23a64f9970183da007ae36f1655aa16ae5489fc; it equals preserved upstream-submit.
2. Create isolated Orca worktree; `npm ci --ignore-scripts` then `npm run build` (required reproduction preparation).
3. `node --input-type=module -e 'import {searchDocs} from "./dist/index.js"; console.log(JSON.stringify(searchDocs({query:"그룹관리"})));'` prints `[]`.
4. Public docs at 94ce018e817fa2bddde6aad3a02cbefeca8afcda contain indented `  title: 그룹관리`.

## Suspected Code Paths
- scripts/generate-catalog.mjs: computeDocsMap title regex anchors title at column zero.
- catalog/components.json: generated docs title fields.
- src/guide.ts: searchDocs title ranking and getGuide selection (consumers, no code change expected).

## Root Cause Hypothesis
High confidence: the column-zero regex misses indented frontmatter keys and emits the filename fallback. Upstream and fork open/closed PR lists checked on 2026-10-26; no matching contribution exists. Upstream open PR #71 concerns CI dependency audit and remains untouched.

## Proposed Remediation
Preferred: accept horizontal whitespace before title while retaining current quoted/unquoted extraction conventions, regenerate using pinned common-components archive and docs snapshot, and verify only title fields change. Add offline consumer regressions and generator behavior coverage with synthetic Markdown, including colon/quotes and missing-title fallback.

**Files likely to change**: scripts/generate-catalog.mjs, catalog/components.json, test/docs-search.mjs, focused generator test if needed.

**Tests to add or update**: filename fallback absence for current catalog; exact Korean title/path search; indented versus column-zero extraction compatibility.

## Risks & Considerations
No API change or mapping expansion. Preserve archive metadata and all other catalog values. `npm ci` reports five existing dependency audit findings; dependency upgrades are outside scope. Full ZIP extraction encountered a filename error; only Markdown files were extracted separately for deterministic input.

## Open Questions
Overview-page mapping needs upstream product judgment and is excluded. No blocker to this narrow fix. No private security reports accessed.
