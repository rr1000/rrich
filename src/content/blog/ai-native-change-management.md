---
title: "When the Same Model Writes and Reviews the Code"
date: 2026-06-10
description: "Code review survived as a compliance control long after it stopped assuring quality. Now agents write and review the code, and the control is fiction. Here's how to rebuild it."
---

Anthropic now says more than 80% of the code merged into its production codebase is written by Claude, up from low single digits when Claude Code launched in February 2025. Its engineers have stopped reading most of that output line by line. The reviewing is done by other agents.

Meanwhile, nearly every SOC 2, ISO 27001, and CMMC program in existence still contains some version of the same sentence: changes are reviewed and approved by a second qualified individual before deployment.

Those two facts cannot both be load-bearing. One of them is now a story we tell auditors.

## The control that broke

Pull request approval has been the workhorse evidence of change management for a decade. An engineer opens a PR, a second engineer clicks approve, and the platform records a tidy artifact with two names on it. Auditors love it because it is legible: a timestamp, an approver, a clear before and after.

The problem is that the artifact no longer points at anything real. When the code was written by an agent and reviewed by a fleet of agents, the human "approval" on the PR is either absent or ceremonial. The control still passes the audit. It just stopped meaning what everyone agreed it meant.

This is worse than it sounds, because it fails in two directions. Either you get theater, with engineers rubber-stamping diffs they did not read so the evidence keeps generating, or you get a finding, with an auditor discovering mid-engagement that the "second reviewer" is a person who hasn't actually reviewed in months. Theater corrodes the program from inside. Findings get improvised into bad remediation. Both are worse than redesigning the control on purpose, before someone forces you to.

## What code review was ever for

You cannot replace a control until you are honest about what it was doing. Peer review was never one thing. It was five, bundled into a single GitHub click:

- Defect detection: catching bugs before they reach production.
- Intent verification: confirming the change solves the right problem.
- Independence: a second party with no authorship bias looking at the work.
- Knowledge transfer: a second human who now understands the change.
- Accountability: a name attached to the approval.

Here is the uncomfortable part. Defect detection, the function everyone cites first, was always the weakest of the five. Humans skim large diffs. Anyone who has approved a 2,000-line PR knows they did not trace every branch, and the research on review effectiveness has said the same thing for years: past a few hundred lines, defect-finding falls off a cliff. Auditors never accepted PR approval because it reliably caught bugs. They accepted it because it was legible evidence of the other four functions, especially independence and accountability.

So the right question is not "how do we make agents review diffs like humans did." It is "where did each of these five functions go." Because all five survived. None of them lives in the diff anymore.

## Agents ate both sides of the review

The current state is not subtle. Agents write the code. Agents write the tests. Agents review the pull request. Specialized review tools now run fleets of agents that scan for logic errors, security vulnerabilities, and regressions, gate findings by severity, and auto-merge what clears the bar.

The reason this happened is plain economics. Automated correctness checking got cheaper and better than human diff reading, and it kept improving while human attention stayed flat. A senior engineer reading a 2,000-line agent-written diff adds almost nothing on correctness, and it burns the one resource you cannot buy more of: senior judgment. Spending it on line-by-line review of machine output is the most expensive way to catch the fewest bugs.

So review did not disappear. It moved. Verification of the work product got automated. Human attention relocated up the stack, from "did the agent write this correctly" to "is the agent building the right thing, under the right rules." That is the shift Anthropic describes in its own framing: less time asking whether Claude did the work right, more time asking whether Claude is doing the right work.

## The independence problem nobody is naming

Here is the part most of the agentic-coding conversation skips, and it is the part a controls person cannot skip.

When the same model writes the code and reviews the code, you do not have independent review. You have one mind checking its own work. The reviewer shares the author's training data, the author's biases, and the author's blind spots. Whatever the writer got confidently wrong, the reviewer is primed to wave through, because it would have made the same mistake.

This is not a new problem. It is the exact problem segregation of duties was invented to solve in financial controls. You never let the same party originate a transaction and approve it, not because either person is dishonest, but because a single point of judgment is a single point of failure. Maker-checker exists precisely because correlated judgment is not real review.

Writer model versus reviewer model is the new maker-checker. And the same logic applies: independence has to be engineered in, not assumed. In an agentic pipeline that means three concrete things. Model diversity, so a different model family reviews the work rather than the one that produced it. Author-independent verification, meaning property tests, contract tests, and human-authored invariants the writing agent did not get to generate for itself. And adversarial review, prompts and harnesses designed to attack the change rather than bless it.

A second pair of eyes only counts if it is a second brain. Two instances of the same model are one pair of eyes looking twice.

I will be honest about the maturity here. Reviewing Claude-written code with a different frontier model is directionally right, and there is early evidence that diverse reviewers catch defects that correlated reviewers miss. But the evidence base is young, and anyone selling you a clean number is guessing. The principle is sound. The calibration is not settled.

## Review moves from the diff to the spec

