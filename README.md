# prototype

Agent skill `/prototype`: build a throwaway prototype to answer one design question. Part of OneStack.

## What it does

Turns a design question into one self-contained HTML file to look at and judge, on one of two branches:

- **Logic**: a page that drives a state model (free-play buttons plus guided walkthroughs), built over a pure module.
- **UI**: several structurally different variations of one screen, inside a reproduction of the real app's frame, switched with `#A`/`#B`/`#C` and a floating bottom bar.

While prototyping it only reads the project: no writes, no git. When the question is settled, and only if you ask, it implements the validated decision in the real code as ordinary production code, with nothing from the prototype (shell, switcher, flags, sample data).

Based on Matt Pocock's `prototype` skill ([mattpocock/skills](https://github.com/mattpocock/skills) at `4ae2317`, 2026-10-31, MIT), adapted to the Skill Standard: slash-only, references in `references/`, no UI-only metadata.

### Changes from upstream

Kept here so an upstream sync can be merged by hand (setup review, 2026-11-01):

1. **HTML only.** The UI branch builds its variants in one HTML file that reproduces the app's frame, not as variants on a project route behind `?variant=`; upstream's sub-shapes A/B and `NODE_ENV` gating are gone.
2. **No writes, no git.** Upstream's rule 6 (commit the prototype to a throwaway branch, link it from the issue) is removed; nothing is committed, branched or pushed unless the user asks.
3. **Clean fold-in.** New *Folding into the code* section: only on request, the decision is rebuilt as production code; nothing from the prototype is carried over.
4. **Language.** Labels follow the language of the project's documentation (else the user's).
5. **Style.** "One accent colour" replaced by: the project's design system, or monochrome with no accent.

## Usage

```text
/prototype does the refund state machine handle a partial refund after a chargeback?
/prototype three layouts for the settings page
```

Output: one HTML file (path given, opened when possible) with the question stated at the top; later, on request, the validated decision implemented in the project.

## Install

From OneStack: `./sync-install.sh` — or copy this folder as `<skills dir>/prototype/`.

## Requirements

None.

## Layout

- `SKILL.md` — branch choice and the rules for both branches.
- `references/LOGIC.md` — logic branch process.
- `references/UI.md` — UI branch process.

## License

MIT — see `LICENSE`. Original work by Matt Pocock.
