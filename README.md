# Terms of Use - Xtra Computing Server

**Introduction**
The Xtra Computing Server provides computational resources (GPU, CPU, memory, and storage) primarily to support research and academic activities. Users must follow the guidelines outlined in this document to ensure fair resource allocation and maintain a productive computing environment.

## Account

### Creation

Users must apply via the provided registration form: https://forms.gle/Wf8qbNeuSPS2ia8u6

### Account Management

| Event              | Action                 | Notes                                                    |
|--------------------|------------------------|----------------------------------------------------------|
| Account Expiration | Account Frozen         | You cannot log in. Contact admin within 6 months to unfreeze. |
| 6 months post-expiration | Account Removal | Data preserved temporarily in cold storage; integrity not guaranteed. |
| 12 months post-expiration | Data Deletion | Files permanently deleted; recovery impossible.         |

Administrators will contact users by email **7 days** before account expiration. All notifications and alerts are communicated exclusively via your registered email address. Users are responsible for regularly checking their email.

**Unfreezing Your Account**
To request reactivation after account freezing, contact the administrator via email at **admin@example.com**. Reactivation requests are typically processed within 1-2 business days.

---

## Disk

**Disk Space Allocation**

| User Category          | Home Disk Quota |
|------------------------|-----------------|
| PhD Students           | 512 GiB         |
| RA & External Collaborators | 256 GiB         |

High usage users may be contacted to reduce disk usage. Additional disk space requests are possible via email to the administrator and are considered based on project justification and resource availability.

For hosting large datasets, please contact the administrator. Dataset hosting will not count against your quota.

**Data Integrity**
Data integrity is not guaranteed. Users must perform regular backups. Weekly backups are recommended, with more frequent backups suggested for critical data. For critical data requiring higher reliability, use the `/shared/` directory protected by RAIDZ2 (resilient to two drive failures).

**Privacy**
By using the servers, users consent to file reviews by administrators to ensure compliance. Reviews typically occur in response to specific incidents, abnormal resource usage, or during security audits. Routine or random checks without cause are not standard practice.

Do not store private or sensitive files (e.g., personal photos, videos, confidential documents).

**Disk Usage Accounting**
Disk usage is tracked monthly (GB/month) across all servers, attributed uniquely per user.

---

## GPU

**Resource Allocation**

| User Category          | GPUs Allowed Without Application |
|------------------------|----------------------------------|
| PhD Students           | 2 GPUs freely                    |
| RA & External Collaborators | 1 GPU freely                     |

Extra GPUs may be utilized without application when available. However, if another user requires GPU resources within their quota, they may terminate your extra usage processes using the command `kgpus`. Users whose processes are terminated will receive automated email notifications.

If multiple users exceed GPU quotas, processes of users with the highest GPU-hour usage will be terminated first.

For projects requiring additional GPU resources, please contact administrators to reserve GPUs.

**GPU Usage Accounting**
GPU usage is tracked by the following formula:

`GPU-hours (P) = Number of GPUs (N) × Hours used (H) × User-group scaling factor (S)`

*Example*: For a PhD student (S=1) using 2 GPUs for 10 hours:  
`P = 2 GPUs × 10 hours × 1 = 20 GPU-hours`

---

## CPU & Memory

**Excessive Usage**
Excessive CPU or memory usage negatively impacting others, defined as utilization consistently exceeding 90% for periods longer than 2 hours, may result in penalties:

| Offense Level | Action                                |
|---------------|---------------------------------------|
| 1st           | Notification                          |
| 2nd           | Warning                               |
| 3rd           | Account frozen for 1 day              |
| 4th           | Account frozen for 1 week             |
| 5th           | Account frozen for 4 weeks            |
| 6th           | Permanent ban from all infrastructures|

---

### General Disclaimer
Xtra Computing Server administrators and affiliates are not responsible for data loss, damages, or inconveniences arising from hardware failures, software issues, or user actions. Users assume full responsibility for data backups and accept resources as-is without warranty.

