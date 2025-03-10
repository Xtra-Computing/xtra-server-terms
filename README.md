# Terms of Use - Xtra Computing Server

**Introduction**
The Xtra Computing Server provides computational resources (GPU, CPU, memory, and storage) primarily to support research and academic activities. Users must follow the guidelines outlined in this document to ensure fair resource allocation and maintain a productive computing environment.

> [!NOTE]  
> Resources are for Xtra Computing Group use only.
> 
> If your improper usage is reported, your computing tasks may be terminated.


## Account

### Creation

Users must apply via the provided registration form: https://forms.gle/Wf8qbNeuSPS2ia8u6

### Account Management

| Event              | Action                 | Notes                                                    |
|--------------------|------------------------|----------------------------------------------------------|
| Account Expiration | Account Frozen         | You cannot log in. Contact admin within 6 months to unfreeze. |
| 6 months post-expiration | Account Removal | Data preserved temporarily in cold-storage[^1]; integrity not guaranteed. |
| 12 months post-expiration | Data Deletion | Files permanently deleted; recovery impossible.         |

[^1]: Cold storage refers to a type of data storage designed for infrequently accessed data. Since moving data in and out of cold storage takes a long time, it is mainly used for archiving or backup purposes rather than for data that needs to be accessed frequently.

All notifications and alerts are communicated exclusively via your registered email address.

**Unfreezing Your Account**
To request reactivation after account freezing, contact the administrator. Reactivation requests are typically processed within 1-2 business days.

---

## Disk

### **Disk Space Allocation**

| User Category          | Home Disk Quota |
|------------------------|-----------------|
| PhD Students           | 512 GiB         |
| Others | 256 GiB         |



Additional disk space requests are possible via email to the administrator and are considered based on project justification and resource availability.

For hosting large datasets, please contact the administrator. Dataset hosting will not count against your quota.

### **Data Integrity**

Data integrity is not guaranteed. Users must perform regular backups. Weekly backups are recommended, with more frequent backups suggested for critical data. For critical data requiring higher reliability, use the `/shared/hdd` or `/shared/ssd` directory protected by RAIDZ2 (resilient to two drive failures).

### **Privacy**

By using the servers, users consent to file reviews by administrators to ensure compliance. Reviews typically occur in response to specific incidents, abnormal resource usage, or during security audits. We never do routine or random inspections for no reason.

**Do not** store private or sensitive files (e.g., personal photos, videos, confidential documents).

### **Disk Usage Accounting**

Disk usage is tracked monthly (GB/month) across all servers, attributed uniquely per user. High usage users may be contacted to reduce disk usage. 

---

## GPU

### Default Quota

We aim to ensure that all users have equal and convenient access to GPU resources. Our system is designed to be as unrestricted as possible while maintaining fairness among users.

| User Category          | GPUs Allowed Without Application | Scaling Factor |
|------------------------|----------------------------------|-----|
| PhD Students           | 2 GPUs freely                    | 1.0 |
| PostDoc           | 2 GPUs freely                    | 1.0 |
| RA / Visiting Student | 2 GPUs freely                | 1.2 |
| External Collaborators | 1 GPU freely                | 1.3 |
| FYP or Master Student | 1 GPU freely                | 1.3 |

Users can utilize GPUs freely within their quota and may also exceed their quota when additional GPUs are available and not in use by others.

### Extra GPU Usage

Extra GPUs may be utilized without application when available. However, if another user requires GPU resources within their quota, they may terminate your extra usage processes using the command `kgpus`. Users whose processes are terminated will **not** be notified.

This command will only terminate extra usage processes and will not affect allocated projects with approved reservations.

If multiple users exceed their GPU quotas, processes of users with the highest GPU-hour usage will be terminated first.

For projects requiring additional GPU resources, please contact administrators to reserve GPUs.

**GPU Usage Accounting:** GPU usage is tracked by the following formula:

`GPU-hours (P) = Number of GPUs (N) × Hours used (H) × User-group scaling factor (S)`

*Example*: For a PhD student (S=1) using 2 GPUs for 10 hours:  
`P = 2 GPUs × 10 hours × 1 = 20 GPU-hours`

Users can check their real-time GPU usage through the system monitoring tools provided. (we will provide it later)

---

## CPU & Memory

Currently, we do not limit the CPU and memory usage of our users. However, excessive CPU or memory usage negatively impacting others, may result in penalties.

Excessive usage is determined based on its impact on system stability

- Out-of-memory (OOM) errors that prevent other users from accessing the server (e.g., making it impossible to SSH in).
- Any behavior that requires administrator intervention to restore normal operations.

| Offense Times | Action                                |
|---------------|---------------------------------------|
| 1st           | Notification                          |
| 2nd           | Warning                               |
| 3rd           | Account frozen for 2 day              |
| 4th           | Account frozen for 2 weeks            |
| 5th           | Permanent ban from all infrastructures|

---

### General Disclaimer

Xtra Computing Server administrators and affiliates are not responsible for data loss, damages, or inconveniences arising from hardware failures, software issues, or user actions. Users assume full responsibility for data backups and accept resources as-is without warranty.

