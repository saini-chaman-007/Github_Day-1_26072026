# Git Complete Notes (Git Basics 1 & 2)

> **Learning Mantra:** 30% technology aur 70% "explain karne ka tarika". Sirf jaan lena kaafi nahi hai — jab tak tum kisi aur ko simple tareeke se samjha nahi paate, tab tak concept clear nahi hua. Isliye ye notes is tarah likhe gaye hain ki tum khud padho aur dusro ko bhi sikha sako.

---

## 1. Why Git? (Git ki zaroorat kyu padi)

Pehle ke zamane me code/files manage karne ke liye **SVN, Bitkeeper** jaise tools chalte the, lekin unme kaafi dikkatein (dard) thi — jaise centralised hona, connectivity pe depend hona, single point of failure, etc.

Isi problem ko solve karne ke liye **Git** aaya:

- Git ek **technology** hai jisse tum apna code, branching strategy ke saath, kisi bhi SCM (Source Code Management) tool par रख सकते ho.
- Git hai: **Centralised aur Distributed Version Control System**.
- Code push karne se pehle ek strategy banayi jaati hai jise **Git Branching Strategy** kehte hain:
  - **Infrastructure code** ke liye → **Trunk Based Branching Strategy**
  - **Application code** ke liye → **Git Flow Branching Strategy**
  - Har project apni khud ki strategy choose kar sakta hai.

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

Version Control System hume batata hai:
- **Kaun si date ko**
- **Kitne baje**
- **Kis line me**
- **Kya change kiya gaya**

Version Control 2 tarah ka hota hai:

### A) Centralised Version Control (CVCS)
- Sabhi users ek **common/central server** se connect hote hain (main.tf, variable.tf, provider.tf, terraform.tfvars files central location par hoti hain).
- **Problems:**
  - **Single Point of Failure** — agar central server down ho gaya to sabka kaam ruk jaata hai.
  - **Connectivity zaroori hai** — Internet/network down hote hi kaam nahi ho sakta.

### B) Distributed Version Control (DVCS) — Example: **Git**
- Har user ke paas apni **local copy** hoti hai (poora repository — history samet).
- User1 aur User2 dono ke paas independently `main.tf`, `variable.tf`, `provider.tf`, `terraform.tfvars` + version history (`version1`, `version2`) hoti hai.
- Koi single point of failure nahi hota — sabke paas apna khud ka complete local repo hota hai.

---

## 3. Git kya hai (Definition)

> **Git is a Distributed Version Control System.** Ye teams ke collaboration ko aasan banata hai, version history track karta hai, rollback/branching allow karta hai, aur smooth development & deployment enable karta hai.

Kisi bhi folder ki files me hone wale changes track karwane ke liye us folder ko **git repository** banana padta hai.

```bash
git init
```
- `git init` command se hum kisi bhi folder ko "git" bulate hain (activate karte hain).
- Jaise hi git us folder me aata hai, ek hidden **`.git`** folder ban jaata hai — yahi folder us directory ke saare changes ko manage/track karta hai.

Local repository (`.git`) ko aage remote SCM tools se connect kiya ja sakta hai:
- GitHub
- Azure Repos
- Bitbucket
- (aur jaise Photos → Google Photos backup hoti hai, waise hi local repo → remote repo backup/sync hoti hai)

---

## 4. Git ke 3 Areas (Architecture)

Git me code 3 stages/areas se hoke guzarta hai:

```
Working Area  --(git add <filename>)-->  Staging Area  --(git commit -m "message")-->  Committed Area (Local Repo)
```

| Area | Matlab |
|---|---|
| **Working Area** | Jahan tum apni files edit/create karte ho (abhi tak track nahi ho rahi) |
| **Staging Area** | `git add` karne ke baad files yahan aati hain — ye batata hai ki agla commit me kaunsi files jaayengi |
| **Committed Area (Local Repo)** | `git commit` karne ke baad ek permanent **version/snapshot** ban jaata hai (jaise "Version 1 - Rg code done") |

