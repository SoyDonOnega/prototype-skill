# UI Prototype

Generate **several radically different UI variations** of one screen in a single self-contained HTML file, switchable from a floating bottom bar. The user flips between variants in the browser, picks one (or steals bits from each), then throws the file away.

If the question is about logic/state rather than what something looks like, this is the wrong branch. Use [LOGIC.md](LOGIC.md).

## When this is the right shape

- "What should this page look like?"
- "I want to see a few options for this dashboard before committing."
- "Try a different layout for the settings screen."
- Any time the user would otherwise spend a day picking between three vague mockups in their head.

## Make it feel like the real screen

A variant is much easier to judge when it sits inside the rest of the app: real header, real sidebar, real data, real density. An empty page is a vacuum: every variant looks fine. So, reading the project only:

- **Context.** Reproduce, in plain HTML, the frame the screen lives in: the app's header, navigation and neighbouring content, simplified but at the real size and density. When the screen does not exist yet, reproduce the page it would naturally live inside.
- **Data.** Use data shaped like the real thing: field names, lengths, counts and edge cases taken from the project's types, fixtures, seeds or docs. Long names, empty states and the busiest realistic case belong in it.
- **Style.** Copy the project's design tokens (colors, fonts, spacing, radii) into CSS custom properties; reuse the look of its components without importing them.
- Never fetch from the project's APIs or write to it. Everything is inline.

## Process

### 1. State the question and pick N

Default to **3 variants**. More than 5 stops being radically different and starts being noise, so cap there.

Write the plan in one line at the top of the page, e.g. "Three variants of the settings page, to choose the information hierarchy."

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

Open the file for the user. The interesting feedback is usually **"I want the header from B with the sidebar from C"**: add that as a new variant rather than editing the originals, so the comparison stays honest.

### 6. Verdict and fold-in

State which variant won and why. If the user asks to fold it in, follow *Folding into the code* in the [SKILL](../SKILL.md): build the winner with the project's own components, routing and data; none of the prototype's markup, switcher or sample data goes into the project.

## Anti-patterns

- **Variants that differ only in colour or copy.** That's a tweak, not a prototype. Real variants disagree about structure.
- **Sharing too much code between variants.** A shared header is fine; a shared layout defeats the point. Each variant should be free to throw out the layout.
- **A context-free page.** A lone component on a blank page hides the problems the real screen would expose.
- **Real mutations or network calls.** Buttons may change in-page state; the question is "what should this look like", not "does the backend work".
- **Porting the prototype markup to production.** It was written under prototype constraints. Rebuild the winner properly when you fold it in.
