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

### 2026-05-06 — Phase 2: Computer GPO Validation (AUP Banner) SUCCESS

Objective:
Validate that a COMPUTER-scoped GPO applies correctly when linked to the Workstations OU and enforces behavior prior to user logon.

Actions Taken:

- Created new GPO: HD7-GPO-Workstations-AUP-Notice
- Configured settings under:
  Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options:
  - Interactive logon: Message title for users attempting to log on
    → "HALEDISTRICT AUTHORIZED USE ONLY"
  - Interactive logon: Message text for users attempting to log on
    → "This system is for authorized use only. All activity may be monitored and recorded. Unauthorized access is prohibited."
- Verified GPO Status = Enabled
- Linked GPO to: HD7-Workstations OU
- Confirmed Security Filtering = Authenticated Users (default, appropriate for baseline)
- Ran gpupdate /force on TEACH01
- Rebooted TEACH01 using:
  shutdown /r /t 0

Validation Results:

- AUP login banner appears BEFORE credential prompt
- Title and message display exactly as configured
- Behavior consistent across reboot cycle
- Confirms COMPUTER-scoped GPO applies based on computer object OU (HD7-Workstations)

Outcome:
Computer-scoped GPO successfully applied and validated in a clean, deterministic manner.

Key Learnings:

- COMPUTER Configuration applies based on computer object location (OU linkage)
- COMPUTER GPOs apply before user logon (pre-authentication)
- USER vs COMPUTER scope separation is now clearly validated in HD7
- Proper OU design (Users vs Workstations) eliminates ambiguity in GPO processing
- Simple, single-purpose GPOs improve clarity and troubleshooting

Design Principles Reinforced:

- “Boring infrastructure” → predictable, explainable behavior
- One GPO = one purpose
- Link where it applies, not where it is stored
- Clean separation of USER and COMPUTER policy domains

Current State:

- USER GPOs validated (CMD block, Control Panel block)
- COMPUTER GPO validated (AUP login banner)
- OU structure functioning as designed
- GPO processing is deterministic and fully understood

Status:
🟢 Phase 2 COMPLETE
🟢 Core GPO model validated (User + Computer)
🟢 Ready for controlled expansion (Pilot Ring v2 or Loopback introduction)

Next Phase (Planned):

- Introduce Security Group filtering (Pilot Ring v2)
- Begin controlled, targeted GPO application strategy

### 2026-05-07 — Phase 2: Security Filtering + Group-Based Targeting SUCCESS

Objective:
Transition from OU-based GPO application to group-based targeting using Security Filtering, establishing a scalable Pilot → Production model.

Actions Taken:

- Created new security group:
  - SG-HD7-Production-Users

- Added HD7-Teacher01 to:
  - SG-HD7-Pilot-Users
  - SG-HD7-Production-Users (for expanded targeting test)

- Updated GPO: HD7-GPO-Users-ControlPanel-Block
  - Scope → Security Filtering:
    - Removed: Authenticated Users
    - Added:
      - SG-HD7-Pilot-Users
      - SG-HD7-Production-Users

- Verified Delegation configuration:
  - Authenticated Users → Read (retained for GPO visibility)
  - SG groups → Read (via Security Filtering)
  - Confirmed no unintended Apply permissions

- Forced policy update:
  - gpupdate /force

- Observed initial issue:
  - GPO filtered out (Denied - Security)

- Identified root cause:
  - User token did not include updated group membership

- Performed corrective action:
  - Full logoff/logon of HD7-Teacher01

Validation Results:

- gpresult /r confirms:
  - HD7-GPO-Teachers-Baseline → Applied
  - HD7-GPO-Users-ControlPanel-Block → Applied

- Functional validation:
  - Control Panel access blocked
  - Settings access blocked
  - Behavior consistent with GPO configuration

Key Learnings:

- Security Filtering determines WHO applies a GPO (not just where it is linked)
- Group membership changes require logoff/logon (token refresh)
- Delegation and Security Filtering serve different purposes:
  - Delegation → Read access (visibility)
  - Security Filtering → Apply permission (targeting)
- Clean separation of:
  - OU structure (scope)
  - Group membership (targeting)
    significantly improves flexibility and scalability

