# Operations Runbook

A how-to guide for the operations team during live incidents.

## Restart the primary API

**Symptom:** The API stops responding or returns 503 errors.

**Steps:**

1. Check that the database is healthy first. If the connection pool is exhausted, run:
   ```bash
   docker compose restart postgres
   ```
2. Restart the API container:
   ```bash
   docker compose restart api
   ```
3. Wait for the container to start, then run the smoke test. The health endpoint must return 200.
4. Confirm the queue is healthy. Consumer lag must be below 10k. If it is above 10k, run:
   ```bash
   rabbitmqctl purge_queue jobs
   ```
5. If the API still fails, escalate to the Flight Director and start the rollback plan: revert to the stable v1.4.2 image (see [DEPLOYMENT.md](../DEPLOYMENT.md)).

## Other scenarios

| Scenario | Symptom | Exact action |
| --- | --- | --- |
| Restart database | Connection pool exhausted | `docker compose restart postgres` |
| Clear stuck queue | Consumer lag exceeds 10k | `rabbitmqctl purge_queue jobs` |
