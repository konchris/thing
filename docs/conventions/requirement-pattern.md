# Requirement Pattern

Use this pattern when documenting functional requirements, non-functional requirements, constraints, or interface requirements.

---

## Requirement

### <PREFIX>-<CATEGORY>-<NUMBER> — <Short Name>

The system shall <observable behavior or constraint>.

**Rationale:**  
Explain why the requirement exists.

**Fit Criterion / Verification:**  
Explain how this will be checked.

**Verification Method:**  
Inspection / Analysis / Demonstration / Test

**Priority:**  
Must / Should / Could

**Status:**  
Draft / Accepted / Deprecated

**Traces to:**  
`<upstream-id>`

**Verified by:**  
`<verification-id>`

---

## Example

### NFR-REL-001 — Recovery after interrupted workflow

The system shall preserve workflow artifact state after an interrupted agent session.

**Rationale:**  
AI coding agents may lose context or terminate unexpectedly. The workflow library should support durable recovery.

**Fit Criterion / Verification:**  
Terminate an agent session during an active workflow and confirm that generated artifacts can be resumed without loss of required traceability fields.

**Verification Method:**  
Test

**Priority:**  
Must

**Status:**  
Draft

**Traces to:**  
`CONOPS-OP-003`

**Verified by:**  
`VER-TEST-002`
