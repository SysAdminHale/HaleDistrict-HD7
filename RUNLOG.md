\# HaleDistrict HD7 RUNLOG

\## 2026-04-27 — Phase 0: Foundation Setup

\- Created HD7 project directory on NewThinkPad

\- Created initial HD7-Charter.md

\- Refactored charter to prioritize progress-first design

\- Introduced hybrid identity (Entra ID) as primary objective

\- Defined HD7-Core vs HD7-Extended architecture

# HaleDistrict HD7 RUNLOG

## 2026-04-27 — Phase 0: Foundation Setup + HD6 Closure Strategy

### 🧱 HD7 Foundation Established

- Created `HaleDistrict-HD7` project directory on NewThinkPad
- Created initial `HD7-Charter.md`
- Refined charter to prioritize:
  - Progress-first design philosophy
  - Simplicity over complexity ("Toyota Corolla > BMW M5")
  - Clean, stable infrastructure over experimental builds

- Introduced **Entra ID** as a new strategic objective for HD7

---

### 🗂️ Documentation & Version Control

- Created `RUNLOG.md` at project start (earlier than previous builds)
- Initialized Git repository (`git init`)
- Staged and committed initial files:
  - `HD7-Charter.md`
  - `RUNLOG.md`

- Created GitHub repository: `HaleDistrict-HD7`
- Resolved remote origin issue (`git remote set-url origin`)
- Successfully pushed initial commit to GitHub

---

### 🧠 Strategic Reflection (Major Insight)

- Identified key HD6 pain point:
  - Time lost in circular troubleshooting loops (PowerShell / WinRM issues)

- Established guiding principle for HD7:
  - **Sustained progress > technical perfection**

- Shift in mindset:
  - From preserving infrastructure → preserving knowledge
  - From complex builds → controlled, repeatable systems

---

### 🧹 HD6 Closure Planning (Pre-Execution)

- Evaluated value of exporting full HD6 VMs vs selective artifacts
- Decision made:
  - ❌ Do NOT export all VMs (low value, high storage cost)
  - ✅ Preserve high-value assets only:
    - Scripts (FS01 Scripts$ and HealthChecks)
    - RUNLOG and documentation
    - Optional screenshots of validated configurations

- Defined HD6 shutdown plan:
  - Backup critical artifacts to external SSD
  - Cleanly shut down all HD6 VMs
  - Remove HD6 VMs from Hyper-V
  - Retain only Golden Images where applicable

---

### 🎯 Plan for Next Session (2026-04-28)

- Export high-value HD6 artifacts (scripts, docs)
- Perform Hyper-V cleanup:
  - Remove HD6 VMs
  - Reclaim disk space

- Establish fully clean environment
- Begin **HD7 Phase 1: Core Infrastructure Build (DC01-first approach)**

---

### 💬 Notes

- Strong alignment between process discipline and motivation
- Early Git + RUNLOG setup expected to prevent HD6-style drift
- Clear excitement and direction heading into HD7 build

### 2026-04-28 — HD7 Phase 0: Environment Reset & Clean Slate Achieved

- Completed full teardown and cleanup of HD6 Hyper-V environment on NewThinkPad
- Deleted all HD6 VMs from Hyper-V Manager (DC01, FS01, RT01, ADM01, TEACH01, STUD01)
- Removed all associated differencing disks from C:\HyperV\Disks
- Verified C:\HyperV\Disks is empty (no orphaned VHDX/AVHDX files remaining)

- Archived high-value HD6 artifacts to external SSD (T7:\HaleDistrict_Archive\HD6.0):
  - Docs:
    - HD6-Architecture.md
    - HD6-Charter.md
    - HD6-FS01.md
  - RUNLOG:
    - RUNLOG.md (complete historical build record)
    - README.md
  - Scripts:
    - Full Scripts directory structure preserved (Baseline, Config, Features, HealthChecks, Lib, etc.)
    - Key retained assets:
      - HD6-HealthCheck-\* scripts (Core, DC01, FS01, Client, All)
      - HD6-Run-HealthCheck-\* wrappers
      - Lib\HD-Common.ps1 (shared function library)

