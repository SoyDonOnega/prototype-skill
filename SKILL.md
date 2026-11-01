---
name: prototype
description: >-
  Builds a throwaway prototype of a new feature: a single self-contained HTML file with
  several radically different versions of the feature, each integrated into a working
  replica of the app it will live in, switchable from a floating bar. If the user then
  asks, folds only the chosen version into the real code, cleanly. Use when the user runs
  /prototype <feature> to see how a new feature would look and work inside their app.
license: MIT
argument-hint: "<feature to add>"
disable-model-invocation: true
---

# Prototype

A prototype is **a throwaway HTML file that shows a new feature inside the app it will live in**, in several versions to choose from. The app around it is a working replica: its existing functions keep working, and each version of the feature works with them. While prototyping, the skill only reads the project: it never writes into it, and it runs no git command. Once the user has chosen, and only if the user asks, it implements the chosen version in the real code as ordinary production code.
Paths in this file are relative to the skill root (the folder that contains this SKILL.md).

## When invoked

1. **Identify the feature**, from the user's arguments, the surrounding code, or by asking the user if they are around.
2. **Read the project and the app, do not change them.** Take what the prototype needs to feel real: names, data shapes, sample data, design tokens, documentation language. Capture the app the feature lives in (the host app): see *Host app* in `references/UI.md`.
3. **Propose before building.** In the chat, in a few lines: the versions you will build (one line each, how they differ), the existing functions of the app that will work in the replica, and the ones that will stay inert and why. If the user is around, wait for their OK.
4. **Build the HTML** following `references/UI.md` and the rules below. Save it where the user asks; otherwise in the system temp folder.
5. **Serve it and give a working link.** A `file://` page in an embedded browser panel can show as a still image with no JavaScript, so serve the file instead:
   - From the HTML's folder, start a static server in the background: `python3 -m http.server <free port> --bind 127.0.0.1`.
   - Open `http://127.0.0.1:<port>/<file>.html` in the agent's browser panel when it has one.
   - Check that it works before handing it over: switch versions, use the feature's main function and a few of the app's existing functions. Fix what does not respond.
   - Give the user the link and the file path, and say the link lasts only as long as this session; the file still opens by double-click.
6. **Iterate** on the HTML with the user until they choose. State the choice in one or two sentences.
7. **Fold in (only if the user asks).** See *Folding into the code*.

## Rules

1. **One file, nothing to install.** Plain HTML, CSS and JS inline: no framework, bundler or build step. It opens by double-click; the local static server of step 5 only shows it.
2. **Never write into the project while prototyping,** and run no git command (no branch, commit, push or issue). The HTML lives outside the repository unless the user puts it there.
3. **Language:** every label in the language of the project's documentation (README, docs); if the project has none, the user's language.
4. **Style:** the host app's own look, for the replica and for the feature alike. Without a host app, the project's design system when it has one; otherwise monochrome: near-black ink on off-white, gray for secondary text, thin gray lines, highlight by inversion, no accent color.
5. **No persistence.** State lives in memory and is simulated in the page: changes show in the replica for as long as the page is open.
6. **Skip the polish.** No tests, no error handling beyond what keeps the page usable, no abstractions.
7. **Only the app on the page.** The plan and the explanations go in the chat. The page holds the replica, the feature and the version switcher; no question, plan or state bar.

## Folding into the code

Only when the user asks, after the choice.

- Build the chosen version (or the combination the user chose) as ordinary production code with the project's own components, data, structure, conventions and tests, as if the prototype had never existed.
- Carry over the decision, not the prototype: no HTML shell, version switcher, version flags, replica code, prototype names, sample data, stubs, comments about the prototype, or files the decision does not need.
- Touch only the files the decision needs. No git command unless the user asks for it.

## Resources

- `references/UI.md` — the process: the host-app replica and its working functions, the versions, the floating switcher.
