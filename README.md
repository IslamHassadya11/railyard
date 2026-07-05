# 🚂 RAILYARD

**A regex playground with a visible engine.** Not just another matcher — RAILYARD compiles your regular expression into a real state machine, shows you how the engine walks your text, and turns it into a full text-processing toolkit.

🔗 **Live demo:** `https://IslamHassadya11.github.io/railyard`

---

## Why it's different

Most regex tools highlight matches and stop there. RAILYARD opens up the machine:

- **Railroad diagram** — your pattern is parsed into an abstract syntax tree and drawn as a flowchart, so structure is visible instead of cryptic.
- **State Machine (NFA)** — the pattern is compiled into a real non-deterministic finite automaton using **Thompson's construction**, then rendered as a state diagram with ε-transitions and accepting states. Genuine automata theory, live in the browser.
- **Match Trace** — step through or animate how a backtracking engine scans your text, character by character.
- **Profiler** — benchmarks the pattern against growing inputs and plots the time curve, so **catastrophic backtracking (ReDoS)** shows up as a visible exponential blow-up.

## Full feature list

**Understand**
- Live match highlighting with all 6 flags (`g i m s u y`)
- Railroad (railway) diagram of the pattern
- Plain-English, token-by-token explanation
- Compiled NFA state-machine diagram
- Step-through / animated match trace
- Capture group + named group inspection

**Transform** (give it text and do things)
- **Replace** — rewrite text with `$1`, `$2`, `$&` back-references, live preview, copy out
- **Extract** — pull all matches (or a chosen group) into a clean list
- **Split** — break text apart on the pattern

**Verify & ship**
- **Profiler** — performance curve + ReDoS detection
- **Catastrophic backtracking** heuristic warning
- **Test suite** — "should match" / "should not match" example lists with green/red pass-fail
- **Code export** — ready-to-paste snippets in JavaScript, Python, Java, Go, Rust, PHP, Ruby, C#

**Convenience**
- Pattern library (email, URL, IPv4, hex color, dates, named groups, and a ReDoS demo)
- Clickable cheatsheet that inserts tokens
- Shareable URL — the pattern, flags, and test string are encoded in the link

---

## How it works (architecture)

RAILYARD is a small compiler pipeline running entirely in the browser:

```
pattern string
      │
      ▼
  ┌─────────┐   recursive-descent parser
  │  Parser │   (alternation → sequence → quantifier → atom)
  └─────────┘
      │  AST
      ├──────────────► Explanation   (AST walk → English)
      │
      ├──────────────► Railroad SVG  (measure pass + draw pass)
      │
      ▼
  ┌─────────┐   Thompson's construction
  │   NFA   │   (fragments with ε-transitions)
  └─────────┘
      │
      ├──────────────► State diagram (BFS layer layout → SVG)
      │
      └──────────────► Match trace + profiler
```

The parser produces an AST covering literals, character classes, groups (capturing, non-capturing, named, look-around), alternation, quantifiers (`* + ? {n,m}`, greedy and lazy), anchors, and escapes. The same AST feeds every visualization, so they always stay in sync.

No frameworks, no build step, no dependencies — one `index.html` file.

---
## Roadmap

- DFA conversion (subset construction) shown next to the NFA
- Animated match runner that lights up NFA states as it consumes each character
- `\p{...}` Unicode property class explorer
- Command palette + keyboard shortcuts


## Run it locally

Open `index.html` in any modern browser. That's the whole setup.

Or serve it:

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

---

## License

MIT — free to use, modify, and share.
