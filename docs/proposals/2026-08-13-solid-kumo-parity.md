# Proposal: Solid Kumo Package and Parity Documentation

**Status:** Independent prototype in progress; Cloudflare integration proposed
**Audience:** Kumo maintainers and Cloudflare technical leadership
**Pilot:** `Switch`
**Prototype location:** `../ui` (outside this repository)

## Executive decision brief

### Recommendation

Review a bounded, one-component proof of concept being developed independently
in the Solidaria repository. The pilot answers one question:

> Can a reusable SolidJS package preserve Kumo's public `Switch` contract,
> visual system, and user outcomes while using Solidaria internally?

The external prototype can continue while this proposal is reviewed. It does
not change Kumo's production package or documentation, claim Cloudflare
endorsement, or require Cloudflare to own the experiment. It returns evidence
for an explicit accept, revise, or stop decision before any Cloudflare
integration begins.

### What review covers

The prototype team will:

1. use the pinned published `@cloudflare/kumo@2.10.0` package as the React and
   visual reference in the existing `../ui` comparison app;
2. create an unpublished, clearly experimental, reusable Solid workspace
   package in `../ui` with public exports but no public npm identity;
3. implement the Kumo-compatible `Switch` family through
   `solidaria-components`, `solidaria`, and `solid-stately`;
4. test both packages under the same API, interaction, accessibility,
   rendering, and visual scenarios; and
5. return the evidence and maintenance findings to Kumo maintainers for review.

Cloudflare review can improve the evaluation contract and identify a Kumo
contact for API questions. It is not a prerequisite for continued private work
in `../ui`. Approval is required only before changes are made to Kumo, the
prototype is presented as official Kumo work, or decisions are made about
Cloudflare ownership, naming, publication, documentation, or support.

### Parallel work model

```text
Independent prototype lane                 Cloudflare review lane

Pin released Kumo baseline                 Review this proposal
          |                                          |
Build reusable Solid Switch                Clarify contract questions
          |                                          |
Collect matched evidence  <--------------------------+
          |
          +----> Joint decision gate
                    |        |        |
                   stop    revise   consider official integration
```

The lanes exchange contract feedback, but neither repository depends on the
other by filesystem path, unpublished artifact, or unreviewed source change.

### Why this is low risk

- The pilot covers one component and has an explicit stop point.
- All implementation, dependency, lockfile, and evidence changes remain in
  `../ui` during the proof of concept.
- This Kumo checkout contains only this proposal; it receives no Solid runtime
  dependency, generated output, or production code.
- Both sides are consumed through package entry points, so the harness cannot
  hide fixes that belong in a library.
- The published Kumo version and Solidaria revision are pinned before evidence
  is collected.
- Kumo documentation integration requires a separate decision after the pilot
  evidence is reviewed; external prototype progress does not bypass that gate.

### Evidence returned

The review receives:

- a versioned React-versus-Solid `Switch` scenario matrix;
- interaction, accessibility, SSR, hydration, and visual results;
- every difference classified as a match, intentional difference, React gap,
  Solid gap, or not tested;
- the package and framework versions needed to reproduce the result; and
- an assessment of package boundaries and likely ongoing maintenance cost.

The pilot does not produce a framework winner score or performance claim.

### Feedback requested now

1. Is the fair-comparison contract in this proposal credible?
2. Would the maintainers consider the resulting evidence when evaluating a
   later official integration?
3. If the team is interested, which Kumo maintainer can clarify public-contract
   ambiguities and review gap classifications while the prototype develops?

### Explicitly deferred

Review does not select a permanent repository, owner, public package name, npm
scope, publication plan, support policy, or broader component roadmap. It also
does not approve adding Solid to the Kumo documentation. Those decisions
require the pilot evidence and a separate Cloudflare decision.

## Context and opportunity

Kumo is currently a React component library. Its interactive components use a
mix of Base UI primitives and native React implementations. A separate project
is developing SolidJS accessibility and component primitives in these layers:

```text
solid-stately -> solidaria -> solidaria-components -> styled component
```

A Solid Kumo package could reuse that behavior stack while preserving Kumo's
consumer-facing API and visual system. The Kumo documentation is a useful place
to review the result because Astro can render React and Solid components as
independent islands on the same page.

The existing comparison app in the Solidaria repository is the development and
evidence harness. It should consume a fresh published Kumo package rather than
source files from this checkout. This keeps the reference realistic, gives the
comparison a reproducible package boundary, and avoids making unapproved code
changes in the Cloudflare repository.

The documentation is a verification surface. It must not contain behavior or
styling fixes that belong in either component package.

## Local preparation status

As of 2026-08-13, the existing `../ui` comparison app pins
`@cloudflare/kumo@2.10.0` and its required
`@phosphor-icons/react@2.1.10` peer as exact dependencies. The workspace lockfile
records the resolved dependency graph, and the unchanged comparison app builds
successfully with those dependencies installed.

This establishes the released-package baseline only. Subsequent prototype
status is tracked in `../ui`, where work may continue in parallel with this
review. No external prototype work implies Cloudflare approval.

## Goals

- Provide a reusable SolidJS package with Kumo-compatible component APIs.
- Preserve Kumo's visual language, semantic tokens, and documented states.
- Compare equivalent React and Solid components under matched conditions.
- Detect gaps in either implementation instead of treating React as correct by
  definition.
- Produce evidence that maintainers can inspect and reproduce.
- Keep the first review and implementation narrow enough to revert cleanly.
- Prove the package boundary in the Solidaria repository before adding a second
  framework to Cloudflare's documentation toolchain.

## Non-goals

- Port all Kumo components before the pilot is evaluated.
- Replace React Kumo or change its support policy.
- Compare React and Solid as general-purpose frameworks.
- Require identical internal state models or DOM trees.
- Publish performance claims from a page that loads both frameworks.
- Commit imports, links, or workspace configuration that depend on `../ui`.
- Choose a public package name or npm scope in this proposal.

## Proposed architecture

### Solid package

The Solid implementation is a reusable package, not a documentation fixture.
Its public surface follows Kumo unless a framework difference makes an exact
match impossible.

```text
Kumo-compatible Solid component
  -> solidaria-components
  -> solidaria
  -> solid-stately
```

For the `Switch` pilot, the intended public surface includes:

- `Switch`
- `Switch.Group`
- `Switch.Item`
- `Switch.Legend`
- Kumo prop names such as `checked`, `onCheckedChange`, `label`, `size`,
  `variant`, `disabled`, `required`, and `controlFirst`
- Kumo semantic tokens, visual states, and applicable `data-kumo-*` markers

The adapter may translate Kumo props to Solidaria concepts such as
`isSelected` and `onChange`. Solid-native JSX and ref types are expected where
React types do not apply. These differences must be documented, not hidden.

During the prototype, the package may use locked Solidaria workspace
dependencies in `../ui`; the evidence must identify the exact repository
commit. Any later published or Cloudflare-integrated package must depend on a
reproducible published version, commit, or approved source location rather than
an assumed sibling checkout.

### Released Kumo reference in the Solidaria repository

The `../ui` comparison app should add `@cloudflare/kumo` from npm. At the start
of the pilot, resolve the latest stable release and commit the exact resolved
version and lockfile. Do not use a floating `latest` range during evidence
collection.

The comparison manifest must record:

- the `@cloudflare/kumo` package version;
- the Solid Kumo package version or commit;
- the relevant Solidaria package versions or commit;
- the browser and viewport used for captured evidence; and
- the date the upstream Kumo version was checked.

An explicit update command or freshness report may say when a newer Kumo
release exists, but it must not silently change the comparison baseline. A new
baseline requires regenerated evidence and review.

