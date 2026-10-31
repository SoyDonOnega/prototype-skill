# prototype

Agent skill `/prototype`: build a throwaway prototype to answer one design question. Part of OneStack.

## What it does

Turns a design question into the smallest runnable artifact that answers it, on one of two branches:

- **Logic**: a single self-contained HTML file that drives a state model (free-play buttons plus guided walkthroughs), built over a pure module that can later be lifted into the real code.
- **UI**: several structurally different variations on one route, switched with `?variant=` and a floating bottom bar.

When the question is settled, only the validated decision goes into main; the prototype is kept on a throwaway branch as a primary source.

Based on Matt Pocock's `prototype` skill ([mattpocock/skills](https://github.com/mattpocock/skills) at `4ae2317`, 2026-10-31, MIT), adapted to the Skill Standard: slash-only, references in `references/`, no UI-only metadata.

## Usage

```text
/prototype does the refund state machine handle a partial refund after a chargeback?
/prototype three layouts for the settings page
```

Output: the prototype files next to the code they serve, the one command (or file) to run them, and the question stated at the top of the prototype.

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