**Example:** Working area me files hain — `bittu.tf`, `dhondhu.tf`, `main.tf`, `pinki.tf`, `provider.tf`, `terraform.tfvars`, `tinku.tf`, `variables.tf`
→ `git add bittu.tf` → sirf `bittu.tf` Staging Area me chali jaati hai
→ `git commit -m "added code"` → wo file Committed Area (local repo) me permanently save ho jaati hai.

---

## 5. Repository Management — Basic Commands

| Command | Kaam |
|---|---|
| `git init` | Folder ko git repository banata hai (local repo create) |
| `git clone <url>` | Remote repository ko local machine par copy karta hai |
| `git status` | Batata hai kitni files **Working Area** me hain aur kitni **Staging Area** me hain |
| `git add <filename>` | File ko Working Area se Staging Area me le jaata hai (`git add .` = sab files) |
| `git commit -m "message"` | Staged files ko permanently Local Repo me save karta hai (ek version create hota hai) |
| `git log` | Committed area me hue saare commits/changes dikhata hai (history) |
| `git show <commit-id>` | Kisi specific commit ki poori details dikhata hai |
| `git restore` | Files ko wapas purani state me le aata hai |
| `git push` | Local commits ko remote repo (GitHub/Azure Repos/Bitbucket) par bhejta hai |
| `git pull` | Remote repo ke latest changes ko local me le aata hai |

---

## 6. Commit History aur Branch

**Commit History** — har commit ka ek unique ID (hash) hota hai, jaise:
```
f2180acea9bd1b1f85b075642da7c5403f35fa87   → Initial commit
5d8071e8d7e749b785f84770fe7eac6cf4e9c1ee   → added landing zone code
```

### Branch kya hota hai?
Branch ek **parallel line of development** hoti hai — taaki main code ko chede bina naya kaam kiya ja sake.

Common branch names: `main`/`master`, `feature`, `release`, etc.

### Branch se Related Commands
| Command | Kaam |
|---|---|
| `git branch <name>` | Naya branch banata hai |
| `git checkout <branch>` | Branch switch karta hai (purana style) |
| `git checkout -b <branch>` | Naya branch banata hai **aur** turant switch bhi kar deta hai |
| `git switch <branch>` | Branch switch karne ka modern command |
| `git stash` | Abhi ke uncommitted changes ko temporarily "side me rakh dena" |
| `git tag` | Kisi commit ko naam/label dena (jaise version release: v1.0) |
| `git history` | Commits/changes ki history dekhna |
| `git cherry-pick <commit-id>` | Kisi doosre branch ke ek specific commit ko current branch me le aana |
| `git merge` | Ek branch ke changes ko doosre branch me milana |
| **Merge Conflicts** | Jab dono branches ne ek hi line/file me alag-alag changes kiye ho, tab Git khud decide nahi kar paata — manually resolve karna padta hai |

---

## 7. Pull Requests aur Branch Protection

> **"Nadi kinare saanp hai, aur main branch me directly push karna maha paap hai!"**
> (Matlab: seedha `main` branch me push karna risky hai — hamesha bachke rehna chahiye)

### Kyu zaroori hai Branch Protection?
Jab risk zyada ho, tab `main` branch ko turant **protect** kar dena chahiye:

1. **Branch Protection Rules** lagao:
   - Merge to `main` se pehle **Pull Request (PR) compulsory** karo
   - **Kam se kam 1 Approver/Reviewer** zaroori rakho

2. `main` branch se ek **feature branch** banao aur usme checkout kar lo.

3. Feature branch me apna naya code/change karo (jaise ek naya resource group tfvars me add karna).

4. Changes commit/push karo:
   ```bash
   git status
   git add .
   git commit -m "added rg in feature"
   git push
   ```

### Pull Request (PR) Process — Step by Step

