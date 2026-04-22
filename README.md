# recursive-research

> Claude Code **plugin & skill** for recursive research up to PhD level on any topic — science, tech, business, arts, humanities. Source tiering, loop auto-regulation, disk checkpointing, and WDM + Munger inversion for autonomous decisions.

**Version:** 2.0.0 · **License:** [MIT](LICENSE) · **Author:** Joseph Huayhualla ([@Anjos2](https://github.com/Anjos2))

---

## What it does

You give it a **research seed** (a topic) and the skill:

1. Asks for mode (`web` / `local` / `mixed`), local paths if applicable, priority/excluded sources, and a cycle cap.
2. Identifies **3-5 seed threads** applying [WDM (Weighted Decision Matrix) + Munger Inversion](#wdm--munger-inversion).
3. Detects available MCPs (Firecrawl, Context7, WebFetch, WebSearch) and prioritizes by speed and quality.
4. **Iterates in auto-regulated cycles** — each cycle picks the least-covered thread, selects sources, investigates, consolidates.
5. Tiers every source into **Tier 1 / 2 / 3 / Rejected** with transparent criteria.
6. Saves **disk checkpoints** every cycle — survives context compaction.
7. Closes when the **5-criteria PhD fitness function** is met, or upon hitting the cycle cap.
8. Asks if you want to keep going. **Research can be infinite.**

---

## Why it's different

| Feature | How it solves it |
|---|---|
| Works across **any domain** | Generic source tiering (papers, academic books, official archives, raw data), not code-only |
| **Rejects garbage sources** automatically | Explicit criteria: no author, data-less marketing, SEO spam, unsupervised AI content |
| **Survives context limits** | Per-cycle disk checkpoint + `--resume` mode for new sessions |
| **Self-critical** | Munger inversion applied to the consolidated knowledge: what do I not know? what bias do my sources share? what's missing? |
| **Asks before assuming** | Full Phase 0 interrogation of the user |
| **Transparent** | Every non-trivial autonomous decision runs WDM + Munger and shows the reasoning |

---

## Installation

The repo ships as a **Claude Code plugin** (canonical format) that can also be installed as a standalone skill.

### Option A — As a plugin (recommended)

Gives you namespaced invocation (`/recursive-research:recursive-research`) and shows up in `/plugin` management.

**Linux / macOS:**

```bash
git clone https://github.com/Anjos2/recursive-research.git
mkdir -p ~/.claude/plugins/recursive-research
cp -r recursive-research/.claude-plugin recursive-research/skills ~/.claude/plugins/recursive-research/
```

**Windows (PowerShell):**

```powershell
git clone https://github.com/Anjos2/recursive-research.git
New-Item -ItemType Directory -Force -Path "$HOME/.claude/plugins/recursive-research"
Copy-Item -Recurse recursive-research/.claude-plugin,recursive-research/skills "$HOME/.claude/plugins/recursive-research/"
```

Verify with `/plugin` inside Claude Code — you should see `recursive-research` listed.

### Option B — As a standalone skill (minimal)

Only the skill markdown. Invoked without namespace as `/recursive-research`.

**Linux / macOS:**

```bash
git clone https://github.com/Anjos2/recursive-research.git
mkdir -p ~/.claude/skills/recursive-research
cp recursive-research/skills/recursive-research/SKILL.md ~/.claude/skills/recursive-research/
```

**Windows (PowerShell):**

```powershell
git clone https://github.com/Anjos2/recursive-research.git
New-Item -ItemType Directory -Force -Path "$HOME/.claude/skills/recursive-research"
Copy-Item recursive-research/skills/recursive-research/SKILL.md "$HOME/.claude/skills/recursive-research/"
```

Verify with `/help` inside Claude Code.

### Future: official Plugin Directory

Once approved in the official [Claude Code Plugin Directory](https://claude.com/plugins), install will be a single line:

```
/plugin install recursive-research
```

---

## Usage

### Standard invocation

```
/recursive-research:recursive-research     # if installed as plugin
/recursive-research                        # if installed as standalone skill
```

The skill guides you interactively. Answer in natural language.

### Resume a paused research

```
/recursive-research:recursive-research --resume <slug>
```

`<slug>` = kebab-case name of the topic (e.g., `episodic-memory-humans`).

### List saved research

```
/recursive-research:recursive-research --list
```

---

## Usage examples by domain

| Domain | Suggested seed |
|---------|------------------|
| Neuroscience | "Mechanisms of episodic memory in humans" |
| Philosophy | "Modern application of Stoic philosophy" |
| Music | "Minimalism in 20th-century music" |
| Business | "B2B SaaS monetization models in 2025" |
| History | "Fall of the Western Roman Empire: economic causes" |
| Biology | "CAR-T cell immunotherapy against cancer" |
| Technology | "Hexagonal architecture in microservices" |
| Law | "European AI Act and its extraterritorial impact" |

---

## "PhD level" criterion

The skill only declares PhD when **all 5 criteria** are met:

1. **Coverage ≥80%** across all seed threads
2. **≥3 Tier-1 sources per thread**
3. **New-finding saturation ≤5%** for 3 consecutive cycles
4. **Munger inversion applied** to the knowledge (what I don't know, what sources contradict, what biases exist)
5. **≥3 explicit cross-thread connections** between different threads

If any criterion fails, it **does not declare PhD** and keeps iterating — or asks for confirmation upon hitting the cycle cap (default 20, configurable).

---

## Source tiering

| Tier | Qualifies | WDM weight |
|------|-----------|------------|
| **1** | Peer-reviewed papers · academic books · official standards (W3C, RFC, ISO, WHO) · primary archives · official datasets | 5 |
| **2** | Official repos · blogs from citable authors · recorded conferences · Wikipedia with references · reports with methodology | 3 |
| **3** | Blogs with citations to T1/T2 · high-voted forum answers with sources · recorded interviews with identifiable experts | 2 |
| **Reject** | No author · data-less marketing · SEO spam · tutorials without sources · unsupervised AI content | 0 |

Every consulted source is logged in a per-tier file for later audit.

---

## Pre-loaded seed sources

The skill auto-suggests reliable sources by domain. Examples:

- **Science**: arXiv, Semantic Scholar, Google Scholar, Connected Papers, OpenReview
- **Medicine**: PubMed, Cochrane Library, WHO, ClinicalTrials.gov
- **Humanities**: JSTOR, SSRN, Project MUSE
- **Code**: GitHub, Context7, RFCs, W3C specs
- **Data**: World Bank, OECD Data, Our World in Data, Pew Research
- **Art/culture**: Europeana, Google Arts & Culture, Internet Archive, Project Gutenberg

The user can add or reject any before starting.

---

## WDM + Munger inversion

Decision frameworks applied at each non-trivial autonomous step:

- **WDM (Weighted Decision Matrix)** — enumerate 3+ viable alternatives, criteria with weights, 1-5 scoring, compared totals.
- **Munger inversion** (Charlie Munger via Jacobi) — ask inverted about the winning option: *"how would it fail? what bias does it have? what am I ignoring?"*

The skill applies both to:
- Select seed threads
- Select sources per cycle
- Decide when to close the research
- Validate the consolidated knowledge at the end

Reference: [Charlie Munger's essay on mental inversion](https://fs.blog/inversion/).

---

## Recommended MCPs

The skill detects what you have and uses them in this order:

1. **[Firecrawl MCP](https://github.com/mendableai/firecrawl-mcp-server)** (highly recommended) — AI-optimized scraping
2. **[Context7 MCP](https://github.com/upstash/context7)** — official library docs
3. **WebSearch + WebFetch** (built-in Claude Code) — universal fallback
4. **Chrome DevTools MCP** — only when content requires real JS execution; generally **too slow** for large-scale research

---

## Generated files

In `memoria/investigaciones/<slug>/` of the active project:

- `estado.md` · `hilos.md` · `hallazgos.md`
- `fuentes-tier-1.md` · `fuentes-tier-2.md` · `fuentes-tier-3.md` · `fuentes-rechazadas.md`
- `ciclo-01.md`, `ciclo-02.md`, ..., `ciclo-N.md` (checkpoints)
- `sintesis.md` · `acciones.md` · `gaps.md` (upon closing)

**If `memoria/` doesn't exist in the project, the skill creates it** (notifying the user) — it's an explicit dependency.

---

## Repository structure

```
recursive-research/
├── .claude-plugin/
│   └── plugin.json                     ← plugin manifest
├── skills/
│   └── recursive-research/
│       └── SKILL.md                    ← the skill instructions (the actual content)
├── LICENSE                             ← MIT
├── README.md                           ← this file
└── .gitignore
```

---

## Contributing

Issues and PRs welcome. If you improve a criterion, add a tier, find a new anti-pattern, or want support for more domains: open a PR.

### Roadmap

- [ ] Native integration with reference managers (Zotero, Mendeley)
- [ ] Export research to academic formats (LaTeX, BibTeX)
- [ ] Collaborative mode — multiple agents investigating threads in parallel
- [ ] Automatic source-quality metrics (h-index, journal impact factor)
- [ ] Support for PDFs behind legal paywalls

---

## GitHub topics

`claude-code` · `claude-code-skill` · `claude-code-plugin` · `ai-agent` · `research-tool` · `recursive-research` · `knowledge-management` · `weighted-decision-matrix` · `mental-models`

---

## License

[MIT](LICENSE) — use, modify, distribute freely. Just keep the copyright notice.

---

## Acknowledgments

- **Charlie Munger** — for "Invert, always invert" (via Carl Jacobi)
- **Claude Code team** — for the skill and plugin format
- **Anthropic** — for the model that makes this possible

If this skill was useful, consider giving the repo a ⭐.
