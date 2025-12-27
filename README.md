# Linux Permission Hardening & Least Privilege Audit

### Technical Objective: Audit and remediate Linux file permissions to enforce the Principle of Least Privilege.

---

## Scenario:

As a Security Analyst, I was tasked with auditing and hardening a project directory on a Linux system. The objective was to eliminate "permission creep" and ensure that only authorized researchers retained the specific access required for their roles, effectively mitigating the risk of unauthorized data modification.

---

## Task 1: Directory Permission Audit & Gap Analysis

Performed an initial audit of the target directory using `ls -l` to establish a security baseline. Identified several high-risk permission configurations, specifically over-permissive write access for "others" on project files and unnecessary execute bits for groups on sensitive directories.

* **Command:**
    ```bash
    cd projects
    ls -l
    ```
* **Output:**
    ```
    total 20
    drwx--x--- 2 researcher2 research_team 4096 Oct 14 18:40 drafts
    -rw-rw-rw- 1 researcher2 research_team 46 Oct 14 18:40 project_k.txt
    -rw-r----- 1 researcher2 research_team 46 Oct 14 18:40 project_m.txt
    -rw-r----- 1 researcher2 research_team 46 Oct 14 18:40 project_r.txt
    -rw-rw-r-- 1 researcher2 research_team 46 Oct 14 18:40 project_t.txt
    ```
* **Analysis:** This output showed the initial permissions. For example, the `project_k.txt` file had **read/write** permissions for the **user**, **group**, and **others**, which needed to be changed. The `drafts` directory also showed **execute** permissions for the **group**, which allowed them to enter the directory.

---

## Task 2: Fixing Over-Permissive Write Access

Mitigated the risk of unauthorized data modification by stripping write permissions from the "others" class. Applied `chmod o-w` to enforce data integrity and ensure that only owner and group-level collaborators maintain modification rights.

* **The Problem:** The `project_k.txt` file had write access for "others," which is a big security no-no.
* **The Fix:** I used the `chmod` command to specifically remove the write permission for others on that file.
* **Command:**
    ```bash
    chmod o-w project_k.txt
    ```

---

## Task 3: Hardening Hidden Archive Integrity

Discovered a hidden archive with excessive write permissions, creating a vulnerability in data persistence. Executed `chmod u-w,g-w` to convert the asset into a read-only state for all users, successfully hardening the hidden file against accidental or malicious overwrites.

* **The Problem:** The user and group had write permissions on this hidden file, which wasn't supposed to happen.
* **The Fix:** I used a different `chmod` command to remove the write permissions from both the user and the group.
* **Command:**
    ```bash
    chmod u-w,g-w .project_x.txt
    ```

---

## Task 4: Securing Sensitive Project Silos

Remediation Action: Restricted directory traversal access to the primary owner to prevent unauthorized information gathering. Removed the execute bit (`-x`) from the `research_team group`, effectively "siloing" the directory and enforcing a strict Zero Trust access model for the restricted asset.

* **The Problem:** The `research_team` group could access the `drafts` directory because they had execute permissions.
* **The Fix:** I used `chmod` one more time to take away the execute permission from the group.
* **Command:**
    ```bash
    chmod g-x drafts
    ```

---

## What I Learned
- Principle of Least Privilege: Successfully applied the security concept of providing users with the minimum level of access required to complete their job functions.

- Technical Auditing: Leveraged `ls -l` to perform a manual audit of owner, group, and world permissions to identify security vulnerabilities.

- Permission Hardening: Mastered the use of `chmod` to strip unnecessary write (`-w`) and execute (`-x`) bits from sensitive files and directories.

- Access Control Management: Developed a systematic approach to revoking permissions from unauthorized groups while maintaining operational availability for the primary user.

- Hidden File Security: Identified and secured hidden assets (e.g., `.project_x.txt`) that were vulnerable to unauthorized modification.

---

**Author:** Joan 

---
