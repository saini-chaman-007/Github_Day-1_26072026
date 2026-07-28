# Git, GitHub & Terraform Workflow — Notes

> Yeh notes diagram/whiteboard content ko organize karke banaye gaye hain, taaki khud revise karne aur dusro ko samjhane dono me aasani ho.

---

## 1. Recap — Ab Tak Kya Sikha

### Phase 1: Manual Deployment
- Windows aur Linux VM par **one-tier app** ka deployment kiya gaya.

### Phase 2: Azure Landing Zone → Manual
Landing zone manually banayi gayi, jisme yeh components cover hue:
- Hierarchy
- Networking
- Compute
- Load Balancing
- Bastion
- Database
- NSG (Network Security Group)

### Phase 2: Azure Landing Zone → Terraform
Ab wahi landing zone **Terraform** ke through automate ki gayi, Child aur Parent Module ka use karke. Concepts cover hue:
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

**Key takeaway:** Azure landing zone ko Terraform ke through child-parent module best practices ke saath banana seekha — jisme state file ko **remote backend storage account** ke andar rakha gaya.

### Home Work
- Keyvault ka code banake laana hai.
- VM creation ke baad, Terraform aur nginx sab machines par **provisioners / custom data script** se install hone chahiye.

---

## 2. Source Code Management (Git) — Basics

### Code kya hota hai aur kaha rakha jata hai?
Common Locations (Source Code Management platforms):
- GitHub
- BitBucket
- GitLab
- AWS CodeCommit
- Azure Repos

> **Important:** Yeh sab platforms ek hi common technology par kaam karte hain — **Git**.

### Source Code Management ke Benefits
1. Version history store hoti hai
2. Code ka backup rehta hai
3. Multiple teammates ek saath kaam kar sakte hain
4. Conflict management ho paata hai
5. Rollback possible hota hai
6. Branch isolation milta hai
7. Code review ka process ho paata hai

> Jo code local machine par rakha hota hai, use remote GitHub par le jaana hota hai — aur ek proper **branching strategy** ke saath rakhna hota hai.

---

## 3. GitHub Setup — Step by Step

1. Naya GitHub account banao
2. GitHub account me login karo, aur repository create karo
3. Repository ko local machine me clone karo
4. Feature branch banao, aur code us par push karo
5. Feature branch se main branch me **Pull Request (PR)** raise karo
6. PR ko reviewer review karega aur approve karega

### Detailed Setup Flow

| Step | Action |
|---|---|
| 1 | GitHub account create karo |
| 2 | Public repository create karo (README + .gitignore enable karke) |
| 3 | Computer me Git install karo |
| 4 | Repository ko local me clone karo → `git clone <clone_url>` |
| 5 | Repository verify karo → `git branch`, `git status` |
| 6 | Agar match ho gaya → "Nothing to commit" dikhega |
| 7 | Module aur Environment folder ko cut karke sahi jagah paste karo (jaha README/gitignore hai, wahi level par) |
| 8 | `git status` dobara chalao — naye folders "untracked" dikhenge |
| 9 | Naye files ko track me laane ke liye → `git add .` |
| 10 | Commit karo → `git commit -m "new commit"` |
| 11 | Push karo → `git push` |
| 12 | Main branch ko **Protection Rule** se protect karo (GitHub repo settings → branch protection policy) |

> ⚠️ **Kabhi bhi main branch me directly push nahi karna hai!**

Naye changes ke liye hamesha ek feature branch banani padegi, main branch se:

```bash
git branch                     # current branch dikhata hai
git checkout -b feature/new-rg # naya branch banane ke liye
```

---

## 4. Git Workflow — Local se Remote Tak

Code 4 stages se guzarta hai:

```
Working Directory  --(git add)-->  Staging Area
Staging Area       --(git commit -m)-->  Committed Area
Committed Area     --(git push)-->  Remote (GitHub)
```

| Stage | Command | Kaam |
|---|---|---|
| Working Directory | — | Jaha aap file edit karte ho |
| Staging Area | `git add .` | Changes ko commit ke liye ready karna |
| Committed Area | `git commit -m "message"` | Changes ko local repo history me save karna |
| Remote GitHub | `git push` | Changes ko GitHub par bhejna |

### Basic Local Git Commands Flow (Diagram Reference)
```
tf-bhakua-monolithic-lz (local)
   git add . → git commit → git push  →  GitHub (Remote Repo)
   git clone <clone_url>  ← ← ←  Code
```

---

## 5. CI/CD Workflow with Branch Protection (Real Example)

**Example Requirement:** Ek naya Resource Group (RG) add karna hai.

### Step-by-Step Flow

1. Feature branch banao aur changes push karo
2. GitHub Action me automatically pipeline run hogi
3. **Scan Stage** → `tfsec`, `gitleaks`, `tflint`, `infracost`
4. **Plan Stage** → `terraform init`, `fmt`, `validate`, `plan`
5. Feature branch se main branch me PR raise karo
6. Human Reviewer PR ko review karega
7. Reviewer check karega ki feature code, scan report aur plan output sab sahi hai
8. Human approval dega
9. Code main branch me merge hoga → aur main branch se automatic pipeline run hogi

### Pipeline Sequence (Golden Rule)
```
Scan → Plan → Approval (sirf main par) → Apply (sirf main par)
```

---

## 6. Best Practices — Hamesha Yaad Rakhna

- ❌ Kabhi bhi directly main branch me commit mat karo
- ✅ Hamesha feature branch se PR raise karke hi main me merge karo
- ✍️ Hamesha meaningful commit messages likho
- 🔹 PR ko short aur focused rakho
- 🔒 Hamesha branch policy enable karo
- 👥 Kam se kam 2 reviewers rakho
- 🔑 Secrets ko **GitHub Secrets** me store karo (code me kabhi nahi)
- 🏷️ Production ke liye hamesha **Tags** use karo
- 🧹 Merge hone ke baad feature branch ko delete kar do (repo clean rakhne ke liye)

---

## Quick Revision Summary

- **Git** = version control ka core technology; GitHub/GitLab/BitBucket/Azure Repos sab isi par based hain.
- **Workflow:** Working Dir → Staging → Commit → Push → Remote.
- **Golden Rule:** Main branch par direct push nahi, sab kuch PR ke through.
- **CI/CD Pipeline:** Scan → Plan → Approval → Apply, dono stages sirf main branch par apply/approval hote hain.
- **Security:** Secrets GitHub Secrets me, code review mandatory, minimum 2 reviewers.
