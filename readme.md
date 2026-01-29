# GitHub Multiple Accounts Setup on macOS (SSH + GitHub CLI)

This guide explains how to configure **multiple GitHub accounts (Personal + Office)** on **macOS** using **SSH**, and how to:
- Push a **new repository**
- Update an **existing repository**
- **Clone** repositories
- **Switch accounts** using GitHub CLI (`gh`)
- Work entirely from the **terminal / VS Code terminal**

---

## 1. Prerequisites

- macOS
- Git installed  
  ```bash
  git --version
  ```
- GitHub accounts (e.g. personal + office)
- Homebrew installed

---

## 2. SSH Keys for Multiple Accounts

### 2.1 Check existing SSH keys
```bash
ls ~/.ssh
```

### 2.2 Create SSH keys

#### Personal
```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_personal -C "personal@email.com"
```

#### Office
```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_office -C "office@email.com"
```

---

### 2.3 Add keys to SSH agent
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_personal
ssh-add ~/.ssh/id_ed25519_office
```

---

### 2.4 Add keys to GitHub
```bash
pbcopy < ~/.ssh/id_ed25519_personal.pub
pbcopy < ~/.ssh/id_ed25519_office.pub
```

GitHub → Settings → SSH and GPG keys

---

## 3. SSH Config

```bash
nano ~/.ssh/config
```

```ini
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
  IdentitiesOnly yes

Host github-office
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_office
  IdentitiesOnly yes
```

```bash
chmod 600 ~/.ssh/config
```

---

## 4. GitHub CLI (gh)

### Install
```bash
brew install gh
```

### Login
```bash
gh auth login
```

### Switch account
```bash
gh auth logout
gh auth login
```

---

## 5. Push New Repository

```bash
git init
git branch -M main
git add .
git commit -m "Initial commit"
gh repo create my-project --source=. --push
```

---

## 6. Update Existing Repository

```bash
git add .
git commit -m "Update"
git push
```

---

## 7. Clone Repository

### Personal
```bash
git clone git@github-personal:USERNAME/REPO.git
```

### Office
```bash
git clone git@github-office:ORG/REPO.git
```

---

## 8. Open in VS Code
```bash
code .
```

---

## 9. Useful Commands

```bash
ssh -T github-personal
ssh -T github-office
gh auth status
git remote -v
```
