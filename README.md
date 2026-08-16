# factories

The registry for a family of **factories** — repeatable pipelines that turn a fuzzy idea into a
finished, quality-assured artifact. *Arrive at an idea → say "ship it" → know it's in good hands.*

Three real legs exist, so the shared shape is now extractable (Rule of Three). The extraction is a
**named pattern + a method**, not a merged codebase — the factories differ too much in weight to force
into one core, and each already works standalone. Naming the pattern is the lever (it becomes
buildable-upon); merging the code would be premature abstraction.

## The shared pattern

Every factory is the same spine, keyed to its output:

```
FRAME → PRODUCE → VERIFY (the lit state) → COMPOUND (memory) → SHIP
```

Two invariants make it a factory and not a checklist:
- **Verify = the lit state.** The medium is light — code is inert until run, a page until a user flows
  through it. The verify step checks the *activated* state (runs / renders / loads / reconciles), and
  it only exists where that state is **machine-checkable**. No checkable lit-state → no factory (just a
  helper). Be honest about the ceiling: verify what you can, don't fake what you can't.
- **Compound = memory.** Each run leaves the next one ahead — a profile, a library, a ratchet.

## The registry

| Factory | Makes | Repo | Verify (its lit state) |
|---|---|---|---|
| **code** | code changes, shipped | [code-factory](https://github.com/dylanpulver/code-factory) | tests pass · fail-before/pass-after · differential |
| **research** | decision-ready sourced brief | [research-factory](https://github.com/dylanpulver/research-factory) | every claim → a real source · 3-vote adversarial |
| **landing-page** | a live landing page | `landing-page-factory` *(not yet public)* | Lighthouse CWV · WCAG-AA (axe) · SEO — honest that *conversion* isn't statically verifiable |

## The method: research generates the standards

Don't hand-author a factory's judgment layer — **generate it.** Run the `research` factory on
"best practices for <output>"; the brief becomes the new factory's reviewers + verify bars. The
research factory pays for itself twice: as a tool, and as the factory-builder.
(See [code-factory/docs/building-factories.md](https://github.com/dylanpulver/code-factory/blob/main/docs/building-factories.md).)

## What to build next

The candidate map — ~40 output types tiered by whether their lit-state is machine-checkable — lives in
[code-factory/docs/factory-catalog.md](https://github.com/dylanpulver/code-factory/blob/main/docs/factory-catalog.md). High-verifiability code-family + knowledge/data outputs are
where factories pay off; taste-dominant outputs (song/art) get only the mechanical spine.

## Discipline
- **Use before build** — a new factory earns its build from real friction, not imagination.
- **Verify the lit state or don't build** — no checkable output, no factory.
- **Name the pattern, don't merge the code** — the framework is this convention, not a shared runtime.
