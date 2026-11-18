# Document model decision log

> Only Opus writes this file (`docs/model/DOCUMENT_MODEL.md` sections 11 and 13). One row per decision. Old rows are never edited; a later decision is a new row.

## Decisions

| decision_id | decided_on | model_version | kind | doc_key | finding_ids | decision | next_doc_status | approved_by_role |
|---|---|---|---|---|---|---|---|---|
| `MD-001` | 2026-10-17 | `CDP-DOCMODEL-v1` | `MODEL_CREATED` | | | First version of the model, BRD template `CDP-BRD-v1`, and Mapping template `CDP-MAPPING-v1`. `model_status` is `MODEL_DRAFT`. Waiting for Opus review and user approval | | |
| `MD-002` | 2026-10-17 | `CDP-DOCMODEL-v1` | `MODEL_CHANGED` | | | User decisions: the BRD has no personal-data-and-consent section (Mapping keeps `data_class`); model and role files are in English, templates and user-facing files in Vietnamese; template and architecture changes are handled by Opus; Composer 2.5 runs at default effort, not Fast. `model_status` stays `MODEL_DRAFT` | | |
| `MD-003` | 2026-10-17 | `CDP-DOCMODEL-v1` | `ARCHITECTURE_CHANGED` | | | User decision: a Codex review is in English with a Vietnamese summary of at most 10 lines under `## Tóm tắt`, placed right after the `REVIEW_VERDICT:` line. Codex's reply contains nothing outside the review block | | |

## Allowed values

| field | value | Meaning |
|---|---|---|
| `kind` | `MODEL_CREATED` | New model created |
| `kind` | `MODEL_CHANGED` | Model edited while `MODEL_DRAFT` |
| `kind` | `MODEL_APPROVED` | User approved. `approved_by_role` is required |
| `kind` | `MODEL_RETIRED` | A `model_version` is no longer used for new documents |
| `kind` | `ARCHITECTURE_CHANGED` | User-requested change to role files, `AGENTS.md`, `README.md`, or folder layout |
| `kind` | `FINDING_RESOLVED` | A `MODEL` finding is handled for one `doc_key`. `finding_ids` and `next_doc_status` are required. If the documents must move to a new model, `model_version` holds the new version |
| `kind` | `FINDING_REJECTED` | Opus finds the `MODEL` finding incorrect. `decision` gives the reason with the model section. `next_doc_status` is required |
| `next_doc_status` | `CHANGES_REQUESTED`, `READY_FOR_REVIEW` | The status Composer sets on the BRD and Mapping after the decision |
