# Git — Complete Notes

> **Learning Mantra:** Mastering a topic is 30% technology and 70% "the ability to explain it well." Knowing something isn't enough — until you can explain it simply to someone else, the concept isn't truly clear. These notes are organized so you can both study them yourself and teach them to others.

---

## 1. Why Git?

In the earlier days, tools like **SVN** and **Bitkeeper** were used to manage code/files, but they came with several problems — being centralized, depending on constant connectivity, having a single point of failure, and so on.

**Git** was built to solve exactly these problems:

- Git is a **technology** that lets you keep your code, along with a branching strategy, on any SCM (Source Code Management) tool.
- Git is both a **Centralized and Distributed Version Control System**.
- Before pushing code, teams define a strategy called a **Git Branching Strategy**:
  - For **infrastructure code** → **Trunk Based Branching Strategy**
  - For **application code** → **Git Flow Branching Strategy**
  - Every project can choose its own strategy depending on its needs.

### Popular Git-based SCM Tools

| Tool | Provider |
|---|---|
| GitHub | GitHub / Microsoft |
| Azure Repos | Microsoft Azure |
| AWS CodeCommit | Amazon |
| Google Cloud Source Repos | Google |
| IBM DevOps Code | IBM |
| Bitbucket | Atlassian |
| GitLab | GitLab |

---

## 2. Version Control System (VCS)

A Version Control System tells you:
- **On which date**
- **At what time**
- **In which line**
- **What change** was made

There are two main types of version control:

### A) Centralized Version Control (CVCS)
- All users connect to a single **common/central server** (files like `main.tf`, `variable.tf`, `provider.tf`, `terraform.tfvars` live at a central location).
- **Problems:**
  - **Single Point of Failure** — if the central server goes down, everyone's work stops.
  - **Requires constant connectivity** — no internet/network means no work.

### B) Distributed Version Control (DVCS) — Example: **Git**
- Every user has their own **local copy** of the entire repository, including its full history.
- User1 and User2 each independently have `main.tf`, `variable.tf`, `provider.tf`, `terraform.tfvars`, plus their own version history (`version1`, `version2`).
- There's no single point of failure — everyone has their own complete local repository.

---

## 3. What is Git? (Definition)

> **Git is a Distributed Version Control System.** It makes team collaboration easy, tracks version history, enables rollback and branching, and supports smooth development and deployment.

To track changes made to files in any folder, that folder needs to be turned into a **Git repository**.

```bash
git init
```
- The `git init` command "activates" Git inside a folder.
- As soon as Git is initialized, a hidden **`.git`** folder is created — this folder manages and tracks every change made inside that directory.

The local repository (`.git`) can later be connected to a remote SCM tool such as:
- GitHub
- Azure Repos
- Bitbucket
- (Just like your **Photos** app backs up to **Google Photos**, your local repo syncs/backs up to a remote repo.)

---

## 4. The 3 Areas of Git (Architecture)

In Git, code moves through 3 stages/areas:

```
Working Area  --(git add <filename>)-->  Staging Area  --(git commit -m "message")-->  Committed Area (Local Repo)
```

| Area | Meaning |
|---|---|
| **Working Area** | Where you create/edit files (not yet tracked) |
| **Staging Area** | After `git add`, files land here — this decides what goes into the next commit |
| **Committed Area (Local Repo)** | After `git commit`, a permanent **version/snapshot** is created (e.g., "Version 1 — Rg code done") |

**Example:** Working Area contains files — `bittu.tf`, `dhondhu.tf`, `main.tf`, `pinki.tf`, `provider.tf`, `terraform.tfvars`, `tinku.tf`, `variables.tf`
→ `git add bittu.tf` → only `bittu.tf` moves to the Staging Area
→ `git commit -m "added code"` → that file is permanently saved into the Committed Area (local repo).

---

## 5. Repository Management — Basic Commands

| Command | What it does |
|---|---|
| `git init` | Turns a folder into a Git repository (creates a local repo) |
| `git clone <url>` | Copies a remote repository onto your local machine |
| `git status` | Shows how many files are in the Working Area and how many are in the Staging Area |
| `git add <filename>` | Moves a file from Working Area to Staging Area (`git add .` = all files) |
| `git commit -m "message"` | Permanently saves staged files into the Local Repo (creates a version) |
| `git log` | Shows all commits/changes in the Committed Area (history) |
| `git show <commit-id>` | Shows the full details of a specific commit |
| `git restore` | Reverts files back to a previous state |
| `git push` | Sends local commits to a remote repo (GitHub/Azure Repos/Bitbucket) |
| `git pull` | Brings the latest changes from the remote repo into your local repo |

---

## 6. Commit History and Branches

**Commit History** — every commit has a unique ID (hash), for example:
```
f2180acea9bd1b1f85b075642da7c5403f35fa87   → Initial commit
5d8071e8d7e749b785f84770fe7eac6cf4e9c1ee   → added landing zone code
```

### What is a Branch?
A branch is a **parallel line of development** — it lets you work on something new without touching the main code.

