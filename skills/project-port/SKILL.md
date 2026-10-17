---
name: project-port
description: Ports the functionality of an existing system to another platform, technology or use-case, with or without access to its source code, including closed-source and legacy systems. Use when the user wants to move, re-platform, replace, rebuild or repurpose an existing application, or when project-rescue chooses a different route. Identifies and scopes the functions to reproduce from whatever evidence is available, including lawful reverse engineering, explores ways to improve them, agrees the functionality, then compares port approaches in a decision document and produces a bottom-up spec (architecture through to UI design), a starting prompt and an effort estimate for a new codebase.
---

# Project Port

## Purpose

Create a new system on a target platform or for a new use-case, taking what is valuable from an existing system and leaving its accidental complexity behind. The result is normally a new codebase. It may draw on the source's documentation, code, data, business rules and interface design.

A port is a chance to keep what the source got right and drop what it got wrong. It is not a line-by-line copy.

## Ground rules

- **The source system is read-only.** Never modify, delete or reconfigure it. Read it, run it on a copy, and export from it, but change nothing.
- **Data is migrated from copies.** Migration runs and tests use copies of the source data, never the live source.
- **Reverse engineering informs, it is not copied.**
  - Decompiling, disassembling or otherwise reverse-engineering the source is allowed where the developer has a lawful basis. For example: they own or are licensed to use the system, and the applicable law or licence permits analysis for understanding or interoperability.
  - Before starting, the developer confirms that basis. Claude flags anything uncertain and notes that it is not giving legal advice.
  - The purpose is to understand behaviour, business rules, data formats, file structures and integration protocols, so the new application can deliver an optimised or improved version of the function.
  - **Clean-room output:** findings are written up as behaviour statements, data and format specifications, and rule descriptions. Decompiled or proprietary code, assets and distinctive visual design are never copied into the new codebase. The new implementation is written from the functional specification.
  - Do not bypass copy protection, licence enforcement, authentication or other security controls.
- **Reuse is checked:** before carrying over code, assets, designs or third-party components, confirm the licence and ownership allow it in the target.
- **Limited documents:** findings live in PORT.md, and the decision in PORT-DECISION.md. If a port is approved, also NEW-SPEC.md and START-PROMPT.md. No other new documents.
- **Approval gates:** the developer approves the functional specification before architecture work, and the port decision before any new code is written.

## Phase 1: Define the target

Agree with the developer and record in PORT.md:
- **Why port:** the problem with the current platform or the new opportunity.
- **Target platform or use-case:** what the new system runs on, and who it serves.
- **Target operating envelope:** users, environment, interfaces (web, desktop, mobile web, mobile app), data, availability, lifespan and maintainers, as in kiss-advanced Step 1. Use kiss-basic's sizing if the target is small.
- **Coexistence:** whether the source keeps running during or after the port, and for how long.

## Phase 2: Discover the source (read-only)

### Establish source access

Record which of these is available:
- source code (full or partial);
- binaries or installed program files available for analysis;
- a running instance or test copy the developer can use;
- documentation, manuals, help files and training material;
- screenshots, screen recordings, reports and printed outputs;
- data exports, database schemas, or file formats;
- integration specifications or vendor API documentation;
- users and administrators who know how the system is really used.

### Gather evidence with the methods the access allows

- **Task walkthroughs:**
  - the developer or users carry out real tasks, and Claude records each step, input, output and decision;
  - use screenshots or recordings where possible.
- **Screen anatomy:** for each screen, record what it shows, what it accepts, what actions it offers, and what data it needs.
- **Outputs and data:** reports, exports and file formats reveal the data model and business rules.
- **Rules by observation:**
  - try edge cases on a test copy (invalid inputs, limits, permissions);
  - record the behaviour, including error messages.
