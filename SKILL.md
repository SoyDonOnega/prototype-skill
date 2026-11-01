---
name: prototype
description: >-
  Builds a throwaway prototype to answer one design question: a single self-contained HTML
  file to look at and judge, either driving a state model or showing several radically
  different UI variations. If the user then asks, folds only the validated decision into
  the real code, cleanly. Use when the user runs /prototype <question> to sanity-check
  whether a state model or logic feels right, or to explore what a UI should look like.
license: MIT
argument-hint: "<design question>"
disable-model-invocation: true
---

# Prototype

A prototype is **a throwaway HTML file that answers a question**. The question decides its shape. While prototyping, the skill only reads the project: it never writes into it, and it runs no git command. Once the question is settled, and only if the user asks, it implements the validated decision in the real code as ordinary production code.
Paths in this file are relative to the skill root (the folder that contains this SKILL.md).

## When invoked

1. **Identify the question**, from the user's arguments, the surrounding code, or by asking the user if they are around.
2. **Pick the branch:**
   - **"Does this logic / state model feel right?"** → read `references/LOGIC.md`. One HTML file with free-play buttons and guided walkthroughs that push the state model through the cases that are hard to reason about on paper.
   - **"What should this look like?"** → read `references/UI.md`. One HTML file with several radically different variations of the screen, switchable from a floating bar.
   If the question is genuinely ambiguous and the user is not reachable, default to the branch that better matches the surrounding code (a backend module → logic; a page or component → UI) and state the assumption at the top of the prototype.
3. **Read the project, do not change it.** Take what the prototype needs to feel real: names, data shapes, sample data, design tokens, documentation language.
4. **Build the HTML** following the branch's process and the rules below. Save it where the user asks; otherwise in the system temp folder. Give the path and open it if you can.
5. **Iterate** on the HTML with the user until the question has an answer. State the verdict in one or two sentences.
6. **Fold in (only if the user asks).** See *Folding into the code*.

## Rules

1. **One file, nothing to install.** Plain HTML, CSS and JS inline: no framework, bundler or server. It opens by double-click.
2. **Never write into the project while prototyping,** and run no git command (no branch, commit, push or issue). The HTML lives outside the repository unless the user puts it there.
3. **Language:** every label and explanation in the language of the project's documentation (README, docs); if the project has none, the user's language.
4. **Style:** follow the project's design system (colors, fonts, spacing) when it has one. Otherwise monochrome: near-black ink on off-white, gray for secondary text, thin gray lines, highlight by inversion, no accent color.
5. **No persistence.** State lives in memory. If the question is about persistence itself, simulate it in the page.
6. **Skip the polish.** No tests, no error handling beyond what keeps the page usable, no abstractions.
7. **Surface the state.** After every action (logic) or on every variant switch (UI), render the full relevant state so the user sees what changed.
8. **State the question** at the top of the page, in one visible paragraph.

## Folding into the code

Only when the user asks, after the verdict.

- Implement the validated decision as ordinary production code in the project's language, structure, conventions and tests, as if the prototype had never existed.
- Carry over the decision, not the prototype: no HTML shell, variant switcher, variant flags, prototype names, sample data, stubs, comments about the prototype, or files the decision does not need.
- Logic branch: rewrite the validated pure module in the project's idiom; do not paste the page's script.
- UI branch: build the winning variant (or the combination the user chose) with the project's own components and data; do not port the prototype markup.
- Touch only the files the decision needs. No git command unless the user asks for it.

## Resources

- `references/LOGIC.md` — logic branch: a pure, liftable module inside a single-file demo.
- `references/UI.md` — UI branch: variants in one file, the floating switcher, how to make them feel like the real screen.
