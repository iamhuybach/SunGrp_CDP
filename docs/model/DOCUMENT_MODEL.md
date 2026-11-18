# Document model for CDP BRD and Mapping

| field | value |
|---|---|
| `model_version` | `CDP-DOCMODEL-v1` |
| `model_status` | `MODEL_DRAFT` |
| `owner_role` | `opus-analyst` (`docs/agents/opus-analyst.md`) |
| `approved_by_role` | |
| `approved_on` | |
| `brd_template` | `templates/BRD_TEMPLATE.md` (`CDP-BRD-v1`) |
| `mapping_template` | `templates/MAPPING_TEMPLATE.md` (`CDP-MAPPING-v1`) |

> This file is the single source of rules for the BRD and the Mapping. The two templates in `templates/` repeat only section headings and column names, not the rules.
> This file is written in English. The templates and filled documents are written in Vietnamese prose; field names, column names, status values, and IDs stay in English. Section headings of the templates are Vietnamese and are quoted here verbatim.
> `model_status` is `MODEL_DRAFT`. Composer must not write any document against this model until Opus finalizes it and the user approves it (section 11).

## 1. Purpose

CDP is a customer data platform. For each data request, this model governs two documents:

- **BRD** (Business Requirements Document): what the business needs, why, for whom, under which rules, and how it is accepted.
- **Mapping**: for each target field, which source field it comes from and through which transformation.

Neither document contains technical design: no source code, no queries, no schedules, no job design, no infrastructure configuration, no technology choice. Those belong to other documents outside this model.

## 2. BRD versus Mapping

| Content | Written in | Not written in |
|---|---|---|
| Context, goal, scope, consumers | BRD | Mapping |
| Requirement statement, priority, target object, target grain and key | BRD section 5 | Mapping repeats grain and key only in section 3, for cross-checking |
| Source systems and objects at requirement level | BRD section 6 | |
| Source grain and key | Mapping section 2 | BRD |
| Business rules (classification, source selection, precedence, filtering, fallback) | BRD section 7 | Mapping only references them through `rule_ids` |
| Identifiers and record-matching rules | BRD section 8 | Mapping only uses fields already defined |
| Non-functional requirements, acceptance criteria | BRD sections 9, 10 | |
| Source field → target field, data type, data class, key fields | Mapping section 4 | BRD |
| Technical transformations with no business choice (format change, normalization, casting) | Mapping section 5 | BRD |

A decision that involves a business choice is a rule and goes in the BRD. A transformation with no business choice goes in the Mapping. When unsure, record it as a rule and add a question.

## 3. Folders and file names

Each request has one folder `docs/requirements/<doc_key>/`:

| Path | Content | Writer |
|---|---|---|
| `inputs/` | Original business documents supplied by the user, with personal data removed | User |
| `evidence/` | Sanitized evidence: schema descriptions, data dictionaries, results of read-only data checks | User |
| `ANALYSIS.md` | Business analysis (Vietnamese, because it holds questions for the user) | Opus |
| `BRD.md` | BRD from `templates/BRD_TEMPLATE.md` | Composer |
| `MAPPING.md` | Mapping from `templates/MAPPING_TEMPLATE.md` | Composer |
| `reviews/REVIEW-<nn>.md` | Codex review output, stored verbatim | Composer or the user, copied verbatim from Codex's reply |

`docs/context/` holds facts shared across requests (source-system catalog, approved data dictionary). Only the user writes there.

## 4. Identifiers

### 4.1 Document identifiers