Outcome:

Successfully implemented a group-based GPO targeting model supporting:

- Pilot rollout (SG-HD7-Pilot-Users)
- Production rollout (SG-HD7-Production-Users)

This establishes a scalable, enterprise-aligned policy deployment pattern without reliance on OU restructuring.

Next Steps:

- Introduce second user (HD7-Teacher02)
- Validate Pilot → Production promotion workflow using group membership only
- Confirm identical policy behavior across multiple users

## 2026-05-07 Phase 2 Continued: Pilot → Production Validation

---

### 🎯 Objective

Validate that GPO targeting scales beyond pilot by introducing a production group and confirming consistent policy application across multiple users.

---

### 🛠️ Work Completed

#### 1. Production Security Group Expansion

- Created additional user:
  - `HD7-Teacher03`
- Added users to **SG-HD7-Production-Users**:
  - HD7-Teacher01
  - HD7-Teacher02
  - HD7-Teacher03

---

#### 2. GPO Targeting Model (Confirmed)

**GPO:** `HD7-GPO-Users-ControlPanel-Block`  
**Link Location:** `HD7-Users OU`

**Security Filtering:**

- SG-HD7-Pilot-Users
- SG-HD7-Production-Users

**Delegation:**

- Authenticated Users → Read (Allow)
- No unintended Apply permissions

---

#### 3. Validation Testing

**Test Method:**

- Log on as multiple users
- Run:
  gpupdate /force  
  gpresult /r

**Observed Results:**

✔ Users in Production group receive:

- `HD7-GPO-Teachers-Baseline`
- `HD7-GPO-Users-ControlPanel-Block`

✔ No filtering errors  
✔ No dependency on Pilot group  
✔ Policy behavior consistent across all tested users

---

### 🧠 Key Concepts Reinforced

#### OU vs Security Group Roles

- **OU** → Defines _scope of possible policy_
- **Security Group** → Defines _who actually receives policy_

---

#### Identity-Based Policy Control

Successfully demonstrated:

- Same OU
- Same GPO
- Different outcomes based solely on group membership

Core principle validated:

> Policy targeting is identity-driven, not location-driven.

---

#### Pilot → Production Rollout Model

Validated real-world deployment workflow:

1. Deploy to Pilot group
2. Validate behavior
3. Expand to Production group
4. Confirm consistent results

---

### ⚠️ Observations / Notes

- Group membership changes required **logoff/logon** to refresh token
- `gpresult /r` confirmed as reliable validation method
- No need to modify GPO linking during scale-out
- Existing delegation model remained correct during expansion

---

### 🧱 Current State of Environment

- Stable OU structure
- Functional GPO targeting model
- Working Pilot + Production group strategy
- Verified repeatable deployment pattern
- No known errors or inconsistencies

---

### 🚀 Next Phase (Planned)

**Transition to Device-Based Control (Loopback Processing)**

Goal:

- Introduce computer-based policy enforcement
- Control user experience based on **device**, not just identity

Planned work:

- Create loopback GPO (MERGE mode first)
- Link to `HD7-Workstations`
- Validate user behavior changes based on login location

---

### 💬 Session Summary

Short, focused session with strong conceptual clarity.  
Successfully transitioned from pilot testing to scalable production rollout.  
All validation steps confirmed expected behavior with no anomalies.

Environment is stable and ready for next phase.

## 2026-05-08 — Phase 2 Continued: Loopback Processing (MERGE + REPLACE) SUCCESS

---

### 🎯 Objective

Validate and fully understand Group Policy loopback processing modes (MERGE and REPLACE), and confirm behavior using real user + computer scenarios.

---

### 🛠️ Work Completed

#### 1. Loopback (MERGE) Implementation + Validation

- Created GPO:
  - `HD7-GPO-Workstations-Loopback-MERGE`
- Linked to:
  - `HD7-Workstations` OU
- Configured:
  - Computer Configuration → Policies → Administrative Templates → System → Group Policy
  - Enabled **"Configure user Group Policy loopback processing mode" = MERGE**

