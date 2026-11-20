# Q1: Why are Friday 5 PM deployments dangerous?
If production crashes at that hour, the office is empty and nobody is there to fix it. The handout’s example has no recovery docs, so there is no guide to recover from either. The deadline slide also says “No Friday 5 PM Deploys!”

# Q2: Which plan component undoes a failed release?
The Rollback Plan, also called the launch abort. It restores the previous state right away, for example by reverting the container tag to the stable v1.4.2 image and running the pre-tested down-migration script.

# Q3: What document explains APIs and message queue flows?
Integration Documentation. It is the technical manual explaining how systems talk to each other. It covers APIs and contracts (endpoints, payloads, auth tokens), data flow (queue topics and DB writes) and error handling (retries, timeouts, dead-letter queues).

# Q4: Which artifact guides ops teams during live outages?
The Operations Runbook, the “Flight Crew Manual”. It is a how-to guide for the operations team during live incidents. Each entry pairs a scenario and symptom with the exact command, such as docker compose restart postgres.

# Q5: Name all four items inside a Handover Package.

Architecture Diagram
API Documentation
Operations Runbook
Known Issues