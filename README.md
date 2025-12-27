# linux-permission-hardening-lab
A detailed write-up and demonstration of a lab focused on managing authorization using Linux.

# My First Cyber Lab: Managing Authorization

## Scenario

As a Security Analyst, I was tasked with auditing and hardening a project directory on a Linux system. The objective was to eliminate "permission creep" and ensure that only authorized researchers retained the specific access required for their roles, effectively mitigating the risk of unauthorized data modification.

---

## Task 1: Initial Recon

First things first, I needed to see what I was working with. I jumped into the project directory and used a command to list all the files and their current permissions. This gave me a full breakdown of the owner, group, and other users for each file.

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

## Task 2: Fixing File Permissions

Next, I found a few files that were way too open. The task was to make sure no one could write to them unless they were supposed to.

* **The Problem:** The `project_k.txt` file had write access for "others," which is a big security no-no.
* **The Fix:** I used the `chmod` command to specifically remove the write permission for others on that file.
* **Command:**
    ```bash
    chmod o-w project_k.txt
    ```

---

## Task 3: Handling Hidden Files

Then came a hidden file, `.project_x.txt`. It was supposed to be a read-only archive, but it had a vulnerability.

* **The Problem:** The user and group had write permissions on this hidden file, which wasn't supposed to happen.
* **The Fix:** I used a different `chmod` command to remove the write permissions from both the user and the group.
* **Command:**
    ```bash
    chmod u-w,g-w .project_x.txt
    ```

---

## Task 4: Locking Down a Directory

Finally, I had to secure a directory called `drafts`. The goal was to make it so only the main user could even get inside. The problem was that the group had "execute" permissions, which lets them enter the directory.

* **The Problem:** The `research_team` group could access the `drafts` directory because they had execute permissions.
* **The Fix:** I used `chmod` one more time to take away the execute permission from the group.
* **Command:**
    ```bash
    chmod g-x drafts
    ```

---

## What I Learned

This lab was a solid crash course in managing permissions, which is a fundamental part of cybersecurity. I got hands-on experience with the `ls` and `chmod` commands and learned how to apply the principle of **least privilege**—giving users only the access they absolutely need. This skill is critical for any SOC analyst, threat hunter or pen tester.