- Test Result (TEACH01):
  - User GPO: `HD7-GPO-Users-ControlPanel-Block` = BLOCK
  - Computer GPO (loopback): allowed Control Panel
  - Final behavior: **Control Panel accessible**

- Validation:
  - `gpresult /r` showed BOTH user + computer GPOs applied
  - Behavior confirmed MERGE = combine policies (computer can override)

---

#### 2. Loopback (REPLACE) Implementation + Validation

- Created GPO:
  - `HD7-GPO-Workstations-Loopback-REPLACE`
- Linked to:
  - `HD7-Workstations` OU
- Disabled:
  - `HD7-GPO-Workstations-Loopback-MERGE` (Link Disabled)

- Configured:
  - Same path as above
  - Mode = **REPLACE**

- Test Result (TEACH01 with HD7-Teacher03):
  - User GPO: **NOT applied**
  - Computer GPO: applied and enforced restrictions
  - Final behavior: **Control Panel BLOCKED**

- Validation:
  - `gpresult /r` showed:
    - `HD7-GPO-Workstations-Loopback-REPLACE` = applied
    - User GPO (`HD7-GPO-Users-ControlPanel-Block`) = NOT applied
  - System message:
    - “This operation has been cancelled due to restrictions...”

---

#### 3. Multi-User Expansion (Production Group)

- Created new user:
  - `HD7-Teacher03`
- Added users to:
  - `SG-HD7-Production-Users`
    - HD7-Teacher01
    - HD7-Teacher02
    - HD7-Teacher03

- Validated:
  - Consistent behavior across multiple users
  - Group-based targeting scales cleanly

---

### 🧠 Key Learnings

- Loopback processing fundamentally changes **policy precedence model**

#### MERGE:

- Combines user + computer policies
- Computer policies apply last → can override user

#### REPLACE:

- Ignores user-linked GPOs entirely
- Only computer-linked user settings apply

---

- GPO application depends on:
  - OU (scope)
  - Security Filtering (who)
  - Loopback (how user policies are processed)

---

- `gpresult /r` is the definitive validation tool:
  - Shows applied GPOs
  - Shows filtered GPOs
  - Confirms loopback behavior clearly

---

### 🧾 Outcome

Successfully implemented and validated:

- User-based GPO targeting (Security Groups)
- Computer-based policy override (Loopback MERGE + REPLACE)
- Clean separation of:
  - WHO gets policy (groups)
  - WHERE it applies (computers)

---

### 🚀 Current State

Phase 2 COMPLETE:

- GPO targeting model = stable and scalable
- Loopback processing = fully understood and validated
- Multi-user testing = successful

---

### 🔜 Next Steps (Phase 3)

- Validate cross-machine behavior:
  - Same user on TEACH01 (loopback) vs ADM01 (no loopback)
- Confirm:
  - Machine context drives experience (not just user)

- Expand test matrix:
  - Multiple users × multiple machines

---

### 🧭 Strategic Note

HD7 continues to follow core design principles:

- Boring, predictable infrastructure
- Validation-first approach
- Clear separation of concerns:
  - OU = scope
  - Group = targeting
  - GPO = configuration

This iteration is significantly cleaner and more deterministic than HD6.

---

## 2026-05-18 — Phase 4 COMPLETE: Administrative Boundary Separation + Clean OU Architecture

### 🎯 Objective

Validate that administrative workstations can be cleanly separated from standard workstation policy inheritance in HD7.

Primary goals:

- Prevent ADM01 from inheriting workstation UX/restriction policies
- Preserve clean workstation policy behavior for TEACH01
- Refine OU architecture for long-term scalability and future Entra alignment
- Continue validation-first HD7 methodology

---

### 🧱 Initial Architecture

Original structure:

```text
HD7-Workstations
├── Pilot
├── Production
└── Admins
```

This caused:

- ADM01 to remain inside the workstation policy inheritance tree
- ADM01 to inherit:
  - HD7-GPO-Workstations-AUP-Notice
  - HD7-GPO-Workstations-Loopback-REPLACE

Even after:

- moving ADM01 into Admins
- using Block Inheritance concepts

Reason:

```text
OU=Admins,OU=HD7-Workstations
```

was still a child of the workstation OU.

---

### 🔍 Validation Performed