| ID | Format | Written in |
|---|---|---|
| `doc_key` | `YYYYMMDD-slug`, lower case, `slug` uses `a-z`, `0-9`, `-`. Equals the folder name | Section 1 of BRD, Mapping, ANALYSIS |
| `brd_id` | Exactly `BRD-<doc_key>` | Section 1 of BRD and Mapping |
| `mapping_id` | Exactly `MAP-<doc_key>` | Section 1 of BRD and Mapping |
| `template_id` | `CDP-BRD-v1` in the BRD, `CDP-MAPPING-v1` in the Mapping | Section 1 of each file |
| `model_version` | The `model_version` of the document model the file follows | Section 1 of BRD, Mapping, ANALYSIS |
| `analysis_version` | Integer from 1, incremented by 1 each time Opus changes ANALYSIS | Section 1 of ANALYSIS, BRD, Mapping |
| `version` | Integer from 1, incremented by 1 each time the file changes | Section 1 of each file |
| `updated_on` | `YYYY-MM-DD` | Section 1 of each file |
| `last_review_id` | `REVIEW-<nn>` of the latest review, or blank if none | Section 1 of BRD and Mapping |

### 4.2 Row prefixes

| Prefix | File | Section |
|---|---|---|
| `SCOPE-` | BRD | 3 |
| `CSM-` | BRD | 4 |
| `BR-` | BRD | 5 |
| `DS-` | BRD | 6 |
| `RULE-` | BRD | 7 |
| `ID-` | BRD | 8 |
| `NFR-` | BRD | 9 |
| `AC-` | BRD | 10 |
| `BGAP-` | BRD | 11 |
| `BCONF-` | BRD | 12 |
| `BQ-` | BRD | 13 |
| `SRC-` | Mapping | 2 |
| `TRG-` | Mapping | 3 |
| `FLD-` | Mapping | 4 |
| `XF-` | Mapping | 5 |
| `MGAP-` | Mapping | 6 |
| `MCONF-` | Mapping | 7 |
| `MQ-` | Mapping | 8 |
| `AR-`, `AG-`, `AQ-` | ANALYSIS | Candidate requirements, gaps or conflicts, questions |
| `REVIEW-<nn>-F` | Review | Finding, as `REVIEW-<nn>-F001` |

- A row ID is the prefix plus three digits, starting at `001`, unique within the file. A deleted ID is never reused.
- Reference columns (`req_ids`, `ar_ids`, `consumer_ids`, `rule_ids`, `id_rule_ids`, `xf_ids`, `src_id`, `trg_id`, `blocks`) hold only IDs of rows that exist. Several IDs are comma-separated.
- Templates and filled documents contain no example rows.

## 5. Allowed values

| field | value | Meaning |
|---|---|---|
| `doc_status` | `DRAFT`, `READY_FOR_REVIEW`, `CHANGES_REQUESTED`, `MODEL_BLOCKED`, `WAITING_INPUT`, `APPROVED` | See section 10 |
| `model_status` | `MODEL_DRAFT`, `MODEL_APPROVED`, `MODEL_RETIRED` | See section 11 |
| `evidence_status`, `*_evidence` | `REPO_STATED`, `EVIDENCE_FILE`, `USER_STATED`, `MISSING_EVIDENCE`, `CONFLICT` | See section 6 |
| `name_status`, `trg_name_status` | `EXISTING` | The name already appears in `docs/context/` or `evidence/`. `context_ref` points to where |
| `name_status`, `trg_name_status` | `PROPOSED` | New name. Write `[PROPOSED]` in `note`. Requires `USER_STATED` |
| `priority` | `MUST`, `SHOULD`, `COULD` | Decided by the user |
| `scope` | `IN`, `OUT` | Decided by the user |
| `data_class` | `PII`, `SENSITIVE_PII`, `NON_PERSONAL` | `PII`: identifies or contacts a person. `SENSITIVE_PII`: sensitive personal data under the regulation that applies to the project. `NON_PERSONAL`: cannot be linked to a person |
| `data_type` | `STRING`, `INTEGER`, `DECIMAL(p,s)`, `BOOLEAN`, `DATE`, `TIMESTAMP`, `JSON`, `ARRAY<type>` | Logical type, not tied to a technology |
| `is_key` | `true`, `false` | The field is part of the target object's key |
| `resolution_type` | `REPO_READ` | Read more files in the repository |
| `resolution_type` | `DATA_CHECK` | The user runs a read-only check on data and stores the sanitized result in `evidence/`. State pass and fail criteria in words, no code |
| `resolution_type` | `USER_ANSWER` | The user or business owner answers. Requires a question whose `blocks` contains the `gap_id` |
| `resolution_type` | `SOURCE_TEAM` | Waiting for the team that owns the source system. Requires a question whose `blocks` contains the `gap_id` |
| `context_ref` | `<path> §<section>`, `<path>` with a section name, `<q_id>`, `USER_INPUT` | Source of the value. Several sources are comma-separated |

