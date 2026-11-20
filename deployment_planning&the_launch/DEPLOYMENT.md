# Deployment Plan

A step-by-step guide for releasing the system to production.

## Team Roles

| Role | Responsibility |
| --- | --- |
| Flight Director | Leads the release and makes the Go/No-Go and abort decisions |
| Engineers | Run the deployment steps and the post-deployment validation |

## Environment Configs

Secrets (database password, queue credentials, API auth keys) are stored in environment variables and are never committed to Git. Service endpoints are set per environment.

## Pre-Flight Checklist

### 1. Service startup sequence

1. **PostgreSQL (database):** start first and run the DB migrations.
2. **RabbitMQ (message queue):** start second.
3. **API (container):** start last, after the database and queue are healthy.
4. **Public traffic:** open only after the Go/No-Go checks pass.

### 2. Go/No-Go: 3 health checks before public traffic

| # | Health check | Go condition |
| --- | --- | --- |
| 1 | Database | PostgreSQL accepts connections and the migrations finished with no errors |
| 2 | Message queue | RabbitMQ is running, the `jobs` queue exists, and consumer lag is below 10k |
| 3 | API | Smoke test passes: the health endpoint returns 200 and a sample request works end to end |

If any check fails, the decision is **No-Go** and public traffic stays closed.

## Deployment Steps

- **Stage 1, Pre-Deployment:** take backups and check the environment.
- **Stage 2, Deployment Steps:** run the DB migrations, then start the containers in the order above.
- **Stage 3, Post-Validation:** run smoke tests and check telemetry.
- **Stage 4, Rollback Plan:** restore the previous state instantly if production fails.

## Rollback Plan (Abort Trigger)

**Failure threshold:** roll back immediately if either of these happens after deployment:

- any smoke test or Go/No-Go health check fails, or
- consumer lag on the queue exceeds 10k.

An exhausted database connection pool also triggers rollback.

**Abort protocol:**

1. Revert the container tag to the stable v1.4.2 image.
2. Execute the pre-tested down-migration script.