1. **Feature branch** se ek pipeline chalti hai jisme automatic scanning hoti hai:
   - **Stage 1 — Scan Stage:** saare security/quality scanning tasks
   - **Stage 2 — Plan Stage:** kya-kya change hoga uska planning/preview

2. Developer apna code validate karke ek **PR (Pull Request)** raise karta hai aur ek **Reviewer** assign karta hai.

3. Reviewer ko PR ka link milta hai → Reviewer PR khol kar feature branch ki pipeline check karta hai (scan stage + plan stage ke results).

4. Agar sab kuch sahi lagta hai:
   - Reviewer code ko **compare** karke check karta hai
   - Agar changes accha lagte hain → **PR Approve** ho jaata hai → code `main` me **merge** ho jaata hai

5. **Main branch** se dubara pipeline chalti hai, lekin is baar **3 stages** hoti hain:
   - **Scan** → **Plan** → **Apply** (+ manual approval)

> **Important Design Rule (Pipeline Condition):**
> - `feature` branch se → sirf **Scan + Plan** chalta hai
> - `main` branch se → **Scan + Plan + Apply** teeno chalte hain (with manual approval)

---

## 8. Git Best Practices

- Kabhi bhi directly `main`/`master` branch me push mat karo.
- Har naye feature/change ke liye alag **feature branch** banao.
- Meaningful **commit messages** likho (jaise `"added rg in feature"`).
- `git status` frequently check karte raho.
- Merge se pehle **PR + review** ka process follow karo.
- Branch protection rules (min. 1 approver) hamesha `main` par laga kar rakho.
- Infra code ke liye **Trunk Based Branching**, application code ke liye **Git Flow Branching** use karo.

---

## 9. End-to-End Practical Workflow (Real Example)

```
1. GitHub account banao, Organization banao, Repository banao
2. Apne local machine me Git install karo
3. Repo ko clone karo:
   git clone https://github.com/devopsinsiders/azure-landing-zone-b18.git
4. main branch se feature branch banao:
   git branch feature/mcp
   git checkout feature/mcp
   (ya seedha: git checkout -b feature/mcp)
5. feature/mcp branch me naya code add karo
6. git status
7. git commit -m "added new code"
8. git push
```

Iske baad:
- Ek **Pull Request** raise karke reviewer ko assign kiya jaata hai.
- Reviewer approve karta hai → code `main` branch me **merge** ho jaata hai.
- `main` branch push status me dikhta hai jaise: *"1 commit ahead"*.

---

## 10. Quick Recap — Cheat Sheet

| Concept | One-liner |
|---|---|
| Git | Distributed Version Control System |
| `git init` | Folder ko repo banana |
| `git clone` | Remote repo copy karna |
| `git status` | Working/Staging area ka status |
| `git add` | Working → Staging |
| `git commit` | Staging → Local Repo (permanent version) |
| `git push` | Local Repo → Remote Repo |
| `git pull` | Remote → Local (latest lena) |
| `git log` | Commit history dekhna |
| `git show` | Ek specific commit ki detail |
| `git branch` | Naya branch banana |
| `git checkout -b` | Branch banao + switch karo |
| `git switch` | Branch switch karna |
| `git stash` | Changes temporarily side rakhna |
| `git merge` | Branches ko milana |
| `git cherry-pick` | Specific commit doosre branch me lana |
| Pull Request | Review ke baad main me merge karne ka process |
| Branch Protection | main ko galat/direct push se bachana |

---

## 11. Homework (Practice Tasks)

1. Ek Pull Request raise karo reviewer ke saath, aur khud usko `main` me merge karo.
2. Isi pure setup ko **AntiGravity** ki madad se **GitHub Actions pipeline** me banane ki koshish karo.
3. Pura Git flow (init → add → commit → push → PR → merge) ko kam se kam **20 baar bol kar record** karo — jab tak fluently explain na kar pao.

---

*Notes prepared from "Git Basics" session PDFs — organized for self-study aur teaching dono ke liye.*
