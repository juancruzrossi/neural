# Visual plan — design language & method

`VISUAL.md` owns *what* goes on the page; this owns *how it looks*. **Design the page
for each feature — don't fill a fixed template.** The structure is fixed (so parity
holds and the page stays self-contained); the visual identity you create. The baseline
below is a floor to elevate, never the answer to ship unchanged.

Design like a studio that gives every client an identity no one else could be mistaken
for: a page that reads as templated has failed, even a tasteful one — make deliberate,
subject-specific choices and take one justified aesthetic risk.

## The brief

- **Subject.** An implementation plan a human reviews *before* an AI executes it.
  An engineering specification: sequential, dependency-ordered, evidence-driven.
  Its vernacular is code — file paths, test runners, red→green slices.
- **The page's one job.** Let a reviewer confirm, at a glance and in depth, that the
  plan is complete, correctly sequenced, and a faithful twin of the Markdown the
  machine will run.
- **Audience.** One engineer, cross-checking the machine. Not a marketing visitor.

## Design the page (every render)

Two passes. Do pass 1 in your head/notes before writing any HTML.

**Pass 1 — a compact token system grounded in *this* feature.**

- **Color (4–6 named hex).** A near-monochrome base — warm paper, graphite ink,
  hairline rules — is the standing frame. Choose **one accent derived from the
  feature's domain**, not the default SaaS indigo: read what the feature *is* and
  pick a hue with a reason (auth → vault steel; payments → ledger green; data/API →
  terminal teal; infra/build → blueprint blue; docs/content → ink). Keep the semantic
  trio (green/amber/red) for badges only. If a color is on the page, it means
  something.
- **Type (three roles).** A **display face** for the feature name / hero — a
  *system serif* stack (`Charter`, `"Iowan Old Style"`, `Palatino`, `Georgia`) or a
  confident heavy sans, but never the body font; it carries personality. A **body**
  sans. A **mono** utility for the eyebrow, section labels, task numbers, file paths,
  and badges — the engineering voice. Set a real scale with intentional weights and
  tight tracking on the hero.
- **Signature.** The **dependency spine** — numbered tasks threaded on a 1px rail,
  each node showing what it depends on — is the standing signature; the numbering is
  earned because the tasks are a real sequence. Keep it, and add **one** feature-
  specific touch that embodies this plan (how the accent threads the spine, a hero
  treatment of the thesis). Spend boldness in one place; keep everything else quiet.
- **Motion.** One orchestrated moment: a quiet page-load reveal of the sections and a
  hover lift on task nodes. Honor `prefers-reduced-motion`. Sparingly — scattered
  animation reads as AI-made.

**Pass 2 — critique, then build.**

- Would this page look identical for *any other* plan? If yes, the accent and type
  are defaults — revise until the choice is specific to this feature.
- Avoid the three AI-default looks: cream + serif + terracotta; near-black + acid
  accent; broadsheet hairline columns.
- **Mirror test before shipping** (remove one accessory): every color carries meaning,
  the accent earns its place (hero, links, active nav, spine) and appears nowhere else,
  and nothing reads as a generic SaaS dashboard.

## What stays fixed (the skeleton — never redesign)

- The sections, their `id`s, their order, and the **class names** `VISUAL.md` maps
  into. The design changes the *tokens*, not the structure.
- **Self-contained.** One `.html`; all CSS and the small theme/scroll script inline.
  **No external fonts, no CDN, no JS framework, no build** — system stacks only,
  nothing installs.
- The **dependency spine** for tasks; **semantic badges** as squared mono tags
  (color = meaning, never rounded marketing pills); **circle markers** on behaviors
  (they read as pending tests).
- **Quality floor:** responsive to mobile, visible keyboard focus, reduced motion
  respected, clean print (contents rail + toggle hidden).
- Escape `<`, `>`, `&` in user content.

## Banned AI tells

