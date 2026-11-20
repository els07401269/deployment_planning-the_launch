# Known Issues

| Issue | Workaround |
| --- | --- |
| `rabbitmqctl purge_queue jobs` removes every waiting job, so queued work is lost | Use it only when consumer lag exceeds 10k, and notify the Flight Director first |
| Rollback fails if the down-migration script was never tested | Test the down-migration script before every release |
| Exhausted database connection pool makes the API return 503 | Restart PostgreSQL, then the API (see the runbook) |
