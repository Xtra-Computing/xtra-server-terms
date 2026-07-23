# Terms of Use - Xtra Computing Server

*Last updated: 2026-07-23*

**Introduction**
The Xtra Computing Server provides computational resources (GPU, CPU, memory, and storage) primarily to support research and academic activities. Users must follow the guidelines outlined in this document to ensure fair resource allocation and maintain a productive computing environment.

> [!IMPORTANT]
> Resources are intended for the use of Xtra Computing Group only.

> [!WARNING]
> Any misuse may result in the termination of your computing tasks.

## Account

### Creation

Users must apply via the provided registration form: https://forms.gle/Wf8qbNeuSPS2ia8u6

Account validity is determined by the expiration date provided by the user during registration, subject to confirmation by the administrator.

### Account Management

| Event              | Action                 | Notes                                                    |
|--------------------|------------------------|----------------------------------------------------------|
| Account Expiration | Account Frozen         | You cannot log in. Contact admin within 6 months to unfreeze. |
| 6 months post-expiration | Account Removal | Data preserved temporarily in cold-storage[^1]; integrity not guaranteed. |
| 12 months post-expiration | Data Deletion | Files permanently deleted; recovery impossible.         |

[^1]: Cold storage refers to a type of data storage designed for infrequently accessed data. Since moving data in and out of cold storage takes a long time, it is mainly used for archiving or backup purposes rather than for data that needs to be accessed frequently.

All notifications and alerts are communicated exclusively via your registered email address.

> [!CAUTION]
> If your account remains expired, your data may be removed after 6 months and permanently deleted after 12 months.

**Unfreezing Your Account**
To request reactivation after account freezing, contact the administrator. Reactivation requests are typically processed within 1-2 business days.

---

## Acceptable Use and Security

Users must not:

- Share accounts, passwords, SSH keys, access tokens, or any other credentials.
- Mine cryptocurrency or use server resources for related activities.
- Conduct unauthorized attacks, vulnerability scans, network scans, malware activity, or attempts to disrupt systems or other users.
- Bypass, evade, disable, or interfere with resource limits, monitoring, access controls, or policy-enforcement mechanisms.
- Store, distribute, or process unlawful content, or violate software licenses, copyrights, or other intellectual-property rights.
- Process sensitive, regulated, confidential, or research-participant data without prior approval from the administrator and any other approvals required by applicable institutional policies.

This list is not exhaustive. Upon discovery of any such activity, the account will be permanently banned. These violations are not subject to the progressive offense schedule described below.

---

## Disk

### **Disk Space Allocation**

| User Category          | `home` Disk Quota |
|------------------------|-----------------|
| PhD Students           | 512 GiB         |
| Others | 256 GiB         |

Additional disk space requests are possible via email to the administrator and are considered based on project justification and resource availability.

For hosting large datasets, please contact the administrator. Dataset hosting will not count against your quota.

### **Data Integrity**

Data integrity is **not** guaranteed. Users must perform regular backups. Weekly backups are recommended, with more frequent backups suggested for critical data. The `/shared/hdd` and `/shared/ssd` directories use RAIDZ2 for fault tolerance against up to two drive failures. RAIDZ2 is **not a backup** and does not protect against every form of data loss. There is currently no per-user quota on these shared directories; please use them responsibly.

> [!IMPORTANT]
> Backup responsibility belongs to the user. Always maintain restorable checkpoints for critical work.

### **Privacy**

Administrators may collect and review necessary access, security, audit, system, and resource-usage logs and metrics for operations, security, capacity planning, incident response, and policy enforcement. Administrators may also review files when investigating incidents, abnormal resource usage, security concerns, or suspected policy violations. By using the servers, users consent to this monitoring and review.

**Do not** store private or sensitive files (e.g., personal photos, videos, confidential documents).

### **Disk Usage Accounting**

Disk usage is tracked monthly (GB/month) across all servers, attributed uniquely per user. High usage users may be contacted to reduce disk usage. 

---

## GPU

### Default Quota

We aim to ensure that all users have equal and convenient access to GPU resources. Our system is designed to be as unrestricted as possible while maintaining fairness among users.

| User Category          | GPUs Allowed Without Application |
|------------------------|----------------------------------|
| all_user           | 2 GPUs freely                    |


Users can utilize GPUs freely within their quota and may also exceed their quota when additional GPUs are available and not in use by others.

The GPU quota defines the maximum standard allocation that may be used without an application. It is not a guarantee of GPU availability, uninterrupted access, job completion, or performance.

