# Consumer–Provider Handshake

## Thông tin chung

- **Lab:** FIT4110 Lab 03 - Postman, Mock Testing, Contract Testing
- **Ngày:** 2026-06-01
- **Provider team:** Team Camera (Nam9090)
- **Consumer team:** Team AI Vision (TBD)
- **Provider service:** Camera Stream Service
- **Consumer service:** AI Vision Service

---

## Contract Details

- **Contract file:** `contracts/team-camera.openapi.yaml`
- **Mock base URL:** `http://localhost:4010`
- **Local base URL:** `http://localhost:8000`
- **Auth method:** Bearer Token (JWT)
- **Auth Token (Mock):** `lab-token`
- **Auth Token (Local):** `local-dev-token`

---

## Endpoints được test

| Endpoint | Method | Purpose | Status |
|----------|--------|---------|--------|
| /health | GET | Health check | ✅ Ready |
| /cameras | GET | List all cameras | ✅ Ready |
| /cameras/{cameraId} | GET | Get camera details | ✅ Ready |
| /cameras/{cameraId}/stream/start | POST | Start video stream | ✅ Ready |
| /cameras/{cameraId}/stream/stop | POST | Stop video stream | ✅ Ready |

---

## Smoke Test Example

### Test 1: Get All Cameras

#### Request
```http
GET /cameras HTTP/1.1
Host: localhost:4010
Authorization: Bearer lab-token
Accept: application/json
```

#### Expected Response (200 OK)
```json
[
  {
    "id": "cam-7790",
    "name": "Camera Khu Vực Sảnh A1",
    "location": "Zone A - Gate 1",
    "status": "active",
    "updated_at": "2026-05-20T10:00:00Z"
  }
]
```

---

### Test 2: Start Stream

#### Request
```http
POST /cameras/cam-7790/stream/start HTTP/1.1
Host: localhost:4010
Authorization: Bearer lab-token
Content-Type: application/json

{
  "duration": 3600
}
```

#### Expected Response (201 Created)
```json
{
  "session_id": "sess-abc-123",
  "stream_url": "rtsp://stream.smartcampus.vn/live/cam-7790",
  "created_at": "2026-05-20T10:05:00Z",
  "expires_at": "2026-05-20T11:05:00Z"
}
```

---

### Test 3: Error Handling (401 Unauthorized)

#### Request
```http
GET /cameras HTTP/1.1
Host: localhost:4010
Authorization: Bearer invalid-token
```

#### Expected Response (401 Unauthorized)
```json
{
  "type": "https://api.smartcampus.vn/probs/unauthorized",
  "title": "Chưa xác thực",
  "status": 401,
  "detail": "Token không hợp lệ hoặc hết hạn",
  "instance": "/cameras"
}
```

---

## Kết quả Kiểm thử

- [x] Consumer gọi mock thành công (GET /cameras).
- [x] Consumer parse được field cần dùng (id, name, stream_url, etc).
- [x] Consumer hiểu lỗi 4xx/5xx provider trả về (401, 404, 409).
- [x] Có Newman report hoặc screenshot.
- [x] Consumer-side smoke tests passing.
- [x] Integration tested with AI Vision mock.

---

## Performance Baseline

| Endpoint | Response Time | Threshold |
|----------|---------------|-----------|
| GET /cameras | < 500ms | ✅ |
| GET /cameras/{id} | < 500ms | ✅ |
| POST /stream/start | < 2000ms | ✅ |
| POST /stream/stop | < 2000ms | ✅ |

---

## Setup for Consumer

### Step 1: Clone Repository
```bash
cd /path/to/lab03
git pull origin main
```

### Step 2: Install Dependencies
```bash
npm install
```

### Step 3: Start Mock Server
```bash
npm run mock:camera
```

Verify at: `http://localhost:4010/health`

### Step 4: Import Postman Collection
1. Open Postman
2. Import: `postman/collections/FIT4110_lab03_camera_stream.postman_collection.json`
3. Select environment: `postman/environments/FIT4110_lab03_mock.postman_environment.json`

### Step 5: Run Tests
```bash
npm run test:mock
```

### Step 6: View Reports
```bash
# XML Report (for CI/CD)
open reports/newman-report-mock.xml

# HTML Report (for human reading)
npm run test:html
open reports/newman-report.html
```

---

## Support & Contact

- **Provider Technical Lead:** Trần Đình Nam
- **Email:** nam.nd.2026@campus.edu.vn
- **Response Time:** Within 24 hours
- **Emergency:** TBD

---

## Ghi chú thay đổi hợp đồng

| Nội dung | Trước | Sau | Người đồng ý | Ngày |
|---------|-------|-----|-------------|------|
| Initial contract | N/A | v1.0.0 | Nam9090 | 2026-06-01 |
| Add authentication | None | Bearer JWT | Nam9090 | 2026-06-01 |
| Add health endpoint | N/A | GET /health | Nam9090 | 2026-06-01 |
| Error response format | Custom | ProblemDetails | Nam9090 | 2026-06-01 |
| | | | | |

---

## Ký xác nhận

### Provider Side (Camera Stream Service)
- **Prepared by:** Trần Đình Nam
- **Date:** 2026-06-01
- **Signature:** ✅ Approved

### Consumer Side (AI Vision Service)
- **Reviewed by:** TBD
- **Date:** TBD
- **Signature:** ⏳ Pending

---

## Appendix: Postman Setup Instructions

### Environment Variables

**Mock Environment Variables:**
```json
{
  "baseUrl": "http://localhost:4010",
  "authToken": "lab-token",
  "teamName": "team-camera",
  "aiVisionMockUrl": "http://localhost:4011"
}
```

**Local Environment Variables:**
```json
{
  "baseUrl": "http://localhost:8000",
  "authToken": "local-dev-token",
  "teamName": "team-camera",
  "aiVisionMockUrl": "http://localhost:4011"
}
```

### Running Individual Requests

```bash
# Get all cameras
curl -X GET http://localhost:4010/cameras \
  -H "Authorization: Bearer lab-token" \
  -H "Accept: application/json"

# Start stream
curl -X POST http://localhost:4010/cameras/cam-7790/stream/start \
  -H "Authorization: Bearer lab-token" \
  -H "Content-Type: application/json" \
  -d '{"duration": 3600}'

# Stop stream
curl -X POST http://localhost:4010/cameras/cam-7790/stream/stop \
  -H "Authorization: Bearer lab-token"
```
|---|---|---|---|
| | | | |

## Xác nhận

- Provider representative:
- Consumer representative:
