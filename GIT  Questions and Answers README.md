---

# Git & Source Code Management Questions Every DevOps Professional Should Know**
---

### **1. What is Source Code Management (SCM)?**

SCM is the process of **tracking and controlling every change** to code. It maintains a complete, reliable **history** of a project, enabling collaboration, audits, and safe rollbacks.

---

### **2. Advantages of Source Code Management**

SCM provides:

* **Team collaboration**
* **Audit trail/history** for all changes
* **Parallel development** without conflicts
* Ensures **system stability** with easy rollbacks

---

### **3. Popular Source Code Management Tools**

* **Git:** Distributed Version Control System (DVCS)
* Hosting Platforms: **GitHub, GitLab, Bitbucket**
* Older centralized systems: **SVN**

---

### **4. What is Git and GitHub?**

* **Git:** Tracks code changes locally, distributed across developers
* **GitHub:** Remote hosting platform for Git repos, enabling centralized access and team collaboration

---

### **5. Advantages of Git**

* **Distributed:** Full history on every machine
* **Fast** and offline-capable
* **Resilient:** Survives server failures
* Supports **branching and merging** efficiently

---

### **6. Stages in Git**

* **Working Directory:** Where files are edited
* **Staging Area:** Prepares changes for commit
* **Repository:** Permanent snapshot storage

---

### **7. Common Branching Strategy**

* **Feature Branching:** Develop new features or fixes in isolated branches to protect main codebase (`main`/`develop`) until fully tested and reviewed

---

### **8. Types of Repositories**

* **Local Repository:** Your private copy on your machine
* **Remote Repository:** Shared central repo hosted online (GitHub, GitLab, Bitbucket)

---

### **9. What is a Snapshot in Git?**

A snapshot is a **commit recording the full state** of all files at a point in time, rather than just the differences.

---

### **10. What is Git Merge?**

Integrates changes from one branch into another, creating a **merge commit** that combines histories.

---

### **11. What is Git Stash?**

A **temporary storage** for unfinished work. Saves local changes so you can switch branches or perform operations without committing incomplete code.

---

### **12. What is Git Reset?**

Moves the **HEAD pointer** to a previous commit. Useful for undoing local commits **before pushing**.

```bash
git reset --hard <commit_id>
```

---

### **13. What is Git Revert?**

Creates a **new commit** that undoes a previous commit without rewriting history — a safe way to reverse changes.

---

### **14. Difference Between Git Pull, Clone, and Fetch**

* **Clone:** One-time download of entire remote repo
* **Fetch:** Downloads remote changes without merging
* **Pull:** Fetch + merge into current branch

---

### **15. Difference Between Git Merge and Git Rebase**

* **Merge:** Preserves full commit graph (non-linear)
* **Rebase:** Moves commits onto another branch, creating **linear history**

---

### **16. What is Git Bisect?**

A **binary search** to locate a commit that introduced a bug, efficiently isolating regressions.

---

### **17. What is Git Squash?**

Combines multiple small commits into **one meaningful commit** to keep project history clean.

---

### **18. What are Git Hooks?**

**Executable scripts** that run on Git events (pre-commit, post-push) for automation or enforcing policies.

---

### **19. What is Git Cherry-pick?**

Apply a **specific commit** from one branch onto another unrelated branch.

---

### **20. Difference Between Git and SVN**

| Feature   | Git         | SVN         |
| --------- | ----------- | ----------- |
| Model     | Distributed | Centralized |
| Speed     | Fast        | Slower      |
| Offline   | Yes         | No          |
| Branching | Lightweight | Heavy       |

---

### **21. Submodules and Modules in Git**

* **Submodule:** Git repository within another, managing external dependencies
* **Module:** Standard code structure in a repo

---

### **22. Difference Between Git Branch and Git Tag**

* **Branch:** Movable pointer to latest commit
* **Tag:** Fixed pointer marking a release

---

### **23. Merge Specific Commit in a Branch**

```bash
git cherry-pick <commit_id>
```

Applies the changes as a **new commit** on the target branch.

---

### **24. Remote and Local Repositories in Pipeline**

* Developers commit to **local repo**
* CI/CD pipelines pull from **remote repo**, the **source of truth** for builds and deployments

---

### **25. How PRs (Pull Requests) Work**

PR is a formal **proposal to merge** a feature branch. Initiates **code review, testing, and collaboration** before integration.

---

### **26. How to Resolve Conflicts**

* Manually edit conflicting files
* Use `git add` to mark as resolved
* Commit the resolution

---

### **27. Do You Know Git Rebase?**

Yes. Rebase **rewrites history** by moving commits onto a new base, creating a **linear and clean history** before merging.

---

### **28. What Branching Strategy Do You Use?**

* **Feature Branching:** Isolates new work
* Keeps shared branches (`main`/`develop`) **stable and reliable**

---

### **29. How to Avoid Pushing Sensitive Files**

* Use **`.gitignore`** to exclude them
* Store secrets in **environment variables** or **secret managers**

---

### **30. Clone a Particular Branch with Last Commit ID**

```bash
git clone -b <branch> --single-branch <repo_url>
git log -1
```

Clones only the branch and shows the latest commit ID.

---

### **31. Make a Teammate Track a Renamed File**

Git tracks renames automatically. Teammates just run:

```bash
git pull
```

Changes are applied, including file renames.

---

### **32. Check Last 6 Months of Commits**

```bash
git log --since="6 months ago"
```

Filters commit history based on timestamps, useful for reviews and audits.

---

✅ **Conclusion:**
These **32 Git & SCM questions** cover core concepts, commands, branching strategies, and practical DevOps workflows.
Mastering these answers ensures confidence in interviews and **real-world DevOps collaboration**.

---

If you want, I can **combine this Git guide with the previous Ansible guide** into **one complete, 100+ question DevOps interview-ready document** in the **same Mihir Popat style** — fully formatted, with bold highlights, commands, and headings.

Do you want me to do that?