If you have compute-intensive tasks, please consider using the [HACC Cluster](https://xacchead.d2.comp.nus.edu.sg/) or the [SoC Cluster](https://dochub.comp.nus.edu.sg/cf/guides/compute-cluster/access), which are better suited for high-performance computing needs.

### Extra GPU Usage

**You can use more than 2 GPUs without application.** However, extra GPU usage beyond your quota is opportunistic and subject to preemption:

- Jobs using GPUs within the standard quota are not guaranteed to remain available or uninterrupted.
- GPUs beyond your quota are **best-effort**. When another user needs GPUs to fill their own quota, your extra jobs may be terminated to free up resources.
- Any GPU job may also be interrupted or terminated because of maintenance, failures, security incidents, policy enforcement, or other operational needs.
- The administrator may attempt to notify affected users before termination, but advance notice is not guaranteed.
- Users running extra GPU jobs should implement checkpointing to minimize progress loss from preemption.

> [!IMPORTANT]
> Jobs exceeding your GPU quota may be terminated at any time to ensure fair access for all users. Always checkpoint your work.

### Responsible Use

To ensure fair and efficient utilization of our shared GPU infrastructure, we are introducing the following resource management policy effective immediately:

#### Automatic Termination Policy

Any process that:
1. Occupies large GPU memory, and
2. Maintains less than 1% GPU utilization continuously for 10 minutes

may be automatically terminated by the system.

> [!WARNING]
> Low-utilization, high-memory GPU jobs are subject to automatic termination.

#### User Responsibility

Users are fully responsible for monitoring their jobs. Any loss of progress, data, or runtime caused by automatic termination under this policy is the responsibility of the job owner.

Please ensure that your scripts:
1. Do not hold GPU memory while idle
2. Properly release resources when inactive
3. Avoid prolonged zero-utilization states

> [!CAUTION]
> Progress or data loss caused by policy-triggered termination is the user’s responsibility.

#### Temporary Exceptions

Temporary exceptions may be granted on a case-by-case basis depending on the application. If your workload legitimately requires GPU memory residency with low or zero utilization, please contact the administrator in advance with justification.

Thank you for your cooperation.

### Reserve GPUs  

To reserve GPUs, please fill out the reservation form: [Reservation Form](https://forms.gle/6W1CxQAojMANpx1FA).

---

## CPU & Memory

Each user is subject to a **500 GiB RAM limit** per node, normally enforced via the systemd `user-.slice` cgroup. Processes that exceed this limit may be OOM-killed within the user's slice. Enforcement behavior and process isolation are not guaranteed. Higher limits may be granted on request with justification.

CPU usage is not capped by default. On a single machine, if a user's processes continuously consume CPU capacity equivalent to more than 50% of that machine's logical CPU cores for more than 8 consecutive hours, a per-machine CPU quota will be imposed without advance notice. The quota limits the user's aggregate CPU usage on that machine to one quarter (1/4) of its logical CPU cores. Once imposed, the quota remains in effect permanently. Beyond the RAM cap, memory usage is not otherwise limited. However, excessive memory usage that negatively impacts others may result in penalties.

Excessive usage is determined based on its impact on system stability

- Out-of-memory (OOM) errors that prevent other users from accessing the server (e.g., making it impossible to SSH in).
- Any behavior that requires administrator intervention to restore normal operations.

| Offense Times | Action                                |
|---------------|---------------------------------------|
| 1st           | Notification                          |
| 2nd           | Warning                               |
| 3rd           | Account frozen for 2 days             |
| 4th           | Account frozen for 2 weeks            |
| 5th           | Permanent ban from all infrastructures|

The offense counter starts from the date of the first violation and is monitored over a rolling 3-month window. Offenses outside this window are not counted. *(Provisional rule, subject to revision.)*

Administrators may determine that a violation occurred and impose a penalty based on their operational judgment; no formal evidence or evidentiary procedure is required. Affected users may appeal by contacting the administrator. The administrator has sole and final authority to interpret these terms and determine the outcome of violations, exceptions, enforcement actions, and appeals.

### Docker-induced Violations

Misuse via Docker containers — including (but not limited to) causing OOM errors or improperly occupying GPUs — follows its own escalation focused on Docker privileges:

| Offense Times | Action                                                          |
|---------------|-----------------------------------------------------------------|
| 1st           | Notification                                                    |
| 2nd           | Warning                                                         |
| 3rd           | Permanent revocation of Docker privileges; account flagged     |

The same rolling 3-month window applies. *(Provisional rule, subject to revision.)*

---

## Network

Users may run services on designated open ports. Port availability is governed by NUS School of Computing firewall policies and may change without notice.

For full details, see: [Network Policy](docs/network.md).

---

### General Disclaimer

All machines, services, and resources are provided entirely **as-is** and **as-available**, without any promise or guarantee of availability, uptime, access, capacity, allocation, uninterrupted execution, job completion, data durability, performance, compatibility, or support. Nothing in these terms or related documents creates a service-level commitment. Xtra Computing Server administrators and affiliates are not responsible for data loss, damages, or inconveniences arising from hardware failures, software issues, operational actions, policy enforcement, or user actions. Users assume full responsibility for data backups and use the resources at their own risk.

> [!CAUTION]
> Service is provided as-is without warranty. Keep independent backups and recovery plans.

For detailed administrator boundaries, see: [Admin Liability](docs/admin-liability.md).

---

## Contact

For all administrative requests, policy questions, or exception applications, contact the administrator at: **hhh@u.nus.edu**

Last update: July 23, 2026
