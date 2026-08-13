---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** Plans are typically executed task-by-task with review between tasks.

**Save plans to:** a markdown path the user chooses — default `docs/plans/YYYY-MM-DD-<feature-name>.md`, or follow the project convention (`plans/`, `docs/adr/`, `docs/design/`, etc.).
- (User preferences for plan location override this default. Plans are NOT restricted to `docs/superpowers/`.)

## Scope Check

If the spec covers multiple independent subsystems, it should have been broken into sub-project specs during brainstorming. If it wasn't, suggest breaking this into separate plans — one per subsystem. Each plan should produce working, testable software on its own.

## File Structure

Before defining tasks, map out which files will be created or modified and what each one is responsible for. This is where decomposition decisions get locked in.

- Design units with clear boundaries and well-defined interfaces. Each file should have one clear responsibility.
- You reason best about code you can hold in context at once, and your edits are more reliable when files are focused. Prefer smaller, focused files over large ones that do too much.
- Files that change together should live together. Split by responsibility, not by technical layer.
- In existing codebases, follow established patterns. If the codebase uses large files, don't unilaterally restructure - but if a file you're modifying has grown unwieldy, including a split in the plan is reasonable.

This structure informs the task decomposition. Each task should produce self-contained changes that make sense independently.

## Task Right-Sizing

A task is the smallest unit that carries its own test cycle and is worth a
fresh reviewer's gate. When drawing task boundaries: fold setup,
configuration, scaffolding, and documentation steps into the task whose
deliverable needs them; split only where a reviewer could meaningfully
reject one task while approving its neighbor. Each task ends with an
independently testable deliverable.

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Code Density

Plans should be **lean and scannable**, not code dumps. The goal is
"everything the engineer needs to do the task correctly, nothing they
could write themselves in 30 seconds from context already in the plan."

**Always show in full — these are contracts:**
- File paths
- Public interfaces: function/method signatures, type names, field names, exported constants. Other tasks rely on these exact names.
- The first failing test of a behavior (it sets the contract for that behavior).
- Non-obvious logic: regexes, edge cases, tricky error handling, ordering, concurrency.
- Architectural wiring: DI, factory registration, middleware order, route mounting.

**Sample or abbreviate freely — these are not contracts:**
- Boilerplate: imports, type aliases, `package.json` deps, `tsconfig` tweaks, lint config.
- Repetitive shapes: tests 4–10 for the same component, CRUD methods after the first, handlers after the first. Show 1–2 examples, then say "follow this shape for the rest."
- Long files where the change is localized — show the changed region with line numbers (`Modify: src/x.ts:142-160`) and a short snippet.
- Standard config any engineer in the stack writes from memory.

**Allowed shorthand in steps:**
- "Add the other 4 tests following the shape in Step 1, covering: [list of cases]."
- "Implement the remaining CRUD methods (`update`, `delete`) following the `create` pattern in Step 3."
- "Add `validate()` and `format()` to the same module, mirroring the helpers in `src/utils/strings.ts:42-58`."
- "Wire the new route in `app.ts` next to the existing `/health` route (same pattern)."

**Still never write — these are plan failures:**
- "TBD", "TODO", "fill in details", "implement later".
- "Add appropriate error handling" / "add validation" / "handle edge cases" with no specifics.
- "Similar to Task N" when Task N is in a different file or context the implementer of this task won't have read.
- References to types, functions, or methods not defined in this plan.
- A critical contract (the failing test, the public signature other tasks consume) shown as a sample — full code is required for contracts even if short.

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use subagent-driven-development (recommended) or executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

## Global Constraints

[The spec's project-wide requirements — version floors, dependency limits,
naming and copy rules, platform requirements — one line each, with exact
values copied verbatim from the spec. Every task's requirements implicitly
include this section.]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Interfaces:**
- Consumes: [what this task uses from earlier tasks — exact signatures]
- Produces: [what later tasks rely on — exact function names, parameter
  and return types. A task's implementer sees only their own task; this
  block is how they learn the names and types neighboring tasks use.]

- [ ] **Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

- [ ] **Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

- [ ] **Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

- [ ] **Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

- [ ] **Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## No Critical Placeholders

The Code Density rules above say when to show full code vs. samples. This
section is the floor: below this line, every step is a plan failure.

**Always plan failures — never write them:**
- "TBD", "TODO", "fill in details", "implement later"
- "Add appropriate error handling" / "add validation" / "handle edge cases" with no specifics about *what* handling or *which* cases
- "Write tests for the above" with no test code or named cases
- "Similar to Task N" if Task N is in a different file/context the implementer of this task may not have read
- Steps that describe *what* to do without showing *how* (a code block, a command, or a clear shorthand like "follow the shape in Step 2" with Step 2 actually showing the shape)
- References to types, functions, or methods not defined in any task in this plan

The bar is "an engineer reading this one task in isolation can do it
without guessing." Code Density controls *how much* code; this section
controls *whether the code (or its shorthand) is specific enough*.

## Self-Review

After writing the complete plan, look at the spec with fresh eyes and check the plan against it. This is a checklist you run yourself — not a subagent dispatch.

**1. Spec coverage:** Skim each section/requirement in the spec. Can you point to a task that implements it? List any gaps.

**2. Placeholder scan:** Search your plan for red flags — any of the patterns from the "No Critical Placeholders" section above. Fix them.

**3. Type consistency:** Do the types, method signatures, and property names you used in later tasks match what you defined in earlier tasks? A function called `clearLayers()` in Task 3 but `clearFullLayers()` in Task 7 is a bug.

If you find issues, fix them inline. No need to re-review — just fix and move on. If you find a spec requirement with no task, add the task.

## Execution Handoff

After saving the plan, offer execution choice:

**"Plan complete and saved to `<path>`. Two execution options:**

**1. Subagent-Driven (recommended)** - Dispatch a fresh subagent per task, review between tasks, fast iteration

**2. Inline Execution** - Execute tasks in this session using executing-plans, batch execution with checkpoints

**Which approach?"**

**If Subagent-Driven chosen:**
- **REQUIRED SUB-SKILL:** Use subagent-driven-development
- Fresh subagent per task + two-stage review

**If Inline Execution chosen:**
- **REQUIRED SUB-SKILL:** Use executing-plans
- Batch execution with checkpoints for review