## 6. Evidence rules and marking missing items

### 6.1 Meaning of `evidence_status`

| Value | When |
|---|---|
| `REPO_STATED` | A file under `docs/context/` or `docs/requirements/<doc_key>/inputs/` states the exact value. `context_ref` gives the path and section |
| `EVIDENCE_FILE` | A file under `docs/requirements/<doc_key>/evidence/` states the exact value. `context_ref` gives the file path |
| `USER_STATED` | The user or business owner sets the value. `context_ref` is `USER_INPUT` when stated in the writing session, or the `q_id` of a question with `answer` and `answered_on` filled |
| `MISSING_EVIDENCE` | No valid source, or the source states the value as unconfirmed |
| `CONFLICT` | Two valid sources disagree |

- Two or three of `REPO_STATED`, `EVIDENCE_FILE`, `USER_STATED` may be comma-separated. `MISSING_EVIDENCE` and `CONFLICT` always stand alone.
- `USER_STATED` is valid only for requirements and decisions: requirement statements, scope, priority, new rules, non-functional values, `PROPOSED` names, grain and key of `PROPOSED` objects, data class of a `PROPOSED` field.
- `USER_STATED` is never valid for facts about `EXISTING` objects: field names, grain, keys, data types, uniqueness, null rates, formats, enumerated values, rules or formulas already in use. Those need `REPO_STATED` or `EVIDENCE_FILE`.

### 6.2 When there is no source

1. Leave the value cell blank. Set `MISSING_EVIDENCE` in the row's evidence cell.
2. Add one row to the missing-evidence section of the same file (BRD section 11, Mapping section 6) whose `blocks` contains the ID of the incomplete row.
3. If `resolution_type` is `USER_ANSWER` or `SOURCE_TEAM`, add one question to the open-questions section of the same file whose `blocks` contains the `gap_id`.
4. A source that states a value but says it is unconfirmed: write the value, set `MISSING_EVIDENCE`, and give the reason in `note`.
5. Never guess objects, fields, types, keys, grain, enumerated values, formulas, or rules. Not stated means missing.

### 6.3 When two sources disagree

Do not pick one. Set `CONFLICT`, add one row to the conflicts section and one question to the open-questions section of the same file, with the question's `blocks` containing the `conflict_id`.

### 6.4 When an answer arrives

Fill `answer`, `answered_on`, `answer_ref` on the question row. Update the affected cells under 6.1 and remove their missing or conflict status. Keep the gap or conflict row and write `RESOLVED: <q_id>` in its `note`. Increment `version`.

### 6.5 Empty sections

A section whose "≥1 row" column in sections 7 and 8 is `yes` must have at least one row. Any other section with no content keeps its table header, removes the blank row, and has the line `Không có.` directly below the table.

## 7. Required BRD sections

Every section below must be present, with the exact heading, order, columns, and column order.

