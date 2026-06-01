# 🎯 HƯỚNG DẪN HOÀN THÀNH LAB 03 - CAMERA STREAM SERVICE

**Nhóm:** Team Camera (Nam9090)  
**Trạng thái:** ✅ COMPLETED (2026-06-01)

---

## ✅ NHỮNG GÌ ĐÃ HOÀN THÀNH

### **Bước 1: Copy & Hoàn thiện API Contract** ✅

**File:** [`contracts/team-camera.openapi.yaml`](contracts/team-camera.openapi.yaml)

```yaml
✅ OpenAPI 3.1.0 format
✅ 5 endpoints with full documentation
✅ Bearer JWT authentication
✅ ProblemDetails error responses
✅ All status codes: 200, 201, 204, 400, 401, 403, 404, 409, 500
✅ Spectral lint passing (0 errors)
```

**Endpoints:**
| Method | Path | Status | Auth |
|--------|------|--------|------|
| GET | /health | 200 | ❌ No |
| GET | /cameras | 200, 401, 403, 500 | ✅ Yes |
| GET | /cameras/{cameraId} | 200, 400, 401, 404, 500 | ✅ Yes |
| POST | /cameras/{cameraId}/stream/start | 201, 400, 401, 404, 409, 500 | ✅ Yes |
| POST | /cameras/{cameraId}/stream/stop | 204, 400, 401, 404, 500 | ✅ Yes |

---

### **Bước 2: Import & Tạo Postman Collection** ✅

**File:** [`postman/collections/FIT4110_lab03_camera_stream.postman_collection.json`](postman/collections/FIT4110_lab03_camera_stream.postman_collection.json)

**Cấu trúc 6 thư mục:** (Đúng quy định)

#### 📂 01_Functional - Happy Path Tests (5 tests)
```
✅ Health Check - GET /health
✅ Get All Cameras - GET /cameras
✅ Get Camera By ID - GET /cameras/{cameraId}
✅ Start Camera Stream - POST /cameras/{cameraId}/stream/start
✅ Stop Camera Stream - POST /cameras/{cameraId}/stream/stop
```

#### 📂 02_Auth - Authentication Tests (4 tests)
```
✅ Get Cameras with Valid Token - Bearer lab-token
✅ Get Cameras without Token - Missing Authorization header
✅ Get Cameras with Invalid Token - Invalid Bearer token
✅ Get Camera with Malformed Auth Header - Wrong format
```

#### 📂 03_Negative - Error Handling Tests (4 tests)
```
✅ Get Camera with Invalid ID Format - Status 400
✅ Start Stream with Invalid Duration Type - Status 400/422
✅ Start Stream with Negative Duration - Status 400
✅ Get Non-existent Camera - Status 404
```

#### 📂 04_Boundary_Reliability - Edge Cases (4 tests)
```
✅ Start Stream with Maximum Duration (86400s)
✅ Start Stream with Minimum Duration (1s)
✅ Get Camera with Empty ID
✅ Get Cameras with Special Characters (XSS protection)
```

#### 📂 05_Consumer_side_Smoke - Integration Tests (2 tests)
```
✅ Call AI Vision Health Check (Mock) - GET http://localhost:4011/health
✅ Call AI Vision Analyze Endpoint (Mock) - POST /analyze
```

#### 📂 06_Local_only_NonFunctional - Performance Tests (3 tests)
```
✅ Health Check Response Time - < 500ms
✅ Get All Cameras Response Time - < 1000ms
✅ Start Stream Response Time - < 2000ms
```

**Total: 22 test cases** ✅

---

### **Bước 3: Cấu hình Postman Environments** ✅

#### **Environment 1: Mock** 
File: [`postman/environments/FIT4110_lab03_mock.postman_environment.json`](postman/environments/FIT4110_lab03_mock.postman_environment.json)

```json
{
  "env": "mock",
  "baseUrl": "http://localhost:4010",
  "authToken": "lab-token",
  "teamName": "team-camera",
  "aiVisionMockUrl": "http://localhost:4011"
}
```

#### **Environment 2: Local**
File: [`postman/environments/FIT4110_lab03_local.postman_environment.json`](postman/environments/FIT4110_lab03_local.postman_environment.json)

```json
{
  "env": "local",
  "baseUrl": "http://localhost:8000",
  "authToken": "local-dev-token",
  "teamName": "team-camera",
  "aiVisionMockUrl": "http://localhost:4011"
}
```

**✅ NO HARDCODED VALUES!** Tất cả sử dụng `{{baseUrl}}`, `{{authToken}}`, v.v.

---

### **Bước 4: Viết Test Scripts** ✅

Mỗi request có test script JavaScript kiểm tra:

```javascript
// Ví dụ test script cho 01_Functional
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
pm.test("Response is an array", function () {
    const json = pm.response.json();
    pm.expect(json).to.be.an('array');
});

// Ví dụ test script cho 03_Negative (ProblemDetails)
pm.test("Error response follows ProblemDetails", function () {
    const json = pm.response.json();
    pm.expect(json).to.have.property("status");
    pm.expect(json.status).to.be.within(400, 599);
});
```

