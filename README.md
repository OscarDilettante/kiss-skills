# KISS Skills for Claude

Four skills that stop Claude from over-engineering software.

Ask Claude for a simple app and it can easily plan something that takes weeks. These skills make it build the simplest thing that works, and build it in a tidy, consistent way.

KISS stands for **Keep It Simple, Stupid**.

## The four skills

| Skill | Use it when… |
|---|---|
| **kiss-basic** | You want a small app, tool, prototype or learning project. Default: one Python file, a desktop window, a simple database, started from a desktop icon. |
| **kiss-advanced** | You're building real software that other people will depend on: web, desktop, iPhone or Android. |
| **project-rescue** | An existing project has got out of control and you need to get it back on track, or decide whether to start again. |
| **project-port** | You want to move an existing system to a new platform or rebuild what it does in a new app. |

Claude picks the right skill automatically. You can also ask for one by name.

## Install

Install in **both** places: the Claude app (for planning) and Claude Code (for building).

### 1. Claude app (Mac or Windows)

1. Download the four skill ZIP files from the [`downloads`](downloads) folder: `kiss-basic.zip`, `kiss-advanced.zip`, `project-rescue.zip`, `project-port.zip`.
2. In Claude, go to **Settings > Capabilities** and turn on **code execution and file creation**.
3. Go to **Customize > Skills**.
4. Click **+**, then **Create skill**, then **Upload a skill**.
5. Upload each ZIP, one at a time.
6. Check all four are switched on.

### 2. Claude Code

Put the four skill folders inside your personal skills folder:

- **Mac:** `~/.claude/skills/`
- **Windows:** `C:\Users\<your name>\.claude\skills\`

Each skill keeps its own folder, for example `.claude/skills/kiss-basic/SKILL.md`.

**Easiest way:** download `all-four-skills.zip` from the [`downloads`](downloads) folder, give it to Claude Code and say:

> Install these skills as my personal Claude Code skills, one folder per skill, then confirm they're listed.

If Claude Code doesn't see them straight away, type `/reload-skills`.

## Use

### Small app

1. **Plan in the Claude app:**
   > Use kiss-basic in teaching mode. I want to build [your idea]. Help me write a tech spec and a simple explainer.
2. **Build in Claude Code:** open an empty project folder and say:
   > Use kiss-basic. Build the app in SPEC.md. Set up Git and commit after each working feature.

You'll end up with a working app you start by double-clicking a desktop icon.

### Bigger project

> Use kiss-advanced. I want to build [your idea] for [who will use it].

Claude will help you agree the scope, choose the technology, and build from the foundations up: architecture, then features, then screens, then branding.

### Project out of control

> Use project-rescue on this project.

Nothing gets deleted. Claude reviews the project first, then helps you decide whether to fix it or start again, and gives you a written plan.

### Moving an existing system

> Use project-port. I want to move [system] to [new platform].

This works even without the original source code.

## Handy phrases

- **"teaching mode"**: Claude explains its choices as it goes. Good for learning.
- **"quiet mode"**: Claude just gets on with it.

## Credits

The idea for these skills came from the [anti-overengineering skill](https://www.skillsdirectory.com/skills/marcmarti9-anti-overengineering) by [marcmarti9](https://github.com/marcmarti9/agentit). Parts of its BUILDER/REVIEW approach are adapted in kiss-advanced and project-rescue. That work is also licensed under the Apache License 2.0; see [NOTICE](NOTICE).

## Licence

[Apache License 2.0](LICENSE).