| Heading (verbatim) | ≥1 row | Columns |
|---|---|---|
| `Cách điền` | — | Fixed text from the template. Not edited |
| `1. Thông tin tài liệu` | yes | `template_id`, `model_version`, `doc_key`, `brd_id`, `mapping_id`, `analysis_version`, `version`, `doc_status`, `owner_role`, `updated_on`, `last_review_id` |
| `2. Bối cảnh và mục tiêu` | yes | Three labelled paragraphs: **Vấn đề nghiệp vụ**, **Mục tiêu**, **Nguồn** (`context_ref`) |
| `3. Phạm vi` | yes | `scope_id`, `scope`, `description`, `evidence_status`, `context_ref`, `note` |
| `4. Bên sử dụng dữ liệu` | yes | `consumer_id`, `consumer`, `use_case`, `delivery_need`, `evidence_status`, `context_ref`, `note` |
| `5. Yêu cầu nghiệp vụ` | yes | `req_id`, `ar_ids`, `priority`, `statement`, `consumer_ids`, `trg_object`, `trg_name_status`, `grain`, `grain_evidence`, `key_fields`, `key_evidence`, `rule_ids`, `id_rule_ids`, `evidence_status`, `context_ref`, `note` |
| `6. Nguồn dữ liệu` | yes | `ds_id`, `req_ids`, `src_system`, `src_object`, `src_fields`, `name_status`, `evidence_status`, `context_ref`, `note` |
| `7. Rule nghiệp vụ` | no | `rule_id`, `statement`, `applies_to`, `evidence_status`, `context_ref`, `note` |
| `8. Định danh và khớp bản ghi` | no | `id_rule_id`, `identifier`, `definition`, `applies_to`, `evidence_status`, `context_ref`, `note` |
| `9. Yêu cầu phi chức năng` | no | `nfr_id`, `item`, `value`, `evidence_status`, `context_ref`, `note` |
| `10. Tiêu chí nghiệm thu` | yes | `ac_id`, `req_ids`, `criterion`, `how_to_check`, `evidence_status`, `context_ref`, `note` |
| `11. Mục thiếu evidence` | no | `gap_id`, `blocks`, `missing_fact`, `why_unanswered`, `resolution_type`, `pass_fail_criterion`, `note` |
| `12. Mâu thuẫn` | no | `conflict_id`, `blocks`, `item`, `source_a`, `source_b`, `note` |
| `13. Câu hỏi mở` | no | `q_id`, `blocks`, `question`, `asked_to_role`, `resolution_type`, `answer`, `answered_on`, `answer_ref` |
| `14. Lịch sử phiên bản` | yes | `version`, `updated_on`, `doc_status_after`, `changed_by_role`, `change_summary`, `finding_ids` |

BRD-specific rules:

- `statement` in section 5 is one verifiable requirement, in the user's terms, without describing how it is built.
- `grain` answers "one record of the target object corresponds to what". `key_fields` are the fields that make each record unique. If unknown, leave blank, set `MISSING_EVIDENCE`, and add a row in section 11.
- `how_to_check` in section 10 describes the check in words. No code.
- Section 14 has one row per `version`. `finding_ids` lists the findings of the previous review that this version addresses.

## 8. Required Mapping sections

| Heading (verbatim) | ≥1 row | Columns |
|---|---|---|
| `Cách điền` | — | Fixed text from the template. Not edited |
| `1. Thông tin tài liệu` | yes | `template_id`, `model_version`, `doc_key`, `mapping_id`, `brd_id`, `analysis_version`, `version`, `doc_status`, `owner_role`, `updated_on`, `last_review_id` |
| `2. Đối tượng nguồn` | yes | `src_id`, `src_system`, `src_object`, `grain`, `grain_evidence`, `key_fields`, `key_evidence`, `name_status`, `context_ref`, `note` |
| `3. Đối tượng đích` | yes | `trg_id`, `trg_object`, `grain`, `grain_evidence`, `key_fields`, `key_evidence`, `name_status`, `context_ref`, `note` |
| `4. Mapping trường` | yes | `fld_id`, `req_ids`, `src_id`, `src_field`, `trg_id`, `trg_field`, `data_type`, `data_class`, `is_key`, `xf_ids`, `rule_ids`, `evidence_status`, `context_ref`, `note` |
| `5. Biến đổi` | no | `xf_id`, `description`, `inputs`, `output`, `evidence_status`, `context_ref`, `note` |
| `6. Mục thiếu evidence` | no | `gap_id`, `blocks`, `missing_fact`, `why_unanswered`, `resolution_type`, `pass_fail_criterion`, `note` |
| `7. Mâu thuẫn` | no | `conflict_id`, `blocks`, `item`, `source_a`, `source_b`, `note` |
| `8. Câu hỏi mở` | no | `q_id`, `blocks`, `question`, `asked_to_role`, `resolution_type`, `answer`, `answered_on`, `answer_ref` |
| `9. Lịch sử phiên bản` | yes | `version`, `updated_on`, `doc_status_after`, `changed_by_role`, `change_summary`, `finding_ids` |