Used repeated:

```powershell
gpresult /r
```

validation on:

- TEACH01
- ADM01

Observed:

#### TEACH01

Correctly received:

- Workstation AUP Notice
- Loopback REPLACE behavior

#### ADM01

Incorrectly continued receiving:

- Workstation AUP Notice
- Loopback REPLACE

This proved:

- OU inheritance path still mattered
- Admins OU placement was architecturally incorrect

---

### 🧠 Key Learning

Administrative workstations must NOT exist inside the workstation policy tree.

Clean separation of:

- user workstations
- admin infrastructure workstations

is critical for:

- predictability
- troubleshooting
- scalability
- future cloud/Entra integration

---

### 🛠 Architecture Refinement

Created new top-level OU:

```text
HD7-Admins
```

New structure:

```text
HD7-Workstations
├── Pilot
└── Production

HD7-Admins
```

Moved:

```text
HD7-ADM01
```

to:

```text
OU=HD7-Admins
```

---

### ✅ Final Validation Results

After reboot +:

```powershell
gpupdate /force
gpresult /r
```

ADM01 results:

#### COMPUTER SETTINGS

Only:

- Default Domain Policy

No longer inherited:

- AUP Notice
- Loopback REPLACE

#### USER SETTINGS

Returned:

```text
Applied Group Policy Objects
    N/A
```

This is the intended clean admin workstation state.

---

### 🧠 Strategic Learnings

#### OU placement drives inheritance boundaries

Even a "special purpose" child OU still inherits from its parent path.

---

#### Administrative workstations should be isolated infrastructure

Admin systems should:

- avoid classroom/user restrictions
- avoid loopback experiments
- remain clean management endpoints

---

#### `gpresult /r` remains the definitive truth source

Critical for:

- inheritance validation
- loopback validation
- troubleshooting
- proving final architecture behavior

---

### 🏗 HD7 Architecture Status

Current clean structure:

```text
HD7-Users

HD7-Workstations
├── Pilot
└── Production

HD7-Admins
```

This now provides:

- clean separation of concerns
- scalable workstation targeting
- isolated admin systems
- simpler troubleshooting
- Entra-friendly structure
- deterministic inheritance behavior

---

### 🚀 Phase 4 Outcome

Successfully validated:

- Workstation policy isolation
- Loopback boundary behavior
- Administrative workstation separation
- OU inheritance architecture
- Clean enterprise-style workstation design

HD7 architecture is now significantly cleaner than HD6.

---

### 🔜 Next Phase

Phase 5:

- RSAT-based administration workflows
- Remote management from ADM01
- "Admin from workstation, not from DC" operational model
- Enterprise-style management patterns
- Preparation for future hybrid/Entra concepts

---

### 🧭 Strategic Note

HD7 continues validating the core philosophy:

- Boring infrastructure
- Clean boundaries
- Deterministic behavior
- Validation-first design
- Simplicity over cleverness

This refinement represents a major architectural improvement over HD6.

---

### 2026-07-21 — Phase 5 Kickoff: Environment Resume + Domain Triad Re-Validation SUCCESS

---

## OBJECTIVE

Resume HD7 after an extended pause (Germany relocation prep) by:

- Confirming the lab environment survived the pause without drift or breakage
- Re-validating the domain triad (network → DNS → domain trust) from ADM01
- Re-confirming GPO processing pipeline still functions end-to-end
- Formally locking in Charter v2 scope decisions before resuming build work

---

## CONTEXT

HD7 paused after Phase 4 (2026-05-18) during final push on Germany relocation logistics. No changes made to the environment during the pause. This session is the first touch since then.

---

## STARTING STATE

- DC01: powered off at last check
- ADM01, TEACH01: running
- Unknown whether environment had drifted after months untouched

---

## ACTIONS TAKEN

### 1. Environment Boot Check