The React fixture must import `@cloudflare/kumo` and
`@cloudflare/kumo/styles` through their public package entry points. It must not
import source from this repository or reproduce Kumo styles inside the harness.

### Solidaria comparison app

Before Cloudflare docs integration, the existing Astro comparison app in
`../ui` hosts the pilot:

```text
Solidaria comparison page
  -> framework-neutral comparison shell
     -> released @cloudflare/kumo React island
     -> workspace Solid Kumo island
```

This is the primary development loop and the first place where the full pilot
matrix is collected. It proves that both sides work from real package entry
points while keeping all experimental implementation code out of Kumo.

### Astro documentation

After the pilot is accepted, the existing Kumo docs remain React-first and add
Solid support only for isolated comparison fixtures:

```text
component page
  -> framework-neutral Astro comparison shell
     -> React Kumo island
     -> Solid Kumo island
```

React and Solid JSX transforms must use non-overlapping include paths. Solid
fixtures must not live under the existing React demo directory because that
directory feeds Kumo's component registry code generation.

One framework-neutral scenario model drives both islands. Controls update the
URL and dispatch a DOM event with serializable props. Each island adapts those
props locally; neither framework owns the other island's state.

## Fair comparison contract

### References

Each result is evaluated against the reference appropriate to that dimension:

| Dimension | Reference |
| --- | --- |
| Public API | Kumo's documented component contract |
| Visual design | Pinned released `@cloudflare/kumo` npm artifact |
| Accessibility | Applicable web platform and WAI-ARIA requirements |
| Interaction | Documented Kumo behavior plus platform conventions |
| Solid internals | The Solidaria layer ownership rules |

React Kumo is the visual reference, not the universal correctness oracle. If a
test identifies a React accessibility or behavior gap, the comparison reports
that gap instead of requiring the Solid component to reproduce it.

### Matched conditions

Both implementations receive the same:

- scenario content and initial state;
- rendered area, viewport, browser, zoom, and device scale factor;
- Kumo theme, mode, semantic tokens, and font assets;
- reduced-motion setting and animation-settle interval;
- interaction sequence and state reset; and
- test assertions at their public package boundary.

The evidence header displays package and environment provenance. A screenshot
or report without that provenance is incomplete.

Tests run against independently mounted components. A failure in one side must
not prevent the other side from completing.

### Equivalent outcomes, not identical markup

Framework and primitive choices may produce different valid DOM. For example,
a switch may use a button with `role="switch"` or a checkbox input exposed as a
switch. Literal DOM equality is therefore not a parity requirement.

Parity is based on observable outcomes:

- accessible role, name, description, and state;
- pointer, keyboard, focus, and disabled behavior;
- controlled and uncontrolled state, when included in Kumo's contract;
- form submission and reset behavior, when included in Kumo's contract;
- public events and callback values;
- geometry, typography, color, focus indication, and motion; and
- server-rendered and hydrated behavior.

### Result vocabulary

Every check reports one of these states:

- **Match:** Both implementations meet the agreed contract.
- **Intentional difference:** The difference is necessary, documented, and has
  no incompatible user outcome.
- **React gap:** React Kumo does not meet the agreed contract.
- **Solid gap:** Solid Kumo does not meet the agreed contract.
- **Not tested:** No evidence has been collected yet.

The docs must not show an aggregate winner, parity percentage, or framework
score. Those summaries conceal the importance and cause of individual gaps.

## Comparison presentation

The comparison should read as a compact component laboratory within the
existing Kumo visual language.

1. **Matched preview:** equally sized React and Solid frames with persistent,
   explicit implementation labels.
2. **Scenario controls:** one control set for state, size, variant, disabled,
   required, mode, and other component-specific inputs.
3. **Consumer source:** comparable snippets using each public package, not raw
   primitives or docs-only adapters.
4. **Evidence:** API, accessibility, keyboard, forms, hydration, and visual
   checks with their result state and test source.
5. **Known differences:** short explanations for deliberate framework or DOM
   differences.

