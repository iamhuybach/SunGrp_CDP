# SunGrp CDP — BRD và Mapping

Repo này chỉ dùng để phân tích, viết và review hai tài liệu cho mỗi yêu cầu dữ liệu của CDP (nền tảng dữ liệu khách hàng): **BRD** (Business Requirements Document) và **Mapping** nguồn → đích. Repo không chứa mã, thiết kế kỹ thuật hay cấu hình hạ tầng.

## Ai làm gì

| Vai | Công cụ và model | Làm | Ghi | Không bao giờ |
|---|---|---|---|---|
| **Phân tích** | Cursor, Opus 5.5, effort Medium | Phân tích nghiệp vụ, thiết kế và chốt model tài liệu, xử lý mọi thay đổi template hay kiến trúc, xử lý finding về model | `ANALYSIS.md`, `docs/model/*`, `templates/*`; `docs/agents/*`, `AGENTS.md`, `README.md` khi người dùng yêu cầu đổi kiến trúc | Ghi BRD hoặc Mapping |
| **Viết** | Cursor, Composer 2.5, effort mặc định (không bật Fast) | Thao tác repo, viết BRD và Mapping theo model đã duyệt, sửa theo review | `BRD.md`, `MAPPING.md`, chép nguyên văn file review | Đổi model tài liệu, template hay kiến trúc |
| **Review** | Codex CLI, `gpt-5.6-sol`, effort `medium`, sandbox `read-only` | Kiểm BRD và Mapping theo model, trả finding | Không ghi file nào | Sửa câu chữ, ghi BRD hoặc Mapping |

Chi tiết từng vai (tiếng Anh): [docs/agents/opus-analyst.md](docs/agents/opus-analyst.md), [docs/agents/composer-writer.md](docs/agents/composer-writer.md), [docs/agents/codex-reviewer.md](docs/agents/codex-reviewer.md).

Luật của tài liệu (tiếng Anh): [docs/model/DOCUMENT_MODEL.md](docs/model/DOCUMENT_MODEL.md). Mẫu trống (tiếng Việt): [templates/BRD_TEMPLATE.md](templates/BRD_TEMPLATE.md), [templates/MAPPING_TEMPLATE.md](templates/MAPPING_TEMPLATE.md).

## Thứ tự làm việc

```text
 0. Chốt model    Opus rà model ─► người dùng duyệt ─► Opus đặt MODEL_APPROVED
                                     │
 1. Phân tích     người dùng bỏ tài liệu vào inputs/ ─► Opus ghi ANALYSIS.md
                     INPUT_INSUFFICIENT  ─► người dùng trả lời câu hỏi, chạy lại bước 1
                     MODEL_CHANGE_NEEDED ─► quay lại bước 0 với model_version mới
                     READY_TO_WRITE      ─► bước 2
                                     │
 2. Viết          Composer tạo BRD.md + MAPPING.md (DRAFT) ─► tự kiểm ─► READY_FOR_REVIEW
                                     │
 3. Review        Codex review (chỉ đọc) ─► Composer chép nguyên văn vào reviews/REVIEW-<nn>.md
                                     │      và đổi doc_status theo verdict
                     CHANGES_REQUESTED   ─► Composer sửa finding WRITER ─► bước 3
                     MODEL_ISSUE         ─► MODEL_BLOCKED: Opus quyết định ở MODEL_CHANGELOG.md ─► Composer ─► bước 3
                     PASS_WITH_OPEN_GAPS ─► WAITING_INPUT: người dùng trả lời ─► Composer (DRAFT) ─► bước 3
                     PASS                ─► APPROVED
```

### Bước 0. Chốt model (một lần, và mỗi khi đổi model)

1. Mở Cursor, chọn Opus 5.5 effort Medium, đính kèm `@docs/agents/opus-analyst.md`.
2. Yêu cầu Opus rà `docs/model/DOCUMENT_MODEL.md` và hai mẫu. Model hiện là `CDP-DOCMODEL-v1`, trạng thái `MODEL_DRAFT`.
3. Khi đồng ý, nói rõ với Opus là bạn duyệt. Opus đặt `MODEL_APPROVED` và ghi dòng ở `docs/model/MODEL_CHANGELOG.md`.

Composer không viết tài liệu nào khi model chưa `MODEL_APPROVED`.

### Bước 1. Phân tích

