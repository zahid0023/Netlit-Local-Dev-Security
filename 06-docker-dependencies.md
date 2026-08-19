# Docker, Dependencies & Browser Security

Controls covered in this phase:

1. Development Dependencies
2. Docker Security
3. Local Database Security
4. Browser Security

## 1. Development Dependencies

**Status:** See [Control 1 — Development Dependencies](#control-1--development-dependencies) below.

## 2. Docker Security

**Status:** See [Control 2 — Docker Security](#control-2--docker-security) below.

## 3. Local Database Security

**Status:** See [Control 3 — Local Database Security](#control-3--local-database-security) below.

## 4. Browser Security

**Status:** See [Control 4 — Browser Security](#control-4--browser-security) below.

---

## Control 1 — Development Dependencies

**Phase:** 6 — Docker, Dependencies & Browser

**Control:** 1 — Development Dependencies

**Objective:** Ensure development tools are on recent, maintained versions.

**Current Status:** Verified

### 1. Check
```powershell
git --version
node -v
npm -v
pnpm -v
java -version
docker --version
```

### 2. Understand
- Outdated tooling can carry unpatched vulnerabilities
- Versions should be rechecked periodically, not just once

### 3. Fix
- No fix required — all tools are on recent releases

### 4. Verify
- Re-ran the same version commands and confirmed the values below

### 5. Record
| Tool | Version |
|---|---|
| Git | 2.47.1.windows.1 |
| Node.js | v22.15.0 |
| npm | 10.9.2 |
| pnpm | 10.30.3 |
| Java (JDK) | 23.0.2 |
| Docker | 29.4.3 |
| Verification Date | August 19, 2026 |
| Status | Verified |

---

## Control 2 — Docker Security

**Phase:** 6 — Docker, Dependencies & Browser

**Control:** 2 — Docker Security

**Objective:** Ensure Docker is updated and containers aren't unnecessarily exposed (privileged mode, host mounts, published ports, secrets in images).

**Current Status:** In Progress

### 1. Check
```powershell
docker version
docker ps -a
```
Docker Desktop is installed (version 29.4.3). At the time of this check, the Docker Desktop engine was not running, so no containers were active.

### 2. Understand
- Docker version is confirmed current, but a container-level audit (privileged containers, host filesystem mounts, published ports, image trust, secrets in env vars) requires the engine to be running
- Nothing can be claimed about container configuration until that audit is actually performed

### 3. Fix
- Not yet performed — requires starting Docker Desktop first

### 4. Verify
- Not yet performed

### 5. Record
| Item | Value |
|---|---|
| Docker version | 29.4.3 |
| Engine running at check time | No |
| Container audit performed | No — blocked on engine being started |
| Verification Date | August 19, 2026 |
| Status | Docker installed and updated; container-level audit still pending |

---

## Control 3 — Local Database Security

**Phase:** 6 — Docker, Dependencies & Browser

**Control:** 3 — Local Database Security

**Objective:** Ensure local development databases are not reachable from outside the machine.

**Current Status:** In Progress

### 1. Check
```powershell
Get-NetTCPConnection -State Listen
```
- Identified `postgres` (PostgreSQL 17, Windows service `postgresql-x64-17`) listening on `0.0.0.0:5433` and `[::]:5433`
- Located configuration file: `C:\Program Files\PostgreSQL\17\data\postgresql.conf`
- Confirmed `listen_addresses = '*'` — binds to all network interfaces

### 2. Understand
- `listen_addresses = '*'` makes PostgreSQL reachable from any interface, not just the local machine
- Inbound exposure is currently reduced by the Windows Firewall (verified in [01-computer-security.md, Control 3](01-computer-security.md#control-3--windows-firewall)), but the database should not depend on the firewall alone as its only protection — this violates defense in depth
- A local development database should bind to `localhost` unless external access is explicitly required

### 3. Fix
- Changed `listen_addresses = '*'` to `listen_addresses = 'localhost'` in `postgresql.conf`
- Restarting the `postgresql-x64-17` service is required for the change to take effect, and requires an elevated (Administrator) PowerShell session:
  ```powershell
  Restart-Service -Name "postgresql-x64-17" -Force
  Get-NetTCPConnection -LocalPort 5433 | Select-Object LocalAddress, LocalPort
  ```
  Expected result after restart: only `127.0.0.1` and `::1` listed, no `0.0.0.0` or `::`.

### 4. Verify
- Configuration file change confirmed saved
- Service restart: pending — requires Administrator privileges, not yet performed

### 5. Record
| Item | Value |
|---|---|
| Service | postgresql-x64-17 (PostgreSQL 17) |
| Port | 5433 |
| Initial `listen_addresses` | `*` (all interfaces) |
| Updated `listen_addresses` | `localhost` |
| Config file | `C:\Program Files\PostgreSQL\17\data\postgresql.conf` |
| Service restart applied | Pending (requires Administrator) |
| Verification Date | August 19, 2026 |
| Status | Fix applied, restart and re-verification pending |

---

## Control 4 — Browser Security

**Phase:** 6 — Docker, Dependencies & Browser

**Control:** 4 — Browser Security

**Objective:** Ensure browsers used for development are updated, and extensions/accounts are reviewed.

**Current Status:** In Progress

### 1. Check
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*", "HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" -ErrorAction SilentlyContinue |
  Where-Object { $_.DisplayName -match "Chrome|Firefox|Edge|Brave|Opera" } |
  Select-Object DisplayName, DisplayVersion -Unique
```
Extensions installed in each browser, and separation of personal vs. work accounts, were not yet reviewed.

### 2. Understand
- All three installed browsers report current version numbers
- Extension review and personal/work account separation require manually going through each browser's settings — not derivable from the registry

### 3. Fix
- No fix required for browser versions — all current
- Not yet performed: extension review, personal/work account separation check

### 4. Verify
- Browser versions confirmed via the command above
- Extension review and account separation: not yet performed

### 5. Record
| Browser | Version |
|---|---|
| Brave | 151.1.93.136 |
| Google Chrome | 151.0.7922.169 |
| Microsoft Edge | 151.0.4129.86 |
| Extensions reviewed | Pending |
| Personal/work account separation reviewed | Pending |
| Verification Date | August 19, 2026 |
| Status | Browser versions verified; extension and account review pending |
