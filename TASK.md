# TASK.md

> Execution checklist for AI coding agents.
> This file is the operational task list.
> Use `TODO.md` as the background spec and trace each task back to its source section.

## Working rules

- Read `TODO.md` first for product/context understanding.
- Use this file as the execution order.
- Every task must include a source section from `TODO.md`.
- Every task must end with validation evidence.
- If a dependency is missing or unclear, stop and report the blocker.

## Task 1.1 — Validate project scope and constraints
Source Section: (1.1)
Related Requirements: (1.3), (2.3), (3.1)
Depends On: none
Priority: P0
Status: [ ] planned
Type: spec / validation
Goal:
- Confirm addon is Meteor client-side only.
- Confirm no Baritone source patching.
- Confirm no server-plugin requirement.
Done when:
- The scope is documented clearly.
- No server-side dependency is required.
Validation:
- Check dependency declarations and docs.
- Verify build system matches Meteor + Fabric only.
Notes:
- If not confirmed, stop and report the blocker.

## Task 1.2 — Finalize addon identity and versioning
Source Section: (1.2)
Related Requirements: (1.1), (2.7), (2.9)
Depends On: (1.1)
Priority: P0
Status: [ ] planned
Type: config / metadata
Goal:
- Choose addon name, mod ID, Java package, JAR naming, version scheme.
Done when:
- Identity and versioning are consistent in metadata.
Validation:
- Open `fabric.mod.json` and confirm values.
- Run build metadata check.
Notes:
- Keep names stable and consistent with target version.

## Task 2.3 — Pin target versions and verify real artifacts
Source Section: (2.3)
Related Requirements: (2.4), (2.5), (2.6), (2.7)
Depends On: (1.1), (1.2)
Priority: P0
Status: [ ] planned
Type: dependency / validation
Goal:
- Lock Minecraft, Meteor, Fabric Loader, Baritone, JDK versions.
- Verify real JARs before build.
Done when:
- Version matrix is fixed and documented.
Validation:
- Inspect Gradle config.
- Check artifact metadata and dependency resolution.
- Run compile smoke test.
Notes:
- Do not assume versions from memory.

## Task 2.4 — Verify Meteor build metadata and checksum
Source Section: (2.4)
Related Requirements: (2.3), (2.6)
Depends On: (2.3)
Priority: P0
Status: [ ] planned
Type: validation
Goal:
- Confirm Meteor build number and commit metadata.
- Save checksum and build details.
Done when:
- The exact JAR and build info are documented.
Validation:
- Compare artifact metadata to the recorded build data.
Notes:
- If artifact cannot be verified, stop and report mismatch.

## Task 2.5 — Verify Baritone compatibility and API contract
Source Section: (2.5)
Related Requirements: (2.3), (4.1), (4.6)
Depends On: (2.3)
Priority: P0
Status: [ ] planned
Type: validation / compatibility
Goal:
- Confirm Baritone tag and commit.
- Verify API compatibility with installed JAR.
Done when:
- Baritone version is pinned and documented.
- Compatibility is confirmed or blocked explicitly.
Validation:
- Inspect dependency metadata.
- Run minimal runtime compatibility smoke test.
Notes:
- Never guess API shape from memory.

## Task 3.1 — Create the Meteor entrypoint and lifecycle foundation
Source Section: (3.1)
Related Requirements: (3.2), (10.3), (13.3)
Depends On: (1.2), (2.3), (2.7)
Priority: P0
Status: [ ] planned
Type: scaffold / integration
Goal:
- Register module entrypoint.
- Register commands.
- Register settings.
- Register event listeners.
- Connect lifecycle hooks.
Done when:
- The addon boots without runtime errors.
- Module loads correctly.
Validation:
- Run dev client or compile test.
- Log startup output.
Notes:
- Keep scope tight; do not add optional features yet.

## Task 4.1 — Build Baritone adapter validation layer
Source Section: (4.1)
Related Requirements: (4.2), (4.6), (4.9)
Depends On: (3.1), (2.5)
Priority: P0
Status: [ ] planned
Type: integration / safety
Goal:
- Detect Baritone runtime availability.
- Verify version and API compatibility.
- Fail gracefully when unavailable.
Done when:
- The adapter can detect missing or incompatible Baritone.
Validation:
- Compile code.
- Trigger missing-Baritone behavior.
- Trigger normal path and confirm readiness.
Notes:
- No assumptions about Baritone API without validation.

## Task 4.2 — Read selection correctly from API
Source Section: (4.2)
Related Requirements: (4.3), (4.4), (4.7)
Depends On: (4.1)
Priority: P0
Status: [ ] planned
Type: logic / validation
Goal:
- Read selection from real API instead of parsing chat text.
- Validate min/max and bounding box.
Done when:
- A valid selection returns expected dimensions.
Validation:
- Unit test no-selection, single-selection, multi-selection cases.
- Run runtime smoke test.
Notes:
- If selection cannot be read, mining logic must not run.

## Task 5.1 — Define tool detection and calibration contract
Source Section: (5.1)
Related Requirements: (5.2), (5.3), (5.7), (6.1)
Depends On: (4.2)
Priority: P1
Status: [ ] planned
Type: logic / calibration
Goal:
- Detect tool identity and server-specific profile information.
- Prepare calibration contract for 3x3 tools.
Done when:
- Tool fingerprint and profile metadata are readable.
Validation:
- Log detected tool information.
- Simulate known vs unknown tool scenarios.
Notes:
- Default to unsafe if tool cannot be validated.

## Task 6.1 — Implement the core Smart Selection Miner workflow
Source Section: (6.1)
Related Requirements: (6.2), (6.8), (6.9), (6.15)
Depends On: (4.2), (5.1), (4.1)
Priority: P0
Status: [ ] planned
Type: feature / safety
Goal:
- Read selected area and tool.
- Prepare a mining plan using safe defaults.
- Produce preview before execution.
Done when:
- The system can create a valid mining plan.
- Strict mode blocks unsafe out-of-bounds blocks.
Validation:
- Run dev-client selection test.
- Confirm preview and state transitions.
- Confirm no mining before confirm.
Notes:
- Default to Safe and Strict until validated.

## Task 15.5 — Phase A implementation milestone
Source Section: (15.5)
Related Requirements: (1.3), (2.7), (3.1), (4.1), (4.2)
Depends On: (1.1), (1.2), (2.3), (3.1)
Priority: P0
Status: [ ] planned
Type: milestone
Goal:
- Deliver skeleton, adapter foundation, command/status layer, selection read path.
Done when:
- The project builds and boots.
- A command/status layer is present.
- Selection can be read and passed to planner logic.
Validation:
- Run compile check.
- Start dev client.
- Confirm command registration and selection output.
Notes:
- Phase A is the minimum usable foundation; avoid scope creep.

## Recommended order

1. (1.1) Validate scope and constraints
2. (1.2) Finalize addon identity and versioning
3. (2.3) Pin versions and verify artifacts
4. (2.4) Verify Meteor metadata
5. (2.5) Verify Baritone compatibility
6. (3.1) Create entrypoint foundation
7. (4.1) Build adapter validation
8. (4.2) Read selection correctly
9. (5.1) Define tool detection and calibration contract
10. (6.1) Build MVP Smart Selection Miner flow
11. (15.5) Phase A milestone

This file is the execution checklist for agent work. `TODO.md` remains the source spec.
