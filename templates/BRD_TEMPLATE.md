# Business Requirements Document (BRD)

## Cách điền

> Mẫu `CDP-BRD-v1`, theo model `CDP-DOCMODEL-v1` (`docs/model/DOCUMENT_MODEL.md`). Luật nằm ở model. Mẫu chỉ giữ tên mục và tên cột. Không sửa đoạn này.

1. Chỉ Composer sao chép mẫu này thành `docs/requirements/<doc_key>/BRD.md`, khi `ANALYSIS.md` là `READY_TO_WRITE` và model là `MODEL_APPROVED`.
2. Giữ nguyên tên mục, thứ tự mục, tên cột và thứ tự cột. Không thêm hay xóa cột.
3. Chỉ ghi giá trị có nguồn hợp lệ theo model mục 6. Chưa có nguồn: để trống, đặt `MISSING_EVIDENCE`, thêm dòng ở mục 11, và thêm câu hỏi ở mục 13 khi cần.
4. Mục không có nội dung và không bắt buộc ≥1 dòng: xóa dòng trống, ghi `Không có.` dưới bảng.
5. Mỗi lần sửa nội dung: tăng `version`, cập nhật `updated_on`, thêm 1 dòng ở mục 14.
6. Không ghi giá trị dữ liệu cá nhân, secret, dữ liệu trích từ môi trường thật, hay tên người phụ trách (model mục 14).

## 1. Thông tin tài liệu

| field | value |
|---|---|
| `template_id` | `CDP-BRD-v1` |
| `model_version` | |
| `doc_key` | |
| `brd_id` | |
| `mapping_id` | |
| `analysis_version` | |
| `version` | |
| `doc_status` | |
| `owner_role` | |
| `updated_on` | |
| `last_review_id` | |

## 2. Bối cảnh và mục tiêu

**Vấn đề nghiệp vụ:**

**Mục tiêu:**

**Nguồn:**

## 3. Phạm vi

| scope_id | scope | description | evidence_status | context_ref | note |
|---|---|---|---|---|---|
| `SCOPE-001` | | | | | |

## 4. Bên sử dụng dữ liệu

| consumer_id | consumer | use_case | delivery_need | evidence_status | context_ref | note |
|---|---|---|---|---|---|---|
| `CSM-001` | | | | | | |

## 5. Yêu cầu nghiệp vụ

| req_id | ar_ids | priority | statement | consumer_ids | trg_object | trg_name_status | grain | grain_evidence | key_fields | key_evidence | rule_ids | id_rule_ids | evidence_status | context_ref | note |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| `BR-001` | | | | | | | | | | | | | | | |

## 6. Nguồn dữ liệu

| ds_id | req_ids | src_system | src_object | src_fields | name_status | evidence_status | context_ref | note |
|---|---|---|---|---|---|---|---|---|
| `DS-001` | | | | | | | | |

## 7. Rule nghiệp vụ

| rule_id | statement | applies_to | evidence_status | context_ref | note |
|---|---|---|---|---|---|
| `RULE-001` | | | | | |

## 8. Định danh và khớp bản ghi

| id_rule_id | identifier | definition | applies_to | evidence_status | context_ref | note |
|---|---|---|---|---|---|---|
| `ID-001` | | | | | | |

## 9. Yêu cầu phi chức năng

| nfr_id | item | value | evidence_status | context_ref | note |
|---|---|---|---|---|---|
| `NFR-001` | | | | | |

## 10. Tiêu chí nghiệm thu

| ac_id | req_ids | criterion | how_to_check | evidence_status | context_ref | note |
|---|---|---|---|---|---|---|
| `AC-001` | | | | | | |

## 11. Mục thiếu evidence

| gap_id | blocks | missing_fact | why_unanswered | resolution_type | pass_fail_criterion | note |
|---|---|---|---|---|---|---|
| `BGAP-001` | | | | | | |

## 12. Mâu thuẫn

| conflict_id | blocks | item | source_a | source_b | note |
|---|---|---|---|---|---|
| `BCONF-001` | | | | | |

## 13. Câu hỏi mở

| q_id | blocks | question | asked_to_role | resolution_type | answer | answered_on | answer_ref |
|---|---|---|---|---|---|---|---|
| `BQ-001` | | | | | | | |

## 14. Lịch sử phiên bản

| version | updated_on | doc_status_after | changed_by_role | change_summary | finding_ids |
|---|---|---|---|---|---|
| | | | | | |