Side-by-side preview is the desktop default. On narrow screens, the interface
may switch between implementations, but it must preserve the same scenario and
make the active implementation unambiguous.

## `Switch` pilot matrix

The pilot is complete only when it covers:

| Area | Required scenarios |
| --- | --- |
| API | Controlled off/on, callback value, forwarded supported props |
| Pointer | Control activation and visible-label activation |
| Keyboard | Tab order, Space activation, optional Enter behavior recorded |
| Accessibility | Role, accessible name, checked state, description/error links |
| Focus | Visible keyboard focus and focus retention after activation |
| State | Off, on, disabled, required, and transitioning |
| Visual | All Kumo sizes and variants in light and dark modes |
| Motion | Normal and reduced motion |
| Composition | Group, item, legend, description, and error presentation |
| Rendering | SSR output and hydration without state drift or console errors |
| Forms | Submission and reset behavior, if retained in the agreed Kumo API |

The pilot must record unsupported or undefined Kumo behaviors instead of
silently expanding the API.

## Verification strategy

- Unit tests cover the Solid adapter's prop and callback translations.
- Shared browser scenarios run the same user actions and assertions against
  each labeled island.
- Accessibility assertions inspect roles, names, states, relationships, and
  focus outcomes.
- Visual screenshots use isolated frames and a stable environment. Pixel diffs
  are evidence for review, not proof of accessibility or behavior.
- Hydration tests fail on console errors, state drift, or duplicate identifiers.
- Source examples import only public package entry points.

Performance and bundle measurements are deferred. If added later, they require
separate React-only and Solid-only routes, equivalent production builds, a
pinned environment, repeated samples, and published methodology. The combined
comparison page is not a fair performance benchmark.

## Delivery stages

### Stage 0: Maintainer review, parallel with the prototype

- Review this proposal in a GitHub discussion or feature request, as requested
  by Kumo's contribution guide for non-trivial work.
- Provide feedback on the bounded `Switch` scope and fair-comparison contract
  while Stages 1 through 3 proceed independently in `../ui`.
- If the team is interested, identify the Kumo maintainer who can answer
  contract questions and later review gap classifications.
- Do not interpret prototype development as a request for Cloudflare ownership,
  endorsement, naming, publication, or support.
- Pin the Solidaria source and version used by the pilot.

### Stage 1: Fresh Kumo reference in `../ui`

- Use the exact `@cloudflare/kumo@2.10.0` dependency and locked graph already
  added to the comparison app.
- Add a minimal React fixture that imports only Kumo's public package and style
  entry points.
- Record reference provenance in the comparison manifest and rendered page.
- Verify that the unmodified package builds and hydrates in the dual-framework
  Astro app before writing the Solid implementation.

### Stage 2: Reusable `Switch`

- Scaffold a private, reusable workspace package in `../ui` under an
  experimental working identity.
- Implement the Kumo-compatible `Switch` family through the Solidaria stack.
- Add unit and package-level browser tests.
- Keep its exports and dependency boundaries movable to a future approved
  repository and package identity.
- Do not modify Kumo docs yet.

### Stage 3: Comparison proof of concept in `../ui`

- Add the shared Kumo `Switch` scenario model to the existing comparison app.
- Mount the released React and workspace Solid packages independently.
- Run the full pilot matrix and publish evidence with version provenance.
- Correct defects in their owning packages, never in comparison fixtures.

### Stage 4: Evidence review and acceptance decision

- Publish gaps without changing their classification to improve presentation.
- Have Kumo and Solidaria maintainers review the evidence and maintenance cost.
- Choose one outcome: accept the pilot, request a bounded revision, or stop.
- If accepted, decide ownership, permanent package location and identity,
  style versioning, publication scope, and support expectations before Kumo
  integration begins.

### Stage 5: Kumo documentation integration

Only after Stage 4 acceptance:

- add the official Astro Solid integration with scoped renderer paths;
- add the framework-neutral comparison shell and scenario protocol;
- add React and Solid `Switch` fixtures from public package entry points; and
- add the evidence display to the existing Switch documentation.

### Stage 6: Expansion decision

Only after the pilot is accepted, decide whether to continue. Suggested next
components are Tabs, Dialog, and Select because they progressively exercise
focus management, overlays, collections, and form behavior.

## Keeping the Kumo repository clean

Throughout the parallel prototype and review:

- keep this proposal as the only Kumo repository change;
- do not add dependencies, lockfile changes, generated files, or a changeset;
- do not scaffold a package or reserve a public name;
- do not commit symlinks, file dependencies, or imports that reference `../ui`;
- do not modify generated registry or theme files; and
- keep proof-of-concept implementation work on a separate branch or repository.

Stages 1 through 4 may proceed independently in `../ui`; all dependency,
implementation, lockfile, and evidence changes belong there. This Kumo checkout
retains only this proposal. Stage 5 begins as a separate Kumo change only after
the pilot and its maintenance implications have been accepted explicitly.

If the proposal is rejected, removing this single file restores the repository
to its prior code and dependency state.

## Risks and mitigations

| Risk | Mitigation |
| --- | --- |
| Independent Solid work appears Cloudflare-endorsed | Label the prototype as external and unofficial; defer Kumo changes, naming, and publication |
| Solid implementation drifts from Kumo | Contract tests, pinned Kumo revision, and per-component evidence |
| The docs hide package defects | Keep fixes and behavior in the owning package |
| Kumo integration assumes a sibling `../ui` checkout | Keep cross-repository paths out of Kumo and require reproducible package artifacts before official integration |
| A floating Kumo dependency changes the baseline | Pin the resolved npm version and regenerate evidence only through an explicit update |
| Pixel matching dominates accessibility | Keep separate evidence categories with no aggregate score |
| React behavior is copied even when incorrect | Use standards and the public contract as independent references |
| Dual JSX renderers conflict | Use explicit, non-overlapping integration include paths |
| Maintenance scope grows implicitly | Require a decision after the one-component pilot |

## Review questions

### Requested during parallel review

1. Is the comparison contract credible enough for maintainers to consider the
   eventual results?
2. Which Kumo maintainer, if any, can review contract ambiguities and gap
   classifications?
3. Are there missing Kumo `Switch` scenarios that would make the evidence
   incomplete?

### Recorded during the pilot

1. Is form participation part of the existing `Switch` contract, or should it
   be recorded as an implementation difference?
2. Should the evidence track only the pinned stable Kumo release, or also test
   the current development branch as a separate, clearly labeled channel?
3. Does the temporary style-sharing approach expose problems that require a
   framework-neutral Kumo style artifact?

### Deferred until evidence review

1. Who owns and maintains an accepted Solid package?
2. Does it remain in the Solidaria repository, move to this monorepo, or move to
   another home?
3. What public package name and npm scope may it use?
4. Is Kumo compatibility covered by Kumo's support and release policy?
5. How are Kumo visual styles shared and versioned without duplicating generated
   theme output?
6. Should the comparison become part of Kumo's official Astro documentation?

## Exit criteria for the proof of concept

The proof of concept is successful when:

- the Solid `Switch` is consumed from a reusable package with public exports;
- released React Kumo and workspace Solid Kumo render and hydrate independently
  in the `../ui` Astro comparison app;
- the required pilot matrix has reproducible evidence;
- each evidence artifact identifies the exact Kumo and Solidaria versions;
- all differences use the agreed result vocabulary;
- no docs-only fix changes package behavior or appearance;
- the Kumo production package is unchanged unless a separately reviewed gap is
  fixed; and
- maintainers can make an informed expand, revise, or stop decision.

Rendering the comparison in Kumo's Astro docs is a follow-up success criterion,
not a prerequisite for deciding whether the package proof of concept is sound.
