# BetStan: from learning TypeScript to AI-assisted engineering

_Stan Vasilyev · personal project; opinions are my own._

![BetStan public home page](../assets/betstan.png)

[BetStan](https://github.com/vasilyevstan/betstan) is a sportsbook **simulation** I
build hands-on with AI coding agents. It has scheduled football events, compressed
live matches, pre-match and live betting, moderation and settlement, but
[no real-money bets or real match feeds](https://github.com/vasilyevstan/betstan/blob/eeb60e6ebec34945b16de734c3361d00fcbc300b/README.md#L3-L7).

## 2023: learning by building

I started BetStan in 2023 to learn TypeScript and build microservices. Within two
months, I had a working version running on DigitalOcean. Then I put the project
on the shelf.

## 2026: a second life

In 2026 I returned to it with a new purpose: a place to practise AI-assisted
engineering on a codebase with real dependencies, state and deployment concerns.
That includes multi-agent development: deciding which work can run independently,
giving agents context, and bringing their contributions back together.

The repository contains [reusable specialist agents](https://github.com/vasilyevstan/betstan/blob/e3c2edc7e6dc76bd4b36d690696e25f68625bd16/.github/agents/README.md#L13-L29)
for backend, frontend, testing and other tasks. An [optional coordinator](https://github.com/vasilyevstan/betstan/blob/e3c2edc7e6dc76bd4b36d690696e25f68625bd16/.github/agents/betstan-delivery-coordinator.agent.md#L9-L28)
can delegate bounded work, but one owner remains responsible for the result.
These are development workflows, not AI agents making betting decisions.

In my own work, the productivity gain has been enormous. That is my experience,
not a measured speedup. Turning that capacity into reliable software still means
setting scope, sharing context, checking output and avoiding unnecessary handoffs.
More agents are not automatically better.

The compatibility case below is one example from that second phase. A visual
editor was the brief; preserving compatibility with older services was the less
obvious engineering problem.

## The invariant: presentation never decides bet validity

BetStan's services exchange durable facts over a message bus. Delivery is
[at-least-once](https://github.com/vasilyevstan/betstan/blob/eeb60e6ebec34945b16de734c3361d00fcbc300b/docs/wiki/Architecture.md#L159-L170):
duplicates and reordering are expected, consumers must be safe to replay, and the
live stream to browsers is a projection rather than the source of truth.

A presentation feature therefore has a firm boundary. Layout can change how
outcomes appear, but it must not change the configuration used for pricing,
acceptance, moderation or settlement. This is a deliberately broad engineering
laboratory, not evidence that every product needs this service architecture or
its operational overhead.

## The brief: a visual market-layout editor

Operators needed to arrange outcome tiles: which tiles go where, how wide they
are, and how the same layout can be reused. The feature also needed to preserve
existing standard markets and avoid rewriting shared contracts merely to
change a screen.

The first impression is "just add some layout metadata." The compatibility
question is what happens when older code reads, copies, compares or rewrites the
record containing that metadata.

## The risk, in plain language

Mixed-version behavior matters during upgrades and rollbacks. An older writer
does not know about a new layout field, but it still knows the old template
version shape and the fingerprint used to recognize a repeated event-market
creation request.

Nesting optional presentation inside those existing structures is not
automatically safe. A new field can change the shape that predecessor code copies
or compares, even if the author regards it as cosmetic. Shared betting
configuration must also retain its strict, layout-free shape.

[PR #1057](https://github.com/vasilyevstan/betstan/pull/1057) records that an
independent service-contract review found a mixed-writer issue in the initial
representation. The rejected representation was corrected in isolated tests;
this was **not a production incident**.

## How the delivery process caught it

Three parts of the workflow mattered.

**One accountable owner.** BetStan's
[agent guidance](https://github.com/vasilyevstan/betstan/blob/eeb60e6ebec34945b16de734c3361d00fcbc300b/docs/wiki/Agents.md)
keeps one owner responsible for scope, implementation, verification and
acceptance. Specialists are optional capabilities, not a conveyor belt.

**Independent verification at the changed boundary.** This feature touched
persisted records shared with predecessor writers. The review therefore examined
service contracts and compatibility, not only the appearance of the editor.
After the correction, a narrow verification checked the specific boundary again.

**Separate source acceptance from release.** The
[contribution rules](https://github.com/vasilyevstan/betstan/blob/eeb60e6ebec34945b16de734c3361d00fcbc300b/CONTRIBUTING.md)
distinguish accepting source, passing the relevant exact-source checks, and
deploying it. A merged PR is not proof of production availability.

The useful contribution of AI-assisted engineering here was exploration and
implementation within that process. Agreement between models was not the
acceptance criterion; executable evidence was.

## The decision and the tradeoffs

```mermaid
flowchart LR
    T["Template versions<br/>predecessor-compatible shape"] --> C["Unchanged betting configuration"]
    C --> B["Pricing, acceptance and settlement"]
    L["Backoffice layouts map<br/>keyed by template version"] --> S["Event-market layout snapshot"]
    S --> UI["Browser presentation"]
```

The [final design](https://github.com/vasilyevstan/betstan/blob/eeb60e6ebec34945b16de734c3361d00fcbc300b/docs/wiki/Architecture.md#L219-L237)
separates the two paths:

- **Layouts live beside authority, not inside its shared data.** Backoffice stores
  a [map keyed by template version](https://github.com/vasilyevstan/betstan/blob/eeb60e6ebec34945b16de734c3361d00fcbc300b/backoffice/src/model/MarketTemplate.ts#L5-L41)
  and copies a layout snapshot onto each event market. Stored template versions
  and creation fingerprints keep the predecessor shape. Layout is joined into
  the authoring HTTP representation, never shared configuration or betting messages.
- **A constrained grid rather than executable design.** Coordinates, spans and
  outcome keys are validated. Missing or duplicate outcomes and overlapping tiles
  are rejected. Arbitrary HTML, CSS and code are out of scope.
- **A bounded loss is acceptable.** An older writer may omit or drop optional
  presentation during an edit. Rules, prices and accepted bets remain unchanged.
  We accepted that limitation instead of assuming every writer upgrades together.
- **No shared-package migration.** Keeping presentation out of the shared
  contracts avoids a coordinated contract release for this feature.

## The evidence

Two regression tests exercise the compatibility claim:

- The [predecessor-shaped test](https://github.com/vasilyevstan/betstan/blob/eeb60e6ebec34945b16de734c3361d00fcbc300b/backoffice/src/route/__test__/Markets.test.ts#L106-L130)
  uses the old instance shape and fingerprint, replays the creation request,
  publishes, and checks that publisher arguments contain no layout.
- The [snapshot test](https://github.com/vasilyevstan/betstan/blob/eeb60e6ebec34945b16de734c3361d00fcbc300b/backoffice/src/route/__test__/Markets.test.ts#L61-L91)
  checks that editing or archiving a template does not change the event-market
  layout snapshot, and that the published configuration remains layout-free.

PR #1057 also records service tests and eight synthetic-browser journeys.
Those are **historical PR-reported results**, not tests rerun for this article.
The PR explicitly makes no production-availability claim. The sources establish
an AI-assisted workflow, but do not identify which particular model found the
defect.

## What transfers to other teams

The strongest outcome was a smaller contract change, not more orchestration.
Make the risky boundary explicit, keep one person accountable, and ask for
independent evidence at that boundary. Once a concrete defect is corrected,
verify that correction rather than repeating an entire review ceremony.

The same principle informs my private home-energy project: assistants are
read-only by default, device writes follow a separate guarded path, and new
controllers start in zero-write Shadow mode. Greater control requires evidence
gates or an explicit owner decision; controllers are never armed automatically.

**Agents help explore and implement. Accountable people and executable evidence
decide what is accepted.**
