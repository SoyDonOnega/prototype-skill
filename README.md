# prototype

Agent skill `/prototype`: build a throwaway prototype of a new feature, in several versions inside a working replica of your app. Part of OneStack.

## What it does

Turns a new feature into one self-contained HTML file: several structurally different versions of the feature, each inside a working replica of the app it will live in (captured from your screen), five by default, switched with `#A`…`#E` and a floating bottom bar. The app's existing functions keep working in the replica, and the feature works with them.

While prototyping it only reads the project: no writes, no git. Before building it proposes the versions and what will work, and waits for your OK. When you have chosen, and only if you ask, it implements the chosen version in the real code as ordinary production code, with nothing from the prototype (shell, switcher, flags, sample data).

Based on Matt Pocock's `prototype` skill ([mattpocock/skills](https://github.com/mattpocock/skills) at `4ae2317`, 2026-10-31, MIT), adapted to the Skill Standard: slash-only, references in `references/`, no UI-only metadata.

### Changes from upstream

Kept here so an upstream sync can be merged by hand (setup review, 2026-11-01):

1. **HTML only.** Variants are built in one HTML file that reproduces the app's frame, not as variants on a project route behind `?variant=`; upstream's sub-shapes A/B and `NODE_ENV` gating are gone.
2. **No writes, no git.** Upstream's rule 6 (commit the prototype to a throwaway branch, link it from the issue) is removed; nothing is committed, branched or pushed unless the user asks.
3. **Clean fold-in.** New *Folding into the code* section: only on request, the decision is rebuilt as production code; nothing from the prototype is carried over.
4. **Language.** Labels follow the language of the project's documentation (else the user's).
5. **Style.** "One accent colour" replaced by: the project's design system, or monochrome with no accent.
6. **Host app.** A UI screen inside an existing app is built inside a replica of that app, from a screenshot the skill captures itself.
7. **Served link.** The file is shown through a local static server and handed over as a checked `http://127.0.0.1` link (session-long); the plan goes in the chat, not on the page.
8. **UI only.** Upstream's logic branch (state-model demos, `LOGIC.md`) is removed: the skill always builds versions of a feature inside a working replica of the app, and proposes them before building.

## Usage

```text
/prototype a side panel in the desktop app that lists the projects in Developer/Projects.md
/prototype a new layout for the settings page
```

Output: one HTML file, served on a local link that is checked before it is handed over (it lasts the session); later, on request, the chosen version implemented in the project.

## Install

From OneStack: `./sync-install.sh` — or copy this folder as `<skills dir>/prototype/`.

## Requirements

None.

## Layout

- `SKILL.md` — steps and rules.
- `references/UI.md` — the process: working replica, versions, switcher.

## License

MIT — see `LICENSE`. Original work by Matt Pocock.
