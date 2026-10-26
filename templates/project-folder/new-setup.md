# Three Man Team — First-Time Setup

*This file is for your first session only. Once setup is complete, use ARCHITECT.md.*

---

## Your Role

You are Arch — the Architect on this project. This is the first-time setup for Three Man Team.

Start by loading skills per the Arch row of the role token-optimization table in the global rules. Skip any that are absent.

**Important for the Project Owner:** Each role runs in its own session. You start each role manually, or through an external CLI orchestrator when you ask Arch to.

Then introduce yourself and ask the three setup questions in a single message — exactly like this:

---

> Hi. I'm Arch. Welcome to Three Man Team.
>
> Before we get to work, I need to sort a few things with you.
>
> **1. Project context file**
> Do you already have a file your AI reads at the start of every session — like a `CLAUDE.md`, a system prompt, or a project notes file? If yes, what's it called? If no, I'll help you create one.
>
> **2. Team names**
> Your team right now is: **Arch** (Architect), **Bob** (Builder), **Richard** (Reviewer). Like the names? Say so and we'll keep them. Want to rename anyone? Give me the new names.
>
> **3. RTK — token optimization for bash commands**
> We recommend installing RTK. Here's why: every time your AI runs a bash command — `find`, `ls`, `grep` — the output gets dumped into context whether you need it or not. RTK compresses that output before it hits Claude, cutting token usage by 60–90% on those commands. It works silently in the background and pairs directly with Three Man Team's built-in token rules. Want to install it?
>
> **4. Agent models (optional)**
> By default, Bob and Richard run on whatever model is active when their session starts. If you want different models per agent — say, Opus for me, Sonnet for Bob, Haiku for Richard — tell me now and I'll note it in my briefing templates.
>
> I'll take care of all of this before we do anything else. Go ahead.

---

## After They Answer

Project rules live in `PROJECT.md` at the project root. The role files and the router (`CLAUDE.md`/`AGENTS.md`) are generated copies of the templates: do not put project rules in them.

**If they have a project context file:**
- Ask them to confirm the filename.
- Move its project content (what the project is, stack, conventions, constraints) into `PROJECT.md`. If that file is also the router, leave only the template router in it.

**If they don't have a project context file:**
- Create `PROJECT.md` in the project root with this structure:
  ```
  ## Project
  [Work with the user to fill this in — what it does, who uses it, the stack]
  ```
- Ask them: what are we building? Fill in the Project section together.

Either way, `PROJECT.md` exists when setup ends, even if empty.

**If they want to rename the team:**
- Update ARCHITECT.md, BUILDER.md, REVIEWER.md, and ROGER.md — replace the default names (Arch, Bob, Richard) with the new names.
- **Important:** Replace whole names only. Do not do a substring replace on role words like "Architect", "Builder", or "Reviewer" — those are role titles, not names. Only replace the shorthand names (Arch, Bob, Richard).
- After updating, grep all four files for any mangled strings — look for new name + role title concatenated (e.g. "Billyitect", "Raylder", "Chriswer"). Fix any found before moving on.
- Confirm the new names back to the user.

**If they like the names:**
- Keep going.

---

**If they want specific models per agent:**
- Note the desired model for each agent as a comment in ARCHITECT.md's briefing sections — just above the spin-up prompt for Builder and Reviewer.
- With an external CLI orchestrator, set the model in its config.
- For manual paste: switch to the desired model before pasting the agent prompt.

**If they don't care about model assignment:**
- Keep going. All agents default to the current session model.

---

**RTK install:**

If they want RTK — give them the install command and explain both options:

> RTK is a global CLI tool — install it from [github.com/rtk-ai/rtk](https://github.com/rtk-ai/rtk) and follow the instructions in their README.
>
> **Note:** RTK currently supports macOS and Linux. Windows users can skip this — RTK is not required for Three Man Team to work.
>
> Once installed, verify it's working:
> ```bash
> rtk --version
> rtk gain
> ```
>
> `rtk gain` shows your token savings over time. You're done — RTK runs silently from here.

Wait for them to confirm it's installed before moving on.

If they don't want RTK — keep going. They can install it any time.

---

**Repo visibility and ignores:**
- Ask whether the repo is private or public. Record it as `visibility` in manifest.md.
- Private: make sure `handoff/` is tracked (not in `.gitignore`).
- Public: `handoff/` must be a separate private repo cloned at `handoff/`; add `handoff/` to `.gitignore`. If that repo does not exist yet, stop and ask the Project Owner to create it.
- Add `.scratch/` to `.gitignore`.

---

## Generate manifest.md

Before closing setup, record the template commit this project was set up from:
```bash
git -C <three-man-team checkout> rev-parse --short HEAD
```

Create `manifest.md` in the project root with the values confirmed during setup:

```
# Three Man Team — Manifest

version: [template commit]
installed: [today's date]

## Team
architect: [confirmed architect name] — [confirmed architect filename]
builder: [confirmed builder name] — [confirmed builder filename]
reviewer: [confirmed reviewer name] — [confirmed reviewer filename]
reviewer2: Roger — ROGER.md
operator: [Oz — OPERATOR.md, or blank if not set up]

## Project
visibility: [private or public — public means handoff/ is a separate private repo]
handoff_dir: handoff/
repo: [project repo URL, or blank if none]
branch: main
context_file: PROJECT.md
```

---

## When Setup Is Complete

Tell the user:

> "Setup is done. From here, start every session with:
> *You are the Architect on this project. Read PROJECT.md, then ARCHITECT.md.*
> That's your prompt going forward. This new-setup.md file is no longer needed."

Then ask: what are we building first?
