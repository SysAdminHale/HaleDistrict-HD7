# HD7 Charter v2 — Hybrid Identity Platform (Progress-First Design)

> **Revision note:** This is v2 of the HD7 Charter, updated 2026-07-21. The original charter (HD7-Charter.md, 2026-04-27) is preserved unchanged as the historical record. This version reflects sequencing decisions made after Phase 4 was completed and the environment paused for the Germany relocation.

## What Changed from v1

- **Confirmed:** Entra ID hybrid identity is next, before Intune (v1 already stated this — v2 just reconfirms it after some drift in memory during the pause).
- **New:** Intune is deferred to HD8, alongside the *return* of FS01 (file server + healthcheck scripts), rather than Intune alone as v1 originally planned.
- **New:** FS01 is intentionally paused for all of HD7. It was a valued part of HD5/HD6 but is not needed to prove the Entra ID objective, and reviving it now would violate the "one new concept at a time" principle.
- **Clarified:** No additional VMs or users (e.g., TEACH02, STUD01/02) are required to meet HD7-Core success criteria. Existing HD7-Teacher01 + HD7-TEACH01 are sufficient. Additional users/machines are optional *extension* work (testing scoped/filtered sync) — not a prerequisite.
- **Confirmed via screenshot (2026-07-21):** RT01 was never built in HD7, unlike HD3–HD6. This is consistent with the original charter's "Out of Scope" list and requires no corrective action — it's an intentional simplification, not a gap.

---

## Overview

HD7 represents the next evolution of the HaleDistrict environment, transitioning from the complex, fully on-premises infrastructure of HD6 to a simplified, modern hybrid identity platform.

HD7 is intentionally designed to prioritize **continuous progress, reliability, and clarity of learning** over architectural ambition.

The primary objective is to introduce and validate **hybrid identity using Active Directory and Entra ID**, while avoiding the complexity traps encountered in HD6.

---

## Core Philosophy

HD7 follows a strict guiding principle:

> **"Build the Toyota Corolla, not the BMW M5."**

This means:

* Only one major new concept is introduced at a time
* Infrastructure is intentionally simplified
* Each phase must result in a working, validated system
* Progress is prioritized over perfection

---

## Primary Objective

> **Successfully implement and understand hybrid identity between Active Directory and Entra ID**

All design decisions must support this objective.

---

## Current State (as of 2026-07-21)

HD7-Core is substantially complete:

* HD7-DC01 (AD DS + DNS) — built, validated, currently powered off (needs restart before next session)
* HD7-ADM01 (management workstation, RSAT + GPMC) — built, validated
* HD7-TEACH01 (client) — built, validated, domain-joined
* OU architecture — validated (`HD7-Users`, `HD7-Workstations` [Pilot/Production], `HD7-Admins`)
* Security-group-based GPO targeting — validated (Pilot → Production model)
* Loopback processing (MERGE + REPLACE) — validated
* Administrative workstation isolation — validated

**Remaining HD7 objective:** Entra ID hybrid identity (Entra Connect sync + dual-environment authentication).

**No new VMs or users required** to meet this objective — HD7-Teacher01 and HD7-TEACH01 are sufficient to validate sync and dual-auth.

---

## HD7 Architecture Strategy

HD7 is divided into two layers:

---

### HD7-Core (Phase 1–5) — Simple, Fast, Reliable

Focus:

* Minimal infrastructure
* Reliable remoting baseline
* Entra ID integration
* Fast validation cycles

#### Core Components

* HD7-DC01 (AD DS + DNS)
* HD7-ADM01 (management workstation)
* HD7-TEACH01 (single client)
* Entra ID tenant

#### Key Requirements

* WinRM baseline applied consistently on all machines
* Remoting validated early:
  * ADM01 → DC01 = PASS
  * ADM01 → TEACH01 = PASS
* DNS clean and reliable from day one
* No VLAN or multi-subnet complexity initially

---

### HD7-Extended (Optional, within HD7) — Controlled Expansion of Entra Scope

These are optional refinements to the Entra objective, not prerequisites:

* Scoped/filtered sync validation (e.g., syncing only `SG-HD7-Production-Users`)
* Additional test users/machines (TEACH02, STUD01/02) — only if testing filtered sync specifically

---

### Deferred to HD8 — Device Management + File Services Return

These capabilities remain part of the long-term HaleDistrict vision but are deliberately deferred until HD7's Entra ID objective is complete:

* **Intune** — device management, compliance policy, cloud-native policy evolution (was already slated for HD8 in v1)
* **FS01** — file server + healthcheck script system, returning alongside Intune rather than reviving mid-HD7
* VLAN-based multi-school segmentation (HS / MS / ES)
* Advanced GPO structures
* Distributed client health check system (GPO-deployed tasks)
* RT01 advanced routing/persistence validation (not built in HD7; revisit in HD8 if needed)
* Expanded script versioning and orchestration

---

## Lessons Incorporated from HD6

The following lessons remain critical and are carried forward:

### 1. IPv6 Handling

* IPv6 may be disabled early to prevent Hyper-V WinRM issues
* This remains part of the build checklist

### 2. WinRM Baseline

* All machines must receive a consistent WinRM configuration immediately after domain join
* Remoting is treated as a **foundational requirement**, not optional

### 3. Network Profile Validation

* All machines must show:
  * DomainAuthenticated network profile
* Verified after every major change

### 4. Validation Discipline

* HealthChecks remain a core strength of the HaleDistrict approach overall
* In HD7 specifically, healthcheck scripting is paused (see FS01 deferral above) and will resume in HD8
* `gpresult /r` and layered validation (IP → Ping → DNS → Domain → Tool) have served as HD7's validation backbone in place of scripted healthchecks

---

## Scope

### In Scope (Remaining HD7 Work)

* Entra ID tenant setup
* Entra Connect installation and configuration
* Identity synchronization (AD → Entra ID)
* Dual-environment authentication validation
* (Optional) Scoped/filtered sync validation

### Out of Scope (HD7, Deferred to HD8)

* Intune
* FS01 / file services / healthcheck script system
* VLAN segmentation
* Multi-school architecture
* Complex DFS/file services
* Large-scale client deployment
* Advanced GPO layering
* RT01

---

## Success Criteria

HD7-Core is successful when:

* WinRM remoting is reliable across all nodes
* AD users synchronize successfully to Entra ID
* A user can authenticate in both environments
* A client device participates in the identity model
* All validation checks return PASS

HD7 is considered **complete** once the above criteria are met — HD7-Extended scoped-sync work is a bonus, not a gate.

---

## Execution Strategy

Remaining HD7 phases:

1. Restart DC01, confirm environment health (ping/DNS/domain triad still PASS)
2. Entra ID tenant setup
3. Entra Connect installation and configuration
4. Identity sync validation (HD7-Teacher01 appears in Entra ID)
5. Dual-environment authentication validation (HD7-Teacher01 authenticates against both AD and Entra ID)
6. (Optional) Scoped/filtered sync validation

Each phase must produce a **working, validated state** before progressing.

---

## Forward Vision

HD7 establishes the foundation for:

* **HD8** — Device Management (Intune) + File Services Return (FS01, healthcheck scripts), policy evolution
* **HD9** — Enterprise Scale Simulation (multi-site, segmentation, security)

---

## Status

HD7-Core is substantially built and validated (Phases 1–4 complete). Entra ID integration (Phase 5) is the next and final major HD7 objective. Environment paused for Germany relocation; resuming now with daily, incremental progress.
