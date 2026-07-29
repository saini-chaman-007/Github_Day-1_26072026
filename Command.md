Bilkul bhai, pura GitHub + Terraform workflow me jo commands use hongi unka ek ready-made cheat sheet de raha hu.

# Repository Clone

```bash
git clone https://github.com/<username>/<repo-name>.git
cd <repo-name>
```

---

# Current Status Check

```bash
git status
```

---

# Current Branch Check

```bash
git branch
```

Current branch show karega:

```text
* main
```

---

# Remote Repository Check

```bash
git remote -v
```

---

# Latest Code Pull

```bash
git pull origin main
```

---

# New Feature Branch Create

```bash
git checkout -b feature/add-rg
```

Ya latest syntax:

```bash
git switch -c feature/add-rg
```

---

# Branch Switch

```bash
git checkout main
```

Ya

```bash
git switch main
```

---

# Files Add To Staging

Single file:

```bash
git add main.tf
```

All files:

```bash
git add .
```

---

# Check Staging Status

```bash
git status
```

---

# Commit

```bash
git commit -m "Added resource group module"
```

---

# Commit History

```bash
git log --oneline
```

---

# Push Feature Branch

First Time:

```bash
git push -u origin feature/add-rg
```

Next Time:

```bash
git push
```

---

# Push Main Branch (Generally Not Allowed)

```bash
git push origin main
```

---

# Fetch Latest Branches

```bash
git fetch
```

---

# Show All Branches

```bash
git branch
```

Local + Remote:

```bash
git branch -a
```

---

# Delete Local Branch

```bash
git branch -d feature/add-rg
```

Force Delete:

```bash
git branch -D feature/add-rg
```

---

# Delete Remote Branch

```bash
git push origin --delete feature/add-rg
```

---

# Merge Branch

```bash
git checkout main
git merge feature/add-rg
```

---

# Undo Staging

```bash
git reset
```

---

# Remove Specific File From Staging

```bash
git reset main.tf
```

---

# Undo Last Commit (Keep Changes)

```bash
git reset --soft HEAD~1
```

---

# Discard All Local Changes

⚠️ Dangerous

```bash
git reset --hard HEAD
```

---

# Terraform Commands Used In Pipeline

Format Check:

```bash
terraform fmt
```

Check Only:

```bash
terraform fmt -check
```

Initialize:

```bash
terraform init
```

Validate:

```bash
terraform validate
```

Plan:

```bash
terraform plan
```

Plan File Generate:

```bash
terraform plan -out=tfplan
```

Apply:

```bash
terraform apply
```

Apply Without Confirmation:

```bash
terraform apply -auto-approve
```

Destroy:

```bash
terraform destroy
```

---

# Azure Login Commands

```bash
az login
```

Subscription Check:

```bash
az account show
```

All Subscriptions:

```bash
az account list -o table
```

Change Subscription:

```bash
az account set --subscription "<subscription-id>"
```

---

# Terraform Resource Import

Jaise tera VNET wala error tha:

```bash
terraform import azurerm_virtual_network.vnet_prod "/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>"
```

---

# Daily Enterprise Workflow (Most Used)

```bash
git checkout main

git pull origin main

git checkout -b feature/add-rg

# code changes

git add .

git commit -m "Added new resource group"

git push -u origin feature/add-rg

# Raise PR in GitHub

# After Merge

git checkout main

git pull origin main

git branch -d feature/add-rg
```

Ye 90% commands hain jo tu daily DevOps project me GitHub + Terraform + Azure ke saath use karega. Isko print karke ya notes me rakh le, interview aur project dono me kaam aayega.
