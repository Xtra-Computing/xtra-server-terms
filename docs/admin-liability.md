# Admin Liability

This page clarifies the operational responsibility boundaries of Xtra Computing Server administrators.

## What Administrators Are Responsible For

- Maintaining normal server operation, baseline availability, and core infrastructure operations.
- Applying reasonable security and access controls for shared environments.
- Conducting server audits for security, compliance, and operational integrity when needed.
- Enforcing published resource policies (GPU, CPU, memory, disk, and account rules).
- Responding to service incidents that affect multiple users or platform stability.
- Communicating major policy or operational changes through official channels.

## What Administrators Are Not Responsible For

- Resource coordination between users. Users are expected to discuss and coordinate shared resource usage among themselves.
- User code quality, training logic, experiment design, or runtime correctness.
- Debugging user code, model pipelines, or custom scripts.
- Data loss caused by user actions, script bugs, accidental deletion, or unsafe workflows.
- Job interruption due to policy enforcement (including automatic termination rules).
- Recovering files beyond available backup/retention windows.
- Performance guarantees for every workload, model, or software stack.

## User-Side Responsibilities

- Monitor jobs actively and validate expected GPU/CPU utilization behavior.
- Implement checkpoints and backups for all critical work.
- Release resources promptly when jobs become idle or no longer needed.
- Keep environments and dependencies reproducible and maintainable.
- Contact administrators in advance for exceptional workloads requiring policy exceptions.

## Incident Handling Boundary

- Administrators may intervene when system stability, fairness, or security is impacted.
- Interventions may include throttling, termination, account restrictions, or temporary suspension.
- In emergency cases, protective actions can be taken before individual notifications.

## Support Scope

- Best-effort support is provided for account access, platform-level failures, and policy clarification.
- Deep debugging of user applications, model pipelines, or custom environments is out of scope by default.

If your case is unclear, contact the administrator with reproducible details, expected behavior, and urgency.
