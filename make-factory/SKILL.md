---
name: make-factory
description: Build a new factory — a repeatable pipeline turning an idea into one finished, quality-assured artifact type. Use for 'make a factory for X', 'this is a factory to make', '/make-factory', or when repeated multi-step output should become a factory.
---

# make-factory

The meta-process for standing up a new sibling in the factory family. A factory turns a fuzzy idea
into a finished, quality-assured artifact of ONE output type — *arrive at an idea → say ship it →
know it's in good hands.* Existing legs: **code**, **research**, **landing-page** (see
`factories/README.md`).

The build is cheap because you don't hand-author the standards — the **research factory generates
them**, and this skill supplies the conventions + the skeleton.

## Step 0 — the fit gate (say no here if it fails)

**A factory only pays off where the output's *lit state* is machine-checkable.** The medium is light:
code is inert until run, a page until a user flows through it. Ask: *can a machine verify the
activated state (runs / renders / loads / reconciles / passes a spec)?*
- **Yes** → build it.
- **No** (taste-dominant: a song's soul, prose voice, a logo's beauty) → **don't build a factory** —
  build a helper for the mechanical spine only, and say so. A factory whose verify can't check what
  matters is a false-confidence checklist.

Also gate on: is this **repeated** (not one-off) and **worth** the build? One-off → just do it.

## Step 1 — FRAME the two registers

Name the output, then split its judgment layer by evidentiary strength (this is the honest core):
- **Verify layer** — the parts with real, machine-checkable backing → **hard gates**.
- **Advisory layer** — the parts that are taste/hypothesis (no controlled evidence) → **flag as
  suggestions, never assert as truth**. (Landing-page example: CWV/WCAG/SEO are gates; conversion copy
  is a hypothesis to A/B test — >50% of CRO ideas fail when tested.)

## Step 2 — RESEARCH the standards (don't hand-author)

Run the **`research` factory** (survey mode) on "best practices for <output>", asking specifically for
the machine-checkable bars (the verify layer) and the structural/quality criteria (the reviewer). The
brief IS the pack content. Check the research library first — a prior brief may already cover it.

## Step 3 — SCAFFOLD the spine

Every factory is the same shape, keyed to its output:
```
FRAME → PRODUCE → VERIFY (the lit state) → COMPOUND (memory) → SHIP
```
Its own thin sibling repo (`<output>-factory`), matching the family:
- `bin/verify-*` — the **lit-state prover** (the one genuinely-new bit): runs the real checks from the
  research brief, pass/fail, exit nonzero on fail, `--selftest` for the gate logic.
- `reviewers/<output>-reviewer.md` — structural/quality presence (fix) + advisory items (flag, don't
  assert). Opus, read-only.
- `commands/ship-<output>.md` — the create-flow the user always does, made repeatable.
- `README.md` — what it makes, the two-register table, the honest ceiling.

## Step 4 — apply the CONVENTIONS (below)

## Step 5 — SHIP + REGISTER

Commit, create a **private** repo (`gh repo create dylanpulver/<output>-factory --private`), and add a
row to `factories/README.md`'s registry table. Globalize any command that should be callable anywhere.

---

## Factory-building conventions

How to build a factory *well*. (Grounded in the agentic-pipeline research: Anthropic's effective-agents
taxonomy — simplicity-first; Berkeley compound-AI — generate-then-verify beats bigger models;
verifiability + tool-density predict fit. See `research-factory/library/`.)

**Architecture**
- **Workflow, not free agent.** Predefined steps with judgment layers beat an open-ended agent. Add
  agentic complexity only when it *demonstrably* improves outcomes.
- **Deterministic tools discover + verify; the LLM only generates.** Don't make the model do what a
  tool checks reliably (Google's migration-factory lesson). The verify step is deterministic code, not
  an LLM opinion.
- **Verify = the lit state.** Not the artifact on the bench — the activated state.
- **Panels decorrelate by lens first, salt for extras.** N agents on the same prompt share every blind
  spot — the ensemble buys nothing (research-factory's verify panel learned this: 3 identical voters →
  diverse lenses). Give each voter a distinct lens/role; need more voters than lenses → salt the extras
  (shuffle evidence order, vary the stance), don't clone. Never jitter a seeded eval — regression gates
  need determinism.
- **Compound = memory.** Each run leaves the next ahead (a profile / library / ratchet). Verified on
  use: remembered claims are hints, re-checked before trusted, so they self-heal as the world changes.

**Code (ponytail — lazy means efficient, not careless)**
- Reuse before building: one tool that covers three jobs beats three (Lighthouse bundles axe-core).
  Stdlib / native / an installed CLI over custom code. One line before fifty.
- No speculative abstraction, no framework for one thing, fewest files.
- Non-trivial logic (a gate, a parser, a money/security path) leaves ONE runnable check — an
  assert-based `--selftest`, no test framework.
- Mark deliberate shortcuts with a `ponytail:` comment naming the ceiling + upgrade path.
- Bundled scripts (from Anthropic's skill-authoring docs): **solve, don't punt** — handle errors in
  the script, don't leave them for the model; **no voodoo constants** — justify every config value in a
  comment; **forward slashes** in all paths; MCP tools use fully-qualified `Server:tool` names.

**Deliver it as a skill/command — grounded in Anthropic's Agent-Skills docs**
(source: platform.claude.com/docs/…/agent-skills/best-practices — primary, this is the standard, not our habit)
- **The `description` is what makes it trigger** — the one load-bearing field. Write it **third person**
  ("Builds X…", never "I/you can…"), and include BOTH *what it does* AND *when to use it* (concrete
  triggers/phrases). Max 1024 chars. `name`: ≤64 chars, lowercase-hyphens only, no "claude"/"anthropic".
- **SKILL.md body under 500 lines.** Over that → split into referenced files loaded on demand.
- **Progressive disclosure, one level deep** — reference files link *directly* from SKILL.md, never
  nested (the model previews nested files with `head` and misses content). A reference file >100 lines
  gets a table of contents.
- **Match freedom to fragility** — high-freedom text steps for open tasks; low-freedom "run exactly
  this" for fragile/consistency-critical ones.
- **Eval-first** (maps to the factory's own verify thesis): before writing the skill, define 2-3
  concrete success scenarios + a baseline. Build the minimum that passes them — don't document imagined
  needs.
- **Scripts are executed, not loaded** — say "run `x.py`" (execute) vs "see `x.py`" (read as reference);
  execution costs only the output's tokens.

**Skill vs Command vs Subagent vs MCP** (the delivery choice)
- **Skill** — reusable knowledge/procedure the model auto-loads when a task matches (the default for a
  factory's conventions + process, e.g. this one).
- **Slash command** — a user-typed shortcut for a fixed action (e.g. `ship-<output>`).
- **Subagent** — an isolated-context delegated task (use for the parallel/fan-out work, like the
  reviewers or research fan-out).
- **MCP** — a connection to an *external* system/tool. Not for in-repo logic.

**Organization**
- Thin sibling repo, not a merged mono-framework (the family shares a *pattern*, not a runtime).
- `bin/` machinery · `reviewers/` markdown · `commands/` markdown · `README.md` with the two-register
  honesty and the ceiling.
- Register in `factories/README.md`.

**Honesty**
- State the ceiling: what the factory guarantees (the floor) vs what it can't certify.
- Don't fake a verify. If the lit state isn't checkable, say so — don't dress a checklist as proof.

**Don't**
- Don't build meta-levels for their own sake — leaf factories do the real work.
- Don't build the factory in the abstract if a real target exists — build against it.
- Don't extract shared *code* across factories until the pattern is proven at 3+ and the common core
  is obvious.
