# Verification and Validation Plan

**System:** TBD
**Feature:** TBD
**Date:** TBD
**Status:** DRAFT — initiated by se-nfr, updated continuously
**Last Updated By:** TBD

---

## Purpose

This plan defines how every requirement and stakeholder need
will be verified or validated. It is a threaded artifact:
initiated in Phase 1 with NFR entries, extended in Phase 2
with functional requirement entries, and updated through
Phase 6 when results are recorded.

A requirement without a verification method is incomplete
by definition.

---

## Verification Methods

| Code | Method | When to Use |
| ---- | ------ | ----------- |
| T | Test | Quantitative criterion, system execution required |
| A | Analysis | Mathematical or model-based, no execution required |
| I | Inspection | Review of artifact, no execution required |
| D | Demonstration | Qualitative, witnessed operation sufficient |

---

## Verification Stages

| Code | Stage | Entry Criterion |
| ---- | ----- | --------------- |
| UT | Unit Test | Component implementation complete |
| IT | Integration Test | All contributing UT events passed |
| ST | System Test | All IT events passed, system integrated |
| AT | Acceptance Test | TBD — defined in se-vvplan |

---

## V&V Plan Table

Valid Status values: `Planned` | `Pass` | `Fail` | `Waived` | `Carried Forward`

When Status = `Carried Forward`, the `Notes` field must contain:
(a) the rationale for deferral and (b) the target Feature ID expected to close this entry.

| V-ID | Req ID | Type | Description | Method | Stage | Pass Criterion | Responsible | Feature | Status | Notes |
| ---- | ------ | ---- | ----------- | ------ | ----- | -------------- | ----------- | ------- | ------ | ----- |

---

## Validation Plan

| VAL-ID | Scenario ID | Description | Stakeholder | Environment | Success Criterion | Responsible | Feature | Status | Notes |
| ------ | ----------- | ----------- | ----------- | ----------- | ----------------- | ----------- | ------- | ------ | ----- |

---

## Regulatory and Certification Events

*Populated if applicable — see se-vvplan skill.*

---

## Coverage Summary

| Category | Total Items | V&V Entries | Coverage |
| -------- | ----------- | ----------- | -------- |
| Functional Requirements | TBD | TBD | TBD |
| Must Have NFRs | TBD | TBD | TBD |
| Interfaces | TBD | TBD | TBD |
| Operational Scenarios (Validation) | TBD | TBD | TBD |
| **Total** | TBD | TBD | **TBD** |
