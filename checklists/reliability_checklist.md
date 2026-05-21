# Reliability Checklist — FIT4110 Lab 03 — team-core (A6 Core Business)

Điền checklist này trước khi nộp Lab 03.

## 1. Functional tests

- [x] Có test cho endpoint health (`TC01` — GET /health, status=ok).
- [x] Có test happy path cho endpoint chính (`TC02` — POST /events vision, `TC03` — POST /access-events).
- [x] Có kiểm tra status code 2xx (`TC02` 201, `TC03` 201, `TC04` 200, `TC07` 201, `TC08` 200).
- [x] Có kiểm tra field quan trọng trong response (`businessEventId`, `status`, `eventType`, `severity`, `notificationId`).
- [x] Có ít nhất 1 test đọc dữ liệu danh sách hoặc chi tiết (`TC04` GET by ID, `TC08` List notifications).

## 2. Auth tests

- [x] Có test thiếu token (`TC09` — POST /events không có Authorization header).
- [x] Có test sai token hoặc token rỗng (`TC10` — POST /events với token "invalid-token-xyz").
- [x] Endpoint public được khai báo rõ nếu không cần auth (GET /health có `security: []` trong contract).
- [x] Test thể hiện đúng expected status 401/403 (`TC09`, `TC10` expect [401, 403]).

## 3. Negative tests

- [x] Có test thiếu field bắt buộc (`TC11` — POST /events thiếu `eventId`).
- [x] Có test sai kiểu dữ liệu / giá trị vi phạm schema (`TC12` — objects=[]).
- [x] Có test sai enum hoặc giá trị ngoài miền (`TC13` — decision="INVALID_DECISION").
- [x] Lỗi trả về theo cùng một error model (`ProblemDetails` — có `status`, `title` trong tất cả lỗi).

## 4. Boundary tests

- [x] Có test min/max hoặc dữ liệu sát ngưỡng (`TC15` — title 200 chars, `TC16` — confidence=1.0).
- [x] Có test limit/pagination nếu endpoint có danh sách (`TC17` — GET /notifications?limit=1).
- [x] Có ghi chú kỳ vọng xử lý dữ liệu biên (TC15: maxLength 200 → 201 accepted hoặc 400 rejected cleanly).
- [x] Test boundary kiểm tra response từ server, không chỉ request body.

## 5. Reliability tests cơ bản

- [x] Có kiểm tra response time (`TC19` — Local only, responseTime < 1000ms).
- [x] Có mô tả timeout mong muốn (< 1000ms cho health endpoint trên local).
- [x] Có test idempotency / conflict handling (`409 Conflict` được khai báo trong contract cho duplicate eventId).
- [x] Có consumer-side smoke test với ít nhất 1 mock của nhóm khác (`TC18` — gọi AI Vision mock POST /detect).

## 6. Evidence

- [x] Collection export JSON: `postman/collections/team-core.postman_collection.json`
- [x] Environment mock export JSON: `postman/environments/team-core_mock.postman_environment.json`
- [x] Environment local export JSON: `postman/environments/team-core_local.postman_environment.json`
- [ ] Newman report XML/HTML (chạy sau khi có mock server).
- [x] Test-case matrix đã điền: `templates/test-case-matrix.csv` (19 test cases).
- [x] Biên bản handshake đã điền: `templates/consumer-provider-handshake.md`.
