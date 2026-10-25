# Domain model card: Catalog guide titles

## One-sentence definition
A guide mapping associates a scaffold component with a public documentation path and human-readable title for guide selection and offline keyword search.

## Why it is needed
[Public fork issue #4](https://github.com/leejongyoung/egovframe-scaffold-mcp/issues/4) identifies filename fallbacks that prevent Korean title searches, including 그룹관리.

## Existing models and terminology
Published tools `search_egovframe_docs` and `get_egovframe_guide` expose catalog guide mappings; see [guide.ts](../../src/guide.ts) and [tools](../../docs/tools.md). Observed `searchDocs` awards title matches three points and deduplicates by path. Observed generator `computeDocsMap` chooses dominant class references and uses a filename when its title regex misses. Issue claims are independently checked below; no policy to expand mappings is inferred.

## Synthetic example data
A fictitious Markdown document has `---`, `  title: "예제: 그룹 관리"`, `---`, and three references to `egovframework.com.sec.gmt`. Its mapping has string `path` and `title`; absent titles retain the existing filename fallback.

## Successful behavior
An indented title produces its human-readable value, preserving embedded colon and quote content. Existing unindented titles remain equivalent. Searching 그룹관리 should return `common-component/security/group-management.md` with title 그룹관리 (issue #4 and actual public docs). Paths, component assignment, source archive fingerprint and non-title fields remain identical.

## Rejected or invalid behavior
Empty search returns no results; unknown guide component IDs and out-of-range guide indexes are rejected by `getGuide`. These existing consumer boundaries are outside title parsing. No new YAML validation or rejection policy is proposed; missing/unrecognized title syntax retains filename fallback.

## Outside this concept’s responsibility
Do not expand the dominant-reference mapping heuristic, add the 29 overview documents, edit the documentation repository, change ranking, refresh archive provenance, or alter tool schemas.

## Rule sources and assumptions
| Rule | Source and revision/date | Evidence kind | Assumptions or limits |
| --- | --- | --- | --- |
| Offline title/path/component search | src/guide.ts at a23a64f | Observed | No body indexing |
| Dominant references select guides | scripts/generate-catalog.mjs at a23a64f | Observed | No mapping expansion |
| Indented titles are real input | egovframe-docs 94ce018e817fa2bddde6aad3a02cbefeca8afcda | Observed public source | Markdown extracted from pinned archive |
| Preserve public compatibility | [constitution](../memory/constitution.md) | Local policy | Work-only artifact |

## Open questions
Whether to map overview documents belongs to upstream maintainers and is excluded from this fix. Full YAML support is not established by the current contract and is unnecessary for the evidenced whitespace bug.

## Next step and evidence of completion
[Assessment](../bugs/catalog-guide-titles/assessment.md). Before changes, fresh upstream a23a64f built successfully and `searchDocs({query:"그룹관리"})` returned `[]`; 24 filename-title entries represent 18 unique paths. Final generator and consumer verification pending.
