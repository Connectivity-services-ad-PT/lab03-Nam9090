# Reliability Checklist — FIT4110 Lab 03 - Camera Stream Service

Điền checklist này trước khi nộp Lab 03.

**Service:** Camera Stream Service  
**Team:** Team Camera (Nam9090)  
**Date:** 2026-06-01

## 1. Functional tests

- [x] Có test cho endpoint health (/health).
- [x] Có test happy path cho endpoint chính (/cameras, /stream/start, /stream/stop).
- [x] Có kiểm tra status code 2xx (200, 201, 204).
- [x] Có kiểm tra field quan trọng trong response (id, session_id, stream_url, etc).
- [x] Có ít nhất 1 test đọc dữ liệu danh sách (GET /cameras) hoặc chi tiết (GET /cameras/{id}).

**Status:** ✅ PASS (5/5 test cases)

## 2. Auth tests

- [x] Có test thiếu token (401 Unauthorized).
- [x] Có test sai token hoặc token rỗng (401 Unauthorized).
- [x] Endpoint public được khai báo rõ nếu không cần auth (/health is public).
- [x] Test thể hiện đúng expected status 401/403.
- [x] Bearer token format validated.

**Status:** ✅ PASS (4/4 test cases)

## 3. Negative tests

- [x] Có test thiếu field bắt buộc (invalid request body).
- [x] Có test sai kiểu dữ liệu (string instead of number for duration).
- [x] Có test sai enum hoặc giá trị ngoài miền (negative duration).
- [x] Lỗi trả về theo cùng một error model (ProblemDetails).
- [x] Non-existent resource returns 404.

**Status:** ✅ PASS (4/4 test cases)

## 4. Boundary tests

- [x] Có test min/max duration (1 second to 86400 seconds).
- [x] Có test limit/pagination nếu endpoint có danh sách (N/A - no pagination).
- [x] Có test payload lớn hoặc metadata thiếu.
- [x] Có ghi chú kỳ vọng xử lý dữ liệu biên.
- [x] Special characters and SQL injection attempts handled.

**Status:** ✅ PASS (4/4 test cases)

## 5. Reliability tests cơ bản

- [x] Có kiểm tra response time (< 500ms, < 1000ms, < 2000ms).
- [x] Có mô tả timeout mong muốn và baselines.
- [x] Có consumer-side smoke test với ít nhất 1 mock của nhóm khác (AI Vision).
- [x] Performance metrics documented in test cases.
- [x] Retry/idempotency noted for POST /stream/stop.

**Status:** ✅ PASS (3/3 test cases + 2/2 consumer tests)

## 6. Error Response Compliance

- [x] All 4xx/5xx responses use ProblemDetails format.
- [x] ProblemDetails includes: type, title, status, detail, instance.
- [x] Error codes consistent with HTTP standards.
- [x] Error descriptions helpful for debugging.
- [x] Localization in Vietnamese (if applicable).

**Status:** ✅ PASS

## 7. Evidence

- [x] Collection export JSON: `postman/collections/FIT4110_lab03_camera_stream.postman_collection.json`
- [x] Environment mock export JSON: `postman/environments/FIT4110_lab03_mock.postman_environment.json`
- [x] Environment local export JSON: `postman/environments/FIT4110_lab03_local.postman_environment.json`
- [x] Test-case matrix CSV: `templates/test-case-matrix.csv`
- [x] Consumer-provider handshake: `templates/consumer-provider-handshake.md`
- [x] OpenAPI contract: `contracts/team-camera.openapi.yaml`

**Status:** ✅ ALL REQUIRED ARTIFACTS PRESENT

## 8. API Contract Quality

- [x] All endpoints documented with description.
- [x] All request/response schemas defined.
- [x] All error responses documented (400, 401, 403, 404, 409, 500).
- [x] Authentication scheme properly declared (Bearer).
- [x] Spectral lint rules passing.

**Status:** ✅ PASS

## 9. Mock Server & Testing

- [x] Mock server starts: `npm run mock:camera`
- [x] Mock server health check passes: GET http://localhost:4010/health
- [x] Newman tests run successfully: `npm run test:mock`
- [x] Tests generate XML/HTML reports.
- [x] All test assertions pass.

**Status:** ✅ PASS

## 10. Integration Readiness

- [x] Package.json scripts configured correctly.
- [x] Postman environment variables not hardcoded.
- [x] Consumer-side services reachable (AI Vision mock).
- [x] CI/CD pipeline configuration ready.
- [x] GitHub Actions ready for automated testing.

**Status:** ✅ PASS

---

## Overall Score

**Total Checklist Items:** 50  
**Passed:** 50 ✅  
**Failed:** 0  

**Result: APPROVED FOR SUBMISSION** 🎉

---

## Sign-Off

- **Prepared by:** Trần Đình Nam (Team Camera)
- **Date:** 2026-06-01
- **Status:** ✅ READY FOR PRODUCTION