If human review left the diff, where did it go? Up the stack, onto three artifacts that used to be afterthoughts.

The spec. Does this describe the right work, with the constraints stated explicitly, including the security, privacy, and performance properties that no test can infer from a description?

The verification plan. What gates must this change clear, what severity blocks a merge, what evidence gets retained when it passes?

The automation policy. What is allowed to auto-merge, under what conditions, and which named human owns that decision?

This is the screenshot, so I will state it cleanly:

Spec-level change management means the thing being reviewed and approved is no longer the diff. It is the intent plus the verification regime that governs how the intent becomes code. A human approves the specification and the rules; the pipeline produces and checks the implementation against them. Approval moves from "I read the change" to "I authored and approved the constraints this change had to satisfy."

And this is not pure loss. Read honestly, it is partly an upgrade. A spec and a policy can be reviewed at a depth no human ever reached skimming a diff. The evidence trail (an approved spec with version history, plus the review run that enforced it) is more complete than PR archaeology ever was. We are not lowering the bar. We are moving it somewhere a human can actually clear it.

## The new control set

This is the load-bearing section. Here is the direct mapping from the control language in your current report to the control that actually means something, with the evidence an auditor receives.

| Old control | Replacement control | Evidence an auditor receives |
|---|---|---|
| PR approved by a second engineer | Spec reviewed and approved by an accountable owner before implementation | Approved spec with version history |
| Reviewer sign-off on the diff | Agent review fleet runs against defined severity gates | Review run logs, findings, gate outcomes |
| A second set of eyes | Reviewer independence via model diversity and author-independent verification | Pipeline config showing writer/reviewer separation |
| Change-by-change human accountability | Named human owns the auto-merge policy, reviewed on a defined cadence | Policy doc, named owner, cadence-review records |
| Manual rollback authority | Kill switch and exception path with human escalation | Exception logs, tested rollback procedure |

Read in prose, in case the table gets clipped: PR approval by a second engineer becomes spec approval by an accountable owner before implementation. Reviewer sign-off on the diff becomes an agent review fleet run against defined severity gates. Second set of eyes becomes reviewer independence enforced through model diversity and author-independent verification.

I call this set AI-Native Change Management, or ANCM, because it needs a name you can put in a control matrix and reuse across frameworks. None of it asks an auditor to accept less assurance. It maps onto criteria they already use: SOC 2 CC8.1, ISO 27001 Annex A 8.32, A.5.3 segregation of duties, and the CMMC configuration-management practices. The pitch to your auditor is not "trust us, the robots are fine." It is "here is more evidence than a PR approval ever gave you, and here is the criterion it satisfies." That sentence disarms the obvious objection before it is raised.

## What the new model doesn't catch

A control set that claims to catch everything is the surest sign its author is lying. Here is where the residual risk actually concentrates.

Spec holes. The agent faithfully builds the wrong thing because the intent was underspecified. The pipeline is green. The outcome is still wrong, because nothing in the verification regime knew what you forgot to ask for.

Properties tests cannot express. Data-handling choices, performance regressions, security assumptions that pass every gate while violating an intent nobody wrote down. Absence of a failing test is not presence of correct behavior.

Correlated failure that survives diversity. Different model families still share large swaths of training distribution. Diversity reduces correlated blind spots. It does not zero them, and treating it as if it does is its own new control gap.

Quiet erosion of human system knowledge. The knowledge-transfer function of review dies without an alarm. No incident fires the day your engineers stop understanding their own system. You find out later, during the outage, when nobody in the room can reason about the thing that broke.

So human review is concentrated, not eliminated. It goes where leverage is highest: deep review of specs, sampled deep-dives on the changes that can actually hurt you (auth, crypto, data flows, billing), and review driven by incidents after they happen. Far less review by volume, far more value per hour spent.

## The next twelve months decide the precedent

Auditors are going to walk into agent-written, agent-reviewed codebases this year whether the profession is ready or not. The only open question is who writes the control story first.

If practitioners do not bring a coherent one, auditors will improvise, one client at a time. Improvised precedent is conservative, inconsistent, and frequently stupid. The likely default, absent a better option, is requiring humans to re-read agent diffs as a checkbox. That is theater with worse latency. It satisfies no one and assures nothing.

So three things to do this quarter, if any of this is your problem to own. Inventory which controls in your current report silently assume a human reads the change. Draft the replacement control language before your next audit window, not during it. And brief your auditor proactively, walking in with the mapping above rather than getting cross-examined on the gap.

I wrote the five controls up as a one-page set you can copy straight into a control matrix, with the framework crosswalk attached: [the ANCM control set](/ancm-control-set.md). Take it, adapt the wording to your environment, and bring it to your next audit before the audit brings something worse to you.

The control did not survive because it worked. It survived because nobody had to replace it yet. Now you do. The teams that write down the new control set first are going to set the standard everyone else gets measured against, and that is a much better seat than the one where an auditor improvises your future for you.