Common branch names: `main`/`master`, `feature`, `release`, etc.

### Branch-related Commands

| Command | What it does |
|---|---|
| `git branch <name>` | Creates a new branch |
| `git checkout <branch>` | Switches to a branch (the older way) |
| `git checkout -b <branch>` | Creates a new branch **and** switches to it immediately |
| `git switch <branch>` | The modern command for switching branches |
| `git stash` | Temporarily sets aside your current uncommitted changes |
| `git tag` | Labels a commit with a name (e.g., a version release: v1.0) |
| `git history` | Views the history of commits/changes |
| `git cherry-pick <commit-id>` | Brings a specific commit from another branch into the current branch |
| `git merge` | Merges the changes of one branch into another |
| **Merge Conflicts** | Happen when two branches change the same line/file differently — Git can't decide automatically, so it must be resolved manually |

---

## 7. Pull Requests and Branch Protection

> **"There's a snake by the riverbank — pushing directly to the `main` branch is a cardinal sin!"**
> (In other words: pushing straight to `main` is risky and should always be avoided.)

### Why is Branch Protection Necessary?

When the risk is high, the `main` branch should be protected immediately:

1. **Set up Branch Protection Rules:**
   - Require a **Pull Request (PR)** before merging into `main`
   - Require **at least 1 approver/reviewer**

2. Create a **feature branch** from `main` and check it out.

3. Make your new code changes inside the feature branch (e.g., adding a new resource group in tfvars).

4. Commit and push your changes:
   ```bash
   git status
   git add .
   git commit -m "added rg in feature"
   git push
   ```

### The Pull Request (PR) Process — Step by Step

1. A pipeline runs automatically from the **feature branch**, which includes automated scanning:
   - **Stage 1 — Scan Stage:** all security/quality scanning tasks
   - **Stage 2 — Plan Stage:** a preview of what changes will be made

2. The developer validates the code and raises a **PR**, assigning a **reviewer**.

3. The reviewer receives the PR link → opens it and checks the feature branch pipeline (scan stage and plan stage results).

4. If everything looks good:
   - The reviewer compares the code changes
   - If the changes look correct → the **PR is approved** → the code is **merged** into `main`

5. The pipeline runs again from the **main branch**, but this time with **3 stages**:
   - **Scan** → **Plan** → **Apply** (with manual approval)

> **Important Design Rule (Pipeline Condition):**
> - From the `feature` branch → only **Scan + Plan** run
> - From the `main` branch → **Scan + Plan + Apply**, all three run (with manual approval)

---

## 8. Git Best Practices

- Never push directly to the `main`/`master` branch.
- Create a separate **feature branch** for every new feature/change.
- Write meaningful **commit messages** (e.g., `"added rg in feature"`).
- Check `git status` frequently.
- Follow the **PR + review** process before merging.
- Always keep branch protection rules (minimum 1 approver) enabled on `main`.
- Use **Trunk Based Branching** for infrastructure code and **Git Flow Branching** for application code.

---

## 9. End-to-End Practical Workflow (Real Example)

```
1. Create a GitHub account, organization, and repository
2. Install Git on your local machine
3. Clone the repository:
   git clone https://github.com/devopsinsiders/azure-landing-zone-b18.git
4. Create a feature branch from main:
   git branch feature/mcp
   git checkout feature/mcp
   (or directly: git checkout -b feature/mcp)
5. Add new code inside the feature/mcp branch
6. git status
7. git commit -m "added new code"
8. git push
```

After this:
- A **Pull Request** is raised and assigned to a reviewer.
- Once approved by the reviewer, the code gets **merged** into the `main` branch.
- The push status on `main` shows something like *"1 commit ahead."*

---

## 10. Quick Recap — Cheat Sheet

| Concept | One-liner |
|---|---|
| Git | Distributed Version Control System |
| `git init` | Turn a folder into a repo |
| `git clone` | Copy a remote repo locally |
| `git status` | Status of Working/Staging areas |
| `git add` | Working → Staging |
| `git commit` | Staging → Local Repo (permanent version) |
| `git push` | Local Repo → Remote Repo |
| `git pull` | Remote → Local (fetch latest) |
| `git log` | View commit history |
| `git show` | Details of a specific commit |
| `git branch` | Create a new branch |
| `git checkout -b` | Create + switch to a branch |
| `git switch` | Switch branches |
| `git stash` | Temporarily set changes aside |
| `git merge` | Merge branches together |
| `git cherry-pick` | Bring a specific commit into another branch |
| Pull Request | The process of merging into main after review |
| Branch Protection | Prevents unsafe/direct pushes to main |

---

## 11. Homework (Practice Tasks)

1. Raise a Pull Request with a reviewer, and then merge it into `main` yourself.
2. Try to recreate this entire setup as a **GitHub Actions pipeline**, with help from **AntiGravity**.
3. Explain the full Git flow (init → add → commit → push → PR → merge) out loud and record yourself doing it **at least 20 times** — until you can explain it fluently.

---

*Notes prepared from the "Git Basics" session PDFs — organized for both self-study and teaching others.*
