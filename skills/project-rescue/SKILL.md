---
name: project-rescue
description: Recovers a software project that has grown out of control, with scope creep, layered or inconsistent architecture, test sprawl, endless review or refactor loops, unclear status, multiplying documents, or cost far beyond progress. Use when the user says a project is out of control, stalled, bloated or unmaintainable, or when kiss-basic or kiss-advanced detect several warning signs. Freezes new features, recovers the original intent, inventories what exists, and either consolidates it to a coherent working state without destroying anything, or produces a route decision comparing repair, partial rebuild, fresh build or a different route, with a new spec, starting prompt and effort estimate.
---

# Project Rescue

## Purpose

Bring an overgrown project back to a state where it does what was intended, works, is built in a consistent way, and can be maintained. Then hand it back to normal development under kiss-basic or kiss-advanced.

Rescue is about removing and consolidating, not adding. It is complete when the project is smaller or clearer, not when it has more in it.

## Ground rules (for the whole rescue)

- **Feature freeze:** no new features until rescue ends.
- **Nothing is destroyed:**
  - Code is removed on a branch and stays recoverable from Git history.
  - Documents are moved to an archive folder, not deleted.
  - Never delete files, databases, containers or volumes without asking first and describing exactly what will be removed.
- **Read before changing:** Phases 1–3 are read-only, apart from the safety snapshot and the rescue report.
- **Approval first:** the developer approves the route and the plan before any code changes.
- **Limited documents:**
  - All rescue findings live in RESCUE.md.
  - The route decision lives in ROUTE-DECISION.md.
  - If a rebuild is chosen, also NEW-SPEC.md and START-PROMPT.md.
  - No other new documents.

## Phase 1: Safety snapshot

- Check Git status. With the developer's agreement, commit or stash any outstanding work.
- Tag the current state (for example, pre-rescue-YYYY-MM-DD) and create a rescue branch.
- Confirm the database and any other data have a backup.
- Confirm tests will run against throwaway data, never real data.
- Record the current baseline: lines of code, number of files, dependency count, test count, and full test runtime.

## Phase 2: Recover the intent

Find the original goal in the spec, the early conversations or the README. Confirm it with the developer as:
- a one-paragraph purpose;
- the operating envelope (users, environment, interfaces, data, availability), as it really is rather than as hoped;
- the must-have feature list for a usable release.

Everything else counts as not must-have for now. The developer confirms this before the rescue continues.

## Phase 3: Inventory (read-only)

Record the following in RESCUE.md.

1. **Features:** each feature, with its status (working, half-built, broken, or not requested) and whether it is a must-have.
2. **Mechanisms:**
   - For each kind of functionality (data models, pages and endpoints, permissions, forms and validation, background work, integrations, configuration, interface contract, presentation), list every distinct way it is implemented.
   - More than one way per kind is a consolidation candidate.
   - Note where the platform's native mechanism was bypassed.
   - Note business rules duplicated across interfaces or placed in presentation code.
3. **Layers:** wrappers, services, interfaces, factories or abstractions that have a single consumer or only forward calls.
4. **Tests:**
   - the count and runtime;
   - which tests protect must-haves;
   - which duplicate each other;
   - which test framework behaviour or implementation details;
   - which are slow or flaky.
5. **Documents:** every plan, report and document, whether it is current, and what it duplicates.
6. **Dependencies:** each one, what uses it, and whether the platform or standard library already covers it.
7. **Process symptoms:** review or refactor loops, repeated full-suite runs, agent or reviewer sprawl, and what triggered each.

## Phase 4: Route decision

This is a meaningful choice between modifying the existing codebase and abandoning it, including reaching the objective by a completely different route. Treat the existing code as a sunk cost: weigh it only by what it can still contribute, not by what it took to build.

### Routes to consider

Include every route that is realistic for this project:
1. **Repair in place:** consolidate and finish the existing codebase.
2. **Partial rebuild:**
   - keep the parts that are sound (often the data model, data, or core logic);
   - rebuild the rest around them;
   - replace sections one at a time while the old system keeps working.
3. **Fresh build, same stack:** start clean on the same language and platform, reusing proven code and lessons.
4. **Different route:**
   - reach the objective another way: a different platform, language or architecture;
   - or an existing product, module or service that already does much of the job, configured rather than built.
   - When this means moving to another platform, hand over to the project-port skill for the detailed carry-over map and port decision, rather than repeating that work here.

### Assessing each route

For each route, set out:
- **Objective:** what it delivers, against the confirmed purpose and must-haves from Phase 2.
- **How:** the platform, architecture and approach, and what is salvaged from the current project.
- **Positive outcomes**, for example:
  - maintainability;
  - extensibility;
  - interoperability with other systems;
  - consistency of functionality;
  - security;
  - performance;
  - lower running or maintenance cost;
  - easier hand-over to other maintainers.
- **Negative outcomes and necessary sacrifices**, for example:
  - features dropped or deferred;
  - work discarded;
  - time to reach parity;
  - data migration effort and risk;
  - learning a new platform;
  - disruption to current users;
  - licensing or service costs.
