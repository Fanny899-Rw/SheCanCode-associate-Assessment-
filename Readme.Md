📖 Idempotency Gateway (The "Pay-Once" Protocol)
1. Business Context
Problem: Double charging happens when payment requests are retried.

Solution: An API layer checks the Idempotency-Key to ensure each request is processed only once.

2. Architecture Diagram
Flow: Client → API Gateway → Idempotency Layer → Payment Processor → Cache/DB → Client.

Paths:

First Request → process + store response.

Duplicate Request → return cached response.

Different Request → error (409/422).

In-flight Request → block & return same result.

## Architecture Diagram

![Idempotency Gateway Architecture](docs/Architecture.png)


3. Setup Instructions
Fork Repository into your GitHub account.

Install Java JDK (version 21 or 25).

Install Maven.

Run locally:

bash
mvn spring-boot:run
Verify API: POST /process-payment.

4. API Documentation
Endpoint: POST /process-payment

Headers:

Idempotency-Key: <unique-string>

Request Body:

json
{ "amount": 100, "currency": "RWF" }
Responses:

First request → 201 Created, "Charged 100 RWF".

Duplicate → cached response + X-Cache-Hit:true.

Different body → 409 Conflict / 422 Unprocessable Entity.

In-flight → waits, returns same result.

5. User Stories & Acceptance Criteria
User Story 1: Happy path.

User Story 2: Duplicate attempt.

User Story 3: Same key, different body.

Bonus User Story: In-flight duplicate.

6. Design Decisions
Storage Choice: Redis/SQLite/Map for caching responses.

Hashing: Compare payloads for fraud check.

Locking: Prevent race conditions.

7. Developer’s Choice Feature
Audit Logging: Log duplicate attempts (timestamp, key, client IP).

Purpose: Detect retry patterns, prevent abuse, improve monitoring.

8. Pre-Submission Checklist
Repo is Public.

Clean unnecessary files.

Server runs without crash.

README replaced with documentation.

Diagram included.

Multiple commits with clear messages.
