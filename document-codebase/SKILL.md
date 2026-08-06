---
name: document-codebase
description: Generate onboarding documentation for a repository when the user asks to document, explain, map, or onboard a codebase.
---

# document-codebase

Create practical onboarding docs for the current repository.

## Use when
- User asks to document a codebase.
- User asks for architecture/module walkthrough for onboarding.
- User asks for a "start here" guide for engineers new to the repo.

## Output destination (adaptive)
Write docs under `docs/codebase/`.

Always write:
1. `README.md`
2. `gaps.md` (mandatory)

Usually write:
3. `modules/README.md`
4. `modules/<module-slug>.md` (one file per important module)

Write only when useful:
5. `architecture.md`
6. `operations.md`

## Workflow

1. **Map the repo first**
   - List top-level files and dirs.
   - Read existing docs first: `README*`, `docs/`, `AGENTS*`, manifests (`package.json`, `pyproject.toml`, `go.mod`, etc.), and main config files.
   - Identify language(s), likely entry points, test locations, and run/build commands.

2. **Enumerate modules before writing docs**
   - Build a module inventory from top-level packages/apps/services/libs/major dirs.
   - Keep only high-value onboarding modules.
   - Merge tiny/support-only dirs into parent modules instead of creating noise.
   - Exclude generated/vendor/build/cache dirs.
   - Record skipped candidates and reasons in `gaps.md`.

3. **Inspect key files**
   - Read the smallest useful set of files to explain architecture and ownership.
   - Prefer `find`/`grep` to locate real entry points and symbols before broad reading.
   - For important claims, include evidence paths (file paths and symbols) near the claim.

4. **Optional scout/subagent pass (large repos only)**
   - If a `subagent` tool is available, use it only for narrow scouting tasks.
   - At most one scout at a time (sequential only).
   - Scouts provide concise notes with evidence paths.
   - Verify scout claims before final docs; if not verified, mark low confidence in `gaps.md`.
   - If `subagent` is unavailable, proceed in the active agent.

5. **Write docs**
   - Keep docs concise, navigable, and evidence-linked.
   - Prefer concrete paths/symbols over long prose.
   - Use one module file per important module at `docs/codebase/modules/<module-slug>.md`.
   - If uncertain, say so in `gaps.md` instead of guessing.

6. **Sanity check**
   - Re-read generated docs for contradictions and unsupported claims.
   - Verify every cited path exists and every command mentioned was actually run or clearly marked unverified.
   - Avoid absolute claims like "does not exist" unless confirmed with `find`/`ls`.
   - Fix obvious issues.

7. **Dry-run locating-code tasks**
   - Simulate 3-5 realistic "where is this code?" onboarding questions.
   - Check that the docs point to the right files/symbols quickly.
   - If the docs fail this test, revise the docs before finishing.
   - Final response should list written files and the most important remaining gaps.

## File content requirements

### `docs/codebase/README.md` (always)
- Repo purpose in 2-5 bullets.
- "Start here" reading order (numbered list of key files/dirs).
- Key local commands (build/test/run if found).
- Links to generated docs (at least `gaps.md`, module index, and any architecture/operations docs created).

### `docs/codebase/modules/README.md` (usually)
- Table of documented modules.
- For each module: 1-line responsibility, path scope, and link to module doc.
- Note major skipped modules and why (short).

### `docs/codebase/modules/<module-slug>.md` (per module)
- Responsibility/ownership.
- Entry points (paths/symbols/routes/commands).
- Key files/components (small table with evidence paths).
- Main internal flow (short, practical).
- Integrations/dependencies (inbound/outbound where identifiable).
- Module-specific unknowns.

### `docs/codebase/architecture.md` (only when useful)
- Entry points (CLI/server/jobs/scripts) with paths.
- Runtime flow and main data flow (short sections).
- Major boundaries/layers and integrations.
- Evidence paths for each major claim.

### `docs/codebase/operations.md` (only when useful)
- Install/build/test/run commands discovered in repo.
- Config/env requirements and where they are defined.
- Deployment/runtime notes only if present in source/docs.

### `docs/codebase/gaps.md` (always)
- Skipped areas and why.
- Low-confidence claims and what would verify them.
- Brief collection limitations only when they affect onboarding accuracy.
- Best next files to read for highest confidence gain.
- Prefer user-relevant onboarding unknowns over tool/runtime noise.

## Confidence rules
- Don’t claim full coverage unless the repo is truly small and fully inspected.
- Every major architecture/module claim should cite an evidence path in `path[:line]` or `path + symbol` form when possible.
- Unknowns go in `gaps.md`.
- Prefer explicit verification over inference when a claim affects onboarding navigation.

## Constraints
- No custom slash commands.
- No scheduler, no resumability/state machine, no concurrency orchestration.
- The active agent is the coordinator; this skill is instruction-only.
- Do not require subagents; treat them as optional helpers when available.