- Started all HD7 VMs (DC01, ADM01, TEACH01)
- Visually confirmed via Hyper-V Manager and direct console:
  - ADUC: `haledistrict.local` structure fully intact — HD7-Teacher01/02/03, SG-HD7-Pilot-Users, SG-HD7-Production-Users, OU structure (HD7-Admins, HD7-Users, HD7-Workstations) all present
  - GPMC: all 6 custom GPOs present and Enabled (Teachers-Baseline, Users-ControlPanel-Block, Workstations-AUP-Notice, Workstations-Loopback-MERGE, Workstations-Loopback-REPLACE, plus Default Domain / Default DC policies)
  - TEACH01: AUP login banner still firing correctly pre-authentication
  - TEACH01: login screen correctly defaulting to last-used user (HD7-Teacher03), consistent with prior Loopback REPLACE testing

### 2. Domain Triad Validation (from ADM01 — not DC01, per "admin from workstation" principle)

Ran standardized validation checklist from HD7-ADM01:

```powershell
ping 10.0.0.10
nltest /dsgetdc:haledistrict.local
gpupdate /force
```

---

## VALIDATION RESULTS

**Ping Test:**

- 10.0.0.10 (DC01) → SUCCESS, 0% loss, <1ms round trip

**Domain Controller Discovery:**

- `\\HD7-DC01.haledistrict.local` located at 10.0.0.10
- Flags confirmed: PDC, GC, DS, LDAP, KDC, WRITABLE, DNS_DC, DNS_DOMAIN, DNS_FOREST, CLOSE_SITE, FULL_SECRET, WS
- Command completed successfully

**Group Policy Refresh:**

- Computer Policy update: SUCCESS
- User Policy update: SUCCESS

**Overall: Domain triad fully PASS. No degradation after multi-month pause.**

---

## CHARTER UPDATE (Companion Decision)

Reviewed and finalized HD7 Charter v2 (saved alongside original, not replacing it):

- **Confirmed:** Entra ID hybrid identity is the next and final HD7-Core objective (Intune was mistakenly recalled as "first" during the pause — original April charter and now v2 both confirm Entra ID precedes Intune, since Intune requires Entra as its identity source)
- **Deferred to HD8:** Intune (device management) + FS01 (file server / healthcheck scripts) — FS01 intentionally paused for all of HD7 to preserve "one new concept at a time" discipline
- **Clarified:** No new VMs/users required for Entra ID objective — HD7-Teacher01 + HD7-TEACH01 sufficient per Charter's own success criteria. TEACH02/STUD01/STUD02 would only be needed for optional scoped-sync testing, not core validation
- **Confirmed via screenshot:** RT01 was never built in HD7 (unlike HD3–HD6) — intentional simplification per original charter's Out of Scope list, not a gap

---

## KEY LEARNINGS

1. **Deterministic infrastructure pays off over time, not just at build time**
   - Static IP + Internal Switch design meant zero config drift after months of inactivity
   - No DHCP lease expiration, no APIPA fallback, no DNS staleness — the same class of problems that plagued HD6 never had a chance to occur here

2. **"Admin from workstation, not from DC" validated as a durable habit**
   - Running the triad check from ADM01 (not DC01) reinforced the Phase 5 (2026-05-04) operational model rather than reverting to old habits under pause-induced rust

3. **Memory drift is real and worth checking against written record**
   - Recalled Intune-before-Entra ordering during the pause, which contradicted both the original Charter and basic technical dependency (Intune requires Entra ID as identity source)
   - RUNLOG + Charter as source of truth caught this before it caused wasted work

4. **A stable pause is not the same as a failed project**
   - Zero rebuild required after resuming — this is itself a validation of HD7's "boring infrastructure" design philosophy

---

## CURRENT STATE (END OF SESSION)

Infrastructure:

- HD7-DC01: 10.0.0.10 — Healthy, AD DS + DNS fully functional
- HD7-ADM01: 10.0.0.100 — Healthy, RSAT/GPMC functional, used as validation source
- HD7-TEACH01: 10.0.0.50 — Healthy, domain-joined, GPOs enforcing correctly

Directory:

- Users: HD7-Teacher01, HD7-Teacher02, HD7-Teacher03 — intact
- Groups: SG-HD7-Pilot-Users, SG-HD7-Production-Users — intact
- OUs: HD7-Admins, HD7-Users, HD7-Workstations (Pilot/Production) — intact
- GPOs: all 6 custom GPOs present, Enabled, correctly linked — intact

