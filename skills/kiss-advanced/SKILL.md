---
name: kiss-advanced
description: Keep It Simple, Stupid, for production software with a defined user base and operating environment. Use when planning, speccing, building or reviewing an application that real users will depend on, including iPhone and Android apps (networked, multi-user, hosted, or handling sensitive data), or when kiss-basic escalates. Bounds every decision by a written operating envelope, helps the developer choose language, platform and architecture from explicit options, builds one core that web, desktop and mobile interfaces share, presents server/client and security/usability trade-offs as options, builds bottom-up from architecture to branding, and runs a BUILDER/REVIEW cycle with proportional verification.
---

# KISS: Advanced

KISS: Keep It Simple, Stupid. Simple does not mean small here: it means no more than the operating envelope needs, built in one consistent way.

## Purpose

Deliver production software that fits its real operating envelope. It should be robust where it matters, no larger than required, and built from consistent layers that stay maintainable.

Avoid two failures equally:
- speculative enterprise architecture;
- a "bag of features", each built its own way.

## Hard justification rule

Every new component, dependency, abstraction, service, test, document or review pass must trace to at least one of:
1. a current explicit requirement;
2. the operating envelope;
3. an observed bug or regression;
4. an established project pattern or contract;
5. a real risk boundary touched by the change.

"Best practice", "future-proof", "could be useful" and "just to be safe" are not justifications.

## Step 1: Write the operating envelope

Before any design, agree the following with the developer and record it in SPEC.md:
- **Users:** who they are, how many, their roles, and peak concurrent use.
- **Environment:** hosting, existing platforms and infrastructure, devices and browsers.
- **Interfaces:** which of web, desktop, mobile web and mobile app are needed now.
- **Mobile:** whether the app must run on iPhone and/or Android, whether it needs app store distribution, which device features it needs (camera, location, notifications, biometrics, background work), and whether it must work offline.
- **Data:** volume, growth, sensitivity, retention, and legal or regulatory duties.
- **Availability:** acceptable downtime, recovery expectations, and backups.
- **Lifespan and maintainers:** how long the system lives, who maintains it, and their skills.
- **Constraints:** budget, deadline, and integrations.

Ask one question at a time for anything that can't be inferred.

The envelope is the upper bound. Any design beyond it needs an explicit decision from the developer.

## Step 2: Choose language, platform and architecture

This is a decision made with the developer, not a default. Present options and help them decide.

### Decision factors

Assess each candidate against:
- **Envelope fit:** users, concurrency, interfaces, data, availability and hosting from Step 1.
- **Maintainers:** languages and platforms the developer or team already know well. This weighs heavily, because familiarity reduces both build time and maintenance risk.
- **Extension mechanisms:** whether the platform provides consistent ways to add functionality (accounts, permissions, forms, data models, migrations, admin, caching, queues), or the project would have to build them.
- **Hosting and operations:** what the existing infrastructure supports, and the ongoing effort to deploy, update and secure it.
- **Ecosystem health:** maturity, security track record, release and support lifecycle, and long-term viability.
- **Performance needs:** only where the envelope makes them real.
- **Mobile constraints:**
  - store distribution and review;
  - developer account costs (Apple charges an annual fee; Google a one-off fee);
  - Mac hardware required to build iPhone apps;
  - the device features needed;
  - offline needs.
- **Cost and licensing.**

### Architecture shapes to consider

Shortlist only those that fit the envelope:
- a single application on an established platform or framework (for example Drupal, Django, Laravel or Rails);
- a single application on a lighter framework, with explicit layers defined by the project;
- a static or mostly static site, with small server-side functions for the dynamic parts;
- a desktop or local application, when the envelope is really one site or one machine;
- a separate front end and API, only when several clients (web, mobile, third parties) genuinely need the same API.

