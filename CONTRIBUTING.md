# Contributing to RAILYARD

Thanks for taking a look! RAILYARD is a single-file, dependency-free project, which makes it easy to hack on. This guide explains how it's organized and how to add a feature.

## Getting set up

There's no build step and nothing to install.

```bash
git clone https://github.com/YOURUSERNAME/railyard.git
cd railyard
# open index.html directly, or serve it:
python3 -m http.server 8000   # → http://localhost:8000
```

Edit `index.html`, refresh the browser. That's the whole loop.

## How the code is organized

Everything lives in one `<script>` block in `index.html`. The core is a small compiler pipeline:

```
pattern string
     │
     ▼
  parseRegex()         recursive-descent parser
     │  AST
     ├──► explainAST()      → plain-English breakdown
     ├──► renderRailroad()  → railway diagram (SVG)
     ├──► buildNFA()        → Thompson NFA → renderNFA() state diagram
     ├──► buildTrace()      → step-through match trace
     └──► (native RegExp)   → matches, replace, extract, split, profiler, tests
```

The **single source of truth** is the `state` object (`{ pattern, flags, test }`). The `run()` function reads `state` and fans out to every render function. Each render function owns exactly one tab and doesn't touch the others.

A **function map** lives in a comment block at the top of the `<script>` — search the headings (e.g. `3. RAILROAD DIAGRAM`) to jump around.

## Adding a new feature (the pattern)

Say you want to add a new tab. There are four steps, and they mirror the existing tabs so you can copy one as a template:

1. **Add a tab button** in the `.tabs` bar (`data-tab="yourthing"`).
2. **Add a pane** — a `<div class="tabpane" id="pane-yourthing">`.
3. **Write a `renderYourthing(regex)` function** that reads from `state` and fills the pane. Follow the existing render functions for style.
4. **Call it from `run()`** alongside the other render calls, and wire any buttons/inputs at the bottom of the file where the other event listeners are.

Because every view derives from the same AST or `state`, you rarely have to touch anything outside your own function.

## Style conventions

- **Vanilla JS only.** No frameworks, no bundler, no external runtime dependencies — that's a core constraint of the project (it has to run from a single static file).
- **Comment the intent, not the syntax.** Explain *why* a piece of code exists, the way the existing comments do.
- **Keep views independent.** A render function should read from `state`/`regex` and write to its own pane only.
- **Escape anything user-typed** before putting it in the DOM — use the existing `escapeHtml()` helper.
- **CSS uses the variables** defined in `:root` (colors, fonts). Reuse them rather than hard-coding hex values so theming stays consistent.

## Ideas worth picking up

- **DFA conversion** (subset construction) shown next to the NFA
- **Animated match runner** that lights up NFA states as it consumes each character
- **`\p{...}` Unicode property** class support in the parser and explainer
- **Command palette** + keyboard shortcuts
- More **pattern library** entries and **export languages**

## Submitting a change

1. Fork the repo and create a branch: `git checkout -b my-feature`
2. Make your change; test it by opening `index.html` in a couple of browsers.
3. Keep the diff focused — one feature or fix per pull request.
4. Open a PR with a short description of what changed and why.

Bug reports and ideas are just as welcome as code — open an issue any time.

## License

By contributing, you agree your work is released under the project's MIT license.
