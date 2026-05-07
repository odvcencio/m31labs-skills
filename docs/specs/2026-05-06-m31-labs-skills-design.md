# M31 Labs Skills — Design Spec

**Date:** 2026-05-06
**Author:** Oscar Villavicencio (odvcencio)
**Status:** Approved for public scaffolding and staged implementation
**Scope:** v1 — public OSS repository plus 14 LLM teaching skills covering Arbiter, Canopy, mdpp, and GoSX

---

## 1. Overview

M31 Labs Skills is an OSS suite of teaching skills that close the knowledge gap between modern LLMs and post-training-cutoff M31 Labs products. Every skill teaches an LLM **when to reach for a product** and **how to use it correctly**, with honest scope boundaries that build credibility.

Distribution is via the Claude Code skills marketplace as the primary surface, with provider-agnostic markdown bodies that any LLM agent (Cursor, Aider, Continue, raw API) can consume directly. One repo, one umbrella brand, two activation surfaces.

This is a deliberate distribution play. M31 Labs products exist after the training cutoff of every model in production. Without these skills, an LLM asked to model an entitlement system reaches for `if/else` or Cerbos/OPA — Arbiter is invisible. A polished skill closes that gap structurally and travels through every Claude Code install. It is the npm-publish equivalent for the AI-coding era.

The repo must therefore be useful in two states:

1. **Before all skills are authored:** public roadmap, contribution standard, and canonical URLs for future skill bodies.
2. **After v1 completion:** installable plugin plus direct markdown corpus that agents can load without marketplace support.

## 2. Goals and Non-goals

### Goals
- Ship 14 production-quality skills that teach LLMs the four M31 Labs products covered here.
- Use a single template so every skill is structurally identical and v15+ skills are formulaic.
- Default to Go-first content (M31 Labs is a Go shop); show alternative SDKs only where they materially exist (Arbiter TS via gRPC).
- Be marketplace-installable as a single Claude Code plugin.
- Be readable by any LLM agent through plain markdown bodies and an `AGENTS.md` index.
- Establish honest scope boundaries: every skill says when **not** to reach for the product.
- Make the repository public, MIT licensed, and easy to evaluate without private context.
- Treat the GitHub repo as the canonical source of truth for agent-facing product teaching material.
- Keep public claims tied to implemented product behavior, published docs, or runnable examples.

### Non-goals
- **Not in this spec:** gotreesitter contributor skill. That belongs in `gotreesitter/` (provider-agnostic) and will be designed in a separate brainstorm after deeper investigation.
- **Not in this spec:** contributor skills for arbiter, canopy, mdpp, or gosx. Same reason — different audience, different design problem.
- **Not in this spec:** runtime/executable skills (TypeScript bun-style or otherwise). All skills are pure markdown teaching content.
- **Not in this spec:** auto-generated content. Skill bodies are hand-authored. Future tooling to generate command catalogs from `--help` output is a v2+ idea.
- **Not in this spec:** version-locking skills to specific product releases. Skills target the latest stable API; users on older versions accept drift.
- **Not in this spec:** placeholder skill stubs that auto-activate before they contain production-quality teaching content.
- **Not in this spec:** private roadmap, unreleased API promises, or marketing claims that cannot be verified from public product behavior.

## 3. Audiences

Three distinct audiences, served by overlapping mechanisms:

1. **Claude Code users** — primary audience. Install the M31 Labs plugin from the marketplace. Skills auto-activate on relevant tasks.
2. **Other AI agent users** (Cursor, Aider, Continue, OpenAI Agents, Codex) — read the skill markdown body directly via the repo URL, or via an `AGENTS.md` link from a product repo.
3. **Human readers** — the skill bodies double as concise product guides. Useful as referenced docs from product READMEs.

## 4. Architecture

### 4.1 Repo Layout