For mobile, also consider:
- **Mobile web app (PWA):** one web codebase that can be installed to the home screen. Cheapest to build and update, with no store review. Device access and background features are limited, and more so on iPhone.
- **Cross-platform mobile app** (for example Flutter or React Native): one codebase for iPhone and Android, distributed through the stores, with good access to device features. Adds a build toolchain and store release process.
- **Web app wrapped as a store app** (for example Capacitor): reuses an existing web front end with access to device features. Suits a web-first product that also needs a store presence.
- **Native apps** (Swift for iPhone, Kotlin for Android): the best performance and platform integration, but two codebases to build and maintain. Justify only by a specific envelope need.

For mobile, the server-side application or API remains the authority (see Step 4). Choose the smallest mobile option that meets the device-feature and distribution needs in the envelope.

Lean towards a single deployable application on a platform the maintainers know. Move away from it only for a specific envelope requirement, and name that requirement.

### Process

1. Shortlist two or three language/platform/architecture combinations.
2. Lay out the trade-offs side by side against the decision factors.
3. Recommend one, tied to the envelope and the maintainers.
4. The developer decides.
5. Record the decision in decisions.md, including what would make us revisit it.

### Then define consistent layers

Once the platform is chosen:
- Use its native mechanisms for everything it provides. Do not rebuild these.
- If the chosen option has no platform, define the minimal equivalent layers.

Record the project's **extension patterns** in a short ARCHITECTURE section of SPEC.md. This names the one way each kind of functionality is added:
- data entities and models;
- pages, views and endpoints;
- permissions and access checks;
- forms and validation;
- background work;
- external integrations;
- configuration;
- the interface contract (see Step 3);
- presentation and the design foundation (see Step 3).

Rules:
- Every feature uses an existing pattern, or explicitly extends one, and the extension is recorded as a decision.
- Never introduce a second mechanism for a kind of functionality that already has one.
- For project-specific repetition outside the platform's patterns, abstract when the third instance appears and the shared shape is clear. Not before, and not much later.

Coherence is part of minimalism: one consistent mechanism is less code to understand than five clever ones.

## Step 3: One core, many interfaces

A data structure or feature may be used through a web app, a desktop app, mobile web, a mobile app, or several of these. Interfaces and widget sets may also be chosen per audience for the best usability. Design so that adding or changing an interface never requires rebuilding the core or degrades performance.

### Separation

- **Core:** data model, business rules, validation, permissions and workflows. These live once, server-side or in a shared core, and are never duplicated in an interface.
- **Interface contract:** the one way interfaces reach the core. Use the platform's own mechanism where it has one (for example Drupal's JSON:API, or Django REST Framework). Version the contract, and change it deliberately.
- **Presentation:** screens, widgets and interaction, specific to each interface. It contains no business rules, except duplicated validation for responsiveness, which the core still enforces.
- **Design foundation:** shared design tokens (colours, type, spacing) and component definitions. Each interface maps them onto its own widget set. This lets an interface switch or offer alternative widget sets without touching the core, and lets branding change in one place.

### Performance

- Shape the contract around real tasks, not raw tables. Each screen should get what it needs in as few requests as practical, with no over-fetching. Use field selection, included related data, or task-specific endpoints where the platform supports them.
- Cache at the contract and data layers, using the platform's caching mechanisms.
- Paginate and filter server-side.
- For mobile and offline interfaces, sync deltas rather than full datasets.

### Proportionality

Build only the interfaces in the operating envelope. Do not build a mobile app "in case". The separation above is what keeps a future interface cheap, so the seam is required, but the extra interfaces are not.

Where the platform already provides the core/contract/presentation split, use it rather than adding layers of your own.

## Step 4: Present trade-offs as options

For each significant design choice:
1. Give the developer two or three real options, each with its trade-offs.
2. Recommend one, tied to the envelope.
3. Record the choice in decisions.md: the date, the decision, the reason, and what would make us revisit it. Keep entries short.

### Where processing happens: server, client or hybrid

