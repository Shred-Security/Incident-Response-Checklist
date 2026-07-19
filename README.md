# Web3 Incident Response Checklist

**Version:** v1.0.0  
**Status:** Stable  
**Last updated:** 2026-04-07

## Table of Contents

- [Introduction](#introduction)  
- [1. Pre-Incident Preparation (Readiness Gate)](#1-pre-incident-preparation-readiness-gate)  
- [2. Phase 1: Detection & Immediate Alert (0–5 Minutes)](#2-phase-1-detection--immediate-alert-0-5-minutes)  
- [3. Phase 2: Containment — Stop the Bleed (0–30 Minutes)](#3-phase-2-containment--stop-the-bleed-0-30-minutes)  
- [4. Phase 3: Assessment & Forensics (First Few Hours)](#4-phase-3-assessment--forensics-first-few-hours)  
- [5. Phase 4: Communication & Remediation Strategy (First 24 Hours)](#5-phase-4-communication--remediation-strategy-first-24-hours)  
- [6. Phase 5: Root Cause Analysis & Fix (First Few Days)](#6-phase-5-root-cause-analysis--fix-first-few-days)  
- [7. Phase 6: Recovery & Phased Rollout (1–2 Weeks)](#7-phase-6-recovery--phased-rollout-1-2-weeks)  
- [8. Phase 7: Post-Mortem, Hardening & Lessons (Weeks+)](#8-phase-7-post-mortem-hardening--lessons-weeks)  
- [Contributions](#contributions)  
- [About Shred Security](#about-shred-security)  
- [License](#license)

---

## Introduction

Protocol Incident Response, Done Right.

This checklist defines a production-grade baseline standard for Web3 incident response in the critical hours after a hack, exploit, or security incident, including key compromise, flash-loan attacks, UI/DNS hijacking, oracle manipulation, governance attacks, and more.
Designed for protocol teams who need a clear, battle-tested sequence of actions when every second counts, and for auditors verifying that a protocol's security posture and operational maturity meet the bar required for real-world deployment.

**Core Rule (BLOCKER if violated):** Containment BEFORE investigation or communication. No debugging, no detailed tweets until losses are contained.

---

## 1. Pre-Incident Preparation (Readiness Gate — Must be 100% before mainnet or upgrades)

| # | Action / Control | Applies When | Severity | Required? | Evidence / Note | Status |
|---|------------------|--------------|----------|-----------|-----------------|--------|
| 1.1 | Incident Response Team assembled with named roles (Ops Lead, Tech Lead, Comm Lead, Legal) | Always | BLOCKER | Yes | Team roster + contact list (Signal/Telegram group) | [ ] |
| 1.2 | Incident triggers defined (TVL drop, anomaly alerts, community pings) | Always | BLOCKER | Yes | Written trigger matrix in playbook | [ ] |
| 1.3 | Pausable contracts + circuit breakers implemented (EIP-7265 style) | All contracts with funds | BLOCKER | Yes | Tx hash of deployment + pause function test | [ ] |
| 1.4 | Multisig + timelocks on ALL admin functions (≥4-of-7 geographic distribution) | Always | BLOCKER | Yes | Multisig address + signer list + timelock config | [ ] |
| 1.5 | Hardware wallet isolation + key rotation policy (dev laptops, API keys, AWS/GCP) | Always | BLOCKER | Yes | Policy doc + last rotation log | [ ] |
| 1.6 | Automated monitoring + invariant bots (solvency, large outflows, pause triggers) | Always | BLOCKER | Yes | Dashboard links + alert test logs | [ ] |
| 1.7 | Pre-approved external partners (auditor retainer, SEAL 911, Chainalysis/TRM Labs) | Always | WARNING | Yes | Retainer contracts + contact sheet | [ ] |
| 1.8 | War-room channels + communication templates ready | Always | WARNING | Yes | Private Discord/Telegram + template folder | [ ] |
| 1.9 | Tabletop exercises completed (flash-loan, key theft, UI compromise) | Quarterly | INFO | Yes | Exercise report + lessons log | [ ] |
| 1.10 | Loss accounting spreadsheet template + cold storage backup | Always | WARNING | Yes | Template link + treasury snapshot | [ ] |

**Readiness Score:** (BLOCKER items must be ✅ before launch/upgrade)

---

## 2. Phase 1: Detection & Immediate Alert (0–5 Minutes)

| # | Action / Control | Applies When | Severity | Required? | Evidence / Note | Status |
|---|------------------|--------------|----------|-----------|-----------------|--------|
| 2.1 | Anomaly confirmed via ≥2 sources (Etherscan, monitoring, community) | Any trigger fires | BLOCKER | Yes | Screenshot + timestamp of first detection | [ ] |
| 2.2 | Activate Incident Mode (war room ping + normal ops suspended) | Confirmed anomaly | BLOCKER | Yes | War-room message timestamp | [ ] |
| 2.3 | No public communication yet (no tweets, no Discord details) | Always in first 5 min | BLOCKER | Yes | Screenshot of silence (or minimal ack only) | [ ] |
| 2.4 | Ops Lead logs initial observations | Always | WARNING | Yes | Timeline entry #1 | [ ] |

---

## 3. Phase 2: Containment — Stop the Bleed (0–30 Minutes)

| # | Action / Control | Applies When | Severity | Required? | Evidence / Note | Status |
|---|------------------|--------------|----------|-----------|-----------------|--------|
| 3.1 | Pause / kill-switch executed on all affected contracts | Contracts pausable | BLOCKER | Yes | Pause tx hash | [ ] |
| 3.2 | All exploit outflows stopped | Active exploit | BLOCKER | Yes | On-chain verification (no further malicious tx) | [ ] |
| 3.3 | Privileged keys rotated / revoked | Key compromise suspected | BLOCKER | Yes | Rotation tx hashes + new multisig setup | [ ] |
| 3.4 | Withdrawals / high-risk functions disabled | User funds at risk | BLOCKER | Yes | Contract state screenshot | [ ] |
| 3.5 | Malicious addresses blocked / blacklisted | Attacker visible | WARNING | Yes | Address list + block tx | [ ] |
| 3.6 | Whitehat rescue evaluated & executed if funds still drainable | Funds still at risk | WARNING | No (but document decision) | Decision log + rescue tx (if done) | [ ] |
| 3.7 | Backend infrastructure isolated (AWS/GCP keys rotated, DNS checked) | UI / infra compromise possible | WARNING | Yes | Rotation logs + DNS snapshot | [ ] |
| 3.8 | Minimal public update sent (“aware of irregularity, operations paused”) | After containment | WARNING | Yes | Tweet / Discord post link | [ ] |

**Containment Success Gate:** All BLOCKER items ✅ → Proceed to assessment.

---

## 4. Phase 3: Assessment & Forensics (First Few Hours)

| # | Action / Control | Applies When | Severity | Required? | Evidence / Note | Status |
|---|------------------|--------------|----------|-----------|-----------------|--------|
| 4.1 | Loss accounting spreadsheet created (Confirmed Lost / At Risk / Secure) | Always | BLOCKER | Yes | Google Sheet / Excel link with categories | [ ] |
| 4.2 | Full incident timeline built (T-Minus last upgrade → T-Zero first malicious tx) | Always | BLOCKER | Yes | Timestamped timeline doc | [ ] |
| 4.3 | External experts engaged (auditor / SEAL 911) | Always | WARNING | Yes | Email / Telegram thread + response timestamp | [ ] |
| 4.4 | Secondary vectors checked (malicious UI, DNS, oracle, bridge, insider) | Always | WARNING | Yes | Checklist + screenshots/logs | [ ] |
| 4.5 | All evidence snapshotted (RPC logs, CloudTrail, Git history, Discord audit) | Always | BLOCKER | Yes | Folder link + hashes | [ ] |
| 4.6 | Exploit vector verified (flash-loan / logic error / key theft) | Always | WARNING | Yes | Tenderly/Foundry debug link + PoC tx | [ ] |

---

## 5. Phase 4: Communication & Remediation Strategy (First 24 Hours)

| # | Action / Control | Applies When | Severity | Required? | Evidence / Note | Status |
|---|------------------|--------------|----------|-----------|-----------------|--------|
| 5.1 | Situation Report (SitRep) drafted — factual only | Always | BLOCKER | Yes | SitRep doc / blog draft | [ ] |
| 5.2 | Transparent updates issued (Discord → X thread) using approved templates | Always | WARNING | Yes | Post links + timestamps | [ ] |
| 5.3 | Exchanges and stablecoin issuers notified to flag attacker addresses | Stablecoins or tokens stolen | WARNING | Yes | Email threads / support tickets | [ ] |
| 5.4 | Remediation plan decided (patch / migrate / redeploy) | Always | BLOCKER | Yes | Decision log with rationale | [ ] |
| 5.5 | Whitehat bounty offered (10–20% via on-chain or private channel) | Funds recoverable | WARNING | No (document) | On-chain message or email | [ ] |
| 5.6 | Evidence preservation logged (who paused, who tweeted, when) | Always | WARNING | Yes | Decision log spreadsheet | [ ] |

**Communication Rule (BLOCKER violation):** Never say "funds are safe" unless 100% verified.

---

## 6. Phase 5: Root Cause Analysis & Fix (First Few Days)

| # | Action / Control | Applies When | Severity | Required? | Evidence / Note | Status |
|---|------------------|--------------|----------|-----------|-----------------|--------|
| 6.1 | Full Root Cause Analysis (RCA) completed with invariant broken + PoC | Always | BLOCKER | Yes | RCA report + fork PoC | [ ] |
| 6.2 | Patch privately audited before any deployment | Patch exists | BLOCKER | Yes | Auditor review report | [ ] |
| 6.3 | Adversarial testing of fix completed | Always | WARNING | Yes | Test report + failed attack attempts | [ ] |
| 6.4 | Treasury assessment + restitution options documented | Losses confirmed | WARNING | Yes | Spreadsheet + board decision | [ ] |

---

## 7. Phase 6: Recovery & Phased Rollout (1–2 Weeks)

| # | Action / Control | Applies When | Severity | Required? | Evidence / Note | Status |
|---|------------------|--------------|----------|-----------|-----------------|--------|
| 7.1 | Phased restart executed: Phase 1 = Withdrawals only | Always | BLOCKER | Yes | Tx hashes per phase | [ ] |
| 7.2 | Phase 2 = Capped deposits | After Phase 1 stable | BLOCKER | Yes | Config tx + limits | [ ] |
| 7.3 | Phase 3 = Full operations after third-party verification | After Phase 2 | BLOCKER | Yes | Auditor sign-off | [ ] |
| 7.4 | All keys re-initialized + funds migrated if needed | Key compromise | BLOCKER | Yes | Migration tx list | [ ] |

---

## 8. Phase 7: Post-Mortem, Hardening & Lessons (Weeks+)

| # | Action / Control | Applies When | Severity | Required? | Evidence / Note | Status |
|---|------------------|--------------|----------|-----------|-----------------|--------|
| 8.1 | Full post-mortem published (technical PoC + changes) | Always | BLOCKER | Yes | Public post-mortem link | [ ] |
| 8.2 | Operational hardening applied (multisig expansion, timelocks, geo-distribution) | Always | BLOCKER | Yes | New contract txs + config | [ ] |
| 8.3 | Real-time invariant monitoring + auto-pause bot added | Always | WARNING | Yes | Bot deployment tx + test | [ ] |
| 8.4 | Continuous security retainer + high-value bug bounty launched | Always | WARNING | Yes | Retainer agreement + bounty program link | [ ] |
| 8.5 | Playbooks updated + new tabletop drill scheduled | Always | INFO | Yes | Updated doc version + drill date | [ ] |

---

## Contributions

We welcome and appreciate contributions to the Web3 Incident Response Checklist! If you'd like to improve or add to the checklist, please fork the repository, make your changes, and submit a pull request (PR). All contributions are reviewed, and we encourage discussions around new ideas or improvements.

### How to contribute:

1. Fork the repository
2. Create a new branch for your changes
3. Make the necessary edits
4. Submit a pull request (PR)

Please ensure that your changes follow the style and formatting conventions used in the checklist. We also encourage you to provide a clear description of what your contribution improves or adds.

Thank you for helping us improve this security resource for the Web3 community!

## License

© 2026 Shred Security

This work is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0).

You are free to share and adapt this checklist for any purpose, including commercial use, provided appropriate credit is given to Shred Security.

## About Shred Security

Shred Security is a research-driven security firm specializing in smart contract auditing, threat modeling, and blockchain security.

For review engagements, deployment war rooms, or full protocol audits, you can reach us at [shredsecurity.io](https://shredsecurity.io).