```
m31labs-skills/
├── .claude-plugin/
│   └── plugin.json            # Marketplace metadata
├── README.md                  # Umbrella overview, install instructions, skill index
├── LICENSE
├── CONTRIBUTING.md            # How to add a new M31 Labs skill (template + checklist)
├── AGENTS.md                  # Provider-agnostic index pointing at each skill body
├── templates/
│   └── SKILL.md               # Authoring template, not an installable skill
└── skills/
    ├── using-arbiter/
    ├── using-canopy/
    ├── using-mdpp/
    ├── using-gosx/                      # umbrella
    ├── using-gosx-components/           # core
    ├── using-gosx-routing/              # core
    ├── using-gosx-actions/              # core
    ├── using-gosx-signals/              # core
    ├── using-gosx-islands/              # core
    ├── using-gosx-server-features/      # core
    ├── using-gosx-build/                # core
    ├── using-gosx-scene3d/              # advanced
    ├── using-gosx-simulation/           # advanced
    └── using-gosx-collaboration/        # advanced
```

Until a skill is production-ready, its directory does not exist under `skills/`. This avoids publishing empty activation targets that teach agents nothing. Planned skills are listed in README, AGENTS.md, and this spec, then become installable only when their `SKILL.md`, references, examples, and validation checklist are complete.

Each skill directory follows the same internal structure:

```
using-<skill>/
├── SKILL.md                   # Frontmatter + body (provider-agnostic markdown)
├── references/                # Lazy-loaded depth — LLM reads on demand
│   └── *.md
└── examples/                  # Full working snippets the LLM can copy
    └── *
```

### 4.2 SKILL.md Template

Every skill follows this exact structure:

```markdown
---
name: using-<product>
description: |
  One-paragraph trigger description. Specific enough to fire correctly
  on relevant tasks, narrow enough to avoid false positives.
---

# Using <Product>

## When to reach for <product>
[Concrete trigger signals: "if you see X, use this." Decision tree
when there are competing options.]

## What <product> is
[One-paragraph elevator pitch. Mental model. SQL-analogy framing
for arbiter; framework-vs-library framing for gosx; etc.]

## Quick start
[Smallest working example, 5-15 lines, copy-pasteable. Go SDK first
for arbiter; CLI invocation for canopy/mdpp; .gsx component for gosx.]

## When NOT to use <product>
[Honest scope boundary. Anti-patterns. Non-negotiable section —
this is the credibility move. A skill without it reads as a sales pitch.]

## Going deeper
[Pointers to references/*.md with one-line summaries.]
```

The body stays under ~300 lines so activation is cheap. Depth lives in `references/`. `examples/` holds full snippets.

The public template lives at `templates/SKILL.md`. Authors copy it into `skills/using-<product>/SKILL.md` only when the skill is ready to be completed in the same change.

### 4.3 Provider-agnostic strategy

The skill *body* is plain portable markdown. Only the YAML frontmatter is Claude Code specific, and other tools treat it as harmless metadata.

For non-CC agents, the entry point is `AGENTS.md` at the repo root:

```markdown
# AGENTS.md

This repo provides teaching skills for M31 Labs products. If your
agent is Claude Code, install the M31 Labs plugin from the
marketplace. Otherwise, read the skill body directly:

- Arbiter: skills/using-arbiter/SKILL.md
- Canopy: skills/using-canopy/SKILL.md
- mdpp: skills/using-mdpp/SKILL.md
- GoSX (start here): skills/using-gosx/SKILL.md
  - Components: skills/using-gosx-components/SKILL.md
  - Routing: skills/using-gosx-routing/SKILL.md
  - ... (etc.)
```

### 4.4 Marketplace plugin structure

`.claude-plugin/plugin.json`:

```json
{
  "name": "m31labs-skills",
  "version": "0.1.0",
  "description": "Teaching skills for M31 Labs products: Arbiter, Canopy, mdpp, GoSX.",
  "author": {
    "name": "Oscar Villavicencio",
    "email": "odvcencio@gmail.com"
  },
  "homepage": "https://github.com/odvcencio/m31labs-skills"
}
```