- **Risks:** what could make the route fail, and how to detect that early.
- **Effort and timescale:** an estimate as a range (best, likely, worst), stating the assumptions, and broken down by stage. Compare it with the rate of progress so far. Say how confident the estimate is.

### The decision document

Write ROUTE-DECISION.md with:
1. **Objective:** the confirmed purpose, operating envelope and must-haves.
2. **Current state:** a short summary of the inventory findings and baseline numbers.
3. **Routes:** each route, assessed as above.
4. **Comparison:** a side-by-side table covering objective met, key outcomes, sacrifices, risks and effort.
5. **Recommendation:** the route recommended, why, and what would change the recommendation.
6. **Decision:** left blank for the developer to complete.

The developer decides. Record the decision and its reasons in decisions.md.

### If a rebuild or different route is chosen

Produce two further documents.

**NEW-SPEC.md**, following the kiss-advanced spec structure (or kiss-basic, if the confirmed envelope is small):
- the operating envelope;
- the language, platform and architecture decision;
- the extension patterns, including the core / interface contract / presentation separation;
- the must-have features;
- what is salvaged from the old project (code, data, designs, lessons);
- a data migration plan, if existing data must carry over;
- the effort and timescale estimate.

The build is sequenced as in kiss-advanced, and each stage finishes before the next begins:
0. **Interface discovery:** anatomical analysis and clickable mockups to confirm requirements, using the old project's screens as input.
1. **Architecture foundation:** platform, data model, extension patterns, security boundaries and interface contract.
2. **Consistent functionality:** the must-have features, each built on the established patterns.
3. **User interface design:** layout, flows, usability and accessibility, applied consistently across features and interfaces.
4. **Branding:** visual identity, applied through the design foundation, not feature by feature.

**START-PROMPT.md:** a ready-to-use prompt for starting the new project in Claude Code. It should:
- point to NEW-SPEC.md;
- name the skill to use (kiss-advanced or kiss-basic);
- state the first stage's scope and its "done when" criteria;
- say which parts of the old project may be read for reuse, and which must be ignored.

The old project is not deleted. It stays tagged in Git as a reference until the new one reaches parity and the developer chooses to retire it.

## Phase 5: Triage plan (repair in place or partial rebuild only)

Assign each inventoried item one action:
- **Keep:** it works and is needed.
- **Finish:** a must-have that is half-built or broken.
- **Consolidate:** move it onto the one chosen mechanism for its kind.
- **Park:** it works but is not a must-have. Remove it from the build on the branch and list it in BACKLOG, so it stays recoverable.
- **Remove:** dead code, unused dependencies, single-use layers, and tests with no value.

For tests:
- keep those that protect must-haves and real risk boundaries;
- quarantine slow or flaky tests;
- remove duplicates and tests written only for coverage;
- agree a target full-suite runtime.

For documents: merge what's current into SPEC.md, decisions.md and README.md, and move the rest to /archive.

Order the work: broken must-haves first, then consolidation, then removals.

Show the plan with the expected before and after numbers, and wait for approval.

## Phase 6: Execute in batches (repair in place or partial rebuild only)

- Work in small batches of related changes, with one commit per batch and a clear message.
- After each batch, run only the checks relevant to it, and confirm the must-haves still work.
- Run the full test suite once per group of batches, not after each change.
- If a batch reveals something the inventory missed, update RESCUE.md. If it changes the plan, check with the developer.
- No "while I'm here" improvements. Anything new goes to BACKLOG.

## Phase 7: Exit

**For a fresh build or a different route,** rescue ends when ROUTE-DECISION.md, NEW-SPEC.md and START-PROMPT.md are approved. The new project then starts under the named skill.

**For repair in place or a partial rebuild,** rescue is complete when:
- every must-have works, with fresh evidence;
- each kind of functionality has one mechanism, recorded in the architecture patterns in SPEC.md;
- the test suite runs within the agreed time;
- a sample of tests fails when their feature is broken;
- documents are reduced to SPEC.md, decisions.md, README.md, RESCUE.md and ROUTE-DECISION.md;
- status is accurate, and the backlog holds everything parked.

Finally:
1. Report the before and after baseline numbers.
2. Add a short decisions.md entry on what the rescue taught about how the project went off course.
3. Hand the project back to kiss-advanced (or kiss-basic), whose scope and warning-sign rules prevent relapse.

## Anti-patterns during rescue

- rewriting large parts without the Phase 4 decision;
- adding new abstractions to "clean up" old ones;
- producing multiple reports or plans;
- review loops that keep finding new nice-to-haves;
- deleting instead of parking or archiving.

## Attribution

Portions of this skill (the BUILDER/REVIEW cycle, justification rule and verification discipline) are adapted and modified from the "anti-overengineering" skill by marcmarti9 (https://github.com/marcmarti9/agentit), licensed under the Apache License 2.0. See NOTICE in the KISS Skills repository.
