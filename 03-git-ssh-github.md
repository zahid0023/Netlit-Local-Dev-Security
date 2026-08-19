# Git, SSH & GitHub Security

Controls covered in this phase:

1. Git Security
2. SSH Security
3. GitHub Account Security

## 1. Git Security

**Status:** See [Control 1 — Git Security](#control-1--git-security) below.

## 2. SSH Security

**Status:** See [Control 2 — SSH Security](#control-2--ssh-security) below.

## 3. GitHub Account Security

**Status:** See [Control 3 — GitHub Account Security](#control-3--github-account-security) below.

---

## Control 1 — Git Security

**Phase:** 3 — Git, SSH & GitHub

**Control:** 1 — Git Security

**Objective:** Ensure Git identity and credential handling are configured securely, and that repository-level secret-handling rules are ready to apply once a project repository exists.

**Current Status:** In Progress

### 1. Check
```powershell
git config --list --show-origin | Select-String "credential|signingkey|gpgsign|user\."
```
- Checked global Git identity configuration
- Checked the effective credential helper across all config scopes
- Checked for commit-signing configuration

### 2. Understand
- `user.name` and `user.email` identify the author of commits
- `user.name` is set to `root`, which is my own chosen Git username — not a leftover default
- The credential helper determines how GitHub authentication is stored; `manager` (Git Credential Manager) stores it via Windows Credential Manager rather than in plaintext
- Commit signing is optional and was not found configured
- Rules such as "credentials/private keys must never be committed" and "`.env` files must not be committed" cannot be checked against an actual repository yet, since no test-project repository exists

### 3. Fix
- No fix required for `user.name` — confirmed intentional
- Commit signing and repository-level rules remain open, pending a test-project repository

### 4. Verify
- `user.name` confirmed intentional — no further action needed
- Commit signing and repository-level rules: not yet applicable

### 5. Record
| Item | Value |
|---|---|
| user.name | `root` (confirmed intentional) |
| user.email | zahid.chowdhury023@gmail.com |
| Credential helper | manager (Git Credential Manager) |
| Commit signing configured | No |
| Repository-level rules (no committed secrets, `.gitignore`, least privilege) | Not yet applicable — no test-project repository exists |
| Verification Date | August 19, 2026 |
| Status | Identity confirmed; repository-level rules pending |

---

## Control 2 — SSH Security

**Phase:** 3 — Git, SSH & GitHub

**Control:** 2 — SSH Security

**Objective:** Ensure the SSH key used for Git/GitHub access is a modern key type and is appropriately protected.

**Current Status:** Accepted Risk — private key has no passphrase, by choice

### 1. Check
```powershell
Get-ChildItem $HOME\.ssh
```
| File | Result |
|---|---|
| `id_ed25519` | Present |
| `id_ed25519.pub` | Present |
| `known_hosts` | Present |

```powershell
ssh-keygen -y -f "$HOME\.ssh\id_ed25519"
```
Result: the public key was extracted immediately, with no passphrase prompt.

Only the private key's encryption state was tested — its content was not viewed or exposed, and the command above only reads/derives the public key.

### 2. Understand
- `id_ed25519` is a modern Ed25519 key type, preferred over older RSA keys
- `ssh-keygen -y` succeeding without a prompt means the private key file is **not passphrase-protected** — anyone who obtains the raw key file (via theft, backup leak, malware, etc.) could use it immediately, with no second factor
- Disk encryption (BitLocker, verified in [01-computer-security.md, Control 4](01-computer-security.md#control-4--disk-encryption)) reduces this risk only while the disk is at rest; it does not protect the key once Windows is unlocked and running
- Keys registered on the GitHub side (github.com → Settings → SSH and GPG keys) are not visible from this terminal session and were reviewed directly on GitHub instead

### 3. Fix
- No fix required for key type — Ed25519 is already the recommended type
- No fix applied for the missing passphrase — raised as a finding, but I've decided not to add one to this key
- No fix required for registered GitHub keys — reviewed github.com → Settings → SSH and GPG keys directly; no obsolete or unrecognized keys found

### 4. Verify
- Passphrase: confirmed absent, and confirmed as my own decision rather than an oversight
- GitHub-side key review: confirmed on github.com → Settings → SSH and GPG keys — no obsolete or unrecognized keys present

### 5. Record
| Item | Value |
|---|---|
| Key type | Ed25519 (modern) |
| Private key present | Yes |
| known_hosts present | Yes |
| Passphrase protection | No — confirmed via `ssh-keygen -y` succeeding without a prompt. Accepted risk: I've chosen not to add one. |
| GitHub-side key review completed | Yes — no obsolete or unrecognized keys found |
| Verification Date | August 19, 2026 |
| Status | Accepted risk — no passphrase, by choice; GitHub-side key review complete, clean |

---

## Control 3 — GitHub Account Security

**Phase:** 3 — Git, SSH & GitHub

**Control:** 3 — GitHub Account Security

**Objective:** Ensure the GitHub account used for development is protected by MFA and has no obsolete credentials or unrecognized access.

**Current Status:** In Progress

### 1. Check
- Checked MFA status via github.com → Settings → Password and authentication
- Checked registered SSH keys via github.com → Settings → SSH and GPG keys
- Password strength/uniqueness, authorized OAuth applications, and active personal access tokens were not yet reviewed

### 2. Understand
- MFA substantially reduces the risk of account takeover
- Password strength depends on [Control 1 — Password Management](02-credentials-secrets.md#control-1--password-management) in the credentials document, which is still open
- OAuth apps and access tokens registered on GitHub are only visible on github.com/settings and must be reviewed there directly

### 3. Fix
- No fix required for MFA — already enabled prior to this review
- No fix required for SSH keys — no obsolete or unrecognized keys found on GitHub
- Remaining items (password strength, OAuth apps, tokens) not yet actioned

### 4. Verify
- Confirmed via github.com → Settings → Password and authentication that MFA is active
- Confirmed via github.com → Settings → SSH and GPG keys that no obsolete or unrecognized keys are registered

### 5. Record
| Item | Value |
|---|---|
| MFA | Enabled |
| Password strength/uniqueness | Pending — depends on password manager setup |
| SSH keys reviewed | Yes — no obsolete or unrecognized keys found |
| OAuth apps reviewed | Pending |
| Access tokens reviewed | Pending |
| Verification Date | August 19, 2026 |
| Status | MFA and SSH keys verified; OAuth apps and tokens pending manual review |
