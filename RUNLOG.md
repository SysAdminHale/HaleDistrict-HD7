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

### 2026-05-03 HD7 RUNLOG — Phase 1 ADM01 Build + Admin Control Plane Established

Date: 2026-05-02 (Evening Session)

---

## OBJECTIVE

Extend the HD7 environment by:

- Introducing a dedicated admin workstation (ADM01)
- Maintaining clean network design (HD7-Internal, static IPs)
- Validating domain connectivity and GPO processing from a non-DC endpoint
- Establishing proper separation of roles (DC vs Admin vs Client)

---

## STARTING STATE

- DC01 fully functional (10.0.0.10, DNS + AD DS)
- TEACH01 domain-joined and receiving GPOs (10.0.0.50)
- Internal network (10.0.0.0/24) stable and deterministic
- No admin workstation yet (all management occurring on DC01)

---

## ACTIONS TAKEN

### 1. Created ADM01 VM (HD7-ADM01)

- Generation 2 VM
- Windows 11 (differencing disk from golden image)
- Connected to: HD7-Internal switch
- No Default Switch attached (intentional design choice)

---

### 2. Initial Network Configuration Attempt (Failure + Learning)

Attempted:

New-NetIPAddress -InterfaceAlias "Ethernet" ...

Result:

- ERROR: Interface not found

Root Cause:

- Interface alias did not match actual adapter name
- VM used "Ethernet 3"

Key Learning:

- Never assume interface names
- Always verify using:
  Get-NetAdapter

---

### 3. Correct Network Configuration (Successful)

Configured ADM01:

New-NetIPAddress -InterfaceAlias "Ethernet 3" `  -IPAddress 10.0.0.100`
-PrefixLength 24 `
-DefaultGateway 10.0.0.1

Set-DnsClientServerAddress -InterfaceAlias "Ethernet 3" `
-ServerAddresses 10.0.0.10

Result:

- Static IP successfully applied
- DNS correctly pointing to DC01

---

### 4. Connectivity Validation

Ping Test:
ping 10.0.0.10
→ SUCCESS (0% loss)

DNS Test:
nslookup haledistrict.local
→ SUCCESS (resolved to 10.0.0.10, minor timeout observed but non-blocking)

Conclusion:

- Network and DNS functioning sufficiently for domain join

---

### 5. Domain Join (ADM01)

Command:

Add-Computer -DomainName haledistrict.local -Credential haledistrict\Administrator -Restart

Result:

- ADM01 successfully joined to domain
- Reboot completed without issue

---

### 6. Post-Join Validation

whoami
→ haledistrict\administrator

$env:LOGONSERVER
→ \\HD7-DC01

nltest /dsgetdc:haledistrict.local
→ SUCCESS

- DC located: \\HD7-DC01.haledistrict.local
- All required flags present (PDC, GC, DNS, WRITABLE, etc.)

gpupdate /force
→ SUCCESS

- Computer Policy: SUCCESS
- User Policy: SUCCESS

---

## CURRENT STATE (END OF SESSION)

Infrastructure:

- DC01: 10.0.0.10 (AD DS + DNS)
- TEACH01: 10.0.0.50 (managed endpoint)
- ADM01: 10.0.0.100 (admin workstation)

Network:

- Single subnet: 10.0.0.0/24
- Hyper-V switch: HD7-Internal
- No DHCP / no NAT / fully controlled

Validation Status:

- DC connectivity: PASS
- DNS resolution: PASS
- Domain join: PASS
- GPO processing: PASS
- Admin login: PASS

---

## ARCHITECTURAL MILESTONE

Established proper role separation:

- DC01 → Identity + DNS (infrastructure core)
- TEACH01 → Managed client (policy target)
- ADM01 → Administrative control plane

This eliminates the anti-pattern of managing from the domain controller.

---

## KEY LEARNINGS

1. Interface Alias Matters

- PowerShell networking commands require exact adapter names
- Always validate with Get-NetAdapter

2. Deterministic Networking Continues to Pay Off

- Static IP + internal switch = predictable behavior
- Troubleshooting remains simple and fast

