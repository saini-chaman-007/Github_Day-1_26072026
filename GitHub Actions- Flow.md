Bhai agar tu Terraform + Azure + GitHub Actions + Branch Protection + PR Review wala real enterprise workflow seekhna chahta hai, to main tujhe start se end tak pura flow bata raha hu. Isko ek baar practically kar lega to interview me bhi confidence aa jayega.

# Phase 1: GitHub Repository Setup

## Step 1: GitHub Account aur Repository

GitHub me login kar.

New Repository create kar:

```text
tf-bhakua-monolithic-lz
```

Options:

```text
✅ Add README
✅ Add .gitignore (Terraform)
```

Repository create.

---

## Step 2: Local Machine me Clone

```bash
git clone https://github.com/username/tf-bhakua-monolithic-lz.git
```

Repository folder me jao:

```bash
cd tf-bhakua-monolithic-lz
```

Check:

```bash
git status
git branch
```

Output:

```text
On branch main
nothing to commit
```

---

# Phase 2: Terraform Code Add

Folder structure:

```text
tf-bhakua-monolithic-lz

├── modules
│   ├── resource_group
│   ├── vnet
│   ├── subnet
│
├── environments
│   ├── dev
│   ├── test
│   └── prod
│
├── .github
│   └── workflows
│
├── README.md
```

Ab code paste kar.

Check:

```bash
git status
```

Output:

```text
modified files
```

---

# Phase 3: Staging Area

Code directly commit nahi hota.

Pehle staging me jata hai.

```bash
git add .
```

Verify:

```bash
git status
```

Output:

```text
Changes to be committed
```

---

# Phase 4: Commit

Commit matlab snapshot.

```bash
git commit -m "Initial Terraform landing zone structure"
```

Verify:

```bash
git log --oneline
```

---

# Phase 5: Push

Agar branch policy nahi lagi hai to:

```bash
git push origin main
```

Lekin enterprise me ye allowed nahi hota.

---

# Phase 6: Branch Protection

GitHub:

```text
Settings
→ Branches
→ Add Rule
```

Main branch select:

```text
main
```

Enable:

```text
✅ Require Pull Request
✅ Require Approval
✅ Require Status Check
✅ Restrict Direct Push
```

Ab koi bhi:

```bash
git push origin main
```

Karega to reject ho jayega.

---

# Real Project Scenario

Manager bolta hai:

```text
Naya Resource Group add karo
```

Kabhi bhi:

```bash
git checkout main
```

par changes nahi karne.

---

# Phase 7: Feature Branch

Latest code lo:

```bash
git checkout main
git pull origin main
```

Feature branch banao:

```bash
git checkout -b feature/add-rg
```

Check:

```bash
git branch
```

Output:

```text
* feature/add-rg
main
```

---

# Phase 8: Code Change

Example:

```hcl
module "rg" {
 source = "../../modules/resource_group"

 resource_group_name = "rg-airtel"
 location = "Central India"
}
```

---

# Phase 9: Commit

```bash
git add .
```

```bash
git commit -m "Added Airtel resource group"
```

---

# Phase 10: Push Feature Branch

```bash
git push origin feature/add-rg
```

Ab GitHub me new branch create ho jayegi.

---

# Phase 11: Pull Request

GitHub me:

```text
Compare & Pull Request
```

Click.

PR title:

```text
Added Airtel Resource Group
```

Create PR.

---

# Phase 12: GitHub Actions Automatically Run

PR create hote hi:

```text
Scan Stage
```

Tools:

```text
Checkov
TFSec
Trivy
Gitleaks
Infracost
```

Check:

```text
Security Issue?
Hardcoded Secret?
Cost Increase?
Terraform Best Practice?
```

---

# Phase 13: Terraform Validation

Pipeline next stage:

```bash
terraform fmt -check
```

```bash
terraform init
```

```bash
terraform validate
```

```bash
terraform plan
```

Plan output PR me attach ho jayega.

Example:

```text
Plan: 1 to add
0 to change
0 to destroy
```

---

# Phase 14: Reviewer

Reviewer dekhega:

```text
Terraform Code
Plan Output
Security Scan
Cost Scan
```

Agar issue mila:

```text
Changes Requested
```

Phir tu:

```bash
git checkout feature/add-rg
```

Code fix.

```bash
git add .
git commit -m "Addressed reviewer comments"
git push
```

PR automatically update ho jayega.

---

# Phase 15: Approval

Reviewer approve karega.

```text
✅ Approved
```

---

# Phase 16: Merge

GitHub:

```text
Merge Pull Request
```

Ab:

```text
feature/add-rg
↓
main
```

me merge ho gaya.

---

# Phase 17: Main Branch Pipeline

Ab actual deployment start.

Pipeline:

```text
Scan
↓
Plan
↓
Approval
↓
Apply
```

Only Main Branch:

```bash
terraform apply
```

run karega.

Feature branch kabhi deploy nahi karegi.

---

# Phase 18: Azure Deployment

Terraform create karega:

```text
RG
VNET
Subnet
NIC
VM
NSG
Storage
```

Jo bhi code me hai.

---

# Phase 19: Cleanup

Merge ke baad:

```bash
git branch -d feature/add-rg
```

Remote delete:

```bash
git push origin --delete feature/add-rg
```

---

# Pura Enterprise Flow (Interview Answer)

```text
Developer
   ↓
Feature Branch
   ↓
Code Change
   ↓
git add
   ↓
git commit
   ↓
git push
   ↓
Pull Request
   ↓
Security Scan
   ↓
Terraform Validate
   ↓
Terraform Plan
   ↓
Reviewer Approval
   ↓
Merge to Main
   ↓
Main Branch Pipeline
   ↓
Terraform Apply
   ↓
Azure Resources Created
```

Golden Rule yaad rakh:

```text
❌ Never Push Directly To Main

✅ Create Feature Branch
✅ Raise PR
✅ Get Approval
✅ Merge To Main
✅ Deploy From Main Only
```

Yahi workflow TCS, Infosys, Accenture, Ericsson, Airtel, Jio aur almost sab enterprise DevOps projects me follow hota hai.
