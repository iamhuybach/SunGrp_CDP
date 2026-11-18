# Mapping nguồn → đích

## Cách điền

> Mẫu `CDP-MAPPING-v1`, theo model `CDP-DOCMODEL-v1` (`docs/model/DOCUMENT_MODEL.md`). Luật nằm ở model. Mẫu chỉ giữ tên mục và tên cột. Không sửa đoạn này.

1. Chỉ Composer sao chép mẫu này thành `docs/requirements/<doc_key>/MAPPING.md`, cùng lúc với BRD của cùng `doc_key`.
2. Giữ nguyên tên mục, thứ tự mục, tên cột và thứ tự cột. Không thêm hay xóa cột.
3. Chỉ ghi giá trị có nguồn hợp lệ theo model mục 6. Chưa có nguồn: để trống, đặt `MISSING_EVIDENCE`, thêm dòng ở mục 6, và thêm câu hỏi ở mục 8 khi cần.
4. Mục 4: 1 dòng = 1 trường đích. Trường dẫn xuất ghi `DERIVED` ở `note` theo model mục 8. Mục 5 mô tả biến đổi bằng lời, không viết mã.
5. Mục không có nội dung và không bắt buộc ≥1 dòng: xóa dòng trống, ghi `Không có.` dưới bảng.
6. Mỗi lần sửa nội dung: tăng `version`, cập nhật `updated_on`, thêm 1 dòng ở mục 9.
7. Không ghi giá trị dữ liệu cá nhân, secret, dữ liệu trích từ môi trường thật, hay tên người phụ trách (model mục 14).

## 1. Thông tin tài liệu

| field | value |
|---|---|
| `template_id` | `CDP-MAPPING-v1` |
| `model_version` | |
| `doc_key` | |
| `mapping_id` | |
| `brd_id` | |
| `analysis_version` | |
| `version` | |
| `doc_status` | |
| `owner_role` | |
| `updated_on` | |
| `last_review_id` | |

## 2. Đối tượng nguồn

| src_id | src_system | src_object | grain | grain_evidence | key_fields | key_evidence | name_status | context_ref | note |
|---|---|---|---|---|---|---|---|---|---|
| `SRC-001` | | | | | | | | | |

## 3. Đối tượng đích

| trg_id | trg_object | grain | grain_evidence | key_fields | key_evidence | name_status | context_ref | note |
|---|---|---|---|---|---|---|---|---|
| `TRG-001` | | | | | | | | |

## 4. Mapping trường

| fld_id | req_ids | src_id | src_field | trg_id | trg_field | data_type | data_class | is_key | xf_ids | rule_ids | evidence_status | context_ref | note |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| `FLD-001` | | | | | | | | | | | | | |

## 5. Biến đổi

| xf_id | description | inputs | output | evidence_status | context_ref | note |
|---|---|---|---|---|---|---|
| `XF-001` | | | | | | |

## 6. Mục thiếu evidence

| gap_id | blocks | missing_fact | why_unanswered | resolution_type | pass_fail_criterion | note |
|---|---|---|---|---|---|---|
| `MGAP-001` | | | | | | |

## 7. Mâu thuẫn

| conflict_id | blocks | item | source_a | source_b | note |
|---|---|---|---|---|---|
| `MCONF-001` | | | | | |

## 8. Câu hỏi mở

| q_id | blocks | question | asked_to_role | resolution_type | answer | answered_on | answer_ref |
|---|---|---|---|---|---|---|---|
| `MQ-001` | | | | | | | |

## 9. Lịch sử phiên bản

| version | updated_on | doc_status_after | changed_by_role | change_summary | finding_ids |
|---|---|---|---|---|---|
| | | | | | |
