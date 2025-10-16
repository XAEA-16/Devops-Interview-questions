### **1. What is Source Code Management (SCM)?**
SCM is the process of **tracking and controlling every change** to code. The concept is to maintain a complete, reliable **history** of the project, allowing for collaboration and safe rollbacks.

---

### **2. Advantages of Source Code Management**
The core benefits are enabling **team collaboration**, providing a full **audit trail/history**, facilitating **parallel development**, and ensuring system **stability** by allowing rollbacks.

---

### **3. Popular Source Code Management Tools**
The standard is **Git** (a distributed system), used with hosting services like **GitHub**, **GitLab**, and **Bitbucket**. Older, centralized systems like **SVN** are also in use.

---

### **4. What is Git and GitHub?**
**Git** is the **Distributed Version Control System (DVCS)** software that handles local change tracking. **GitHub** is the **remote platform** where Git repositories are hosted, enabling centralized access and team interaction.

---

### **5. Advantages of Git**
Git's main power comes from being **distributed** (full history on every machine), which makes it incredibly **fast**, highly **resilient**, and supportive of **offline work**.

---

### **6. Stages in Git**
Files move from the **Working Directory** (editing) to the **Staging Area** (preparing the commit) to the **Repository** (permanent storage of the snapshot).

---

### **7. Common Branching Strategy**
We use **Feature Branching**: A strategy where every new feature or fix is developed in an **isolated branch** to protect the main codebase (`main`/`develop`) until the work is complete and reviewed.

---

### **8. Types of Repositories**
The **Local Repository** is your private copy of the project history on your machine. The **Remote Repository** is the shared, central copy hosted online (e.g., GitHub) that team members push and pull from.

---

### **9. What is a Snapshot in Git?**
A snapshot is the core concept of a Git **commit**. Instead of just storing differences, a commit records the **full state of all project files** at that exact moment in time.

---

### **10. What is Git Merge?**
It's the operation that **integrates changes** from one line of development (a branch) into another, officially combining their histories and creating a new **merge commit**.

---

### **11. What is Git Stash?**
The concept is a **temporary holding spot** for unfinished work. It saves changes locally so you can switch branches or perform an operation without committing messy, incomplete code.

---

### **12. What is Git Reset?**
The command **rewrites history** by moving the branch pointer (**HEAD**) to a previous commit. It's used to *un-do* local commits before they are shared.

---

### **13. What is Git Revert?**
It's a **safe undo** command. It doesn't rewrite history but instead **creates a brand new commit** that precisely negates the changes of a previous one, preserving the original commit history.

---

### **14. Difference Between Git Pull, Clone, and Fetch**
**Clone** is the one-time download of the entire remote repo. **Fetch** downloads remote data. **Pull** is the combined action: fetching the data and **immediately merging** it into the current branch.

---

### **15. Difference Between Git Merge and Git Rebase**
**Merge** preserves the full commit graph (non-linear history). **Rebase** rewrites history by moving commits to the tip of another branch, resulting in a **cleaner, single-line (linear) history**.

---

### **16. What is Git Bisect?**
A diagnostic tool that uses a **binary search algorithm** to cut the search space in half repeatedly, efficiently isolating the **single commit that introduced a regression/bug**.

---

### **17. What is Git Squash?**
The process of **condensing multiple commits** (often small "WIP" commits) into one single, meaningful commit. The concept is to keep the project history clean for future readers.

---

### **18. What are Git Hooks?**
These are **executable scripts** that run automatically when specific Git events occur (e.g., pre-commit, post-push). They are used to **enforce policy** or run automation (like linting/tests).

---

### **19. What is Git Cherry-pick?**
It allows for **selective commit transfer**. The concept is to apply the changes from **one specific commit** onto a different, usually unrelated, branch.

---

### **20. Difference Between Git and SVN**
**Git** is **Distributed** (every user has the full repo history), which makes it fast and resilient. **SVN** is **Centralized** (requires constant server connection), which makes its branching heavier.

---

### **21. Submodules and Modules in Git**
A **Submodule** allows a Git repository to **contain another independent Git repository** as a sub-directory, managing external dependencies without merging their histories.

---

### **22. Difference Between Git Branch and Git Tag**
A **Branch** is a **movable pointer** to the latest commit in a development line. A **Tag** is a **fixed pointer** to a specific commit, typically used to mark stable release points.

---

### **23. Merge Specific Commit in a Branch**
Use **`git cherry-pick <commit_id>`**. The concept is to apply the specific changes as a new commit on the target branch.

---

### **24. Remote and Local Repositories in Pipeline**
Developers commit to the **Local repo**, which syncs to the **Remote repo**. The remote repo is the **source of truth** that CI/CD pipelines monitor and pull code from for testing and deployment.

---

### **25. How PRs (Pull Requests) Work**
A PR is a formal **proposal to integrate** a feature branch. The core concept is to initiate a **code review, testing, and collaboration** process *before* the merge is approved.

---

### **26. How to Resolve Conflicts**
The process involves **manually editing** the file to choose the correct changes, then using **`git add`** to tell Git the conflict markers are fixed, and finally committing the merge resolution.

---

### **27. Do You Know Git Rebase?**
Yes. I understand its power to **rewrite history** by logically transplanting commits onto a new base, which is used to create a **clean, linear history** *before* sharing or merging.

---

### **28. What Branching Strategy Do You Use?**
We use **Feature Branching**—the concept being **isolation**. New work is kept off the shared branches until it is fully reviewed, tested, and ready for integration.

---

### **29. How to Avoid Pushing Sensitive Files**
The core defense is using the **`.gitignore`** file to stop tracking. For secrets, we enforce storing them in **environment variables** or secret managers, *never* directly in the codebase.

---

### **30. Clone a Particular Branch with Last Commit ID**
I use `git clone -b <branch> --single-branch <repo_url>`. This clones **only the necessary history** for that branch. **`git log -1`** then shows the ID.

---

### **31. Make a Teammate Track a Renamed File**
Git tracks renames automatically via metadata. The teammate simply needs to **`git pull`** to update, and Git correctly applies the rename to their local working directory.

---

### **32. Check Last 6 Months of Commits**
I use **`git log --since="6 months ago"`**. The concept here is using Git's powerful filtering capability based on time stamps.

***