Documentation:

- HD7-Charter-v2.md created (original Charter preserved unchanged, per user preference)

Validation Status:

- Ping: PASS
- nltest: PASS
- gpupdate: PASS
- Visual GPO/AD inventory: PASS (no drift)

---

## NEXT STEPS

Phase 5 (Entra ID Hybrid Identity) — first real build step:

1. Entra ID tenant setup
2. Entra Connect installation and configuration
3. Identity sync validation (confirm HD7-Teacher01 appears in Entra ID)
4. Dual-environment authentication validation
5. (Optional, later) Scoped/filtered sync validation using existing SG-HD7-Production-Users group

---

## SESSION SUMMARY

First HD7 session since the Phase 4 pause (2026-05-18) for Germany relocation prep. Environment resumed with zero drift — every VM, user, group, OU, and GPO exactly as left. Domain triad re-validated clean from ADM01. Charter v2 finalized, locking in Entra-ID-first sequencing and deferring Intune + FS01 to HD8. Environment confirmed stable and ready for Phase 5 build work.

This session reinforces the core HD7 principle that has held since Phase 0: boring, deterministic infrastructure age well.

---

## STATUS

✅ Resume validation COMPLETE
✅ Domain triad re-confirmed healthy
✅ Charter v2 locked in
🟢 Ready to begin Phase 5: Entra ID hybrid identity

### 2026-07-22/23 — Phase 5 Continued: Entra ID Tenant Creation SUCCESS (+ Troubleshooting Detour)

---

## OBJECTIVE

Begin Phase 5 (Entra ID hybrid identity) by:

- Creating a dedicated, disposable Global Admin identity for the HD7 lab
- Creating a new Microsoft Entra ID tenant, fully separate from any personal or work identity
- Getting oriented in the Entra admin center (Users, Groups, Devices, Apps, Entra Connect status)

---

## ACTIONS TAKEN

### 1. Dedicated Admin Identity Created

- Created new Outlook.com account: `HaleDistrict7@outlook.com`
- Rationale: fully disposable identity, isolated from personal/work Microsoft accounts, mirrors the "admin from workstation, not from DC" separation-of-concerns principle already established in HD7

### 2. Accidental LWSD Tenant Exposure (Caught, No Action Taken)

- First attempt to reach entra.microsoft.com auto-signed in via cached SSO to **dhale@lwsd.org** — the real, live LWSD production tenant (44,073 users, 49,345 groups)
- Correctly stopped before making any changes; recognized this was not the lab tenant
- Signed out, closed tab, resumed in a private window with the correct account
- No changes made to LWSD's tenant; viewed Home dashboard only (read-only, no risk)
- **Note:** LWSD account apparently has some baseline read access to their Entra dashboard — not investigated further, not relevant to HD7, and irrelevant now given upcoming Aug 3 resignation

### 3. Entra ID Tenant Creation (SUCCESS)

- Signed into entra.microsoft.com with `HaleDistrict7@outlook.com`
- Tenant auto-created on first sign-in (no existing tenant for this identity)
- **Tenant ID:** `f8cdef31-a31e-4b4a-93e4-5f571e91255a`
- **Primary domain:** `f8cdef31-a31e-4b4a-93e4-5f571e91255a.onmicrosoft.com` (default, unrenamed — see Known Gaps)
- Confirmed via dashboard: 0 users, 0 groups, 0 devices, 0 apps (expected — brand new tenant)
- Confirmed: Microsoft Entra Connect status = Disabled, "Sync has never run" (expected — not yet configured)

### 4. Browser/Token Troubleshooting Saga

Encountered persistent "Interaction required" / AADSTS16000 errors when navigating most Entra admin center pages (Overview, Domain names via search, various blades).

**Root cause identified:** Not a tenant problem. The Entra admin center's background recommendation/personalization widgets were failing to silently refresh tokens for a brand-new MSA-based Global Admin account — occurred across multiple browsers and devices (Safari/Mac, Chrome/Mac, Chrome/Windows), which ruled out device- or browser-specific causes (initially suspected Safari ITP, then extensions — both ruled out).