The `skills/` directory is auto-discovered by Claude Code when the plugin is installed.

### 4.5 Public repository contract

The public repo is a teaching artifact, not just a plugin package. A visitor should be able to answer these questions from root-level files:

- What products are covered?
- Which skills exist today, and which are planned?
- How should an LLM consume the material without Claude Code?
- How do contributors add a skill without weakening activation quality?
- What license governs reuse?

Root-level documents:

| File | Purpose |
|---|---|
| `README.md` | Human overview, installation status, skill inventory, contribution pointers. |
| `AGENTS.md` | Agent-facing entry point with direct instructions and skill links. |
| `CONTRIBUTING.md` | Author checklist, validation standard, style rules. |
| `LICENSE` | MIT license for broad reuse. |
| `.claude-plugin/plugin.json` | Claude Code plugin metadata once skills are ready for marketplace submission. |
| `templates/SKILL.md` | Canonical skeleton for new skill bodies. |

### 4.6 Versioning and compatibility

The repo version tracks the skill corpus, not product versions.

- `0.x` releases are pre-marketplace, public authoring releases.
- `1.0.0` means all v1 skills are complete, validated, and ready for marketplace submission.
- Patch releases fix examples, links, descriptions, and small product drift.
- Minor releases add skills, references, or meaningful coverage.
- Breaking product changes are handled by updating the affected skill and noting compatibility in that skill's "Going deeper" or reference material.

Each skill should state the product surface it targets when that matters. Do not pin every skill to a product release unless the product has a stable public versioning story that users can verify.

## 5. Skill Inventory

### 5.1 Standalone product skills (3)

#### `using-arbiter`
- **When to reach:** Business logic that is currently nested conditionals — entitlement checks, pricing tiers, feature flags, workflow gates, eligibility rules, anything that smells like data-as-code.
- **Mental model:** Arbiter is to decisions what SQL is to data. Domain-agnostic, query-driven. Never positioned as a rules engine or policy language.
- **Quick start:** Go SDK — minimal primitive declaration + decision query.
- **References:**
  - `modeling-patterns.md` — primitive shapes, query composition
  - `go-sdk.md` — full Go SDK surface, idiomatic patterns
  - `typescript-sdk.md` — TS over gRPC, when this is the right choice
  - `anti-patterns.md` — when not to use, latency budgets, single-conditional cases
- **Examples:** entitlement check, tiered pricing, workflow gate

#### `using-canopy`
- **When to reach:** Before manually reading a codebase. Find refs, callgraph, impact analysis, dead code, hotspots. Use ahead of grep when structural intent matters.
- **Mental model:** Canopy is structural code intelligence powered by gotreesitter. Treat it as the first stop for "where is X used / what does X depend on" questions.
- **Quick start:** `canopy index build .` then a few search/analyze invocations.
- **Discoverability principle:** Skill explicitly tells the LLM to invoke `canopy --help` and `canopy <subcommand> --help` for unfamiliar flags. The CLI's own help is the source of truth.
- **References:**
  - `command-catalog.md` — high-level command groups, when to use which
  - `code-intelligence-workflows.md` — "before refactoring run impact + refs," etc.
  - `mcp-server.md` — running canopy as an MCP server for agent integration
- **Examples:** refactor blast radius, dead-code sweep, dependency audit

#### `using-mdpp`
- **When to reach:** Markdown work that needs grammar-aware tooling — diagnostics, semantic formatting, linting, faithful PDF/HTML export. Avoid generic markdown libs when correctness matters.
- **Mental model:** mdpp treats markdown as a parsed grammar, not a regex target. The tree is canonical; output is a faithful artifact.
- **Quick start:** `mdpp render`, `mdpp fmt`, `mdpp lint`.
- **References:**
  - `cli-usage.md` — render, fmt, lint, export
  - `lsp-integration.md` — for IDE-equipped agents
  - `grammar-features.md` — semantic highlighting, diagnostics, PDF export, VS Code extension