- Made intentional decision NOT to export full HD6 VMs:
  - Determined that VM exports provide low long-term value relative to storage cost
  - Identified scripts, documentation, and architecture decisions as the highest-value artifacts
  - Adopted "artifact-first, not VM-first" preservation strategy going forward

- Cleaned Hyper-V Virtual Switch configuration:
  - Deleted all HD6-specific switches:
    - HD6-Students
    - HD6-Teachers
    - HD6-Admin
    - HD6-Internal
  - Retained only:
    - Default Switch (Hyper-V NAT + DHCP)

- Confirmed no custom virtual switches exist post-cleanup
- Confirmed MAC Address Range left unchanged (Hyper-V managed, no action required)

- Established HD7 networking philosophy:
  - Phase 1 will use Default Switch ONLY
  - No custom switches, VLANs, or routing infrastructure at initial build stage
  - RT01 explicitly deferred to later phase (only if needed)
  - FS01 explicitly deferred to later phase (only if needed)

- Reinforced HD7 core design principle:
  - "Collapse variables EARLY"
  - Remove unnecessary infrastructure layers to avoid compounding failure points
  - Prioritize deterministic, low-friction validation of core services (AD, DNS, WinRM)

- Initialized HD7 workspace:
  - Created C:\HaleDistrict-HD7
  - Created initial repository structure in VS Code
  - Initialized Git repository
  - Created:
    - HD7-Charter.md
    - RUNLOG.md
  - Successfully committed initial Phase 0 baseline

- Successfully connected local repo to GitHub:
  - Repo: https://github.com/SysAdminHale/HaleDistrict-HD7
  - Resolved remote origin misconfiguration
  - Pushed initial commit to main branch

- Current state:
  - Hyper-V environment is fully clean
  - Storage environment is clean and validated
  - Archive strategy implemented and tested
  - Git + GitHub workflow operational
  - HD7 design philosophy clarified and simplified

- Next step (afternoon session):
  - Build HD7-DC01 using Default Switch
  - Begin Phase 1: Core Domain (DC01 → ADM01 → TEACH01)
  - Validate domain creation, DNS, and authentication before introducing any additional infrastructure

### 2026-04-29 — HD7 Phase 1 Start: DC01 Build + Domain Promotion (SUCCESS)

- Began HD7 build from a fully cleaned Hyper-V environment (no prior VMs, only Default Switch present)
- Reinforced HD7 design philosophy:
  - Collapse variables early
  - Prefer simplicity over speed
  - Avoid premature infrastructure (no RT01, no FS01)
  - Validate before layering additional services

- Decision point: ISO vs Golden Image
  - Chose fresh Windows Server ISO instead of GOLD image
  - Rationale: eliminate hidden variables (network config, SID history, roles)
  - Establish clean, known baseline for HD7

- Created VM: HD7-DC01
  - Generation 2
  - 4GB RAM (static)
  - Default Switch networking (DHCP)
  - New VHDX (no differencing disk)
  - Automatic checkpoints disabled (critical for stability)
  - Adjusted CPU from 8 → 2 vCPUs post-install to reduce unnecessary overhead

- Installed Windows Server (Desktop Experience)
  - Renamed system to HD7-DC01
  - Verified 64-bit PowerShell environment ([Environment]::Is64BitProcess = True)
  - Validated network connectivity via DHCP (172.25.x.x range)
  - Confirmed internet access (Test-NetConnection successful)

- Installed AD DS role via PowerShell:
  - Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
  - Verified successful installation (Get-WindowsFeature)

- Promoted HD7-DC01 to Domain Controller:
  - Created new forest: haledistrict.local
  - NetBIOS name: HALEDISTRICT
  - Installed DNS as part of promotion
  - Acknowledged expected warnings:
    - No static IP (intentional at this stage)
    - DNS delegation not created (normal in isolated lab)
  - Promotion completed successfully with automatic reboot