1. Chọn `doc_key` dạng `YYYYMMDD-slug`. Bỏ tài liệu nghiệp vụ đã bỏ dữ liệu cá nhân vào `docs/requirements/<doc_key>/inputs/`.
2. Trong Cursor với Opus và `@docs/agents/opus-analyst.md`, đưa `doc_key`.
3. Opus ghi `ANALYSIS.md` bằng tiếng Việt. Nếu không đủ dữ liệu, Opus dừng và chỉ để lại câu hỏi. Opus không đoán.

### Bước 2. Viết

1. Mở chat mới trong Cursor, chọn Composer 2.5 (effort mặc định, tắt Fast), đính kèm `@docs/agents/composer-writer.md`.
2. Đưa `doc_key`. Composer tạo `BRD.md`, `MAPPING.md` từ mẫu, viết theo ANALYSIS, đánh dấu chỗ thiếu bằng `MISSING_EVIDENCE` và câu hỏi.
3. Composer tự kiểm rồi đặt `READY_FOR_REVIEW`.

### Bước 3. Review

1. Chạy Codex ở thư mục gốc repo:

   ```text
   codex -m gpt-5.6-sol -c model_reasoning_effort="medium" --sandbox read-only --ask-for-approval never
   ```

2. Gõ: `Read docs/agents/codex-reviewer.md and review doc_key <doc_key>.`
3. Đưa nguyên câu trả lời của Codex cho Composer để chép vào `reviews/REVIEW-<nn>.md` và đổi `doc_status`.

### Khi muốn đổi template hay kiến trúc

Mọi thay đổi về mẫu BRD, mẫu Mapping, model tài liệu, file vai trò, `AGENTS.md`, `README.md` hay bố cục thư mục đều giao cho Opus (Cursor, Opus 5.5, effort Medium) với `@docs/agents/opus-analyst.md`. Opus ghi quyết định ở `docs/model/MODEL_CHANGELOG.md`. Nếu model đã duyệt, Opus tạo `model_version` mới và chờ bạn duyệt lại. Composer và Codex không sửa những file này.

## Ba luật không được phá

1. **Composer không đổi model tài liệu.** Không sửa `docs/model/` hay `templates/`, không thêm, xóa hay đổi tên mục hoặc cột, không đổi `model_version` khi chưa có quyết định của Opus. Khi review còn finding mở, Composer chỉ sửa câu chữ trong khuôn model hiện tại.
2. **Codex không ghi gì.** Không ghi BRD, Mapping hay bất kỳ file nào, và không viết câu thay thế. Finding chỉ nêu chỗ sai và luật bị vi phạm.
3. **Không ai đoán.** Thiếu nguồn thì đánh dấu `MISSING_EVIDENCE` và hỏi. Hai nguồn nói khác nhau thì đánh dấu `CONFLICT` và hỏi. Quyết định nghiệp vụ thuộc người dùng.

## Giới hạn đã biết

Cursor không tự chặn quyền theo vai. Opus và Composer giữ đúng ranh giới nhờ việc đính kèm đúng file vai trò và nhờ bước review bắt lỗi vượt quyền. Riêng Codex bị sandbox `read-only` chặn ghi ở mức công cụ.

## Ngôn ngữ

| File | Ngôn ngữ |
|---|---|
| `templates/*`, `BRD.md`, `MAPPING.md`, `ANALYSIS.md`, `README.md`, README trong `docs/context/` và `docs/requirements/` | Tiếng Việt |
| `docs/model/*`, `docs/agents/*`, `AGENTS.md` | Tiếng Anh |
| `reviews/*` | Tiếng Anh. Đầu file, ngay sau dòng `REVIEW_VERDICT:`, có mục `## Tóm tắt` bằng tiếng Việt, tối đa 10 dòng |

Tên trường, tên cột, giá trị trạng thái và ID luôn giữ bằng tiếng Anh. Agent trả lời người dùng bằng tiếng Việt.

## Cấu trúc thư mục

```text
README.md                         trang này
AGENTS.md                         chỉ dẫn chung cho mọi agent mở repo
docs/model/DOCUMENT_MODEL.md      model tài liệu: mục bắt buộc, khóa liên kết, trạng thái, quyền
docs/model/MODEL_CHANGELOG.md     quyết định về model và kiến trúc
docs/agents/                      ba file vai trò
docs/context/                     sự kiện dùng chung, người dùng ghi
docs/requirements/<doc_key>/      inputs/, evidence/, ANALYSIS.md, BRD.md, MAPPING.md, reviews/
templates/                        mẫu BRD và Mapping trống
```
