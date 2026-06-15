# AI-Native Change Management (ANCM)

## A one-page control set for agent-written, agent-reviewed code

Version 1.1. Copy-paste control language for SOC 2, ISO 27001, and CMMC programs
where code is authored and reviewed primarily by AI agents.

The premise: pull-request approval by a second human no longer corresponds to a real
assurance activity when an agent wrote the code and a fleet of agents reviewed it. The
five functions of peer review (defect detection, intent verification, independence,
knowledge transfer, accountability) all survive, but none of them lives in the diff
anymore. ANCM relocates each function to where it now actually happens and specifies the
evidence an auditor receives.

---

## The five controls

### ANCM-1: Spec approval before implementation
**Function:** Intent verification.
**Control language:** Material changes require a written specification, reviewed and
approved by an accountable owner before implementation begins. The specification states
functional intent and the security, privacy, and performance constraints the change must
satisfy.
**Replaces:** "Pull request approved by a second engineer."
**Evidence:** Approved specification with version history and named approver.

### ANCM-2: Automated review with severity gates
**Function:** Defect detection.
**Control language:** Every change is evaluated by an automated review process that scans
for logic errors, security vulnerabilities, and regressions. Findings are graded by
severity. Findings at or above a defined threshold block merge until resolved.
**Replaces:** "Reviewer sign-off on the diff."
**Evidence:** Review run logs, the findings produced, and the gate outcome (pass/block) per change.

### ANCM-3: Reviewer independence (writer/reviewer separation)
**Function:** Independence.
**Control language:** The agent or instance that authors a change does not review it under
the same role, goal, and context. Reviewer independence is engineered by decorrelating the
reviewer from the writer along three axes: role and goal (the reviewer operates under an
adversarial mandate to find the flaw, not to confirm the work), context (the reviewer runs
from a clean context that did not inherit the author's reasoning), and tooling. It is
anchored by an author-independent verification harness: property tests, contract tests,
fuzzers, static analysis, and execution that the authoring agent did not write and cannot
edit.
**Replaces:** "A second set of eyes."
**Evidence:** Pipeline configuration showing the adversarial reviewer role and a clean
reviewer context, verification and harness ownership separate from the authoring agent, and
harness run logs.

### ANCM-4: Knowledge transfer via spec ownership
**Function:** Knowledge transfer.
**Control language:** Understanding of a change resides with the human who authors and
approves its specification, not with a diff reader. High-risk areas are kept current
through sampled human deep-dives on a defined cadence.
**Replaces:** "A second human who read and understood the diff."
**Evidence:** Approved specification with named owner; records of sampled human reviews of
high-risk changes.

### ANCM-5: Accountability, auto-merge ownership, and rollback
**Function:** Accountability.
**Control language:** Conditions under which a change may merge without human approval are
defined in a written auto-merge policy owned by a named individual and reviewed on a defined
cadence. A documented mechanism exists to halt automated merges and roll back deployed
changes. Exceptions to standard gates require human escalation and are logged.
**Replaces:** "Change-by-change human accountability and manual rollback authority."
**Evidence:** The auto-merge policy document and its named owner, cadence-review records,
exception logs, and a tested rollback procedure.

---

## Framework crosswalk

| ANCM control | SOC 2 | ISO 27001:2022 | CMMC |
|---|---|---|---|
| ANCM-1 Spec approval | CC8.1 | A.8.32 Change management | CM.L2-3.4.3 |
| ANCM-2 Automated review + gates | CC8.1 | A.8.32, A.8.28 Secure coding | CM.L2-3.4.3, SI |
| ANCM-3 Reviewer independence | CC8.1 | A.8.32, A.5.3 Segregation of duties | CM.L2-3.4.3 |
| ANCM-4 Knowledge transfer | CC8.1 | A.8.32, A.6.3 Awareness and competence | CM.L2-3.4.3 |
| ANCM-5 Accountability + rollback | CC8.1, CC7.x | A.5.2 Roles, A.8.32, A.5.3 | CM.L2-3.4.4, CM.L2-3.4.3 |

---

## How to use this

1. Inventory which controls in your current report assume a human reads the change.
2. Replace that control language with the ANCM-1 through ANCM-5 wording above.
3. Brief your auditor before the audit window, walking in with this crosswalk.

You are not asking the auditor to accept less assurance. ANCM produces more evidence than
a PR approval ever did, mapped to criteria already in their program.

---