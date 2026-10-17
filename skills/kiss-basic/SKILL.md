---
name: kiss-basic
description: Keep It Simple, Stupid. Keeps small software projects small. Use when someone wants to plan, spec or build a personal tool, household or small-team app, learning project or prototype, especially a local app with a simple database. Also use first whenever a build or spec request does not state its scale. Sizes the project before any spec is written, offers a single Python script with a desktop window and a SQLite file as the default, starts from a desktop icon, and builds in short working steps. Hands off to kiss-advanced only if the project genuinely needs real users over a network or a phone app.
---

# KISS: Basic

KISS: Keep It Simple, Stupid.

## Purpose

Turn an idea into small, working, understandable software, typically in an afternoon. The person should finish with something that runs from an icon on their desktop, and should understand what they built.

The most expensive mistake happens before any code is written: designing for a scale nobody asked for. This skill prevents that first, then keeps the build small.

## Step 1: Size the project before writing a spec

Before proposing any design, establish these five facts. Infer them from the conversation where possible. Ask only for what is missing, one question at a time.

1. Who uses it? Just me, a few people taking turns on one computer, or many people at once?
2. Where does it run? My computer, a shared computer, or on the internet?
3. How much data? Hundreds or thousands of records, or millions?
4. Does it handle other people's sensitive data, payments, or anything regulated?
5. Is it a learning exercise, a prototype, or something people will depend on?

Stay in this skill if the answers are: one person or a few people taking turns, one computer, modest data, and nothing sensitive or regulated.

Recommend kiss-advanced only if at least one of these is true:
- several people use it at the same time from different computers;
- it must be reachable over the internet;
- it must run as an app on a phone;
- it handles other people's personal, financial or regulated data;
- failure would cause real harm or cost.

Say which answer triggered the recommendation, in one sentence. Never escalate because the bigger option "would be more professional" or "could scale later".

## Step 2: Default design

Offer Python as the default, with a one-line reason: it is readable, well suited to learning, and includes everything needed for a desktop app with a database.

If the person prefers another language (one they already know, or one that fits a tool they already use), use it with the same shape.

The default shape is:
- one source file (app.py in Python);
- a desktop window using the language's built-in or standard GUI toolkit (tkinter in Python);
- data in one SQLite file next to the program (sqlite3 in Python);
- no third-party packages.

That is a complete, legitimate application. Do not apologise for it or describe it as a stepping stone.

Each of these deviations is allowed only with a stated reason:
- a third-party package that removes a lot of hand-written code (for example, reading Excel files);
- splitting into two or three files once the main file passes roughly 600 lines;
- a browser-based local interface, only if the person specifically wants one.

Never in basic: web servers, user accounts or logins, REST APIs, Docker, cloud hosting, background workers, ORMs, plugin systems, CI pipelines, configuration frameworks, or separate front-end and back-end code.

On macOS with Python, check tkinter works before building. Python from python.org includes it; Homebrew Python needs the python-tk package installed. On Windows, Python from python.org includes it.

## Step 3: Keep it coherent, even when small

Small does not mean disorganised. Lay out the program in the same sections every time:
1. Settings: file names and constants. Locate the database and other files relative to the program's own folder, not the current working directory.
2. Database: one function per action (create tables, add, list, update, delete).
3. Logic: calculations and rules, with no GUI code.
4. Screens: one function or class per window.
5. Start-up: the main block.

Every feature uses this same pattern. Screens call logic or database functions; screens never contain SQL. When adding a feature, extend the existing sections rather than inventing a new way of doing things. This is what keeps a small app easy to change later.

## Step 4: The spec (planning stage)

The tech spec fits on one page:
- **Purpose:** one or two sentences.
- **Who and where:** the answers from Step 1, including which operating system the person uses.
- **Screens:** each window and what it does.
- **Data:** each table and its fields, in plain words.
- **Actions:** what the person can do (add, search, export, and so on).
- **Out of scope:** what this version deliberately leaves out.
- **Done when:** a short checklist the person can tick by using the app. It always includes "Starts by double-clicking the desktop icon".
- **Build estimate:** expected size in lines and time in hours. If it exceeds roughly 2–3 hours or 600 lines, say so and offer to cut scope before building.

The explainer document is for the person and their users. It covers what the app does, how to put the icon on the desktop and start the app from it, how to use each screen, and where the data lives and how to back it up. It uses plain language, with no architecture talk.

These two files, SPEC.md and README.md (the explainer), are the only documents. Track progress by ticking the "Done when" checklist in SPEC.md. Do not create plans, reports, changelogs or status files.

## Step 5: Build (coding stage)

Work one feature at a time, in this loop:
1. Build the feature.
2. Run the app and use it.
3. Fix what's broken.
4. Commit.
5. Move to the next feature.

**Version control.** Set up Git in the project folder at the start, and commit after each working feature with a plain message such as "Add search box". The person does not need to know how to set up Git. In teaching mode, explain what each Git command did in one line.

**Checks.** Run the app after each feature. Add a small test only for logic that is easy to get wrong, such as dates, money or calculations; a few plain assertions are enough. No test framework, no coverage targets, and no test runs after every edit.

**Data safety.** The database file is the person's data. Never delete or overwrite it. Before any change to the table structure, copy the .db file to a dated backup and say so. Keep test data in a separate file.

## Step 6: Start from an icon, not the command line

A finished basic app must start by double-clicking an icon or shortcut that the person can put on their desktop. Build this as the last feature, for the operating system the person uses. Most people use Windows, so ask if it isn't clear.

The program must find its own files (see Step 3, Settings). A shortcut usually starts the program from somewhere else, so this matters.

Use the simplest launcher for each system:
- **Windows:**
  - Give the Python file a .pyw extension so it opens without a black console window.
  - Create a desktop shortcut to it (Claude can do this with a one-line PowerShell command).
- **macOS:**
  - Create a small double-clickable app (for example with osacompile) that runs the program using the full path to the correct Python.
  - Alternatively, use a .command file, which is simpler but opens a Terminal window.
  - The first time it opens, macOS may ask the person to confirm. Tell them what to expect.
- **Linux:**
  - Create a .desktop launcher file in the applications folder and/or on the desktop, and make it executable.
  - Some desktops ask the person to "Allow launching" once. Tell them what to expect.

Use full paths in every launcher. Test the launcher by actually double-clicking it, or by asking the person to, before calling it done.

A standalone executable (for example with PyInstaller) is an optional extra. Offer it only if the app will be given to someone who doesn't have Python installed.

## Step 7: Finish

When every "Done when" item is ticked:
- start the app from its icon and run through every screen once;
- read through the code for dead code, leftovers, and anything that broke the section pattern;
- confirm README.md matches what was built;
- make a final commit.

Then stop. Suggest at most three ideas for next time, and do not build them.

## Modes: teaching and quiet

**TEACHING** (the default for learners): for each significant choice, give one or two sentences on why, naming the concept. For example: "A SQLite file, not a database server, because only one person uses it at a time." Occasionally note what would change at a larger scale, without doing it.

**QUIET:** build, and report briefly. Explain only when asked.

The person switches by saying "teaching mode" or "quiet mode". When switching, or at the end of a session, make sure the SPEC.md checklist shows the current status, so any later session can pick up the work.

## Warning signs: stop and simplify

- The spec mentions servers, accounts, APIs or deployment.
- The estimate has grown beyond an afternoon.
- A second way of doing the same kind of thing appears in the code.
- More time is going into tests, documents or setup than into features.
