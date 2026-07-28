# Git, GitHub & Terraform Workflow — Notes

> These notes are organized from diagram/whiteboard content, so it's easy to revise on your own and also explain to others.

---

## 1. Recap — What We've Learned So Far

### Phase 1: Manual Deployment
- Deployed a **one-tier app** on Windows and Linux VMs.

### Phase 2: Azure Landing Zone → Manual
The landing zone was built manually, covering these components:
- Hierarchy
- Networking
- Compute
- Load Balancing
- Bastion
- Database
- NSG (Network Security Group)

### Phase 2: Azure Landing Zone → Terraform
The same landing zone was then automated using **Terraform**, through Child and Parent Modules. Concepts covered:
- `for_each`
- Map of objects
- Dynamic block
- Conditional iteration
- Output block
- Data block
- Lifecycle
- Remote backend
- Custom data script
- Terraform functions
- Provider
- Locking
- Import block
- Null resource
- `terraform taint`

**Key takeaway:** Learned to build an Azure landing zone using Terraform child-parent module best practices, where the state file was stored inside a **remote backend storage account**.

### Home Work
- Build the code for Keyvault.
- After VM creation, Terraform and nginx should be installed on all machines using **provisioners / custom data script**.

---

## 2. Source Code Management (Git) — Basics

### Where is code stored? (Common Locations)
Common Locations (Source Code Management platforms):
- GitHub
- BitBucket
- GitLab
- AWS CodeCommit
- Azure Repos

> **Important:** All these platforms work on one common underlying technology — **Git**.

### Benefits of Source Code Management
1. Stores version history of the code
2. Keeps a backup of the code
3. Multiple teammates can work together at the same time
4. Enables conflict management
5. Allows rollback
6. Provides branch isolation
7. Enables code review

> Code that is kept locally needs to be pushed to remote GitHub — and maintained with a proper **branching strategy**.

---

## 3. GitHub Setup — Step by Step

1. Create a new GitHub account
2. Log in to the GitHub account and create a repository
3. Clone the repository to your local machine
4. Create a feature branch, and push code to it
5. Raise a **Pull Request (PR)** from the feature branch to the main branch
6. The PR will be reviewed and approved by the reviewer

### Detailed Setup Flow

| Step | Action |
|---|---|
| 1 | Create a GitHub account |
| 2 | Create a public repository (with README + .gitignore enabled) |
| 3 | Install Git on your computer |
| 4 | Clone the repository locally → `git clone <clone_url>` |
| 5 | Verify the repository → `git branch`, `git status` |
| 6 | If everything matches → it will show "Nothing to commit" |
| 7 | Cut the Module and Environment folder and paste it in the right place (at the same level as README/gitignore) |
| 8 | Run `git status` again — new folders will show as "untracked" |
| 9 | To bring new files under tracking → `git add .` |
| 10 | Commit the changes → `git commit -m "new commit"` |
| 11 | Push the changes → `git push` |
| 12 | Protect the main branch with a **Protection Rule** (GitHub repo settings → branch protection policy) |

> ⚠️ **Never push directly to the main branch!**

For any new change, you always need to create a feature branch off of main:

```bash
git branch                     # shows the current branch
git checkout -b feature/new-rg # to create a new branch
```

---

## 4. Git Workflow — From Local to Remote

Code passes through 4 stages:

```
Working Directory  --(git add)-->  Staging Area
Staging Area       --(git commit -m)-->  Committed Area
Committed Area     --(git push)-->  Remote (GitHub)
```

| Stage | Command | Purpose |
|---|---|---|
| Working Directory | — | Where you edit your files |
| Staging Area | `git add .` | Prepares changes to be committed |
| Committed Area | `git commit -m "message"` | Saves changes into local repo history |
| Remote GitHub | `git push` | Sends changes to GitHub |

### Basic Local Git Commands Flow (Diagram Reference)
```
tf-bhakua-monolithic-lz (local)
   git add . → git commit → git push  →  GitHub (Remote Repo)
   git clone <clone_url>  ← ← ←  Code
```

---

## 5. CI/CD Workflow with Branch Protection (Real Example)

**Example Requirement:** Need to add a new Resource Group (RG).

### Step-by-Step Flow

1. Create a feature branch and push the changes
2. The pipeline runs automatically in GitHub Actions
3. **Scan Stage** → `tfsec`, `gitleaks`, `tflint`, `infracost`
4. **Plan Stage** → `terraform init`, `fmt`, `validate`, `plan`
5. Raise a PR from the feature branch to the main branch
6. A Human Reviewer reviews the PR
7. The reviewer checks that the feature code, scan report, and plan output are all correct
8. The human gives approval
9. The code is merged into the main branch → and the automatic pipeline runs from the main branch

### Pipeline Sequence (Golden Rule)
```
Scan → Plan → Approval (only on main) → Apply (only on main)
```

---

## 6. Best Practices — Always Remember

- ❌ Never commit directly to the main branch
- ✅ Always raise a PR from a feature branch to merge into main
- ✍️ Always write meaningful commit messages
- 🔹 Keep PRs short and focused
- 🔒 Always enable branch policy
- 👥 Keep at least 2 reviewers
- 🔑 Store secrets in **GitHub Secrets** (never in code)
- 🏷️ Always use **Tags** for production
- 🧹 Delete the feature branch after it's merged (to keep the repo clean)

---

## Quick Revision Summary

- **Git** = the core version control technology; GitHub/GitLab/BitBucket/Azure Repos are all built on top of it.
- **Workflow:** Working Dir → Staging → Commit → Push → Remote.
- **Golden Rule:** No direct push to main branch, everything goes through a PR.
- **CI/CD Pipeline:** Scan → Plan → Approval → Apply, with approval/apply happening only on the main branch.
- **Security:** Secrets go in GitHub Secrets, code review is mandatory, minimum 2 reviewers.
