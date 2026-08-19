# Credentials & Secrets Management

Controls covered in this phase:

1. Password Management
2. Multi-Factor Authentication

The following are policy sections rather than controls: there is no test-project repository or set of provisioned service credentials yet for them to be audited against. They describe the rules that will apply once that infrastructure exists, and will be converted into dated Control records (matching the format below) at that point.

3. Environment Variables and Secrets
4. Git Ignore Policy
5. Secrets Exposure Review

## 1. Password Management

**Status:** See [Control 1 — Password Management](#control-1--password-management) below.

## 2. Multi-Factor Authentication

**Status:** See [Control 2 — Multi-Factor Authentication](#control-2--multi-factor-authentication) below.

## 3. Environment Variables and Secrets

**Status:** Not yet applicable — no test-project repository exists yet to hold secrets or environment variables. Once created, secrets will be handled via `.env` files excluded from Git (never committed in source), matching Netlit's usual secret-management expectations.

## 4. Git Ignore Policy

**Status:** Not yet applicable — no test-project repository exists yet to review `.gitignore` rules against.

## 5. Secrets Exposure Review

**Status:** Not yet applicable — no test-project repository history exists yet to review for exposed secrets.

---

## Control 1 — Password Management

**Phase:** 2 — Credentials & Secrets

**Control:** 1 — Password Management

**Objective:** Ensure passwords for important development-related accounts are unique, strong, and managed via a password manager rather than reused or memorized.

**Current Status:** Open

### 1. Check
- Reviewed current password manager usage
- Confirmed: no password manager is currently configured

### 2. Understand
- Passwords for GitHub, email, Microsoft account, and future cloud/VPN/service accounts must not be reused or memorized
- A password manager is required to generate and store a unique, long, random password per account
- This control blocks the "important accounts use unique passwords" checklist item until resolved

### 3. Fix
- Not yet performed. Requires selecting and installing a password manager (e.g. Bitwarden, 1Password) and migrating credentials for the accounts listed in the Record table below.

### 4. Verify
- Not applicable — no password manager has been configured yet, so there is nothing to verify

### 5. Record
| Item | Value |
|---|---|
| Password manager configured | No |
| Candidate options | Bitwarden, 1Password |
| Accounts pending migration | GitHub, email, Microsoft account, future cloud/VPN/service accounts |
| Verification Date | August 19, 2026 |
| Status | Open action item |

---

## Control 2 — Multi-Factor Authentication

**Phase:** 2 — Credentials & Secrets

**Control:** 2 — Multi-Factor Authentication

**Objective:** Ensure MFA is enabled on important development-related accounts.

**Current Status:** In Progress

### 1. Check
- Checked MFA status on GitHub via github.com → Settings → Password and authentication
- Checked MFA status on the primary email account via its account security settings
- Cloud platform, VPN, and other company-related accounts do not exist yet — not applicable

### 2. Understand
- MFA significantly reduces the risk of account takeover if a password alone is compromised
- GitHub and email are the only priority accounts currently in active use; the rest will be provisioned as the test project starts
- MFA status on GitHub and email is not independently verifiable from a local terminal session — it was confirmed directly in each account's settings

### 3. Fix
- No fix required for GitHub or email — MFA was already enabled on both prior to this review

### 4. Verify
- Confirmed, via each account's own security settings page, that MFA is active on both GitHub and email

### 5. Record
| Item | Value |
|---|---|
| GitHub MFA | Enabled |
| Email MFA | Enabled |
| Cloud/VPN/other accounts | Not yet provisioned — N/A |
| Verification Method | Manual confirmation via account security settings (not terminal-verifiable) |
| Verification Date | August 19, 2026 |
| Status | Verified for GitHub and email; remaining accounts pending as they are provisioned |
