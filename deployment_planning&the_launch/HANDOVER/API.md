# API Documentation

## POST /api/jobs

Creates a job and places it on the `jobs` queue. An auth token is required.

### Request

```http
POST /api/jobs
Authorization: Bearer <token>
Content-Type: application/json

{
  "type": "report",
  "payload": { "user_id": 42 }
}
```

### Response (success)

```http
201 Created
{
  "job_id": "a1b2c3",
  "status": "queued"
}
```

### Error codes

| Code | Meaning | What to do |
| --- | --- | --- |
| 400 | Missing or invalid field in the payload | Fix the request body and resend |
| 401 | Missing or invalid auth token | Send a valid token |
| 503 | Queue or database unavailable | Retry later; the operations team follows the runbook |