Mapping-specific rules:

- Section 4: one row = one target field. Required cells: `req_ids`, `trg_id`, `trg_field`, `data_type`, `data_class`, `is_key`.
- `src_id` and `src_field` are required except for a derived field: a value produced by `xf_ids` or `rule_ids` that is not taken directly from one source field. Then `src_field` is blank, `xf_ids` or `rule_ids` is not blank, and `note` says `DERIVED`.
- `data_class` of an `EXISTING` field needs `REPO_STATED` or `EVIDENCE_FILE`. `data_class` of a `PROPOSED` field may be `USER_STATED`.
- Section 5 describes each transformation in words: inputs, output, logic. No code. A transformation that involves a business choice must become a rule in BRD section 7.
- `evidence_status` of a section 4 row: when every required cell has a source, record the sources. When a required cell has no source, leave it blank, set `MISSING_EVIDENCE`, name the cell in `note`, and add a row in section 6.

## 9. Link keys between the two documents

| Code | Rule |
|---|---|
| `L-01` | BRD and Mapping have the same `doc_key`. Both state `brd_id` = `BRD-<doc_key>` and `mapping_id` = `MAP-<doc_key>` |
| `L-02` | BRD, Mapping, and ANALYSIS have the same `model_version`, and that model has `model_status` `MODEL_APPROVED` |
| `L-03` | BRD and Mapping have the same `analysis_version`, equal to the current `analysis_version` of `ANALYSIS.md` |
| `L-04` | BRD and Mapping have the same `doc_status` and the same `last_review_id` |
| `L-05` | Mapping section 4 `req_ids` point to BRD section 5. `rule_ids` point to BRD section 7 |
| `L-06` | For each `trg_object` in BRD section 5 there is exactly one Mapping section 3 row with the same `trg_object`, and `grain` and `key_fields` are equal |
| `L-07` | For each requirement, the set of (`src_system`, `src_object`) in BRD section 6 rows whose `req_ids` contain it equals the set of (`src_system`, `src_object`) in Mapping section 2 for the `src_id` values used by Mapping section 4 rows whose `req_ids` contain it |
| `L-08` | Each requirement in BRD section 5 has at least one Mapping section 4 row and at least one BRD section 10 row |
| `L-09` | `key_fields` of each Mapping section 3 row equals the set of `trg_field` with `is_key` = `true` for that `trg_id` in Mapping section 4 |
| `L-10` | Each `AR-` with `scope` `IN` in ANALYSIS appears in `ar_ids` of at least one BRD section 5 row. Each `ar_ids` value in the BRD points to an existing `AR-` |
| `L-11` | `blocks` of gaps and conflicts point to rows in the same file. `blocks` of questions point to gaps or conflicts in the same file |

When the two files disagree, Composer fixes them before setting `READY_FOR_REVIEW`. Codex reports every disagreement as a finding.

## 10. Document status and who may write or review

The BRD and the Mapping of one `doc_key` always share one `doc_status` (`L-04`).

