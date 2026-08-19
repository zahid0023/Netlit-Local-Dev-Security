# Recovery

Controls covered in this phase:

1. Backup
2. Recovery
3. Credential Compromise Procedure
4. Lost/Stolen Device Procedure

## 1. Backup

**Status:** See [Control 1 — Backup](#control-1--backup) below.

## 2. Recovery

**Status:** See [Control 2 — Recovery](#control-2--recovery) below.

## 3. Credential Compromise Procedure

**Status:** See [Control 3 — Credential Compromise Procedure](#control-3--credential-compromise-procedure) below.

## 4. Lost/Stolen Device Procedure

**Status:** See [Control 4 — Lost/Stolen Device Procedure](#control-4--lostslashstolen-device-procedure) below.

---

## Control 1 — Backup

**Phase:** 5 — Recovery

**Control:** 1 — Backup

**Objective:** Confirm what important development data is actually backed up right now: source code, non-source local files, and the BitLocker recovery key.

**Current Status:** In Progress

### 1. Check
```powershell
Get-WinEvent -LogName "Microsoft-Windows-BitLocker/BitLocker Management" |
    Where-Object { $_.Message -match "recovery" } |
    Select-Object TimeCreated, Id, Message
```
- Reviewed whether source code is backed up
- Reviewed whether non-source local files (notes, configuration) are backed up
- Reviewed the BitLocker Management event log for recovery-key backup confirmation

### 2. Understand
- No test-project repository exists yet, so there is currently no source code to back up — this isn't a gap, it's just not applicable yet
- No backup method exists for non-source local files (notes, configuration)
- The event log shows two `Event ID 828` entries confirming BitLocker recovery information for `C:` was backed up successfully to my Microsoft account, timestamped during the BitLocker setup in [01-computer-security.md, Control 4](01-computer-security.md#control-4--disk-encryption)

### 3. Fix
- No fix required for the recovery key — already backed up
- Not yet performed: backup method for non-source local files

### 4. Verify
- Recovery key backup confirmed via the event log entries above
- Non-source-file backup: not yet performed, nothing to verify

### 5. Record
| Item | Value |
|---|---|
| Source code backup | Not applicable — no test-project repository exists yet |
| Non-source local file backup | None configured |
| BitLocker recovery key backup | Confirmed — backed up to Microsoft account (Event ID 828, August 19, 2026) |
| Verification Date | August 19, 2026 |
| Status | Recovery key backup confirmed; non-source-file backup still an open action item |

---

## Control 2 — Recovery

**Phase:** 5 — Recovery

**Control:** 2 — Recovery

**Objective:** Confirm that what is backed up can actually be retrieved when needed, and that access to the retrieval path itself is protected.

**Current Status:** In Progress

### 1. Check
- Reviewed the retrieval path for the BitLocker recovery key: `account.microsoft.com/devices/recoverykey`, under the same Microsoft account confirmed in Control 1
- Checked whether that Microsoft account has MFA enabled

### 2. Understand
- The recovery key is only as protected as the Microsoft account it's stored under — if that account were compromised, the recovery key would be exposed along with it
- MFA on this specific Microsoft account has not yet been confirmed in this review; MFA was confirmed separately for GitHub and email in [02-credentials-secrets.md, Control 2](02-credentials-secrets.md#control-2--multi-factor-authentication), but not for the Microsoft account itself
- There is nothing else to test recovery of yet, since non-source-file backups don't exist (Control 1)

### 3. Fix
- Not yet performed — need to confirm MFA status on the Microsoft account holding the recovery key

### 4. Verify
- Not yet performed

### 5. Record
| Item | Value |
|---|---|
| Recovery key retrieval path known | Yes — account.microsoft.com/devices/recoverykey |
| MFA confirmed on that Microsoft account | Not yet confirmed |
| Non-source-file recovery tested | Not applicable — no backup exists yet |
| Verification Date | August 19, 2026 |
| Status | Retrieval path known; MFA on the Microsoft account still needs confirming |

---

## Control 3 — Credential Compromise Procedure

**Phase:** 5 — Recovery

**Control:** 3 — Credential Compromise Procedure

**Objective:** Have a concrete procedure ready to follow if a development credential (SSH key, API key, account password) is compromised.

**Current Status:** In Progress

### 1. Check
- Reviewed whether I have an actual, specific procedure ready, versus generic incident-response steps
- Reviewed whether I currently know who to contact at Netlit if this happens

### 2. Understand
- A generic "revoke, rotate, notify" checklist is only useful if the specific contact and revocation steps are already known — right now I don't yet have a named Netlit contact or an established notification channel, since the test project hasn't started
- The credentials that exist today (GitHub, email, the SSH key) are the ones this procedure would actually apply to right now

### 3. Fix
- Not yet performed — a named Netlit incident contact needs to be confirmed once the test project starts

### 4. Verify
- Not yet performed

### 5. Record

My procedure if a credential is compromised:
1. Stop using the compromised credential immediately.
2. Revoke it (GitHub: Settings → Password and authentication, or SSH key removal; email: change password and re-issue app passwords/sessions).
3. Generate a replacement credential.
4. Review account activity/access logs where available (e.g. GitHub security log).
5. Notify the Netlit contact — **not yet established**.
6. Investigate how the credential was likely exposed before treating it as resolved.

| Item | Value |
|---|---|
| Procedure documented | Yes (above) |
| Applies to credentials that exist today | GitHub account, email account, SSH key |
| Netlit incident contact established | No — pending test-project start |
| Verification Date | August 19, 2026 |
| Status | Procedure documented; Netlit contact still needs to be established |

---

## Control 4 — Lost/Stolen Device Procedure

**Phase:** 5 — Recovery

**Control:** 4 — Lost/Stolen Device Procedure

**Objective:** Have a concrete procedure ready to follow if this development device is lost or stolen, and confirm what protection is already in place before that happens.

**Current Status:** In Progress

### 1. Check
- Reviewed what protection is already active on this device if it were lost or stolen today
- Reviewed whether I currently know who to contact at Netlit for this scenario

### 2. Understand
- BitLocker full-disk encryption is already enabled and verified ([01-computer-security.md, Control 4](01-computer-security.md#control-4--disk-encryption)) — this is the main protection that's actually in place right now, since it prevents offline access to the disk contents without the recovery key
- Windows account access still requires my Windows password, and the built-in Administrator account is disabled ([01-computer-security.md, Control 7](01-computer-security.md#control-7--windows-account))
- As with Control 3, there is no established Netlit contact yet for this scenario, since the test project hasn't started

### 3. Fix
- Not yet performed — a named Netlit incident contact needs to be confirmed once the test project starts

### 4. Verify
- Disk encryption status re-confirmed as already verified in 01-computer-security.md
- Netlit contact: not yet established

### 5. Record

My procedure if this device is lost or stolen:
1. Notify Netlit immediately if company data or access is involved — contact **not yet established**.
2. Revoke active sessions on GitHub and email where possible.
3. Revoke the SSH key's access on GitHub (remove the registered key).
4. Report the device loss (to Microsoft, for device tracking/remote actions where applicable).
5. Rely on BitLocker encryption to prevent offline access to disk contents without the recovery key.

| Item | Value |
|---|---|
| Disk encryption active (mitigates data exposure) | Yes — verified in 01-computer-security.md, Control 4 |
| Built-in Administrator account | Disabled |
| Procedure documented | Yes (above) |
| Netlit incident contact established | No — pending test-project start |
| Verification Date | August 19, 2026 |
| Status | Disk encryption already mitigates the main risk; procedure documented; Netlit contact still needs to be established |
