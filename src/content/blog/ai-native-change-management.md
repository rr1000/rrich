---
title: "When the Same Model Writes and Reviews the Code"
date: 2026-06-10
description: "Code review survived as a compliance control long after it stopped assuring quality. Now agents write and review the code, and the control is fiction. Here's how to rebuild it."
---

Anthropic now says more than 80% of the code merged into its production codebase is written by Claude, up from low single digits when Claude Code launched in February 2025. The reviewing is increasingly handled by other agents, and by implication the humans are reading far less of that output line by line than they once did.

Meanwhile, nearly every SOC 2, ISO 27001, etc. program in existence still contains some version of the same sentence: "changes are reviewed and approved by a second qualified individual before deployment".

Those two facts cannot both be load-bearing.

## The control that broke

Pull request approval has been the workhorse evidence of change management for a decade. An engineer opens a PR, a second engineer writes a comment or clicks approve, and the platform records a tidy artifact with two names on it. Auditors love it because it is legible: a timestamp, an approver, a clear before and after.

The problem is that the artifact no longer points at anything real. When the code was written by an agent and reviewed by a fleet of agents, the human "approval" on the PR is either absent or ceremonial. The control still passes the audit. It just stopped meaning what everyone agreed it meant.

This is worse than it sounds, because it fails in two directions. Either you get theater, with engineers rubber-stamping diffs they did not read so the evidence keeps generating, or you get a finding, with an auditor discovering mid-engagement that the "second reviewer" is a person who hasn't actually reviewed in months. Theater corrodes the program from inside. Findings get improvised into bad remediation. Both are worse than redesigning the control on purpose, before someone forces you to.

## What code review was ever for

You cannot replace a control until you are honest about what it was doing. Peer review was never one thing. It was five, bundled into a single GitHub click:

- Defect detection: catching bugs before they reach production.
- Intent verification: confirming the change solves the right problem.
- Independence: a second party with no authorship bias looking at the work.
- Knowledge transfer: a second human who now understands the change.
- Accountability: a name attached to the approval.

Defect detection, the function everyone cites first, was always the weakest of the five. Humans skim large diffs. Anyone who has approved a 2,000-line PR knows they did not trace every branch, and the research on review effectiveness has said the same thing for years: past a few hundred lines, defect-finding falls off a cliff. Auditors never accepted PR approval because it reliably caught bugs. They accepted it because it was legible evidence of the other four functions, especially independence and accountability.

So the right question is not "how do we make agents review diffs like humans did." It is "where did each of these five functions go." Because all five survived. None of them lives in the diff anymore.

## Agents ate both sides of the review

The current state is not subtle. Agents write the code now, along with the tests, and a fleet of other agents reviews the pull request. Specialized review tools scan for logic errors, security vulnerabilities, and regressions, gate findings by severity, and auto-merge what clears the bar.

The reason this happened is plain economics. Automated correctness checking got cheaper and better than human diff reading, and it kept improving while human attention stayed flat. A senior engineer reading a 2,000-line agent-written diff adds almost nothing on correctness, and it burns the one resource you cannot buy more of: senior judgment. Spending it on line-by-line review of machine output is the most expensive way to catch the fewest bugs.

Review survived all of this by changing location. Verification of the work product got automated, and human attention moved up the stack, from "did the agent write this correctly" to "is the agent building the right thing, under the right rules." Reading Anthropic's own description of the change, that is the shift: less time spent asking whether Claude did the work right, and more asking whether Claude is doing the right work.

## The independence problem nobody is naming

The obvious worry is that when the same model writes the code and reviews the code, you have one mind checking its own work, sharing its own blind spots. That worry is half right, and the half it gets wrong matters more than the half it gets right. It quietly assumes the model is deterministic, that its output is fixed by its weights no matter how you prompt it. It is not. A model told "ship this feature" and the same model told "find the flaw that gets this rolled back" are not running the same computation. Different goal, different context, different scaffolding, different sampling. They fail in different places.

So the real variable is how correlated their failure modes are, and you decorrelate that along several axes, only one of which is model family:

- Role and goal. The writer optimizes for "make it work." The reviewer optimizes for "make it break." Adversarial framing, not a polite second look.
- Context. The reviewer does not inherit the author's context window, its rationalizations, or the half-built mental model that produced the bug. A clean slate looking at the diff and the spec catches what the author was blind to precisely because it never saw the author's reasoning.
- Tooling. Author-independent verification: property tests, contract tests, fuzzers, static analysis, execution that the writing agent did not author and cannot edit. This is independence you can point at, because it does not depend on the reviewer being smarter.
- Model family. A different frontier model on review.

