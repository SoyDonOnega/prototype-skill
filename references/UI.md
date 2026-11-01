# Prototype process

Generate **several radically different versions** of one new feature in a single self-contained HTML file, each inside a working replica of the app, switchable from a floating bottom bar. The user flips between versions in the browser, uses the app around them as usual, picks one (or steals bits from each), then throws the file away.

## When this is the right shape

- "Add a projects panel to the app: show me a few options."
- "I want to see a few options for this dashboard before committing."
- "Try a different layout for the settings screen."
- Any time the user would otherwise spend a day picking between three vague mockups in their head.

## Make it feel like the real screen

A variant is much easier to judge when it sits inside the rest of the app: real header, real sidebar, real data, real density. An empty page is a vacuum: every variant looks fine. So, reading the project only:

- **Context.** Reproduce, in plain HTML, the frame the screen lives in: the app's header, navigation and neighbouring content, at the real size and density. When the screen does not exist yet, reproduce the page it would naturally live inside. When that frame is an existing app the user runs, follow *Host app* below.
- **Data.** Use data shaped like the real thing: field names, lengths, counts and edge cases taken from the project's types, fixtures, seeds or docs. Long names, empty states and the busiest realistic case belong in it.
- **Style.** Copy the project's design tokens (colors, fonts, spacing, radii) into CSS custom properties; reuse the look of its components without importing them.
- Never fetch from the project's APIs or write to it. Everything is inline.

## Host app

When the screen lives inside an existing app the user runs, the frame is that app, as the user sees it. It is a replica, not a simplification.

- **Source.** Capture the user's own window yourself, in this order, and stop at the first that works:
  1. **Shell.** On macOS run `screencapture -x <temp folder>/host.png` (whole screen, no sound) and read the image. It also captures the app the agent runs in, such as the Claude desktop app, which screen-control tools leave out. On other systems use the platform's own command-line capture. It needs screen-recording permission for the app that runs the shell; if the image is blank or the command fails, go to the next step.
  2. **Screen-control or browser tool**, when the host is another app or a web page.
  3. **Ask the user** for a screenshot, only when both fail.
  If the host window is not on screen when you capture, ask the user to bring it forward and capture again. Never invent the frame.
- **Plugins and mods.** When the new piece extends the agent's own app (a plugin, mod, pane, panel or slash command of the agent's harness), the host app is that app as the user runs it: desktop app, terminal or IDE. If the capture shows a pane or panel already open, the new piece takes the same place: side, width, header and its buttons. The command or button that opens it works in the page.
- **Look.** Copy the screenshot: theme, layout and proportions, every region of the window, icons, controls, typography, borders, radii and spacing. The new piece uses the host app's own look, not the project's or a default style.
- **The app keeps working.** The existing functions visible in the capture, and those the user would use around the new piece, work in the page with sample data and simulated effects: navigation, tabs, lists, opening and closing panels and panes, toggles, the buttons on screen and what they change. Keep inert only what a page cannot simulate with sense (sending a message to a model, paying, calling an outside service), and name those in the chat (step 3 of the [SKILL](../SKILL.md)).
- **The feature works with the app.** Each function the new piece will have works in the page, with persistence simulated: confirmations, the effect on the data, the feedback the host app gives and a view of the changed data. The way the user opens it, closes it, resizes it or moves around it works. Where the app's state matters to the feature, the feature follows it: change the selection, the session or the tab in the replica and the feature responds.
- **Size.** Build at the size of the screenshot and scale the whole page to fit the preview, so text stays as legible as in the real app.
- **Variants.** Variants change only the new piece. The replica, and what works in it, is identical in every variant.

## Process

### 1. Pick N and agree the plan

Default to **3 variants**. More than 5 stops being radically different and starts being noise, so cap there.

The plan goes in the chat before building (step 3 of the [SKILL](../SKILL.md)), e.g. "Three variants of the projects panel: grouped list, status board, next steps first. The sidebar, tabs and session cards work; sending a message stays inert." The page holds only the replica, the feature and the switcher.

### 2. Generate radically different variants

Draft each variant as its own section of the page, held to:

- The screen's purpose and the data it has access to.
- The project's look (tokens above).
- A short name shown in the switcher, e.g. `B · Sidebar layout`.

Variants must be **structurally different**: different layout, different information hierarchy, different primary affordance, not just different colours. Three slightly-tweaked card grids isn't a UI prototype.

### 3. Wire them together

One variant visible at a time, chosen by the URL hash (`#A`, `#B`, `#C`) so a reload or a shared link keeps the choice:

```js
const variants = ['A', 'B', 'C'];
const names = { A: 'Card grid', B: 'Sidebar layout', C: 'Single column' };
const show = () => {
  const v = variants.includes(location.hash.slice(1)) ? location.hash.slice(1) : variants[0];
  document.querySelectorAll('[data-variant]').forEach(el => { el.hidden = el.dataset.variant !== v; });
  document.getElementById('switcher-label').textContent = `${v} · ${names[v]}`;
};
addEventListener('hashchange', show); show();
```

### 4. Build the floating switcher

A small fixed bar at the bottom centre with:

- **Left arrow**: previous variant (wraps around).
- **Variant label**: key and name, e.g. `B · Sidebar layout`.
- **Right arrow**: next variant (wraps around).

Keyboard `←` and `→` also cycle, except when an `<input>`, `<textarea>` or `[contenteditable]` has focus. Make the bar visually distinct from the design being judged (a high-contrast pill), so it is obviously not part of it.

### 5. Iterate

Give the user the served link (step 5 of the [SKILL](../SKILL.md)). The interesting feedback is usually **"I want the header from B with the sidebar from C"**: add that as a new variant rather than editing the originals, so the comparison stays honest.

### 6. Verdict and fold-in

State which variant won and why. If the user asks to fold it in, follow *Folding into the code* in the [SKILL](../SKILL.md): build the winner with the project's own components, routing and data; none of the prototype's markup, switcher or sample data goes into the project.

## Anti-patterns

- **Variants that differ only in colour or copy.** That's a tweak, not a prototype. Real variants disagree about structure.
- **Sharing too much code between variants.** A shared header is fine; a shared layout defeats the point. Each variant should be free to throw out the layout.
- **A context-free page.** A lone component on a blank page hides the problems the real screen would expose.
- **A dead replica.** An app that only looks right, where nothing but the new piece responds, hides how the feature lives with what the app already does.
- **Real mutations or network calls.** Buttons may change in-page state; the prototype shows how the feature looks and works in the app, not whether the backend works.
- **Porting the prototype markup to production.** It was written under prototype constraints. Rebuild the winner properly when you fold it in.