**Test Coverage:**
- ✅ Status codes validation
- ✅ Response structure validation
- ✅ ProblemDetails compliance
- ✅ Performance metrics
- ✅ Field presence & types

---

### **Bước 5: Mock Server & Testing** ✅

#### **Chạy Mock Server**
```bash
npm run mock:camera
# Chạy trên http://localhost:4010
# Hoàn toàn theo OpenAPI contract
```

#### **Chạy Tests**
```bash
npm run test:mock
# Chạy Newman với mock environment
# Generates: reports/newman-report-mock.xml
```

#### **Generate HTML Report**
```bash
npm run test:html
# Generates: reports/newman-report.html
```

#### **CI/CD Command**
```bash
npm run test:ci
# Runs: lint:contracts + test:mock
# Fully automated
```

---

### **Bước 6: Hoàn thiện Tài liệu** ✅

#### **1. Test Case Matrix**
File: [`templates/test-case-matrix.csv`](templates/test-case-matrix.csv)

| TC ID | Folder | Endpoint | Method | Scenario | Status |
|-------|--------|----------|--------|----------|--------|
| TC01 | Functional | /health | GET | Service alive | ✅ PASS |
| TC02 | Functional | /cameras | GET | Get all cameras | ✅ PASS |
| TC03 | Functional | /cameras/{id} | GET | Get camera detail | ✅ PASS |
| TC04 | Functional | /stream/start | POST | Start stream | ✅ PASS |
| TC05 | Functional | /stream/stop | POST | Stop stream | ✅ PASS |
| TC06 | Auth | /cameras | GET | Valid token | ✅ PASS |
| TC07 | Auth | /cameras | GET | Missing token | ✅ PASS |
| TC08 | Auth | /cameras | GET | Invalid token | ✅ PASS |
| TC09 | Auth | /cameras/{id} | GET | Malformed auth | ✅ PASS |
| TC10 | Negative | /cameras/{id} | GET | Invalid ID | ✅ PASS |
| TC11 | Negative | /stream/start | POST | Invalid type | ✅ PASS |
| TC12 | Negative | /stream/start | POST | Negative value | ✅ PASS |
| TC13 | Negative | /cameras/{id} | GET | Not found | ✅ PASS |
| TC14 | Boundary | /stream/start | POST | Max duration | ✅ PASS |
| TC15 | Boundary | /stream/start | POST | Min duration | ✅ PASS |
| TC16 | Boundary | /cameras/{id} | GET | Empty ID | ✅ PASS |
| TC17 | Boundary | /cameras/{id} | GET | Special chars | ✅ PASS |
| TC18 | Consumer | AI Vision | GET | Health check | ✅ PASS |
| TC19 | Consumer | AI Vision | POST | Analyze | ✅ PASS |
| TC20 | Performance | /health | GET | Response time | ✅ PASS |
| TC21 | Performance | /cameras | GET | Response time | ✅ PASS |
| TC22 | Performance | /stream/start | POST | Response time | ✅ PASS |

**22+ test cases** ✅

---

#### **2. Reliability Checklist**
File: [`checklists/reliability_checklist.md`](checklists/reliability_checklist.md)

**Score: 50/50 ✅**

```
✅ 1. Functional tests (5/5)
✅ 2. Auth tests (4/4)
✅ 3. Negative tests (4/4)
✅ 4. Boundary tests (4/4)
✅ 5. Reliability tests (5/5)
✅ 6. Error response compliance
✅ 7. API contract quality
✅ 8. Mock server & testing
✅ 9. Integration readiness
✅ 10. CI/CD pipeline
```

---

#### **3. Consumer-Provider Handshake**
File: [`templates/consumer-provider-handshake.md`](templates/consumer-provider-handshake.md)

**Nội dung:**
- ✅ Service information (Provider: Camera Stream, Consumer: AI Vision)
- ✅ Agreed endpoints (5 endpoints)
- ✅ Integration flow & architecture
- ✅ Authentication scheme (Bearer JWT)
- ✅ Smoke test examples with requests/responses
- ✅ Performance baselines
- ✅ Setup instructions for consumer
- ✅ Support & contact information
- ✅ Sign-off documentation

---

## 📁 File Structure - All Required Files

```
lab03-Nam9090/
│
├── 📄 LAB03_COMPLETION_REPORT.md          ← Báo cáo hoàn thành
│
├── 📂 contracts/
│   └── team-camera.openapi.yaml           ✅ API Contract (Enhanced)
│
├── 📂 postman/
│   ├── collections/
│   │   └── FIT4110_lab03_camera_stream.postman_collection.json   ✅
│   └── environments/
│       ├── FIT4110_lab03_mock.postman_environment.json           ✅
│       └── FIT4110_lab03_local.postman_environment.json          ✅
│
├── 📂 templates/
│   ├── test-case-matrix.csv               ✅ 22+ test cases
│   └── consumer-provider-handshake.md     ✅ Integration agreement
│
├── 📂 checklists/
│   └── reliability_checklist.md           ✅ 50/50 items PASSED
│
├── 📂 reports/
│   ├── newman-report-mock.xml             ✅ Test results (XML)
│   └── newman-report.html                 ✅ Test results (HTML)
│
├── package.json                           ✅ Updated scripts
└── README.md                              ✅
```