**Resolution:** Clicking "Ignore" on the error dialog allowed continued use of the portal; the error was isolated to specific background widgets (notably the Home "Recommendations"/"My Feed" panel) and did not affect core tenant functionality, which worked correctly throughout (Tenant ID, Users, Groups, Devices, Apps all displayed and were navigable).

### 5. Domain Rename — Abandoned (Not a Blocker)

- Attempted to rename primary domain from the default `.onmicrosoft.com` GUID string to a readable name (e.g., `haledistricthd7.onmicrosoft.com`)
- Hit a dead-end URL (incorrect/outdated blade route `DomainsListMenuBlade`, error code 404 — an AI-assistant navigation error, not a tenant or account issue)
- **Decision: Abandoned the rename.** Cosmetic only — technically irrelevant to Entra Connect setup or hybrid identity functionality. Not worth further time investment.

---

## KEY LEARNINGS

1. **Always verify which account/tenant is active before touching any admin panel**
   - Cached SSO can silently authenticate into the wrong Microsoft tenant, especially on a device that's touched multiple Microsoft identities (personal, work, lab)
   - Private/incognito windows are cheap insurance against this and should be the default for any new lab identity work going forward

2. **"Interaction required" errors on brand-new tenants are often cosmetic, not structural**
   - New MSA-based Global Admin accounts can trip background personalization/recommendation services in the Entra admin center
   - Core tenant data and functionality (Users, Groups, Devices, Apps, Tenant ID) remained correct and accessible throughout — the errors were isolated to secondary UI widgets
   - "Ignore" is a safe response when the underlying page content is still rendering correctly beneath the error

3. **Not everything Microsoft surfaces is required**
   - The "Activate/Buy" licensing page and Microsoft Learn profile prompts were unrelated upsells/tangents, not gates blocking lab progress
   - Free tier is sufficient for Entra Connect + basic hybrid sync (the actual HD7-Core objective)

4. **Cosmetic polish (domain rename) is deferrable — don't let it block real progress**
   - Consistent with HD7's "one concept at a time" philosophy: the domain name is not part of the stated Charter success criteria and was correctly deprioritized once it started consuming disproportionate time

---

## CURRENT STATE (END OF SESSION)

Identity:

- Dedicated lab admin account created: `HaleDistrict7@outlook.com`

Entra Tenant:

- Tenant ID: `f8cdef31-a31e-4b4a-93e4-5f571e91255a`
- Primary domain: default `.onmicrosoft.com` (unrenamed — deferred indefinitely, non-blocking)
- Global Admin: HaleDistrict7@outlook.com (Dave Hale)
- Users/Groups/Devices/Apps: 0 (expected, pre-Entra Connect)
- Entra Connect: Disabled, sync never run (expected, not yet configured)

No changes made to LWSD production tenant. No purchases made. No licensing upgrades needed for next phase.

---

## NEXT STEPS

Phase 5 continued — Entra Connect installation:

1. Decide install location: DC01 vs. a dedicated sync box (open question — worth deciding based on "keep DC01 clean" precedent already set with ADM01)
2. Download and install Microsoft Entra Connect
3. Run setup wizard using Express Settings (simplest path, consistent with "Corolla not BMW" philosophy)
4. Validate initial sync: confirm HD7-Teacher01 appears in Entra ID
5. Validate dual-environment authentication

---

## SESSION SUMMARY

Productive session despite friction. Successfully created an isolated, disposable Entra ID tenant and admin identity for HD7, fully separate from LWSD and personal accounts. Navigated a real (but ultimately low-stakes) accidental sign-in into LWSD's production tenant without making any changes. Diagnosed and worked around a browser/token quirk affecting a new MSA-based Global Admin account. Made the deliberate call to abandon a cosmetic domain rename rather than let it consume further time. Tenant is confirmed live, correctly isolated, and ready for Entra Connect installation next session.

---

## STATUS

✅ Dedicated lab identity created
✅ Entra ID tenant created and verified isolated from LWSD/personal accounts
✅ Tenant oriented (Users/Groups/Devices/Apps/Connect status all reviewed)
🟡 Domain rename deferred indefinitely (non-blocking, cosmetic only)
🟢 Ready for Entra Connect installation
