---
name: prototype
description: >-
  Builds a throwaway prototype to answer one design question: a single shareable HTML
  file that drives a state model, or several radically different UI variations on one
  route. Use when the user runs /prototype <question> to sanity-check whether a state
  model or logic feels right, or to explore what a UI should look like.
license: MIT
argument-hint: "<design question>"
disable-model-invocation: true
---

# Prototype

A prototype is **throwaway code that answers a question**. The question decides the shape. The skill does not build production code: it answers the question, then folds only the validated decision into the real code.
Paths in this file are relative to the skill root (the folder that contains this SKILL.md).

## When invoked

1. Identify which question is being answered, from the user's arguments, the surrounding code, or by asking the user if they are around.
2. Pick the branch:
   - **"Does this logic / state model feel right?"** → read `references/LOGIC.md`. Build a single shareable HTML file (free-play buttons plus tabbed guided walkthroughs) that pushes the state machine through cases that are hard to reason about on paper, and that a non-developer can drive.
   - **"What should this look like?"** → read `references/UI.md`. Generate several radically different UI variations on a single route, switchable via a URL search param and a floating bottom bar.
3. The two branches produce very different artifacts, so getting this wrong wastes the whole prototype. If the question is genuinely ambiguous and the user is not reachable, default to the branch that better matches the surrounding code (a backend module → logic; a page or component → UI) and state the assumption at the top of the prototype.
4. Follow the branch's process, then apply the rules below.

## Rules

1. **Throwaway from day one, and clearly marked as such.** Locate the prototype code close to where it will actually be used (next to the module or page it is prototyping for) so context is obvious, but name it so a casual reader can see it is a prototype, not production. For throwaway UI routes, obey whatever routing convention the project already uses; do not invent a new top-level structure.
2. **Trivial to run.** A UI prototype starts from one command in the project's task runner: `pnpm <name>`, `python <path>`, `bun <path>`, etc. A logic demo is a single HTML file the user double-clicks. Either way, no thinking required to start it.
3. **No persistence by default.** State lives in memory. Persistence is the thing the prototype is _checking_, not something it should depend on. If the question explicitly involves a database, hit a scratch DB or a local file with a clear "PROTOTYPE, wipe me" name.
4. **Skip the polish.** No tests, no error handling beyond what makes the prototype _runnable_, no abstractions. The point is to learn something fast.
5. **Surface the state.** After every action (logic) or on every variant switch (UI), print or render the full relevant state so the user can see what changed.
6. **Capture it when done.** Fold any validated decision into the real code, then capture the prototype itself as a **primary source**: commit it to a throwaway branch, out of main, and leave a context pointer to that branch on the implementation issue. Capture the answer too (the verdict and the question it settled) in the issue or a commit. The main branch keeps only the validated decision.

## Resources

- `references/LOGIC.md` — logic branch: the shareable single-file HTML demo over a pure, liftable module.
- `references/UI.md` — UI branch: variants on one route behind `?variant=`, the floating switcher, clean-up.
