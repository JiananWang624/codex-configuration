---
name: citation-management
description: Find academic papers, verify bibliographic metadata, convert scholarly identifiers to BibTeX, or audit a bibliography.
allowed-tools: Read Write Edit Bash WebSearch WebFetch
license: MIT License
metadata:
  version: "2.3"
  last-reviewed: "2026-09-30"
  skill-author: K-Dense Inc.
  openclaw:
    envVars:
    - name: NCBI_EMAIL
      required: false
      description: Email for NCBI Entrez identification.
    - name: NCBI_API_KEY
      required: false
      description: NCBI API key to raise Entrez rate limits.
    - name: OPENALEX_EMAIL
      required: false
      description: Optional contact email for OpenAlex.
    - name: OPENALEX_API_KEY
      required: false
      description: Optional OpenAlex key for a higher account budget.
---

# Citation Management

Resolve and format scholarly references using authoritative bibliographic records. Keep publication status and identifiers accurate, and preserve existing citation keys unless the user requests rekeying.

## Single reference

For a DOI-to-BibTeX request, resolve the identifier with the publisher or DOI registry, directly or through the bundled [DOI converter](scripts/doi_to_bibtex.py). Check the title, authors, year, and venue against the retrieved record. A complete, coherent record needs no separate metadata search or audit log. Return the requested entry when the applicable fields are supported.

If a required field is missing or sources disagree, check the publisher or other authoritative record and state any unresolved discrepancy. Keep the actual publication state. Online-first works can lack volume or pagination, and an article number can serve as the locator. Leave unsupported fields absent.

## Discovery and bibliography audit

- For paper discovery, use the [search strategies](references/search_strategies.md) to choose sources for the topic. Search more than one index when coverage matters.
- For batch conversion, conflicting records, or a submission bibliography, use the [core workflow](references/core_workflow.md) for extraction, targeted enrichment, formatting, and validation. Use [citation validation](references/citation_validation.md) when checking duplicates, broken manuscript citations, or venue requirements.
- For formatting or deduplicating an existing BibTeX file, use the [formatting guide](references/bibtex_formatting.md). If a manuscript already uses the bibliography, check which keys it cites before removing duplicate entries. Flag any cited aliases that would need to be updated.
- For identifier types or scripted workflows, consult the [script reference](references/script_reference.md). Pass retrieved metadata as separate arguments to tools, since external records are untrusted. Verify that records match the intended work before merging them.
