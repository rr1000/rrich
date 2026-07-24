---
title: "When AI Writes and Reviews the Code"
date: 2026-06-10
description: "Code review survived as a compliance control long after it stopped assuring quality. Now agents write and review the code, and the control is fiction. Here's how to rebuild it."
---

Anthropic now says Claude writes more than 80% of the code merged into its production codebase. That figure was in the low single digits when Claude Code launched in February 2025. Other agents increasingly handle the reviews, which means humans read far less of the output line by line than they once did.

Meanwhile, nearly every SOC 2 or ISO 27001 program still contains some version of the same sentence: "changes are reviewed and approved by a second qualified individual before deployment."

The control language no longer describes the work.

## The control that broke

For a decade, pull request approval has been the standard evidence for change management. One engineer opens a PR, another comments or clicks approve, and the platform records a tidy artifact with two names on it. Auditors like it because it is easy to read: there is a timestamp, an approver, and a clear before and after.

Now the artifact often records a ritual instead of a review. If an agent wrote the code and a fleet of agents reviewed it, the human "approval" is either missing or ceremonial. The control may still pass an audit, but it no longer means what everyone agreed it meant.

That leaves you with theater or a finding. Engineers rubber-stamp diffs they did not read so the evidence keeps appearing, or an auditor discovers midway through an engagement that the "second reviewer" has not actually reviewed anything in months. Theater corrodes the program from within. A finding usually leads to hurried, awkward remediation. It is better to redesign the control before either happens.

## What code review was ever for

To replace the control, we need to be honest about what it did. Peer review bundled five jobs into one GitHub click:

- Defect detection: catching bugs before they reach production.
- Intent verification: confirming the change solves the right problem.
- Independence: a second party with no authorship bias looking at the work.
- Knowledge transfer: a second human who now understands the change.
- Accountability: a name attached to the approval.

Defect detection, usually the first justification people offer, was always the weakest of the five. Humans skim large diffs. Anyone who has approved a 2,000-line PR knows they did not trace every branch. Review effectiveness drops sharply once a diff reaches a few hundred lines. Auditors did not accept PR approval because it caught bugs reliably. They accepted it because the approval gave them legible evidence of the other four functions, especially independence and accountability.

The question, then, is where those five functions live now. They still matter, but the diff is no longer where we should look for them.

## Agents ate both sides of the review

Agents now write code and tests, while other agents review the pull request. Specialized tools scan for logic errors, security vulnerabilities, and regressions. They sort findings by severity and automatically merge changes that clear the required gates.

The economics explain why. Automated correctness checks became cheaper and better while human attention stayed fixed. Asking a senior engineer to read a 2,000-line, agent-written diff adds little assurance and consumes a scarce resource: senior judgment. Line-by-line review of machine output is an expensive way to catch very few bugs.

Review did not disappear. Verification of the work product became automated, and human attention moved to the decisions above the diff. Instead of asking whether the agent wrote the code correctly, people spend more time asking whether it is building the right thing under the right rules. Anthropic describes the change in similar terms: less attention on whether Claude did the work right, and more on whether Claude is doing the right work.

## The independence problem nobody is naming

The obvious objection is that the same model writing and reviewing code amounts to one mind checking its own work. The writer and reviewer would share the same blind spots. That risk is real, but models are not deterministic. They do not produce the same kind of answer regardless of their instructions. "Ship this feature" and "find the flaw that gets this rolled back" create different work. The goal, context, scaffolding, and sampling all change, so the two runs can fail in different places.

What matters is how closely the writer's and reviewer's failures are correlated. You can reduce that correlation in three ways:

- Give the roles different goals. The writer tries to make the change work; the reviewer tries to break it. The second pass should be adversarial, not polite.
- Start the reviewer with a clean context. It should not inherit the writer's rationalizations or the half-built mental model that produced the bug. Looking only at the diff and the spec helps it see mistakes the writer missed.
- Use author-independent tooling. Property tests, contract tests, fuzzers, static analysis, and execution should sit outside the writing agent's control. The agent neither authors nor edits these checks.