- Post-promotion validation:
  - whoami → haledistrict\administrator (confirmed domain context)
  - Hostname → HD7-DC01
  - DNS servers correctly set to loopback (::1 and 127.0.0.1)
  - DHCP still enabled (intentional; static conversion deferred)

- Key HD7 workflow reinforcement:
  - Validate → THEN build (replacing HD6 “build then troubleshoot” pattern)
  - Delay static IP assignment until AFTER AD + DNS are operational
  - Avoid introducing unnecessary variables early (networking, routing, multi-server dependencies)

- Status at pause:
  - Fully functional Domain Controller (AD DS + DNS)
  - Clean, stable core domain established
  - Ready next step: convert DC01 to static IP and validate DNS resolution

- Overall assessment:
  - Cleanest DC build to date
  - No troubleshooting loops encountered
  - HD7 approach validated as faster and more reliable than HD6 methodology

### 2026-05-01 – HD7 Build Session (Morning)

- Resumed HD7 build with HD7-DC01 online and functioning
- Identified DNS inconsistency caused by stale A record (172.25.45.238) from previous network configuration
- Removed stale DNS records from haledistrict.local zone
- Flushed DNS cache and re-registered DNS records
- Verified correct DC IP (172.19.45.107) as sole authoritative record
- Confirmed successful name resolution via nslookup
- Executed dcdiag and confirmed overall health with one expected DNS warning (forwarder/root hint behavior)

- Began workstation deployment: HD7-TEACH01
- Created VM using GOLD-WIN11-BUILD image
- Initially attached GOLD image directly (identified as incorrect approach)
- Recovered by:
  - Removing GOLD disk from VM
  - Creating proper differencing disk (HD7-TEACH01.vhdx)
  - Reattaching child disk to VM

- Encountered UEFI boot failure:
  - "No operating system loaded"
  - PXE boot attempts observed
- Root cause: Boot order lost after disk change (Firmware configuration issue)

- Resolved by:
  - Navigating to VM Settings → Firmware
  - Moving Hard Drive to top of boot order
  - Confirming Secure Boot enabled (Microsoft Windows template)

- Successfully booted into Windows 11 OOBE environment
- Confirmed system operational (PowerShell accessible, desktop loaded)

### Key Lessons / Reinforcements

- Golden images must NEVER be directly attached to running VMs
- Differencing disks are required for all child systems
- UEFI boot order must be manually corrected after disk changes
- DNS hygiene is critical—stale records can silently break domain functionality
- Recovery process is part of the build—not a failure

### Current State

- HD7-DC01: Healthy, DNS functional
- HD7-TEACH01: Successfully built and booted (ready for OOBE + domain join)

### Next Steps (Afternoon)

- Complete OOBE for TEACH01
- Rename machine to HD7-TEACH01 (if not already set)
- Configure DNS to point to DC01
- Join domain (haledistrict.local)
- Validate domain connectivity (ping, nslookup, gpresult)
- Begin STUD01 build (repeat clean process)

### 2026-05-01 — HD7 Phase 1 (Workstation + Loopback GPO SUCCESS)

- Restarted HD7 build with simplified architecture (no RT01 / FS01 for now; Default Switch only)
- Created HD7-TEACH01 using differencing disk from GOLD image
- Identified and resolved issue from accidental GOLD boot:
  - Deleted contaminated differencing disk
  - Recreated clean child disk
  - Preserved GOLD integrity (critical lesson reinforced: never boot GOLD)
- Encountered boot failure (“No OS loaded / PXE boot”)
  - Root cause: firmware / Secure Boot mismatch
  - Fix: corrected firmware boot configuration → system booted successfully
- Completed initial Windows boot and verified local admin access

### Networking + Domain Join

- Identified NIC alias mismatch (Ethernet vs Ethernet 2)
- Resolved DNS configuration:
  - Set DNS server to DC01 (172.19.45.107)
  - Required elevated PowerShell (fixed permission/CIM error)