Assess each option against:
- security: what the client can see and tamper with;
- performance: latency, server load and payload size;
- offline use and resilience;
- data exposure;
- complexity;
- the maintainers' skills.

These rules apply whichever option is chosen:
- The server is the authority. Authorisation, validation, pricing and business rules are always enforced server-side, even when also checked in the client for responsiveness.
- Send the client only the data the current user is allowed to see.
- Secrets never reach the client.

Typical guidance:
- For platform-based apps, render server-side by default.
- Move work to the client only for interactivity or responsiveness the envelope requires.
- Prefer hybrid (server-rendered pages with targeted client enhancement) before a full single-page app.

For mobile apps:
- Treat the device as an untrusted client, like a browser.
- Keep tokens and secrets in the platform's secure storage (Keychain on iPhone, Keystore on Android), never in plain app storage.
- If offline use is required, decide the sync and conflict rules explicitly. Store only the data the user needs on the device.

### Security vs usability conflicts

Never solve these by weakening security or by accepting user pain. Name the conflict, then offer a solution that keeps both. Known patterns:

- **Session timeout loses work:**
  - autosave drafts (server-side, or client storage for non-sensitive data);
  - warn before expiry, with an option to extend;
  - re-authenticate in place without leaving the page, then resubmit the preserved form;
  - use a sliding idle timeout with an absolute maximum.
- **Form tokens expire on long forms:** refresh the token on re-authentication, or in the background before submit. Never discard the user's input on a token failure.
- **Strong authentication vs friction:** use passkeys or "remember this device", with step-up authentication for sensitive actions.
- **Repeated logins on mobile vs session security:**
  - use short-lived access tokens with a securely stored refresh token;
  - unlock the app with biometrics instead of re-entering a password;
  - require full re-authentication only for sensitive actions.
- **Rate limiting vs legitimate bursts:** set per-user and per-action limits, with clear messages and retry guidance.
- **Strict content security policy vs third-party widgets:** allow-list specific sources rather than relaxing the whole policy.
- **Detailed errors vs information leakage:** show friendly messages with a reference ID to users, and keep full detail only in server logs.

## Step 5: Build sequence

Build in this order. Each stage finishes before the next begins, because UI and branding built on unstable architecture get rebuilt.

### 0. Interface discovery (lightweight, before architecture is fixed)

Confirm requirements from the user's point of view before committing to data structures:
- **Anatomical analysis:** for each key task, break the screen or interaction into its parts: what the user sees, what they enter, what actions they take, and what data each part needs.
- **Clickable mockups** where they help: quick, low-fidelity flows the developer and users can click through.

These are disposable tools for discovering requirements. They are not production UI, and no styling or branding effort goes into them. Their outputs feed the data model and the feature list.

### 1. Architecture foundation

Platform, data model, extension patterns, security boundaries, and the interface contract.

### 2. Consistent functionality

The must-have features, each built on the established patterns and exposed through the interface contract.

### 3. User interface design

Layout, flows, usability and accessibility, built per interface on the shared design foundation.

### 4. Branding

Visual identity, applied through the design foundation (design tokens and theme), never feature by feature.

## Step 6: Scope and size control

- **Scope:** the SPEC.md feature list for the current phase is the scope. New ideas, from anyone including Claude, go to a BACKLOG section and are not built in this phase.
- **Size:** before building, estimate size and effort per feature. If actual work runs past roughly 1.5 times the estimate, stop and report rather than pushing on.
- **Documents** are limited to:
  - SPEC.md (envelope, architecture patterns, features, status);
  - decisions.md;
  - README.md (how to run, deploy and operate);
  - anything the platform or team convention requires.

  No per-session reports or duplicate plans.

### Launching, installation and distribution (only if applicable)

This applies only when the application runs on users' own machines or phones. Skip the desktop part for hosted web applications. For those, a bookmark, or an installable web app where the envelope justifies it, is the equivalent.

For desktop or local apps, present options with trade-offs:
- desktop shortcuts or launchers per operating system;
- a packaged standalone executable;
- a proper installer.