| `doc_status` | Composer may write BRD and Mapping | Codex may review | Entered when | Set by |
|---|---|---|---|---|
| `DRAFT` | Yes. Writes content from ANALYSIS and the model | No. Returns `NOT_REVIEWABLE` | Created from templates when ANALYSIS is `READY_TO_WRITE` and the model is `MODEL_APPROVED`; or reopened under 10.2 | Composer |
| `READY_FOR_REVIEW` | No, except storing the review verbatim and changing `doc_status` under 12.3 | Yes | Composer finished and the self-check in `docs/agents/composer-writer.md` passes | Composer |
| `CHANGES_REQUESTED` | Yes, only to address findings with `owner` `WRITER` in the latest review | No | Verdict `CHANGES_REQUESTED` | Composer |
| `MODEL_BLOCKED` | No | No | Verdict `MODEL_ISSUE` | Composer |
| `WAITING_INPUT` | Only `answer`, `answered_on`, `answer_ref` when the user answers | No | Verdict `PASS_WITH_OPEN_GAPS` | Composer |
| `APPROVED` | No | No | Verdict `PASS` | Composer |

### 10.1 Transitions

| From | To | Condition |
|---|---|---|
| `DRAFT` | `READY_FOR_REVIEW` | Self-check passes. The version-history section has a row for the current `version` |
| `READY_FOR_REVIEW` | per verdict | Review stored at `reviews/REVIEW-<nn>.md`. Status follows 12.3. `last_review_id` = `REVIEW-<nn>` |
| `CHANGES_REQUESTED` | `READY_FOR_REVIEW` | Every `WRITER` finding at `BLOCKER` or `MAJOR` is addressed in a new `version`. `finding_ids` in the version history lists them |
| `MODEL_BLOCKED` | `CHANGES_REQUESTED` or `READY_FOR_REVIEW` | `docs/model/MODEL_CHANGELOG.md` has an Opus decision naming the `doc_key`, the `MODEL` findings handled, the `model_version` the documents must use, and the next status |
| `WAITING_INPUT` | `DRAFT` | The user answered at least one open question. `version` + 1 |
| `APPROVED` | `DRAFT` | The user asked for a change and Opus incremented `analysis_version`. `version` + 1 |

### 10.2 Reopening an approved document

Composer never reopens an `APPROVED` document on its own. It reopens only when the user asks for a change and ANALYSIS has a new `analysis_version`.

## 11. Model status, template and architecture changes

The "document model" is `docs/model/DOCUMENT_MODEL.md`, `docs/model/MODEL_CHANGELOG.md`, `templates/BRD_TEMPLATE.md`, `templates/MAPPING_TEMPLATE.md`.
The "architecture" is the document model plus the role files in `docs/agents/`, `AGENTS.md`, `README.md`, and the folder layout in section 3.

| `model_status` | Meaning | Composer may write documents against it | Opus may edit it |
|---|---|---|---|
| `MODEL_DRAFT` | Opus is designing it | No | Yes |
| `MODEL_APPROVED` | Approved by the user | Yes | No. A change requires a new `model_version` |
| `MODEL_RETIRED` | Replaced. Documents pinned to it stay until Opus decides to move them | No, for new documents | No |

- Every template or architecture change the user asks for is handled by Opus (`docs/agents/opus-analyst.md`). Composer and Codex never edit the document model or the architecture, in any `doc_status`.
- Opus sets `MODEL_APPROVED` only after the user explicitly approves in the session. Opus fills `approved_by_role`, `approved_on`, and adds a row to `MODEL_CHANGELOG.md`.
- Changing an approved model: Opus copies the current version to `docs/model/archive/<model_version>/`, increments `model_version`, sets `MODEL_DRAFT`, edits the model and both templates in the same change, and waits for user approval.
- Composer does not change `model_version` in the BRD or Mapping unless a `MODEL_CHANGELOG.md` row names that `doc_key` and the new `model_version`. While a review has open findings, Composer does not change `model_version`, does not edit templates, and does not remove or rename sections or columns to avoid a finding.

