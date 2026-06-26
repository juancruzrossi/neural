# Visual plan rendering — fidelity & content mapping

How the `--visual` flag of `/neural:neural-plan` turns the finalized PLAN.md into
`PLAN.html`. This file owns *what* goes on the page and the fidelity rules. The look
lives in `STYLES.md`, which has you **design the page per feature** on a fixed
structure — follow its method and keep that structure (sections, ids, class names);
this file fills it with verbatim plan content.

## Contract

- **PLAN.md is the source of truth.** Execute, review, and sync read the Markdown,
  never the HTML. Always write PLAN.md first, then render PLAN.html from it.
- **The two files must say literally the same thing.** The machine reads PLAN.md;
  the human reads PLAN.html. They must not diverge. The HTML is a *verbatim*
  rendering of PLAN.md — same overview, same tasks, same behaviors, same risks,
  same acceptance criteria, in the same order, word for word. Markup is the only
  thing added; never reword, summarize, reorder, add, or drop plan content. If the
  plan changes later, re-render.
- **One self-contained file.** Write `.neural/wip/<feature>/PLAN.html` as a single
  `.html`. No CDN, no external fonts, no JS framework, no build step — nothing for
  the user to install. All CSS and the small theme/scroll script are inline in the
  `STYLES.md` template.
- **Stand on its own.** A reviewer who never saw the chat must understand the page.
  The only content that comes from outside PLAN.md is a small upstream **Context**
  block sourced *verbatim* from `CONTEXT.md` (so it cannot diverge either) plus a
  map of the spec workspace.

## Fidelity is the first rule

Everything on the page is verbatim from one of two files, so nothing can drift:

- **From PLAN.md (verbatim):** Overview, File Map, every Task (title, What, Why,
  Files, every Behavior, Acceptance), Risk Assessment, Acceptance Criteria,
  and Cross-Review if present.
- **From CONTEXT.md (verbatim):** the Problem statement and the binding Decisions
  for the Context block. Pull the text as written; do not paraphrase. Skip the
  CONTEXT sections that would duplicate the plan (Acceptance Criteria, Scenarios,
  Non-goals) — show Problem + Decisions only.
- **Authored fresh:** nothing. You are rendering, not writing.

Escape user content as HTML — never leave raw `<`, `>`, or `&` from code snippets
unescaped inside text.

## Section mapping

| Source (PLAN.md unless noted)        | HTML block                                                                 |
|--------------------------------------|----------------------------------------------------------------------------|
| Title `# Plan: <name>`               | `<h1>` feature name + eyebrow                                               |
| Overview (first sentence)            | `.thesis` lead under the title (verbatim first sentence)                    |
| Overview (full)                      | `#overview` paragraph (verbatim, complete)                                  |
| Test Runner                          | `#test-runner` section — command in `<code>`, the rest as a `.dim` note     |
| Model Invocable Skills (if present)  | `#skills` section — one bullet per skill (`/name` + reason); drop if absent  |
| `CONTEXT.md` Problem (verbatim)      | `#context` lead paragraph                                                   |
| `CONTEXT.md` Decisions / ADRs (verbatim) | `#context` "Binding decisions" bullets + spec-workspace tree           |
| File Map table                       | `#files` table, one action tag per row                                     |
| Tasks table + details                | `#tasks` spine: one `.task` per task — node number, title, estimate tag, depends, What/Why/Files, Behaviors checklist, Acceptance |
| Risk Assessment                      | `#risks` table with impact + likelihood tags                               |
| Acceptance Criteria                  | `#acceptance` square checklist (verbatim)                                   |
| Cross-Review (only if present)       | `#cross-review` note — include only if PLAN.md has it                       |

Behaviors render as a circle checklist because each becomes a red→green test slice
in execute — keep the circle marker so they read as pending tests.

## Population rules

- Fill the contents `<nav>` with one link per section you actually render; drop
  links for sections the plan omits (e.g. no Cross-Review).
- Tag classes by value — see the **Tag mapping** table in `STYLES.md`. Map every
  Action, Estimate, Impact, and Likelihood to its class.
- `Depends on` of `—` → `<span class="tag t-mute">no deps</span>`; otherwise
  `needs <b>3, 4</b>` with the real task numbers.
- File paths and inline code use `<code>`; multi-line snippets use `<pre><code>`.
- `{{TASK_COUNT}}` is the real number of tasks, shown in the Tasks heading.
- **Test runner** maps the whole `## Test Runner` line: the command goes in
  `{{TEST_RUNNER}}` (inside `<code>`); any trailing note/parenthetical goes in
  `{{TEST_RUNNER_NOTE}}` with a leading `— ` separator. Leave the note empty if the
  plan states only a command. Do not invent a note.
- **Model Invocable Skills** renders the `#skills` section and its TOC link only if
  PLAN.md has that section — one bullet per skill (`{{SKILL}}` `/name` + `{{SKILL_WHY}}` reason).
- **Context block** uses `CONTEXT.md` + ADRs already read in step 1. Put the
  Problem statement verbatim in `{{CONTEXT_PROBLEM}}`. List each binding Decision
  (and each ADR) verbatim as a `bullets` item; if there are none, drop the "Binding
  decisions" heading and its list. Do not restate the plan's Acceptance Criteria.
- **Spec workspace** lists the real files under `.neural/wip/<feature>/`: always
  `CONTEXT.md`, `PLAN.md`, `PLAN.html`, plus one row per ADR that actually exists
  under `docs/adr/` (drop the ADR rows when there are none). Use the real slug for
  `{{FEATURE_SLUG}}`.
- Resolve **every** `{{PLACEHOLDER}}` and remove every `<!-- repeat -->` marker.
  No template syntax may survive in the output.

## Parity check (run before handoff)

Re-read PLAN.md and confirm the HTML is its literal twin:

1. **Tasks** — every task title, in order, appears once; the task count matches.
2. **Behaviors** — every behavior of every task is present, verbatim; the per-task
   counts match. None summarized, merged, or invented.
3. **File map** — every row present, same file / action / responsibility.
4. **Risks** — every row present, same severities.
5. **Acceptance criteria** — every item present, verbatim, same order.
6. **No additions** — the page contains nothing that is not in PLAN.md, except the
   Context block (verbatim from CONTEXT.md) and the spec-workspace tree.

If any item fails, fix the HTML — never the plan — and re-check.

## Final visual check

1. No `{{...}}` and no `<!-- repeat -->` markers survive in the output.
2. Toggle the theme button — both light and dark are readable; no hard-coded color
   leaks through.
3. Contents links jump to their sections; the active link tracks scroll position.
4. Print preview / "Save as PDF" is clean — contents rail and toggle hidden, no
   clipped cards.
5. Run the `STYLES.md` self-critique (the mirror test).