3. Domain Validation Pattern is Now Standardized
   Core validation checklist:

- ping DC
- nslookup domain
- nltest /dsgetdc
- gpupdate /force

4. Separation of Duties Improves Design Quality

- Admin tasks no longer tied to DC01
- Environment now mirrors real enterprise workflow

---

## KNOWN GAPS / FUTURE CONSIDERATIONS

- Default Gateway (10.0.0.1) still not implemented (no router yet)
- No internet access (intentional)
- RSAT tools not yet installed on ADM01
- No FS01 (file services) yet
- No RT01 (routing/NAT) yet

---

## NEXT STEPS (NEXT SESSION)

1. Install RSAT on ADM01 (HIGH PRIORITY)
   - Active Directory Users and Computers
   - Group Policy Management
   - Begin remote administration model

2. Begin managing AD from ADM01 instead of DC01
   - Validate full remote admin workflow

3. Expand GPO Baseline
   - Introduce Student OU policies
   - Continue refinement of Teacher baseline

4. Optional:
   - Rename network adapters for consistency (e.g., "LAN")

---

## SESSION SUMMARY

Major milestone achieved:

- Successfully introduced ADM01 as dedicated admin workstation
- Maintained clean, deterministic network architecture
- Validated full domain functionality from a non-DC system

HD7 now has a complete foundational triad:
Identity (DC01) + Client (TEACH01) + Admin (ADM01)

Environment is stable, scalable, and ready for Phase 2.

---

### HD7 RUNLOG — Phase 1: ADM01 Domain Integration & RSAT Validation

**Date:** 2026-05-04
**Systems:** HD7-ADM01, HD7-DC01
**Focus:** Establish ADM01 as a fully functional domain-joined admin workstation with RSAT + GPMC

---

## OBJECTIVE

Build a stable, deterministic admin workstation (ADM01) capable of:

- Resolving and communicating with DC01
- Running RSAT tools (ADUC, GPMC)
- Serving as the control plane for HD7

---

## INITIAL STATE / PROBLEM

- ADM01 NIC was toggled between Default Switch and HD7-Internal
- Mixed DHCP + static configurations caused:
  - Incorrect IP assignments (172.x.x.x vs 10.0.0.x)
  - DNS inconsistency
  - Domain lookup failures (ERROR_NO_SUCH_DOMAIN)
- RSAT installation partially succeeded (ADUC worked, GPMC missing)
- Add-WindowsCapability failed with error `0x8024402c` (no internet path)

---

## ROOT CAUSE

1. Network ambiguity
   - Default Switch = internet access (NAT/DHCP)
   - Internal Switch = domain-only network (no internet)
   - Switching between them without resetting NIC caused config drift

2. DNS dependency
   - Domain operations require DNS = DC01 (10.0.0.10)
   - Internet operations require upstream DNS access

3. State drift
   - Residual DHCP/APIPA + static config overlap
   - Gateway and DNS inconsistencies

---

## RESOLUTION STEPS

### 1. Re-established Internet Connectivity (Default Switch)

- Switched ADM01 NIC → Default Switch
- Verified:
  - ping 8.8.8.8 ✅
  - ping google.com ✅
- Successfully installed RSAT components:
  - AD DS tools
  - Group Policy Management Tools (GPMC)

---

### 2. Returned to Domain Network (HD7-Internal)

- Switched NIC back to HD7-Internal
- Observed failure state:
  - APIPA address (169.254.x.x)
  - DNS resolution failure
  - nltest failed (ERROR_NO_SUCH_DOMAIN)

---

### 3. Clean Network Rebuild (CRITICAL FIX)

- Avoided incremental fixes
- Rebuilt NIC config cleanly:

New-NetIPAddress -InterfaceAlias "Ethernet 3" `  -IPAddress 10.0.0.100`
-PrefixLength 24 `
-DefaultGateway 10.0.0.1

Set-DnsClientServerAddress -InterfaceAlias "Ethernet 3" `
-ServerAddresses 10.0.0.10

- Result:
  - Static IP correctly applied
  - DNS correctly pointed to DC01
  - No DHCP/APIPA remnants

---

## VALIDATION (SUCCESS CRITERIA)

