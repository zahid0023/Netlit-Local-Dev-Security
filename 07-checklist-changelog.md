# Verification Checklist, Evidence & Change Log

## 1. Security Verification Checklist

### Operating System
- [x] Windows version documented
- [x] Windows fully updated
- [x] Microsoft Defender enabled
- [x] Real-time protection enabled
- [ ] Tamper Protection enabled *(not yet independently re-verified since Control 2)*
- [x] Firewall enabled
- [x] Secure Boot verified
- [x] TPM verified *(TPM confirmed present/ready as part of BitLocker setup; standalone `tpm.msc` check performed)*
- [x] Disk encryption verified

### Authentication
- [ ] Strong Windows password *(not independently verified)*
- [ ] Windows Hello configured where appropriate
- [ ] Automatic screen lock configured
- [ ] Password manager configured — **open action item**
- [ ] Important accounts use unique passwords *(blocked on password manager)*
- [x] MFA enabled on important accounts *(GitHub, email verified; other accounts pending as provisioned)*

### Git / GitHub
- [x] Git version verified (2.47.1)
- [ ] Git configuration reviewed (user.name/email, signing)
- [x] GitHub MFA enabled
- [x] GitHub SSH keys reviewed *(github.com/settings — no obsolete or unrecognized keys found)*
- [x] Obsolete SSH keys removed *(none found)*
- [x] SSH private key protected — *no passphrase (confirmed via `ssh-keygen -y`); accepted risk, developer's choice not to add one*
- [ ] Repository permissions reviewed
- [ ] `.gitignore` reviewed
- [ ] Repositories checked for secrets

### Network
- [x] Wi-Fi reviewed *(not applicable — no wireless adapter in use, wired Ethernet only)*
- [x] Ethernet reviewed *(Realtek Gaming GbE Family Controller, Public network category)*
- [x] Firewall verified *(see 01-computer-security.md, Control 3)*
- [x] DNS reviewed *(8.8.8.8, 1.1.1.1 — deliberate, not ISP default)*
- [x] Remote Access reviewed *(RDP disabled, WinRM stopped/manual, no SSH server installed)*
- [x] Listening ports reviewed
- [x] Unnecessary services disabled *(only OS-standard services found on all interfaces, apart from Postgres — see Docker/Dependencies document)*
- [x] VPN requirements confirmed as still pending with Netlit *(candidate providers proposed for discussion — see [04-network.md](04-network.md#6-vpn))*
- [ ] VPN configured according to Netlit requirements

### Development Environment
- [x] JDK updated (23.0.2)
- [x] Node.js updated (v22.15.0)
- [x] pnpm/npm updated (10.30.3 / 10.9.2)
- [x] Git updated (2.47.1)
- [ ] IDE updated
- [x] Docker updated (29.4.3)
- [x] Development dependencies reviewed

### Docker
- [ ] Docker configuration reviewed *(daemon was not running at review time)*
- [ ] Containers reviewed
- [ ] Published ports reviewed
- [ ] Privileged containers reviewed
- [ ] Host filesystem mounts reviewed
- [ ] Container secrets reviewed
- [ ] Trusted images used

### Local Database
- [ ] Local databases not publicly exposed — **finding: PostgreSQL was bound to `0.0.0.0:5433`; config fixed, service restart pending (requires Administrator)**

### Browser
- [x] Browser versions verified (Brave, Chrome, Edge)
- [ ] Browser extensions reviewed
- [ ] Personal/work account separation reviewed

### Data Protection
- [x] Disk encryption enabled
- [x] Recovery key securely stored *(confirmed backed up to Microsoft account via BitLocker event log, Event ID 828)*
- [ ] MFA confirmed on the Microsoft account holding the recovery key
- [ ] Secrets excluded from Git
- [ ] Non-source-file backups configured — **open action item**
- [ ] Recovery process understood *(retrieval path known; untested since no non-source backups exist yet)*

### Recovery Procedures
- [x] Credential compromise procedure documented
- [ ] Netlit incident contact established *(pending test-project start)*
- [x] Lost/stolen device procedure documented
- [x] Disk encryption confirmed as the primary mitigation already in place for device loss/theft

## 2. Verification Evidence

The following evidence can be provided to Tommy where appropriate without exposing confidential information:

- Windows Security status
- Windows Update status
- Disk encryption status
- TPM status
- Secure Boot status
- Firewall status
- Network configuration summary (Wi-Fi/Ethernet/DNS/Remote Access)
- Git version
- Docker version
- Development-tool versions
- Relevant configuration summaries
- Security checklist

Sensitive information must not be included in screenshots or documentation.

**Examples of information that must be redacted:**
- Passwords
- API keys
- Access tokens
- Private SSH keys
- Recovery keys
- Database passwords
- VPN credentials
- Session cookies
- Secret environment variables

## 3. Change Log

| Date | Change | Status |
|---|---|---|
| 2026-08-14 | Initial security-hardening document created | In Progress |
| 2026-08-19 | Windows security audit (Update, Defender, Firewall, Secure Boot, Account) | Completed |
| 2026-08-19 | Disk encryption verification (BitLocker) | Completed |
| 2026-08-19 | Development dependency versions verified (Git, Node, npm, pnpm, Java, Docker) | Completed |
| 2026-08-19 | SSH key type and presence verified (Ed25519) | Completed |
| 2026-08-19 | Finding: SSH private key `id_ed25519` has no passphrase (confirmed via `ssh-keygen -y`) | Accepted risk — developer's decision not to add one |
| 2026-08-19 | GitHub-registered SSH keys reviewed (github.com/settings) | Completed — no obsolete or unrecognized keys found |
| 2026-08-19 | GitHub MFA and email MFA verified as enabled | Completed |
| 2026-08-19 | Network/listening-port review performed | Completed |
| 2026-08-19 | Finding: PostgreSQL listening on `0.0.0.0:5433`; `postgresql.conf` fixed to `localhost` | Fix applied, service restart pending (requires Administrator) |
| 2026-08-19 | Docker container-level audit | Pending — Docker Desktop was not running at review time |
| 2026-08-19 | Password manager selection | Pending — not yet configured |
| 2026-08-19 | Backup method for non-source-code files | Pending — not yet configured |
| 2026-08-19 | VPN candidate providers proposed for Bangladesh use | Pending Netlit confirmation of requirements |
| 2026-08-19 | Browser versions verified (Brave, Chrome, Edge) | Completed |
| 2026-08-19 | Wi-Fi, Ethernet, DNS, Remote Access reviewed | Completed |
| 2026-08-19 | Document set restructured: 04-network.md split from Docker/Dependencies/Browser content, which moved to new 06-docker-dependencies.md; this checklist renumbered from 06 to 07 | Completed |
| 2026-08-19 | Finding: BitLocker recovery key backup confirmed via event log (Event ID 828, backed up to Microsoft account) | Completed |
| 2026-08-19 | 05-backup-incident-response.md replaced by 05-recovery.md (Backup, Recovery, Credential Compromise Procedure, Lost/Stolen Device Procedure), rewritten as dated audit findings rather than generic policy | Completed |
| — | MFA confirmation on the Microsoft account holding the BitLocker recovery key | Pending |
| — | Non-source-file backup method | Pending — not yet configured |
| — | Git/SSH GitHub-side audit (OAuth apps, tokens) | Pending |
| — | Secrets/`.gitignore` repository audit | Pending |
| — | Final review with Tommy | Pending |