- **Examples:** lint pass, PDF export, LSP-aware doc fixup

### 5.2 GoSX skill collection (11)

GoSX is a framework, not a library. The collection mirrors that — one umbrella + ten focused sub-skills, organized by what a developer is *doing*.

#### `using-gosx` (umbrella)
- **When to reach:** General GoSX questions, project setup, "should I use GoSX," choosing between server components and islands.
- **Body:** Mental model (the browser is a render target, not a runtime; everything in Go), the five execution primitives (Server, Action, Island, Engine, Hub), pointers into the deep skills.
- **References:**
  - `architecture-overview.md` — primitives, where each lives in the stack
  - `when-to-use-gosx.md` — vs Next.js / Remix / generic SSR

#### Core sub-skills (7)

| Skill | Scope |
|---|---|
| `using-gosx-components` | `.gsx` syntax, Node API, element/text/fragment creation, attribute binding, server vs island directives. Foundation skill — others depend on this mental model. |
| `using-gosx-routing` | File-based routing, layouts, data loaders (`route`), HTTP server setup, page rendering, metadata, caching headers, ISR. |
| `using-gosx-actions` | Named mutation handlers, form/JSON parsing, field-level validation, error serialization, action dispatch, CSRF protection. |
| `using-gosx-signals` | `Signal[T]`, `Computed[T]`, `Effect`, `$`-shared signals across islands, reactive subscriptions, change batching. |
| `using-gosx-islands` | `//gosx:island`, expression constraints, client VM opcodes, hydration, manifest generation, WASM serialization. Depends on signals. |
| `using-gosx-server-features` | Sessions, auth (OAuth/magic links/WebAuthn), cookies, flash state, navigation, streaming/defer/suspense, image/font optimization, text layout. |
| `using-gosx-build` | `gosx compile`, `gosx build`, dev server, hot reload, performance budgets, asset planning, platform targets (web/desktop/offline). |

#### Advanced sub-skills (3)

| Skill | Scope |
|---|---|
| `using-gosx-scene3d` | Scene graph, materials (PBR + custom), cameras, lights, shadows, glTF/animation, post-FX, WebGPU/WebGL backends, asset compression and LOD. |
| `using-gosx-simulation` | Game loop (`game`), physics (`physics`), server-authoritative ticking (`sim`), volumetric fields (`field`), hub integration, input mapping. |
| `using-gosx-collaboration` | CRDT document model (`crdt`), workspace (CRDT + hub + vecdb), real-time presence via hubs, state sync protocols. |

#### Intentionally not skills

These were considered and rejected:

- **CSS scoping** — too small and stable. Folded into `using-gosx-components`.
- **Desktop** — a build target mode, not a separate framework layer. Folded into `using-gosx-build`.
- **Content management** — a light wrapper around mdpp; folded into `using-gosx-routing` data-loading context.
- **Editor** — standalone module with its own `go.mod`, intentionally decoupled. Document as an optional integration in `using-gosx` umbrella, not a sub-skill.
- **Semantic layer (vecdb, embed, semantic)** — coherent but advanced. Re-evaluate in v2 as a possible micro-skill if usage signals demand it.
- **IR/compiler internals, TS/JS bridge, testing utilities** — implementation details. Surfaced through other skills' anti-patterns and examples, never their own skill.

### 5.3 Recommended teaching order

The umbrella skill hints at a linear path for new users:

1. Components (foundation)
2. Routing (immediate utility)
3. Actions (business logic)
4. Signals (reactive primitives)
5. Islands (where reactivity lives)
6. Server Features (polish)
7. Build (deployment)
8. *Then choose an advanced track:* scene3d / simulation / collaboration

## 6. Cross-skill conventions