### Network

- ipconfig /all
  - IP: 10.0.0.100 ✅
  - Gateway: 10.0.0.1 ✅
  - DNS: 10.0.0.10 ✅

### Connectivity

- ping 10.0.0.10 → success
- ping google.com (on Default Switch only) → success

### Domain

- nltest /dsgetdc:haledistrict.local → SUCCESS
  - DC located: HD7-DC01
  - DNS resolution confirmed
  - Secure channel established

### RSAT Tools

- ADUC launches successfully
- GPMC launches successfully (gpmc.msc)

---

## CURRENT STATE (STABLE BASELINE)

ADM01 is now:

- Domain-joined and trusted
- Using deterministic static IP config
- Resolving DNS via DC01
- Running RSAT (ADUC + GPMC)
- Functioning as HD7 admin control plane

---

## KEY LESSONS (CRITICAL FOR HD7+)

1. Default Switch vs Internal Switch
   - Default = internet (required for RSAT install)
   - Internal = domain network (required for AD operations)
   - Never mix configs between them

2. DNS is everything
   - AD breaks immediately if DNS is wrong
   - Always validate DNS first

3. Clean rebuild > incremental fixes
   - State drift caused most issues
   - Rebuilding NIC config was the decisive fix

4. Validate every layer
   - IP → Ping → DNS → Domain → Tools
   - This layered validation prevented hidden issues

---

## HD7 DESIGN PRINCIPLE REINFORCED

"Prefer clean, deterministic state over clever fixes."

---

## NEXT PHASE

Phase 2: Initial GPO Framework

- Create core OUs:
  - HD7-Workstations
  - HD7-Users
- Deploy first test GPO (Block CMD)
- Validate GPO processing on client machine (TEACH01)

---

## STATUS

✅ Phase 1 COMPLETE  
✅ Stable baseline established  
🟢 Ready for controlled GPO rollout

---

### 2026-05-05 – Phase 1: User GPO Validation SUCCESS

Objective:
Validate that a user-scoped GPO applies correctly using clean OU structure and proper linkage

Actions Taken:

- Installed RSAT tools on HD7-ADM01
- Launched Group Policy Management (gpmc.msc)
- Verified domain connectivity using:
  - ping 10.0.0.10 (DC01)
  - nltest /dsgetdc:haledistrict.local
- Created and confirmed OU structure:
  - HD7-Users (for user objects)
  - HD7-Workstations (for computer objects)
- Created test user:
  - HD7-teacher01 in HD7-Users OU
- Verified TEACH01 computer object resides in HD7-Workstations OU
- Identified that HD7-GPO-Teachers-Baseline contained USER configuration (CMD block)
- Corrected GPO linkage:
  - Removed link from incorrect location (Workstations context)
  - Linked GPO to HD7-Users OU
- Ensured GPO Status = Enabled
- Logged into TEACH01 as HD7-teacher01
- Forced policy update and verified with:
  - gpupdate /force
  - gpresult /r

Validation Results:

- GPO appears under USER SETTINGS in gpresult output
- Command Prompt successfully blocked with message:
  "The command prompt has been disabled by your administrator."

Outcome:
User-scoped GPO successfully applied and enforced in a clean, predictable manner

Key Learnings:

- GPO scope is determined by where it is LINKED, not where it is stored
- USER Configuration applies based on USER object OU location
- COMPUTER Configuration applies based on COMPUTER object OU location
- Proper OU design is critical for predictable GPO behavior
- Clean separation of Users and Workstations simplifies troubleshooting significantly

Notes:

- Previous HD6 confusion around GPO scope, filtering, and loopback avoided by simplifying design in HD7
- Environment now stable and behaving deterministically

Next Steps:

- Perform controlled validation (Option A):
  - Move HD7-teacher01 out of HD7-Users OU → confirm GPO no longer applies
  - Move user back → confirm GPO reapplies
- After validation, proceed to:
  - Create and test computer-scoped GPO (HD7-Workstations)
  - Introduce Loopback Processing in a controlled, well-understood manner

Status:
SUCCESS – Core GPO user policy pipeline fully validated
