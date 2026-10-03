## IGNORE THIS FILE ##

### Instructions to create engineering planner and reviewer

Create two project-level Claude Code subagents with `memory: project`. Only create the agents; do not run them.

**1. engineering-planner**

* Run only when explicitly invoked.
* Read `docs/ContractIQ_PRD.md` fully and strictly follow `skills/engineering-planner/SKILL.md`.
* Generate **only** `docs/engineering/engineering-doc.md`. Do not create implementation specs or other documents.
* Extract every PRD requirement into an internal checklist, including functional, technical, data, security, privacy, retention, user-flow, edge-case, constraint, and acceptance requirements.
* Write the engineering doc, then compare it against the PRD requirement-by-requirement.
* If ANY requirement is missing, vague, incorrect, conflicting, or not technically addressed, fix the document and check again.
* Repeat this self-review → fix cycle until every PRD requirement is fully covered.
* Only after reaching zero gaps, invoke `engineering-reviewer`.
* Store important decisions and run history in project memory.

**2. engineering-reviewer**

* Independently compare `docs/ContractIQ_PRD.md` with `docs/engineering/engineering-doc.md` requirement-by-requirement.
* Return `👍 😊 APPROVED` only when there are zero gaps; otherwise return `❌ NEEDS REVISION` with exact issues.
* If revision is needed, send all issues to `engineering-planner`, which must fix them and invoke the reviewer again.
* Repeat until `👍 😊 APPROVED`.
* Store review history in project memory.

Create both agents under `.claude/agents/` and stop. **Do not generate any document until `engineering-planner` is explicitly invoked.**


### Instructions to create implementation planner and reviewer

Create two project-level Claude Code subagents with `memory: project`. Only create the agents; do not run them.

**1. implementation-spec-planner**

* Run only when explicitly invoked.
* Read `docs/ContractIQ_PRD.md` and `docs/engineering/engineering-doc.md`.
* Strictly follow `skills/implementation-specs/SKILL.md`.
* Generate only the implementation specification under `docs/implementation/`.
* Cover every feature, workflow, technical requirement, API, database change, frontend/backend detail, edge case, and acceptance criterion from both the PRD and engineering doc.
* Before finishing, compare the spec against both source documents requirement-by-requirement.
* If anything is missing, vague, conflicting, or incomplete, fix it and check again.
* Repeat this self-review → fix loop until there are zero gaps.
* Only then invoke `implementation-spec-reviewer`.
* Store important decisions and run history in project memory.

**2. implementation-spec-reviewer**

* Independently compare the generated implementation spec against both `docs/ContractIQ_PRD.md` and `docs/engineering/engineering-doc.md`.
* Return `👍 😊 APPROVED` only if everything is fully and correctly covered.
* Otherwise return `❌ NEEDS REVISION` with exact gaps.
* If revision is needed, send all issues back to `implementation-spec-planner` and repeat until approved.
* Store review history in project memory.

### Setting up a Supabase DB
https://github.com/JohnFrancisPM/MLAI-community-labs/blob/main/Cohort-Labs/cohort-10/Optional%20Lab/01%20-%20ai-app-development-with-claude/02-Building-the-Application-Lab/01-building-the-application/readme.md

Create both agents under `.claude/agents/` and stop. Do not generate the implementation spec until `implementation-spec-planner` is explicitly invoked.
