\# HD7 Charter — Hybrid Identity Platform (Progress-First Design)

\## Overview

HD7 represents the next evolution of the HaleDistrict environment, transitioning from the complex, fully on-premises infrastructure of HD6 to a simplified, modern hybrid identity platform.

HD7 is intentionally designed to prioritize \*\*continuous progress, reliability, and clarity of learning\*\* over architectural ambition.

The primary objective is to introduce and validate \*\*hybrid identity using Active Directory and Entra ID\*\*, while avoiding the complexity traps encountered in HD6.

\---

\## Core Philosophy

HD7 follows a strict guiding principle:

> \*\*“Build the Toyota Corolla, not the BMW M5.”\*\*

This means:

\* Only one major new concept is introduced at a time \* Infrastructure is intentionally simplified \* Each phase must result in a working, validated system \* Progress is prioritized over perfection

\---

\## Primary Objective

> \*\*Successfully implement and understand hybrid identity between Active Directory and Entra ID\*\*

All design decisions must support this objective.

\---

\## HD7 Architecture Strategy

HD7 is divided into two layers:

\---

\### HD7-Core (Phase 1–3) — Simple, Fast, Reliable

Focus:

\* Minimal infrastructure

\* Reliable remoting baseline

\* Entra ID integration

\* Fast validation cycles

\#### Core Components

\* HD7-DC01 (AD DS + DNS)

\* HD7-ADM01 (management workstation)

\* HD7-CL01 (single client)

\* Entra ID tenant

\#### Key Requirements

\* WinRM baseline applied consistently on all machines

\* Remoting validated early:

&#x20; \* ADM01 → DC01 = PASS

&#x20; \* ADM01 → CL01 = PASS

\* DNS clean and reliable from day one

\* No VLAN or multi-subnet complexity initially

\---

\### HD7-Extended (Phase 4+) — Controlled Expansion

These capabilities are \*\*intentionally deferred\*\* until HD7-Core is stable:

\* VLAN-based multi-school segmentation (HS / MS / ES)

\* Advanced GPO structures

\* Distributed client health check system (GPO-deployed tasks)

\* RT01 advanced routing/persistence validation

\* Expanded script versioning and orchestration

These features remain part of HD7 but are introduced only after identity integration is complete.

\---

\## Lessons Incorporated from HD6

The following lessons remain critical and are carried forward:

\### 1. IPv6 Handling

\* IPv6 may be disabled early to prevent Hyper-V WinRM issues

\* This remains part of the build checklist

\### 2. WinRM Baseline

\* All machines must receive a consistent WinRM configuration immediately after domain join

\* Remoting is treated as a \*\*foundational requirement\*\*, not optional

\### 3. Network Profile Validation

\* All machines must show:

&#x20; \* DomainAuthenticated network profile

\* Verified after every major change

\### 4. Validation Discipline

\* HealthChecks remain a core strength

\* However, scripts will be \*\*simplified initially\*\*

\* Expand only after core functionality is stable

\---

\## Client Health Strategy (Revised)

HD7 will adopt a \*\*phased approach\*\*:

\### Phase 1–3:

\* Use simple remote validation (Invoke-Command)

\* Keep validation lightweight

\### Phase 4+:

\* Introduce GPO-deployed scheduled task model:

&#x20; \* Local execution on clients

&#x20; \* Results written to central share

&#x20; \* ADM01 aggregates results

This approach preserves the HD6 insight while avoiding early complexity.

\---

\## Scope

\### In Scope (Initial Build)

\* Active Directory (DC01)

\* Admin workstation (ADM01)

\* Single client (CL01)

\* Entra ID integration

\* Identity synchronization

\* Remoting baseline validation

\---

\### Out of Scope (Initial Phase)

\* VLAN segmentation

\* Multi-school architecture

\* Complex DFS/file services

\* Large-scale client deployment

\* Advanced GPO layering

\---

\## Success Criteria

HD7-Core is successful when:

\* WinRM remoting is reliable across all nodes

\* AD users synchronize successfully to Entra ID

\* A user can authenticate in both environments

\* A client device participates in the identity model

\* All validation checks return PASS

HD7-Extended is successful when:

\* Advanced segmentation is introduced without breaking core functionality

\* Distributed client health checks operate reliably

\* Full environment validation passes across all layers

\---

\## Execution Strategy

HD7 will be built in controlled phases:

1\. Core AD + ADM01 deployment

2\. WinRM baseline and validation (must PASS before continuing)

3\. Entra ID integration and identity sync

4\. Client identity validation

5\. Controlled expansion into advanced features

Each phase must produce a \*\*working, validated state\*\* before progressing.

\---

\## Forward Vision

HD7 establishes the foundation for:

\* HD8 — Device Management (Intune, policy evolution)

\* HD9 — Enterprise Scale Simulation (multi-site, segmentation, security)

\---

\## Status

HD7 is in active planning and charter refinement phase.