None may appear: **stat-counter chips** (`7 tasks · 31 behaviors · Generated <date>`)
— counts belong inline in a section heading, and never a generated date; **the
colored left-border active nav** (`.toc a.active { border-left-color }`) — active is
ink + weight + marker; gradients, hero art, logos, drop shadows, glassmorphism;
rounded "pill" badges; emoji; marketing voice; boilerplate footers ("Rendered from
PLAN.md… re-run to refresh").

## Baseline template

A working baseline that already satisfies the skeleton. **Keep the structure and
class names; restyle the token region — the `:root` palette, the font stacks, the
type scale, and the motion — per your pass-1 plan. Do not ship these defaults
unchanged.** Then replace every `{{PLACEHOLDER}}` and remove every `<!-- repeat -->`
marker. `VISUAL.md` maps PLAN.md content into the slots.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Plan · {{FEATURE_NAME}}</title>
<script>
  // Apply saved theme before paint to avoid a flash.
  (function () {
    try {
      var t = localStorage.getItem('neural-plan-theme');
      if (t) document.documentElement.setAttribute('data-theme', t);
    } catch (e) {}
  })();
</script>
<style>
  /* ── DESIGN TOKENS — author per feature (see "Design the page").
        The values below are ONE instance, not the answer. ───────────── */
  :root {
    --paper:#fcfcfa; --raise:#ffffff; --ink:#1a1a1c; --muted:#71717a;
    --faint:#9a9aa2; --line:#e7e7e1; --line-2:#d8d8d1;
    --accent:#2f5d86; --accent-soft:#eaf0f6;          /* ← derive from the feature */
    --ok:#3f7d52; --ok-soft:#eaf2ec;
    --warn:#956321; --warn-soft:#f5eede;
    --danger:#a23b32; --danger-soft:#f6e8e6;
    --display:"Charter","Bitstream Charter","Iowan Old Style","Palatino Linotype",Palatino,Georgia,serif;
    --sans:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
    --mono:ui-monospace,SFMono-Regular,"SF Mono",Menlo,Consolas,"Liberation Mono",monospace;
  }
  @media (prefers-color-scheme:dark) {
    :root:not([data-theme="light"]) {
      --paper:#0f0f11; --raise:#161619; --ink:#ececee; --muted:#9a9aa3;
      --faint:#6a6a72; --line:#262629; --line-2:#34343a;
      --accent:#7ea8d6; --accent-soft:#15212e;
      --ok:#7fb98f; --ok-soft:#13231a;
      --warn:#cda360; --warn-soft:#231c10;
      --danger:#d98a80; --danger-soft:#251513;
    }
  }
  [data-theme="dark"] {
    --paper:#0f0f11; --raise:#161619; --ink:#ececee; --muted:#9a9aa3;
    --faint:#6a6a72; --line:#262629; --line-2:#34343a;
    --accent:#7ea8d6; --accent-soft:#15212e;
    --ok:#7fb98f; --ok-soft:#13231a;
    --warn:#cda360; --warn-soft:#231c10;
    --danger:#d98a80; --danger-soft:#251513;
  }
  /* ── STRUCTURE — keep ─────────────────────────────────────────────── */
  * { box-sizing:border-box; }
  html { scroll-behavior:smooth; }
  body {
    margin:0; background:var(--paper); color:var(--ink);
    font-family:var(--sans); font-size:15.5px; line-height:1.62;
    -webkit-font-smoothing:antialiased; text-rendering:optimizeLegibility;
  }
  .shell {
    max-width:1060px; margin:0 auto; padding:0 32px;
    display:grid; grid-template-columns:196px minmax(0,1fr); gap:0 60px;
  }
  a { color:var(--accent); text-decoration-thickness:1px; text-underline-offset:2px; }
  a:focus-visible, .toggle:focus-visible, .toc a:focus-visible {
    outline:2px solid var(--accent); outline-offset:2px; border-radius:4px; }
  code {
    font-family:var(--mono); font-size:.86em; color:var(--ink);
    background:var(--accent-soft); padding:1px 5px; border-radius:5px;
  }
  pre { background:var(--raise); border:1px solid var(--line); border-radius:9px;
    padding:13px 15px; overflow-x:auto; }
  pre code { background:none; padding:0; font-size:13px; }

  .toc { position:sticky; top:0; align-self:start; height:100vh;
    padding:56px 0; overflow-y:auto; }
  .toc .lbl { font-family:var(--mono); font-size:10.5px; letter-spacing:.14em;
    text-transform:uppercase; color:var(--faint); margin:0 0 14px; }
  .toc a { display:flex; align-items:center; gap:9px; padding:5px 0;
    font-family:var(--mono); font-size:12.5px; color:var(--muted);
    text-decoration:none; transition:color .15s; }
  .toc a::before { content:""; width:5px; height:5px; border-radius:1px;
    background:var(--line-2); flex:none; transition:background .15s; }
  .toc a:hover { color:var(--ink); }
  .toc a.active { color:var(--ink); font-weight:600; }
  .toc a.active::before { background:var(--accent); }

  main { padding:56px 0 110px; min-width:0; max-width:768px; }
  .eyebrow { font-family:var(--mono); font-size:11.5px; letter-spacing:.16em;
    text-transform:uppercase; color:var(--accent); margin:0 0 16px; }
  h1 { font-family:var(--display); font-size:41px; line-height:1.08;
    letter-spacing:-.015em; font-weight:600; margin:0 0 16px; }
  .thesis { font-size:17.5px; line-height:1.55; color:var(--muted);
    max-width:64ch; margin:0 0 22px; }
  .dim { color:var(--muted); }

  .sec { margin-top:48px; scroll-margin-top:24px; }
  .sec > h2 { font-family:var(--mono); font-size:12px; font-weight:600;
    letter-spacing:.1em; text-transform:uppercase; color:var(--muted);
    margin:0 0 18px; padding-bottom:10px; border-bottom:1px solid var(--line); }
  .sec > h2 .count { color:var(--faint); font-weight:400; letter-spacing:.04em; }
  p { margin:0 0 13px; }

  table { width:100%; border-collapse:collapse; font-size:14px; }
  thead th { font-family:var(--mono); font-size:10.5px; font-weight:600;
    letter-spacing:.07em; text-transform:uppercase; color:var(--muted);
    text-align:left; padding:0 14px 10px; border-bottom:1px solid var(--line-2); }
  tbody td { padding:11px 14px; border-bottom:1px solid var(--line);
    vertical-align:top; }
  tbody tr:last-child td { border-bottom:none; }
  td code { background:none; padding:0; }

  .tag { display:inline-block; font-family:var(--mono); font-size:10.5px;
    font-weight:600; letter-spacing:.05em; text-transform:uppercase;
    padding:2px 7px; border-radius:5px; white-space:nowrap; }
  .t-ok { background:var(--ok-soft); color:var(--ok); }
  .t-warn { background:var(--warn-soft); color:var(--warn); }
  .t-danger { background:var(--danger-soft); color:var(--danger); }
  .t-accent { background:var(--accent-soft); color:var(--accent); }
  .t-mute { background:var(--accent-soft); color:var(--muted); }

  .tasks { margin-top:4px; }
  .task { position:relative; display:grid; grid-template-columns:34px minmax(0,1fr);
    column-gap:22px; padding-bottom:32px; }
  .task:last-child { padding-bottom:0; }
  .task::before { content:""; position:absolute; left:16px; top:36px; bottom:0;
    width:1px; background:var(--line); }
  .task:last-child::before { display:none; }
  .tnode { position:relative; z-index:1; width:34px; height:34px; border-radius:9px;
    border:1px solid var(--line-2); background:var(--paper); color:var(--accent);
    font-family:var(--mono); font-weight:600; font-size:14px;
    display:flex; align-items:center; justify-content:center;
    transition:border-color .16s, color .16s, transform .16s; }
  .task:hover .tnode { border-color:var(--accent); transform:translateY(-1px); }
  .thead { display:flex; align-items:baseline; gap:11px; flex-wrap:wrap;
    padding-top:5px; margin-bottom:4px; }
  .ttitle { font-size:18px; font-weight:600; letter-spacing:-.012em; margin:0;
    flex:1; min-width:220px; }
  .deps { font-family:var(--mono); font-size:11.5px; color:var(--muted); }
  .deps b { color:var(--ink); font-weight:600; }
  .task dl { margin:13px 0 0; display:grid;
    grid-template-columns:max-content minmax(0,1fr); gap:9px 18px; }
  .task dt { font-family:var(--mono); font-size:10.5px; letter-spacing:.07em;
    text-transform:uppercase; color:var(--muted); padding-top:3px; }
  .task dd { margin:0; }
  .files { display:flex; flex-wrap:wrap; gap:6px; }
  .files code { font-size:12.5px; }

  ul.checks, ul.boxes { list-style:none; margin:3px 0 0; padding:0; }
  ul.checks li, ul.boxes li { position:relative; padding-left:25px; margin:7px 0; }
  ul.checks li::before { content:""; position:absolute; left:1px; top:.42em;
    width:11px; height:11px; border-radius:50%; border:1.5px solid var(--accent); }
  ul.boxes li::before { content:""; position:absolute; left:1px; top:.42em;
    width:12px; height:12px; border-radius:3px; border:1.5px solid var(--muted); }

  ul.bullets { margin:4px 0 0; padding-left:19px; }
  ul.bullets li { margin:5px 0; }
  .trail { margin-top:24px; padding-top:18px; border-top:1px solid var(--line); }
  .trail .lbl { font-family:var(--mono); font-size:10.5px; letter-spacing:.1em;
    text-transform:uppercase; color:var(--faint); }
  ul.tree { list-style:none; margin:12px 0 0; padding:0; }
  ul.tree li { padding:6px 0; font-size:13.5px; border-top:1px solid var(--line); }
  ul.tree li:first-child { border-top:none; }
  ul.tree .what { color:var(--muted); }

  .note { border:1px solid var(--line); border-radius:10px; padding:18px 20px;
    background:var(--raise); }
  .note h3 { font-family:var(--mono); font-size:11px; font-weight:600;
    letter-spacing:.07em; text-transform:uppercase; color:var(--muted);
    margin:0 0 9px; }
  .note h3:not(:first-child) { margin-top:16px; }
  .note ul { margin:0; padding-left:19px; }
  .note li { margin:5px 0; }

  .toggle { position:fixed; top:18px; right:18px; z-index:20; width:34px; height:34px;
    border-radius:9px; border:1px solid var(--line-2); background:var(--raise);
    color:var(--muted); cursor:pointer; font-size:15px; line-height:1;
    display:flex; align-items:center; justify-content:center; }
  .toggle:hover { color:var(--accent); border-color:var(--accent); }

  /* Motion — one quiet page-load reveal; disabled under reduced-motion. */
  @media (prefers-reduced-motion:no-preference) {
    @keyframes riseIn { from { opacity:0; transform:translateY(10px); }
      to { opacity:1; transform:none; } }
    main > header, main > .sec {
      animation:riseIn .55s cubic-bezier(.22,.61,.36,1) both; }
    main > *:nth-child(2) { animation-delay:.05s; }
    main > *:nth-child(3) { animation-delay:.10s; }
    main > *:nth-child(4) { animation-delay:.14s; }
    main > *:nth-child(5) { animation-delay:.18s; }
    main > *:nth-child(6) { animation-delay:.22s; }
    main > *:nth-child(n+7) { animation-delay:.26s; }
  }

  @media (max-width:880px) {
    .shell { grid-template-columns:1fr; padding:0 22px; }
    .toc { position:static; height:auto; padding:26px 0 0; }
    .toc nav { display:flex; flex-wrap:wrap; gap:4px 16px; }
    .toc a::before { display:none; }
    main { padding-top:26px; }
    h1 { font-size:34px; }
  }
  @media print {
    .toc, .toggle { display:none; }
    .shell { grid-template-columns:1fr; max-width:none; }
    body { background:#fff; color:#000; font-size:11pt; }
    main > header, main > .sec { animation:none; }
    .task, table, pre, .note { break-inside:avoid; }
  }
</style>
</head>
<body>
<button class="toggle" onclick="neuralToggleTheme()" aria-label="Toggle light/dark theme" title="Toggle light/dark">◐</button>
<div class="shell">
  <aside class="toc">
    <p class="lbl">Contents</p>
    <nav>
      <!-- One link per rendered section. Drop links for sections the plan omits. -->
      <a href="#overview">Overview</a>
      <a href="#test-runner">Test runner</a>
      <!-- Skills: keep only if the plan has a Model Invocable Skills section -->
      <a href="#skills">Skills</a>
      <a href="#context">Context</a>
      <a href="#files">File map</a>
      <a href="#tasks">Tasks</a>
      <a href="#risks">Risks</a>
      <a href="#acceptance">Acceptance</a>
    </nav>
  </aside>

  <main>
    <header>
      <p class="eyebrow">Implementation Plan</p>
      <h1>{{FEATURE_NAME}}</h1>
      <p class="thesis">{{OVERVIEW_LEAD}}</p>
    </header>

    <section class="sec" id="overview">
      <h2>Overview</h2>
      <p>{{OVERVIEW_FULL}}</p>
    </section>

    <section class="sec" id="test-runner">
      <h2>Test runner</h2>
      <p><code>{{TEST_RUNNER}}</code> <span class="dim">{{TEST_RUNNER_NOTE}}</span></p>
    </section>

    <!-- Skills: include only if PLAN.md has a Model Invocable Skills section. -->
    <section class="sec" id="skills">
      <h2>Model Invocable Skills</h2>
      <ul class="bullets">
        <!-- repeat per skill --><li><code>{{SKILL}}</code> — {{SKILL_WHY}}</li><!-- /repeat -->
      </ul>
    </section>

    <section class="sec" id="context">
      <h2>Context</h2>
      <p>{{CONTEXT_PROBLEM}}</p>
      <h3 class="eyebrow" style="margin:22px 0 10px">Binding decisions</h3>
      <ul class="bullets">
        <!-- repeat per binding decision (verbatim from CONTEXT.md Decisions / ADRs); drop heading + list if none -->
        <li>{{DECISION}}</li>
        <!-- /repeat -->
      </ul>
      <div class="trail">
        <span class="lbl">Spec workspace · <code>.neural/wip/{{FEATURE_SLUG}}/</code></span>
        <ul class="tree">
          <li><code>CONTEXT.md</code> <span class="what">— feature context from <code>/neural:neural-interview</code></span></li>
          <!-- repeat per ADR actually present under docs/adr/ -->
          <li><code>docs/adr/{{ADR_FILE}}</code> <span class="what">— {{ADR_TITLE}} (binding)</span></li>
          <!-- /repeat -->
          <li><code>PLAN.md</code> <span class="what">— this plan, source of truth for execute</span></li>
          <li><code>PLAN.html</code> <span class="what">— this page</span></li>
        </ul>
      </div>
    </section>

    <section class="sec" id="files">
      <h2>File map</h2>
      <table>
        <thead><tr><th>File</th><th>Action</th><th>Responsibility</th></tr></thead>
        <tbody>
          <!-- repeat per file -->
          <tr>
            <td><code>{{FILE_PATH}}</code></td>
            <td><span class="tag {{ACTION_TAG}}">{{ACTION}}</span></td>
            <td>{{RESPONSIBILITY}}</td>
          </tr>
          <!-- /repeat -->
        </tbody>
      </table>
    </section>

    <section class="sec" id="tasks">
      <h2>Tasks <span class="count">· {{TASK_COUNT}} · dependency order</span></h2>
      <div class="tasks">
        <!-- repeat per task -->
        <article class="task">
          <div class="tnode">{{N}}</div>
          <div>
            <div class="thead">
              <h3 class="ttitle">{{TASK_TITLE}}</h3>
              <span class="tag {{EST_TAG}}">{{ESTIMATE}}</span>
              <span class="deps">{{DEPENDS}}</span>
            </div>
            <dl>
              <dt>What</dt><dd>{{WHAT}}</dd>
              <dt>Why</dt><dd>{{WHY}}</dd>
              <dt>Files</dt><dd class="files"><code>{{FILE_A}}</code> <code>{{FILE_B}}</code></dd>
              <dt>Behaviors</dt>
              <dd>
                <ul class="checks">
                  <!-- repeat per behavior (verbatim) -->
                  <li>{{BEHAVIOR}}</li>
                  <!-- /repeat -->
                </ul>
              </dd>
              <dt>Accept</dt><dd>{{ACCEPTANCE}}</dd>
            </dl>
          </div>
        </article>
        <!-- /repeat -->
      </div>
    </section>

    <section class="sec" id="risks">
      <h2>Risk assessment</h2>
      <table>
        <thead><tr><th>Risk</th><th>Impact</th><th>Likelihood</th><th>Mitigation</th></tr></thead>
        <tbody>
          <!-- repeat per risk -->
          <tr>
            <td>{{RISK}}</td>
            <td><span class="tag {{IMPACT_TAG}}">{{IMPACT}}</span></td>
            <td><span class="tag {{LIKELIHOOD_TAG}}">{{LIKELIHOOD}}</span></td>
            <td>{{MITIGATION}}</td>
          </tr>
          <!-- /repeat -->
        </tbody>
      </table>
    </section>

    <section class="sec" id="acceptance">
      <h2>Acceptance criteria</h2>
      <ul class="boxes">
        <!-- repeat per criterion (verbatim) -->
        <li>{{CRITERION}}</li>
        <!-- /repeat -->
      </ul>
    </section>

    <!-- Optional: include only if PLAN.md has these sections.
    <section class="sec" id="cross-review">
      <h2>Cross-review</h2>
      <div class="note">{{CROSS_REVIEW}}</div>
    </section>
    -->
  </main>
</div>

<script>
  function neuralToggleTheme() {
    var root = document.documentElement;
    var cur = root.getAttribute('data-theme');
    var dark = cur === 'dark' ||
      (cur !== 'light' && matchMedia('(prefers-color-scheme:dark)').matches);
    root.setAttribute('data-theme', dark ? 'light' : 'dark');
    try { localStorage.setItem('neural-plan-theme', dark ? 'light' : 'dark'); } catch (e) {}
  }
  // Active contents link tracks scroll — color + weight + marker, never a border bar.
  (function () {
    var links = [].slice.call(document.querySelectorAll('.toc a'));
    var map = {};
    links.forEach(function (a) { map[a.getAttribute('href').slice(1)] = a; });
    var obs = new IntersectionObserver(function (entries) {
      entries.forEach(function (en) {
        if (en.isIntersecting && map[en.target.id]) {
          links.forEach(function (l) { l.classList.remove('active'); });
          map[en.target.id].classList.add('active');
        }
      });
    }, { rootMargin: '-18% 0px -72% 0px' });
    document.querySelectorAll('.sec').forEach(function (s) { obs.observe(s); });
  })();
</script>
</body>
</html>
```

## Tag mapping

| PLAN.md value | class |
|---|---|
| Action `Create` / estimate `S` / risk `L` | `t-ok` |
| Action `Modify` / estimate `M` / risk `M` | `t-warn` |
| Action `Delete` / estimate `L` / risk `H` | `t-danger` |
| `Create/Modify` (both) | `t-accent` |