- Verified connectivity:
  - ipconfig /all confirmed correct DNS
  - nslookup behavior understood (timeout nuance discussed)
- Successfully joined haledistrict.local
- Validated domain trust:
  - whoami → haledistrict\administrator
  - $env:LOGONSERVER → \\HD7-DC01
  - nltest /dsgetdc successful

### OU Structure + Computer Placement

- Created / validated OU structure:
  - HD7-Workstations
    - Teachers
    - Students
- Moved HD7-TEACH01 into:
  - OU=Teachers,OU=HD7-Workstations
- Verified placement via ADUC and gpresult

### Loopback GPO Deployment (CORE MILESTONE)

- Created GPO: HD7-GPO-Teachers-Baseline
- Linked to: OU=Teachers
- Enabled:
  - Loopback Processing Mode = Replace
- Configured User Policy:
  - Prohibit access to Control Panel and PC settings

### Validation (CRITICAL SUCCESS)

- Ran:
  gpupdate /force
  gpresult /r
- Confirmed:
  - GPO applied under USER SETTINGS
  - Source = HD7-DC01
- Observed behavior:
  - Control Panel access blocked with restriction message

### Key Breakthrough (Conceptual)

- Successfully demonstrated:
  - Loopback (Replace) forces USER policy based on COMPUTER OU
- Validated scenario:
  - Domain Admin user (normally unrestricted)
  - Logged into Teacher workstation
  - Still received restricted experience

### Core Principle Learned

- WITHOUT loopback → user OU controls user experience
- WITH loopback (Replace) → computer OU controls user experience

### HD7 Architectural Progress

- Established foundation for:
  - Classroom / lab-style workstation control
  - Role-based environments (Teachers vs Students)
  - Future pilot rings and layered GPO strategy
- Confirmed ability to:
  - Build clean VMs from GOLD safely
  - Diagnose boot + firmware issues
  - Configure DNS and domain join correctly
  - Deploy and validate GPOs with precision

### Next Steps (Next Session)

- Expand Teacher GPO:
  - Add additional user restrictions (Run menu, CMD, etc.)
- Create Student workstation GPO (separate experience)
- Begin pilot ring strategy (HD7 refinement over HD6)
- Introduce drive mapping + file services later in build (intentionally deferred)

### Reflection

- Multiple early friction points (disk contamination, boot failure, DNS errors) resolved methodically
- Strong validation-first workflow maintained throughout
- First true end-to-end success of HD7:
  - VM → Domain → OU → GPO → Enforcement → Validation

### 2026-05-03 (Morning Session) — Phase 1 (Core Network + GPO Baseline Stabilization)

---

## OBJECTIVE

Stabilize HD7 core infrastructure by:

- Fixing domain connectivity issues between TEACH01 and DC01
- Eliminating unreliable Default Switch/DHCP behavior
- Establishing deterministic internal networking (10.0.0.0/24)
- Validating GPO application in a clean, controlled environment

---

## INITIAL STATE / PROBLEM

TEACH01 lost connectivity to the domain:

- gpupdate /force failed with “lack of network connectivity to a domain controller”
- ping to DC01 (172.19.x.x) returned “Destination net unreachable”
- TEACH01 receiving DHCP address (172.17.x.x) from Hyper-V Default Switch
- DNS server incorrectly pointing to external/non-domain address
- Result: Domain trust path effectively broken

Root Cause:

- Reliance on Hyper-V Default Switch (NAT/DHCP) introduced non-deterministic networking
- No consistent subnet shared between DC01 and TEACH01
- DNS not pointing to DC01 (critical failure for AD)

---

## DECISION (OPTION B)

Move to a fully controlled internal network model:

- Use Hyper-V Internal Switch (HD7-Internal)
- Assign static IPs
- Eliminate DHCP/NAT dependency
- Align with “boring, predictable infrastructure” principle

---

## ACTIONS TAKEN

### 1. Hyper-V Network Reconfiguration

