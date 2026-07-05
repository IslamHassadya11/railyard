# 🚂 RAILYARD

**A regex playground with a visible engine.** Not just another matcher — RAILYARD compiles your regular expression into a real state machine, shows you how the engine walks your text, and turns it into a full text-processing toolkit.

🔗 **Live demo:** `https://YOURUSERNAME.github.io/railyard`

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

## Run it locally

Open `index.html` in any modern browser. That's the whole setup.

Or serve it:

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

---

## Publish it online (free)

### GitHub Pages — recommended

**Drag-and-drop (no command line):**

1. Create a **free** account at [github.com](https://github.com) if you don't have one.
2. Click **+ → New repository**, name it `railyard`, keep it **Public**, click **Create repository**.
3. On the repo page, click **"uploading an existing file"**.
4. Drag `index.html` and `README.md` in, then **Commit changes**.
5. Go to **Settings → Pages → Source: Deploy from a branch → main → / (root) → Save**.
6. Wait ~60 seconds. Your live link appears at the top of the Pages settings:
   `https://YOURUSERNAME.github.io/railyard`

Share that link with anyone — it works on phones, laptops, anything with a browser.

**Command line:**

```bash
git init
git add index.html README.md
git commit -m "RAILYARD — regex playground"
git branch -M main
git remote add origin https://github.com/YOURUSERNAME/railyard.git
git push -u origin main
```

Then enable Pages as in step 5 above.

### Other free hosts

Any of these will host a single HTML file for free, often with drag-and-drop:

- **Netlify Drop** — drag the folder onto [app.netlify.com/drop](https://app.netlify.com/drop), instant public URL.
- **Vercel** — import the repo, deploys automatically.
- **Cloudflare Pages** — connect the repo, free tier.
- **GitHub Gist + gistpreview** — quickest for a single file.

---

## Roadmap

- DFA conversion (subset construction) shown next to the NFA
- Animated match runner that lights up NFA states as it consumes each character
- `\p{...}` Unicode property class explorer
- Command palette + keyboard shortcuts

## License

MIT — free to use, modify, and share.