- **User knowledge:** what people rely on, what they work around, and what frustrates them.
- **Code:** if available, read it to confirm behaviour, not as a template.
- **Reverse engineering** (when the ground rule's conditions are met):
  - inspect binaries, bytecode, scripts, database schemas, file formats, configuration and network traffic between the system and its integrations;
  - aim to answer specific questions: how a calculation works, how data is stored, what an undocumented rule does, how an integration talks to its partner;
  - record each finding in PORT.md as a behaviour or format statement, with its evidence. Keep extracted code in a separate reference folder, never inside the new project;
  - note where the analysis reveals weaknesses (slow algorithms, poor data structures, security flaws). These become candidates for improvement in Phase 3.

### Record in PORT.md

1. **Functions:** each function the system performs, how much it is used and valued, and the evidence behind it.
2. **Data:** entities, relationships, volumes, data quality, and how data can be extracted.
3. **Business rules:** written as plain behaviour statements, each marked as confirmed (by evidence) or assumed (needs checking).
4. **Interfaces:** screens and flows, and what users rely on.
5. **Integrations:** external systems, imports, exports and APIs.
6. **Pain points:** workarounds, slow tasks, errors, and missing capabilities.
7. **Gaps:** what could not be discovered, and how to resolve each (ask a user, test a case, accept an assumption).

## Phase 3: Scope and improve the functionality

### Carry-over map

For each function, assign one treatment:
- **Reproduce:** the target needs it, and it works well as it is.
- **Improve:** keep the purpose, but do it better.
- **Replace:** meet the underlying need with a different approach.
- **Drop:** not needed for the target use-case.
- **Defer:** needed later, out of the first release.

Parity is defined by the target's must-haves, not by everything the source does. Do not port features, workarounds or complexity just because they exist.

### Explore improvements

For each function marked Improve or Replace, present options with their trade-offs, drawing on:
- the pain points and weaknesses from Phase 2;
- what the target platform and interfaces make possible;
- simpler workflows (fewer steps, fewer screens, better defaults);
- automation of repetitive steps;
- better validation and error prevention;
- reporting and search;
- integration with other systems;
- accessibility.

Keep improvements tied to a real user need. Do not add features because the new platform makes them easy.

### Agree the functional specification

The functional specification records:
- each function in scope, with its treatment and the agreed improvements;
- the business rules;
- the data;
- user tasks and acceptance criteria.

The developer approves it before any architecture or platform work begins. This is the gate between deciding what the system does and deciding how it is built.

## Phase 4: Port decision

### Approaches to consider

Include every approach that is realistic:
1. **Rebuild from the functional specification:** a clean codebase, with the behaviour and data carried over. Works whether or not source code exists.
2. **Translate:** convert source code to the target language or platform, then refactor. Only when the source code is available and sound, and the platforms are close.
3. **Reuse through an interface:** keep part of the source running and expose it to the new system through an interface contract, replacing it later if needed. Only where the source can be kept running and accessed lawfully.
4. **Hybrid:** combine approaches by area, for example rebuilding the interfaces while reusing the data and rules.
5. **Adopt an existing product or platform module** that already covers much of the job, configured rather than built.

### Assessing each approach

For each approach, set out:
- **Objective:** what it delivers against the target envelope and the functional specification.
- **How:** platform, architecture, and what is carried over, adapted or rebuilt.
- **Positive outcomes**, for example:
  - maintainability;
  - extensibility;
  - interoperability;
  - consistency of functionality;
  - performance;
  - security;
  - reach to new interfaces or users;
  - lower running cost.
- **Negative outcomes and necessary sacrifices**, for example:
  - features dropped or deferred;
  - behaviour differences users will notice;
  - migration effort and risk;
  - a period of running two systems;
  - learning the new platform;
  - licensing or service costs.
- **Risks:** what could make it fail, and how to detect that early.
- **Effort and timescale:** a range (best, likely, worst), with stated assumptions, broken down by build stage, and a confidence level.

### The decision document

Write PORT-DECISION.md with:
1. **Objective:** why the port is happening, the target, the envelope and the agreed functionality.
2. **Source summary:** the discovery findings and the carry-over map.
3. **Approaches:** each approach, assessed as above.
4. **Comparison:** a side-by-side table covering objective met, key outcomes, sacrifices, risks and effort.
5. **Recommendation:** the approach recommended, why, and what would change the recommendation.
6. **Decision:** left blank for the developer to complete.

Record the decision and its reasons in decisions.md.

## Phase 5: New spec and starting prompt

NEW-SPEC.md is built from the agreed functional specification, from the bottom up. Each layer is settled before the one above it.

1. **Operating envelope:** users, environment, interfaces, data, availability, lifespan and maintainers.
2. **Architecture:** language, platform and architecture decision; core / interface contract / presentation separation; extension patterns; security boundaries.
3. **Data model:** entities and relationships designed for the target, not copied from the source, plus the data migration plan (extraction, mapping, cleaning, dry runs on copies, verification, cutover).
4. **Core functionality:** each function as behaviour on the established patterns, with its business rules and acceptance criteria.
5. **Interface contract:** how each interface reaches the core, shaped around user tasks.
6. **User interface design:** screens and flows per interface, informed by the source's screen anatomy and the agreed improvements, built on the shared design foundation.
7. **Branding:** visual identity applied through the design foundation.

The spec also includes:
- the coexistence and cutover plan, if the source stays in use;
- open gaps and assumptions still to be confirmed;
- the effort and timescale estimate per layer.

The build then follows the kiss-advanced build sequence: interface discovery (using the source's screens and flows as input), architecture foundation, consistent functionality, user interface design, branding.

**START-PROMPT.md** is a ready-to-use prompt for starting the new project in Claude Code. It should:
- point to NEW-SPEC.md;
- name the skill to use (kiss-advanced or kiss-basic);
- state the first stage's scope and "done when" criteria;
- list exactly which PORT.md findings, documents, screenshots, exports or assets may be used;
- state that the source system must not be modified;
- state that the reverse-engineering reference folder must not be read by the build session, so the new code is written from the specification alone.

## Phase 6: Exit

Port planning is complete when PORT-DECISION.md, NEW-SPEC.md and START-PROMPT.md are approved. The new project then starts under the named skill.

The source system remains untouched until the new system reaches the agreed parity, and the developer chooses to retire it.

## Anti-patterns

- copying the source's structure, workarounds or unused features by default;
- translating tangled code instead of re-implementing its intent;
- copying decompiled logic into the new codebase instead of re-implementing from the specification;
- modifying the source system "just to make export easier" without approval;
- skipping the business-rules capture and rediscovering them as bugs;
- running migration tests against live data;
- producing more documents than listed in the ground rules.