Weigh them against:
- the number of users;
- the operating systems in the envelope;
- how updates will be delivered;
- operating system trust warnings: unsigned apps trigger macOS Gatekeeper and Windows SmartScreen, and code signing removes these at a cost.

For mobile apps, plan distribution as part of the build, not after it:
- developer accounts;
- test distribution (TestFlight for iPhone, internal testing tracks for Android);
- store listing and privacy declarations;
- review lead times;
- how updates and minimum supported OS versions are handled.

Record these choices in decisions.md.

## Step 7: Build cycle (BUILDER, then REVIEW)

### Minimum-solution ladder

For each feature, stop at the first option that works:
1. Do we need it at all?
2. Does the project's existing pattern cover it?
3. Does the platform provide it?
4. Does an installed dependency provide it?
5. Can a small direct implementation do it?
6. Only then, add new structure.

### BUILDER (the default during implementation)

Work in this loop:
1. Implement the feature.
2. Run the cheapest relevant check: a typecheck, one focused test group, or a quick runtime smoke test. For mobile, a simulator or emulator is enough here.
3. Fix any demonstrated breakage.
4. Commit.
5. Move to the next feature.

Do not do these in BUILDER:
- run the full suite after every edit;
- add tests for trivial glue, static content or framework behaviour;
- refactor unrelated code;
- add speculative abstractions, layers or configuration;
- spawn reviewer committees;
- update documents after every slice.

Add a test when it buys durable confidence: non-trivial business logic, a regression that must not return, a public contract, a critical path, or a risk boundary. Never weaken, skip or delete a valid failing test to get a green result.

**Risk override (applies in any mode).** Verify immediately and specifically when touching authentication, authorisation, payments, destructive data changes, migrations, secrets, production data, concurrency, or irreversible external actions.

### REVIEW (when the phase's features are complete, or on request)

Feature scope is frozen. Review the whole diff and system against:
- acceptance criteria;
- the full test suite and build gates;
- security and trust boundaries;
- migrations and rollback;
- dependency changes;
- performance, where relevant;
- accessibility, for affected user flows;
- pattern consistency (any feature that bypassed the architecture patterns is a finding);
- interface separation: no business rules in presentation code, every interface uses the contract, and design tokens are used rather than hard-coded styles;
- dead code and unrelated churn;
- documentation drift.

For mobile, REVIEW includes testing on at least one real iPhone and one real Android device, where both are in the envelope.

Also check that the tests are worth having. Break a random sample of features and confirm their tests fail, and flag tests that exist only to satisfy coverage.

Fix findings in batches using targeted checks, then rerun the full gate once. Nice-to-haves become backlog items, not review extensions.

Review ends when the acceptance criteria pass with fresh evidence, and every finding is either fixed or recorded.

Do not switch between BUILDER and REVIEW after every feature. Finish the phase's build first, unless a genuine blocking risk appears.

## Modes: teaching and quiet

**TEACHING:** for each significant decision, give one or two sentences on the trade-off and why the chosen option fits the envelope.

**QUIET:** decide within the recorded patterns and envelope, report briefly, and surface only genuine decisions for the developer.

Switch by saying "teaching mode" or "quiet mode". At any mode switch or session end, SPEC.md status must be current, so another session can continue.

## Warning signs: stop and consider project-rescue

- scope creeping past the phase's feature list;
- more than one mechanism for the same kind of functionality;
- verification time dominating build time;
- repeated review or refactor loops;
- unclear status;
- documents multiplying;
- cost or effort far beyond the estimate.

If several are present, recommend switching to the project-rescue skill.

## Attribution

Portions of this skill (the BUILDER/REVIEW cycle, justification rule and verification discipline) are adapted and modified from the "anti-overengineering" skill by marcmarti9 (https://github.com/marcmarti9/agentit), licensed under the Apache License 2.0. See NOTICE in the KISS Skills repository.
