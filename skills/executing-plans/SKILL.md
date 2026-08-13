---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**Note:** Tell your human partner that Superpowers works much better with access to subagents. If subagents are available, use subagent-driven-development instead of this skill.

## The Process

### Step 1: Load and Review Plan
1. Ensure you are working in the appropriate directory
2. Read plan file
3. Review critically - identify any questions or concerns about the plan
4. If concerns: Raise them with your human partner before starting
5. If no concerns: Create todos for the plan items and proceed

**Review plan scope for commits:** While reviewing, note whether the plan's
scope authorizes commits. The default is **do not commit**. Commit only if
the plan explicitly includes or implies producing committed code (e.g.,
"implement X with tests on a branch", "land this on a feature branch").
If the plan is about research, exploration, design, drafting, or any task
that does not call for committed code, do not commit. If unsure, ask
the human partner before starting — never assume.

### Step 2: Execute Tasks

For each task:
1. Mark as in_progress
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. **Commits:** follow the plan's commit guidance exactly. If the plan
   scope does not authorize commits, leave changes uncommitted in the
   working tree. If the plan authorizes commits, commit only the work
   the plan assigns — never commit unrelated changes, and never amend,
   force-push, or squash without explicit user consent.
5. Mark as completed

### Step 3: Complete Development

After all tasks complete and verified:
- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent
- **Do not commit unless the plan's scope authorizes it.** Research,
  exploration, design, and drafting tasks produce uncommitted work; only
  plans that explicitly call for producing code on a branch warrant
  commits. When in doubt, ask before committing.
