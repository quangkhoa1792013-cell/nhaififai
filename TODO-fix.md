# TODO-fix.md

> Execution checklist for AI coding agents.
> Read this file to perform implementation work.
> For source context, refer back to `TODO.md` and use the `Source Section` field to trace each task to the original requirement.
> Do not treat this file as the product spec. This file is the action list.

## Agent working rules

- Read `TODO.md` first for background context.
- Use this file as the execution list.
- Each task must be traceable to one or more source sections in `TODO.md`.
- Each task must end with explicit validation evidence.
- If requirement or version information is missing, stop and report the blocker.

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
- Confirm no server plugin requirement.
Done when:
- Scope is explicitly documented in project setup and README.
- All implementation decisions remain client-side.
- The project does not rely on server-side code changes.
Validation:
- Check project structure and dependency declarations.
- Confirm no server-side plugin requirement in docs.
- Verify build system matches Meteor + Fabric only.
Notes:
- If the requirement cannot be confirmed, stop and report the blocker.

## Task 1.2 — Finalize addon identity and versioning
Source Section: (1.2)
Related Requirements: (1.1), (2.7), (2.9)
Depends On: (1.1)
Priority: P0
Status: [ ] planned
Type: config / metadata
Goal:
- Choose addon display name.
- Choose mod ID.
- Choose Java package.
- Choose JAR artifact naming.
- Define versioning scheme.
Done when:
- All names and versioning rules are consistent across project metadata.
- `fabric.mod.json` matches selected identity.
Validation:
- Open `fabric.mod.json` and confirm values.
- Build project and check generated metadata.
- Check README for the final naming convention.
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
- Artifact metadata matches the version list.
- Build system uses those exact versions.
Validation:
- Inspect Gradle config and dependency declarations.
- Check actual downloaded JAR metadata or local artifact metadata.
- Run dependency resolution and compilation smoke test.
Notes:
- Do not assume versions from memory; verify with real artifacts.

## Task 2.4 — Verify Meteor build metadata and checksum
Source Section: (2.4)
Related Requirements: (2.3), (2.6)
Depends On: (2.3)
Priority: P0
Status: [ ] planned
Type: validation
Goal:
- Confirm Meteor build number and commit metadata.
- Save checksum, tag metadata, and build details.
Done when:
- Meteor build reference is documented in a stable file or README section.
- The exact JAR file has a checksum record.
Validation:
- Compare the downloaded JAR against the recorded build metadata.
- Record final checksum in a reproducible location.
Notes:
- If the artifact cannot be verified, stop and report mismatch.

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
- API compatibility is confirmed or explicitly blocked.
Validation:
- Check dependency version and source metadata.
- Run a minimal runtime compatibility smoke test.
- Log any API mismatch and stop the build before implementing against wrong APIs.
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
- Meteor module loads correctly.
- Commands and settings are available after launch.
Validation:
- Run dev client or compile test.
- Log startup output.
- Confirm no registration exceptions.
Notes:
- Keep the scope minimal; do not add optional features yet.

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
- Produce a user-friendly failure path if unavailable.
Done when:
- Adapter can detect missing or incompatible Baritone.
- Adapter fails gracefully instead of crashing the client.
- Compatibility is validated before goal execution.
Validation:
- Compile the code.
- Trigger missing-Baritone path.
- Trigger normal path and confirm adapter readiness.
Notes:
- Do not assume Baritone API without runtime validation.

## Task 4.2 — Read selection correctly from API
Source Section: (4.2)
Related Requirements: (4.3), (4.4), (4.7)
Depends On: (4.1)
Priority: P0
Status: [ ] planned
Type: logic / validation
Goal:
- Read selection from the real API instead of parsing chat text.
- Validate bounding box and selection properties.
Done when:
- A valid selection returns the expected min/max and dimensions.
- Edge cases are handled correctly.
Validation:
- Unit test selection values for no-selection, single-selection, and multi-selection cases.
- Run runtime smoke test in dev client.
Notes:
- If selection cannot be read, do not execute mining logic.

## Task 5.1 — Define tool detection and calibration contract
Source Section: (5.1)
Related Requirements: (5.2), (5.3), (5.7), (6.1)
Depends On: (4.2)
Priority: P1
Status: [ ] planned
Type: logic / calibration
Goal:
- Detect tool identity and server-specific profile information.
- Prepare the real calibration contract for 3x3 tools.
Done when:
- Tool fingerprint and profile metadata are readable from the runtime environment.
- The system distinguishes known, unknown, and unsafe tools.
Validation:
- Log detected tool information.
- Simulate a known tool and unknown tool scenario.
Notes:
- Default to unsafe when the tool cannot be validated.

## Task 6.1 — Implement the core Smart Selection Miner workflow
Source Section: (6.1)
Related Requirements: (6.2), (6.8), (6.9), (6.15)
Depends On: (4.2), (5.1), (4.1)
Priority: P0
Status: [ ] planned
Type: feature / safety
Goal:
- Read selected area and tool.
- Prepare the mining plan using safe defaults.
- Produce a preview before execution.
Done when:
- The system can create a valid mining plan from a user selection.
- Strict mode blocks unsafe out-of-bounds blocks.
- Preview is shown before confirm.
Validation:
- Run a small selection test in dev client.
- Confirm preview and state transitions.
- Confirm no mining occurs before user confirm.
Notes:
- Default to `Safe` and `Strict` until validated.

## Task 15.5 — Phase A implementation milestone
Source Section: (15.5)
Related Requirements: (1.3), (2.7), (3.1), (4.1), (4.2)
Depends On: (1.1), (1.2), (2.3), (3.1)
Priority: P0
Status: [ ] planned
Type: milestone
Goal:
- Deliver the project skeleton, adapter foundation, command/status layer, and selection read path.
Done when:
- The project builds at minimum and module boots.
- A command and status layer are present.
- Selection can be read and passed to the planner layer.
Validation:
- Run a compile check.
- Start the dev client.
- Confirm command registration and selection read output.
Notes:
- Phase A is the minimum viable foundation. Do not expand scope beyond the phase gate.

## Task template for future work
Source Section: <TODO section>
Related Requirements: <related IDs>
Depends On: <task ids>
Priority: P0 | P1 | P2
Status: [ ] planned
Type: spec | config | build | validation | feature | integration
Goal:
- <what this task is trying to achieve>
Done when:
- <clear completion condition>
Validation:
- <how to check correctness>
Notes:
- <blockers, constraints, or defaults>

## Recommended working order

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

This order is designed to minimize risk and keep AI work traceable to the original spec.