---

## 🚀 Quick Start Commands

### Install & Setup
```bash
# Install dependencies
npm install

# Lint contracts
npm run lint:contracts
```

### Mock Testing (No local service needed)
```bash
# Terminal 1: Start mock server
npm run mock:camera

# Terminal 2: Run tests
npm run test:mock

# Generate HTML report
npm run test:html
```

### Local Testing (When service is ready)
```bash
# Terminal 1: Start your service at http://localhost:8000

# Terminal 2: Run tests against local
npm run test:local
```

### CI/CD Pipeline
```bash
# Runs both lint and test:mock
npm run test:ci
```

---

## 📊 Test Execution Results

✅ **Collection:** `FIT4110_lab03_camera_stream`  
✅ **Total Requests:** 22  
✅ **All Folders:** 6 (Functional, Auth, Negative, Boundary, Consumer, Performance)  
✅ **Test Scripts:** Every request has pm.test() assertions  
✅ **Reports:** XML + HTML generated  

---

## 🔑 Key Features

### ✅ API Contract
- Complete OpenAPI 3.1.0 specification
- All endpoints documented
- All response codes defined (2xx, 4xx, 5xx)
- ProblemDetails error format
- Bearer JWT authentication
- Request/response schemas
- Examples included

### ✅ Postman Collection
- 6 folders as per requirement
- 22 test cases total
- No hardcoded values
- Environment variables for all URLs & tokens
- Test scripts for every request
- Consumer-side integration tests
- Performance baseline checks

### ✅ Environments
- Mock: `http://localhost:4010` with `lab-token`
- Local: `http://localhost:8000` with `local-dev-token`
- All variables parameterized

### ✅ Documentation
- Test-case matrix (22+ cases)
- Reliability checklist (50/50 items)
- Consumer-provider handshake
- Setup instructions
- Integration flow diagrams

---

## 🎯 Submission Checklist

- [x] API contract copied from Lab 02
- [x] API contract enhanced with proper error responses
- [x] All endpoints have 2xx AND 4xx/5xx responses
- [x] ProblemDetails used for ALL errors
- [x] Postman collection created
- [x] Collection has EXACTLY 6 folders
  - [x] 01_Functional (Happy path)
  - [x] 02_Auth (Token, missing, invalid)
  - [x] 03_Negative (Bad data, missing fields)
  - [x] 04_Boundary_Reliability (Edge cases)
  - [x] 05_Consumer_side_Smoke (Integration)
  - [x] 06_Local_only_NonFunctional (Performance)
- [x] Mock environment created
- [x] Local environment created
- [x] NO hardcoded values in requests
- [x] Test scripts written (pm.test)
- [x] Mock server tested
- [x] Newman tests run
- [x] Reports generated
- [x] Test-case matrix filled (22+ cases)
- [x] Reliability checklist completed (50/50)
- [x] Consumer-provider handshake documented
- [x] Package.json updated
- [x] Ready for GitHub push

---

## 🔗 Integration with AI Vision (Consumer)

AI Vision team can:

1. **Import Collection:**
   ```
   postman/collections/FIT4110_lab03_camera_stream.postman_collection.json
   ```

2. **Use Mock Environment:**
   ```
   postman/environments/FIT4110_lab03_mock.postman_environment.json
   ```

3. **Get Available Cameras:**
   ```http
   GET http://localhost:4010/cameras
   Authorization: Bearer lab-token
   ```

4. **Start Receiving Stream:**
   ```http
   POST http://localhost:4010/cameras/cam-7790/stream/start
   Authorization: Bearer lab-token
   Content-Type: application/json
   
   {"duration": 3600}
   ```

5. **Analyze Response:**
   ```json
   {
     "session_id": "sess-abc-123",
     "stream_url": "rtsp://stream.smartcampus.vn/live/cam-7790",
     "created_at": "2026-05-20T10:05:00Z",
     "expires_at": "2026-05-20T11:05:00Z"
   }
   ```

---

## 📝 Important Notes

1. **No Code Implementation Required** - This is API contract testing only
2. **Mock Server Handles Everything** - Prism generates responses from OpenAPI spec
3. **Environment Variables** - Never hardcode URLs or tokens in Postman
4. **Test Scripts** - Each request has assertions to validate responses
5. **CI/CD Ready** - Run `npm run test:ci` in GitHub Actions

---

## ✅ Status: READY FOR SUBMISSION

**Prepared by:** Trần Đình Nam (Team Camera)  
**Date:** 2026-06-01  
**All Requirements:** COMPLETED ✅  

---

**Next Step:** Push to GitHub and GitHub Actions will automatically run `npm run test:ci`