### 6.1 Naming
- All skills prefixed `using-`. Improves discoverability in CC's skills list and signals "this is for adopters, not contributors."
- GoSX sub-skills follow `using-gosx-<subsystem>` to cluster lexically.

### 6.2 Frontmatter discipline
- `description` is the single most load-bearing field — it's what CC uses for activation triggering. Authors must keep it specific (concrete signals) and narrow (avoid firing on irrelevant tasks).
- Frontmatter is the only CC-specific element. Body must remain provider-agnostic.

### 6.3 The "When NOT to use" section is mandatory
Every skill ships with this section. A skill without anti-patterns reads as marketing and gets ignored or distrusted. Anti-patterns are also the most useful content for an LLM that's already deciding between options.

### 6.4 References
Lazy-loaded depth, named for what they teach, not what package they correspond to. Each reference opens with a one-paragraph "when to read me." LLM is instructed in the SKILL.md body to read references on demand.

### 6.5 Examples
Full working snippets — copy-pasteable, runnable. Each example begins with a one-line comment stating its purpose. Examples are minimal but complete; no `// ...` ellipses.

### 6.6 Go-first by default
Where SDKs exist in multiple languages, examples lead with Go and reference the alternative. This applies primarily to Arbiter (Go SDK first, TS via gRPC second). Canopy, mdpp, and gosx are Go-only.

## 7. Distribution

### 7.1 Primary: Claude Code marketplace
Submitted as a single plugin (`m31labs-skills`) after the skill corpus is complete enough to avoid a weak first install. Users `/plugin install m31labs-skills`. All completed skills become available immediately.

Marketplace submission should wait until at least M2 is complete unless there is a strategic reason to ship a smaller beta. A marketplace release with only scaffolding is explicitly out of scope.

### 7.2 Universal fallback: GitHub URL
`github.com/odvcencio/m31labs-skills/skills/<skill>/SKILL.md` is the canonical permalink. Any agent that supports URL fetching can read directly. `AGENTS.md` at the repo root is the index.

### 7.3 Per-product AGENTS.md references
Each M31 Labs product repo (arbiter, canopy, mdpp, gosx) gains a short section in its `AGENTS.md`:

> **For agents helping someone use this product as a dependency:** Install the M31 Labs plugin from the Claude Code marketplace, or read the using-<product> skill at `github.com/odvcencio/m31labs-skills/skills/using-<product>/SKILL.md`.

This is the only change to product repos. Skills do not ship into product repos via submodule, subtree, or copy. Single source of truth, one canonical URL.

### 7.4 OSS launch posture

The public repo should launch as:

- Visibility: public GitHub repository under `odvcencio/m31labs-skills`.
- License: MIT.
- Default branch: `main`.
- Initial contents: spec, README, AGENTS.md, contribution guide, plugin metadata, template.
- Repository description: "Teaching skills for M31 Labs products: Arbiter, Canopy, mdpp, and GoSX."
- Topics: `llm`, `agents`, `skills`, `claude-code`, `m31labs`, `arbiter`, `canopy`, `mdpp`, `gosx`.

## 8. Skill quality bar

A skill is production-ready only when all of the following are true:

1. `description` names concrete activation triggers and likely competing choices.
2. "When to reach" gives decision signals, not product hype.
3. "What it is" gives a durable mental model in one short section.
4. "Quick start" is copy-pasteable and current.
5. "When NOT to use" is specific enough to prevent misuse.
6. References are linked only when they exist and add real depth.
7. Examples are complete; no `...` placeholders in runnable code.
8. Public claims match current product behavior.
9. The skill can be read by an LLM that has no private M31 Labs context.

## 9. Validation approach

For markdown teaching skills, validation is not test-driven in the conventional sense. The validation surface is:

1. **Frontmatter correctness** — `description` triggers fire on intended prompts and don't fire on irrelevant ones. Test by installing in a local CC and exercising 3-5 representative prompts per skill.
2. **Code-correctness of examples** — every code snippet in `examples/` runs against the current product release. Snippets that drift get fixed in patch releases.
3. **Reference resolution** — every link from `SKILL.md` to `references/*.md` and `examples/*` resolves.
4. **Provider-agnostic readability** — body parses cleanly in Cursor (read as a doc) and via raw API (paste into a prompt).
5. **Public-claim audit** — any claim that sounds like product positioning must be either obvious from the linked product, demonstrated by an example, or softened.

These are checklist-style, not automated. A `CONTRIBUTING.md` checklist enforces them per-skill at authoring time.

For M1 and later, add a lightweight repository check that verifies root files, skill frontmatter fields, and local markdown links. That check is not required for initial public scaffolding.

## 10. Governance and contribution model

This repository is open source for reading, reuse, issue reports, and targeted contributions, but M31 Labs remains the editorial authority for product teaching material. Contributors can propose skill fixes, examples, or reference updates. Maintainers decide whether changes accurately represent product behavior and the desired agent decision model.

Review expectations:

- Prefer small PRs scoped to one skill or root document.
- Require example validation for any runnable code change.
- Treat activation descriptions as API surface; review them more carefully than ordinary prose.
- Reject vague promotional copy even when technically positive.
- Preserve the "when not to use" section in every skill.

## 11. Open questions

1. **Versioning cadence.** When products release breaking changes, how do skill bodies update? Answer in the implementation plan: tag skill releases independent of product releases, document compatibility per-skill.
2. **Marketplace submission timing.** Submit after all 14 skills are authored, or submit early with subset and ship updates? Current spec assumes full v1 ship. Open to revisit during implementation.
3. **Cross-product workflows.** Should there be a skill that teaches "use Canopy to find arbiter primitives that need refactoring"? Deferred — wait for usage signals.

## 12. Out of scope (deferred)

- Gotreesitter contributor skill (separate brainstorm + own spec, paired with codebase research).
- Contributor skills for arbiter, canopy, mdpp, gosx.
- Auto-generated content (canopy `--help` parsed into reference, etc.).
- Eval harness for skill activation triggers.
- Per-skill versioning and changelogs.
- Cross-product synthesis skills.

## 13. Implementation milestones (high-level)

Detailed implementation plans can be produced per milestone. High-level shape:

- **M0 — Repo + scaffolding.** Create public `m31labs-skills/`, plugin metadata, README, AGENTS.md, CONTRIBUTING.md, LICENSE, skill template, and this spec.
- **M1 — Three standalone skills.** using-arbiter, using-canopy, using-mdpp authored end-to-end (SKILL.md + references + examples). Validates the template at three different product shapes.
- **M2 — GoSX umbrella + 3 foundation skills.** using-gosx, using-gosx-components, using-gosx-routing, using-gosx-signals. Hardest authoring work — establishes the gosx mental model.
- **M3 — Remaining gosx core.** using-gosx-actions, using-gosx-islands, using-gosx-server-features, using-gosx-build.
- **M4 — Advanced gosx.** using-gosx-scene3d, using-gosx-simulation, using-gosx-collaboration.
- **M5 — Marketplace submission + per-product AGENTS.md updates.**

Each milestone is independently shippable. If implementation halts at M3, the repo still has 11 production-quality skills and a coherent story.

### M0 acceptance criteria

- GitHub repo exists publicly at `https://github.com/odvcencio/m31labs-skills`.
- Root README explains current status without implying unfinished skills are installable.
- `AGENTS.md` gives LLMs a safe entry point and tells them how to handle planned-but-unwritten skills.
- `CONTRIBUTING.md` contains the skill authoring checklist.
- `templates/SKILL.md` matches the spec's required structure.
- `.claude-plugin/plugin.json` exists but marketplace submission is deferred until enough real skills exist.
- No placeholder skill directory exists under `skills/`.