- Created / used: HD7-Internal switch
- Attached BOTH:
  - HD7-DC01
  - HD7-TEACH01
- Removed dependency on Default Switch

---

### 2. DC01 Network Configuration (Authoritative Anchor)

Configured static IP:

New-NetIPAddress -InterfaceAlias "Ethernet" `  -IPAddress 10.0.0.10`
-PrefixLength 24

Set-DnsClientServerAddress -InterfaceAlias "Ethernet" `
-ServerAddresses 127.0.0.1

Result:

- DC01 now authoritative DNS + domain endpoint
- No external dependency

---

### 3. TEACH01 Network Configuration (Domain Client)

Configured static IP:

New-NetIPAddress -InterfaceAlias "Ethernet 2" `  -IPAddress 10.0.0.50`
-PrefixLength 24 `
-DefaultGateway 10.0.0.1

Configured DNS:

Set-DnsClientServerAddress -InterfaceAlias "Ethernet 2" `
-ServerAddresses 10.0.0.10

---

### 4. Connectivity Validation

Ping Test:
ping 10.0.0.10
Result: SUCCESS (0% loss)

Domain Controller Discovery:
nltest /dsgetdc:haledistrict.local
Result: SUCCESS

- DC located: \\HD7-DC01.haledistrict.local
- Flags confirm full DC functionality (PDC, GC, DNS, etc.)

Group Policy Refresh:
gpupdate /force
Result:

- Computer Policy: SUCCESS
- User Policy: SUCCESS

---

## GPO VALIDATION (PREVIOUS + CURRENT)

Confirmed Working:

- Prohibit Control Panel → SUCCESS
- Remove Run Menu → ENABLED
- Prevent CMD → ENABLED

Behavior Observed:

- Restrictions apply correctly to scoped user
- Admin context bypass behavior understood and expected
- Logoff/logon cycle confirmed as required trigger for user policies

---

## KEY LEARNINGS

1. DNS IS EVERYTHING
   If DNS ≠ DC → Active Directory breaks

- Authentication fails
- GPO fails
- Domain discovery fails

2. DEFAULT SWITCH = UNCONTROLLED ENVIRONMENT

- DHCP introduces randomness
- NAT obscures topology
- Not suitable for deterministic lab builds

3. STATIC INTERNAL NETWORK = STABILITY

- Predictable IP space (10.0.0.0/24)
- Clear troubleshooting
- Mirrors real enterprise design

4. SUCCESSFUL DOMAIN TRIAD
   Working AD requires:

- Network connectivity ✅
- DNS resolution ✅
- Domain trust ✅

All three now confirmed functional

---

## CURRENT STATE (END OF SESSION)

Infrastructure:

- DC01: 10.0.0.10 (DNS + AD)
- TEACH01: 10.0.0.50 (domain joined)
- Network: HD7-Internal (isolated, controlled)

Validation Status:

- Ping: PASS
- DNS: PASS
- nltest: PASS
- gpupdate: PASS
- GPO enforcement: PASS

---

## KNOWN GAPS / FUTURE CONSIDERATIONS

- Default Gateway (10.0.0.1) not yet implemented (no router present)
- No internet access (intentional at this phase)
- No admin workstation (ADM01) yet
- No file services (FS01) yet
- No routing/NAT layer (RT01 deferred by design)

---

## NEXT STEPS (AFTERNOON SESSION OPTIONS)

1. Build ADM01 (recommended)
   - Introduce proper admin workstation
   - Install RSAT tools
   - Begin remote management model

2. Expand GPO Baseline
   - Continue teacher restrictions
   - Introduce student OU + differentiation

3. Establish Validation Framework
   - Formalize repeatable health checks
   - Begin HD7 HealthCheck script foundation

---

## SESSION SUMMARY

Major milestone achieved:

- Transitioned from unstable NAT-based networking → controlled internal architecture
- Restored full domain functionality
- Validated end-to-end AD + GPO pipeline

This marks the first fully stable foundation state of HD7.

---
