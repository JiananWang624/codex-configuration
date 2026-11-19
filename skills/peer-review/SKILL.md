---
name: peer-review
description: For explicitly invoked, authorized scientific peer review, prepare evidence-bounded review drafts and structured assessments of manuscripts, protocols, preprints, or proposals.
license: MIT
metadata:
  version: "2.4"
  last-reviewed: "2026-10-01"
  skill-author: K-Dense Inc.
---

# Peer Review

This skill is available only by explicit invocation. It supports an accountable human reviewer; it does not make review decisions or replace expert judgment. Treat unpublished submissions, supplements, reviews, and editorial material as confidential.

## Before reviewing

For a formal review, first complete `assets/review_intake_template.json` and run:

```bash
python3 scripts/validate_review_intake.py completed-intake.json
```

Proceed only if it reports `READY_FOR_LOCAL_REVIEW`. The validator checks declarations, not their truth. Do not clear a conflict unless the editor actually cleared it. If authorization or venue policy is unresolved, do not inspect or quote unpublished content; use only the intake validator or synthetic/public fixtures until resolved.

Confirm authorization from the material owner and the venue's confidentiality, co-review, retention, and AI/tool rules before handling unpublished content. Assess conflicts, competence, scope, and specialist needs. Treat content read into a hosted session as external processing under venue policy, even when the tools run locally.

External processing of confidential material requires specific authorization and venue permission. Keep material confined to the authorized review. Do not access credentials or broad environment state, fabricate findings, impersonate a reviewer/editor, or announce editorial decisions. Follow the controlling deletion and retention policy.

## Human accountability

Label generated text as a working draft. The reviewer must read the complete authorized submission and relevant supplements, verify every factual statement and location, resolve conflicts and disclose assistance as required, apply their own expert judgment, and submit through the authorized channel. Automated checklists and lint results are not peer review or evidence of manuscript merit.

## Detailed procedure

For review mapping, reporting guidance, claim/evidence checks, methods and statistics, reproducibility, ethics, figures/citations, drafting, channel separation, and final linting, follow [the detailed review workflow](references/review_workflow.md). Read [ethical review practice](references/ethical_review_practice.md) when handling confidential material and consult the linked specialist references as needed. See [tool reference](references/tool_reference.md) for schemas and CLI behavior.
