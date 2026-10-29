# AI Proposal Triage Design

**Assessment reference:** MATACS-TH-2026-02  
**Candidate full name:** Muhammad Rehan  
**Actual time spent:** 60 minutes

This submission designs an AI-assisted proposal review process for the Commercial Solar Installation category. AI extracts evidence; configurable rules check eligibility and calculate scores; a person reviews uncertain cases and makes the final award decision.

## Deliverables

| File | Purpose |
|---|---|
| [WRITEUP.md](WRITEUP.md) | Design assumptions, scoring rubric, worked results and answers to all five assessment questions; under 1,000 words. |
| [Process-flow diagram](diagram/process-flow.pdf) | One-page flow showing AI, rules, human checkpoints, exception handling and criteria changes. |
| [PROMPTS.md](PROMPTS.md) | Exact intended extraction message templates, structured output contract and validation requirements. |
| [ranking.json](ranking.json) | All five proposal outcomes using the required fields and status values. |
| [README.md](README.md) | Candidate details, file index and AI-use disclosure. |

The numerical results are worked design examples, not output from a deployed or executed proposal-processing application. The optional prototype is not included.

## Reading the sample results

The demonstration uses configuration **commercial-solar-demo-v1**. The scoring targets are proposed design choices, not requirements supplied by the brief. A and E are ranked only under the explicit sample-only experience assumptions in the write-up; their project dates have not been verified. C and D remain flagged, and B is ineligible as submitted. All calculations and status reasons are explained in the write-up.

The original brief and sample proposal text are intentionally excluded. Proposals are identified by A-E. Runtime source documents would be supplied separately; no API keys or real client data are included.

## AI-use disclosure

I used ChatGPT/Codex to help interpret the brief, draft the design and prompts, create the diagram, and check scoring and consistency. I adopted a correction identified during the AI-assisted review: monitoring can mean documented progress reporting or performance monitoring, rather than only post-installation monitoring. This matches the stated criterion, which does not impose that restriction.