## 12. Review

### 12.1 Finding severity and owner

| field | value | Meaning |
|---|---|---|
| `severity` | `BLOCKER` | The document cannot be used: broken structure, link-key mismatch, a value with no source, forbidden content |
| `severity` | `MAJOR` | A model rule is broken: wrong evidence kind, missing required gap or question, unverifiable requirement, missing acceptance criterion |
| `severity` | `MINOR` | No rule broken, but hard to read or easy to misread. Does not affect the verdict |
| `owner` | `WRITER` | Composer can fix it within the current model |
| `owner` | `MODEL` | The model cannot express the requirement, or two model rules conflict. Only Opus handles it |

### 12.2 Open finding

An open finding is a `BLOCKER` or `MAJOR` finding of a review that no later review has marked `CLOSED`. Only Codex closes findings, in the next review. Composer never closes a finding. Opus resolves `MODEL` findings with a decision in `MODEL_CHANGELOG.md`, and Codex confirms in the next review.

### 12.3 Verdict and `doc_status`

Codex picks the first rule that matches:

| Verdict | When | Next `doc_status` |
|---|---|---|
| `NOT_REVIEWABLE` | A file is missing, `doc_status` is not `READY_FOR_REVIEW`, `doc_key` mismatches, or `model_version` does not exist or is not `MODEL_APPROVED` | Unchanged. Tell the user |
| `MODEL_ISSUE` | An open finding has `owner` `MODEL` | `MODEL_BLOCKED` |
| `CHANGES_REQUESTED` | An open finding has `owner` `WRITER` | `CHANGES_REQUESTED` |
| `PASS_WITH_OPEN_GAPS` | No open finding, but a gap or conflict row is not `RESOLVED`, or a question has no `answer` | `WAITING_INPUT` |
| `PASS` | No open finding and no open gap, conflict, or question | `APPROVED` |

## 13. Write access by path

| Path | Opus | Composer | Codex | User |
|---|---|---|---|---|
| `README.md`, `AGENTS.md`, `docs/agents/*` | Yes, when the user asks for an architecture change | No | No | Yes |
| `docs/model/*`, `templates/*` | Yes, under section 11 | No | No | Approves |
| `docs/context/*` | No | No | No | Yes |
| `docs/requirements/<doc_key>/inputs/*`, `evidence/*` | No | No | No | Yes |
| `docs/requirements/<doc_key>/ANALYSIS.md` | Yes | No | No | Reads |
| `docs/requirements/<doc_key>/BRD.md`, `MAPPING.md` | No | Yes, under section 10 | No | Reads |
| `docs/requirements/<doc_key>/reviews/*` | No | Only verbatim copies of Codex replies | No. Returns content in its reply | May copy verbatim |

## 14. Content forbidden in every document

- Real personal data values: a person's name, phone number, email, ID number, address. Field names are allowed; values are not.
- Secrets, tokens, passwords, connection strings.
- Data extracted from a live environment. Evidence in `evidence/` is limited to structure descriptions, counts, or sanitized check results.
- Name, email, or phone number of the person in charge. Write the role instead.

## 15. Language

| File | Language |
|---|---|
| `templates/*`, filled `BRD.md` and `MAPPING.md` | Vietnamese prose. Field names, column names, status values, IDs in English |
| `ANALYSIS.md` | Vietnamese prose, because the user answers its questions. Identifiers in English |
| `README.md`, `docs/context/README.md`, `docs/requirements/README.md` | Vietnamese (user-facing) |
| `docs/model/*`, `docs/agents/*`, `AGENTS.md` | English |
| `reviews/*` | English, except a Vietnamese summary of at most 10 lines under `## Tóm tắt` right after the `REVIEW_VERDICT:` line |
| Agent replies to the user | Vietnamese. Paths, IDs, and status values in English |
