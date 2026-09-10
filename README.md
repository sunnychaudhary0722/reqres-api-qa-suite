# Reqres API — QA Test Suite

A self-directed QA portfolio project: manual and scripted API testing of the public [Reqres](https://reqres.in) mock REST API, covering functional, negative, edge-case, schema, and basic performance testing.

## Why this project

Built to demonstrate practical manual and API testing skills — test planning, test case design, defect-style analysis, and Postman scripting — end to end on a real API.

## Contents

| File | Purpose |
|---|---|
| `QA_Test_Cases_Reqres_API.xlsx` | 20 documented test cases: functional, negative, edge case, performance, and schema validation, with priority and status tracking |
| `Reqres_API_Tests.postman_collection.json` | Importable Postman collection with scripted `pm.test` assertions covering the high-priority test 
## Test Plan

### 1. Objective
Verify the functional correctness, error handling, and basic performance characteristics of the Reqres REST API across its core resources: Users (CRUD) and Auth (register/login).

### 2. Scope

**In scope:**
- `GET /api/users` (list, pagination, single-user lookup)
- `POST /api/users`, `PUT /api/users/:id`, `DELETE /api/users/:id`
- `POST /api/register`, `POST /api/login`
- Response schema validation
- Basic performance check on the delayed-response endpoint
- Response header validation

**Out of scope:**
- Load/stress testing beyond a single delayed request
- Security/penetration testing
- UI testing (Reqres has no UI to test)
- Authentication token usage in subsequent authenticated calls (Reqres does not require token reuse)

### 3. Test Approach
- **Manual testing** first, to explore actual API behavior against documented expectations.
- **Scripted verification** via Postman `pm.test` assertions for repeatability, covering status codes, response body structure, and key field values.
- **Negative and edge-case testing** deliberately included (missing fields, invalid IDs, out-of-range pagination) since these are the cases most likely to reveal real defects or undocumented behavior.
- Test data is either fixed (Reqres's documented mock users/credentials) or intentionally invalid, per test case.

### 4. Environment
- Base URL: `https://reqres.in/api`
- Tooling: Postman (manual runs + collection runner)
- **Authentication:** None required. Per Reqres's own published docs (`reqres.in/llm.txt`), the demo endpoints used in this suite (`/api/users`, `/api/login`, `/api/register`, `/api/unknown`) are explicitly no-auth. Reqres also offers a separate, authenticated "Project API" (`/api/collections/*`) for persistent data — that surface is out of scope here and not exercised by this collection.
- **Rate limits (per Reqres's published docs):** `/api/users` — 20 requests/minute per IP; `/api/*` generally — 100 requests/minute per IP. See Finding 1 below for a discrepancy observed against these published figures.

### 5. Entry / Exit Criteria
- **Entry:** API endpoints are reachable and documented behavior is known from public docs.
- **Exit:** All 20 test cases executed and marked Pass/Fail in the tracker; any unexpected behavior logged as a finding below.

### 6. Deliverables
- Test case tracker (`.xlsx`)
- Postman collection (`.json`) — importable and runnable via Postman or Newman (CLI runner)
- This test plan and summary of findings


## How to run

1. Import `Reqres_API_Tests.postman_collection.json` into Postman — no signup or API key needed, since these are all no-auth demo endpoints.
2. Run the collection (Collection Runner or individually per request) — each request has assertions attached under its Tests tab.
3. Cross-reference results against `QA_Test_Cases_Reqres_API.xlsx` and update the Status/Actual Result columns.
4. If you get rate-limited mid-run, wait a few minutes (the published limit is per-minute, not per-day) and resume.
5. Optional: run headlessly via [Newman](https://github.com/postmanlabs/newman) for a CLI/CI-style run:
   ```
   newman run Reqres_API_Tests.postman_collection.json
   ```
