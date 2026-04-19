# Replay Attack Simulation & Defense Comparison

**Client:** Grand Marina Hotel  
**Prepared for:** Marcus Reed, VP of Operations  
**Analyst:** Fabella Terry  
**Date:** April 13, 2026  
**Externship:** Hydroficient IoT Cybersecurity Externship

---

## Overview

Simulated three replay attack types against a Python MQTT pipeline across four defense configurations. Measured rejection rates per configuration and delivered a plain-language findings report to the VP of Operations with a deployment recommendation. Referenced AWS IoT Core standards, IEC 62443, and MQTT v5.

## What Is a Replay Attack?

A replay attack captures a legitimate MQTT message and retransmits it later to trick the broker or application into acting on stale or duplicated data — for example, replaying a "valve open" command hours after it was originally authorized.

## Attack Types Tested

| Attack Type | Description |
|---|---|
| **Immediate Replay** | Message retransmitted within seconds of capture |
| **Delayed Replay** | Message retransmitted hours after original transmission |
| **Modified Replay** | Captured message with payload tampered before retransmission |

## Defense Configurations Tested

| Configuration | Mechanism |
|---|---|
| No defense | Raw MQTT, no protections |
| Timestamp only | Messages include a timestamp; broker rejects if outside window |
| HMAC-SHA256 only | Message signed with shared secret; signature validated by broker |
| Timestamp + HMAC | Both controls combined |

## Experiment Results

| Defense Config | Immediate Replay | Delayed Replay | Modified Replay |
|---|---|---|---|
| No defense | 0% rejected | 0% rejected | 0% rejected |
| Timestamp only | 100% rejected | 100% rejected | 0% rejected |
| HMAC only | 100% rejected | 0% rejected | 100% rejected |
| Timestamp + HMAC | **100% rejected** | **100% rejected** | **100% rejected** |

## Key Finding

Neither timestamp-only nor HMAC-only provides complete protection. The combination of both — **Timestamp + HMAC-SHA256** — is the only configuration that blocks all three attack types with 100% rejection rate.

## Deployment Recommendation

Deploy Timestamp + HMAC-SHA256 on all MQTT sensor messages in production. This aligns with:

- **AWS IoT Core** message broker security standards
- **IEC 62443** — industrial IoT message integrity requirements
- **MQTT v5** — enhanced authentication and reason codes

## Plain-Language Framing (VP Report)

> "A replay attack is like a thief recording your voice saying 'open the front door' and playing it back later when you're not home. Our timestamp + signature solution is like adding a one-time code to every command — even if someone records it, the code expires and the door won't open."

## Artifacts

| File | Description |
|---|---|
| `Fabella_Terry-Defense_Comparison_Report_.docx` | Full executive report to VP of Operations |
| `Fabella_Terry-_Defense_Experiment_Results.docx` | Experiment worksheets with raw rejection rate data |

---

**Author:** Fabella Terry | AWS Certified Cloud Practitioner  
**Target Domain:** IAM Governance & GRC | IoT Security