The first three axes are recoverable by prompting and orchestration. The fourth is not. What survives even perfect adversarial prompting is the shared-weights blind spot: a systematic misconception baked into training that the model cannot prompt its way out of, because the gap is in what it knows, not in what it is looking for. That, and only that, is what model diversity buys you. Closing that gap is exactly what a different model family is for, which is why it earns its place as a required control rather than an optional nicety.

That reframes maker-checker correctly. Financial controls never required the checker to be a different species of human. They required a different job, independent visibility, and a mandate to reject. Independence there is structural, not biological. The same is true here. A single model gives you real independence when the writing seat and the reviewing seat have genuinely different goals, context, and tooling, with a different model family layered on to cover the one thing prompting cannot.

A second pass only counts if it is a genuinely different pass. Same model, same goal, same context, run twice, is one pair of eyes looking twice. Same model under an adversarial goal, a clean context, and an independent harness is most of the way to real review, and a second model family closes the rest.

The maturity here deserves honesty. Adversarial same-model review with an independent harness is the cheap, available layer, and a different frontier model on top to catch residual shared-weights blind spots is directionally right. There is early evidence that decorrelated reviewers catch defects correlated ones miss, but the evidence base is young, and anyone selling you a clean number is guessing. What no one can yet put a defensible number on is how much each axis actually contributes.

## Review moves from the diff to the spec

If human review left the diff, where did it go? Up the stack, onto three artifacts that used to be afterthoughts.

The spec. Does this describe the right work, with the constraints stated explicitly, including the security, privacy, and performance properties that no test can infer from a description?

The verification plan. What gates must this change clear, what severity blocks a merge, what evidence gets retained when it passes?

The automation policy. What is allowed to auto-merge, under what conditions, and which named human owns that decision?

Spec-level change management means the thing being reviewed and approved is no longer the diff. It is the intent plus the verification regime that governs how the intent becomes code. A human approves the specification and the rules; the pipeline produces and checks the implementation against them. Approval moves from "I read the change" to "I authored and approved the constraints this change had to satisfy."

And this is not pure loss. Read honestly, it is partly an upgrade. A spec and a policy can be reviewed at a depth no human ever reached skimming a diff. The evidence trail (an approved spec with version history, plus the review run that enforced it) is more complete than PR archaeology ever was. The bar moved to a place a human can actually clear, and it sits no lower for the move.

## The new control set

Here is the direct mapping, function by function, from the control language in your current report to the control that actually means something. Watch what it does to defect detection in particular: the weakest function under human review, the one a 2,000-line diff defeated, is the one the new regime flatly improves, because fleets, severity gates, and author-independent fuzzers catch far more than a skimming human ever did.


| Review function     | Old control (the PR click)               | Replacement control                                                                                                                  | Evidence an auditor receives                                                                               |
| ------------------- | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| Defect detection    | Reviewer sign-off on the diff            | Agent review fleet runs against defined severity gates, backed by author-independent tests and fuzzers                               | Review run logs, findings, gate outcomes                                                                   |
| Intent verification | PR assumed to match intent               | Spec reviewed and approved by an accountable owner before implementation                                                             | Approved spec with version history                                                                         |
| Independence        | A second set of eyes                     | Reviewer independence engineered by decorrelating role, context, and tooling, with a different model family for residual blind spots | Pipeline config showing adversarial review role, independent verification ownership, reviewer model family |
| Knowledge transfer  | A second human who understood the change | The human who authors and approves the spec holds the understanding; sampled deep-dives keep engineers fluent in high-risk areas     | Approved spec with named owner, records of sampled human reviews                                           |
| Accountability      | Change-by-change human sign-off          | Named human owns the auto-merge policy and kill switch, reviewed on a defined cadence                                                | Policy doc, named owner, cadence-review records, exception and rollback logs                               |


Defect detection moves from a reviewer skimming the diff to an agent fleet running against severity gates; intent verification moves from an implicit assumption to a spec approved before implementation; independence is engineered by decorrelating the reviewer's role, context, and tooling from the writer's, with a different model family for the blind spots prompting cannot reach; knowledge transfer shifts to the human who authors and approves the spec, kept current by sampled deep-dives; and accountability becomes a named owner of the auto-merge policy and kill switch, reviewed on a cadence.

I call this set AI-Native Change Management, or ANCM, because it needs a name you can put in a control matrix and reuse across frameworks. None of it asks an auditor to accept less assurance. It maps onto criteria they already use. The pitch to your auditor is not "trust us, the robots are fine." It is "here is more evidence than a PR approval ever gave you, and here is the criterion it satisfies." That sentence disarms the obvious objection before it is raised.