Adversarial prompting cannot solve every independence problem. The writer and reviewer may share a misconception baked into the model's training. No prompt can fill a gap in what the model knows. That is why the harness matters more than adding another review pass. Property tests, fuzzers, static analysis, and execution are not models, so they do not share the model's blind spots. A mistake both agent roles believe still has to survive checks that neither one authored.

The harness cannot catch everything. Novel logic and judgment calls still depend on the adversarial role and clean context. Some risk remains, as it does in any review regime, whether human or machine.

This is the useful way to think about maker-checker. Financial controls never required the checker to be a different kind of person. They required a separate job, independent visibility, and the authority to reject. The independence is structural. A single model can provide it when the writer and reviewer have genuinely different goals, context, and tooling, backed by verification the writer did not author and cannot edit.

Running the same model twice with the same goal and context does not count as independent review. Change the goal, clear the context, and add an independent harness, and the second pass becomes meaningful. Early evidence suggests that separating role, context, and tooling catches defects that a polite second look misses. The evidence base is still young, though, and any precise claim about how much each factor contributes is guesswork.

## Review moves from the diff to the spec

Human review moves away from the diff and onto three artifacts that used to be afterthoughts:

- The spec describes the work and states its constraints, including security, privacy, and performance requirements that a test cannot infer from a vague description.
- The verification plan defines the gates a change must clear, the severity that blocks a merge, and the evidence retained after a successful run.
- The automation policy sets the conditions for automatic merges and names the human who owns that decision.

Under this model, the diff is no longer the artifact being approved. The human approves the intent and the verification regime that will govern the implementation. The pipeline then produces the code and checks it against those rules. Approval changes from "I read the change" to "I authored and approved the constraints this change had to satisfy."

That is not necessarily a loss. People can review a spec and policy more carefully than they can skim a large diff. An approved spec with version history, paired with the review run that enforced it, also leaves a more complete evidence trail than PR archaeology. The human still has to clear a high bar, but it is now a bar they can realistically clear.

## The new control set

The table below maps each function of peer review to a control that still means something. Defect detection gets the clearest improvement. Review fleets, severity gates, and author-independent fuzzers can examine work that a human would only skim.


| Review function     | Old control (the PR click)               | Replacement control                                                                                                                  | Evidence an auditor receives                                                                               |
| ------------------- | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| Defect detection    | Reviewer sign-off on the diff            | Agent review fleet runs against defined severity gates, backed by author-independent tests and fuzzers                               | Review run logs, findings, gate outcomes                                                                   |
| Intent verification | PR assumed to match intent               | Spec reviewed and approved by an accountable owner before implementation                                                             | Approved spec with version history                                                                         |
| Independence        | A second set of eyes                     | Reviewer independence engineered by decorrelating role, context, and tooling from the writer's, anchored by an author-independent verification harness | Pipeline config showing adversarial review role and verification ownership separate from the writing agent; harness run logs |
| Knowledge transfer  | A second human who understood the change | The human who authors and approves the spec holds the understanding; sampled deep-dives keep engineers fluent in high-risk areas     | Approved spec with named owner, records of sampled human reviews                                           |
| Accountability      | Change-by-change human sign-off          | Named human owns the auto-merge policy and kill switch, reviewed on a defined cadence                                                | Policy doc, named owner, cadence-review records, exception and rollback logs                               |


I call this set AI-Native Change Management, or ANCM, because a control needs a stable name before it can live in a matrix and work across frameworks. It does not ask an auditor to accept less assurance. It maps new evidence to criteria they already use.

Walk an auditor through the evidence each control produces and the criterion each artifact satisfies. That is a stronger position than asking everyone to pretend a ceremonial click still counts as review.
